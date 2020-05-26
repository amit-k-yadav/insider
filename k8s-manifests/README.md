# Kubernetes manifests

* For this type of deployment, imagePullSecret needs to be created manually before proceeding with the deployment. Let's quickly do that.

```sh

# Get the latest token
export TOKEN=`aws ecr get-login-password --region ${REGION}`


# Create the secret to be used to pull the image
kubectl create secret docker-registry paytm-insider \
   --docker-server=https://${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com \
   --docker-username=AWS \
   --docker-password=${TOKEN}

```

* Also, the image repository needs to be manually changed in [deployment.yaml](./deployment.yaml#L26) file.

```yaml
...
- image: 0123456789.dkr.ecr.ap-south-1.amazonaws.com/nodejs-test:latest
...
```

* Finally, deploy the application and verify the running service

```sh

# create the resources
kubectl apply -f ./


# Get the external endpoint of the LoadBalancer service
export SERVICE_ENDPOINT=`kubectl get service/paytm-insider \
  -o jsonpath="{..status..loadBalancer..ingress..hostname}"`

curl http://${SERVICE_ENDPOINT}:3000

---
## Output
Hello, welcome to Paytm Insider
---

```

### NOTE:

1. [priorityClass.yaml](./priorityClass.yaml) creates a custom priorityClass. This is only needed if you Kubernetes version is 1.16 or below and not you are not using `kube-system` namespace. 
2. This is because the Kubernetes is shipped with `system-cluster-critical` and `system-node-critical` priority classes, and for kubernetes v1.16 and older version, these classes are limited to kube-system namespace.
3. For every other namespace, we can create our own priority-class
