---

title : "Tổng quan và kiến trúc hệ thống"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
-----------------------

## 5.1. Tổng quan và kiến trúc hệ thống

### Tổng quan

CloudMenu là hệ thống gọi món tại bàn được xây dựng theo mô hình ứng dụng web và triển khai trên nền tảng AWS. Hệ thống hướng đến việc số hóa quy trình gọi món trong nhà hàng hoặc quán ăn, giúp khách hàng có thể chủ động xem thực đơn, lựa chọn món và gửi đơn trực tiếp bằng điện thoại thông qua mã QR được gán cho từng bàn.

Thay vì sử dụng menu giấy và chờ nhân viên phục vụ đến ghi nhận yêu cầu, mỗi bàn được cung cấp một mã QR chứa thông tin nhận diện bàn. Khi khách hàng quét mã QR, trình duyệt mở giao diện Customer của CloudMenu và truyền thông tin bàn vào hệ thống. Khách hàng có thể xem danh sách món ăn, lựa chọn số lượng, quản lý giỏ hàng, gửi đơn và theo dõi trạng thái xử lý của đơn hàng.

CloudMenu được thiết kế với ba nhóm giao diện chính tương ứng với các vai trò khác nhau trong quy trình vận hành:

* **Customer Interface:** giao diện dành cho khách hàng, hỗ trợ truy cập menu theo bàn, lựa chọn món, quản lý giỏ hàng, gửi đơn và theo dõi trạng thái đơn hàng.
* **Kitchen Interface:** giao diện dành cho nhân viên bếp, cho phép theo dõi các đơn hàng được gửi từ khách hàng và cập nhật trạng thái xử lý trong quá trình chế biến.
* **Manager Dashboard:** giao diện dành cho quản lý, cung cấp thông tin tổng hợp về đơn hàng, doanh thu, trạng thái xử lý, hoạt động theo bàn và các dữ liệu thống kê cần thiết.

Việc tách hệ thống thành các giao diện theo từng vai trò giúp mỗi nhóm người dùng tập trung vào đúng chức năng cần thiết, đồng thời vẫn sử dụng chung một backend và nguồn dữ liệu đơn hàng.

### Kiến trúc hệ thống

CloudMenu được triển khai theo kiến trúc serverless trên AWS nhằm hạn chế việc quản lý máy chủ truyền thống và đơn giản hóa quá trình vận hành trong phạm vi của Workshop.

Kiến trúc tổng thể của hệ thống được minh họa trong sơ đồ sau:

![Sơ đồ kiến trúc AWS CloudMenu](/images/AWS_CloudMenu.png)

Kiến trúc CloudMenu có thể được chia thành ba lớp chính: lớp phân phối frontend, lớp xử lý backend và lớp lưu trữ dữ liệu.

**Lớp frontend** sử dụng Amazon S3 để lưu trữ các file tĩnh của ứng dụng như HTML, CSS, JavaScript và các tài nguyên giao diện. Amazon CloudFront được sử dụng để phân phối nội dung từ S3 đến trình duyệt của người dùng thông qua CDN. Customer, Kitchen và Manager đều truy cập các giao diện tương ứng thông qua frontend được phân phối bởi CloudFront.

**Lớp backend** sử dụng Amazon API Gateway làm điểm tiếp nhận các HTTP request từ frontend. API Gateway định tuyến request đến AWS Lambda để thực hiện logic nghiệp vụ. Các Lambda Function chịu trách nhiệm xử lý những chức năng chính như tạo đơn hàng, lấy thông tin đơn hàng và cập nhật trạng thái đơn.

**Lớp dữ liệu** sử dụng Amazon DynamoDB để lưu trữ dữ liệu đơn hàng của CloudMenu. Sau khi Lambda xử lý request, dữ liệu có thể được ghi vào hoặc đọc từ bảng `CloudMenuOrders`, sau đó kết quả được trả về frontend thông qua API Gateway.

Luồng xử lý chính của hệ thống có thể được mô tả như sau:

**Customer/Kitchen/Manager → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Đối với việc phân phối giao diện, luồng truy cập được thực hiện thông qua:

**User → Amazon CloudFront → Amazon S3**

AWS IAM được sử dụng để kiểm soát quyền truy cập giữa các thành phần backend. Lambda được gán IAM Role với các quyền cần thiết để thao tác với bảng DynamoDB thay vì lưu AWS Access Key hoặc Secret Access Key trực tiếp trong source code. Việc giới hạn quyền theo nhu cầu sử dụng giúp hệ thống tuân theo nguyên tắc **Least Privilege**.

Ngoài các thành phần xử lý chính, Amazon CloudWatch hỗ trợ thu thập log và metric của backend để phục vụ quá trình kiểm thử, theo dõi hoạt động và xác định nguyên nhân khi Lambda phát sinh lỗi.

### Đặc điểm của kiến trúc

Việc sử dụng các dịch vụ managed và serverless giúp CloudMenu không yêu cầu duy trì một máy chủ backend hoạt động liên tục. API Gateway tiếp nhận request, Lambda thực thi logic khi có yêu cầu và DynamoDB đảm nhiệm việc lưu trữ dữ liệu. Mô hình này phù hợp với phạm vi hiện tại của CloudMenu, trong đó lưu lượng sử dụng có thể thay đổi theo thời điểm và hệ thống đang được triển khai ở quy mô thử nghiệm.

Kiến trúc cũng giúp phân tách tương đối rõ trách nhiệm của từng thành phần: S3 và CloudFront phụ trách frontend, API Gateway cung cấp lớp API, Lambda xử lý nghiệp vụ, DynamoDB lưu trữ dữ liệu, IAM kiểm soát quyền và CloudWatch hỗ trợ monitoring.

Trong phạm vi hiện tại, CloudMenu tập trung vào các chức năng cốt lõi của quy trình gọi món và chưa triển khai toàn bộ các thành phần thường có trong một hệ thống production. Trong các giai đoạn tiếp theo, kiến trúc có thể được mở rộng với Amazon Cognito để cung cấp authentication và authorization cho nhân viên, AWS WAF để tăng cường bảo vệ các endpoint public, CI/CD để tự động hóa quá trình triển khai và các cơ chế monitoring, alerting nâng cao hơn.

Kiến trúc này tạo nền tảng để các phần tiếp theo của Workshop triển khai lần lượt hạ tầng AWS, backend serverless, cơ sở dữ liệu, API, frontend hosting, kiểm thử và monitoring của hệ thống CloudMenu.
