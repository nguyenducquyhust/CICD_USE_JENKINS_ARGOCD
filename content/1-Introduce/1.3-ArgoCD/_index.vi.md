---
title : "Argo CD"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

#### Argo CD

**Argo CD** là một dự án mã nguồn mở chỉ định để cung cấp triển khai tự động cho các dự án Kubernetes. Nó tuân theo nguyên lý **"GitOps"**, tự động đồng bộ hóa các dự án trên Kubernetes để phù hợp với mã nguồn trong git repo. Chỉ cần commit các thay đổi vào git, **Argo CD** sẽ đảm nhiệm việc cập nhật lại các dự án trong Kubernetes. 

Các tính năng chính của **Argo CD** bao gồm:
- **Tự động hóa triển khai**: Khi có các commit mới vào nhánh được theo dõi trong Git repository, Argo CD tự động triển khai các thay đổi này vào Kubernetes.
- **Quản lý đa môi trường**: Argo CD có thể quản lý nhiều môi trường Kubernetes, từ phát triển (dev), kiểm thử (staging) đến sản xuất (production).
- **Health and Integrity Checks**: Nó có thể tự động kiểm tra "sức khỏe" của ứng dụng sau khi triển khai và bảo đảm sự toàn vẹn theo thời gian.
- **Rollback & Rollout**: Hỗ trợ quay về các phiên bản trước hoặc triển khai lần lượt các bản cập nhật mới một cách dễ dàng.

![ArgoCD](/images/1-Introduce/argocd.png?featherlight=false&width=60pc)