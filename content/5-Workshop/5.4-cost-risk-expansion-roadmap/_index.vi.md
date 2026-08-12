---

title : "Chi phí, rủi ro và định hướng mở rộng"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4. </b> "
-----------------------

## 5.4. Chi phí, rủi ro và định hướng mở rộng

### Tối ưu chi phí

CloudMenu được xây dựng theo kiến trúc serverless và sử dụng các managed services nhằm hạn chế chi phí vận hành cũng như tránh phải duy trì máy chủ hoạt động liên tục. Các dịch vụ chính cần được theo dõi về chi phí gồm Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB và Amazon CloudWatch.

Trong môi trường development và testing, có thể áp dụng một số biện pháp tối ưu như sau:

* Sử dụng Amazon S3 để lưu trữ frontend, đồng thời theo dõi dung lượng lưu trữ, số lượng request và data transfer phát sinh.
* Theo dõi lượng request và lưu lượng truyền dữ liệu của Amazon CloudFront, đặc biệt khi số lượng người dùng truy cập tăng.
* Tối ưu số lần gọi và thời gian thực thi của AWS Lambda để giảm chi phí liên quan đến invocation và compute time.
* Thiết kế DynamoDB phù hợp với workload thực tế của CloudMenu, hạn chế các thao tác đọc không cần thiết và tối ưu cách truy vấn bảng `CloudMenuOrders`.
* Theo dõi số lượng request đến Amazon API Gateway, nhất là đối với các API được gọi thường xuyên như Get Orders.
* Quản lý CloudWatch Logs hợp lý và thiết lập thời gian lưu log phù hợp để tránh giữ lại dữ liệu không cần thiết trong thời gian dài.
* Tận dụng AWS Free Tier trong phạm vi phù hợp với môi trường thử nghiệm và thường xuyên kiểm tra chi phí thông qua AWS Billing và Cost Explorer.
* Chỉ bổ sung các dịch vụ như VPC, NAT Gateway, WAF hoặc Cognito khi hệ thống thực sự có nhu cầu, nhằm giữ kiến trúc đơn giản và hạn chế chi phí phát sinh không cần thiết.

Do CloudMenu sử dụng Lambda và DynamoDB theo mô hình serverless, chi phí có thể thay đổi theo mức sử dụng thực tế thay vì phát sinh một khoản cố định để duy trì application server liên tục.

### Rủi ro và giảm thiểu

Một số rủi ro chính của CloudMenu bao gồm:

**Rủi ro tăng chi phí:** số lượng request đến API Gateway, Lambda, DynamoDB hoặc CloudFront tăng cao có thể khiến chi phí tăng theo mức sử dụng.

**Rủi ro cấu hình sai:** việc cấu hình S3, IAM policy, API Gateway hoặc CloudFront không chính xác có thể dẫn đến truy cập ngoài mong muốn hoặc khiến hệ thống hoạt động không đúng.

**Rủi ro mất hoặc sai lệch dữ liệu:** lỗi trong quá trình tạo đơn hàng, lấy dữ liệu hoặc cập nhật trạng thái có thể làm thông tin trong bảng `CloudMenuOrders` không chính xác.

**Rủi ro liên quan đến credentials:** nếu AWS Access Key, Secret Access Key hoặc các thông tin nhạy cảm được lưu trực tiếp trong source code thì có thể dẫn đến rò rỉ thông tin.

**Rủi ro khi triển khai frontend thủ công:** CloudMenu hiện vẫn upload frontend lên S3 bằng phương pháp thủ công, vì vậy có thể xảy ra trường hợp thiếu file, sử dụng nhầm phiên bản hoặc CloudFront chưa phân phối đúng phiên bản mới.

**Rủi ro khi thay đổi Lambda:** việc cập nhật logic backend hoặc thay đổi cấu hình API Gateway có thể ảnh hưởng đến cả ba giao diện Customer, Kitchen và Manager.

**Rủi ro phụ thuộc vào AWS services:** CloudMenu sử dụng nhiều AWS managed services, vì vậy các thay đổi kiến trúc trong tương lai cần xem xét sự tương thích và phụ thuộc giữa các dịch vụ.

Các biện pháp giảm thiểu gồm:

* Áp dụng nguyên tắc Least Privilege cho IAM Roles và policies.
* Không lưu AWS credentials hoặc secret trực tiếp trong source code hay commit lên GitHub.
* Cấu hình quyền truy cập S3 phù hợp và hạn chế quyền ghi hoặc xóa khi không cần thiết.
* Không cho phép frontend truy cập trực tiếp DynamoDB; mọi thao tác dữ liệu được thực hiện thông qua API Gateway và Lambda.
* Sử dụng CloudWatch Logs và Metrics để phát hiện lỗi và theo dõi các hoạt động bất thường.
* Thiết lập cơ chế backup và recovery phù hợp cho dữ liệu DynamoDB.
* Kiểm thử các API quan trọng trước khi triển khai phiên bản mới.
* Sử dụng checklist khi upload frontend hoặc cập nhật Lambda để giảm nguy cơ sai sót trong quá trình triển khai thủ công.

### Lộ trình mở rộng

CloudMenu có thể được mở rộng theo từng giai đoạn tùy theo nhu cầu và quy mô sử dụng của hệ thống.

* **Chuẩn hóa CI/CD:** xây dựng pipeline tự động từ GitHub để build và deploy frontend lên S3, đồng thời tự động triển khai các Lambda Function.
* **Tăng cường kiểm thử:** bổ sung unit test, integration test và smoke test cho các nghiệp vụ quan trọng như tạo đơn hàng, lấy danh sách đơn hàng và cập nhật trạng thái.
* **Bổ sung authentication và authorization:** tích hợp Amazon Cognito khi hệ thống cần quản lý tài khoản và phân quyền giữa Customer, Kitchen và Manager.
* **Tăng cường bảo mật API:** có thể bổ sung authentication, authorization, rate limiting và AWS WAF khi CloudMenu được triển khai trong môi trường production với lượng truy cập lớn.
* **Cải thiện khả năng quan sát:** xây dựng CloudWatch Dashboard và Alarms để theo dõi API Gateway, Lambda, DynamoDB và phát hiện lỗi hoặc mức sử dụng bất thường.
* **Mở rộng mô hình dữ liệu:** bổ sung các bảng hoặc entity như `MenuItems`, `Tables`, `Restaurants` và `OrderHistory` khi hệ thống cần hỗ trợ đầy đủ hơn các nghiệp vụ nhà hàng.
* **Hỗ trợ nhiều nhà hàng:** nếu CloudMenu phát triển theo hướng multi-tenant, có thể mở rộng mô hình dữ liệu và cơ chế phân quyền để từng nhà hàng quản lý menu, bàn và đơn hàng riêng.
* **Xử lý bất đồng bộ:** khi xuất hiện các tác vụ nền như gửi thông báo, cập nhật báo cáo hoặc đồng bộ dữ liệu, có thể bổ sung các dịch vụ event-driven phù hợp của AWS.
* **Tối ưu hiệu năng:** khi số lượng request và dữ liệu tăng, có thể xem xét caching, DynamoDB Streams hoặc các dịch vụ phân tích phù hợp với workload thực tế.
* **Tăng cường network security:** chỉ bổ sung VPC, VPC Endpoints hoặc các thành phần network phức tạp khi backend cần truy cập private resources hoặc có yêu cầu kiểm soát mạng ở mức cao hơn.

Mục tiêu của lộ trình là giữ CloudMenu ở trạng thái đơn giản, serverless và tiết kiệm chi phí trong giai đoạn phát triển, đồng thời tạo nền tảng để hệ thống có thể tiếp tục mở rộng khi số lượng nhà hàng, người dùng và đơn hàng tăng lên.
