mikejonesey.kubernetes.manifests
=========

For the most part, all manifests should be created in gitops where they are auto applied, but this role exists for edge cases where that's not possible, eg adding a required manifest pior to gitops bootstrapping.

Requirements
------------

n/a

Role Variables
--------------

### Defaults

| Name                      | Default        | Description                        |
|---------------------------|----------------|------------------------------------|
| kubernetes_kubeconfig     | ~/.kube/config | Path to kubeconfig file for auth.  |
| kubernetes_manifest_files | []             | A list of manifest files to apply. |

### Example

```yaml
kubernetes_manifest_files:
  - rbac_clusterroles/user_ro_nodes.yaml
  - rbac_clusterroles/user_ro_urls.yaml
  - rbac_rolebindings/cluster_ro_group.yaml
```

Dependencies
------------

### Roles

| Name                | Description                       |
|---------------------|-----------------------------------|
| kubernetes.core.k8s | Used to apply each manifest file. |

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: mikejonesey.kubernetes.manifests }

License
-------

GPL-2.0-or-later

Author Information
------------------

Michael Jones https://www.mikejonesey.co.uk/

