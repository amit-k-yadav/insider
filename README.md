# Insider - Assignment

## Introduction and assumptions

### Provided Helm chart and Kubernetes manifests have been deployed and tested on the following environments

* [Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/) - v1.16
* [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine) - v1.16.8
* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) - v1.10.1

### Assumptions to deploy on AWS

* An EKS Cluster is running with enough resources to run the deployment
* Necessary tools like [helm](https://v2.helm.sh/), [aws-cli](https://aws.amazon.com/cli/), [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/), etc. are installed and configured to point to appropriate EKS cluster

### Tools (and their versions) used to complete this task

* [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) - v1.15.3
* [AWS Command Line Interface](https://aws.amazon.com/cli/)- v2.0.16
* [helm](https://v2.helm.sh/)- v2.16.1

### Initializing required variables

Before get started with the deployment, initialize some environment variables to make things simpler

```sh
# Replace below values with appropriate ones
export AWS_ACCOUNT_ID=0123456789
export REGION=ap-south-1
export REPOSITORY=nodejs-test
export EKS_CLUSTER_NAME=insider
export HELM_RELEASE=test-release

```
