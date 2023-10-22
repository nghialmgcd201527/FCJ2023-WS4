---
title: "Tổng hợp findings bằng Security Hub"
weight: 5
chapter: false
pre: " <b> 2.5. </b> "
---

AWS Security Hub là cloud service thực hiện các biện pháp kiểm tra bảo mật best practice, liên tục, tự động đối với AWS resource của bạn. Security Hub tổng hợp các cảnh báo bảo mật (tức là các findings) từ nhiều AWS services và sản phẩm của đối tác theo định dạng chuẩn hóa để bạn có thể dễ dàng xử lý chúng hơn. Để duy trì cái nhìn toàn diện về trạng thái bảo mật của bạn trong AWS, bạn cần tích hợp nhiều công cụ và service bao gồm threat detections từ Amazon GuardDuty, lỗ hổng từ Amazon Inspector, phân loại dữ liệu nhạy cảm từ Amazon Macie, các sự cố config resource từ AWS Config và AWS Partner Network Products. Security Hub đơn giản hóa cách bạn hiểu và cải thiện tình trạng bảo mật của mình bằng các biện pháp bảo mật best practice được tự động hóa được hỗ trợ bởi các AWS Config rules và tích hợp tự động với hàng tá dịch vụ AWS và partner products.

#### Các Amazon GuardDuty findings trong Security Hub

Đầu tiên các bạn hãy vào [Security Hub console](https://console.aws.amazon.com/securityhub/home). Ở trang **Security Hub Summary**, cuộn xuống phần **Latest findings from AWS Integrations**. Click vào **See findings** kế bên **Amazon GuardDuty**. Nó sẽ mở ra trang **Findings** trong **Security Hub console** và điền trước các filter để hiển thị các findings từ Amazon GuardDuty. 











