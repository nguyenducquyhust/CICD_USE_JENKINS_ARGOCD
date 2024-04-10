---
title : "Cài đặt và triển khai Argo CD "
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### I. Cài đặt Minikube

1. Tại giao diện **cmd** đang **ssh vào EC2**, tải xuống trực tiếp **Minikube** 

   ```
   curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \ && chmod +x minikube
    ```

2. Thêm **Minikube** vào biến môi trường **path**

    ```
   sudo mkdir -p /usr/local/bin/
   sudo install minikube /usr/local/bin/
      ```

![Create ArgoCD](/images/3/0001.png?featherlight=false&width=50pc)

3. Kiểm tra xem **Minikube** đã được cài đặt thành công hay chưa

    ```
   minikube version
      ```

4. Sau khi cài đặt thành công, chạy **cluster** trong **minikube** 

    ```
   minikube start
      ```

![Create ArgoCD](/images/3/0002.png?featherlight=false&width=50pc)

   - Kiểm tra xem **Minikube** đã được **khởi động thành công** hay chưa
    
      ```
      minikube status
      ```

![Create ArgoCD](/images/3/0003.png?featherlight=false&width=40pc)

5. Tạo **repo manifest** để chứa các config cho **Kubernetes**
   - Vào file **Jenkinsfile** và thêm đoạn code sau để jenkins có thể thực hiện việc **update repo manifest**      

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
6. Truy cập vào **Jenkins Dashboard** > **Manage Jenkins** > **Credentials** 
   - Chọn **Add new credential** với nội dung như dưới đây:
    
   ![Create ArgoCD](/images/3/0004.png?featherlight=false&width=70pc)

   Trong đó trường **Secret** chính là **access token** của gitlab

#### II. Cài đặt Kubectl

1. Tải xuống phiên bản **kubectl** mới nhất
   
      ```
      curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
      ```

2. Cài đặt **kubectl** 
    ```
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   chmod +x kubectl
   mkdir -p ~/.local/bin
   mv ./kubectl ~/.local/bin/kubectl   
    ```
3.  Kiểm tra để đảm bảo đã cài đặt 
      ```
      kubectl version --client
      ```

   ![Create ArgoCD](/images/3/0005.png?featherlight=false&width=65pc)

#### III. Cài đặt Argo CD trên Minikube

**Đảm bảo đã cài đặt thành công minikube, kubectl và docker**

1. Cài đặt phiên bản Argo CD mới nhất
      ```
     curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
     sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
     rm argocd-linux-amd64   
      ```

2. Cài Argo CD trên cụm K8s
    - Tạo một namespace cho argocd - nơi các dịch vụ và tài nguyên của Argo CD sẽ tồn tại
      ```
      kubectl create namespace argocd
      ```
    - Argo CD có thể được cài đặt bằng cách sử dụng manifest. Tải manifest và áp dụng chúng vào cụm Minikube của mình

     ``` 
      kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
     ```

   ![Create ArgoCD](/images/3/0006.png?featherlight=false&width=50pc)

3. Kiểm tra các pods của Argo CD
    
      ```
      kubectl get pods -n argocd
      ```
   Đảm bảo rằng tất cả các pod Argo CD **(argocd-server, argocd-dex-server, ...**) đang chạy mà không gặp bất kỳ sự cố nào.

   ![Create ArgoCD](/images/3/0010.png?featherlight=false&width=80pc)

### IV. Truy cập vào giao diện Argo CD trên trình duyệt

1. Sau khi cài đặt xong, lấy mật khẩu của Argo CD để đăng nhập:
      ```
         kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
      ```
   ![Create ArgoCD](/images/3/0011.png?featherlight=false&width=50pc)

2. Thực hiện port forward
   ```
      kubectl port-forward svc/argocd-server -n argocd 8088:443
   ```

3. Truy cập vào ```[Public_IPv4_address]:8088``` để đăng nhập vào ArgoCD với Username:admin và password vừa lấy ở trên

### V. Triển khai ứng dụng trên Minikube thông qua giao diện Argo CD

1. Sau khi đăng nhập được thì chọn **New App** và setup dựa theo bên dưới:

![Create ArgoCD](/images/3/0012.png?featherlight=false&width=80pc)
![Create ArgoCD](/images/3/0007.png?featherlight=false&width=80pc)
![Create ArgoCD](/images/3/0008.png?featherlight=false&width=80pc)

2. Kết quả thu được sẽ như này

![Create ArgoCD](/images/3/0009.png?featherlight=false&width=80pc)

Lúc này có thể ấn **Sync** để deploy bản mới nhất
