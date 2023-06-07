# CloudRail Assignment Task: Automate the Deployment and Monitoring of a Web Application Using Open-Source Tools

#### Things to keep in mind: 
  - Application has 2 components which Frontend and Backend
  - Frontend is based on Angular Framework and is of Node 8 version and Angular Cli Version 1.7.3
  - Backend is based on Nodejs and express and is of Node 8 version
  - Application also use MongoDB as a Datatbase. I have used MongoDB Atlas Cluster URI.


#### CICD Flow used for the Project :
  - Source Code is pushed to Github Repository.
  - For Automating Continous Integration , I have used Github Action Workflow which works similar to Jenkins, GitLab CI,etc.
  - I have created ci.yml file which has all the Stages and Step. File is located in .github/workflows directory.
  - The Stages Constitute stages like : Build, Test & Deploy.
  - For Continous Deployment , I have used ArgoCD which is a GitOps Controller and Open Source Tool.


#### Stage 1 in Github Action Workflow : Build & Push
  - In this Stage, I am Checking Out the Code Installing Trivy and Setting Up NodeJs Environment.
  - Trivy is a free and open source tool to check vulnerabilities and scan containers as well as Images build using Docker.
  - Using Docker , I dockerised the Frontend & Backend of the application using Dockerfiles for the respective .
  - After Dockerizing the Frontend & backend , Images Are scanned by trivy and pushed to DockerHub Registery. 


<img width="1440" alt="Screenshot 2023-06-07 at 12 46 42 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/269a8f56-5e39-45a7-8f8f-01f8cd59dc77">


<img width="1440" alt="Screenshot 2023-06-07 at 12 47 15 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/6458e51a-aca7-462a-9d39-7aa59b48e701">


#### Stage 2 in Github Action Workflow : Test Frontend & Backend
  - In this Stage, I am Testing the Build Dependencies Installed in Frontend & Backend.
  - I have Setup the Required Node Verison to install Dependencies and Test.

<img width="1440" alt="Screenshot 2023-06-07 at 12 47 35 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/145a420c-8356-45e4-97ab-1c09b3cf8a1d">


<img width="1440" alt="Screenshot 2023-06-07 at 12 47 56 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/cdeacae9-edcc-42e5-96e4-20da11eaf05d">


#### Stage 3 in Github Action Workflow : Update Manifest & Deploy Application
  - In this Stage, I am updating the YML Manifests with the updated Image build during dockerizing.
  - After completing the above 2 stages we are updating the Image version on a different repository specifically maintained to store        manifest files used for Kubernetes Cluster.
  - After Updating the Deployment.yml for both frontend and backend, I pushed the code with the new commit.



<img width="1440" alt="Screenshot 2023-06-07 at 12 48 21 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/a72e3f88-fe78-4ddf-9026-9772d970937f">



<img width="1440" alt="Screenshot 2023-06-07 at 12 45 59 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/3600bacf-e7b4-4694-b7ec-6948ad99464c">



# Creating AWS EKS Cluster to deploy Application

#### Pre-requisites: 
  - an EC2 Instance (Note : If Using Ubuntu EC2 Instance instead of Amazon Linux then Make Sure to have **aws-iam-authenticator** installed.)


#### Article to Install aws-iam-authenticator :
```sh
https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
```

#### AWS EKS Setup 
1. Setup kubectl   
   a. Download kubectl  
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    
   ```sh 
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version --short --client
   ```
2. Setup eksctl   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
  
3. Create an IAM Role and attach it to EC2 instance    
   `Note: create IAM user with programmatic access if your bootstrap system is outside of AWS`   
   IAM user should have access to   
   IAM   
   EC2   
   VPC    
   CloudFormation
   EKS
   Administrator

4. Create your cluster and nodes 
   ```sh
   eksctl create cluster --name cluster-name  \
   --region region-name \
   --node-type instance-type \
   --nodes-min 2 \
   --nodes-max 2 \ 
   --zones <AZ-1>,<AZ-2>
    ```


# Installing ArgoCD in EKS cluster to make Continous Deployment 

#### Prerequisites:
  - An existing EKS Cluster.
  - AWS Load Balancer Controller Installed.














  
