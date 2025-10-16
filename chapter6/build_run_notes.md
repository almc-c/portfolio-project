## How To Build & Run Locally

using the dockerfile in the chapter6 root, this is the build context

```docker build -t <yourImageName> .```

then check the image

```docker images``` 

then create a container instance from the image with port mapping

```docker run -p 80:80 --name apiContainerName <yourImageName>```

## How to Build & Run Image on AWS Lightsail Container Services
pre-reqs: install AWS CLI https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
install AWS lightsail CLI https://docs.aws.amazon.com/en_us/lightsail/latest/userguide/amazon-lightsail-install-software.html

We are skipping pushing the image to a repo such as ECR, rather we will push it into a container build directly within Lightsail as a local image.

# example commands (thanks AWS Q!)

## Create the container service
```
aws lightsail create-container-service \
    --service-name almc-api-container-svc \
    --power nano \
    --scale 1 \
    #--tags key=Environment,value=Production

# Push your container image
aws lightsail push-container-image \
    --region us-east-2
    --service-name almc-api-container-svc \
    --label almc-api-container \
    --image apicontainerimage:latest


# Create a deployment (after pushing the image)
aws lightsail create-container-service-deployment \
    --service-name almc-api-container-svc \
    --containers "{\"web\": {\"image\": \":apicontainerimage.web.latest\", \"ports\": {\"80\": \"HTTP\"}}}" \
    --public-endpoint "{\"containerName\": \"web\", \"containerPort\": 80}"

aws lightsail create-container-service-deployment     
    --service-name almc-api-container-svc     
    --containers "{\"web\": {\"image\": \":almc-api-container-svc.almc-api-container.latest\", \"ports\": {\"80\": \"HTTP\"}}}"     
    --public-endpoint "{\"containerName\": \"web\", \"containerPort\": 80}"

aws lightsail get-container-services --service-name my-web-app

```