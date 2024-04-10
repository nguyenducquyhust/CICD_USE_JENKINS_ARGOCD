---
title : "Setup connection between Jenkins and Gitlab"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

## Setup connection between Jenkins and Gitlab

1. Install plugin jenkins
   -  Go to **Manage Jenkins** -> **Plugin** -> **Available plugin** install the appropriate plugin for the project: **Gitlab**, **Docker** and **Docker pipeline**

![ConnectJenkins](/images/2/0001.png?featherlight=false&width=80pc)

2. Install **Tools jenkins**

   - **Manage Jenkins** -> **Tools**, scroll down to **JDK installations** section and click on **Add JDK**
   - In the **Name** section, set it as ```Jdk```
   - Choose **Install automatically**
   - For **Git**, **Docker**, **Maven** do the same

![ConnectJenkins](/images/2/0002.png?featherlight=false&width=80pc)

3. Create item 

   - At the Jenkins interface, select **New item**
   - Select **Pipeline** and name the pipeline

![ConnectJenkins](/images/2/0004.png?featherlight=false&width=80pc)

4. Configuration for Jenkins

   - In the **General** section, you can choose as below to save the 5 most recent versions (number can be customized)

![ConnectJenkins](/images/2/0005.png?featherlight=false&width=80pc)

   - Scroll down to the **Build Trigger** section, select the option below so that your Jenkins CI/CD Pipeline can run actions like push and merge code

![ConnectJenkins](/images/2/0006.png?featherlight=false&width=70pc)

   -Go to gitlab and copy the project clone URL

 ![ConnectJenkins](/images/2/0007.png?featherlight=false&width=70pc) 

   - Return to the Jenkins interface, in Advanced Project Options, setup as shown below and the URL is the one just copied from gitlab
  
 ![ConnectJenkins](/images/2/0008.png?featherlight=false&width=80pc) 

   - Add **Credentials** (note that the gitlab account must have admin rights or sufficient project access rights, otherwise the Jenkins CI/CD pipeline cannot clone the code)

![ConnectJenkins](/images/2/0009.png?featherlight=false&width=80pc)  

   - Go out and select the **Credential** I just added
  
![ConnectJenkins](/images/2/0010.png?featherlight=false&width=80pc)  

   - Configuration for Pipeline

![ConnectJenkins](/images/2/0011.png?featherlight=false&width=80pc)  

5. Create a webhook on Gitlab so that Jenkins can connect to the Gitlab repository and perform CI/CD steps when new code is pushed.

   - In the Jenkins interface, next to User_name click on the arrow mark -> **Configure**
 
![ConnectJenkins](/images/2/0012.png?featherlight=false&width=80pc)  

   - Generate tokens for integration in gitlab

![ConnectJenkins](/images/2/0013.png?featherlight=false&width=80pc)

   - Go to the project on gitlab, select **Settings** → **Webhook** -> **Add new webhook**

![ConnectJenkins](/images/2/0014.png?featherlight=false&width=80pc)

   - Please configure the URL section as follows:
         ```
         http://<account trên jenkins>:<token account jenkins>@<địa chỉ jenkins>/project/<tên project trên jenkins>
         ```
   -  For example:
         ```
         http://workshop:116ab6d523026ec1f474f13f1c0ea22345@13.229.78.87:8080/project/Workshop
         ```
   - Configure **Trigger** 

![ConnectJenkins](/images/2/0015.png?featherlight=false&width=80pc)

   - Uncheck **SSL verification** and check the status of successful push events 
    
![ConnectJenkins](/images/2/0016.png?featherlight=false&width=80pc)

6. Complete Gitlab and Jenkins integration

7. Create Jenkinsfile

   - In project at root folder (same level as **docker-compose.yml** file), create **Jenkinsfile** as show
   - I just added a simple job that prints out the current user and directory, now we will try to commit the code on Gitlab

![ConnectJenkins](/images/2/0017.png?featherlight=false&width=70pc)

   - After the commit is complete, merge the request, which will enable the trigger for the CI/CD pipeline to start running

8. Check Jenkins
   
   - At the Jenkins interface, all steps are successful

![ConnectJenkins](/images/2/0018.png?featherlight=false&width=50pc)