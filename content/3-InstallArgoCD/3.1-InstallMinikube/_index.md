---
title : "Installation and Deployment of Argo CD"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---


#### I. Install Minikube

1. At the **cmd** interface, **SSH into EC2** and directly download **Minikube**

   ```
   curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \ && chmod +x minikube
    ```

2. Add **Minikube** to the **PATH** environment variable

    ```
   sudo mkdir -p /usr/local/bin/
   sudo install minikube /usr/local/bin/
      ```

![Create ArgoCD](/images/3/0001.png?featherlight=false&width=50pc)

3. Check if **Minikube** has been **installed successfully** or not

    ```
   minikube version
      ```

4. After successful installation, run a **cluster** within **Minikube**
    ```
   minikube start
      ```

![Create ArgoCD](/images/3/0002.png?featherlight=false&width=50pc)

   - Check whether **Minikube** has been **successfully started** or not
    
      ```
      minikube status
      ```

![Create ArgoCD](/images/3/0003.png?featherlight=false&width=40pc)

5. Create a **repo manifest** to store configurations for **Kubernetes**
   - Navigate to the **Jenkinsfile** and add the following code snippet so that Jenkins can perform the **update repo manifest** task     

 ```
   stage('update-manifest') {
   steps {
   script {
   // Thực hiện checkout từ GitLab repository mới
   checkout([$class: 'GitSCM',
   branches: [[name: 'main']],  // Chọn nhánh bạn muốn checkout
   doGenerateSubmoduleConfigurations: false,
   extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', trackingSubmodules: false]],
   submoduleCfg: [],
   userRemoteConfigs: [[url: 'https://gitlab.com/nguyenducquyhust/cicd-pipeline.git']]])
      // Tiếp tục với các bước khác trong stage
               withCredentials([string(credentialsId: 'gitlab-tokens', variable: 'gitlab-tokens')]) {
                  sh '''
                     git config user.email "your_email"
                     git config user.name "user_name"
                     BUILD_NUMBER=${BUILD_NUMBER}
                     pwd
                     ls
                     realpath deployment.yaml
                     git clean -fdx
                     git checkout main
                     git pull origin main
                     git branch
                     sed -i "s/replaceImageTag/$IMAGE_VERSION.${BUILD_NUMBER}/g" deployment.yaml
                     cat deployment.yaml
                     git status
                     git add .
                     git commit -m "Update deployment image to version $IMAGE_VERSION.${BUILD_NUMBER}"
                     git push -f <https://${GIT_USER_NAME}:${GIT_PASSWORD}@gitlab.com/nguyenducquyhust/cicd-pipeline.git> HEAD:main
                  '''
               }
         }
      }
   }
  ``` 
6. Access the **Jenkins Dashboard** > **Manage Jenkins** > **Credentials** 
   - Select **Add new credential** with the following content:
    
   ![Create ArgoCD](/images/3/0004.png?featherlight=false&width=70pc)

   The field **Secret** is the **access token** for GitLab

#### II. Install Kubectl

1. Download the latest version of **kubectl** 
   
      ```
      curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
      ```

2. Install **kubectl** 
    ```
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   chmod +x kubectl
   mkdir -p ~/.local/bin
   mv ./kubectl ~/.local/bin/kubectl   
    ```
3.  Check to ensure it's installed
      ```
      kubectl version --client
      ```

   ![Create ArgoCD](/images/3/0005.png?featherlight=false&width=65pc)

#### III. Install Argo CD on Minikube

**Ensure that Minikube, kubectl, and Docker are successfully installed**

1. Install the latest version of Argo CD
      ```
     curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
     sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
     rm argocd-linux-amd64   
      ```

2. Install Argo CD on a Kubernetes cluster
    - Create a namespace for argocd - where the services and resources of Argo CD will reside
      ```
      kubectl create namespace argocd
      ```
    - Argo CD can be installed using its manifests. Download these manifests and apply them to your Minikube cluster

     ``` 
      kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
     ```

   ![Create ArgoCD](/images/3/0006.png?featherlight=false&width=50pc)

3. Verify Argo CD Pods
    
      ```
      kubectl get pods -n argocd
      ```
   Ensure that all Argo CD pods **(argocd-server, argocd-dex-server, ...)** are running without any issues

   ![Create ArgoCD](/images/3/0010.png?featherlight=false&width=80pc)

### IV. Access Argo CD UI on Browser

1. Get the initial password for the admin user to log in
      ```
         kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
      ```
   ![Create ArgoCD](/images/3/0011.png?featherlight=false&width=50pc)

2. By default, the ArgoCD server is not exposed outside the cluster. You can expose it using port-forwarding to access the ArgoCD UI.
   ```
      kubectl port-forward svc/argocd-server -n argocd 8088:443
   ```

3. Access ```[Public_IPv4_address]:8088``` to log in to Argo CD with Username: admin and the password obtained earlier

### V. Deploy Application on Minikube using Argo CD UI

1. After successfully logging in, select **New App** and set up based on the instructions below:

![Create ArgoCD](/images/3/0012.png?featherlight=false&width=80pc)
![Create ArgoCD](/images/3/0007.png?featherlight=false&width=80pc)
![Create ArgoCD](/images/3/0008.png?featherlight=false&width=80pc)

2. The result obtained will be like this

![Create ArgoCD](/images/3/0009.png?featherlight=false&width=80pc)

At this point, you can press **Sync** to deploy the latest version

