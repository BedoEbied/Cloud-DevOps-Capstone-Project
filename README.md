# Project 5 - Cloud DevOps Engineer Capstone Project

> In this project, I applied my skills and knowledge which was developed throughout the Cloud DevOps Nanodegree program.
## Project Tasks:

* Working in AWS
* Using Jenkins to implement Continuous Integration and Continuous Deployment
* Building pipelines
* Working with CloudFormation to deploy clusters
* Building Kubernetes clusters
* Building Docker containers in pipelines

## About Project: 

> I created a CI/CD pipeline for a basic website that deploys to a cluster in AWS EKS which is Blue/Green Deployment.
<img src="screenshots/Project.png" width="50%" height="50%">


## Project Requirement:

> To be able to use this CI/CD pipeline you will need to install:
* Jenkins
* Blue Ocean Plugin in Jenkins
* Pipeline-AWS Plugin in Jenkins
* Docker
* Pip
* AWS Cli
* Eksctl
* Kubectl

## The files included are:
```sh
* /Screenshots : Screenshot the result of deploy.
* /creating cluster pipeline : Deployment Script of Containers Pipeline file 
* Jenkinsfile : Jenkinsfile for Creating Pipeline
* Dockerfile : Dockerfile for building the image 
* /green/green.yaml : Create a replication controller green pod
* /green/green-service.yaml : Create the green service
* /blue/blue.yaml : Create a replication controller blue pod
* /blue/blue-service.yaml : Create the blue service
* index.html : Web site Index file.
```
