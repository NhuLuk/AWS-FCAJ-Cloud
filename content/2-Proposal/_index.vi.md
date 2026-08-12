---

title: "Bản đề xuất"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# CloudMenu – Hệ thống gọi món tại bàn trên AWS

## Giải pháp AWS Serverless cho quy trình gọi món, xử lý đơn hàng và theo dõi trạng thái

### 1. Tóm tắt điều hành

CloudMenu là hệ thống gọi món trực tuyến tại bàn, cho phép khách hàng sử dụng điện thoại quét QR Code được gán cho từng bàn để truy cập thực đơn, lựa chọn món và gửi đơn trực tiếp đến khu vực bếp.

Hệ thống được thiết kế cho mô hình nhà hàng hoặc quán ăn quy mô nhỏ và vừa, với ba nhóm người dùng chính:

* **Khách hàng**: xem menu, tìm kiếm hoặc lọc món, quản lý giỏ hàng, gửi đơn và theo dõi trạng thái đơn hàng.
* **Nhân viên bếp**: tiếp nhận đơn, xem thông tin đơn hàng và cập nhật trạng thái chế biến.
* **Admin/Manager**: theo dõi Dashboard tổng hợp từ dữ liệu đơn hàng, bao gồm số lượng đơn, doanh thu, trạng thái đơn hàng, doanh thu theo bàn và các món được gọi nhiều.

CloudMenu được đề xuất triển khai theo kiến trúc **AWS Serverless**, sử dụng Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda và Amazon DynamoDB làm các thành phần chính. Kiến trúc này giúp giảm nhu cầu quản lý máy chủ, hỗ trợ mở rộng theo lưu lượng truy cập và phù hợp với đặc điểm lượng người dùng thay đổi theo từng khung giờ của nhà hàng.

Mục tiêu của dự án là xây dựng một hệ thống end-to-end có thể triển khai thực tế trên AWS, từ frontend, backend, cơ sở dữ liệu, API, bảo mật cơ bản đến kiểm thử, monitoring và clean-up tài nguyên.

### 2. Tuyên bố vấn đề

*Vấn đề hiện tại*

Trong mô hình gọi món truyền thống, khách hàng thường phải chờ nhân viên đến bàn để nhận order. Khi nhà hàng đông khách, quy trình này có thể làm tăng thời gian chờ và tạo thêm áp lực cho nhân viên phục vụ.

Một số hạn chế có thể phát sinh gồm:

* Khách hàng phải chờ nhân viên đến nhận order.
* Dễ xảy ra sai sót khi ghi nhận món, số lượng hoặc số bàn.
* Nhân viên bếp khó theo dõi tập trung các đơn đang chờ xử lý.
* Việc cập nhật trạng thái đơn hàng giữa bếp và khách hàng chưa được tự động hóa.
* Quản lý khó tổng hợp nhanh số lượng đơn, doanh thu và tình trạng hoạt động.
* Hệ thống cần đáp ứng lưu lượng truy cập tăng cao vào các khung giờ cao điểm mà không phải duy trì máy chủ chạy liên tục.

*Giải pháp*

CloudMenu sử dụng QR Code riêng cho từng bàn để khách hàng trực tiếp truy cập giao diện gọi món. Thông tin bàn được truyền vào hệ thống thông qua QR Code, sau đó khách hàng có thể chọn món và gửi đơn.

Luồng xử lý chính:

**QR Code → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Frontend của CloudMenu được lưu trữ trên Amazon S3 và phân phối thông qua Amazon CloudFront. Khi khách hàng gửi đơn, frontend gọi REST API được cung cấp bởi Amazon API Gateway. API Gateway chuyển request đến AWS Lambda để xử lý nghiệp vụ và lưu dữ liệu đơn hàng vào Amazon DynamoDB.

Nhân viên bếp sử dụng giao diện riêng để đọc danh sách đơn và cập nhật trạng thái. Admin/Manager sử dụng Dashboard để tổng hợp và theo dõi dữ liệu đơn hàng.

Giải pháp giúp:

* Tự động nhận diện bàn thông qua QR Code.
* Giảm thao tác ghi nhận order thủ công.
* Hạn chế sai sót về món, số lượng và số bàn.
* Tập trung dữ liệu đơn hàng trên Amazon DynamoDB.
* Cho phép nhân viên bếp theo dõi và cập nhật trạng thái đơn.
* Cung cấp Dashboard hỗ trợ quản lý hoạt động kinh doanh.
* Tận dụng khả năng tự động mở rộng của kiến trúc Serverless.
* Giảm chi phí vận hành trong môi trường thử nghiệm và lưu lượng thấp.

*Lợi ích và giá trị của giải pháp*

CloudMenu giúp chuyển quy trình gọi món truyền thống sang quy trình số hóa đơn giản hơn. Khách hàng chủ động đặt món mà không cần chờ nhân viên ghi nhận order, trong khi nhân viên bếp có một giao diện tập trung để quản lý các đơn cần xử lý.

Đối với quản lý, dữ liệu đơn hàng được tập trung giúp việc theo dõi số lượng đơn, doanh thu, bàn hoạt động và món được gọi nhiều trở nên thuận tiện hơn.

Việc sử dụng kiến trúc Serverless giúp hệ thống không cần duy trì máy chủ chạy liên tục. Các dịch vụ như AWS Lambda và Amazon DynamoDB có thể đáp ứng theo mức sử dụng thực tế, phù hợp với một project cá nhân, môi trường thử nghiệm và mô hình có lưu lượng không cố định.

### 3. Kiến trúc giải pháp

CloudMenu sử dụng kiến trúc Serverless để triển khai frontend, backend và lưu trữ dữ liệu trên AWS.

Người dùng truy cập frontend thông qua Amazon CloudFront. CloudFront phân phối các file HTML, CSS và JavaScript được lưu trong Amazon S3. Frontend gửi request tới Amazon API Gateway, sau đó API Gateway chuyển request đến AWS Lambda để xử lý nghiệp vụ. Lambda đọc hoặc ghi dữ liệu đơn hàng trong Amazon DynamoDB.

AWS IAM cung cấp IAM Role cho Lambda để truy cập DynamoDB theo nguyên tắc **least privilege**, tránh việc hard-code AWS Access Key hoặc Secret Access Key trong source code.

Amazon CloudWatch được sử dụng để thu thập log và metric từ backend, hỗ trợ theo dõi hoạt động của Lambda, phát hiện lỗi và phục vụ quá trình kiểm thử hệ thống.

![Sơ đồ kiến trúc AWS CloudMenu](/images/2-Proposal/AWS_CloudMenu.png)

*Dịch vụ AWS sử dụng*

* *Amazon S3*: Lưu trữ frontend và các static assets của CloudMenu.
* *Amazon CloudFront*: Phân phối frontend thông qua CDN và HTTPS.
* *Amazon API Gateway*: Cung cấp REST API cho frontend giao tiếp với backend.
* *AWS Lambda*: Xử lý các nghiệp vụ tạo đơn hàng, lấy danh sách đơn hàng và cập nhật trạng thái đơn.
* *Amazon DynamoDB*: Lưu trữ dữ liệu đơn hàng, số bàn, danh sách món trong đơn và trạng thái xử lý.
* *AWS IAM*: Cung cấp IAM Role và kiểm soát quyền truy cập giữa Lambda và các AWS Services theo nguyên tắc least privilege.
* *Amazon CloudWatch*: Thu thập log và metric phục vụ monitoring, troubleshooting và kiểm thử backend.
* *AWS Budgets*: Hỗ trợ theo dõi và cảnh báo khi chi phí AWS vượt ngưỡng dự kiến.

*Thiết kế thành phần*

* *Customer Interface*: Cho phép khách hàng truy cập menu từ QR Code, xác định bàn, chọn món, quản lý giỏ hàng, gửi đơn và theo dõi trạng thái đơn.
* *Kitchen Interface*: Cho phép nhân viên bếp xem các đơn hàng và cập nhật trạng thái chế biến.
* *Admin Dashboard*: Tổng hợp dữ liệu đơn hàng để hiển thị số lượng đơn, doanh thu, trạng thái đơn, doanh thu theo bàn và các món được gọi nhiều.
* *Frontend Hosting*: Amazon S3 lưu trữ frontend, Amazon CloudFront cung cấp HTTPS và phân phối nội dung tới người dùng.
* *API Layer*: Amazon API Gateway tiếp nhận request từ frontend và chuyển đến Lambda Function phù hợp.
* *Backend Processing*: AWS Lambda thực hiện xử lý logic nghiệp vụ.
* *Data Storage*: Amazon DynamoDB lưu trữ dữ liệu đơn hàng.
* *Security*: AWS IAM giới hạn quyền truy cập giữa Lambda và DynamoDB.
* *Monitoring*: Amazon CloudWatch lưu log, metric và hỗ trợ thiết lập cảnh báo cho backend.

### 4. Triển khai kỹ thuật

*Các giai đoạn triển khai*

Dự án CloudMenu được triển khai theo các giai đoạn từ học và thiết kế kiến trúc đến xây dựng hệ thống hoàn chỉnh:

1. *Nghiên cứu và thiết kế kiến trúc*: Tìm hiểu AWS Cloud, Serverless Architecture và các dịch vụ phù hợp với CloudMenu.
2. *Thiết kế dữ liệu và backend*: Xây dựng bảng Amazon DynamoDB, IAM Role và các AWS Lambda Function.
3. *Xây dựng REST API*: Sử dụng Amazon API Gateway để kết nối frontend với Lambda backend.
4. *Phát triển giao diện CloudMenu*: Xây dựng Customer Interface, Kitchen Interface và Admin Dashboard.
5. *Triển khai frontend*: Upload frontend lên Amazon S3 và phân phối bằng Amazon CloudFront.
6. *Tích hợp end-to-end*: Kết nối frontend, API Gateway, Lambda và DynamoDB thành một hệ thống hoàn chỉnh.
7. *Kiểm thử và monitoring*: Kiểm thử các luồng sử dụng, kiểm tra CloudWatch Logs, metric và lỗi.
8. *Clean-up và hoàn thiện tài liệu*: Xóa các tài nguyên không còn cần thiết để tránh phát sinh chi phí và hoàn thiện Workshop song ngữ.

*Yêu cầu kỹ thuật*

* *AWS Account*: Tài khoản AWS có quyền tạo và quản lý các dịch vụ sử dụng trong project.
* *AWS Region*: Các tài nguyên backend được triển khai trong cùng Region phù hợp để đơn giản hóa việc quản lý.
* *Frontend*: HTML, CSS và JavaScript.
* *Backend*: AWS Lambda sử dụng Python và AWS SDK for Python (`boto3`).
* *Database*: Amazon DynamoDB với `orderId` làm Partition Key cho bảng `CloudMenuOrders`.
* *API*: Amazon API Gateway REST API.
* *Security*: IAM Role cho Lambda, không hard-code AWS credentials trong source code.
* *Monitoring*: Amazon CloudWatch Logs, Lambda metrics và CloudWatch Alarm.
* *Development tools*: Visual Studio Code, trình duyệt web và các công cụ kiểm thử API khi cần.

### 5. Lộ trình & Mốc triển khai

* *Tuần 1 (22/06 - 26/06) — AWS Cloud và Serverless cơ bản*

  * Làm quen với nền tảng AWS Cloud.
  * Tìm hiểu kiến trúc Serverless.
  * Tìm hiểu tổng quan Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB và AWS IAM.

* *Tuần 2 (29/06 - 03/07) — IAM, Amazon S3 và Amazon CloudFront*

  * Tìm hiểu AWS IAM và nguyên tắc least privilege.
  * Thực hành lưu trữ website tĩnh trên Amazon S3.
  * Tìm hiểu và triển khai Amazon CloudFront.

* *Tuần 3 (06/07 - 10/07) — Amazon DynamoDB và thiết kế dữ liệu*

  * Tìm hiểu Amazon DynamoDB và mô hình NoSQL.
  * Tạo bảng `CloudMenuOrders`.
  * Xác định `orderId` làm Partition Key.
  * Thiết kế cấu trúc dữ liệu đơn hàng.

* *Tuần 4 (13/07 - 17/07) — AWS Lambda và Serverless Backend*

  * Tìm hiểu AWS Lambda và Function as a Service.
  * Tạo IAM Role cho Lambda.
  * Kết nối Lambda với DynamoDB bằng `boto3`.
  * Xây dựng các Lambda Function xử lý đơn hàng.

* *Tuần 5 (20/07 - 24/07) — Amazon API Gateway và REST API*

  * Xây dựng REST API.
  * Tích hợp API Gateway với Lambda.
  * Cấu hình các endpoint phục vụ CloudMenu.
  * Cấu hình CORS và kiểm thử API.

* *Tuần 6 (27/07 - 31/07) — Phân tích và xây dựng CloudMenu*

  * Hoàn thiện yêu cầu chức năng.
  * Xây dựng cơ chế nhận diện bàn bằng QR Code.
  * Xây dựng Customer Interface.
  * Xây dựng Kitchen Interface.

* *Tuần 7 (03/08 - 07/08) — Triển khai và tích hợp trên AWS*

  * Hoàn thiện các thành phần của hệ thống.
  * Upload frontend lên Amazon S3.
  * Phân phối frontend bằng Amazon CloudFront.
  * Kết nối frontend với API Gateway, Lambda và DynamoDB.
  * Kiểm thử luồng Customer → Kitchen.

* *Tuần 8 (10/08 - 15/08) — Hoàn thiện, monitoring và tổng kết*

  * Hoàn thiện Admin Dashboard.
  * Hoàn thiện theo dõi thời gian và trạng thái đơn hàng.
  * Kiểm thử Customer, Kitchen và Admin.
  * Kiểm tra CloudWatch Logs và metrics.
  * Thiết lập CloudWatch Alarm cho backend.
  * Hoàn thiện sơ đồ kiến trúc, Workshop, README và tài liệu báo cáo.
  * Clean-up tài nguyên không còn sử dụng.

### 6. Ước tính ngân sách

CloudMenu sử dụng các dịch vụ AWS Serverless và ưu tiên tận dụng AWS Free Tier trong môi trường phát triển và thử nghiệm.

Chi phí thực tế phụ thuộc vào số lượng request, dung lượng lưu trữ, data transfer và thời gian lưu CloudWatch Logs.

*Chi phí hạ tầng dự kiến*

* Amazon S3: khoảng 0–3 USD/tháng cho frontend và static assets.
* Amazon CloudFront: khoảng 0–15 USD/tháng, tùy theo data transfer và số lượng request.
* Amazon API Gateway: khoảng 0–10 USD/tháng, tùy theo số lượng API request.
* AWS Lambda: khoảng 0–8 USD/tháng, tùy theo số lần invocation và thời gian thực thi.
* Amazon DynamoDB: khoảng 0–10 USD/tháng, tùy theo số lượng request và dung lượng dữ liệu.
* Amazon CloudWatch: chi phí phụ thuộc lượng log, metric và alarm sử dụng.
* AWS IAM: không có phí AWS trực tiếp cho việc sử dụng IAM Users, Roles và Policies.
* AWS Budgets: được sử dụng để theo dõi ngân sách và cảnh báo chi phí.

*Tổng chi phí dự kiến*: khoảng **0–50 USD/tháng** tùy theo lưu lượng sử dụng. Trong môi trường thử nghiệm với lưu lượng thấp và các giới hạn Free Tier còn khả dụng, chi phí thực tế có thể thấp hơn đáng kể.

*Biện pháp kiểm soát chi phí*

* Tận dụng AWS Free Tier khi phù hợp.
* Thiết lập AWS Budgets để cảnh báo khi chi phí vượt mức dự kiến.
* Theo dõi CloudWatch Logs và xóa log không còn cần thiết.
* Tối ưu dung lượng Amazon S3.
* Kiểm tra và xóa các AWS resources thử nghiệm không còn sử dụng.
* Ưu tiên kiến trúc Serverless để tránh duy trì máy chủ chạy liên tục.
* Thực hiện clean-up đầy đủ sau khi hoàn thành Workshop.

### 7. Đánh giá rủi ro

*Ma trận rủi ro*

* Chi phí AWS tăng ngoài dự kiến: Ảnh hưởng trung bình, xác suất thấp.
* Lượng truy cập tăng đột biến: Ảnh hưởng trung bình, xác suất trung bình.
* Lambda hoặc API phát sinh lỗi: Ảnh hưởng cao, xác suất trung bình.
* Mất hoặc xóa nhầm dữ liệu DynamoDB: Ảnh hưởng cao, xác suất thấp.
* QR Code được sử dụng sai bàn: Ảnh hưởng trung bình, xác suất trung bình.
* Người dùng gửi thao tác đặt món nhiều lần: Ảnh hưởng trung bình, xác suất trung bình.
* Truy cập API ngoài ý muốn: Ảnh hưởng cao, xác suất trung bình.

*Chiến lược giảm thiểu*

* *Chi phí*: Sử dụng AWS Budgets, theo dõi Cost Management và clean-up tài nguyên không cần thiết.
* *Lưu lượng*: Tận dụng khả năng tự động mở rộng của API Gateway, Lambda và DynamoDB.
* *Backend errors*: Sử dụng Amazon CloudWatch Logs, metrics và Alarm để phát hiện lỗi.
* *Dữ liệu*: Sử dụng DynamoDB Point-in-Time Recovery hoặc backup khi phù hợp.
* *QR Code*: Gắn định danh bàn vào QR Code và kiểm tra thông tin bàn trước khi tạo order.
* *Duplicate request*: Thiết kế cơ chế chống gửi đơn nhiều lần và kiểm tra `orderId`.
* *API access*: Cấu hình CORS phù hợp, giới hạn IAM permission giữa các AWS Services và xem xét bổ sung authentication/authorization cho các chức năng quản trị.

*Kế hoạch dự phòng*

* Kiểm tra CloudWatch Logs để xác định nguyên nhân khi backend gặp lỗi.
* Kiểm thử trực tiếp Lambda Function nếu API Gateway gặp sự cố trong quá trình troubleshooting.
* Khôi phục dữ liệu từ DynamoDB backup hoặc Point-in-Time Recovery khi được kích hoạt.
* Tạm dừng hoặc xóa các AWS resources không cần thiết nếu phát sinh chi phí ngoài dự kiến.
* Lưu source code và tài liệu cấu hình để có thể triển khai lại hệ thống khi cần.

### 8. Kết quả kỳ vọng

*Cải tiến kỹ thuật*: CloudMenu tạo ra một hệ thống gọi món end-to-end trên AWS, trong đó khách hàng có thể đặt món bằng QR Code, nhân viên bếp có thể tiếp nhận và cập nhật trạng thái đơn, còn Admin/Manager có thể theo dõi hoạt động thông qua Dashboard.

*Kết quả triển khai*: Frontend được phân phối qua Amazon CloudFront và Amazon S3; backend được xây dựng bằng Amazon API Gateway, AWS Lambda và Amazon DynamoDB; IAM được áp dụng để kiểm soát quyền giữa các AWS Services; Amazon CloudWatch hỗ trợ logging và monitoring.

*Khả năng mở rộng*: Kiến trúc Serverless cho phép hệ thống đáp ứng sự thay đổi về lưu lượng mà không yêu cầu quản lý máy chủ cố định.

*Giá trị dài hạn*: CloudMenu có thể tiếp tục được mở rộng với authentication cho nhân viên, quản lý menu động, thanh toán trực tuyến, thông báo thời gian thực, hệ thống đặt bàn hoặc các chức năng phân tích nâng cao trong tương lai.
