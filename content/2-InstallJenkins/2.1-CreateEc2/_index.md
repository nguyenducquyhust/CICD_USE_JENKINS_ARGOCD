---
title : "Create EC2 Server"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

## Create EC2 Instances

1. Access the **AWS Management Console**

   - Navigate to **EC2**
   - Click on **EC2**

![Create EC2](/images/1/0001.png?featherlight=false&width=85pc)

2.In the **EC2** interface

   - Select  **Instances**
   - Choose  **Launch instances**

![Create EC2](/images/1/0002.png?featherlight=false&width=85pc)

3. Provide a **Name and tags** for the instance, enter **```workshop```**

![Create EC2](/images/1/0003.png?featherlight=false&width=55pc)

4. Choose the **AMI**

   - Select  **Quick Start**
   - Select  **Amazon Linux**
   - Select an **AMI**

![Create EC2](/images/1/0004.png?featherlight=false&width=55pc)

5. Select an **Instance type** and opt to **Create new key pair**

![Create EC2](/images/1/0005.png?featherlight=false&width=56pc)

6. In the **Cretae key pair** interface

   - Specify the **Key pair name**, e.g., **```aws-keypair```** (optional name, you can set any).
   - Choose **Key pair type:RSA**
   - Select **Private key file format:.pem**

![Create EC2](/images/1/0006.png?featherlight=false&width=70pc)

7.  Configure the **Network**

    - Select the **VPC: default**
    - Enable **Auto-assign public IP**
    - For **Firewall (Security Group)**, select **Create security group**
    - Select rules as shown

![Create EC2](/images/1/0007.png?featherlight=false&width=55pc)

8. Select **Advanced details**
   - **User data**, write user data file for EC2

```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```
   - Choose **Launch instance**
  
![Create EC2](/images/1/0008.png?featherlight=false&width=70pc)

9.  Complete the instance creation
   - Wait for about 5 minutes until the **Status check** shows  **2/2 checks passed**

![Create EC2](/images/1/0009.png?featherlight=false&width=90pc)
