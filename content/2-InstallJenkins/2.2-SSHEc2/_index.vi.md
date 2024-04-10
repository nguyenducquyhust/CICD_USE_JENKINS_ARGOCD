---
title : "SSH vào EC2 để cài Jenkins"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

#### SSH vào EC2 để cài Jenkins

1. Copy đường dẫn trong mục **Example**
   
![Create EC2](/images/1/0010.png?featherlight=false&width=50pc)

2. Vào thư mục chứa file **key-pair** vừa tải về ở mục **2.1**, mở bằng **Git bash** và dán đường dẫn để ssh vào EC2

![Create EC2](/images/1/0011.png?featherlight=false&width=40pc)

3. Tạo network cho 2 container
   ```
   sudo chmod 666 /var/run/docker.sock
   docker network create jenkins
   ```

![Create EC2](/images/1/0012.png?featherlight=false&width=40pc)

4. Setup docker:dind
   ```
   docker run --name jenkins-docker -d --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume docker-certs-jk:/certs/client --volume jenkins-home:/var/jenkins_home -p 2376:2376 docker:dind --storage-driver overlay2
   ```

![Create EC2](/images/1/0013.png?featherlight=false&width=40pc)

5. Run container Jenkins
      ```
      docker run --name jenkins -d --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v docker-certs-jk:/certs/client:ro jenkins/jenkins:2.426.2-lts-jdk17
      ```

![Create EC2](/images/1/0014.png?featherlight=false&width=40pc)

6. Mở port 8080 cho Ec2
   - Chọn **Security Group**
   - Chọn **Edit inbound rules**
   - Mục **Type** chọn **CustomTCP**
   - **Port range** chọn **8080**
   - **Source** chọn **0.0.0.0/0**

![Create EC2](/images/1/0015.png?featherlight=false&width=90pc)

7. Lấy mật khẩu để unlock Jenkins
      - Thay ```[jenkins_container_id]``` bằng id của bạn
      ```
      docker container ls
      docker logs [jenkins_container_id]
      ```
      - Lưu mật khẩu dùng cho bước sau 

![Create EC2](/images/1/0016.png?featherlight=false&width=70pc)

8. Unlock Jenkins
   - Chọn EC2 vừa khởi chạy
   - Copy địa chỉ **Public IPv4**   

![Create EC2](/images/1/0017.png?featherlight=false&width=70pc)

   - Mở trình duyệt bằng đường dẫn ```Public_IPv4_address:8080```
   - Dán mật khẩu bên trên vào mục **Aministrator password**

![Create EC2](/images/1/0018.png?featherlight=false&width=50pc)

9. Tạo tài khoản Jenkins
   - Đợi **Jenkins** chạy xong phần khởi động

![Create EC2](/images/1/0019.png?featherlight=false&width=50pc)

   - Điền các thông tin của bạn
   - Chọn **Save and Continue**

![Create EC2](/images/1/0020.png?featherlight=false&width=50pc)

10. Instance configuration
   - Dán địa chỉ ```http://[Public_IPv4_address]:8080``` vào mục **Jenkins URL**

![Create EC2](/images/1/0021.png?featherlight=false&width=50pc)

11. Hoàn thành tạo Jenkins

![Create EC2](/images/1/0022.png?featherlight=false&width=70pc)

