---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

## Introduction to CI/CD, Jenkins and ArgoCD

**CI/CD** stands for **Continuous Integration/Continuous Delivery** or **Continuous Deployment**. This is a software development approach aimed at automating the integration, testing, delivery, and deployment processes of applications.

**CI/CD** continuously tests new code commits. This helps to prevent errors in the system before the new code is updated on the server. Regular integration and testing help identify and fix errors early, avoiding major problems in later stages. **CI/CD** automates the development and deployment processes, helping teams accelerate the release of new products.

Popular tools that support **CI/CD** include Jenkins, GitLab CI, Travis CI, CircleCI, and ArgoCD for Kubernetes. Typically, what is used the most is:
+ **Jenkins**: an open-source tool that automates software development tasks such as build, test, and deploy.
+ **ArgoCD** is a continuous orchestration tool for Kubernetes, helping to automate the deployment of applications in microservices architecture and manage multiple Kubernetes environments consistently. This approach not only improves team performance but also enables your product to reach users more quickly and with higher quality.

In the cloud computing environment, developers often use **Docker** to package and **Kubernetes** to orchestrate.

## Contents

- [The CI/CD operations process](1.1-CICDprocess/)
- [Jenkins](1.2-jenkins/)
- [ArgoCD](1.3-argocd/)

In the following sections, we will delve into the fundamental concepts of CI/CD, Jenkins and ArgoCD.
