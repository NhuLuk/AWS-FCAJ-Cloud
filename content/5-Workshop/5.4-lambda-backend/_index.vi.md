---
title: "Kiểm thử Lambda Function"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4.4. </b>"
---

## 5.4.4. Kiểm thử Lambda Function

Sau khi triển khai các Lambda Function, cần kiểm tra hoạt động của từng Function trước khi kết nối hoàn chỉnh với Amazon API Gateway.

AWS Lambda cung cấp chức năng **Test** cho phép tạo Event mẫu và thực thi Function trực tiếp trên AWS Management Console.

### Kiểm thử createOrder

Tạo một Test Event có cấu trúc tương tự request từ API Gateway.

Sau khi chạy Test, kiểm tra:

- Function thực thi thành công.
- Response trả về status `200`.
- Một Item mới được tạo trong bảng `CloudMenuOrders`.
- Trạng thái ban đầu của đơn là `PENDING`.

### Kiểm thử getOrders

Chạy Function `getOrders` và kiểm tra Response.

Function cần trả về danh sách các Item hiện có trong bảng `CloudMenuOrders`.

### Kiểm thử updateOrderStatus

Tạo Event gồm:

- `orderId` trong `pathParameters`.
- Trạng thái mới trong request body.

Kiểm tra quá trình chuyển trạng thái:

**PENDING → PREPARING → COMPLETED**

Sau khi Function thực thi, kiểm tra Item tương ứng trong DynamoDB để xác nhận `status`, `updatedAt` và `completedAt` đã được cập nhật đúng.

<!-- HÌNH 5.4.4
Có thể chèn screenshot Lambda Test Result nếu nhóm có.
Không bắt buộc nếu phần 5.8 sẽ kiểm thử end-to-end bằng giao diện thật.
-->

Sau khi các Lambda Function hoạt động đúng, backend đã sẵn sàng để được tích hợp với Amazon API Gateway trong phần tiếp theo.