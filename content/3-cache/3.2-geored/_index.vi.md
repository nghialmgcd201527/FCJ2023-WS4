---
title: "Geo Location Redirects"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

Trường hợp này thường được dùng để redirect viewer đến country page của website của chúng ta. Ở sơ đồ dưới đây, chúng ta có thể thấy cấu trúc được build cho phần này.

![bổ sung](/images/3.cache/3.1-urired/3.1-1kk.png)

#### Step 1: Tạo CloudFront Cache policy để chuyển tiếp country header:

Để có thể Redirect user dựa vào **country location**. Chúng ta cần đảm bảo rằng **Contry Headeer** liên kết với Lambda@Edge function. Để làm được điều này, chúng ta cần tạo **Cache Policy** nơi header được thêm vào cache key.

1. Đến trang [CloudFront Cache Policies console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/policies/cache).

2. Ở mục **Custom Policies,** click vào nút **Create cache policy**.

![bổ sung](/images/3.cache/3.2-geored/3.2-1.png)

3. Ở trang **Create cache policy**, đặt tên cho nó là **edge-redirect-cache-policy**. Ở phần **Cache key settings**, mở rộng phần **Headers** ra và chọn **Include the following headers**. Ở phần **Add header**, chúng ta tìm kiếm **CloudFront-Viewer-Contry** và chọn nó. Những mục khác chúng ta để mặc định và click vào nút **Create**.

![bổ sung](/images/3.cache/3.2-geored/3.2-2.png)

#### Step 2: Tạo Lambda@Edge function và publish new version

Ở bước này, chúng ta sẽ làm như ở phần trước, tạo **Lambda function** với tên `edge-geo-redirect`, **Runtime** là **Python 3.9** và deploy nó. Sau đó, chúng ta cần publish new version của Lambda function vừa tạo.

Code source của Lambda function này là:

```
import json
def lambda_handler(event, context):
    #Let's first get the Country Code from the Request coming in
    get_country_viewer_header = event['Records'][0]['cf']['request']['headers']['cloudfront-viewer-country']
    define_country = get_country_viewer_header[0]['value']
    #Now let's define which country that is and create our redirected response
    if (define_country == 'US'):
        response = {
            'status': '301',
            'statusDescription': 'Permanent Redirect',
            'headers': {
                'location': [{
                    'key': 'Location',
                    'value': '/en-us.html'
                }]
            }
        }
        #The response above has been created and a response will be sent to the viewer to redirect it to the /en-us.html
        return response
    else:
        #if the country has not been identified then move on with the request
        request = event['Records'][0]['cf']['request']
        return request
```

#### Step 3: Tạo CloudFront behavior cho trường hợp này

Bây giờ Lambda function vừa được tạo và chúng ta phải assign nó cho một Cache Behavior trong CloudFront.

1. Đi đến [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home). Các bạn sẽ thấy Distribution được tạo ra bởi CloudFormation template, nó sẽ được xác định bởi **Edge Redirect Workshop Distribution** như ở mục **Description**.

2. Chọn Distribution đó sau đó chọn vào mục **Behaviors.**

3. Click vào nút **Create Behavior.**

4. Ở mục **Path pattern,** chúng ta nhập `/geo.html`. Ở dưới là phần **Origin and origin groups,** chúng ta chọn **myS3Origin.** Ở phần **Viewer protocol policy,** chúng ta chọn **Redirect HTTP to HTTPS.** Ở mục **Cache key and origin requests**, chúng ta chọn **Cache policy and origin request policy**, tiếp theo mở rộng mục **Cache policy**, chúng ta chọn policy vừa tạo ở bước trên **edge-redirect-cache-policy.** Những mục còn lại chúng ta sẽ để mặc định và click vào nút **Create behavior** ở cuối trang.

![bổ sung](/images/3.cache/3.2-geored/3.2-3.png)

#### Step 4: Kết hợp Lambda Function với CloudFront Behavior

Chúng ta làm như ở trường hợp **URI based Redirects** ở trên, chúng ta sẽ kết hợp Lambda function **edge-geo-redirect** với CloudFront Behavior vừa tạo ở trên.

#### Step 5: Set up clients cho testing

Chúng ta sẽ cần một client để chạy curl commands. Cách dễ dàng đó là tạo **CloudShell Environments**. CloudShell là một shell có sẵn trong AWS console và chúng ta có thể chạy những Linux command từ nó. Đi đến [CloudShell Console](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1#) và chờ đến khi terminal sẵn sàng để dùng.

{{% notice info %}}
Nếu CloudShell không hoạt động thì nếu bạn đang thực hành bài workshop này ở Linux/MacOS client thì hai hệ điều hành này đã có sẵn **curl** và bạn chỉ cần chạy command ở client đó. Nếu bạn đang thực hành trên Windows thì hãy tận dùng online curl tools như [cái này](https://reqbin.com/curl). Có thể chạy EC2 instance hoặc Cloud9 IDE từ AWS COnsole để chạy commands.
{{% /notice %}}

#### Step 6: Test redirect configuration

1. Đi đến [CloudShell Console](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1#) ở AWS Region **us-east-1**.

2. Trong phần test, chúng ta sẽ chạy câu lệnh curl để gửi http request đối với distribution của chúng ta, để làm như vậy, chúng ta cần copy Distribution domain name từ CloudFront console nơi chúng ta có thể tìm thấy.

![VPC](/images/3.cache/3.1-urired/3.1-13.png)

Khi đã tìm thấy distribution domain name, copy câu lệnh sau và thay thế domain name của chúng ta vào.

```
curl -v -o /dev/null https://<YOUR-DISTRIBUTION-DOMAIN-NAME>/geo.html
```

3. Sau khi build câu lệnh trên từ cloudshell, chúng ta sẽ thấy kết quả như dưới đây.

```
curl -v -o /dev/null d2lagt3tnycm19.cloudfront.net/geo.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 108.157.172.138:80...
* Connected to d2lagt3tnycm19.cloudfront.net (108.157.172.138) port 80 (#0)
> GET /geo.html HTTP/1.1
> Host: d2lagt3tnycm19.cloudfront.net
> User-Agent: curl/7.77.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 301 Moved Permanently
< Content-Length: 0
< Connection: keep-alive
< Server: CloudFront
< Location: /en-us.html
< X-Cache: Miss from cloudfront
< Via: 1.1 6fbeae74487f866b555dc44d03fcc2a6.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: MIA3-P3
< X-Amz-Cf-Id: VRNhLpCaNiZ08Fw5f2eMtWn8KfHnkPp3qp4kQeft3CVfEjo6kXIEWQ==
<
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
```

4. Hãy chạy lại câu lẹnh trên một lần nữa ở AWS Region **us-east-1**. Chúng ta sẽ thấy response khác nhau.

```
curl -v -o /dev/null d2lagt3tnycm19.cloudfront.net/geo.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 108.157.172.133:80...
* Connected to d2lagt3tnycm19.cloudfront.net (108.157.172.133) port 80 (#0)
> GET /geo.html HTTP/1.1
> Host: d2lagt3tnycm19.cloudfront.net> User-Agent: curl/7.77.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 301 Moved Permanently
< Content-Length: 0
< Connection: keep-alive
< Server: CloudFront
< Location: /en-us.html
< X-Cache: Hit from cloudfront
< Via: 1.1 e759cef9ef04dc6632a71818dfac3a76.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: MIA3-P3
< X-Amz-Cf-Id: aOe3ADve7yDyHH_Iyb7MGMPAX8LWuTli79qf-nFl8rT40-VxPb_Zeg==
< Age: 158
```

{{% notice info %}}
Chỉ có sự khác biệt ở đây là **X-Cache** response header. Chúng ta sẽ thấy **Hit from cloudfront** vì ở response đầu tiên sau khi request, nó đã được cache ở CloudFront nên request thứ hai sẽ không cần phải đi qua tất cả function thay vào đó nó sẽ chỉ được hoạt đồng từ cached content, điều này cung cấp hiệu suất tốt hơn và cũng giúp tiết kiệm chi phí vì function không cần trigger lần nữa.
{{% /notice %}}

5. Bây giờ, quay trở lại shell console và chạy cùng câu lệnh trên ở AWS Region **eu-west-1**. Bây giờ response của chúng ta sẽ không phải là **HTTP 301** mà là **HTTP 200 OK** như dưới đây.

```
curl -v  d2lagt3tnycm19.cloudfront.net/geo.html
*   Trying 108.157.172.133:80...
* Connected to d2lagt3tnycm19.cloudfront.net (108.157.172.133) port 80 (#0)
> GET /geo.html HTTP/1.1
> Host: d2lagt3tnycm19.cloudfront.net
> User-Agent: curl/7.79.1
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 98
< Connection: keep-alive
< Last-Modified: Wed, 02 Mar 2022 14:15:57 GMT
< ETag: "be3c901839ae019e0c58908e45f5ab45"
< Accept-Ranges: bytes
< Server: AmazonS3
< X-Cache: Miss from cloudfront
< Via: 1.1 9bbdfc2323989883f386114cc53fdbd0.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: MIA3-P3
< X-Amz-Cf-Id: 2UlANwUbwF5Mv1V-XKWcYWKrTzIJZh2asxxw_xOK7aahkVP6TS6tRg==
<

<!DOCTYPE html>
<html>
<body>

<h1>Not our US page</h1>
<p>Not our US page</p>

</body>
</html>
```

{{% notice info %}}
Response HTTP 200 OK của request này nghĩa là nó không được redirected và thay vào đó, resouce được request đã được CloudFront cung cấp.
{{% /notice %}}

Vậy là chúng ta đã deploy thành công redirect logic sử dụng Lambda@Edge và đã test xong. Trường hợp này có thể mở rộng ra phức tạp hơn, xác định không chỉ ở **Contry** mà còn cả các vị trí địa lí khác như **States, Cities, postal codes, time zone**. Tham khảo [tài liệu này](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-cloudfront-headers.html#cloudfront-headers-viewer-location) để tìm hiểu thêm về tất cả header location có thể có trong CloudFront.
