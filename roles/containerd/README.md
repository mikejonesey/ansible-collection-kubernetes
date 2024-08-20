mikejonesey.kubernetes.containerd
=========

Install and configure containerd for kubernetes.

Requirements
------------

n/a

Role Variables
--------------

### Default Variables

| Name  | Default | Description                                                        |
| --- | --- |--------------------------------------------------------------------|
| kubernetes_containerd_sandbox_gvisor_enabled | false | When enabled gVisor sandbox is installed as an additional runtime. |

### K8s Sandbox Runtimes

This role does not setup the manifests for alternate runtimes example:

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

Once the mandifest is added pods can switch from the default runtime to the alternate runtime defined using runtimeClassName.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-gvisor
spec:
  runtimeClassName: gvisor
  containers:
  - name: nginx
    image: nginx
```

Dependencies
------------

n/a

Example Playbook
----------------

See [playbooks/kubernetes.yml](../../playbooks/kubernetes.yml)

    - name: Install k8s Dependancies
      hosts: whole_environment
      become: true
      gather_facts: true
      roles:
        
        ...
        
        - name: Containerd
          role: mikejonesey.kubernetes.containerd
          when: "kubernetes_version is version('1.24.0','>=')"
          tags: containerd
        
        ...
        
      tags: k8s-dep

License
-------

GPL-2.0-or-later

Author Information
------------------

Michael Jones https://www.mikejonesey.co.uk/
