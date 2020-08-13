# GSOC '20 - Cluster Addons: Package All Things!
Project tracker for my GSoC project - Cluster Addons: package all things!- for the Kubernetes organization (CNCF).

To easier track the progress, I've created a [public trello board](https://trello.com/b/6kuguhLw/gsoc-2020-tracker-cluster-addons) where I'll be providing regular progress updates.

# Project Abstract

> Cluster Addons are resources that are considered inherently part of the Kubernetes
> cluster as they help extend the functionality of the cluster. Over time different
> addons have surfaced with increasing complexity while the tools for managing these
> addons have not progressed as much.

> The aim of this proposal is to build and package operators for different popular
> addons that are easy to use and follow best practices in various clusters.

# General Info

* Name: Somtochi Onyekwere
* Github: [SomtochiAma](https://github.com/SomtochiAma)
* Email: somtochionyekwere@gmail.com
* Slack nick: SomtochiAma
* Twitter: [@AmaOnyekwere](https://twitter.com/AmaOnyekwere)
* Mentors: [Justin Santa Barbara]( ), [Leigh Capili](https://github.com/stealthybox)

# Links

* [Project on GSoC website](https://summerofcode.withgoogle.com/projects/#6513360218095616)
* [Proposal Submitted for GSoC](https://github.com/SomtochiAma/gsoc-2020-meta-k8s/blob/master/GSoC%202020%20PROPOSAL%20-%20PACKAGE%20ALL%20THINGS.pdf)
* [Proposal Draft (Google Doc)](https://docs.google.com/document/d/1bRbEps3B5KFuQRlFJEgJtyhcJ35VhGlQYifvhMd0Rlo/edit?usp=sharing)
* [Original Feature Issue](https://github.com/kubernetes-sigs/cluster-addons/issues/39)
* [Progress Tracker (Trello Board)](https://trello.com/b/6kuguhLw/gsoc-2020-tracker-cluster-addons)
* [All commits to kubernetes-sigs/cluster-addons](https://github.com/kubernetes-sigs/cluster-addons/commits?author=SomtochiAma)
* [All commits to kubernetes-sigs/kubebuidler-declarative-pattern](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/commits?author=SomtochiAma)
## Pull Requests and Issues

**Repository: cluster-addons**

Total Pull Requests Created: 13
1. [kubernetes-sigs/cluster-addons#75](https://github.com/kubernetes-sigs/cluster-addons/pull/75) - Generic controller
2. [kubernetes-sigs/cluster-addons#73](https://github.com/kubernetes-sigs/cluster-addons/pull/73) - Kubectl plugin for resources that have a particular ownerref
3. [kubernetes-sigs/cluster-addons#71](https://github.com/kubernetes-sigs/cluster-addons/pull/71) - Uses packages from declarative pattern repo
4. [kubernetes-sigs/cluster-addons#70](https://github.com/kubernetes-sigs/cluster-addons/pull/70) - Adds role for manager
5. [kubernetes-sigs/cluster-addons#69](https://github.com/kubernetes-sigs/cluster-addons/pull/69) - Flannel Operator
6. [kubernetes-sigs/cluster-addons#68](https://github.com/kubernetes-sigs/cluster-addons/pull/68) - Takes in manifest and generates role
7. [kubernetes-sigs/cluster-addons#67](https://github.com/kubernetes-sigs/cluster-addons/pull/67) - [WIP] RBAC gen
8. [kubernetes-sigs/cluster-addons#59](https://github.com/kubernetes-sigs/cluster-addons/pull/59) - NodeLocalDNS operator
9. [kubernetes-sigs/cluster-addons#56](https://github.com/kubernetes-sigs/cluster-addons/pull/56) - Readme for dashboard operator and updates stable version
10. [kubernetes-sigs/cluster-addons#49](https://github.com/kubernetes-sigs/cluster-addons/pull/49) - Updates installer instructions for Mac OS
11. [kubernetes-sigs/cluster-addons#45](https://github.com/kubernetes-sigs/cluster-addons/pull/45) - Updates CoreDNS readme for kubebuilder 2.0
12. [kubernetes-sigs/cluster-addons#44](https://github.com/kubernetes-sigs/cluster-addons/pull/44) - Operator for the Kubernetes dashboard
13. [kubernetes-sigs/cluster-addons#42](https://github.com/kubernetes-sigs/cluster-addons/pull/42) - Adds docs on how to run operator locally

Total Pull Requests Reviewed: 1
1. [kubernetes-sigs/cluster-addons#35](https://github.com/kubernetes-sigs/cluster-addons/pull/35) - kubeproxy operator


**Repository: kubebuilder-declarative-pattern**

Total Pull Requests Created: 11
1. [kubernetes-sigs/kubebuilder-declarative-pattern#112](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/112) - Unstructured support in different functions
2. [kubernetes-sigs/kubebuilder-declarative-pattern#111](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/111) - Checks for annotation for objects in-cluster
3. [kubernetes-sigs/kubebuilder-declarative-pattern#110](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/110) - Uses kstatus to compute status of CR
4. [kubernetes-sigs/kubebuilder-declarative-pattern#105](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/105) - Checks for ignore reconciliation annotations
5. [kubernetes-sigs/kubebuilder-declarative-pattern#104](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/104) - Base git repository working
6. [kubernetes-sigs/kubebuilder-declarative-pattern#103](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/103) - Adds function for parsing list
7. [kubernetes-sigs/kubebuilder-declarative-pattern#102](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/102) - Adds Version Check
8. [kubernetes-sigs/kubebuilder-declarative-pattern#101](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/101) - Uses new apply function so we don't need kubectl
9. [kubernetes-sigs/kubebuilder-declarative-pattern#100](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/100) - Moves some functions from Coredns operator that could be reused
10. [kubernetes-sigs/kubebuilder-declarative-pattern#96](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/96) - Removes some steps that are now been built into kubebuidler
11. [kubernetes-sigs/kubebuilder-declarative-pattern#78](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/78) - Makes file loading more flexible

Total Issues Opened: 1
1. [kubernetes-sigs/kubebuilder-declarative-pattern#84](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/issues/84) - Test framework for the addons
