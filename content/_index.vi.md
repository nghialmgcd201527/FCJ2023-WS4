---
title: "Using Edge Functions to handle Rewrites and Redirects"
date: "`r Sys.Date()`"
weight: 1
chapter: false
---

# Sử dụng Edge Functions để xử lí Rewrites và Redirects

### Tổng quan

Trong bài workshop này, chúng ta sẽ tìm hiểu cách xử lí các trường hợp **redirect** và **rewrite** phổ biến bằng cách sử dụng các tính năng của **Edge Compute** được cung cấp bởi AWS.

Mục tiêu chính của việc này là cung cấp cho người xem kiến thức về cách sử dụng các tính năng của Edge Compute để áp dụng được redirects cho ứng dụng của họ và hiểu cách sử dụng các tính năng đó hiệu quả nhất.

Bài workshop này là nơi bạn sẽ thực hiện các bài labs sử dụng **Compute platforms** có sẵn trong CloudFront để tiếp cận được những trường hợp redirects và rewrites khác nhau. Ở đây chúng ta sẽ sử dụng **Lambda@Edge** vì những responses từ các trường hợp này là cacheable responses.

### Prerequisites

Để hoàn thành bài workshop này, chúng ta cần có kiến thức về CDNs và kiến thức lập trình cơ bản JavaScript/Python để hiểu các đoạn code và những kiến thức xuyên suốt bài workshop.

### Nội dung

1.  [Giới thiệu](1-introduce/)
2.  [Các bước chuẩn bị](2-prerequiste/)
3.  [Xây dựng một serverless backend với AWS Lambda và AWS SAM](3-serverlessbackend/)
4.  [Cấu hình cho API authorization: API Gateway](4-apigateway/)
5.  [Build và deploy ứng dụng web: AWS Amplify](5-deploy/)
6.  [Chạy thử ứng dụng](6-test/)
7.  [Cấu hình cho trính xuất metadata từ hình ảnh: Amazon Rekognition](7-rekognition/)
8.  [Dọn dẹp tài nguyên](8-terminate/)
