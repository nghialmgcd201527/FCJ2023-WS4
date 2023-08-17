---
title: "Solution Deployment"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Cách làm này sẽ thực hiện việc tạo ra các services để phục vụ cho bài workshop này. Các services này sẽ được tạo ra bằng cách sử dụng **AWS CloudFormation**. **AWS CloudFormation** là một dịch vụ của AWS giúp chúng ta tự động hóa việc triển khai các tài nguyên của AWS. Chúng ta sẽ sử dụng AWS CloudFormation để tạo ra **CloudFront distribution, Amazon S3 Bucket và IAM Role.**

Hãy cùng deploy template này như dưới đây:

- [Khởi chạy CloudFormation template này](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://s3.us-east-1.amazonaws.com/ee-assets-prod-us-east-1/modules/9d6613dae7674a6d96e1ccd3b3096dda/v1/edge-workshop.yaml&stackName=edge-redirect-workshop), chúng ta sẽ nhận được một giao diện như sau:

![bo sung](/images/2.prerequisite/2-1.png)

- Ở trong trang này, lướt xuống phần **Capabilities,** chọn vào ô "**I acknowledge that AWS CloudFormation might create IAM resources with custom names.**" và nhấn vào nút **Create stack.**

![bo sung](/images/2.prerequisite/2-2.png)

- Sẽ mất vài phút để stack này được tạo ra thành công. Bạn sẽ thấy tương tự như hình bên dưới.

![bo sung](/images/2.prerequisite/2-3.png)

- Tiếp tục, hãy tải **file html** (sẽ phục vụ cho **Origin** của chúng ta). [Tải ở đây](https://static.us-east-1.prod.workshops.aws/public/cabd0ea6-06a9-4598-8680-71c16b92fe28/static/edge-redirect-origin.zip). Lưu trữ nó ở máy của chúng ta và giải né. File này sẽ bao gồm **file html** mà chúng ta sẽ sử dụng cho **Origin content.** Bây giờ chúng ta hãy di chuyển sang [trang S3 bucket](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#). Chúng ta sẽ thấy S3 bucket được tạo ra bởi CloudFormation template của chúng ta.

![bo sung](/images/2.prerequisite/2-4.png)

- Click vào S3 bucket đó, click vào nút **Upload.**

![bo sung](/images/2.prerequisite/2-5.png)

- Click vào nút **Add files,** chọn **file html** đã tải ở trên và tải file đó lên S3 bucket của chúng ta.

![bo sung](/images/2.prerequisite/2-6.png)

Vậy là chúng ta vừa hoàn thành xong việc cài đặt môi trường. Bây giờ chúng ta có thể bắt đầu thực hành với bài workshop của chúng ta.

{{% notice info %}}
Chúng ta có thể deploy resource của chúng ta theo cách khác đó là **CDK Project.** Chúng ta sẽ dùng [CloudShell](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1) ở AWS Region **us-east-1**. Tải file CDK project dưới dạng zip [ở đường dẫn này](https://s3.amazonaws.com/ee-assets-prod-us-east-1/modules/9d6613dae7674a6d96e1ccd3b3096dda/v1/workshop.zip). Và đọc tài liệu hỗ trợ cho deploy CDK project [ở đây](https://docs.aws.amazon.com/cdk/v2/guide/home.html).
{{% /notice %}}

Hãy cùng xem lại tất cả resources được deploy để hiểu hơn về bài workshop của chúng ta. Rồi hãy cùng bắt tay vào thực hành.
