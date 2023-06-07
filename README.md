# CloudRail Assignment Task: Automate the Deployment and Monitoring of a Web Application Using Open-Source Tools

#### Things to keep in mind: 
  - Application has 2 components which Frontend and Backend
  - Frontend is based on Angular Framework and is of Node 8 version and Angular Cli Version 1.7.3
  - Backend is based on Nodejs and express and is of Node 8 version
  - Application also use MongoDB as a Datatbase. I have used MongoDB Atlas Cluster URI.


#### CICD Flow used for the Project :
  - Source Code is pushed to Github Repository.
  - For Automating CI , I have used Github Action Workflow which works similar to Jenkins, GitLab CI,etc.
  - I have created ci.yml file which has all the Stages and Step.
  - The Stages Constitute stages like : Build, Test & Deploy.
  - For Continous Deployment , I have used ArgoCD which is a GitOps Controller and Open Source Tool.
  

