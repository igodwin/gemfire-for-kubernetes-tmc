# Deploying VMware Data Services in Tanzu Mission Control (TMC)

## Purpose
This repository exists as an example of how to deploy VMware data services or solutions, such as GemFire for Kubernetes, using TMC. Even with the absence of a native Tanzu package or available Helm chart in the TMC catalog, it is still possible to add desired services by leveraging the GitOps and FluxCD components that are integrated in TMC.

## Prerequisites
* Valid log in for [Tanzu Mission Control Console](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-855A8998-E19A-46AC-A833-12C347486EF7.html)
* Be associated with the `cluster.admin` or `clustergroup.admin` role in TMC to perform the following:
    * [Enable Continuous Delivery for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-AB0A6CCE-1CA4-4BDC-B22E-BFE2E2BD8D7F.html)
    * [Create a Repository Credential for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-657661A2-B26E-412A-9A46-7467A44A075A.html)
    * [Add a Git Repository to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-26C2D2F3-0E5C-4E56-B875-B7FB003267E4.html)
    * [Add a Kustomization to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)
* A Cluster or Cluster Group to configure (see [Managing Clusters](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-D8623CB6-D821-4837-A73A-6163BC4132A4.html) if needed)
* Valid log in for any image repositories used (e.g. [VMware Tanzu Network](https://network.tanzu.vmware.com/))

## Getting Started
With the prerequisites satisfied, a Git repository needs to be added to the target Cluster or Cluster Group, then the `base` Kustomization. The Git repository must be named `gemfire-for-kubernetes-tmc` to use this repo as-is. Each task is performed on TMC.

#### 1. Enable Continuous Delivery
Follow the procedure provided in the TMC documentation: [Enable Continuous Delivery for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-AB0A6CCE-1CA4-4BDC-B22E-BFE2E2BD8D7F.html)

#### 2. (Optional) Create a Repository Credential
When wanting to add a private Git repository, you will need to first create a repository credential. To do this, simply follow the procedure provided in the TMC documentation: [Create a Repository Credential for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-657661A2-B26E-412A-9A46-7467A44A075A.html)

#### 3. Add a Git Repository
Remember that the name of the Git repository must be `gemfire-for-kubernetes-tmc` to use this repo as-is. Follow the procedure provide in the TMC documentation: [Add a Git Repository to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-26C2D2F3-0E5C-4E56-B875-B7FB003267E4.html)

#### 4. Add the `base` Kustomization
Follow the procedure provided in the TMC documentation: [Add a Kustomization to a Cluster or Cluster Group
](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)

Use the following values when completing the form:
* Use `base` as the name of the Kustomization (step 5 in TMC documentation)
* Use the Git repository, `gemfire-for-kubernetes-tmc`, that was added during [step 3](#3-add-a-git-repository) of [Getting Started](#getting-started) (step 7 in TMC documentation)
* Use `base` as the path (step 8 in TMC documentation)


## Next Steps
Once you've configured the Cluster or Cluster Group with the `base` Kustomization, you are then ready to continue on to adding the various Kustomizations that will deploy any desired data service product. This repository contains Kustomizations for the following products, each with instructions for adding:

* [VMware GemFire for Kubernetes](gemfire-for-kubernetes/README.md)
* [VMware SQL with Postgres for Kubernetes](sql-postgres/README.md)

## Troubleshooting
Despite our best efforts, issues can arise. With that in mind, here are some notes on what to look at when things aren't working as expected.

### Examine Kustomizations
There are a plenty of opportunities for something to go wrong, or appear to go wrong, during the reconciliation of Kustomizations, their dependencies, or their health checks. Often, you can discover the answer by listing the Kustomizations, and describing specific ones. All of the Kustomizations created from this repository should be located in the `tanzu-continuousdelivery-resources` namespace.

```
❯ kubectl get kustomizations -n tanzu-continuousdelivery-resources
NAME                                       AGE     READY     STATUS
base                                       5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
cert-manager                               4h24m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
envoy-gateway                              5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
envoy-gateway-class                        5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
envoyproxy-repository                      5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
gateway-helm-release                       5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
gemfire-for-kubernetes                     6s      True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
gemfire-for-kubernetes-crd                 6s      False     dependency 'tanzu-continuousdelivery-resources/gemfire-for-kubernetes-repository' is not ready
gemfire-for-kubernetes-operator            6s      False     dependency 'tanzu-continuousdelivery-resources/gemfire-for-kubernetes-crd' is not ready
gemfire-for-kubernetes-repository          5s      Unknown   Reconciliation in progress
package-and-service-instances-namespaces   5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
sql-postgres                               20m     True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
sql-postgres-operator                      20m     True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
sql-postgres-repository                    20m     True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
tanzu-gitops-serviceacct                   5h36m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
tanzu-network-credentials-export-import    15m     True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
tanzu-packages-cert-manager                4h24m   True      Applied revision: main@sha1:c3392958545b4c0f6185a4461a0ab8e12286ac06
```
**Note:** There are some Kustomization* objects that are part of multiple data service products. An example of this in the list above is `tanzu-network-credentials-export-import`. When multiple Kustomizations are added to TMC that reference the Kustomization object on the Kubernetes cluster, you may see the FluxCD controller switching label values from one reconciliation of the object to the next. This should not impact the operability of the data services products, but you may observe the Kustomization object being temporarily gone if you were to purposefully delete the data service product that happened to be listed as the value of the `kustomize.toolkit.fluxcd.io/name` label at that same time. If this does occur, the Kustomization object will be automatically recreated during the next reconciliation.

### Examine Kubernetes Secrets
A lot of the image repositories referenced in this project are private, and require credentials to be specified. In the case of an issue with a `HelmRepository`, `HelmRelease`, or any other image pull, check the `README.md` for the specific data services product you are deploying to make sure you have the correct registry secret(s) created in TMC.