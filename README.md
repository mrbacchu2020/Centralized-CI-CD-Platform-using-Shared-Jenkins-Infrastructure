# Centralized CI/CD Platform using Shared Jenkins Infrastructure

## Project Overview

This project implements a **centralized CI/CD platform** using Jenkins to support multiple applications through a **shared pipeline library**. Instead of each team maintaining separate Jenkins servers and pipelines, a single Jenkins platform is used to standardize build, test, security scanning, and deployment processes.

The platform ensures **consistent pipelines, better security practices, and reduced infrastructure duplication**.

---
# The goal of this project is to design and implement a **shared Jenkins CI/CD platform** that:

* Supports multiple application repositories
* Uses a shared Jenkins pipeline library
* Standardizes build and deployment processes
* Implements role-based access control
* Automates Docker image builds

---

# Technologies Used

* Jenkins
* GitHub
* AWS EC2
* Docker
* Jenkins Shared Library
* Git

---

# Architecture

Developers push code to GitHub repositories. Jenkins automatically pulls the code, executes standardized pipeline stages through the shared library, and builds Docker images.

Pipeline flow:

Developers → GitHub Repositories → Jenkins Server (EC2) → Shared Pipeline Library → Jenkins Agent → Build → Test → Scan → Deploy → Docker Images

---

# System Architecture Diagram

(docs/architecture-diagram.png.png)

---

# Jenkins Infrastructure Setup

The Jenkins server was deployed on an AWS EC2 instance.

## Steps

1. Launch EC2 instance
2. Install Java
3. Install Jenkins
4. Configure Jenkins dashboard
5. Install required plugins
6. Configure Jenkins agent (docker-agent)
7. Add credentials for GitHub and Docker

---

# Jenkins Agent Configuration

A Jenkins agent was configured to run builds and Docker operations.

Agent configuration:

Node Name: docker-agent
Executors: 1
Remote Directory: /home/ubuntu/agent
Launch Method: SSH
Label: docker

The agent is responsible for executing the build pipelines.

---

# Jenkins Shared Library

A shared pipeline library was created to standardize CI/CD stages across multiple applications.

Repository:

jenkins-shared-library

Structure:

```
jenkins-shared-library
│
└── vars
    └── cicdPipeline.groovy
```

Pipeline stages defined in the library:

* Checkout
* Build
* Test
* Scan
* Deploy

Example pipeline stage:

```
stage('Build') {
    steps {
        sh "docker build -t ${config.image} ."
    }
}
```

---

# Application Repositories

Two sample applications were created to demonstrate multi-project CI/CD.

## Application 1 – NodeJS

Repository: app-nodejs

Structure:

```
app-nodejs
│
├── app.js
├── package.json
├── Dockerfile
└── Jenkinsfile
```

The Jenkinsfile calls the shared library pipeline.

---

## Application 2 – Python

Repository: app-python

Structure:

```
app-python
│
├── app.py
├── Dockerfile
└── Jenkinsfile
```

Both applications use the **same shared pipeline library**.

---

# Jenkins Pipeline Jobs

Two Jenkins pipeline jobs were created:

* app-nodejs-pipeline
* app-python-pipeline

Both pipelines use:

```
@Library('shared-library') _
```

and execute the standardized CI/CD stages.

---

# CI/CD Pipeline Stages

Each pipeline executes the following stages:

1. Checkout – Pull source code from GitHub
2. Build – Build Docker image
3. Test – Execute application tests
4. Scan – Perform security checks
5. Deploy – Deploy Docker image

---

# Docker Integration

Docker is used to build container images for applications.

Example build command executed by Jenkins:

```
docker build -t shreyash274/app-nodejs:latest .
```

The images can then be pushed to Docker Hub.

---

# Role-Based Access Control

Role-based access control was implemented in Jenkins to restrict permissions.

Roles configured:

Admin
Full system access

Developer
Build jobs
View pipelines

---

# Project Deliverables

The final deliverables for this project include:

* Jenkins Shared Library repository
* NodeJS sample application repository
* Python sample application repository
* Jenkins pipelines for both applications
* Architecture documentation
* Pipeline execution screenshots

---

# Screenshots

The following screenshots demonstrate the project implementation:

1. Jenkins Dashboard
2. Installed Jenkins Plugins
3. Jenkins Nodes Configuration
4. Shared Library Repository
5. Global Pipeline Library Configuration
6. NodeJS Repository Structure
7. Python Repository Structure
8. Jenkins Dashboard with Multiple Pipelines
9. Pipeline Stage Execution
10. Docker Build Output

---

# Key Learning Outcomes

This project demonstrates practical DevOps skills including:

* Jenkins administration
* CI/CD pipeline automation
* Shared pipeline libraries
* Docker containerization
* Infrastructure on AWS
* Multi-application CI/CD platforms

---

# Conclusion

The centralized CI/CD platform successfully enables multiple development teams to use a **standardized pipeline architecture**. Using Jenkins shared libraries significantly improves maintainability, security, and scalability of CI/CD workflows.

This architecture can be extended further with:

* Kubernetes deployments
* Security scanning tools
* Automated testing frameworks
* Infrastructure as Code
