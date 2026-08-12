---

title : "Cơ sở dữ liệu"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.2.4 </b> "
------------------------

## 5.2.4. Cơ sở dữ liệu

### Tổng quan

CloudMenu sử dụng **Amazon DynamoDB** làm cơ sở dữ liệu chính để lưu trữ và quản lý dữ liệu đơn hàng của hệ thống. DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được AWS quản lý hoàn toàn, phù hợp với kiến trúc serverless đang được sử dụng cho backend của CloudMenu.

Trong kiến trúc hiện tại, frontend không truy cập trực tiếp vào cơ sở dữ liệu. Các request từ Customer, Kitchen hoặc Manager được gửi đến Amazon API Gateway, sau đó được chuyển đến AWS Lambda để xử lý. Lambda thực hiện các thao tác đọc hoặc ghi dữ liệu trên DynamoDB và trả kết quả về frontend thông qua API Gateway.

Luồng truy cập dữ liệu chính được mô tả như sau:

**Customer/Kitchen/Manager → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Việc sử dụng DynamoDB giúp CloudMenu không cần triển khai và duy trì một database server riêng. So với phương án sử dụng cơ sở dữ liệu quan hệ được triển khai trên Amazon RDS, kiến trúc hiện tại không yêu cầu CloudMenu phải cấu hình database instance, quản lý kết nối cơ sở dữ liệu hoặc xây dựng thêm các thành phần mạng chỉ để cung cấp quyền truy cập đến database.

### Thiết kế dữ liệu CloudMenuOrders

Trong phạm vi hiện tại của Workshop, dữ liệu đơn hàng được lưu trong bảng:

**`CloudMenuOrders`**

Mỗi đơn hàng được xác định bởi một `orderId` duy nhất. `orderId` được sử dụng làm **Partition Key**, cho phép hệ thống phân biệt và truy xuất từng đơn hàng.

Một item trong bảng có thể bao gồm các thuộc tính chính sau:

| Thuộc tính      | Mô tả                                                                |
| :-------------- | :------------------------------------------------------------------- |
| **orderId**     | Mã định danh duy nhất của đơn hàng và được sử dụng làm Partition Key |
| **tableNumber** | Số bàn thực hiện gọi món                                             |
| **items**       | Danh sách các món và số lượng tương ứng trong đơn                    |
| **totalAmount** | Tổng giá trị của đơn hàng                                            |
| **status**      | Trạng thái xử lý hiện tại của đơn                                    |
| **createdAt**   | Thời điểm đơn hàng được tạo                                          |
| **updatedAt**   | Thời điểm đơn hàng được cập nhật gần nhất                            |

Cấu trúc này đáp ứng các nghiệp vụ chính của CloudMenu trong giai đoạn hiện tại, bao gồm tạo đơn mới, lấy danh sách đơn và cập nhật trạng thái xử lý.

Khi Customer hoàn tất lựa chọn món và gửi đơn, frontend gửi dữ liệu đến API Gateway. Lambda tiếp nhận request, xử lý dữ liệu cần thiết và tạo item tương ứng trong bảng `CloudMenuOrders`.

Khi Kitchen cần hiển thị các đơn đang chờ hoặc đang được chế biến, backend đọc dữ liệu từ DynamoDB và trả kết quả về giao diện Kitchen. Khi nhân viên bếp thay đổi trạng thái đơn, Lambda cập nhật thuộc tính `status` và thời điểm cập nhật của đơn tương ứng.

Manager Dashboard cũng sử dụng dữ liệu đơn hàng được backend lấy từ DynamoDB để tổng hợp các thông tin như số lượng đơn, doanh thu, trạng thái đơn hàng, dữ liệu theo bàn và các món được gọi nhiều.

### Lý do lựa chọn Amazon DynamoDB

DynamoDB được lựa chọn vì phù hợp với cách CloudMenu xây dựng backend theo kiến trúc serverless.

Một số lý do chính gồm:

* Không cần triển khai hoặc quản lý database server riêng.
* Tích hợp trực tiếp với AWS Lambda thông qua AWS SDK.
* Phù hợp với dữ liệu đơn hàng có cấu trúc tương đối rõ ràng trong phạm vi hiện tại.
* Có khả năng đáp ứng lưu lượng thay đổi mà không yêu cầu quản lý database instance cố định.
* Giảm số lượng thành phần hạ tầng cần cấu hình và vận hành.
* Phù hợp với môi trường development/testing và mục tiêu tối ưu chi phí của Workshop.

Việc sử dụng DynamoDB cũng giúp kiến trúc backend nhất quán với mô hình serverless của CloudMenu, trong đó API Gateway, Lambda và DynamoDB có thể hoạt động mà không cần duy trì một backend server hoặc database server truyền thống chạy liên tục.

### Luồng đọc và ghi dữ liệu

Các thao tác với dữ liệu CloudMenu được thực hiện thông qua Lambda Function thay vì từ frontend.

Đối với quá trình tạo đơn hàng, luồng xử lý chính là:

**Customer → API Gateway → Lambda → Put item vào `CloudMenuOrders`**

Đối với quá trình lấy dữ liệu:

**Kitchen/Manager → API Gateway → Lambda → Read `CloudMenuOrders` → Trả dữ liệu về frontend**

Đối với việc cập nhật trạng thái:

**Kitchen → API Gateway → Lambda → Update order trong `CloudMenuOrders`**

Cách tổ chức này giúp tách giao diện người dùng khỏi lớp lưu trữ dữ liệu. Nếu logic xử lý dữ liệu cần thay đổi, phần lớn thay đổi có thể được thực hiện tại backend mà không yêu cầu frontend truy cập trực tiếp hoặc biết thông tin cấu hình của DynamoDB.

### Bảo mật dữ liệu

CloudMenu không cung cấp quyền truy cập trực tiếp từ trình duyệt đến Amazon DynamoDB.

Frontend chỉ gửi request đến các API endpoint được cung cấp bởi Amazon API Gateway. Các Lambda Function chịu trách nhiệm thực hiện thao tác cần thiết với dữ liệu sau khi nhận request từ API Gateway.

Quyền truy cập từ Lambda đến DynamoDB được kiểm soát thông qua **AWS IAM Role**. Lambda chỉ được cấp các quyền cần thiết để thực hiện nghiệp vụ của hệ thống trên bảng `CloudMenuOrders`.

Kiến trúc truy cập có thể tóm tắt như sau:

**Frontend → API Gateway → Lambda → IAM-authorized access → DynamoDB**

Cách tiếp cận này giúp tránh việc lưu AWS Access Key hoặc Secret Access Key trong JavaScript phía frontend, đồng thời hỗ trợ áp dụng nguyên tắc **Least Privilege** đối với các tài nguyên AWS.

Ngoài ra, hoạt động thực thi của Lambda được ghi nhận thông qua Amazon CloudWatch Logs. Điều này hỗ trợ quá trình kiểm tra request, xác định lỗi backend và troubleshooting khi thao tác với DynamoDB không thành công.

### Phạm vi của thiết kế hiện tại

Trong phiên bản hiện tại, `CloudMenuOrders` tập trung vào dữ liệu phục vụ quy trình gọi món. Đây là thiết kế phù hợp với phạm vi Workshop nhưng chưa phải mô hình dữ liệu hoàn chỉnh cho một hệ thống quản lý nhà hàng ở quy mô production.

Ví dụ, thông tin menu hiện chưa nhất thiết phải được tách thành một hệ thống quản lý dữ liệu động hoàn chỉnh và CloudMenu chưa triển khai mô hình multi-tenant cho nhiều nhà hàng.

Việc giới hạn phạm vi giúp project tập trung vào luồng nghiệp vụ chính:

**Khách hàng tạo đơn → Backend lưu dữ liệu → Bếp xử lý đơn → Quản lý theo dõi dữ liệu**

### Định hướng mở rộng cơ sở dữ liệu

Khi CloudMenu được phát triển ở quy mô lớn hơn, mô hình dữ liệu có thể được mở rộng để hỗ trợ thêm các entity và nghiệp vụ mới, chẳng hạn:

* **Menu Items:** lưu thông tin món ăn, giá, danh mục, hình ảnh và trạng thái còn/hết món.
* **Tables:** quản lý thông tin bàn và QR Code tương ứng.
* **Orders:** mở rộng thông tin đơn hàng và các thuộc tính phục vụ quá trình xử lý.
* **Restaurants:** hỗ trợ nhiều nhà hàng nếu CloudMenu được phát triển theo mô hình multi-tenant.
* **Order History:** lưu dữ liệu lịch sử phục vụ báo cáo và phân tích hoạt động.

Tùy theo yêu cầu trong tương lai, hệ thống cũng có thể nghiên cứu sử dụng **DynamoDB Streams** cho các tác vụ xử lý theo sự kiện, bổ sung cơ chế caching khi lưu lượng đọc tăng hoặc tích hợp các dịch vụ phân tích dữ liệu của AWS để xây dựng báo cáo chuyên sâu hơn.

Việc mở rộng cần dựa trên access pattern thực tế thay vì chỉ bổ sung thêm bảng hoặc thuộc tính. Điều này đặc biệt quan trọng với DynamoDB vì thiết kế dữ liệu NoSQL nên được xây dựng dựa trên cách ứng dụng truy cập dữ liệu.

Trong phạm vi CloudMenu hiện tại, bảng `CloudMenuOrders` đã đáp ứng các nghiệp vụ cốt lõi và cung cấp nền tảng dữ liệu cho Customer Interface, Kitchen Interface và Manager Dashboard.
