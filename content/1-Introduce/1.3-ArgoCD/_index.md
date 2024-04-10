---
title : "Argo CD"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

## Argo CD

**Argo CD** is an open-source project designed to provide automated deployment for Kubernetes projects. It follows the **"GitOps"** principle, automatically synchronizing Kubernetes projects to match the source code in the git repository. Simply commit changes to git, and **Argo CD** will take care of updating the projects in Kubernetes.

The main features of **Argo CD** include:
- **Automated deployment**: When new commits are made to the tracked branch in the Git repository, Argo CD automatically deploys these changes to Kubernetes.
- **Multi-environment management**: Argo CD can manage multiple Kubernetes environments, from development (dev) and testing (staging) to production (production).
- **Health and Integrity Checks**: It can automatically check the "health" of the application after deployment and ensure integrity over time.
- **Rollback & Rollout**: Supports easily going back to previous versions or rolling out new updates in sequence.

![ArgoCD](/images/1-Introduce/argocd.png?featherlight=false&width=60pc)
