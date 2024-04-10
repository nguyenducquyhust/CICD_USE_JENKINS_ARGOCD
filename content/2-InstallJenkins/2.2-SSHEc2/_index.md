---
title : "SSH into EC2 to install Jenkin"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

#### SSH into EC2 to install Jenkin

1. Copy the link in the **Example** section
   
![Create EC2](/images/1/0010.png?featherlight=false&width=50pc)

2. Go to the folder containing the file **key-pair** just downloaded above, open it with **Git bash** and paste the path to ssh into EC2

![Create EC2](/images/1/0011.png?featherlight=false&width=40pc)

3. Create network for 2 containers
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

6. Open port 8080 for Ec2
   - Select **Security Group**
   - Choose **Edit inbound rules**
   - In **Type** select **CustomTCP**
   - **Port range** select **8080**
   - **Source** select **0.0.0.0/0**

![Create EC2](/images/1/0015.png?featherlight=false&width=90pc)

7. Get the password to unlock Jenkins
      - Replace ```[jenkins_container_id]``` with your id
      ```
      docker container ls
      docker logs [jenkins_container_id]
      ```
      - Save the password for later steps

![Create EC2](/images/1/0016.png?featherlight=false&width=70pc)

8. Unlock Jenkins
   - Select the newly launched EC2
   - Copy **Public IPv4** address   

![Create EC2](/images/1/0017.png?featherlight=false&width=70pc)

   - Open the browser using the path ```Public_IPv4_address:8080```
   - Paste the password above into the **Administrator password** section

![Create EC2](/images/1/0018.png?featherlight=false&width=50pc)

9. Create a Jenkins account
   - Wait for **Jenkins** to finish booting

![Create EC2](/images/1/0019.png?featherlight=false&width=50pc)

   - Fill in your information
   - Select **Save and Continue**

![Create EC2](/images/1/0020.png?featherlight=false&width=50pc)

10. Instance configuration
   - Paste the address ```http://[Public_IPv4_address]:8080``` into the **Jenkins URL** field

![Create EC2](/images/1/0021.png?featherlight=false&width=50pc)

11. Complete creating Jenkins

![Create EC2](/images/1/0022.png?featherlight=false&width=70pc)
