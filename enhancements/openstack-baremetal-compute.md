---
title: openstack-baremetal-compute
authors:
  - "@pierreprinetti"
reviewers:
  - "@mandre"
  - TBD
approvers:
  - TBD
creation-date: 2019-12-10
last-updated: 2019-12-10
status: provisional

---

# Openstack Baremetal Compute

This is the title of the enhancement. Keep it simple and descriptive. A good
title can help communicate what the enhancement is and should be considered as
part of any review.

The YAML `title` should be lowercased and spaces/punctuation should be
replaced with `-`.

To get started with this template:
1. **Pick a domain.** Find the appropriate domain to discuss your enhancement.
1. **Make a copy of this template.** Copy this template into the directory for
   the domain.
1. **Fill out the "overview" sections.** This includes the Summary and
   Motivation sections. These should be easy and explain why the community
   should desire this enhancement.
1. **Create a PR.** Assign it to folks with expertise in that domain to help
   sponsor the process.
1. **Merge at each milestone.** Merge when the design is able to transition to a
   new status (provisional, implementable, implemented, etc.). View anything
   marked as `provisional` as an idea worth exploring in the future, but not
   accepted as ready to execute. Anything marked as `implementable` should
   clearly communicate how an enhancement is coded up and delivered. If an
   enhancement describes a new deployment topology or platform, include a
   logical description for the deployment, and how it handles the unique aspects
   of the platform. Aim for single topic PRs to keep discussions focused. If you
   disagree with what is already in a document, open a new PR with suggested
   changes.

The `Metadata` section above is intended to support the creation of tooling
around the enhancement process.

## Release Signoff Checklist

- [ ] Enhancement is `implementable`
- [ ] Design details are appropriately documented from clear requirements
- [ ] Test plan is defined
- [ ] Graduation criteria for dev preview, tech preview, GA
- [ ] User-facing documentation is created in [openshift-docs](https://github.com/openshift/openshift-docs/)

## Summary

OpenStack's [Nova][openstack-nova] service can be configured to provision
baremetal servers as compute instances.

This enhancement will allow [Ironic][openstack-ironic]-provisioned compute
instances to be used as OpenShift compute nodes, when they are made available as
Nova flavors.

## Motivation

In heavy-loaded clusters, or when the workload is better handled with specially
purposed hardware, the functional requirement of controlling where the work is
computed might outweight the added value of virtualisation.

In OpenStack clusters where Nova transparently provisions baremetal nodes, this
enhancement will ensure that OpenShift installations can use them as Compute
nodes.

### Goals

* Baremetal nodes provisioned by Nova through Ironic can be used as OpenShift
  compute nodes
* Baremetal compute nodes can be attached and detached from a running OpenShift
  installation

### Non-Goals

Installation requires the baremetal nodes to be provisioned by Nova through
Ironic, and that the network connection can be software-operated in such a way
that they can be automatically integrated into the OpenShift-cluster virtual
subnet.

An OpenStack setting where baremetal workers sit in their ad-hoc network is not
supported in this enhancement.

## Proposal

This enhancement is less about adding a completely new feature, than it is
about uncovering unknowns in a configuration that might work out-of-the-box in
selected environments.

As such, the implementation consists of:
* identifying a reference architecture
* anticipating probable issues
* testing and fixing errors

### Characteristics of the reference architecture

* The reference OpenStack cluster transparently provisions baremetal nodes
* Provisioned baremetal nodes can be attached to a virtual subnet

### Probable issues

* The Ignition boot system consisting on a second payload downloaded at runtime
  requires the machine to have access to Swift early in the boot process

* When attaching and detaching, the expected timings must be adjusted to
  reflect the different boot and shutdown latencies of physical machines

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
[openstack-ironic]: https://wiki.openstack.org/wiki/Ironic "OpenStack Bare Metal Provisioning Program"
