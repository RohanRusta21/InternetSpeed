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

#### Step 1 : We Create a namespace and Install Manifest for ArgoCD
  
  ```sh
  kubectl create ns argocd
  kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml -n argocd
  ```
  
  
  <img width="1160" alt="Screenshot 2023-06-07 at 12 05 16 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/7279ef80-c32e-4d63-8b5f-33297dfc3eb0">

  
  
  

#### Step 2 : Configuring ArgoCD
  
  - Here I have Edited the ArgoCD Server Manifest file from ClusterIP to LoadBalancer to access the ArgoCD UI in Browser.
  
  ```sh
  kubectl edit svc argocd-server -n argocd
  ```
  
  ![Screenshot 2023-06-07 at 12 06 04 AM](https://github.com/RohanRusta21/InternetSpeed/assets/110477025/5ac500e6-fc85-4e7e-8598-a80b74ead75a)

  
  
  
<img width="1440" alt="Screenshot 2023-06-07 at 12 00 01 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/7770e78f-fcd4-4d7c-be55-edba97183bdb">



- Setting Up the manifest repository in ArgoCD.


<img width="1440" alt="Screenshot 2023-06-07 at 12 01 02 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/4f320be1-8d28-4214-ac55-f56b215ba290">





<img width="1440" alt="Screenshot 2023-06-07 at 12 03 13 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/cdd3f802-1964-4783-8587-a3f4a6a89879">




# Successfully Deployed Our Web Application

 - I have used LoadBalancer in my Service yml manifest to access the application outside the cluster



<img width="1440" alt="Screenshot 2023-06-07 at 12 07 01 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/ce4a8d9b-6824-4d4b-822d-26adb673f87b">



## Cluster Monitoring using Prometheus & Grafana

Key Components :

- Prometheus server - Processes and stores metrics data
- Alert Manager - Sends alerts to any systems/channels
- Grafana - Visualize scraped data in UI

Pre Requisites :
- EKS Cluster is setup already
- Install Helm
- EC2 instance to access EKS cluster

Installation Steps 
```sh
helm repo add stable https://charts.helm.sh/stable
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm search repo prometheus-community
kubectl create namespace prometheus
helm install stable prometheus-community/kube-prometheus-stack -n prometheus
kubectl get pods -n prometheus
kubectl get svc -n prometheus
```

<img width="1440" alt="Screenshot 2023-06-07 at 12 11 12 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/310078d6-6325-4d16-9e79-f25b1ca6da90">





Edit Prometheus Service (Edit type : LoadBalancer)
```sh
kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus
```

Edit Grafana Service (Edit type : LoadBalancer) 
```sh
kubectl edit svc stable-grafana -n prometheus
```

Verify if service is changed to LoadBalancer and also to get the Load Balancer URL.
```sh
kubectl get svc -n prometheus
```



<img width="1440" alt="Screenshot 2023-06-07 at 12 11 40 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/17cb107e-2974-493d-bbf4-4f759b4a0682">




<img width="1440" alt="Screenshot 2023-06-07 at 12 13 00 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/47be03ab-8568-41e8-bf85-b25bb4e7f17e">




Access Grafana Dashboard
```sh
UserName: admin 
Password: prom-operator
```

<img width="1440" alt="Screenshot 2023-06-07 at 12 15 32 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/40efcfec-157c-42a1-8f9e-349c038ddbc9">


![Screenshot 2023-06-07 at 12 16 32 AM](https://github.com/RohanRusta21/InternetSpeed/assets/110477025/5be592cf-d01d-481a-ac24-c3f10c518d95)




- Prometheus UI also used LoadBalancer to access in Browser

<img width="1440" alt="Screenshot 2023-06-07 at 12 17 24 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/b7967d9b-56d4-4361-8fec-755c549ea026">




#### Creating Customised Monitoring Dashboard using Prometheus & Grafana

  - Prometheus is used to gather the dynamic realtime timeseries metrics of nodes from kubelet and we used it to give data to grafana so that we can used it to visualize
  - In Grafana I have used Prometheus as a data source and grafana retrieves data from prometheus using queries.


<img width="1440" alt="Screenshot 2023-06-07 at 12 18 18 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/5293598b-c1fb-47b2-92e1-957c4aa687be">



# Our Dynamic Customised Monitoring Dashboard for our cluster


  - I have used Memory & CPU metrics for the pods and nodes.
  - For checking the realtime scaling and replicas of the deployment we can use other queries to retrieve data.
  - Frontend & Backend Containers can also be monitored in the dashboard.



<img width="1440" alt="Screenshot 2023-06-07 at 12 43 36 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/3b79b96f-704f-4278-b164-d726692ddfac">

  
