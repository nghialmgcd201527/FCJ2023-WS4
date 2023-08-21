---
title: "Device Redirects"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

Trường hợp này được sử dụng để redirect viewer đến proper page của website dựa trên loại thiết bị gửi request đó. Sơ đồ sau đây cho thấy kiến trúc và các requested steps mà chúng ta sẽ làm trong phần này.

![bổ sung](/images/3.cache/3.1-urired/3.1-1kk.png)

#### Step 1: Tạo CloudFront Cache policy để chuyển tiếp device type header:

Để có thể Redirect user dựa vào **device type**, CloudFront headers xác định device type của user sẽ liên kết đến Lambda@Edge function. Để làm được cái đó, bạn sẽ cần tạo Cache Policy trong đó header được thêm vào cache key. Trong bài workshop này, bạn sẽ sử dụng **CloudFront-Is-Mobile-Viewer** những cũng có sẵn những headers khác.

Đến trang [CloudFront Policies console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/policies/cache). Chúng ta làm tương tự như phần **Geo Location Redirects** ở trên, chúng ta tạo một Cache Policy có tên là `device-redirect-cache-policy`. Và ở **Headers** của phần **Cache key settings**, chúng ta tìm kiến và chọn **CloudFront-Is-Mobile-Viewer**. Cuối cùng, chúng ta click vào nút **Create**.

![bổ sung](/images/3.cache/3.3-devicered/3.3-1new.png)

#### Step 2: Tạo Lambda@Edge function và publish new version

Ở bước này, chúng ta sẽ làm như ở phần trước, tạo **Lambda function** với tên `edge-device-redirect`, **Runtime** là **Python 3.9** và deploy nó. Sau đó, chúng ta cần publish new version của Lambda function vừa tạo.

Code source của Lambda function này là:

```
import json
def lambda_handler(event, context):
    print(event)
    #Let's first get the Device Type from the Request coming in
    get_device_viewer_header = event['Records'][0]['cf']['request']['headers']['cloudfront-is-mobile-viewer']
    define_device_type = get_device_viewer_header[0]['value']
    print(define_device_type)
    #Now let's see if this device is a mobile viewer or not and create the redirected response based on that
    if (define_device_type == 'true'):
        response = {
            'status': '301',
            'statusDescription': 'Permanent Redirect',
            'headers': {
                'location': [{
                    'key': 'Location',
                    'value': '/mobile.html'
                }]
            }
        }
        #The response above has been created and a response will be sent to the viewer to redirect it the right device page
        return response
    else:
        #if the device is not mobile, then move along with the request as is.
        request = event['Records'][0]['cf']['request']
        return request
```

#### Step 3: Tạo CloudFront behavior cho trường hợp này

Bây giờ Lambda function vừa được tạo và chúng ta phải assign nó cho một Cache Behavior trong CloudFront. Chúng ta làm như phần **Geo Location Redirects** ở trên. Với **Path Pattern,** chúng ta nhập `/device.html` và hãy nhớ chọn **Cache policy** là **device-redirect-cache-policy** vừa được tạo ở trên.

![bổ sung](/images/3.cache/3.3-devicered/3.3-2new.png)

#### Step 4: Kết hợp Lambda Function với CloudFront Behavior

Chúng ta làm như ở phần **Geo Location Redirects** ở trên, chúng ta sẽ kết hợp Lambda function **edge-device-redirect** với CloudFront Behavior vừa tạo ở trên.

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
curl -v -o /dev/null https://<YOUR-DISTRIBUTION-DOMAIN-NAME>/device.html
```

3. Sau khi build câu lệnh trên từ cloudshell, chúng ta sẽ thấy kết quả như dưới đây.

```

< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 106
< Connection: keep-alive
< ETag: "73940ca66258e1bd0a623690f24fe324"
< Accept-Ranges: bytes
< Server: AmazonS3
< X-Cache: Miss from cloudfront
< Via: 1.1 2f66aa06710fece8ed203ab0ea81eb56.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: IAD89-C3
< X-Amz-Cf-Id: ChlZowW_za1QF6FVmDX2iTENoknUBJxoxpMNV6S28E37g0khLdnxwg==
<

<!DOCTYPE html>
<html>
<body>

<h1>non-mobile URI Page</h1>
<p>non-mobile URI Page</p>

</body>
</html>
```

{{% notice info %}}
Request trên cho thấy response thường xuyên. Điều này xảy ra đơn giản là do device mà chúng ta gửi request không được Lambda@Edge function nhận dạng là mobile device và nó cho phép request diễn ra bình thường.
{{% /notice %}}

4. Hãy chạy lại câu lệnh dưới đây.

```
curl -v https://d2lagt3tnycm19.cloudfront.net/device.html -A "Mozilla/5.0 (iPhone; CPU iPhone OS 6_1_3 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) CriOS/28.0.1500.12 Mobile/10B329 Safari/8536.25"

```

Chúng ta sẽ thấy response khác nhau.

```
< HTTP/1.1 301 Moved Permanently
< Content-Length: 0
< Connection: keep-alive
< Server: CloudFront
< Location: /mobile.html
< X-Cache: Miss from cloudfront
< Via: 1.1 a4cae74c829bc214e4183c38164a2c0a.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: IAD89-C3
< X-Amz-Cf-Id: sQ0_5pUMkOAJOBJUiTdDmEKGaq7ExMDotLmPsi-17WouwqcJBu7mNA==
<
```

{{% notice info %}}
Response ở đây hiện là HTTP 301, biểu thị redirect. Điều này xảy ra vì User-Agent theo request xác định là một mobile device, Lambda@Edge của chúng ta nắm bắt được điều đó và trả lời với một redirect.
{{% /notice %}}

5. Bây giờ, chạy câu lệnh như ở mục 4, chúng ta sẽ thấy sự khác biệt.

```
< HTTP/1.1 301 Moved Permanently
< Content-Length: 0
< Connection: keep-alive
< Server: CloudFront
< Location: /mobile.html
< X-Cache: Hit from cloudfront
< Via: 1.1 38ecebcaa39c8742da2b6336935bb446.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: IAD89-C3
< X-Amz-Cf-Id: 97bOlPobmuIto-LcDA5AbPprf6oXSniK4pq16RRpZSjVPVSb8HUr4Q==
< Age: 133
```

{{% notice info %}}
Sự khác biệt chính ở đây lại là ở X-Cache header có giá trị là **Hit from cloudfront**, nghĩa là function này không cần chạy lại vì request đã được cache.
{{% /notice %}}

Vậy là chúng ta đã deploy thành công redirect logic sử dụng Lambda@Edge và đã test xong. Trường hợp này có thể mở rộng ra phức tạp hơn, xác định không chỉ là **mobile device** mà còn là cả **Android và IOS clients,** xác định **SmartTVs và Tablet** cũng có thể thực hiện được. Tham khảo [tài liệu này](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-cloudfront-headers.html#cloudfront-headers-device-type) để tìm hiểu thêm về tất cả header location có thể có trong CloudFront.
