---
title: "Tự đánh giá"
date: 2026-06-22
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## 6. Tự đánh giá

Trong thời gian thực tập tại **CÔNG TY TNHH AMAZON WEB SERVICES VIỆT NAM** từ **22/06/2026 đến 15/08/2026**, tôi có cơ hội tham gia chương trình **First Cloud AI Journey (FCAJ)** và thực hiện dự án **CloudMenu** – hệ thống gọi món trực tuyến tại bàn được xây dựng và triển khai trên nền tảng Amazon Web Services (AWS).

CloudMenu cho phép khách hàng sử dụng điện thoại truy cập thực đơn thông qua mã QR được gán cho từng bàn, lựa chọn món ăn, gửi đơn hàng và theo dõi trạng thái xử lý. Bên cạnh giao diện dành cho khách hàng, hệ thống còn cung cấp giao diện Kitchen để nhân viên bếp tiếp nhận và cập nhật trạng thái đơn hàng, cùng Dashboard giúp theo dõi các thông tin thống kê của hệ thống.

Trong quá trình thực tập, tôi đã tìm hiểu và áp dụng các kiến thức về điện toán đám mây, kiến trúc Serverless, cơ sở dữ liệu NoSQL và phát triển ứng dụng web để từng bước xây dựng CloudMenu.

Các dịch vụ AWS chính được sử dụng trong dự án gồm:

- **Amazon S3:** Lưu trữ các tệp frontend của CloudMenu.
- **Amazon CloudFront:** Phân phối nội dung frontend đến người dùng thông qua HTTPS.
- **Amazon API Gateway:** Cung cấp HTTP API để frontend giao tiếp với backend.
- **AWS Lambda:** Xử lý các nghiệp vụ liên quan đến đơn hàng.
- **Amazon DynamoDB:** Lưu trữ dữ liệu đơn hàng của hệ thống.
- **AWS Identity and Access Management (IAM):** Quản lý quyền truy cập giữa các dịch vụ AWS.

### Công việc đã thực hiện

Trong quá trình thực hiện dự án CloudMenu, tôi đã tham gia các công việc sau:

- Tìm hiểu các dịch vụ AWS và mô hình kiến trúc Serverless.
- Phân tích yêu cầu và xác định các chức năng chính của hệ thống CloudMenu.
- Tìm hiểu Amazon DynamoDB và thiết kế cấu trúc dữ liệu đơn hàng.
- Tạo bảng `CloudMenuOrders` và sử dụng `orderId` làm Partition Key.
- Tìm hiểu AWS Lambda và xây dựng các Lambda Function phục vụ xử lý đơn hàng.
- Xây dựng Function `createOrder` để tạo đơn hàng mới.
- Xây dựng Function `getOrders` để lấy danh sách đơn hàng.
- Xây dựng Function `updateOrderStatus` để cập nhật trạng thái đơn hàng.
- Kết nối AWS Lambda với Amazon DynamoDB bằng AWS SDK for Python (`boto3`).
- Cấu hình IAM Execution Role và các quyền cần thiết để Lambda truy cập DynamoDB và ghi log.
- Xây dựng HTTP API bằng Amazon API Gateway.
- Tạo các Route `POST /order`, `GET /orders` và `PUT /orders/{orderId}`.
- Tích hợp các Route của API Gateway với Lambda Function tương ứng.
- Kiểm thử API và xử lý các lỗi phát sinh trong quá trình tích hợp.
- Triển khai frontend của CloudMenu lên Amazon S3.
- Sử dụng Amazon CloudFront để phân phối frontend đến người dùng.
- Xây dựng và kiểm thử giao diện Customer để xem menu và đặt món.
- Xây dựng và kiểm thử giao diện Kitchen để theo dõi và cập nhật trạng thái đơn hàng.
- Xây dựng chức năng theo dõi trạng thái đơn hàng cho khách hàng.
- Xây dựng Worklog, Proposal, Workshop, sơ đồ kiến trúc, sơ đồ luồng xử lý và các tài liệu báo cáo liên quan đến dự án.

Thông qua quá trình thực hiện CloudMenu, tôi hiểu rõ hơn cách các dịch vụ AWS có thể được kết hợp để xây dựng một ứng dụng Serverless hoàn chỉnh. Tôi cũng có thêm kinh nghiệm trong việc triển khai và tích hợp các thành phần frontend, API, backend và cơ sở dữ liệu trên môi trường AWS.

Bên cạnh kiến thức chuyên môn, quá trình thực tập cũng giúp tôi cải thiện khả năng tự học, tìm kiếm tài liệu, phân tích vấn đề, xử lý lỗi, quản lý thời gian, giao tiếp và phối hợp với các thành viên trong nhóm.

### Bảng tự đánh giá

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
| --- | --- | --- | :---: | :---: | :---: |
| 1 | **Kiến thức và kỹ năng chuyên môn** | Có khả năng áp dụng Amazon S3, CloudFront, API Gateway, Lambda, DynamoDB và IAM để xây dựng và triển khai CloudMenu. | ✅ | ☐ | ☐ |
| 2 | **Khả năng học hỏi** | Chủ động tìm hiểu các dịch vụ AWS, kiến trúc Serverless và các kiến thức mới trong quá trình thực tập. | ☐ | ✅ | ☐ |
| 3 | **Tính chủ động** | Chủ động nghiên cứu tài liệu, thực hành và tìm hướng xử lý khi gặp vấn đề trong quá trình triển khai. | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Có trách nhiệm với các nhiệm vụ được giao và cố gắng hoàn thành công việc theo tiến độ của chương trình. | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Tuân thủ lịch trình, yêu cầu của chương trình và quy trình thực hiện báo cáo thực tập. | ☐ | ✅ | ☐ |
| 6 | **Tính cầu tiến** | Tiếp nhận góp ý và chủ động điều chỉnh, bổ sung kiến thức cũng như hoàn thiện sản phẩm. | ✅ | ☐ | ☐ |
| 7 | **Giao tiếp** | Trao đổi với các thành viên trong nhóm và mentor khi gặp khó khăn trong quá trình thực hiện dự án. | ☐ | ✅ | ☐ |
| 8 | **Hợp tác nhóm** | Phối hợp với các thành viên trong quá trình phân tích, xây dựng, kiểm thử và hoàn thiện CloudMenu. | ✅ | ☐ | ☐ |
| 9 | **Ứng xử chuyên nghiệp** | Giữ thái độ nghiêm túc, tôn trọng các thành viên và có trách nhiệm với công việc được giao. | ✅ | ☐ | ☐ |
| 10 | **Tư duy giải quyết vấn đề** | Có khả năng tìm nguyên nhân và xử lý các lỗi phát sinh khi cấu hình AWS và tích hợp API Gateway, Lambda và DynamoDB. | ☐ | ✅ | ☐ |
| 11 | **Đóng góp vào dự án** | Tham gia xây dựng CloudMenu, triển khai các thành phần AWS, kiểm thử hệ thống và hoàn thiện tài liệu Workshop. | ✅ | ☐ | ☐ |
| 12 | **Tổng thể** | Hoàn thành tốt các nội dung của chương trình thực tập và tích lũy thêm kiến thức, kỹ năng thực tế về AWS Cloud. | ✅ | ☐ | ☐ |

### Những kiến thức và kỹ năng đạt được

Sau quá trình thực tập, tôi đã đạt được một số kiến thức và kỹ năng quan trọng:

- Hiểu rõ hơn về các khái niệm cơ bản của AWS Cloud và kiến trúc Serverless.
- Hiểu vai trò của Amazon S3 và Amazon CloudFront trong việc lưu trữ và phân phối frontend.
- Biết cách xây dựng backend Serverless bằng Amazon API Gateway và AWS Lambda.
- Biết cách sử dụng Amazon DynamoDB để lưu trữ và truy xuất dữ liệu.
- Hiểu cách sử dụng IAM Role và Policy để quản lý quyền truy cập giữa các dịch vụ AWS.
- Có kinh nghiệm kiểm thử API và xử lý lỗi trong quá trình tích hợp các dịch vụ.
- Hiểu được luồng hoạt động của một ứng dụng web từ frontend đến API, backend và database.
- Có thêm kinh nghiệm xây dựng tài liệu kỹ thuật và Workshop.
- Cải thiện khả năng tự nghiên cứu và tìm kiếm giải pháp khi gặp vấn đề kỹ thuật.
- Cải thiện kỹ năng làm việc nhóm và quản lý tiến độ công việc.

### Những điểm cần cải thiện

Bên cạnh những kết quả đạt được, tôi nhận thấy bản thân vẫn còn một số điểm cần tiếp tục cải thiện:

- Nâng cao kiến thức về thiết kế kiến trúc AWS cho các hệ thống có quy mô lớn và yêu cầu phức tạp hơn.
- Tìm hiểu sâu hơn về bảo mật, xác thực và phân quyền người dùng trong ứng dụng Serverless.
- Nâng cao kỹ năng sử dụng Amazon CloudWatch để giám sát hệ thống, phân tích log và xử lý sự cố.
- Tìm hiểu thêm về các phương pháp tối ưu hiệu năng và chi phí khi vận hành ứng dụng trên AWS.
- Tìm hiểu thêm về Infrastructure as Code và CI/CD để tự động hóa quá trình triển khai.
- Nâng cao kỹ năng kiểm thử và xử lý các trường hợp lỗi của hệ thống.
- Rèn luyện kỹ năng trình bày và giải thích giải pháp kỹ thuật trước nhóm và mentor.
- Tiếp tục cải thiện khả năng đọc hiểu tài liệu kỹ thuật bằng tiếng Anh.

### Tổng kết

Chương trình **First Cloud AI Journey** đã giúp tôi có cơ hội tiếp cận và thực hành với các dịch vụ AWS trong một dự án thực tế. Thông qua quá trình xây dựng CloudMenu, tôi không chỉ hiểu rõ hơn về kiến trúc Serverless mà còn có cơ hội thực hành quá trình thiết kế, triển khai, tích hợp và kiểm thử một ứng dụng trên nền tảng đám mây.

Những kiến thức và kinh nghiệm đạt được trong quá trình thực tập là nền tảng quan trọng để tôi tiếp tục tìm hiểu sâu hơn về Cloud Computing, Software Development và các công nghệ AWS trong thời gian tới.