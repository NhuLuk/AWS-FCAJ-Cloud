---

title : "Frontend Hosting và xác thực người dùng"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
------------------------

## 5.2.2 Frontend Hosting và xác thực người dùng

Phần này trình bày cách CloudMenu tổ chức lớp frontend bằng **Amazon S3** và **Amazon CloudFront**, đồng thời mô tả định hướng xác thực và phân quyền người dùng cho các giao diện Customer, Kitchen và Manager.

Trong kiến trúc hiện tại, frontend được xây dựng dưới dạng static web application và được lưu trữ trên Amazon S3. Amazon CloudFront được đặt phía trước S3 để phân phối nội dung đến người dùng thông qua Internet.

Đối với authentication và authorization, CloudMenu hiện ưu tiên giữ mô hình đơn giản trong giai đoạn development/testing. Khi hệ thống cần quản lý tài khoản tập trung và phân quyền rõ ràng hơn giữa các nhóm người dùng, Amazon Cognito có thể được bổ sung vào kiến trúc.

### Amazon S3

**Vai trò:** Amazon S3 được sử dụng để lưu trữ các file frontend và static assets của CloudMenu.

Các thành phần có thể được lưu trong S3 bao gồm:

* Customer Interface.
* Kitchen Interface.
* Manager Dashboard.
* HTML files.
* CSS files.
* JavaScript files.
* Hình ảnh và các static assets khác.

Trong mô hình này, S3 đóng vai trò là lớp lưu trữ frontend và đồng thời là origin để Amazon CloudFront lấy nội dung.

Người dùng không cần truy cập trực tiếp đến S3 bucket. Thay vào đó, frontend được phân phối thông qua CloudFront.

Luồng truy cập cơ bản:

**User → Amazon CloudFront → Amazon S3**

Sau khi developer cập nhật frontend, các file mới được triển khai lên S3 theo quy trình deployment của project. Trong phạm vi hiện tại của CloudMenu, quá trình upload frontend có thể được thực hiện thủ công và sau đó CloudFront phân phối phiên bản mới đến người dùng.

**Lý do lựa chọn:** Amazon S3 phù hợp với CloudMenu vì frontend chủ yếu gồm các file tĩnh và không cần xử lý server-side.

Một số lợi ích gồm:

* Không cần duy trì web server riêng.
* Phù hợp với static website assets.
* Dễ tích hợp với Amazon CloudFront.
* Có khả năng mở rộng khi dung lượng frontend tăng.
* Phù hợp với mục tiêu tối ưu chi phí trong môi trường development và testing.

### Amazon CloudFront

**Vai trò:** Amazon CloudFront được sử dụng làm lớp phân phối nội dung phía trước Amazon S3.

Khi Customer, Kitchen hoặc Manager truy cập CloudMenu, request frontend được gửi đến CloudFront thay vì trực tiếp đến S3.

CloudFront có thể lấy nội dung từ S3 origin và phân phối đến người dùng thông qua hệ thống edge locations của AWS.

Luồng phân phối frontend:

**Customer / Kitchen / Manager → CloudFront → S3**

CloudFront giúp tách lớp public access khỏi lớp lưu trữ frontend.

Ngoài việc phân phối nội dung, CloudFront còn hỗ trợ HTTPS và caching, giúp giảm số lần phải truy xuất cùng một file từ S3 origin.

Nếu kiến trúc yêu cầu hạn chế truy cập trực tiếp vào bucket, có thể sử dụng **Origin Access Control (OAC)** để chỉ cho phép CloudFront truy cập các object cần thiết trong S3.

Trong mô hình này, bucket không cần phải được public hoàn toàn để người dùng có thể truy cập frontend thông qua CloudFront.

**Lý do lựa chọn:** CloudFront phù hợp với CloudMenu vì:

* Phân phối nội dung frontend thông qua CDN.
* Hỗ trợ HTTPS.
* Giảm latency khi người dùng truy cập nội dung.
* Hỗ trợ caching static assets.
* Giảm nhu cầu expose trực tiếp S3 bucket ra Internet.
* Phù hợp với kiến trúc serverless.

Việc kết hợp S3 và CloudFront giúp CloudMenu cung cấp frontend mà không cần duy trì EC2 instance hoặc web server truyền thống.

### Frontend Configuration

Frontend CloudMenu cần biết địa chỉ backend API để gửi các request liên quan đến dữ liệu và nghiệp vụ.

API endpoint được cung cấp bởi Amazon API Gateway.

Luồng kết nối có thể được mô tả như sau:

**Frontend → API Gateway Endpoint → AWS Lambda → DynamoDB**

Frontend có thể sử dụng một API Base URL để tạo các request đến backend.

Ví dụ:

```text
API_BASE_URL = API Gateway endpoint
```

Các request có thể phục vụ các nghiệp vụ như:

* Tạo đơn hàng.
* Lấy danh sách đơn hàng.
* Cập nhật trạng thái đơn hàng.
* Lấy thông tin phục vụ Customer, Kitchen hoặc Manager.

Việc tách API endpoint khỏi phần logic chính của frontend giúp việc thay đổi môi trường thuận tiện hơn.

Ví dụ, hệ thống có thể có các endpoint khác nhau cho:

* Development.
* Testing.
* Production.

Trong một hệ thống có build pipeline hoàn chỉnh, API Base URL có thể được truyền thông qua environment variable hoặc build configuration.

Trong phạm vi hiện tại, nếu CloudMenu chưa sử dụng một build system hỗ trợ environment variables, endpoint cũng có thể được quản lý thông qua file cấu hình riêng thay vì hard-code rải rác trong nhiều file JavaScript.

### Authentication và Authorization

Ba giao diện của CloudMenu có yêu cầu truy cập khác nhau.

**Customer Interface** chủ yếu được thiết kế cho khách hàng truy cập thông qua QR Code tại bàn.

Trong khi đó, **Kitchen Interface** và **Manager Dashboard** thực hiện các chức năng nội bộ như xử lý đơn hàng, thay đổi trạng thái và xem dữ liệu quản lý.

Do đó, nếu CloudMenu được đưa vào sử dụng thực tế, các giao diện nội bộ không nên chỉ được bảo vệ bằng việc ẩn URL hoặc dựa vào việc người dùng biết đường dẫn.

Authentication cần xác định người dùng là ai, trong khi authorization xác định người dùng đó được phép thực hiện những thao tác nào.

Ví dụ:

* Customer có thể tạo và xem trạng thái đơn liên quan đến bàn.
* Kitchen có thể xem đơn hàng và cập nhật trạng thái chế biến.
* Manager có thể xem Dashboard và dữ liệu thống kê.

Trong kiến trúc hiện tại, CloudMenu chưa bắt buộc sử dụng một hệ thống account management phức tạp. Điều này giúp giữ project đơn giản trong phạm vi Workshop.

Tuy nhiên, khi yêu cầu về bảo mật tăng lên, Amazon Cognito có thể được tích hợp.

### Định hướng sử dụng Amazon Cognito

Amazon Cognito là dịch vụ managed authentication của AWS và có thể được sử dụng để cung cấp hệ thống tài khoản cho CloudMenu.

Một kiến trúc mở rộng có thể sử dụng:

* **User Pool:** quản lý người dùng, đăng ký và đăng nhập.
* **App Client:** cho phép frontend tương tác với Cognito User Pool.
* **JWT Token:** được cấp sau khi người dùng đăng nhập thành công.
* **Groups hoặc roles:** phân biệt Kitchen, Manager hoặc các vai trò khác.
* **API authorization:** kiểm tra token trước khi cho phép truy cập các API được bảo vệ.

Luồng xác thực có thể được mô tả như sau:

**Kitchen/Manager → Cognito → JWT Token → API Gateway → Lambda**

Sau khi đăng nhập thành công, frontend nhận token từ Cognito.

Token có thể được gửi kèm các API request, sau đó backend hoặc API Gateway sử dụng thông tin xác thực để quyết định request có được phép thực hiện hay không.

### Lý do lựa chọn Amazon Cognito trong tương lai

Amazon Cognito phù hợp với hướng mở rộng của CloudMenu vì:

* Là dịch vụ authentication được AWS quản lý.
* Giảm nhu cầu tự xây dựng toàn bộ login system.
* Hỗ trợ token-based authentication.
* Có thể tích hợp với frontend và API Gateway.
* Có khả năng quản lý nhiều user.
* Hỗ trợ mô hình role/group khi hệ thống cần phân quyền.

Tuy nhiên, Cognito không nên được thêm chỉ để tăng số lượng AWS services trong kiến trúc.

Trong giai đoạn hiện tại, nếu CloudMenu chưa có yêu cầu quản lý account thực tế, việc triển khai Cognito có thể được giữ lại cho giai đoạn sau.

### Phạm vi bảo mật hiện tại

Ngay cả khi chưa sử dụng Cognito, CloudMenu vẫn cần duy trì một số nguyên tắc bảo mật cơ bản:

* Frontend không được lưu AWS Access Key hoặc Secret Access Key.
* Frontend không được truy cập trực tiếp DynamoDB.
* Database operations phải thông qua API Gateway và Lambda.
* Lambda sử dụng IAM Role để truy cập DynamoDB.
* S3 permissions cần được giới hạn phù hợp.
* Nếu sử dụng CloudFront với S3 private origin, có thể cấu hình OAC.
* Các API nội bộ cần được xem xét bổ sung authentication trước khi đưa hệ thống vào production.

Điều này giúp phân biệt rõ hai lớp bảo mật:

**IAM** kiểm soát quyền giữa các AWS services.

**Authentication/Authorization** kiểm soát quyền của người dùng ứng dụng.

Hai cơ chế này phục vụ hai mục đích khác nhau và không thể thay thế hoàn toàn cho nhau.

### Định hướng mở rộng

Trong giai đoạn phát triển tiếp theo, kiến trúc frontend và authentication của CloudMenu có thể được mở rộng theo hướng:

**User → CloudFront → S3**

đối với frontend, và:

**Authenticated User → Cognito → API Gateway → Lambda → DynamoDB**

đối với các chức năng yêu cầu xác thực.

Customer có thể tiếp tục sử dụng trải nghiệm truy cập đơn giản thông qua QR Code, trong khi Kitchen và Manager có thể được yêu cầu đăng nhập trước khi truy cập các chức năng nội bộ.

Cách tiếp cận này giúp CloudMenu giữ được trải nghiệm đơn giản cho khách hàng nhưng vẫn có khả năng tăng cường bảo mật cho các chức năng quản trị khi hệ thống phát triển.
