---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

#### Tình huống

Công ty của bạn mới sử dụng cloud và đang dần chuyển workload sang môi trường cloud. Với vai trò là security analyst, bạn có trách nhiệm thiết lập một giải pháp có thể phát hiện các mối đe dọa trên account và resources để bạn có thể ưu tiên và xử lí một cách hợp lí.

Bài workshop này sẽ chia thành 3 phần.

- Giới thiệu Amazon GuardDuty
- Phát hiện mối đe dọa và xử lí tình huống
- Dùng Amazon Detective để dò xét

#### Architecture

Đối với bài workshop này, chúng ta sử dụng CloudFormation để khởi chạy infrastructure và AWS services nhằm mô phỏng real application environment. Nhiều EC2 instances và các resource khác được tạo ra. Các resource trong environment, bao gồm các EC2 instance sẽ không được configure theo các best practices nhằm mô phỏng các security events để demo GuardDuty. Để hiểu rõ hơn các best practices về VPC security (https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-best-practices.html) và EC2 best practices (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-best-practices.html), hãy đọc tài liệu của AWS.

Nhiều AWS security services được configure bằng CloudFormation khi bắt đầu buổi workshop. Những services này bao gồm:
- [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
- [Amazon Detective](https://aws.amazon.com/detective/)
- [AWS Security Hub](https://aws.amazon.com/security-hub/)






