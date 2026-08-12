---
title: "Dọn dẹp tài nguyên"
date: 2026-06-22
weight: 9
chapter: false
pre: "<b>5.9. </b>"
---

## 5.9. Dọn dẹp tài nguyên

Sau khi hoàn thành quá trình triển khai, kiểm thử và đánh giá CloudMenu, các tài nguyên AWS không còn cần thiết nên được dọn dẹp để hạn chế phát sinh chi phí ngoài ý muốn và giữ môi trường AWS gọn gàng.

Các dịch vụ AWS chính được sử dụng trong Workshop gồm:

- Amazon CloudFront
- Amazon S3
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- AWS Identity and Access Management (IAM)

---

### 1. Dọn dẹp Amazon CloudFront

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

Trước khi xóa Distribution, cần kiểm tra để đảm bảo CloudMenu không còn được sử dụng cho quá trình kiểm thử hoặc trình bày.

Sau đó, thực hiện vô hiệu hóa Distribution theo tùy chọn được cung cấp trên AWS Management Console và chờ quá trình cập nhật hoàn tất.

Khi Distribution đã được vô hiệu hóa và không còn phục vụ request, có thể tiến hành xóa Distribution.

Sau khi CloudFront Distribution được xóa, domain:

```text
d3be9t7i3323e7.cloudfront.net
```

sẽ không còn được sử dụng để phân phối frontend CloudMenu.

Do đó, cần đảm bảo quá trình đánh giá và demo hệ thống đã hoàn tất trước khi thực hiện bước này.

---

### 2. Dọn dẹp Amazon S3

Amazon S3 được sử dụng để lưu trữ các tệp frontend của CloudMenu.

Bucket được sử dụng trong Workshop:

```text
ozmr-s3-demo-bucket
```

Truy cập:

```text
AWS Management Console
→ Amazon S3
→ Buckets
→ ozmr-s3-demo-bucket
```

Trước khi xóa bucket, cần kiểm tra và xóa các object không còn cần thiết bên trong bucket.

Các tệp và thư mục frontend có thể bao gồm:

```text
frontend/
index.html
order.html
kitchen.html
dashboard.html
app.js
style.css
```

Danh sách tệp thực tế có thể khác tùy theo phiên bản frontend đang được triển khai.

Sau khi xác nhận các object không còn cần thiết, tiến hành xóa chúng khỏi bucket.

Tiếp theo, quay lại danh sách S3 Bucket và chọn:

```text
ozmr-s3-demo-bucket
```

Sau khi đảm bảo bucket không còn chứa dữ liệu cần giữ lại, có thể tiến hành xóa bucket.

> **Lưu ý:** Cần kiểm tra kỹ nội dung của bucket trước khi xóa để tránh mất các tệp vẫn còn cần thiết cho quá trình demo hoặc đánh giá.

---

### 3. Dọn dẹp Amazon API Gateway

Amazon API Gateway được sử dụng để tiếp nhận request từ frontend và chuyển request đến các AWS Lambda Function tương ứng.

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

CloudMenuAPI cung cấp các Route chính:

```text
POST /order
GET /orders
PUT /orders/{orderId}
```

Trước khi xóa API, cần đảm bảo frontend CloudMenu không còn cần thực hiện các chức năng:

- Tạo đơn hàng.
- Lấy danh sách đơn hàng.
- Cập nhật trạng thái đơn hàng.

Sau khi xác nhận API không còn được sử dụng, có thể tiến hành xóa:

```text
CloudMenuAPI
```

Khi API Gateway được xóa, các endpoint của CloudMenu sẽ không còn khả dụng và frontend sẽ không thể tiếp tục gửi request đến backend thông qua các endpoint này.

---

### 4. Dọn dẹp AWS Lambda

AWS Lambda được sử dụng để xử lý các nghiệp vụ backend của CloudMenu.

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

Các Function tương ứng với các chức năng:

| Lambda Function | Chức năng |
| :--- | :--- |
| `createOrder` | Tạo đơn hàng mới và ghi dữ liệu vào DynamoDB. |
| `getOrders` | Lấy danh sách đơn hàng từ DynamoDB. |
| `updateOrderStatus` | Cập nhật trạng thái của đơn hàng. |

Sau khi API Gateway đã được dọn dẹp và các Function không còn được sử dụng, có thể lần lượt xóa:

```text
createOrder
getOrders
updateOrderStatus
```

Trước khi xóa, cần kiểm tra để đảm bảo các Function không còn được sử dụng bởi API Gateway hoặc tài nguyên AWS khác.

---

### 5. Dọn dẹp Amazon DynamoDB

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

Bảng chứa các thông tin liên quan đến đơn hàng như:

- `orderId`
- `tableNumber`
- `items`
- `totalAmount`
- `status`
- `createdAt`
- `updatedAt`
- `completedAt`

Trước khi xóa bảng, cần kiểm tra xem dữ liệu đơn hàng có còn cần thiết cho quá trình kiểm thử, báo cáo hoặc đánh giá hệ thống hay không.

Nếu dữ liệu không còn cần thiết, có thể tiến hành xóa bảng:

```text
CloudMenuOrders
```

> **Lưu ý:** Việc xóa bảng `CloudMenuOrders` sẽ xóa dữ liệu đơn hàng được lưu trong bảng. Nếu cần giữ lại dữ liệu, cần thực hiện sao lưu hoặc xuất dữ liệu trước khi xóa.

---

### 6. Kiểm tra IAM Role và Policy

Các AWS Lambda Function cần IAM Execution Role để ghi log vào Amazon CloudWatch Logs và truy cập bảng DynamoDB.

Ví dụ, Function `createOrder` sử dụng Execution Role:

```text
createOrder-role-fmflntg9
```

Role này sử dụng các quyền cần thiết để Lambda hoạt động, bao gồm quyền ghi log và quyền truy cập bảng `CloudMenuOrders`.

Trong quá trình triển khai CloudMenu, Policy liên quan đến DynamoDB được sử dụng là:

```text
CloudMenuDynamoPolicy
```

Sau khi các Lambda Function đã được xóa, truy cập:

```text
AWS Management Console
→ IAM
→ Roles
```

Kiểm tra các IAM Role được tạo riêng cho CloudMenu.

Tiếp theo, truy cập:

```text
AWS Management Console
→ IAM
→ Policies
```

Kiểm tra các Policy không còn được sử dụng.

Nếu Role hoặc Policy không còn được gắn với bất kỳ tài nguyên nào, có thể tiến hành dọn dẹp chúng.

> **Lưu ý:** Không nên xóa IAM Role hoặc Policy nếu chúng vẫn đang được sử dụng bởi tài nguyên AWS khác. Cần kiểm tra dependency trước khi thực hiện thao tác xóa.

---

### Thứ tự dọn dẹp đề xuất

Để hạn chế dependency giữa các tài nguyên, quá trình cleanup có thể được thực hiện theo thứ tự:

```text
Amazon CloudFront
        ↓
Amazon S3 Objects
        ↓
Amazon S3 Bucket
        ↓
Amazon API Gateway
        ↓
AWS Lambda Functions
        ↓
Amazon DynamoDB Table
        ↓
IAM Roles / Policies không còn sử dụng
```

Sau khi hoàn thành cleanup, cần kiểm tra lại AWS Management Console để đảm bảo các tài nguyên được tạo riêng cho CloudMenu không còn hoạt động hoặc duy trì không cần thiết.

---

### Kết quả

Quy trình cleanup giúp xác định và loại bỏ các tài nguyên AWS không còn cần thiết sau khi CloudMenu hoàn thành quá trình triển khai và đánh giá.

Việc dọn dẹp tài nguyên giúp:

- Hạn chế phát sinh chi phí ngoài ý muốn.
- Tránh duy trì các tài nguyên thử nghiệm không còn cần thiết.
- Giữ môi trường AWS gọn gàng và dễ quản lý.
- Loại bỏ các IAM Role và Policy không còn được sử dụng.
- Giảm số lượng tài nguyên không cần thiết trong tài khoản AWS.

Trong phạm vi Workshop hiện tại, các tài nguyên CloudMenu vẫn được duy trì để phục vụ quá trình kiểm thử, trình bày và đánh giá hệ thống. Việc dọn dẹp tài nguyên sẽ được thực hiện sau khi quá trình đánh giá hoàn tất.