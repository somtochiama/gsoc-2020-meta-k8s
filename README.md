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
* Mentors: [Justin Santa Barbara](https://github.com/justinsb), [Leigh Capili](https://github.com/stealthybox)

# Links

* [Original Enhancement proposal](https://github.com/kubernetes/enhancements/blob/master/keps/sig-cluster-lifecycle/addons/0035-20190128-addons-via-operators.md)
* [Project on GSoC website](https://summerofcode.withgoogle.com/projects/#6513360218095616)
* [Proposal Submitted for GSoC](https://github.com/SomtochiAma/gsoc-2020-meta-k8s/blob/master/GSoC%202020%20PROPOSAL%20-%20PACKAGE%20ALL%20THINGS.pdf)
* [Proposal Draft (Google Doc)](https://docs.google.com/document/d/1bRbEps3B5KFuQRlFJEgJtyhcJ35VhGlQYifvhMd0Rlo/edit?usp=sharing)
* [Original Feature Issue](https://github.com/kubernetes-sigs/cluster-addons/issues/39)
* [Progress Tracker (Trello Board)](https://trello.com/b/6kuguhLw/gsoc-2020-tracker-cluster-addons)
* [All commits to kubernetes-sigs/cluster-addons](https://github.com/kubernetes-sigs/cluster-addons/commits?author=SomtochiAma)
* [All commits to kubernetes-sigs/kubebuidler-declarative-pattern](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/commits?author=SomtochiAma)


# Work done

## Various additions to kubebuilder-declarative-pattern

The kubebuilder-declarative-pattern[from here on referred to as KDP] repo is an extra layer of addon specific tooling on top of the kubebuilder SDK that is enabled by passing the experimental `--pattern=addon` flag to `kubebuilder create` command. Together, they create the base code for the addon operator. During the internship, I worked on a couple of features in KDP and cluster-addons.

### Version check for the operators
Enabling version check for operators helped in making safer upgrades/downgrades to different versions of the addon even though the operator had complex logic. It is a way of matching the version of an addon to the version of the operator that knows how to manage it well. Most addons have different versions and these versions might need to be managed differently. This feature checks the custom resource for the `addons.k8s.io/min-operator-version` annotation which states the minimum operator version that is needed to manage the version against the version of the operator. If the operator version is below the minimum version required, the operator pauses with an error telling the user that the version of the operator is too low. This helps to ensure that the correct operator is being used for the addon.

**Related Pull Requests**

- [kubernetes-sigs/kubebuilder-declarative-pattern#102](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/102) - Adds Version Check


### Git Repository for storing the manifests
Previously, there was support for only local file directories and https repositories for storing manifests. Giving creators of addon operators the ability to store manifest in GitHub repository enables faster development and version control.  When starting the controller, you can pass in a flag to specify the location of your channels directory(the channels directory contains manifest for different versions - the controller pulls the manifest from this directory and applies it to the cluster). During the internship period, I extended it to include git repositories.

**Related Pull Requests**
-  [kubernetes-sigs/kubebuilder-declarative-pattern#104](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/104) - Base git repository working
- [kubernetes-sigs/kubebuilder-declarative-pattern#119](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/119) - Allows use of private repositories

### Annotations to temporarily disable reconciliation
The reconciliation loop that ensures that the desired state matches the actual state prevents modification of objects in the cluster. This makes it hard to experiment or investigate what might be wrong in the cluster as any changes made are promptly reverted. I resolved this by allowing users to place `addons.k8s.io/ignore` annotation to the resource that they don’t want the controller to reconcile. The controller checks for this annotation and doesn’t reconcile that object. To resume reconciliation, simply remove the annotation from the resource.

**Related Pull Requests**
- [kubernetes-sigs/kubebuilder-declarative-pattern#111](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/111) - Checks for annotation for objects in-cluster
- [kubernetes-sigs/kubebuilder-declarative-pattern#105](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/105) - Checks for ignore reconciliation annotation

### Unstructured support in kubebuilder-declarative-pattern
One of the operators that I worked on is a generic controller that could manage more than one cluster addon that did not require extra configuration. To do this, the operator couldn’t use a particular type and needed the kubebuilder-declarative-repo to support using the unstructured.Unstructured type. There were various functions in the kubebuilder-declarative-pattern that couldn’t handle this type and return an error if the object passed in was not of type `addonsv1alpha1.CommonObject`. The functions were modified to handle both unstructured.Unstructured and addonsv1alpha.CommonObject.

**Related Pull Requests**
[kubernetes-sigs/kubebuilder-declarative-pattern#112](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/112) - Unstructured support in different functions
[kubernetes-sigs/kubebuilder-declarative-pattern#112](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/117) - Refactors the kstatus functions and adds unstructured support

### Refined Status Aggregation

**Related Pull Requests**
- [kubernetes-sigs/kubebuilder-declarative-pattern#110](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/110) - Uses kstatus to compute status of CR
- [kubernetes-sigs/kubebuilder-declarative-pattern#110](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/120) - Aggregate status looks in the correct namespace

### Applying Object without the `kubectl` command

**Related Pull Requests**
- [kubernetes-sigs/kubebuilder-declarative-pattern#103](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/103) - Adds function for parsing list
- [kubernetes-sigs/kubebuilder-declarative-pattern#101](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/101) - Uses new apply function so we don't need kubectl

## Various Refactoring

**Related Pull Requests**
- [kubernetes-sigs/kubebuilder-declarative-pattern#78](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/78) - Makes file loading more flexible
- [kubernetes-sigs/kubebuilder-declarative-pattern#100](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/100) - Moves some functions from Coredns operator that could be reused
- [kubernetes-sigs/kubebuilder-declarative-pattern#180](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/118) - Refactors code to set and get status from object
- [kubernetes-sigs/kubebuilder-declarative-pattern#180](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/116) - Adds helper functions

## Tools and CLI programs
There were also some command-line programs I wrote that could be used to make working with addon operators easier. Most of them have uses outside the addon operators as they try to solve a specific problem that could surface anywhere while working with Kubernetes. I encourage you to check them out when you have the chance!

###  RBAC generator
One of the biggest concerns with the operator was RBAC. You had to manually look through the manifest and add the RBAC rule for each resource as it needs to have RBAC permissions to create, get, update and delete the resources in the manifest when running in-cluster. Building the RBAC generator automated the process of writing the RBAC roles and role bindings. The function of the RBAC generator is simple. It accepts the file name of the manifest as a flag. Then, it parses the manifest and gets the API group and resource name of the resources and adds it to a role. It outputs the role and role binding to stdout or a file if the `--out` flag is parsed.
Additionally, the tool enables you to split the RBAC by separating the cluster roles in the manifest. This lessened the security concern of an operator being over-privileged as it needed to have all the permissions that the clusterrole has. If you want to apply the clusterrole yourself and not give the operator these permissions, you can pass in a `--supervisory` boolean flag so that the generator does not add these permissions to the role. The CLI program resides here.

**Related Pull Requests**
- [kubernetes-sigs/cluster-addons#68](https://github.com/kubernetes-sigs/cluster-addons/pull/68) - Takes in manifest and generates role

### Kubectl Ownerref
It is hard to find out at a glance which objects were created by an addon custom resource, this kubectl plugin alleviates that pain by displaying all the objects in the cluster that a resource has ownerrefs on. You simply pass the kind and the name of the resource as arguments to the program and it checks the cluster for the objects and gives the kind, name, the namespace of such object. It could be useful to get a general overview of all the objects that the controller is reconciling by passing in the name and kind of custom resource.

**Related Pull Requests**
- [kubernetes-sigs/cluster-addons#73](https://github.com/kubernetes-sigs/cluster-addons/pull/73) - Kubectl plugin for resources that have a particular ownerref

## Addon Operators
To fully understand addons operators and make changes to how they are being created, you have to try creating and using them. Part of the summer was spent building operators for some popular addons like the Kubernetes dashboard, flannel, NodeLocalDNS and so on. Please check the cluster addons for the different addon operators. In this section, I will just highlight just one that is a little different from others.

### Generic Controller
The generic controller is a general controller that can be shared between addons that don’t require much configuration. This minimizes resource consumption on the cluster as it reduces the number of controllers that need to be run. It alsoSo instead of building your own operator, you can just use the generic controller and whenever you feel that your needs have grown and you need a more complex operator, you can always scaffold the code with kubebuilder and continue from where the generic operator stopped. To use the generic controller, you can generate the CRD using this tool(generic-addon). You pass in the kind, group, and the location of your channels directory(it could be a git repository!). The tool generates the - CRD, RBAC manifest and two custom resources for you

The process is as follows
- Create the Generic CRD
- Generate all the manifest needed with the `generic-addon` tool found here (here)[https://github.com/kubernetes-sigs/cluster-addons/blob/master/tools/generic-addon/README.md]
This tool creates
1. The CRD for your addon
2. The RBAC for the CRD
3. The RBAC for applying manifest
4. The CR for your addon
5. A Generic CR

The Generic custom resource looks like this:

```
apiVersion: addons.x-k8s.io/v1alpha1
kind: Generic
metadata:
 			name: generic-sample
spec:
  			objectKind:
    kind: NodeLocalDNS
    version: "v1alpha1"
    group: addons.x-k8s.io
channel: "../nodelocaldns/channels"
```

Apply these manifests(Ensure to apply the CRD before the CR)
Run the Generic controller, either on your machine or in-cluster

**Related Pull Requests**
- [kubernetes-sigs/cluster-addons#75](https://github.com/kubernetes-sigs/cluster-addons/pull/75) - Generic controller

### Other Addon Operators

- Dashboard Operator:
  [kubernetes-sigs/cluster-addons#44](https://github.com/kubernetes-sigs/cluster-addons/pull/44) - Operator for the Kubernetes dashboard

- Flannel Operator:
  [kubernetes-sigs/cluster-addons#69](https://github.com/kubernetes-sigs/cluster-addons/pull/69) - Flannel Operator

- NodeLocalDNS Operator:
  [kubernetes-sigs/cluster-addons#59](https://github.com/kubernetes-sigs/cluster-addons/pull/59) - NodeLocalDNS operator

- Additions to the Kube proxy operator:

  [johnsonj/addon-operators#1](https://github.com/johnsonj/addon-operators/pull/1) - Makes controller run in-cluster
  [johnsonj/addon-operators#2](https://github.com/johnsonj/addon-operators/pull/2) - Golden files test for kube .proxy
  [johnsonj/addon-operators#3](https://github.com/johnsonj/addon-operators/pull/3) - Tidy up the operator and ensure create-cluster.sh works
  [johnsonj/addon-operators#4](https://github.com/johnsonj/addon-operators/pull/4) - Updates go version
  [kubernetes-sigs/cluster-addons#71](https://github.com/kubernetes-sigs/cluster-addons/pull/71) - Uses packages from declarative pattern repo


### Documentation

To crown a very fulfilling summer spent working on cluster-addons, I worked on creating and updating various documentation in both cluster addons and kubebuilder-declarative-repo so that is easy for other to understand and use the work I have done

1. [kubernetes-sigs/cluster-addons#42](https://github.com/kubernetes-sigs/cluster-addons/pull/42) - Adds docs on how to run operator locally
2. [kubernetes-sigs/cluster-addons#56](https://github.com/kubernetes-sigs/cluster-addons/pull/56) - Readme for dashboard operator and updates stable version
3. [kubernetes-sigs/cluster-addons#49](https://github.com/kubernetes-sigs/cluster-addons/pull/49) - Updates installer instructions for Mac OS
4. [kubernetes-sigs/cluster-addons#45](https://github.com/kubernetes-sigs/cluster-addons/pull/45) - Updates CoreDNS readme for kubebuilder 2.0
5. [kubernetes-sigs/kubebuilder-declarative-pattern#96](https://github.com/kubernetes-sigs/kubebuilder-declarative-pattern/pull/96) - Removes some steps that are now been built into kubebuidler
6. [kubernetes-sigs/cluster-addons#78](https://github.com/kubernetes-sigs/cluster-addons/pull/78) - Centralize Documentations and README.md


## Further Work
A lot of work was definitely done on the cluster add-ons during the GSoC period. But we need more people building operators and using them in the cluster. We need wider adoption in the community. Build operators for your favourite addons and tell us how it went or if you had any issues. If you are interested in building an operator, Please check out this [README.md](https://github.com/kubernetes-sigs/cluster-addons/blob/master/dashboard/README.md).

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
