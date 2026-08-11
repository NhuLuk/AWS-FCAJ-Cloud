---
title: "Dọn dẹp tài nguyên"
date: 2026-06-22
weight: 9
chapter: false
pre: "<b>5.9. </b>"
---

## 5.9. Dọn dẹp tài nguyên

Sau khi hoàn thành quá trình triển khai và kiểm thử CloudMenu, các tài nguyên AWS không còn cần thiết nên được dọn dẹp để hạn chế phát sinh chi phí ngoài ý muốn.

Các dịch vụ AWS chính được sử dụng trong Workshop gồm:

- Amazon CloudFront
- Amazon S3
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB

> **Lưu ý:** Cần kiểm tra kỹ dữ liệu và các tài nguyên trước khi xóa vì một số thao tác xóa không thể hoàn tác.

### 1. Xóa CloudFront Distribution

Amazon CloudFront được sử dụng để phân phối frontend CloudMenu từ Amazon S3 đến người dùng.

Truy cập:

```text
AWS Management Console
→ CloudFront
→ Distributions
```

Chọn Distribution:

```text
CloudMenu
```

Trước khi xóa Distribution, cần disable Distribution trước.

Thực hiện:

```text
CloudMenu
→ Disable
```

Chờ quá trình cập nhật hoàn tất, sau đó chọn:

```text
Delete
```

Việc xóa CloudFront Distribution sẽ ngừng quá trình phân phối frontend CloudMenu thông qua CloudFront.

![Xóa CloudFront Distribution](images/cleanup-cloudfront.png)

---

### 2. Xóa các Object trong S3 Bucket

Amazon S3 được sử dụng để lưu trữ các tệp frontend của CloudMenu.

Bucket được sử dụng trong Workshop:

```text
ozmr-s3-demo-bucket
```

Truy cập:

```text
Amazon S3
→ Buckets
→ ozmr-s3-demo-bucket
```

Trong tab **Objects**, chọn các object và thư mục frontend không còn cần thiết, sau đó chọn:

```text
Delete
```

Các tệp có thể bao gồm:

```text
frontend/
index.html
order.html
app.js
style.css
```

Xác nhận thao tác để xóa các object đã chọn.

![Xóa các Object trong S3](images/cleanup-s3-objects.png)

---

### 3. Xóa S3 Bucket

Sau khi các object trong bucket đã được xóa, quay lại danh sách S3 Bucket.

Chọn:

```text
ozmr-s3-demo-bucket
```

Sau đó chọn:

```text
Delete
```

Nếu AWS yêu cầu xác nhận, nhập lại tên bucket:

```text
ozmr-s3-demo-bucket
```

Sau khi hoàn tất, bucket được sử dụng để lưu trữ frontend CloudMenu sẽ được xóa.

![Xóa S3 Bucket](images/cleanup-s3-bucket.png)

---

### 4. Xóa API Gateway

CloudMenu sử dụng HTTP API để kết nối frontend với các Lambda Function của backend.

Truy cập:

```text
AWS Management Console
→ API Gateway
→ APIs
```

Chọn API:

```text
CloudMenuAPI
```

API bao gồm các Route chính:

```text
POST /order
GET /orders
PUT /orders/{orderId}
```

Chọn:

```text
Delete
```

và xác nhận thao tác xóa API.

![Xóa API Gateway](images/cleanup-api-gateway.png)

---

### 5. Xóa Lambda Function

AWS Lambda được sử dụng để xử lý logic backend của CloudMenu.

Truy cập:

```text
AWS Management Console
→ Lambda
→ Functions
```

Các Lambda Function được sử dụng trong CloudMenu gồm:

```text
createOrder
getOrders
updateOrderStatus
```

Lần lượt chọn các Function không còn cần thiết và thực hiện:

```text
Actions
→ Delete
```

Sau đó xác nhận thao tác xóa.

![Xóa Lambda Function](images/cleanup-lambda.png)

---

### 6. Xóa DynamoDB Table

Amazon DynamoDB được sử dụng để lưu trữ dữ liệu đơn hàng của CloudMenu.

Truy cập:

```text
AWS Management Console
→ DynamoDB
→ Tables
```

Chọn bảng:

```text
CloudMenuOrders
```

Sau đó chọn:

```text
Delete
```

Xác nhận thao tác để xóa bảng.

> Việc xóa bảng DynamoDB sẽ đồng thời xóa dữ liệu đơn hàng được lưu trong bảng. Chỉ thực hiện bước này khi dữ liệu không còn cần thiết.

![Xóa DynamoDB Table](images/cleanup-dynamodb.png)

---

### Thứ tự dọn dẹp đề xuất

Có thể thực hiện quá trình cleanup theo thứ tự:

```text
CloudFront Distribution
        ↓
S3 Objects
        ↓
S3 Bucket
        ↓
API Gateway
        ↓
Lambda Functions
        ↓
DynamoDB Table
```

Sau khi hoàn tất, kiểm tra lại AWS Management Console để đảm bảo các tài nguyên được tạo riêng cho Workshop CloudMenu đã được xóa hoặc không còn sử dụng.

### Kết quả

Quá trình cleanup giúp loại bỏ các tài nguyên AWS không còn cần thiết sau khi hoàn thành Workshop.

Việc dọn dẹp tài nguyên giúp:

- Hạn chế phát sinh chi phí ngoài ý muốn.
- Tránh duy trì các tài nguyên thử nghiệm không còn sử dụng.
- Giữ môi trường AWS gọn gàng và dễ quản lý.

Đến đây, quá trình triển khai, kiểm thử và dọn dẹp hệ thống **CloudMenu** đã hoàn tất.