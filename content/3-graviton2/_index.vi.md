---
title: "Graviton2"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

![Lambda](/images/1.intro/1-1kkk.png)

Vào tháng 9 năm 2021, [AWS đã thông báo](https://aws.amazon.com/about-aws/whats-new/2021/09/better-price-performance-aws-lambda-functions-aws-graviton2-processor/) AWS Lambda functions sẽ có tùy chọn được cung cấp bởi [AWS Graviton2 Processor](https://aws.amazon.com/ec2/graviton/), một Arm-based architecture do AWS thiết kế và xây dựng.

Lambda functions được hỗ trợ bởi AWS Graviton2 processor có thể **cải thiện hiệu suất/giá tốt hơn tới 34%** so với x86. Graviton2 functions sử dụng Arm-based processor architecture được thiết kế để mang lại hiệu suất tốt hơn tới 19% cùng chi phí thấp hơn 20% cho nhiều Serverless workloads khác nhau, chẳng hạn như web và mobile backend, data và media processing. Với độ trễ thấp hơn và hiệu suất tốt hơn, các functions được hỗ trợ bởi AWS Graviton2 processor rất lí tưởng để cung cấp năng lượng cho các ứng dụng Serverless quan trọng.

Những cải tiến về giá/hiệu suất sẽ thay đổi tùy theo thời gian chạy và source code. Trong module này, chúng ta sẽ tìm hiểu testing sample code với cả hai architecture và hoàn thành phân tích về giá/hiệu suất.

Để tìm hiểu thêm về Graviton2 trên AWS, hãy truy cập vào [Getting Started on Graviton Git Repo](https://github.com/aws/aws-graviton-getting-started).

Hãy xem [bài blog này](https://aws.amazon.com/blogs/compute/migrating-aws-lambda-functions-to-arm-based-aws-graviton2-processors/) để tìm hiểu thêm về cách migrate các Lambda functions của bạn sang Graviton2.









