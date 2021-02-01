# OKD4 Example Apps - Deployment Configuration

[![App Status](https://argocd.baloise.dev/api/badge?name=okd4-example-apps-apps)](https://argocd.baloise.dev/applications/okd4-example-apps-apps)

## Setup
In order to work, this repository needs to be referenced in the corresponding team's root configuration yaml in the [okd4-apps-root-config](https://github.com/baloise-incubator/okd4-apps-root-config) repository.
See the [respository property in the configuration](https://github.com/baloise-incubator/okd4-apps-root-config/blob/master/apps/okd4-example-apps.yaml#L1).

## Structure
Every deployment is in its own directory, persistent environments (test, int, acc, prod) are postfixed with the respective environment name.

A configuration for an application named `demo` that is deployed in test therefore is located in the folder `demo-test`.
Every root directoy in this repository needs to be listed as application under the [applications property](https://github.com/baloise-incubator/okd4-apps-root-config).

This is done automatically, whenever you add or remove a directory in the root of this repository. 
You can find the corresponding tekton pipeline configuration for that synchronisation [here](hhttps://github.com/baloise-incubator/okd4-cluster-infra-apps/blob/master/tekton-chatopshandler/syncapps-pipeline.yaml).
Under the hood, the Job uses the [GitOps CLI](https://baloise.github.io/gitopscli/) to perform a checkout of the `okd4-apps-root-config` and adjusts the applications property accordingly to the list of root directories in this repository.

Keep in mind: Only `master` branch is considered for all of those interactions. 
The content of each directory either contains just Resource Definitions as plain YAML, a [Helm Chart](https://helm.sh/) or a [kustomize yaml configuration](https://github.com/kubernetes-sigs/kustomize).
If you use Helm or Kustomize, the template engine is identified automatically.

## Continuous Delivery
[Argo CD](https://argoproj.github.io/argo-cd/) is used to manage the deployments and sync any changes, based on a webhook in this repository, into the cluster.
You can access the WebUI for baloise incubator [here](https://argocd.baloise.dev/).