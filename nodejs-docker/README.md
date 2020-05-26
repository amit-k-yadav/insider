# Building and pushing a docker image to ECR

To push a docker-image to ECR, we first need to create a registry. Assuming [aws CLI](https://aws.amazon.com/cli/) is configured, run the following commands to create an ECR registry, build and push the image to ECR:

```sh

# Create an ECR repository
aws ecr create-repository --repository-name ${REPOSITORY}


# Build the docker image from nodejs-docker directory
docker build -t \ 
    ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${REPOSITORY} .


# Docker-login for ECR
aws ecr get-login-password --region ${REGION} | docker login \
  --username AWS \
  --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com


# Push the docker image to ECR
docker push \
    ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/nodejs-test:latest

```
