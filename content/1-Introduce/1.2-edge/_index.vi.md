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

![bo sung](/images/2.prerequisite/2.1-createcloud9workspace/2.1-1asdas.png)

1. **Viewer Request** - Function này thực thi khi CloudFront nhận được request từ viewer và trước khi nó kiểm tra xem đối tượng được yêu cầu có trong edge cache hay không.

2. **Origin Request** - Function này chỉ thực thi khi CloudFront chuyển tiếp request đến origin của chúng ta. Khi request object nằm trong edge cache, function không được thực thi.

3. **Origin Response** - Function này thực thi sau khi CloudFront nhận được response từ origin và trước khi nó cache object trong response.

4. **Viewer Response** - Function thực thi trước khi trả lại requested object cho viewer. Function thực thi bất kể object đã có trong edge cache hay chưa.

Tất cả 4 trigger options trên đều khả dụng với **Lambda@Edge** trong khi chỉ có **viewer triggers** khả dung với **CloudFront Functions.** Đây là một trong những điểm khác biệt và quan trọng nhất giữa hai feature trên và chúng ta sẽ khám phá thêm về feature **Lambda@Edge** trong suốt bài workshop.

#### CloudFront Edge Locations và Regional Edge Caches (RECs)

[**CloudFront Edge Locations**](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) là những điểm hiện diện nơi các request của user sẽ được gửi đến dựa trên độ trễ thấp nhất đối với user gửi yêu cầu đó. Content của **CloudFront Delivers** được detect bằng những thứ mà Edge Location có thể phục vụ request và định tuyến users đến Edge Locations đã được xác định.

[**Regional Edge Cache**](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowCloudFrontWorks.html#CloudFrontRegionaledgecaches) là middle tier caching layer nằm giữa **Edge Location** và **Origin.** Các **Regional Edge Cache servers** được dùng để cho phép nhiều content hơn được cache gần hơn với user.

Biểu đồ sau đây biểu diễn cách nhóm **Edge Locations** và **Regional Edge Cache** được CloudFront sử dụng:

![bo sung](/images/2.prerequisite/2.1-createcloud9workspace/2.1-1asdas.png)
