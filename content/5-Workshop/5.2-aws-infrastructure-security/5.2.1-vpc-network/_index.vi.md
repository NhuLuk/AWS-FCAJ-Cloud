---
title : "VPC và mạng"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

## 5.2.1 VPC và mạng

CloudMenu được xây dựng theo kiến trúc serverless, trong đó phần lớn các thành phần chính sử dụng các dịch vụ được AWS quản lý. Vì vậy, thiết kế mạng của hệ thống không yêu cầu toàn bộ tài nguyên phải được đặt bên trong Amazon VPC.

Thay vì áp dụng mô hình 3-tier truyền thống với Application Load Balancer, Amazon ECS và cơ sở dữ liệu Amazon RDS nằm trong các subnet riêng, CloudMenu sử dụng Amazon S3 và Amazon CloudFront cho frontend, đồng thời sử dụng Amazon API Gateway, AWS Lambda và Amazon DynamoDB cho backend.

Với kiến trúc này, AWS chịu trách nhiệm quản lý phần lớn network infrastructure của các dịch vụ serverless. Do đó, S3, CloudFront, API Gateway và DynamoDB không cần được triển khai trực tiếp vào public hoặc private subnet của VPC.

Các thành phần networking có thể được sử dụng trong CloudMenu khi hệ thống có nhu cầu mở rộng gồm:

| Thành phần | Vai trò |
| :--- | :--- |
| **Amazon VPC** | Cung cấp môi trường mạng riêng cho các AWS resources cần network isolation. Trong kiến trúc hiện tại, VPC chỉ thực sự cần thiết khi Lambda phải truy cập private resources hoặc hệ thống bổ sung các thành phần chạy bên trong VPC. |
| **Internet Gateway** | Cho phép các tài nguyên phù hợp trong public subnet giao tiếp với Internet. Thành phần này không phải đường truy cập trực tiếp từ người dùng đến Lambda hoặc DynamoDB. |
| **NAT Gateway** | Cho phép tài nguyên trong private subnet tạo outbound connection ra Internet mà không cần gán public IP trực tiếp. Có thể cần thiết nếu Lambda được đưa vào VPC và phải truy cập external services. |
| **VPC Endpoint** | Tạo đường kết nối từ VPC đến các AWS services được hỗ trợ mà không nhất thiết phải đi qua Internet công cộng. |
| **Security Group** | Kiểm soát network traffic của các resource có network interface trong VPC, ví dụ Lambda được kết nối với VPC hoặc các private resources được bổ sung sau này. |

### Thiết kế VPC và Subnet

Amazon VPC (Virtual Private Cloud) cho phép xây dựng một môi trường mạng logic riêng trên AWS. Trong VPC, có thể quản lý CIDR, subnet, route table, gateway và các cơ chế kiểm soát network traffic.

Tuy nhiên, việc sử dụng AWS không đồng nghĩa với việc mọi resource đều phải nằm trong VPC.

Trong CloudMenu, frontend được lưu trữ trên Amazon S3 và được Amazon CloudFront phân phối đến người dùng. Hai dịch vụ này được AWS quản lý và không cần được đặt trong subnet của CloudMenu.

Tương tự, Amazon API Gateway cung cấp public API endpoint cho frontend, còn DynamoDB là managed NoSQL database service. Hai thành phần này cũng không yêu cầu CloudMenu phải tạo subnet riêng để có thể hoạt động.

AWS Lambda trong kiến trúc hiện tại có thể hoạt động mà không cần kết nối vào VPC. Lambda có thể thực hiện các thao tác với DynamoDB thông qua AWS SDK và IAM permissions mà không yêu cầu CloudMenu phải triển khai NAT Gateway hoặc một hệ thống public/private subnet riêng.

Cách triển khai này phù hợp với phạm vi development và testing của CloudMenu vì giảm số lượng network resources cần cấu hình và quản lý.

Nếu trong tương lai Lambda cần truy cập database hoặc service chỉ tồn tại trong private network, kiến trúc có thể được mở rộng với các subnet như sau:

| Nhóm subnet | Số lượng | Mục đích |
| :--- | :---: | :--- |
| **Public Subnet** | 2 | Có thể bố trí một subnet tại mỗi Availability Zone cho các thành phần cần public routing hoặc các network resources như NAT Gateway. |
| **Private Subnet** | 2 | Có thể bố trí một subnet tại mỗi Availability Zone cho Lambda hoặc backend resources cần được cô lập khỏi inbound traffic trực tiếp từ Internet. |

Đây là kiến trúc mở rộng dự kiến, không có nghĩa CloudMenu hiện tại bắt buộc phải tạo bốn subnet trên.

Chỉ nên bổ sung public/private subnet khi có resource thực sự cần được triển khai trong VPC. Cách tiếp cận này giúp tránh tăng độ phức tạp của kiến trúc khi hệ thống serverless hiện tại chưa có nhu cầu network isolation ở cấp subnet.

### Luồng mạng của CloudMenu

CloudMenu có hai luồng network chính cần phân biệt: luồng phân phối frontend và luồng xử lý dữ liệu.

Đối với frontend:

**Customer / Kitchen / Manager → Amazon CloudFront → Amazon S3**

Người dùng truy cập giao diện thông qua CloudFront. CloudFront phân phối các HTML, CSS, JavaScript, hình ảnh và static assets được lưu trong S3.

Đối với API và dữ liệu:

**Customer / Kitchen / Manager → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Trong luồng này:

- **Amazon CloudFront** cung cấp điểm phân phối frontend đến trình duyệt.
- **Amazon S3** lưu trữ các frontend files và static assets.
- **Amazon API Gateway** tiếp nhận các HTTP request từ frontend.
- **AWS Lambda** thực hiện business logic cho các nghiệp vụ của CloudMenu.
- **Amazon DynamoDB** lưu trữ dữ liệu đơn hàng và chỉ được truy cập thông qua backend.

Thiết kế này giúp tách phần hiển thị frontend khỏi phần xử lý dữ liệu.

Đặc biệt, trình duyệt của Customer, Kitchen hoặc Manager không cần được cấp quyền truy cập trực tiếp vào bảng `CloudMenuOrders`. Frontend chỉ gửi request đến API Gateway, sau đó Lambda thực hiện các thao tác cần thiết với DynamoDB theo IAM permissions được cấp.

### Security Group

Security Group hoạt động như một stateful virtual firewall dành cho các AWS resources có network interface trong VPC.

Các inbound và outbound rules xác định loại traffic được phép đi qua Security Group.

Trong kiến trúc CloudMenu hiện tại, Security Group không được yêu cầu cho Amazon S3, Amazon CloudFront, Amazon API Gateway hoặc Amazon DynamoDB.

Nếu Lambda không được kết nối với VPC thì CloudMenu cũng không cần tạo Security Group chỉ để Lambda gọi DynamoDB.

Security Group trở nên cần thiết khi hệ thống được mở rộng và Lambda hoặc các backend resources được đưa vào VPC.

Một số Security Group có thể xuất hiện trong kiến trúc mở rộng gồm:

| Security Group | Mục đích |
| :--- | :--- |
| **Lambda Security Group** | Được gắn với Lambda khi function kết nối vào VPC và kiểm soát network traffic cần thiết đến các private resources. |
| **Private Resource Security Group** | Có thể sử dụng cho database hoặc service chạy trong VPC và giới hạn traffic chỉ từ Lambda hoặc các nguồn được phép. |
| **VPC Endpoint Security Group** | Có thể được sử dụng với Interface VPC Endpoint để kiểm soát HTTPS traffic từ các resource trong VPC đến endpoint. |

Việc bổ sung Security Group cần dựa trên network flow thực tế thay vì tạo trước khi hệ thống có resource cần sử dụng.

### VPC Endpoint

VPC Endpoint cung cấp khả năng kết nối giữa các resources trong VPC và những AWS services được hỗ trợ mà không yêu cầu traffic phải phụ thuộc hoàn toàn vào đường truyền qua Internet công cộng.

Đối với CloudMenu hiện tại, Lambda hoạt động ngoài VPC nên VPC Endpoint không phải thành phần bắt buộc.

Nếu Lambda được đưa vào private subnet trong tương lai, VPC Endpoint có thể được xem xét nhằm cung cấp private connectivity đến các AWS services cần thiết và trong một số trường hợp giảm sự phụ thuộc vào NAT Gateway.

Có hai nhóm endpoint đáng chú ý:

- **Gateway Endpoint:** được hỗ trợ cho một số dịch vụ như Amazon S3 và được liên kết với route table.
- **Interface Endpoint:** sử dụng network interface trong VPC để cung cấp private connectivity đến các AWS services hỗ trợ AWS PrivateLink.

Một số endpoint có thể được xem xét khi kiến trúc CloudMenu phát triển gồm:

| Resource | Loại | Ý nghĩa vận hành |
| :--- | :--- | :--- |
| **S3 VPC Endpoint** | Gateway | Cho phép các resource phù hợp trong VPC truy cập Amazon S3 thông qua endpoint thay vì phụ thuộc vào Internet path. |
| **CloudWatch Logs Endpoint** | Interface | Có thể hỗ trợ resource trong VPC kết nối đến CloudWatch Logs bằng private connectivity. |
| **ECR API Endpoint** | Interface | Có thể được sử dụng nếu backend trong tương lai triển khai container image và cần truy cập Amazon ECR API. |
| **ECR DKR Endpoint** | Interface | Hỗ trợ kết nối đến Docker Registry của Amazon ECR trong kiến trúc sử dụng container image. |

Hai endpoint liên quan đến ECR không thuộc yêu cầu của CloudMenu hiện tại. Chúng chỉ là khả năng mở rộng nếu backend sau này chuyển sang sử dụng container-based workload.

### NAT Gateway và Internet Access

NAT Gateway không phải thành phần bắt buộc trong kiến trúc CloudMenu hiện tại.

Nếu Lambda hoạt động ngoài VPC, không cần tạo NAT Gateway chỉ để function truy cập DynamoDB.

Trong kiến trúc mở rộng, nếu Lambda được đặt trong private subnet và đồng thời cần tạo outbound connection đến Internet, NAT Gateway có thể được triển khai trong public subnet để cung cấp đường outbound phù hợp.

Tuy nhiên, NAT Gateway làm tăng thêm tài nguyên cần quản lý và có thể phát sinh chi phí ngay cả trong những kiến trúc có lưu lượng thấp.

Vì vậy, CloudMenu chỉ nên bổ sung NAT Gateway khi xuất hiện yêu cầu thực tế thay vì triển khai sẵn trong môi trường development.

### Định hướng mở rộng Networking

Ở giai đoạn hiện tại, CloudMenu ưu tiên kiến trúc serverless đơn giản. Việc không đưa Lambda vào VPC khi chưa có private resource giúp giảm số lượng subnet, route table, gateway và Security Group cần quản lý.

Kiến trúc hiện tại có thể được duy trì theo luồng:

**CloudFront → S3**

và:

**API Gateway → Lambda → DynamoDB**

Khi CloudMenu phát triển và xuất hiện các yêu cầu mới như private database, internal services hoặc network isolation ở mức cao hơn, hệ thống có thể mở rộng sang mô hình VPC.

Khi đó, Lambda có thể được kết nối với private subnets, Security Group được sử dụng để giới hạn network traffic, còn VPC Endpoint hoặc NAT Gateway được lựa chọn dựa trên loại kết nối mà backend thực sự cần.

Cách tiếp cận này giúp CloudMenu tránh over-engineering trong giai đoạn thử nghiệm nhưng vẫn duy trì khả năng mở rộng kiến trúc mạng khi yêu cầu bảo mật, lưu lượng và quy mô hệ thống tăng lên.