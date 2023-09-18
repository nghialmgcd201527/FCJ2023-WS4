---
title: "Generating Load"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

#### Test Client - Artillery Installation

Để generate load trên hai architecture mới được deploy, các bạn sẽ sử dụng công cụ distributed load testing [Artillery.io](https://artillery.io/). Các bước sau đây sẽ hướng dẫn bạn cách cài đặt và sử dụng công cụ này trong Cloud9 environment.

Đầu tiên, các bạn sẽ kiểm tra npm được cài đặt hay chưa bằng câu lệnh `npm -v` trên terminal của Cloud9. Nếu các bạn chưa cài đặt thì hãy cài đặt cả node và npm. Hãy làm theo [hướng dẫn](https://nodejs.org/en/) để cài đặt node và npm cho hệ thống của bạn.

```
npm -v
```

Nếu npm đã được cài đặt, các bạn sẽ thấy output như hình.

![Alt text](image.png)

Tiếp theo, chạy câu lệnh dưới đây để cài đặt **Artillery**. Hãy đọc [Artillery.io documentation](https://artillery.io/docs/guides/overview/welcome.html) để tìm hiểu thêm về công cụ này.

```
npm install -g artillery
```

Khi cài đặt thành công, chúng ta sẽ thấy như hình bên dưới.

![Alt text](image-1.png)

Cài đặt **Artillery Engine Lambda** bằng câu lệnh dưới đây. Tài liệu tham khảo [Artillery Engine Lambda](https://github.com/orchestrated-io/artillery-engine-lambda).

```
npm install -g artillery-engine-lambda@1.0.18
```

Khi cài đặt thành công, chúng ta sẽ thấy như hình bên dưới.

![Alt text](image-2.png)

#### Generate Traffic

Có một file artillery loadTest đã được đưa vào thư mục traffic-throttle được giải nén trước đó. Các bạn có thể kiểm tra bằng cách điều hướng đến thư mục **traffic-throttle/artillery** và mở file **LambdaTest.yaml**.

![Alt text](image-3.png)

Các bạn sẽ cần đến tên của hai Lambda functions đã được deploy ở lúc đầu phàn này. Để lấy được, hãy vào [Lambda Console](https://console.aws.amazon.com/lambda).

![Alt text](image-4.png)

Đầu tiên, hãy cùng test flow của **non-rate limited**. Copy tên function bắt đầu bằng **traffic-throttle-StandardLambdaDDB**. Sau đó, mở lại file **LambdaTest.yaml** ở Cloud9 workspace, paste tên function đó vào key **target**. Đổi **region** thành region mà Lambda function đó được deploy. Lưu lại file.

![Alt text](image-5.png)

Bắt đầu test bằng cách chạy câu lệnh dưới đây ở Cloud9 terminal. Hãy chắc chắn bạn đang ở đường dẫn **traffic-throttle/artillery**. Sẽ mất khoảng 3 đến 4 phút để hoàn thành.

```
artillery run lambdaTest.yaml
```

Bây giờ hãy test flow của **Throttle**. Copy tên function bắt đầu bằng **traffic-throttle-SQSInsertLambda** và paste vào key **target**. Chạy lại câu lẹnh dưới đây để test.

```
artillery run lambdaTest.yaml
```







