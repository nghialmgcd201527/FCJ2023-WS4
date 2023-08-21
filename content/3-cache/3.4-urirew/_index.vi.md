---
title: "URI based Rewrites"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

Đây là một technique không liên quan đến việc redirect client browser đến một URL hoàn toàn mới. Tuy nhiên, browser sẽ tiếp tục nhận ra URI mà nó đã request, nhưng thay vì CloudFront truy xuất nó từ cùng một URI tại Original, n ó sẽ lấy nội dung từ một đường dẫn khác. Ví dụ: giả sử một browser đã gửi request tới resource **www.example.com/uri-rewrite.html**, nhưng bạn muốn lấy nội dung từ một đường dẫn cho backend khác hoặc thậm chí là một Orgin khác mà viewer không biết về cái này. Sau đó, bạn sẽ sử dụng Lambda@Edge function hoặc CloudFront function để làm URI rewrite và thực sự truy cập vào Origin để request một URI chẳng hạn như **www.example.com/rewrite.html**

Đối với trường hợp này, chúng ta sẽ sử dụng **Default Behavior** từ CloudFront và sẽ không sử dụng bất kỳ header cụ thể nào để dựa trên logic function của chúng ta, vì vậy bạn cũng không cần tạo cache policy mới. Tuy nhiên, những thứ đó rất có thể được sử dụng để tạo các tình huống phức tạp hơn, trong đó thay vì redirect, chúng ta có thể rewrite URI ở backend. Ở các ví dụ trước của chúng ta trong workshop này, chúng ta có thể phát hiện location hoặc loại device của viewer và lấy một URI khác từ backend thay vì redirect user. Hình ảnh sau đây mô tả cấu trúc về những gì sẽ được xây dựng trong bài workshop này:

![bổ sung](/images/3.cache/3.1-urired/3.1-1kk.png)

#### Step 1: Tạo CloudFront Behavior:

Xem lại ở [ví dụ **URI based Redirects**](/vi/3-cache/3.1-urired), chúng ta tạo Behavior với **Path Pattern** là `/uri-rewrite.html` và ở mục **Origin and origin group**, chúng ta chọn **myS3Origin**.

![bổ sung](/images/3.cache/3.4-urirew/3.4-1new.png)

#### Step 2: Tạo Lambda@Edge function và publish new version

Ở bước này, chúng ta sẽ làm như ở phần trước, tạo **Lambda function** với tên `edge-uri-rewrite`, **Runtime** là **Python 3.9** và deploy nó. Sau đó, chúng ta cần publish new version của Lambda function vừa tạo.

Code source của Lambda function này là:

```
import json
def lambda_handler(event, context):
    #let's first extract the URI from the request
    get_uri = event['Records'][0]['cf']['request']['uri']
    #let's check what is the URI and decide if the URI sent to the Origin should be modified
    if (get_uri == '/uri-rewrite.html'):
        event['Records'][0]['cf']['request']['uri'] = '/rewrite.html'
        request = event['Records'][0]['cf']['request']
        return request
    #if the uri should not be modified then just continue with the request as is
    else:
        request = event['Records'][0]['cf']['request']
        return request
```

#### Step 3: Kết hợp Lambda Function với CloudFront Behavior

Chúng ta làm như ở phần **Geo Location Redirects** ở trên, chúng ta sẽ kết hợp Lambda function **edge-uri-rewrite** với CloudFront Behavior vừa tạo ở trên.

#### Step 5: Set up clients cho testing

Chúng ta sẽ cần một client để chạy curl commands. Cách dễ dàng đó là tạo **CloudShell Environments**. CloudShell là một shell có sẵn trong AWS console và chúng ta có thể chạy những Linux command từ nó. Đi đến [CloudShell Console](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1#) và chờ đến khi terminal sẵn sàng để dùng.

{{% notice info %}}
Nếu CloudShell không hoạt động thì nếu bạn đang thực hành bài workshop này ở Linux/MacOS client thì hai hệ điều hành này đã có sẵn **curl** và bạn chỉ cần chạy command ở client đó. Nếu bạn đang thực hành trên Windows thì hãy tận dùng online curl tools như [cái này](https://reqbin.com/curl). Có thể chạy EC2 instance hoặc Cloud9 IDE từ AWS COnsole để chạy commands.
{{% /notice %}}

#### Step 6: Test redirect configuration

1. Đi đến [CloudShell Console](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1#).

2. Trong phần test, chúng ta sẽ chạy câu lệnh curl để gửi http request đối với distribution của chúng ta, để làm như vậy, chúng ta cần copy Distribution domain name từ CloudFront console nơi chúng ta có thể tìm thấy.

![VPC](/images/3.cache/3.1-urired/3.1-13new.png)

Khi đã tìm thấy distribution domain name, copy câu lệnh sau và thay thế domain name của chúng ta vào.

```
curl -v -o /dev/null https://<YOUR-DISTRIBUTION-DOMAIN-NAME>/uri-rewrite.html
```

3. Sau khi build câu lệnh trên từ cloudshell, chúng ta sẽ thấy kết quả như dưới đây.

```

< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 92
< Connection: keep-alive
< ETag: "a1764624bd34afb8e52157f114ef6db1"
< Accept-Ranges: bytes
< Server: AmazonS3
< X-Cache: Miss from cloudfront
< Via: 1.1 3cf1bfec064e2e01f071e8051a22d830.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: ATL56-C1
< X-Amz-Cf-Id: HeF7XzxKoVyNxUKAAnP0t8bCzpwbN-pyex3mRMbqcH-mQOT5V3yjAw==
<

<!DOCTYPE html>
<html>
<body>

<h1>Rewrite Page</h1>
<p>Rewrite Page</p>

</body>
</html>
```

{{% notice info %}}
Request trên cho thấy direct response đối với request của chúng ta là HTTP 200 OK, nghĩa là đây không phải là redirect, thay vào đó nội dung đã được cung cấp, tuy nhiên trong trang này, bạn có thể thấy HTML được tạo cho biết đây là nội dung rewrite thay vì nội dung chính.
{{% /notice %}}

Vậy là chúng ta đã deploy thành công trường hợp URI rewrite. Trường hợp này có thể mở rộng để xử lí logic phức tạp hơn cũng như thậm chí truy xuất nội dụng từ các Origin khác nhau nếu được request. Trường hợp này cũng có thể được thực hiện bằng cách sử dụng các function của CloudFront nếu trường hợp của chúng tôi yêu tất cả request phải được evaluate và rewrite.
