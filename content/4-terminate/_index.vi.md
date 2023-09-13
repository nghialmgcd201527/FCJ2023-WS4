---
title: "Dọn dẹp tài nguyên"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

#### CloudFormation

1. Chúng ta vào [CloudFormation Console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringText=&filteringStatus=active&viewNested=true)
2. Chúng ta sẽ thấy stack tên là **edge-redirect-workshop** được tạo từ CloudFormation template. Chúng ta chọn stack đó và click vào nút **Delete** ở trên cùng bên phải.

![VPC](/images/4.clean/4-1new.png)

Chờ khoảng vài phút chúng ta đã xóa thành công CloudFormation cùng những tài nguyên được tạo ra bởi nó trong lúc chúng ta bắt đầu workshop.

#### Lambda Function

1. Chúng ta vào [Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions)
2. Chúng ta sẽ thấy có tất cả Lambda Function được tạo ra trong workshopp này. Tick vào tất cả function sau đó bấm vào nút **Actions** ở trên cùng bên phải và chọn **Delete.**

![VPC](/images/4.clean/4-2new.png)

Chúng ta nhập `delete` vào ô **Confirm** và cuối cùng là nhấn nút **Delete** để xóa. Vậy là chúng ta đã xóa thành công tất cả những Lambda Function được tạo ra trong workshop này.

![VPC](/images/4.clean/4-3new.png)

{{% notice tip %}}
Nếu các bạn nhận được cảnh báo **Deleting Lambda@Edge functions and replicas** trông lúc xóa Lambda Function thì hãy xem ở [tài liệu này](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-delete-replicas.html) và làm theo để xóa thành công.
{{% /notice %}}

Vậy là chúng ta đã thực hiện thành công bài workshop này. Cảm ơn các bạn đã tham gia.
