---
title: "Tối ưu hóa cho Serverless (Performance and Cost)"
date: "`r Sys.Date()`"
weight: 1
chapter: false
---

# Tối ưu hóa cho Serverless (Performance and Cost)

![Lambda](/images/1.intro/1-1kkk.png)

### Giới thiệu

Trong bài workshop này, chúng ta sẽ đi tìm hiểu các cách tối ưu hóa **performance** và **cost** của Serverless.

Để xem thêm nhiều **Serverless Architecture Best Practices**, các bạn hãy truy cập vào [Serverless Application Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html) để tìm từ **AWS Well Architected Framework**. 

Bài workshop này sẽ gồm các phần xoay quanh một mục đích chung là **cost reduction**. Tất cả các module đều được làm độc lập vì vậy các bạn có thể hoàn thành từng phần riêng biệt.

Các module được sắp xếp từ thứ tự dễ thực hiện nhất và có 3 mục chính trong bài workshop này.

**Power Tuning** và **Log Tuning** là những best practice thường bị bỏ qua và việc triển khai các phương pháp này không ảnh hưởng đến hoạt động của ứng dụng của bạn. Trong bài workshop này, các bạn sẽ đi vào thực hiện phương pháp **Power Tuning**.

**Gravition2**, **Direct Integration**, **Provisioned Concurrency** và **Code Tuning** là các phương pháp tiếp cận dựa trên configuration cần một vài code touches và operation overhead. Trong bài workshop này, các bạn sẽ đi vào thực hiện phương pháp **Gravition2**.

**Traffic Throttling** và **Asynchronous Workflows** cần sự thay đổi thiết kế tổng thể đối với ứng dụng Serverless của bạn. Và chúng ta sẽ đi thực hiện **Traffic Throttling**.

#### Những gì bạn sẽ học được

Trong suốt bài workshop này, bạn sẽ được tìm hiểu một số kĩ thuật thực hành tốt nhất để tối ưu hóa workload của Serverless nhằm giảm chi phí và tăng hiệu suất. Bài workshop này tập trung vào AWS Lambda bên cạnh đó các services khác được sử dụng bao gồm:

- Amazon SQS
- Amazon API Gateway
- Amazon DynamoDB
- AWS Step Functions
- AWS AppConfig

#### Những kiến thức, kĩ năng cần có để hoàn thành bài workshop này

Kiến thức cơ bản về các services AWS Serverless cần thiết bao gồm **AWS Lambda**, **API Gateway**, **SQS** và **DynamoDB**. Bạn phải làm quen với **AWS Console**, **AWS CLI**, **AWS IAM** và **CloudFormation**.

Kiến thức cơ bản về **Linux** và **Python** cũng là một lợi thế.

### Nội dung

1.  [Giới thiệu](1-introduce/)
2.  [Solution Deployment](2-prerequiste/)
3.  [Trường hợp cho Cacheable](3-cache/)
4.  [Dọn dẹp tài nguyên](4-terminate/)
