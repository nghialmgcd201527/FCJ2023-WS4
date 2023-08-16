---
title: "Use Cases"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.1 </b> "
---

Trong phần này, chúng ta sẽ tìm hiểu thông tin cơ bản về các tính năng của Edge Compute mà chúng ta sẽ khám phá trong bài workshop này. Nó sẽ cung cấp cho chúng ta kiến thức nền tảng và nội dung liên quan đến các bài labs.

Trước khi giới thiệu về [AWS Edge computes](/vi/1-introduce/1.2-edge/), chúng ta cần có những kiến thức về các techniques khác nhau được sử dụng trong bài workshop này để chuyển hướng các trang từ trang khác từ yêu cầu của người dùng. Nó được gọi là **Redirects** và **Rewrites**.

- **Redirects:** URL redirections đưa trình duyệt đến một resource mới hoặc một trang web khác. Các HTTP response codes phổ biến là HTTP 301, HTTP 302 và HTTP 308. Sau khi trình duyệt nhận được response từ server, nó sẽ fetch content từ URL mà server trả về. Để biết thêm thông tin, hãy xem [ở đây](https://developer.mozilla.org/en-US/docs/Web/HTTP/Redirections). Có một số trường hợp cần sử dụng đó là khách hàng muốn Redirects bao gồm các tình huống cần routing users đến domain mới hoặc ngay cả khi redirect users đến website chính, redirect để đưa user đến trang dựa vào thiết bị hoặc vị trí địa lí của họ và những trường hợp khác mà chúng ta sẽ tìm hiểu trong bài workshop này.

- **Rewrites:** URL rewrites cũng là một technique được sử dụng để lưu trữ một nội dung khác với nội dung users yêu cầu, tuy nhiên nó sẽ không xử lí một redirect đầy đủ. Thay vào đó, nó sẽ chỉ phục vụ một nội dung khác từ phía backend của website hoặc ứng dụng mà user không hề biết hoạt gì đang diễn ra. Ví dụ, chúng ta có thể nghĩ về việc trình duyệt gửi GET request để fetch nội dung cho "**_/index.html_**", tuy nhiên có thể sửa đổi URL theo yêu cầu này và làm cho phía backend trả về nội dung từ "**_/index_rewrite.html_**". Việc này về cơ bản sẽ cung cấp nội dung khác với yêu cầu ban đầu mà không cần server đưa user đến với URL/page hoàn toàn khác. Một số trường hợp sử dụng rewrite URL là các trường hợp cần phải sắp xếp lại các kí tự từ URL, trong trường hợp này, nó sẽ lấy content về và không thay đổi URL là rất quan trọng để trải nghiệm của user có thể đơn giản hơn và những trường hợp khác mà chúng ta sẽ tìm hiểu trong bài workshop này.
