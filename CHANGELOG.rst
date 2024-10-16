====================================
Mikejonesey.Kubernetes Release Notes
====================================

.. contents:: Topics

v1.0.13
=======

Release Summary
---------------

add k8s secrets encryption

Minor Changes
-------------

- add secret encryption
- example manifest updates for prometheus

v1.0.12
=======

Release Summary
---------------

Add manifest templates

Minor Changes
-------------

- add manifest templates
- cleanup of cluster reconfigure after further testing
- cleanup var naming and update readme

v1.0.11
=======

Release Summary
---------------

Audit webhook to falco, and exposing k8s services to prometheus.

Minor Changes
-------------

- add cluster reconfigure role
- add extra params for audit logging
- add extra params for audit webhooks
- upgrade initial cluster setup options

Bugfixes
--------

- change lb auth to pass when >2 lb, until further debug, ah not working on >2 nodes
- replace hardcoded router_id in keepalived template

v1.0.10
=======

Release Summary
---------------

manifests and client certificates

Minor Changes
-------------

- add client certificate role
- add gVisor kvm to containerd
- add manifests role
- audit_webhook role updated

v1.0.9
======

Release Summary
---------------

Add gVisor sandbox support

Minor Changes
-------------

- update containerd to include gVisor sandbox setup
- update secrets readme, namespaces default var documentation

v1.0.8
======

Release Summary
---------------

Documentation updates

Minor Changes
-------------

- update readme for secrets role

v1.0.7
======

Release Summary
---------------

updates to secrets role

Minor Changes
-------------

- changelog updates
- galaxy readme update
- make missing namespace creation optional in secrets role
- update secrets to allow extra lists of secrets to be added

v1.0.6
======

Minor Changes
-------------

- cleanup, remove null lines from template

v1.0.5
======

Minor Changes
-------------

- add example inventory and comments on var and patch generic type secret
- add example playbooks
- comment out example plays unrelated to this collection
- lint example playbooks
- lint example vars
- update cluster setup readme
- update collection dependancies
- update default k8s version
- update galaxy.yml

v1.0.4
======

Minor Changes
-------------

- allow changelog summary in changelist

v1.0.3
======

Minor Changes
-------------

- update galaxy version

v1.0.2
======

Minor Changes
-------------

- add .ansible-lint config file
- update changelog

v1.0.1
======

Minor Changes
-------------

- changelog updates

v1.0.0
======

Minor Changes
-------------

- add static pod lb for ha
- adding cluster_upgrade and rename reset to cluster_reset
- lint updates
- rename role kubernetes to mikejonesey.kubernetes.cluster_setup
- setup new secrets role
- update readme
