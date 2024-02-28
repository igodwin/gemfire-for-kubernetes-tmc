# Deploying Envoy Gateway Using Kustomization
This Kustomization definition can be added to Tanzu Mission Control (TMC) to deploy and manage [Envoy Gateway](https://gateway.envoyproxy.io/)

## Getting Started
To deploy Envoy Gateway using Kustomization, you will need to add a new Kustomization to the Cluster or Cluster Group. This task is performed on TMC.

### 1. Add the `envoy-gateway` Kustomization
Follow the procedure provided in the TMC documentation: [Add a Kustomization to a Cluster or Cluster Group
](https://docs.vmware.com/en/VMware-Tanzu-Mission-Control/services/tanzumc-using/GUID-99916A6D-5DAF-4A26-88C7-28662F847F2F.html)

Use the following values when completing the form:
* Use `envoy-gateway` as the name of the Kustomization (step 5 in TMC documentation)
* Use the Git repository, `gemfire-for-kubernetes-tmc`, that was added during [step 3](#3-add-a-git-repository) of [Getting Started](#getting-started) (step 7 in TMC documentation)
* Use `envoy-gateway` as the path (step 8 in TMC documentation)