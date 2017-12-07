# ocp-node-by-node

Maintained by Chris Kim (oats87)

## Playbook Info

This is a simple playbook designed to perform a task on nodes on an iterative node by node basis, based on the code found in the openshift-ansible repository. It essentially cherry picks the logic (with a little added spice) for checking node readiness so you can guarantee a node is actually ready and schedulable. This helps to reduce downtime across a cluster (especially in production).

## Version Compatibility

I have tested this on both OCP 3.5 and 3.6 clusters. I do not know the status of compatibility with other versions of OpenShift. The `lib_openshift` library was taken from the official 3.6 openshift-ansible playbooks.

## Using These Playbooks

Simply call ansible-playbook with the playbook you'd like to use while specifying your OpenShift hosts file.

I've provided three files in this repository, one that iteratively reboots all of the nodes, one that restarts the atomic-openshift-node service on all of the nodes, and one that does nothing to the nodes (to be customized by the user). Please note that these playbooks will not perform any actions against the masters in your hosts file as to prevent interruptions. It will automatically choose the first listed master to execute API calls on.

## Modifying/Adding Tasks

PLEASE make sure to add the "when" condition to each task you add if you don't want that task to be executed on the master! The appropriate when statement base is

```
    when: inventory_hostname not in masters
```