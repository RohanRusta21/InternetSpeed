# CloudRail Assignment Task: Automate the Deployment and Monitoring of a Web Application Using Open-Source Tools

#### Things to keep in mind: 
  - Application has 2 components which Frontend and Backend
  - Frontend is based on Angular Framework and is of Node 8 version and Angular Cli Version 1.7.3
  - Backend is based on Nodejs and express and is of Node 8 version
  - Application also use MongoDB as a Datatbase. I have used MongoDB Atlas Cluster URI.


#### CICD Flow used for the Project :
  - Source Code is pushed to Github Repository.
  - For Automating Continous Integration , I have used Github Action Workflow which works similar to Jenkins, GitLab CI,etc.
  - I have created ci.yml file which has all the Stages and Step.
  - The Stages Constitute stages like : Build, Test & Deploy.
  - For Continous Deployment , I have used ArgoCD which is a GitOps Controller and Open Source Tool.
  

#### Stage 1 in Github Action Workflow : Build & Push
  - In this Stage, I am Checking Out the Code Installing Trivy and Setting Up NodeJs Environment
  - Trivy is a free and open source tool to check vulnerabilities and scan containers as well as Images build using Docker.
  - Using Docker , I dockerised the Frontend & Backend of the application using Dockerfiles for the respective .
  - After Dockerizing the Frontend & backend , Images Are scanned by trivy and pushed to DockerHub Registery. 


<img width="1440" alt="Screenshot 2023-06-07 at 12 47 15 AM" src="https://github.com/RohanRusta21/InternetSpeed/assets/110477025/6458e51a-aca7-462a-9d39-7aa59b48e701">
