---
title: "Bản đề xuất"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Đề xuất triển khai CloudMenu trên AWS

## 1. Tổng quan dự án

CloudMenu là hệ thống gọi món trực tuyến tại bàn, cho phép khách hàng sử dụng điện thoại quét QR Code được gán cho từng bàn để truy cập thực đơn, lựa chọn món và gửi đơn trực tiếp đến khu vực bếp.

Hệ thống gồm ba nhóm người dùng chính:

- Khách hàng: xem menu, tìm kiếm/lọc món, quản lý giỏ hàng, đặt món và theo dõi trạng thái đơn hàng.
- Nhân viên bếp: tiếp nhận đơn, xem thông tin đơn hàng và cập nhật trạng thái chế biến.
- Admin/Manager: theo dõi Dashboard thống kê về số lượng đơn, doanh thu, trạng thái đơn hàng, doanh thu theo bàn và các món được gọi nhiều nhất.

CloudMenu được đề xuất triển khai theo mô hình Serverless trên AWS, nhằm giảm chi phí vận hành, dễ mở rộng và phù hợp với đặc điểm lưu lượng truy cập không cố định của hệ thống.

## 2. Vấn đề và giải pháp

### Vấn đề hiện tại

Mô hình gọi món truyền thống có thể phát sinh một số hạn chế:

- Khách hàng phải chờ nhân viên đến nhận order.
- Dễ xảy ra sai sót khi ghi nhận món và số lượng.
- Nhân viên bếp khó theo dõi và cập nhật trạng thái đơn hàng theo thời gian thực.
- Quản lý khó tổng hợp doanh thu và thống kê tình hình kinh doanh.
- Hệ thống cần có khả năng đáp ứng lượng truy cập tăng cao vào các khung giờ cao điểm.

### Giải pháp

CloudMenu sử dụng QR Code cho từng bàn để khách hàng trực tiếp truy cập menu và gửi đơn. Các yêu cầu được xử lý thông qua kiến trúc Serverless:

QR Code → Frontend → API Gateway → Lambda → DynamoDB

Giải pháp giúp:

- Tự động xác định số bàn thông qua QR Code.
- Giảm thao tác thủ công và hạn chế sai sót khi đặt món.
- Tập trung dữ liệu đơn hàng trên DynamoDB.
- Tự động mở rộng khả năng xử lý thông qua Lambda.
- Cung cấp Dashboard giúp quản lý theo dõi hoạt động kinh doanh.

## 3. Kiến trúc giải pháp

![Sơ đồ kiến trúc AWS CloudMenu](/images/AWS_CloudMenu.png)

Kiến trúc đề xuất gồm các thành phần chính:

Frontend
- Amazon S3: lưu trữ các file HTML, CSS, JavaScript của CloudMenu.
- Amazon CloudFront: phân phối nội dung frontend thông qua mạng CDN, giúp giảm độ trễ truy cập.

Backend
- Amazon API Gateway: cung cấp RESTful API và tiếp nhận request từ frontend.
- AWS Lambda: xử lý nghiệp vụ như lấy menu, tạo đơn hàng, cập nhật trạng thái và truy vấn thống kê.
- Amazon DynamoDB: lưu trữ dữ liệu menu, đơn hàng, bàn và trạng thái đơn hàng.

Security: AWS IAM: quản lý quyền truy cập giữa các AWS Services và kiểm soát quyền quản trị hệ thống.

## 4. Timeline (8 tuần)

Dự án CloudMenu được thực hiện trong 8 tuần, từ giai đoạn tìm hiểu các dịch vụ AWS và kiến trúc Serverless đến xây dựng, triển khai, tích hợp và kiểm thử hệ thống hoàn chỉnh.

* **Tuần 1 (22/06 - 26/06) — AWS Cloud và Serverless cơ bản**

  * Làm quen với nền tảng AWS Cloud và các nhóm dịch vụ cơ bản.
  * Tìm hiểu kiến trúc Serverless và các thành phần của một ứng dụng web.
  * Tìm hiểu tổng quan Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB và AWS IAM.
  * Xây dựng kiến thức nền tảng để chuẩn bị phát triển hệ thống CloudMenu.

* **Tuần 2 (29/06 - 03/07) — IAM, Amazon S3 và Amazon CloudFront**

  * Tìm hiểu AWS IAM và các nguyên tắc quản lý quyền truy cập.
  * Tìm hiểu Amazon S3 và cách lưu trữ các tài nguyên frontend.
  * Tìm hiểu Amazon CloudFront và cơ chế phân phối nội dung thông qua HTTPS.
  * Thực hành triển khai website tĩnh với Amazon S3 và Amazon CloudFront.

* **Tuần 3 (06/07 - 10/07) — Amazon DynamoDB và thiết kế dữ liệu**

  * Tìm hiểu cơ sở dữ liệu NoSQL và dịch vụ Amazon DynamoDB.
  * Làm quen với các thành phần Table, Item, Attribute và Partition Key.
  * Thực hành tạo, đọc và cập nhật dữ liệu trên DynamoDB.
  * Thiết kế cấu trúc dữ liệu đơn hàng cho hệ thống CloudMenu.
  * Xác định `orderId` làm Partition Key cho bảng `CloudMenuOrders`.

* **Tuần 4 (13/07 - 17/07) — AWS Lambda và Serverless Backend**

  * Tìm hiểu AWS Lambda và mô hình Function as a Service (FaaS).
  * Thực hành tạo, cấu hình và kiểm thử Lambda Function.
  * Kết nối AWS Lambda với Amazon DynamoDB bằng AWS SDK for Python (`boto3`).
  * Xây dựng các Lambda Function để tạo đơn hàng, lấy danh sách đơn hàng và cập nhật trạng thái đơn hàng.

* **Tuần 5 (20/07 - 24/07) — Amazon API Gateway và REST API**

  * Tìm hiểu Amazon API Gateway, REST API và các phương thức HTTP.
  * Xây dựng các API phục vụ quy trình xử lý đơn hàng của CloudMenu.
  * Tích hợp Amazon API Gateway với AWS Lambda và Amazon DynamoDB.
  * Cấu hình CORS và kết nối frontend với backend của hệ thống.

* **Tuần 6 (27/07 - 31/07) — Phân tích và xây dựng hệ thống CloudMenu**

  * Phân tích yêu cầu và xác định các chức năng chính của CloudMenu.
  * Xây dựng cơ chế nhận diện bàn và gọi món thông qua QR Code.
  * Xây dựng giao diện Customer với menu, giỏ hàng và chức năng gửi đơn.
  * Xây dựng giao diện Kitchen để theo dõi và cập nhật trạng thái đơn hàng.

* **Tuần 7 (03/08 - 07/08) — Triển khai và tích hợp CloudMenu trên AWS**

  * Hoàn thiện các thành phần chính của hệ thống CloudMenu.
  * Triển khai frontend lên Amazon S3 và phân phối thông qua Amazon CloudFront.
  * Tích hợp frontend với Amazon API Gateway, AWS Lambda và Amazon DynamoDB.
  * Kiểm thử quy trình từ khách hàng gọi món đến bếp tiếp nhận và hoàn thành đơn hàng.

* **Tuần 8 (10/08 - 15/08) — Hoàn thiện CloudMenu và tổng kết chương trình**

  * Xây dựng và hoàn thiện Dashboard thống kê của hệ thống.
  * Hoàn thiện chức năng theo dõi thời gian và trạng thái đơn hàng.
  * Kiểm thử, sửa lỗi và hoàn thiện toàn bộ hệ thống CloudMenu.
  * Hoàn thiện sơ đồ kiến trúc, sơ đồ luồng xử lý, README, Workshop và tài liệu báo cáo.
  * Tổng kết kiến thức và kỹ năng đạt được trong chương trình First Cloud AI Journey.

  
## 5. Ngân sách

CloudMenu được định hướng sử dụng các dịch vụ AWS Serverless và Free Tier trong giai đoạn thử nghiệm nhằm giảm chi phí triển khai.

Các dịch vụ chính cần xem xét chi phí:

| Dịch vụ AWS | Thành phần / Sử dụng | Chi phí (USD/tháng) |
|---|---|---:|
| Amazon S3 | Frontend hosting + static assets | $0 - $3 |
| Amazon CloudFront | CDN + data transfer | $2 - $15 |
| Amazon API Gateway | REST API + API requests | $0 - $10 |
| AWS Lambda | Backend functions + invocations | $0 - $8 |
| Amazon DynamoDB | PMenu + Orders + Table data | $0 - $10 |
| AWS IAM | Users / Roles / Policies | Không phí AWS trực tiếp |
| **TỔNG CHI PHÍ AWS** |  | **$2 - $50** |

Đề xuất kiểm soát chi phí:
- Tận dụng AWS Free Tier trong giai đoạn phát triển và thử nghiệm.
- Thiết lập AWS Budgets để cảnh báo khi chi phí vượt mức ngân sách đặt ra.
- Tối ưu dung lượng S3 và sử dụng Lifecycle Policy khi dữ liệu tăng.
- Kiểm tra và xóa các tài nguyên thử nghiệm không còn sử dụng.
- Ưu tiên kiến trúc Serverless để tránh chi phí máy chủ chạy liên tục.

## 6. Rủi ro

- Chi phí AWS tăng ngoài dự kiến
  *Giảm thiểu*: Thiết lập Budget và theo dõi Cost Management

- Lượng truy cập tăng đột biến
  *Giảm thiểu*: Sử dụng kiến trúc Serverless có khả năng tự động mở rộng

- Truy cập trái phép API.
  *Giảm thiểu*: Sử dụng IAM và các cơ chế xác thực/phân quyền phù hợp

- Mất hoặc xóa nhầm dữ liệu.
  *Giảm thiểu*: Thiết lập Backup/Point-in-Time Recovery cho DynamoDB

- QR Code bị sử dụng sai bàn
  *Giảm thiểu*: Gắn định danh bàn vào từng QR Code và kiểm tra thông tin bàn khi tạo order

- Thao tác đặt món bị gửi nhiều lần
  *Giảm thiểu*: Thiết kế cơ chế kiểm tra và xử lý duplicate request