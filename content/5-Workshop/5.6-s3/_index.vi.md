---
title: "Amazon S3"
date: 2026-06-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

## 5.6. Triển khai Frontend với Amazon S3

Sau khi hoàn thành backend với Amazon API Gateway, AWS Lambda và Amazon DynamoDB, frontend của CloudMenu được triển khai lên Amazon S3.

Amazon S3 được sử dụng để lưu trữ các tệp tĩnh của giao diện web như HTML, CSS và JavaScript.

Trong kiến trúc CloudMenu, S3 bucket không được public trực tiếp. Thay vào đó, Amazon CloudFront được sử dụng để phân phối nội dung frontend đến người dùng.

Luồng truy cập frontend:

```text
User
  ↓
Amazon CloudFront
  ↓
Amazon S3
```

---

### Tạo S3 Bucket

Truy cập **AWS Management Console → Amazon S3** và tạo một S3 bucket để lưu trữ frontend.

Trong workshop này, bucket được sử dụng là:

```text
ozmr-s3-demo-bucket
```

Bucket được tạo tại Region:

```text
Asia Pacific (Singapore) - ap-southeast-1
```

Sau khi tạo bucket, mở bucket để tiến hành upload các tệp frontend của CloudMenu.

---

### Upload Frontend lên S3

Trong tab **Objects**, chọn **Upload** và tải các tệp frontend của CloudMenu lên bucket.

Các tệp chính bao gồm:

```text
app.js
index.html
order.html
style.css
frontend/
```

Trong đó:

- `index.html`: trang giao diện chính của CloudMenu.
- `order.html`: giao diện liên quan đến đơn hàng.
- `app.js`: xử lý logic phía frontend và kết nối với backend API.
- `style.css`: định dạng giao diện của ứng dụng.
- `frontend/`: chứa các tài nguyên frontend bổ sung.

Sau khi quá trình upload hoàn tất, các tệp được lưu dưới dạng S3 Objects trong bucket.

![CloudMenu frontend files in S3](/images/5-Workshop/5.6/s3-objects.png)

*Hình 1. Các tệp frontend của CloudMenu được lưu trữ trong Amazon S3.*

---

### Kiểm tra cấu hình S3 Bucket

Trong tab **Properties**, kiểm tra các thông tin cấu hình của bucket.

Bucket `ozmr-s3-demo-bucket` được triển khai tại Region:

```text
ap-southeast-1
```

Bucket Versioning hiện được đặt ở trạng thái:

```text
Disabled
```

Đối với phạm vi của workshop, Versioning không bắt buộc vì bucket chủ yếu được sử dụng để lưu trữ các tệp frontend tĩnh của CloudMenu.

![S3 bucket properties](/images/5-Workshop/5.6/s3-properties.png)

*Hình 2. Thông tin cấu hình của S3 bucket.*

---

### Cấu hình quyền truy cập

CloudMenu không cho phép người dùng truy cập trực tiếp các object trong S3 bucket.

Trong tab **Permissions**, tùy chọn **Block all public access** được bật:

```text
Block all public access: On
```

Cấu hình này giúp ngăn các tệp frontend trong bucket bị truy cập công khai trực tiếp thông qua Amazon S3.

Thay vào đó, Amazon CloudFront được sử dụng làm lớp phân phối nội dung đến người dùng. S3 đóng vai trò là private origin của CloudFront.

Bucket policy cho phép dịch vụ Amazon CloudFront truy cập các object cần thiết trong bucket.

CloudFront service principal được sử dụng:

```text
cloudfront.amazonaws.com
```

Nhờ cấu hình này, người dùng truy cập frontend thông qua CloudFront thay vì truy cập trực tiếp vào S3 bucket.

Luồng truy cập:

```text
User
  ↓
Amazon CloudFront
  ↓
Private S3 Bucket
  ↓
Frontend Files
```

![S3 bucket permissions](/images/5-Workshop/5.6/s3-permissions.png)

*Hình 3. Cấu hình quyền truy cập của S3 bucket.*

---

### Kết quả

Sau bước này, frontend của CloudMenu đã được lưu trữ thành công trên Amazon S3.

S3 bucket được giữ ở chế độ private và không cho phép public access trực tiếp. Nội dung frontend sẽ được phân phối thông qua Amazon CloudFront ở bước tiếp theo.

Kiến trúc frontend sau khi hoàn thành bước S3:

```text
CloudMenu Frontend
        ↓
Amazon S3
  (Private Bucket)
        ↓
Amazon CloudFront
        ↓
       User
```

Việc sử dụng Amazon S3 giúp CloudMenu có một nơi lưu trữ đơn giản và phù hợp cho các static assets của frontend, trong khi việc giữ bucket ở chế độ private giúp hạn chế truy cập trực tiếp vào các object.

Trong phần tiếp theo, Amazon CloudFront sẽ được cấu hình để sử dụng S3 bucket làm origin và phân phối giao diện CloudMenu đến người dùng.