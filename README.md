# Introduction

This is a sample application used to demonstrate a POC of using GitHub Actions to deploy to AWS ECR and Fargate.

# Architecture Diagram

![githubactionecr drawio](https://user-images.githubusercontent.com/48310743/232531154-c0dd01d5-8666-4619-af29-aa2d7c2a7e7b.png)

# 3.5 Assignment
### Assignment Rrquirement:

- Create a new repository
- Add sample code to your repository including the dockerfile on it
- Update your repository Readme.md file to have step-by-step how to create an image until it will be deployed to AWS ECR

#### Step-By-Step Guide for how to create an image 
-----------------------------------------

- Taken image from debain slim 

      FROM debian:bookworm-slim 

- update manager for update current docker on our system

      RUN apt-get update 

- For curl install

       RUN apt-get install -y curl   

- Download the setup script

      RUN curl -fsSL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh         

- To Run the setup script with sudo

      RUN bash nodesource_setup.sh      

- To Install the node js 

      RUN apt-get install -y nodejs      

#### Steps to building and pushing Image to Private ECR

- Command to build an image

      docker build -t <IMAGE_NAME>:<IMAGE_TAG> .

- Command example with my image
    
      docker build -t simple-app-image .

- Command to tag an image

      docker tag <IMAGE_NAME>:<IMAGE_TAG> <REPOSITORY_URI>:<IMAGE_TAG>

- Command example with our image and repository

      docker tag simple-app-image:latest <account no.>.dkr.ecr.ap-southeast-1.amazonaws.com/<REPOSITORY_NAME>:latest

- Command to push image:

      docker push <account no.>.dkr.ecr.ap-southeast-1.amazonaws.com/<REPOSITORY_NAME>:latest

#### Deploy to AWS ECR as below:
-----------

- Make sure that you have the latest version of the AWS CLI and Docker installed.
Use the following steps to authenticate and push an image to your repository. For additional registry authentication methods, including the Amazon ECR credential helper, see Registry Authentication .


- Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

      aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/u2q1a2y8


Note: If you receive an error using the AWS CLI, make sure that you have the latest version of the AWS CLI and Docker installed.


- Build your Docker image using the following command. For information on building a Docker file from scratch see the instructions here . You can skip this step if your image is already built:

      docker build -t sara/simple-node-app .


- After the build completes, tag your image so you can push the image to this repository:

      docker tag sara/simple-node-app:latest public.ecr.aws/u2q1a2y8/sara/simple-node-app:latest

- Run the following command to push this image to your newly created AWS repository:

      docker push public.ecr.aws/u2q1a2y8/sara/simple-node-app:latest
