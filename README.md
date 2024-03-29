# Deploying VMware Data Services in Tanzu Mission Control (TMC)

## Purpose
This repository exists as an example of how to deploy VMware data services or solutions, such as GemFire for Kubernetes, using TMC. Even with the absence of a native Tanzu package or available Helm chart in the TMC catalog, it is still possible to add desired services by leveraging the GitOps and FluxCD components that are integrated in TMC.

## Prerequisites
* Valid log in for [Tanzu Mission Control Console](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-855A8998-E19A-46AC-A833-12C347486EF7.html)
* Be associated with the `cluster.admin` or `clustergroup.admin` role in TMC to perform the following:
    * [Enable Helm Service on Your Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-0927CDC8-A5C1-4FAE-9A7C-8A5D62FDF8D8.html)
    * [Enable Continuous Delivery for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-AB0A6CCE-1CA4-4BDC-B22E-BFE2E2BD8D7F.html)
    * [Create a Repository Credential for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-657661A2-B26E-412A-9A46-7467A44A075A.html)
    * [Add a Git Repository to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-26C2D2F3-0E5C-4E56-B875-B7FB003267E4.html)
    * [Add a Kustomization to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)
* A Cluster or Cluster Group to configure (see [Managing Clusters](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-D8623CB6-D821-4837-A73A-6163BC4132A4.html) if needed)
* Valid log in for any image repositories used (e.g. [VMware Tanzu Network](https://network.tanzu.vmware.com/))

## Getting Started
With the prerequisites satisfied, a Git repository needs to be added to the target Cluster or Cluster Group, then the `base` Kustomization. The Git repository must be named `gemfire-for-kubernetes-tmc` to use this repo as-is. Each task is performed on TMC.

#### 1. Enable Helm Service
Follow the procedure provided in the TMC documentation: [Enable Helm Service on Your Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-0927CDC8-A5C1-4FAE-9A7C-8A5D62FDF8D8.html)

#### 2. Enable Continuous Delivery
Follow the procedure provided in the TMC documentation: [Enable Continuous Delivery for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-AB0A6CCE-1CA4-4BDC-B22E-BFE2E2BD8D7F.html)

#### 3. (Optional) Create a Repository Credential
When wanting to add a private Git repository, you will need to first create a repository credential. To do this, simply follow the procedure provided in the TMC documentation: [Create a Repository Credential for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-657661A2-B26E-412A-9A46-7467A44A075A.html)

#### 4. Add a Git Repository
Remember that the name of the Git repository must be `gemfire-for-kubernetes-tmc` to use this repo as-is, and the branch should be set to `main` (step 8 in TMC documentation). Follow the procedure provide in the TMC documentation: [Add a Git Repository to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-26C2D2F3-0E5C-4E56-B875-B7FB003267E4.html)

#### 5. Add the `base` Kustomization
Follow the procedure provided in the TMC documentation: [Add a Kustomization to a Cluster or Cluster Group
](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)

Use the following values when completing the form:
* Use `base` as the name of the Kustomization (step 5 in TMC documentation)
* Use the Git repository, `gemfire-for-kubernetes-tmc`, that was added during [step 4 of Getting Started](#4-add-a-git-repository) (step 7 in TMC documentation)
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
NAME                                          AGE     READY     STATUS
base                                          4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
cert-manager                                  4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
envoy-gateway                                 4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
envoy-gateway-class                           4h51m   False     dependency 'tanzu-continuousdelivery-resources/gateway-helm-release' is not ready
envoyproxy-repository                         4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
gateway-helm-release                          4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
gemfire-for-kubernetes                        4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
gemfire-for-kubernetes-crd                    4h51m   False     dependency 'tanzu-continuousdelivery-resources/gemfire-for-kubernetes-repository' is not ready
gemfire-for-kubernetes-operator               4h51m   False     dependency 'tanzu-continuousdelivery-resources/gemfire-for-kubernetes-crd' is not ready
gemfire-for-kubernetes-registry-credentials   31s     True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
gemfire-for-kubernetes-repository             4h51m   Unknown   Reconciliation in progress
package-and-service-instances-namespaces      4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
sql-postgres                                  3h8m    True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
sql-postgres-operator                         3h8m    False     dependency 'tanzu-continuousdelivery-resources/sql-postgres-repository' is not ready
sql-postgres-registry-credentials             31s     True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
sql-postgres-repository                       3h8m    False     dependency 'tanzu-continuousdelivery-resources/tanzu-packages-cert-manager' is not ready
tanzu-gitops-serviceacct                      4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
tanzu-packages-cert-manager                   4h51m   True      Applied revision: main@sha1:f1769d9c9b007a431831d39c80c853c6c539b8a9
```

### Examine Kubernetes Secrets
A lot of the image repositories referenced in this project are private, and require credentials to be specified. In the case of an issue with a `HelmRepository`, `HelmRelease`, or any other image pull, check the `README.md` for the specific data services product you are deploying to make sure you have the correct registry secret(s) created in TMC.

### Missing Server API
Several required CRDs and operators are installed when enabling [Helm Service](#1-enable-helm-service) and [Continuous Delivery](#2-enable-continuous-delivery) on a Cluster or Cluster Group. If, when examining Kustomizations, the message `HelmRelease/envoy-gateway-system/gateway-helm dry-run failed: failed to get API group resources: unable to retrieve the complete list of server APIs: helm.toolkit.fluxcd.io/v2beta1: the server could not find the requested resource` is encountered, check to make sure the features have been enabled.
