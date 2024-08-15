mikejonesey.kubernetes.cluster_setup
=========

This role provisions a kubernetes cluster for on-prem / home-lab.

Requirements
------------

[kubectl](https://kubernetes.io/docs/reference/kubectl/) for management of clusters.

Role Variables
--------------

defaults/main.yml

| Variable               | Type    | Default                  | Use                                      |
|------------------------|---------|--------------------------|------------------------------------------|
| kubernetes_version     | default | 1.30.3-1.1               | Determine the version of k8s to install  |
| k8s_control_plane_port | default | 6443                     | The external port of the k8s api service |
| k8s_control_plane_host | default | {{ inventory_hostname }} | The hostname of the k8s api              |
| k8s_control_plane_ip   | default | {{ ansible_ssh_host }}   | The ip address of the k8s api            |
| k8s_service_subnet     | default | 10.96.0.0/16             | The Service CIDR                         |
| k8s_pod_subnet         | default | 10.244.0.0/24            | The POD CIDR                             |

Dependencies
------------

Installation:

```commandline
ansible-galaxy collection install mikejonesey.kubernetes
ansible-galaxy collection install mikejonesey.helm_charts
```

Playbook Dependencies:

| Collection              | Role                            | Usage                                         | Required | Comments                                                                    |
|-------------------------|---------------------------------|-----------------------------------------------|----------|-----------------------------------------------------------------------------|
| mikejonesey.kubernetes  | containerd                      | CRI backend for k8s                           | YES      | Playbook runs this role before k8s cluster init                             |
| mikejonesey.kubernetes  | ha_loadbalancer                 | if deploying a HA setup                       | optional |                                                                             | 
| mikejonesey.helm_charts | tigera_operator                 | This is used for setting up calico networking | YES      | Playbook runs this role after the first node is added to the control plane. |

Example Playbook
----------------

See [playbooks/kubernetes.yaml](../../playbooks/kubernetes.yml)

    ansible-playbook -i inventories/example.host kubernetes.yml

License
-------

GPL-2.0-or-later

Author Information
------------------

Michael Jones https://www.mikejonesey.co.uk/

