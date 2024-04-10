---
title : "Tạo máy chủ EC2"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

#### Tạo máy chủ EC2 

1. Truy cập **AWS Management Console**

   - Tìm **EC2**
   - Chọn **EC2**

![Create EC2](/images/1/0001.png?featherlight=false&width=85pc)

2. Trong giao diện **EC2**

   - Chọn **Instances**
   - Chọn **Launch instances**

![Create EC2](/images/1/0002.png?featherlight=false&width=85pc)

3. **Name and tags** của instance, nhập **```workshop```**

![Create EC2](/images/1/0003.png?featherlight=false&width=55pc)

4. Thực hiện chọn **AMI**

   - Chọn **Quick Start**
   - Chọn **Amazon Linux**
   - Chọn **AMI**

![Create EC2](/images/1/0004.png?featherlight=false&width=55pc)

5. Thực hiện chọn **Instance type** và Chọn **Create new key pair**

![Create EC2](/images/1/0005.png?featherlight=false&width=56pc)

6. Trong giao diện **Cretae key pair**

   - **Key pair name**, nhập **```aws-keypair```** (tên tùy chọn, bạn có thể đặt bất kỳ).
   - **Key pair type**, chọn **RSA**
   - **Private key file format**, chọn **.pem**

![Create EC2](/images/1/0006.png?featherlight=false&width=70pc)

7.  Thực hiện cấu hình **Network**

    - **VPC**, chọn **default**
    - **Auto-assign public IP**, chọn **Enable**
    - **Firewall (Security Group)**, chọn **Create security group**
    - Tích chọn rules như hình 

![Create EC2](/images/1/0007.png?featherlight=false&width=55pc)

8. Thực hiện chọn **Advanced details**
   - **User data**, viết file user data cho EC2

```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```
   - Chọn **Launch instance**
  
![Create EC2](/images/1/0008.png?featherlight=false&width=70pc)

9.  Hoàn thành tạo instance
   - Đợi khoảng 5 phút **Status check** sẽ chuyển sang **2/2 checks passed**

![Create EC2](/images/1/0009.png?featherlight=false&width=90pc)

