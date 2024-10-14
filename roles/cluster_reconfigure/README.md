Role Name
=========

This role templates kube admin config, and uses the deployed config file to run dry-run updates to validate and update changes to kube api server, kube control manager, etcd and kube scheduler.

Common uses of this role:

* Enable / Disable prometheus monitoring of services deployed by kubeadm
* Enable / Disable api server audit logging
* Enable / Disable api server audit webhook for example to integrate with falco

Requirements
------------

k8s cluster must be deployed prior.

Role Variables
--------------

### Default Variables

| Name                               | Default                 | Description                                                                      |
|------------------------------------|-------------------------|----------------------------------------------------------------------------------|
| kubernetes_version                 | "1.30.3-1.1"            | Kubernetes package version                                                       |
| k8s_control_plane_port             | "6443"                  | The control plane port                                                           |
| k8s_control_plane_host             | *inventory_hostname*    | The hostname of the control plane                                                |
| k8s_control_plane_ip               | *ansible_ssh_host*      | The ip address of control plane                                                  |
| k8s_service_subnet                 | "10.96.0.0/16"          | The Service CIDR                                                                 |
| k8s_pod_subnet                     | "10.244.0.0/24"         | The POD CIDR                                                                     |
| kubernetes_kubeadm_config_template | "kubeadm-config.yml.j2" | The template to use for kubeadm config, the default is one provided by the role. |

### kubeadm config template (Also Default Variables)

| Name                                       | Default                               | Description                                              |
|--------------------------------------------|---------------------------------------|----------------------------------------------------------|
| kubernetes_audit_log_enabled               | false                                 | If audit policy logging should be enabled                |
| kubernetes_audit_webhook_enable            | false                                 | If audit policy webhook notifications should be enabled  |
| kubernetes_audit_log_path                  | "/var/log/kubernetes/audit/audit.log" | where logs should be written within the pod              |
| kubernetes_audit_log_mode                  | "batch"                               | default blocking                                         |
| kubernetes_audit_log_maxbackup             | 2                                     | max log files to retain                                  |
| kubernetes_audit_log_maxage                | 7                                     | how many days to keep                                    |
| kubernetes_audit_log_maxsize               | 200                                   | size in megabytes before the log is rotated              |
| kubernetes_audit_webhook_server            | "http://localhost:30007/k8s-audit"    |                                                          | 
| kubernetes_audit_webhook_mode              | "batch"                               | default batch, all modes: batch,blocking,blocking-strict 
| kubernetes_audit_webhook_batch_buffer_size | "10000"                               |                                                          |
| kubernetes_audit_webhook_initial_backoff   | "10m"                                 | default 10s - retry of failed                            
| kubernetes_audit_webhook_batch_max_size    | "500"                                 | default 400                                              
| kubernetes_audit_webhook_batch_max_wait    | "30s"                                 | default 30s                                              
| kubernetes_audit_webhook_truncate_enabled  | "true"                                | default false                                            

### Audit Policy Template (Also Default Variables)

| Name                           | Default           | Description            |
|--------------------------------|-------------------|------------------------|
| kubernetes_audit_policy_rules  | See defaults file | Based on Falco example |

minimal example of audit policy rules:

```yaml
kubernetes_audit_policy_rules:
  rules:
    - level: Metadata
      resources:
        - group: ""
          resources: [ "pods/log", "pods/status" ]
    - level: Metadata
      omitStages:
        - "RequestReceived"
```

Dependencies
------------

n/a

Example Playbook
----------------

See playbooks/kubernetes-upgrade.yaml

    - name: Upgrade Cluster
      hosts: '{{ k8s_target | default("k8s") }}'
      become: yes
      serial: 1
      roles:
        - name: K8s Cluster Upgrade
          role: mikejonesey.kubernetes.cluster_upgrade
          tags: upgrade
      tags: k8s-upgrade

License
-------

GPL-2.0-or-later

Author Information
------------------

Michael Jones https://www.mikejonesey.co.uk/
