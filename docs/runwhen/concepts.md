# RunWhen Concepts
- [RunWhen Concepts](#runwhen-concepts)
- [Runwhen Local](#runwhen-local)
  - [CheatSheet Generator](#cheatsheet-generator)
  - [Uploading Cluster Topology to the Platform](#uploading-cluster-topology-to-the-platform)
- [CodeCollections](#codecollections)
- [CodeBundles](#codebundles)
  - [Platform Definition](#platform-definition)
    - [SLX](#slx)
    - [SLI](#sli)
    - [SLO](#slo)
    - [Runbook / TaskSet](#runbook--taskset)
  - [RunWhen Libraries](#runwhen-libraries)
    - [RunWhen Services](#runwhen-services)
  - [Robot Scripts - Task Runner](#robot-scripts---task-runner)

# Runwhen Local
- [source-code](https://github.com/runwhen-contrib/runwhen-local)
- [Helm Chart](https://github.com/runwhen-contrib/helm-charts/tree/main/charts/runwhen-local)
- [Upstream docs](https://docs.runwhen.com/public/v/runwhen-local/)
  
RunWhen Local has two core functions:
- Generate remediation scripts / CheatSheets from included templates for your local cluster
- Upload Cluster Topology to the RunWhen Platform

## CheatSheet Generator
At the moment RunWhen Local does not posses the ability to discover issues in
your cluster and suggest mitigation runbooks / codebundles.

However, it generates a wide set of runbooks for you, if you already know the
root cause. These runbooks contain documentation and pastable shell script
snippets for the searched issue. These scripts / cheatsheet are already pre-templated
with your namespaces and kubernetes resource names.

The collection of cheatsheets / runbooks although not exhaustive, cover a significant portion
of recurring issues and healthcheck failures and can be useful to SREs for quick
resolution of incidents.

[Upstream Examples](https://docs.runwhen.com/public/v/runwhen-local/user-guide/features/user_guide-feature_overview)

## Uploading Cluster Topology to the Platform
The second core function of runwhen-local is to upload cluster topology to the
runwhen platform so you can visualize the cluster workload map from a configured
runwhen workspace.

- First, follow documentation at [Upload to RunWhen Platform](https://docs.runwhen.com/public/v/runwhen-local/user-guide/features/upload-to-runwhen-platform#upload-from-the-cli)
  - To generate the `uploadInfo.yaml` file
- Next, take the yaml object and copy over it's contents to `uploadInfo:[]` section
of the helm [`values.yaml` file](https://github.com/runwhen-contrib/helm-charts/blob/main/charts/runwhen-local/values.yaml#L121)
- Once configured it should look like this:
  ```
  uploadInfo:
    workspaceName: <your-workspace-name>
    token: <your token> # It's not advised to paste the actual token in the values.yaml file and commit it to git
    workspaceOwnerEmail: tester@my-company.com
    papiURL: https://papi.beta.runwhen.com
    defaultLocation: location-01-us-west1 # Or, any other available runwhen location
  ```
- You should pass the token from helm cli, to ensure you are not leaking the token via git
  ```
  helm upgrade --install  ${HELM_RELEASE_NAME} runwhen-contrib/runwhen-local \
    --set uploadInfo.token=${RUNWHEN_PLATFORM_TOKEN} \
     -f ${VALUES_FILE} -n ${NAMESPACE}
  ```

# CodeCollections
CodeCollections are a group of CodeBundles that can be referenced and used in RunWhen Platform.

*N.B. It's important to note here that currently codecollections cannot be imported explicitly and run against your local cluster using RunWhen Local*

Currently RunWhen has published two codecollections:
- [runwhen-public-codecollection](https://github.com/runwhen-contrib/rw-public-codecollection)
  - These contain codebundles that are usually run against services and doesn't involved a Shell / CLI component
- [runwhen-cli-codecollection](https://github.com/runwhen-contrib/rw-cli-codecollection)
  - These are generally targeted towards SRE workloads and wraps various shell-scripts and CLI tooling.

# CodeBundles
  - YAML Configuration (for platform)
  - Robot Framework scripts
  - RunWhen Libraries for Robot Framework
  - Additional Binaries
  - Writing a non-trivial CodeBundle
## Platform Definition
### SLX
### SLI
### SLO
### Runbook / TaskSet

## RunWhen Libraries
### RunWhen Services

## Robot Scripts - Task Runner
