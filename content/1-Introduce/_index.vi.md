---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

#### Giới thiệu CI/CD, Jenkins và ArgoCD

**CI/CD** là viết tắt của **Continuous Integration/Continuous Delivery** hoặc **Continuous Deployment**. Đây là một phương pháp phát triển phần mềm mục tiêu đến việc tự động hóa quá trình tích hợp, kiểm thử, phân phối và triển khai ứng dụng.

**CI/CD** sẽ liên tục kiểm tra test code mới. Điều này giúp ngăn chặn các lỗi trong hệ thống trước khi code mới được cập nhật lên server. Việc tích hợp và kiểm thử thường xuyên giúp phát hiện và sửa lỗi sớm, tránh được các vấn đề lớn ở giai đoạn sau. **CI/CD** tự động hóa quá trình phát triển và triển khai, giúp teams đẩy nhanh việc release sản phẩm mới.

Các công cụ phổ biến hỗ trợ **CI/CD** bao gồm Jenkins, GitLab CI, Travis CI, CircleCI, và ArgoCD cho Kubernetes. Thông thường, hay được sử dụng nhất là:
+ **Jenkins** - một công cụ open-source tự động hóa các nhiệm vụ phát triển phần mềm như build, test và deploy.
+ **ArgoCD** - một công cụ điều phối liên tục dành cho Kubernetes, giúp tự động hóa việc triển khai ứng dụng trong kiến trúc microservices và quản lý nhiều môi trường Kubernetes một cách nhất quán. 
Cách tiếp cận này không chỉ cải thiện hiệu suất làm việc nhóm mà còn giúp sản phẩm của bạn đạt đến tay người dùng nhanh chóng với chất lượng cao hơn.

Ở môi trường điện toán đám mây, các nhà phát triển thường sử dụng **Docker** để đóng gói và **Kubernetes** để điều phối.

#### Nội dung

- [Quy trình hoạt động CI/CD](1.1-CICDprocess/)
- [Jenkins](1.2-jenkins/)
- [ArgoCD](1.3-argocd/)

Bây giờ chúng ta sẽ cùng nhau đi qua các khái niệm cơ bản nhất của CI/CD, Jenkins và ArgoCD nhé.
