# Helm Deployment

* Kubernetes has support for [imagePullSecrets](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/) to pull images from private repositories (like [ECR](https://aws.amazon.com/ecr/), [GCR](https://cloud.google.com/container-registry), etc.). We can create a Kubernetes secret of type `kubernetes.io/dockerconfigjson` and use the same in deployments

* I have tried to "helmify" this step using Helm’s [_helpers.tpl](./templates/_helpers.tpl#L58). To know more see ["Creating Image Pull Secrets - Chart Development Tips and Tricks"](https://helm.sh/docs/howto/charts_tips_and_tricks/)

* Now, presuming Helm is initialized for the running EKS cluster, to create a `imagePullSecret` on the fly and use that for the deployment, run the following commands:

    ```sh

    # Update kube-config to point to the running EKS
    aws eks --region ${REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}


    # Get token and use the to create a k8s secret
    export TOKEN=`aws ecr get-login-password --region=${REGION}`


    # Deploy the Helm chart changing appropriate variables below
    helm install --name ${HELM_RELEASE} --set image.tag=latest,\
        imageCredentials.create=true,\
        image.repository=${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${REPOSITORY},\
        imageCredentials.registry=https://${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com,\
        imageCredentials.username=AWS,\
        imageCredentials.password=${TOKEN} ./

    ```

* Now that we have our app deployed on EKS, let’s verify if the service is working

    ```sh
    # Get the external endpoint of the LoadBalancer service
    export SERVICE_ENDPOINT=`kubectl get service/${HELM_RELEASE}-insider \
    -o jsonpath="{..status..loadBalancer..ingress..hostname}"`


    curl http://${SERVICE_ENDPOINT}:3000

    ---
    ## Output
    Hello, welcome to Paytm Insider
    ---
    ```
