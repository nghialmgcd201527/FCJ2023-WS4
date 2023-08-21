---
title: "Trường hợp cho Cacheable"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

#### Xử lí Redirects bằng Lambda@Edge

Trong phần này, chúng ta sẽ đề cập đến trường hợp có thể được cached. Lý tưởng nhất là các trường hợp này sẽ được xử lí bằng cách sử dụng **Lambda@Edge.**

Như đã nhắc đến trong phần [Edge Compute Introduction](/vi/1-introduce/1.2-edge/), có nhiều triggers có sẵn cho **Lambda@Edge,** các trường hợp sẽ được giải quyết bằng cách sử dụng **Origin facing event triggers.**

![bo sung](/images/3.cache/3-1new.png)

Lambda@Edge Functions đang sử dụng **Origin facing triggers** sẽ được trigger sau khi request được đánh evaluate ở **CloudFront Caching Layers,** do đó, nếu có một response đã được cache thì một response sẽ được back lại viewer, tiết kiệm function invocations. Điều này không chỉ tăng tốc độ phản hồi mà còn giúp giảm chi phí redirects của chúng ta vì Lambda sẽ chỉ được kích hoặc nếu không có response nào được cached.

### Nội dung

- [Khai báo Table trong DynamoDB](3.1-dynamodb/)
- [Tạo Lambda function](3.2-lambdafunction/)
