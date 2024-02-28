# Deploying VMware SQL with Postgres for Kubernetes Using Kustomization
This Kustomization definition can be added to Tanzu Mission Control (TMC) to deploy and manage [VMware SQL with Postgres for Kubernetes](https://docs.vmware.com/en/VMware-SQL-with-Postgres-for-Kubernetes/2.3/vmware-postgres-k8s/GUID-index.html)

## Prerequisites
* [`cert-manager`](../cert-manager/README.md) Kustomization

## Getting Started
To deploy SQL with Postgres for Kubernetes using Kustomization, you will need to create a registry secret named `tanzu-registry-credentials` in the `default` namespace, using TMC. Then, you may add a new Kustomization to the Cluster or Cluster Group. Each of these tasks is performed on TMC.

#### 1. Create Registry Secret (if it does not already exist)
This secret will be automatically exported from the `default` namespace and imported into any necessary namespaces. It is used to authenticate with VMware Tanzu Network for the Helm chart and operator image. Follow the procedure provided in the TMC documentation: [Create a Kubernetes Secret in a Cluster or Cluster Group](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-BBE2404D-C2EE-41C7-B639-C0322783A74D.html)

Use the following values when completing the form:
* Use `tanzu-registry-credentials` as the name (step 5 in TMC documentation)
* Use `default` as the namespace (step 6 in TMC documentation)
* With the secret type being `Registry secret`, use `registry.tanzu.vmware.com` as the Image registry URL (step 7, part A, in TMC documentation)
* Use your own, valid VMware Tanzu Network username and password (step 7, part B, in TMC documentation)

#### 2. Add the `sql-postgres` Kustomization
Follow the procedure provided in the TMC documentation: [Add a Kustomization to a Cluster or Cluster Group
](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)

Use the following values when completing the form:
* Use `sql-postgres` as the name of the Kustomization (step 5 in TMC documentation)
* Use the Git repository, `gemfire-for-kubernetes-tmc`, that was added during [step 3](#3-add-a-git-repository) of [Getting Started](#getting-started) (step 7 in TMC documentation)
* Use `sql-postgres` as the path (step 8 in TMC documentation)