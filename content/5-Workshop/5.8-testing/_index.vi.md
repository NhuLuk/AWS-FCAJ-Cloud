---
title: "Kiểm thử hệ thống"
date: 2026-06-22
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---

## 5.8. Kiểm thử hệ thống

Sau khi hoàn thành việc triển khai frontend và backend của CloudMenu, hệ thống được kiểm thử trực tiếp thông qua các giao diện người dùng để xác nhận các thành phần có thể hoạt động cùng nhau trong luồng sử dụng thực tế.

Việc kiểm thử các API riêng lẻ đã được thực hiện tại phần **5.5.4. Kiểm thử API**. Vì vậy, phần này tập trung vào kiểm thử chức năng trên giao diện và luồng hoạt động End-to-End của CloudMenu.

Các giao diện chính được kiểm thử gồm:

- Giao diện đặt món của khách hàng.
- Giao diện theo dõi trạng thái đơn hàng.
- Giao diện quản lý đơn hàng của bếp.
- Restaurant Dashboard.

Luồng kiểm thử tổng thể:

```text
Customer
   |
   v
View Menu & Create Order
   |
   v
Order Tracking
   |
   v
Kitchen / Restaurant Orders
   |
   v
Update Order Status
   |
   v
Restaurant Dashboard
```

---

### 5.8.1. Kiểm thử giao diện đặt món

Đầu tiên, truy cập giao diện Customer của CloudMenu để kiểm tra quá trình xem thực đơn và tạo đơn hàng.

Giao diện hiển thị danh sách các món ăn với các thông tin và chức năng chính như:

- Hình ảnh món ăn.
- Tên món.
- Giá.
- Danh mục món ăn.
- Tìm kiếm và lọc món ăn.
- Thêm món vào giỏ hàng.

![CloudMenu Customer Menu](/images/5-Workshop/5.8/customer-menu.png)

Khách hàng lựa chọn món ăn và thêm món vào giỏ hàng. Sau khi kiểm tra danh sách món đã chọn và tổng giá trị đơn hàng, khách hàng thực hiện gửi đơn.

Khi đơn hàng được gửi, frontend chuyển thông tin đơn hàng đến backend thông qua Amazon API Gateway.

Luồng xử lý:

```text
Customer Interface
        |
        v
POST /order
        |
        v
Amazon API Gateway
        |
        v
createOrder Lambda
        |
        v
Amazon DynamoDB
```

Sau khi đơn hàng được tạo thành công, thông tin đơn hàng được lưu trong bảng `CloudMenuOrders` với trạng thái ban đầu:

```text
PENDING
```

**Kết quả:** Giao diện Customer có thể hiển thị thực đơn, cho phép khách hàng lựa chọn món và tạo đơn hàng thành công.

---

### 5.8.2. Kiểm thử theo dõi trạng thái đơn hàng

Sau khi tạo đơn hàng thành công, khách hàng có thể truy cập giao diện theo dõi đơn hàng để kiểm tra trạng thái xử lý.

![CloudMenu Order Tracking](/images/5-Workshop/5.8/order-tracking.png)

Giao diện theo dõi hiển thị các thông tin của đơn hàng như:

- Mã đơn hàng.
- Số bàn.
- Danh sách món ăn.
- Tổng giá trị đơn hàng.
- Thời gian đặt.
- Trạng thái hiện tại của đơn hàng.

Trong quá trình xử lý, trạng thái đơn hàng thay đổi theo luồng:

```text
PENDING
   |
   v
PREPARING
   |
   v
COMPLETED
```

Trong đó:

- `PENDING`: Đơn hàng đã được tạo và đang chờ bếp tiếp nhận.
- `PREPARING`: Đơn hàng đã được tiếp nhận và đang được chế biến.
- `COMPLETED`: Đơn hàng đã hoàn thành.

Khi phía nhà hàng cập nhật trạng thái, dữ liệu mới được lưu vào Amazon DynamoDB và trạng thái tương ứng được hiển thị trên giao diện theo dõi đơn hàng.

**Kết quả:** Khách hàng có thể theo dõi trạng thái xử lý của đơn hàng sau khi thực hiện đặt món.

---

### 5.8.3. Kiểm thử giao diện quản lý đơn hàng của bếp

Tiếp theo, truy cập giao diện quản lý đơn hàng dành cho nhân viên bếp.

Các đơn hàng được tạo từ Customer Interface sẽ xuất hiện trên giao diện này để nhân viên theo dõi và xử lý.

![CloudMenu Restaurant Orders](/images/5-Workshop/5.8/restaurant-orders.png)

Mỗi đơn hàng hiển thị các thông tin chính như:

- Mã đơn hàng.
- Số bàn.
- Thời gian tạo đơn.
- Danh sách món ăn.
- Tổng giá trị đơn hàng.
- Trạng thái hiện tại.

Khi một đơn hàng mới được tạo, trạng thái ban đầu là:

```text
PENDING
```

Khi nhân viên bếp bắt đầu xử lý đơn hàng, trạng thái được cập nhật thành:

```text
PREPARING
```

Sau khi hoàn thành quá trình chế biến, trạng thái được cập nhật thành:

```text
COMPLETED
```

Khi trạng thái được thay đổi trên giao diện, frontend gửi request:

```text
PUT /orders/{orderId}
```

đến backend.

Luồng xử lý:

```text
Kitchen Interface
       |
       v
PUT /orders/{orderId}
       |
       v
Amazon API Gateway
       |
       v
updateOrderStatus Lambda
       |
       v
Amazon DynamoDB
```

Sau khi DynamoDB được cập nhật, trạng thái mới của đơn hàng được sử dụng bởi các giao diện liên quan trong hệ thống.

**Kết quả:** Giao diện quản lý đơn hàng có thể hiển thị các đơn hàng được tạo từ Customer Interface và cho phép nhân viên cập nhật trạng thái xử lý của đơn hàng.

---

### 5.8.4. Kiểm thử Restaurant Dashboard

Cuối cùng, truy cập Restaurant Dashboard để kiểm tra khả năng tổng hợp và hiển thị dữ liệu hoạt động của CloudMenu.

![CloudMenu Restaurant Dashboard](/images/5-Workshop/5.8/restaurant-dashboard.png)

Dashboard hiển thị các thông tin tổng quan như:

- Tổng số đơn hàng.
- Tổng doanh thu.
- Số đơn đang chờ.
- Số đơn đang chế biến.
- Số đơn đã hoàn thành.
- Doanh thu theo bàn.
- Các món ăn được gọi nhiều nhất.

Dữ liệu trên Dashboard được tổng hợp từ các đơn hàng hiện có trong hệ thống.

Khi khách hàng tạo thêm đơn hàng hoặc nhân viên cập nhật trạng thái của đơn hàng, dữ liệu mới được lưu trong DynamoDB và được sử dụng để cập nhật các thông tin thống kê trên Dashboard.

**Kết quả:** Restaurant Dashboard có thể hiển thị các thông tin tổng hợp từ dữ liệu đơn hàng và hỗ trợ phía nhà hàng theo dõi hoạt động của hệ thống.

---

### 5.8.5. Kết quả kiểm thử

Sau khi kiểm thử các giao diện chính của CloudMenu, kết quả thu được như sau:

| Chức năng | Kết quả |
| :--- | :--- |
| Hiển thị thực đơn | Thành công |
| Tìm kiếm và lựa chọn món ăn | Thành công |
| Thêm món vào giỏ hàng | Thành công |
| Tạo đơn hàng | Thành công |
| Theo dõi trạng thái đơn hàng | Thành công |
| Hiển thị đơn hàng trên giao diện bếp | Thành công |
| Cập nhật trạng thái đơn hàng | Thành công |
| Hiển thị dữ liệu thống kê | Thành công |

Luồng End-to-End của CloudMenu được kiểm tra theo quy trình:

```text
Customer
   |
   v
View Menu
   |
   v
Create Order
   |
   v
PENDING
   |
   v
Kitchen receives order
   |
   v
PREPARING
   |
   v
COMPLETED
   |
   v
Restaurant Dashboard
```

Kết quả kiểm thử cho thấy các giao diện chính của CloudMenu có thể phối hợp với backend để thực hiện luồng hoạt động từ khi khách hàng xem thực đơn và tạo đơn hàng, theo dõi trạng thái, đến khi phía nhà hàng tiếp nhận, xử lý và hoàn thành đơn hàng.

Các API nền tảng đã được kiểm thử riêng tại phần **5.5.4**, trong khi kết quả tại phần này xác nhận các API có thể được sử dụng thành công trong luồng hoạt động thực tế của ứng dụng CloudMenu.