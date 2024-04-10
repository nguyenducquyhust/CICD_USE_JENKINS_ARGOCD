---
title : "Setup Repo Manifest"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

## Setup Repo Manifest

1. Clone the repository for your project on GitLab

2. For my project, the repository manifest will contain three folders corresponding to three parts: Front-end, Back-end, and Database

   - Run pods corresponding to the 3 components above
   ```
      kubectl apply -f [folder_name]
   ```

Here, **[folder_name]**  is the name of the 3 folders that were just cloned

3. Check the status of pods

   ```
      minikube dashboard
   ```

4. When the pods have finished running, proceed to import data into the database

   - Get the name of the database pod

      ```
     kubectl get pod
      ``` 

   - Perform port-forwarding to a database pod

      ```
         kubectl port-forward [database_pod_name] 3306:3306
      ``` 

   - Afterwards, you can open **mysql workbench** or **datagrip**  to import data into the pod