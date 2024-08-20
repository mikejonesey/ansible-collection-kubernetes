mikejonesey.kubernetes.secrets
=========

Create K8s Secrets, generates a manifest from host_var or group_var, which should be sourced from passwordstore or vault.

Requirements
------------

k8s config.

Role Variables
--------------

### Defaults

| Name                                        | Default                | Description                                                                                                             |
|---------------------------------------------|------------------------|-------------------------------------------------------------------------------------------------------------------------|
| kubernetes_kubeconfig                       | "~/.kube/config"       | Path to the kube_config file to use to access the k8s cluster.                                                          |
| kubernetes_secrets_debug                    | false                  | Generates a local manifest file without applying, which can be used for inspection / debugging                          |
| kubernetes_secrets_lists                    | ['kubernetes_secrets'] | A List of List variables to process secrets from, this allows for additional lists to be created / added in group_vars. |
| kubernetes_secrets_create_missing_namespace | false                  | Auto create missing namespace, by default secrets will not be created in namespaces that don't already exist.           |
| kubernetes_secrets                          | *see below             | The default list of secrets to import.                                                                                  |

**Example kubernetes_secrets**
```yaml
kubernetes_secrets:
  - name: example-docker-registry
    type: docker-registry
    namespaces:
       - default
    docker-email: "user@example.com"
    docker-password: "{{ data_from_passwordstore_or_vault }}"
    docker-server: "https://index.docker.io/v1/"
    docker-username: 'user'

  - name: example-generic
    type: generic
    namespaces:
       - default
    data:
      field1: "{{ data_from_passwordstore_or_vault }}"
      field2: "{{ data_from_passwordstore_or_vault }}"

  - name: example-tls
    type: tls
    namespaces:
       - default
    cert: "{{ data_from_passwordstore_or_vault | b64encode }}"
    key: "{{ data_from_passwordstore_or_vault | b64encode }}"
```
**kubernetes_secrets subkey variables**

| Name            | Type            | Example                          | Description                                  |
|-----------------|-----------------|----------------------------------|----------------------------------------------|
| name            | -               | example-secret                   | The secret name                              |
| type            | -               | docker-registry, generic, tls    | The K8s secret type                          |
| namespaces      | -               | ['default','dev']                | A list of namespaces to create the secret in |
| docker-email    | docker-registry | user@example.com                 | docker-registry user email                   |
| docker-password | docker-registry | password123                      | docker-registry user password                |
| docker-server   | docker-registry | https://index.docker.io/v1/      | docker-registry uri                          |
| docker-username | docker-registry | user                             | docker-registry username                     |
| data            | generic         | webhook_url=https://example.com/ | generic opaque type data                     |
| cert            | tls             | base64 encoded cert              | tls certificate data base64 encoded          |
| key             | tls             | base64 encoded key               | tls key data base64 encoded                  |

### Role Variables

| Name | Description                                                                                                                                                                                     |
| --- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| valid_namespaces | This variable is populated with a list of existing namespaces within the k8s cluster, which is used to determine if a namespace or secret should be created. This variable is not used defined. |

Dependencies
------------

- collections
  - kubernetes.core

Example Playbook
----------------

See [playbooks/kubernetes.yml](../../playbooks/kubernetes.yml)

    ansible-playbook -i inventories/env kubernetes.yml -t secrets

License
-------

GPL-2.0-or-later

Author Information
------------------

Michael Jones https://www.mikejonesey.co.uk/
