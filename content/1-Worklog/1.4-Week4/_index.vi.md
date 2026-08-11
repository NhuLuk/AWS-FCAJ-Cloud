---
title: "Worklog Tuần 4"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4

- Tìm hiểu dịch vụ AWS Lambda và mô hình xử lý backend theo kiến trúc Serverless.
- Tìm hiểu cấu trúc Lambda Function, Event, Handler và Response.
- Thực hành tạo và kiểm thử các Lambda Function trên AWS.
- Tìm hiểu cách sử dụng AWS IAM Role để cấp quyền cho Lambda truy cập Amazon DynamoDB.
- Thực hành xây dựng các chức năng backend cơ bản phục vụ xử lý đơn hàng của hệ thống CloudMenu.

**Thời gian:** 13/07/2026 - 17/07/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Tìm hiểu **AWS Lambda** <br> + Khái niệm Function as a Service (FaaS) <br> + Tìm hiểu cách Lambda hoạt động trong kiến trúc Serverless <br> + Tìm hiểu Runtime, Function, Event và Handler | 13/07/2026 | 13/07/2026 | [https://aws.amazon.com/lambda/](https://aws.amazon.com/lambda/) |
| 2 | - Thực hành với **AWS Lambda** <br> + Tạo Lambda Function <br> + Sử dụng Python làm Runtime <br> + Tìm hiểu cấu trúc `lambda_handler` <br> + Tạo Test Event và kiểm tra kết quả thực thi Function | 14/07/2026 | 14/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) |
| 3 | - Tìm hiểu cách Lambda truy cập **Amazon DynamoDB** <br> + Sử dụng AWS SDK for Python (**Boto3**) <br> + Thực hiện thao tác đọc và ghi dữ liệu DynamoDB từ Lambda <br> + Kiểm tra dữ liệu sau khi Lambda thực thi | 15/07/2026 | 15/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html) |
| 4 | - Tìm hiểu **AWS IAM Role** cho Lambda <br> + Tìm hiểu Execution Role và IAM Policy <br> + Cấp quyền cần thiết để Lambda truy cập DynamoDB <br> + Kiểm tra quyền truy cập giữa Lambda và bảng `CloudMenuOrders` | 16/07/2026 | 16/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) |
| 5 | - Xây dựng các Lambda Function phục vụ hệ thống **CloudMenu** <br> + Xây dựng `createOrder` để tạo đơn hàng <br> + Xây dựng `getOrders` để lấy danh sách đơn hàng <br> + Xây dựng `updateOrderStatus` để cập nhật trạng thái đơn <br> + Kiểm thử việc đọc và ghi dữ liệu với bảng `CloudMenuOrders` | 17/07/2026 | 17/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/](https://docs.aws.amazon.com/lambda/latest/dg/) |

---

### Thành tựu Tuần 4

- Hiểu được vai trò của AWS Lambda trong việc xây dựng backend theo kiến trúc Serverless.
- Nắm được các thành phần cơ bản của Lambda Function như Runtime, Event, Handler và Response.
- Thực hành tạo, cấu hình và kiểm thử Lambda Function trên AWS.
- Hiểu được cách sử dụng Boto3 để Lambda đọc và ghi dữ liệu trên Amazon DynamoDB.
- Nắm được vai trò của IAM Execution Role và Policy trong việc cấp quyền cho Lambda truy cập DynamoDB.
- Xây dựng được các Lambda Function `createOrder`, `getOrders` và `updateOrderStatus` phục vụ xử lý đơn hàng của CloudMenu.
- Chuẩn bị backend để kết nối với Amazon API Gateway trong các tuần tiếp theo.

---