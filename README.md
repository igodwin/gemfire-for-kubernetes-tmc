# Deploying VMware Data Services through Tanzu Mission Control (TMC)

### Purpose
This repository exists as an example of how to deploy VMware data services or solutions, such as GemFire for Kubernetes, using TMC. Even with the absense of a native Tanzu package or available Helm chart in the TMC catalog, it is still possible to add desired services by leveraging the GitOps and FluxCD components that are integrated in TMC.

### Prerequisites
* Valid log in for [Tanzu Mission Control Console](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-855A8998-E19A-46AC-A833-12C347486EF7.html)
* Be associate with the `clsuter.admin` or `clustergrou.admin` role in TMC to perform the following:
    * [Enable Continuous Delivery for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-AB0A6CCE-1CA4-4BDC-B22E-BFE2E2BD8D7F.html)
    * [Create a Repository Credential for a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-657661A2-B26E-412A-9A46-7467A44A075A.html)
    * [Add a Git Repository to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-26C2D2F3-0E5C-4E56-B875-B7FB003267E4.html)
    * [Add a Kustomization to a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)
* A Cluster or Cluster Group to configure (see [Managing Clusters](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-D8623CB6-D821-4837-A73A-6163BC4132A4.html) if needed)
* Valid log in for any image repositories used (e.g. [VMware Tanzu Network](https://network.tanzu.vmware.com/))

### Getting Started
With the prerequisites satisfied, a Git repository needs to be added to the target Cluster or Cluster Group, then the `base` kustomization. The Git repository must be named `gemfire-for-kubernetes-tmc` to use this repo as-is. Each task is performed on TMC.

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


### Next Steps
Once you've configured the Cluster or Cluster Group with the `base` Kustomization, you are then ready to continue on to adding the various Kustomizations that will deploy any desired data service product. This repository contains Kustomizations for the following products, each with instructions for adding:

* [VMware GemFire for Kubernetes](gemfire-for-kubernetes/README.md)
* [VMware SQL with Postgres for Kubernetes](sql-postgres/README.md)
