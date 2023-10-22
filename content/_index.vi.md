---
title: "Phát hiện và ứng phó mối đe dọa bằng Amazon GuardDuty và Amazon Detective"
date: "`r Sys.Date()`"
weight: 1
chapter: false
---

#### Tổng quan

GuardDuty là service phát hiện mối đe dọa thông minh, liên tục giám sát các tài khoản AWS của bạn, các EC2 instances, các EKS clusters và dữ liệu được lưu trữ trong S3 để phát hiện hoạt động độc hại mà không cần sử dụng security software và agents. Nếu phát hiện hoạt động độc hại tiềm ẩn, chẳng hạn như hành vi bất thường, đánh cắp thông tin xác thực hoặc control infrastructure (C2) communication, GuardDuty sẽ tạo ra các phát hiện bảo mật chi tiết có thể được sử dụng để hiển thị bảo mật và hỗ trợ khắc phục. Ngoài ra, việc sử dụng tính năng Malware Protection của Amazon GuardDuty giúp phát hiện các tệp độc hại trên các Amazon Elastic Block Store (EBS) được đính kèm với EC2 instance và container workloads.

Amazon Detective đơn giản hóa quy trình điều tra và giúp các nhóm bảo mật tiến hành điều tra nhanh hơn và hiệu quả hơn. Các tập hợp, tóm tắt và bối cảnh dữ liệu dựng sẵn của Amazon Detective giúp bạn nhanh chóng phân tích và xác định bản chất cũng như mức độ của các vấn đề bảo mật có thể xảy ra. Amazon Detective duy trì dữ liệu tổng hợp lên đến một năm và cung cấp dữ liệu đó một cách dễ dàng thông qua một bộ hình ảnh trực quan cho thấy những thay đổi về loại và khối lượng hoạt động trong một khoảng thời gian đã chọn, đồng thời liên kết những thay đổi đó với các phát hiện bảo mật.

Trong bài workshop này, hãy tìm hiểu kiến thức cơ bản về Amazpm GuardDuty và Amazon Detective, đồng thời đi sâu vào các trường hợp và tình huống sử dụng khác nhau.

### Nội dung

1.  [Giới thiệu](1-intro/)
2.  [Giới thiệu Amazon GuardDuty](2-guardduty/)
3.  [Sử dụng Amazon Detective để điều tra](3-detective/)
