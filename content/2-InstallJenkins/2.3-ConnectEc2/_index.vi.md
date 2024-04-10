---
title : "Thiết lập kết nối giữa Jenkins và Gitlab"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

#### Thiết lập kết nối giữa Jenkins và Gitlab

1. Cài đặt plugin jenkins
   -  Truy cập vào **Manage Jenkins** -> **Plugin** -> **Available plugin** tiến hành cài đặt plugin phù hợp cho project: **Gitlab** , **Docker** và **Docker pipeline**

![ConnectJenkins](/images/2/0001.png?featherlight=false&width=80pc)

2. Cài đặt **Tools jenkins**

   - **Manage Jenkins** -> **Tools**, kéo xuống phần **JDK installations** ấn vào **Add JDK**
   - Trong phần **Name** đặt là ```Jdk```
   - Tích **Install automatically**
   - Đối với **Git**, **Docker**, **Maven** làm tương tự

![ConnectJenkins](/images/2/0002.png?featherlight=false&width=80pc)

3. Tạo item 

   - Tại giao diện Jenkins chọn **New item**
   - Chọn **Pipeline** và đặt tên cho pipeline

![ConnectJenkins](/images/2/0004.png?featherlight=false&width=80pc)

4. Cấu hình cho Jenkins

   - Ở phần **General** có thể chọn như dưới để lưu lại 5 bản chạy gần nhất(số lượng có thể tùy chỉnh)

![ConnectJenkins](/images/2/0005.png?featherlight=false&width=80pc)

   - Kéo xuống phần **Build Trigger**, chọn option như dưới đây để Jenkins CI/CD Pipeline của bạn chạy cả được những action như push, merge code.

![ConnectJenkins](/images/2/0006.png?featherlight=false&width=70pc)

   - Sang gitlab copy URL clone dự án

 ![ConnectJenkins](/images/2/0007.png?featherlight=false&width=70pc) 

   - Quay trở lại giao diện Jenkins, trong Advanced Project Options, setup như hình dưới và URL chính là cái vừa copy từ gitlab
  
 ![ConnectJenkins](/images/2/0008.png?featherlight=false&width=80pc) 

   - Thêm **Credentials** (lưu ý là tài khoản gitlab phải có quyền admin hoặc quyền đủ truy cập dự án, không là Jenkins CI/CD pipeline không clone code về được). 

![ConnectJenkins](/images/2/0009.png?featherlight=false&width=80pc)  

   - Ra ngoài chọn lại **Credential** mình vừa add
  
![ConnectJenkins](/images/2/0010.png?featherlight=false&width=80pc)  

   - Cấu hình Pipeline

![ConnectJenkins](/images/2/0011.png?featherlight=false&width=80pc)  

5. Tạo webhook trên Gitlab để Jenkins có thể kết nối repository Gitlab và thực hiện các bước CI/CD khi có code mới được push lên

   - Ở giao diện Jenkins, cạnh cái User_name ấn vào dấu mũi tên -> **Configure**
 
![ConnectJenkins](/images/2/0012.png?featherlight=false&width=80pc)  

   - Tạo token để tích hợp trong gitlab

![ConnectJenkins](/images/2/0013.png?featherlight=false&width=80pc)

   - Truy cập vào dự án trên gitlab, chọn **Settings** → **Webhook** -> **Add new webhook**

![ConnectJenkins](/images/2/0014.png?featherlight=false&width=80pc)

   - Mục URL hãy cấu hình như sau 
         ```
         http://<account trên jenkins>:<token account jenkins>@<địa chỉ jenkins>/project/<tên project trên jenkins>
         ```
   -  Ví dụ:
         ```
         http://workshop:116ab6d523026ec1f474f13f1c0ea22345@13.229.78.87:8080/project/Workshop
         ```
   - Cấu hình **Trigger** như hình

![ConnectJenkins](/images/2/0015.png?featherlight=false&width=80pc)

   - Bỏ tích chọn **SSL verification** và kiểm tra trạng thái push events thành công 
    
![ConnectJenkins](/images/2/0016.png?featherlight=false&width=80pc)

6. Hoàn thành tích hợp Gitlab và Jenkins

7. Tạo Jenkinsfile

   - Trong dự án tại thư mục gốc (cùng cấp **docker-compose.yml** file), tạo **Jenkinsfile** như dưới đây
   - Mình chỉ thêm job đơn giản là in ra người dùng và thư mục hiện tại, bây giờ chúng ta sẽ thử commit code trên Gitlab

![ConnectJenkins](/images/2/0017.png?featherlight=false&width=70pc)

   - Sau khi commit xong thì merge request, lúc này sẽ enable trigger để CI/CD pipeline bắt đầu chạy

8. Quay sang Jenkins để check
   
   - Tại giao diện Jenkins, các bước đều thành công

![ConnectJenkins](/images/2/0018.png?featherlight=false&width=50pc)
