# Insider - Assignment

## Load Testing

[Locust](https://locust.io/) and [Bombardier](https://github.com/codesenberg/bombardier) were used to perform the load testing of the application. Results have been added in [load-test-results](./load-test-results) directory.


### [bombardier](https://github.com/codesenberg/bombardier) - Fast cross-platform HTTP benchmarking tool written in Go

```
------------------------------------------- Test - 1: One million requests  -----------------------------------------------
Bombarding http://aa6a9c606e2bd-825656066.ap-south-1.elb.amazonaws.com:3000 with 1000000 request(s) using 125 connection(s)
 1000000 / 1000000 [================================================================================] 100.00% 10927/s 1m31s
Done!
Statistics        Avg      Stdev        Max
  Reqs/sec     10929.15    2767.63   18726.61
  Latency       11.43ms     9.61ms   276.30ms
  HTTP codes:
    1xx - 0, 2xx - 1000000, 3xx - 0, 4xx - 0, 5xx - 0
    others - 0
  Throughput:     3.54MB/s
---------------------------------------------------------------------------------------------------------------------------
```

### [locust](https://locust.io/) - An open source load testing tool written in Python

```
 Name                                            no. of reqs     # fails     Avg   Min   Max  |  Median   req/s
---------------------------------------------------------------------------------------------------------------------------
 GET /                                             1190           0(0.00%)    6     4    22   |      6    10.00
---------------------------------------------------------------------------------------------------------------------------
 Total                                             1190           0(0.00%)                                10.00


Percentage of the requests completed within given times   
 Name                                          no. of reqs    50%    66%    75%    80%    90%    95%    98%    99%   100%
---------------------------------------------------------------------------------------------------------------------------
 GET /                                             1190        6      6      7      7      7      7      9     10     22
---------------------------------------------------------------------------------------------------------------------------
 Total                                             1190        6      6      7      7      7      7      9     10     22

```

---

## Helm chart and Kubernetes manifests have been deployed and tested on

* [Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/) - v1.16
* [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine) - v1.16.8
* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) - v1.10.1

## Assumptions to deploy on AWS

* An EKS Cluster is running with enough resources to run the deployment
* Necessary tools like [helm](https://v2.helm.sh/), [aws-cli](https://aws.amazon.com/cli/), [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/), etc. are installed and configured to point to appropriate EKS cluster

## Tools (and their versions) used to complete this task

* [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) - v1.15.3
* [AWS Command Line Interface](https://aws.amazon.com/cli/)- v2.0.16
* [helm](https://v2.helm.sh/)- v2.16.1

## Initializing required variables

* Before we get started with the deployment, initialize some environment variables to make things simpler

    ```sh
    # Replace below values with appropriate ones
    export AWS_ACCOUNT_ID=0123456789
    export REGION=ap-south-1
    export REPOSITORY=nodejs-test
    export EKS_CLUSTER_NAME=insider
    export HELM_RELEASE=test-release

    ```

---

## Deployment Instructions

### 1. Building and pushing the docker image to [ECR](https://aws.amazon.com/ecr/)

The "[nodejs-docker](./nodejs-docker)" directory has necessary Dockerfile and node-js files to build the docker image. Refer to its [README](./nodejs-docker/README.md) for more.

### 2. Deploy using [helm-chart](./helm-chart)

For more details on deploying this application using provided helm-chart, refer to its [README](./helm-chart/README.md)

### 3. Deploy using [k8s-manifests](./k8s-manifests)

For more details using kubectl for the deployment, refer to its [README](./k8s-manifests/README.md)
