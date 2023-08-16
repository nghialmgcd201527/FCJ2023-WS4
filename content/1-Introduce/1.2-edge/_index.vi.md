---
title: "Edge Compute"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.2 </b> "
---

Trước khi bắt tay vào thực hành bài lab, chúng ta nên hiểu cơ bản về loại edge compute capabilities mà chúng ta sẽ sử dụng trong bài lab này, hiện nay đang có sẵn trong CloudFront.

#### CloudFront Edge Compute Features

[**Lambda@Edge:**](https://aws.amazon.com/lambda/edge/) Đây là một tính năng của [Amazon CloudFront](https://aws.amazon.com/cloudfront/) cho phép bạn chạy code gần hơn với users ứng dụng của chúng ta, giúp cải thiện hiệu suất và giảm độ trễ. Với **Lambda@Edge,** chúng ta không phải cung cấp hoặc quản lí cơ sở hạ tầng ở nhiều locations trên khắp thế giới. chúng ta chỉ trả phí cho thời gian compute mà chúng ta sử dụng và không tính phí khi code của chúng ta đang không sử dụng. Với **Lambda@Edge,** chúng ta có thể làm phong phú hơn các ứng dụng web của chúng ta bằng cách phân phối chúng trên toàn cầu và cải thiện hiệu suất của chúng - tất cả đều không cần server administration. **Lambda@Edge** chạy code của chúng ta để phản hồi các sự kiện được tạo ra bởi Amazon CloudFront content delivery network. Chỉ cần upload code của chúng ta lên AWS Lambda, ứng dụng này sẽ xử lí mọi thứ cần thể để run và scale code của chúng ta với tính khả dụng cao tại một AWS location gần với end user nhất của chúng ta.

#### CloudFront Edge Compute Triggers

Amazon CloudFront yêu cầu 4 loại event khác nhau để custom request và response được trao đổi giữa viewer và server (origin).

1. **Viewer Request** - Function này thực thi khi CloudFront nhận được request từ viewer và trước khi nó kiểm tra xem đối tượng được yêu cầu có trong edge cache hay không.

![Name for Cloud9 Workspace và chọn Environment type](/images/2.prerequisite/2.1-createcloud9workspace/2.1-1.png)

6. Sang phần setting cho **New EC2 instance,** chọn **Additional instance types** sau đó chúng ta chọn loại **t3.medium.**

![Setting cho new EC2 instance](/images/2.prerequisite/2.1-createcloud9workspace/2.1-2.png)

7. Giữ nguyên mặc định cho những thiết lập khác. Click nút **Create.**

![Keep default](/images/2.prerequisite/2.1-createcloud9workspace/2.1-3.png)

8. Đợi khoảng 10 phút để Cloud9 Workspace được tạo. Khi Cloud9 Workspace được tạo xong, chúng ta sẽ có một môi trường để làm việc với AWS CLI và các công cụ khác. Trong danh sách các **Environments** được tạo ra, hãy tìm environment **serverless-workshop** và click vào nút **Open** để mở môi trường Cloud9.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/2.1-4.png)

10. Sau khi môi trường mở ra, chúng ta hãy tắt những phần bên dưới đã được khởi tạo lúc bắt đầu và tạo một **trang terminal** mới.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/2.1-5.png)

Workspace của chúng ta sẽ trông như thế này.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/createcloud9-6.png)
