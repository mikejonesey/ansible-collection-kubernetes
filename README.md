# Ansible Collection - mikejonesey.kubernetes

The main purpose of this collection is to provision and update k8s clusters on-prem / home-lab;

- Bare-Metal
- Virtualised Setups

For cloud installs, GKE, EKS, AKS are preffered, but the collection could be run on compute instances too provided network restrictions are lifted eg, source/dest check.

## Installation

```commandline
ansible-galaxy collection install mikejonesey.kubernetes
```

## Supported K8s Cluster Install Topology

1. **Standalone node**
  - K8s Cluster
    - node-cp-1
2. **Single Control Plane Cluster**
  - K8s Cluster
    - node-cp-1
    - node-worker-1
    - node-worker-2
    - node-worker-3
3. **HA Stacked Control Plane External Loadbalancer**
  - Loadbalancer 1 + vip (keepalived+haproxy)
  - Loadbalancer 2 + vip (keepalived+haproxy)
  - K8s Cluster
    - node-cp-1
    - node-cp-2
    - node-cp-3
    - node-worker-1
4. **HA Stacked Control Plane Static Pod Loadbalancer**
  - K8s Cluster
    - node-cp-1 + lb + vip (keepalived+haproxy)
    - node-cp-2 + lb + vip (keepalived+haproxy)
    - node-cp-3 + lb + vip (keepalived+haproxy)
    - node-worker-1

## Host / Node Distro Support

| Distro       | Tested                      |
|--------------|-----------------------------|
| Debian 11    | YES                         |
| Debian 12    | YES                         |
| Ubuntu 24.04 | Not Tested, will test soon. |

## Roles

| Name                                                                    | Description                                                          |
|-------------------------------------------------------------------------|----------------------------------------------------------------------|
| [mikejonesey.kubernetes.containerd](roles/containerd/)                  | Install containerd, and extra sandbox runtime backends.              |
| [mikejonesey.kubernetes.cluster_setup](roles/cluster_setup/)            | K8s cluster installation, and updates eg adding extra nodes later.   |
| [mikejonesey.kubernetes.ha_loadbalancer](roles/ha_loadbalancer/)        | Highly available cluster setup (VIP and loadbalancer)                |
| [mikejonesey.kubernetes.cluster_upgrade](roles/cluster_upgrade/)        | Cluster version upgrade                                              |
| [mikejonesey.kubernetes.secrets](roles/secrets/)                        | K8s secrets creation                                                 |
| [mikejonesey.kubernetes.manifests](roles/manifests)                     | Create K8s Manifests                                                 |
| [mikejonesey.kubernetes.client_certificates](roles/client_certificates) | Create user certificates / kubeconfig for authentication to k8s api. |
| [mikejonesey.kubernetes.cluster_reset](roles/cluster_reset/)            | Cluster uninstall                                                    |

# Example Playbooks

See [playbooks](playbooks/) for examples.

### Cluster Install

```commandline
ansible-playbook -i inventories/... kubernetes.yml
```

### Cluster Upgrade

```commandline
ansible-playbook -i inventories/... kubernetes-upgrade.yml 
```

### Kubernetes Secrets Creation / Update

```commandline
ansible-playbook -i inventories/... kubernetes.yml -t secrets
```

# Todo...

Add support for:

- HA Stacked Control Plane kube-vip Loadbalancer
- HA External etcd Control Plane + External Loadbalancer

More Info on Highly Available Topology: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/ha-topology/

