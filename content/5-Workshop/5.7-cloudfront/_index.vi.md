---
title: "Triển khai Amazon CloudFront"
date: 2026-06-22
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

## 5.7. Triển khai Amazon CloudFront

Sau khi tải mã nguồn frontend lên Amazon S3, bước tiếp theo là cấu hình **Amazon CloudFront** để phân phối giao diện CloudMenu đến người dùng.

Trong kiến trúc này, Amazon S3 đóng vai trò lưu trữ các tệp tĩnh của frontend, trong khi Amazon CloudFront đóng vai trò Content Delivery Network (CDN), tiếp nhận request từ người dùng và phân phối nội dung từ S3.

Luồng truy cập frontend được triển khai như sau:

```text
User
  |
  v
Amazon CloudFront
  |
  v
Amazon S3
  |
  v
/frontend
  |
  v
index.html
```

### Kiểm tra CloudFront Distribution

CloudFront Distribution được sử dụng cho ứng dụng có tên:

```text
CloudMenu
```

Sau khi Distribution được tạo thành công, CloudFront cung cấp Distribution Domain Name:

```text
d3be9t7i3323e7.cloudfront.net
```

Domain này được sử dụng làm địa chỉ truy cập frontend của ứng dụng CloudMenu.

![CloudFront Distribution](images/cloudfront-distribution.png)

Thông tin cấu hình chính của Distribution:

| Cấu hình | Giá trị |
|---|---|
| Distribution | CloudMenu |
| Distribution domain | `d3be9t7i3323e7.cloudfront.net` |
| Default root object | `index.html` |
| Supported HTTP versions | HTTP/2, HTTP/1.1, HTTP/1.0 |
| Standard logging | Off |
| Custom domain | Chưa cấu hình |

### Cấu hình Default Root Object

Trong phần **General → Settings**, cấu hình:

```text
Default root object: index.html
```

![CloudFront General Settings](images/cloudfront-general-settings.png)

`index.html` được thiết lập làm Default Root Object để khi người dùng truy cập trực tiếp CloudFront domain mà không chỉ định tên tệp, CloudFront sẽ tự động yêu cầu tệp `index.html` từ origin.

Ví dụ:

```text
https://d3be9t7i3323e7.cloudfront.net/
```

CloudFront sẽ phân phối nội dung tương ứng với:

```text
index.html
```

Điều này cho phép người dùng truy cập frontend CloudMenu bằng domain chính của CloudFront mà không cần nhập trực tiếp `/index.html`.

### Cấu hình Amazon S3 làm Origin

Trong tab **Origins**, Amazon S3 bucket được cấu hình làm origin cho CloudFront Distribution.

Bucket được sử dụng:

```text
ozmr-s3-demo-bucket
```

Origin Path:

```text
/frontend
```

![CloudFront Origin](images/cloudfront-origin.png)

Việc cấu hình Origin Path là `/frontend` cho phép CloudFront lấy các tệp frontend từ thư mục:

```text
s3://ozmr-s3-demo-bucket/frontend/
```

Thay vì truy cập các object ở root của bucket.

Do đó, khi CloudFront yêu cầu:

```text
index.html
```

request thực tế sẽ được chuyển đến object:

```text
/frontend/index.html
```

trong S3 bucket.

### Bảo vệ S3 Origin

S3 bucket được cấu hình **Block Public Access**, do đó người dùng không truy cập trực tiếp các frontend object trong bucket thông qua public S3 access.

CloudFront được cấu hình quyền truy cập origin để có thể đọc các object cần thiết từ S3.

Luồng truy cập được giới hạn theo mô hình:

```text
User
   |
   v
CloudFront
   |
   v
Private S3 Bucket
```

Thay vì:

```text
User
   |
   X
Direct public access to S3
```

Cách triển khai này giúp S3 tiếp tục được giữ private trong khi frontend vẫn có thể được phân phối thông qua CloudFront.

### Cấu hình Behavior

Trong tab **Behaviors**, Distribution sử dụng default behavior:

```text
Path pattern: Default (*)
```

Viewer Protocol Policy được cấu hình:

```text
Redirect HTTP to HTTPS
```

Cache Policy:

```text
Managed-CachingOptimized
```

![CloudFront Behavior](images/cloudfront-behavior.png)

Với cấu hình **Redirect HTTP to HTTPS**, các request sử dụng HTTP sẽ được chuyển hướng sang HTTPS trước khi nội dung được phân phối.

Ví dụ:

```text
http://d3be9t7i3323e7.cloudfront.net
```

sẽ được chuyển hướng sang kết nối HTTPS.

Cache policy `Managed-CachingOptimized` được sử dụng để CloudFront cache các nội dung phù hợp tại edge locations, giúp hạn chế số request phải gửi lại đến S3 origin.

### Luồng phân phối frontend

Sau khi hoàn thành cấu hình, quá trình phân phối frontend hoạt động theo luồng:

```text
Browser
   |
   | HTTPS Request
   v
Amazon CloudFront
   |
   | Check cached content
   |
   +------ Cache Hit ------> Return content
   |
   | Cache Miss
   v
Amazon S3
ozmr-s3-demo-bucket
   |
   v
/frontend
   |
   v
index.html / CSS / JavaScript
   |
   v
CloudFront
   |
   v
Browser
```

Khi nội dung đã tồn tại trong CloudFront cache, CloudFront có thể trả response trực tiếp cho người dùng. Nếu nội dung chưa có trong cache, CloudFront lấy object từ S3 origin và phân phối lại cho client.

### Kết quả

Sau bước này, CloudMenu đã hoàn thành cấu hình lớp phân phối frontend:

```text
User
   |
   v
Amazon CloudFront
   |
   v
Amazon S3
   |
   v
CloudMenu Frontend
```

CloudFront cung cấp HTTPS endpoint cho frontend, trong khi Amazon S3 tiếp tục đóng vai trò lưu trữ các static assets của ứng dụng.

Việc kiểm thử truy cập website thông qua CloudFront domain và kiểm tra kết nối giữa frontend với backend API sẽ được thực hiện trong phần **5.8. Testing**.