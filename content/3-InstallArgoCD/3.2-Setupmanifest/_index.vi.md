---
title : "Thiết lập Repo Manifest"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2 </b> "
---

#### Thiết lập Repo Manifest

1. Clone repo cho dự án của bạn trên Gitlab

2. Đối với dự án của tôi, trong repo manifest sẽ có 3 folder tương ứng với 3 phần: Front-end, Back-end và Database

   - Chạy các pod tương ứng với 3 phần trên
   ```
      kubectl apply -f [folder_name]
   ```

Trong đó **[folder_name]** chính là tên của 3 folder vừa clone về

3. Kiểm tra trạng thái các pod

   ```
      minikube dashboard
   ```

4. Khi các pod đã chạy xong, thực hiện import dữ liệu vào cho database

   - Lấy tên pod của database

      ```
     kubectl get pod
      ``` 

   - Thực hiện port-forward đến pod của database

      ```
         kubectl port-forward [database_pod_name] 3306:3306
      ``` 

   - Sau đó có thể mở **mysql workbench** hoặc **datagrip** để import dữ liệu vào pod



