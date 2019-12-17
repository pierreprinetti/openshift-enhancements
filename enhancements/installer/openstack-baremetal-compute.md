---
title: openstack-baremetal-compute
authors:
  - "@pierreprinetti"
reviewers:
  - "@openshift/team-openstack-cloud-provider"
approvers:
  - TBD
creation-date: 2019-12-10
last-updated: 2019-12-10
status: provisional

---

# Openstack Baremetal Compute

## Release Signoff Checklist

- [ ] Enhancement is `implementable`
- [ ] Design details are appropriately documented from clear requirements
- [ ] Test plan is defined
- [ ] Graduation criteria for dev preview, tech preview, GA
- [ ] User-facing documentation is created in [openshift-docs](https://github.com/openshift/openshift-docs/)

## Summary

OpenStack's [Nova][openstack-nova] service can be configured to provision
baremetal machines as OpenShift Compute Nodes.

This enhancement will allow [Ironic][openstack-ironic]-provisioned machines to
be used as OpenShift Compute Nodes, when they are made available as Nova
flavors.

## Motivation

In heavy-loaded clusters, or when the workload is better handled with specially
purposed hardware, the functional requirement of controlling where the work is
computed might outweight the added value of virtualisation.

In OpenStack clusters where Nova transparently provisions baremetal machines,
this enhancement will ensure that OpenShift installations can use them as
Compute Nodes.

### Goals

* Enable running OpenShift on a mix of virtual and baremetal machines.
* Baremetal machines can be added and removed from a running OpenShift cluster.

### Non-Goals

**Separate subnets.**

An OpenStack setting where baremetal machines can't be attached to a virtual
subnet is not supported in this enhancement.

**External connectivity via floating IPs.**

The OpenStack cluster might make the baremetal nodes accessible, but exposing
them via floating IPs might represent an increase in complexity and is
currently not a goal.

## Proposal

This enhancement is less about adding a new logical piece, than it is about
uncovering unknowns in a configuration that might work out-of-the-box in
selected environments.

Therefore, the implementation consists of:
* identifying a reference architecture
* anticipating probable issues
* testing and fixing errors

### Characteristics of the reference architecture

* The choice between deploying a virtual or baremetal machine is done by selecting an appropriate Nova flavour
  * which can be done both in install config and passed to Cluster API provider OpenStack
  * for the time being, the interactive prompt does ask the flavor for the Compute Nodes specifically; the choice must be made editing `install-config.yaml`
* The deployment request makes the same API call to Nova as with a virtual machine install (there are no explicit calls to Ironic or anything in the Installer required)
  * so creating baremetal nodes should work transparently from the installer as well as Cluster API provider OpenStack
* both virtual and baremetal machines consume the same image from glance

### Probable issues

* The Ignition boot system consisting on a second payload downloaded at runtime
  requires the baremetal machine to have access to Swift early in the boot
  process.

* When attaching and detaching, the expected timings must be adjusted to
  reflect the different boot and shutdown latencies of physical machines

* When attaching, the Certificate Signing Request timeout must be adjusted to
  reflect the different boot and shutdown latencies of physical machines

### User Stories

#### Increased performance

User wants to run the Compute Nodes on baremetal machines for increased
performance. The Control Plane sits on regular Nova instances to leverage the
flexibility of virtual machines.

### Risks and Mitigations

TBD

## Design Details

### Test Plan

The enhancement will be tested using virtual baremetal machines.

TODO: alternatives
	- OVB on RDO?
        - MOC is planning on adding Ironic
	- Install (and snapshot?) an Ironic-enabled OSP on PSI (see Infrared-upshift)


TODO: references
        - https://gitlab.cee.redhat.com/jstransk/infrared-upshift-guide
	- https://docs.openstack.org/project-deploy-guide/tripleo-docs/latest/environments/virtualbmc.html
        - https://openstack-virtual-baremetal.readthedocs.io/en/latest/introduction.html

### Graduation Criteria

TBD

##### Removing a deprecated feature

TBD

### Upgrade / Downgrade Strategy

TBD

### Version Skew Strategy

TBD

## Implementation History

TBD

## Drawbacks

TBD

## Alternatives

TBD

## Infrastructure Needed

TODO: CI

[openstack-nova]: https://docs.openstack.org/nova "OpenStack Compute (nova)"
[openstack-ironic]: https://docs.openstack.org/ironic "OpenStack Ironic"
