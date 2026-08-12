---

title: "Frontend hosting"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
aliases:

- /5-workshop/5.3-deployment-operations-monitoring/5.3.2-frontend-hosting-auth/

---

## 5.3.2. Frontend hosting

### Tổng quan

CloudMenu sử dụng **Amazon S3** để lưu trữ các file frontend và **Amazon CloudFront** để phân phối nội dung đến người dùng thông qua Internet.

Frontend của hệ thống được xây dựng dưới dạng website tĩnh gồm HTML, CSS, JavaScript, hình ảnh và các static assets cần thiết. Do đó, CloudMenu không cần duy trì một web server hoặc EC2 instance riêng chỉ để phục vụ giao diện người dùng.

Trong kiến trúc hiện tại, Amazon S3 đóng vai trò lưu trữ các file frontend, trong khi Amazon CloudFront đóng vai trò là lớp phân phối nội dung phía trước S3. Người dùng truy cập CloudMenu thông qua CloudFront domain thay vì chạy ứng dụng trực tiếp trên máy phát triển.

CloudMenu hiện bao gồm ba giao diện chính:

* **Customer Interface:** giao diện dành cho khách hàng quét QR Code tại bàn, xem menu, lựa chọn món, quản lý giỏ hàng, gửi đơn và theo dõi trạng thái xử lý.
* **Kitchen Interface:** giao diện dành cho nhân viên bếp để theo dõi các đơn hàng mới và cập nhật trạng thái trong quá trình chế biến.
* **Manager Dashboard:** giao diện dành cho quản lý để theo dõi tổng quan dữ liệu đơn hàng, trạng thái xử lý, doanh thu và các thông tin thống kê.

Mặc dù ba giao diện phục vụ các nhóm người dùng khác nhau, chúng được triển khai chung dưới dạng các static frontend files và được phân phối thông qua cùng mô hình S3 + CloudFront.

### Kiến trúc Frontend Hosting

Kiến trúc frontend của CloudMenu được triển khai theo mô hình:

![Kiến trúc frontend](/images/5-Workshop/AWS_CloudMenu_Frontend.png)

Luồng truy cập cơ bản có thể được mô tả như sau:

**Customer/Kitchen/Manager → Browser → Amazon CloudFront → Amazon S3**

Khi người dùng truy cập CloudFront domain, trình duyệt gửi HTTPS request đến Amazon CloudFront.

CloudFront kiểm tra nội dung được yêu cầu và cung cấp các file frontend đến người dùng. Khi nội dung chưa có trong cache tại edge location tương ứng, CloudFront lấy file từ Amazon S3 origin và sau đó phân phối lại đến trình duyệt.

Các file được lưu trữ trên S3 có thể bao gồm:

* HTML.
* CSS.
* JavaScript.
* Hình ảnh món ăn.
* Icon và các static assets khác.
* Các giao diện Customer, Kitchen và Manager.

Nhờ mô hình này, frontend CloudMenu không phụ thuộc vào máy tính đang phát triển ứng dụng. Developer không cần mở Visual Studio Code hoặc chạy local server để người dùng cuối có thể truy cập hệ thống.

### Vai trò của Amazon S3

Amazon S3 được sử dụng làm nơi lưu trữ các frontend assets của CloudMenu.

Sau khi frontend được hoàn thiện hoặc cập nhật, các file cần thiết được upload vào S3 bucket tương ứng.

Trong phạm vi hiện tại, S3 chịu trách nhiệm lưu trữ các thành phần như:

```text
index.html
kitchen.html
manager.html
css/
js/
images/
```

Tên file và cấu trúc thư mục thực tế phụ thuộc vào cách frontend CloudMenu được tổ chức.

Việc sử dụng S3 phù hợp với frontend dạng static vì không yêu cầu xử lý server-side đối với các file HTML, CSS và JavaScript.

### Vai trò của Amazon CloudFront

Amazon CloudFront được đặt phía trước Amazon S3 và đóng vai trò phân phối nội dung đến người dùng.

Thay vì yêu cầu người dùng truy cập trực tiếp vào S3, CloudMenu sử dụng CloudFront domain làm địa chỉ truy cập frontend.

Một số lợi ích của việc sử dụng CloudFront gồm:

* Phân phối nội dung thông qua hệ thống edge location.
* Hỗ trợ HTTPS cho kết nối từ người dùng đến frontend.
* Giảm số lần phải lấy cùng một nội dung từ S3 nhờ cơ chế caching.
* Tách lớp lưu trữ frontend khỏi địa chỉ người dùng trực tiếp truy cập.
* Có thể mở rộng trong tương lai với custom domain và các cấu hình bảo mật bổ sung.

Trong CloudMenu, CloudFront đóng vai trò là lớp phân phối frontend, trong khi các request liên quan đến dữ liệu đơn hàng được JavaScript phía trình duyệt gửi riêng đến Amazon API Gateway.

Do đó, hai luồng cần được phân biệt:

**Frontend content:**

**Browser → CloudFront → S3**

**Application data:**

**Browser → API Gateway → Lambda → DynamoDB**

CloudFront và S3 không trực tiếp xử lý logic tạo đơn hoặc cập nhật trạng thái đơn hàng.

### Luồng triển khai Frontend

Hiện tại CloudMenu **chưa triển khai CI/CD tự động cho frontend**.

Source code của project được quản lý trong repository, nhưng quá trình đưa phiên bản frontend mới lên Amazon S3 vẫn được thực hiện thủ công trong phạm vi Workshop.

Luồng triển khai hiện tại được mô tả như sau:

![Luồng triển khai Frontend](/images/5-Workshop/AWS_CloudMenu_Frontend2.png)

Quy trình có thể được tóm tắt:

**Developer → Source Code/GitHub → Local Development → Manual Upload → Amazon S3 → Amazon CloudFront → User**

Sau khi developer hoàn thành hoặc cập nhật frontend, các file tương ứng được kiểm tra tại môi trường local trước khi upload lên S3.

Sau khi nội dung mới được upload, CloudFront tiếp tục phân phối frontend đến người dùng thông qua CloudFront domain.

Trong trường hợp các file đã được CloudFront cache trước đó, có thể cần chờ cache hết hạn hoặc thực hiện invalidation phù hợp để người dùng nhận phiên bản mới.

### Lý do sử dụng triển khai thủ công trong Workshop

Việc deployment frontend thủ công được lựa chọn trong giai đoạn hiện tại vì phạm vi CloudMenu chủ yếu tập trung vào việc hiểu và triển khai luồng end-to-end trên AWS.

Cách tiếp cận này có một số ưu điểm trong môi trường development và Workshop:

* Quy trình đơn giản và dễ quan sát.
* Developer có thể kiểm tra từng bước triển khai.
* Không yêu cầu thiết lập thêm pipeline CI/CD.
* Phù hợp với số lượng lần deployment chưa quá lớn.
* Giúp tập trung vào các AWS service cốt lõi của hệ thống.

Tuy nhiên, deployment thủ công cũng có những hạn chế:

* Phải upload lại file sau mỗi lần frontend thay đổi.
* Có khả năng bỏ sót file khi deploy.
* Có thể upload nhầm phiên bản frontend.
* Không có bước kiểm tra tự động trước khi deployment.
* Chưa tự động hóa việc rollback về phiên bản trước.
* Quy trình trở nên khó quản lý hơn khi số lần release tăng.

Trong giai đoạn mở rộng, CloudMenu có thể triển khai CI/CD để tự động build, kiểm tra và đồng bộ frontend từ source repository lên Amazon S3.

### Kiểm tra Frontend sau khi triển khai

Sau khi các frontend files được upload lên Amazon S3 và CloudFront đã phân phối nội dung mới, hệ thống có thể được kiểm tra thông qua CloudFront domain.

Ví dụ:

https://d3be9t7i3323e7.cloudfront.net/index.html?table=02

Query parameter:

```text
?table=02
```

được sử dụng để truyền thông tin bàn vào Customer Interface trong quá trình truy cập thông qua QR Code hoặc URL tương ứng.

### Kiểm tra Customer Interface

Customer Interface cần được kiểm tra bằng URL chứa thông tin bàn.

![Customer](/images/5-Workshop/AWS_CloudMenu_Customer.png)

Các chức năng cần xác nhận gồm:

* Trang menu được tải thành công qua CloudFront.
* Hệ thống nhận diện đúng số bàn.
* Danh sách món ăn hiển thị bình thường.
* Khách hàng có thể thêm hoặc xóa món khỏi giỏ hàng.
* Tổng tiền được cập nhật phù hợp.
* Đơn hàng có thể được gửi tới backend.
* Trạng thái đơn có thể được hiển thị sau khi tạo đơn.

Việc kiểm tra Customer Interface giúp xác nhận frontend không chỉ được phân phối thành công mà còn có thể giao tiếp với backend API của CloudMenu.

### Kiểm tra Kitchen Interface

Kitchen Interface được sử dụng để xác nhận các đơn do Customer gửi đã được backend xử lý và có thể hiển thị cho nhân viên bếp.

![Kitchen](/images/5-Workshop/AWS_CloudMenu_Kitchen.png)

Một số nội dung cần kiểm tra:

* Danh sách đơn hàng được tải thành công.
* Thông tin bàn được hiển thị chính xác.
* Các món và số lượng trong đơn được hiển thị đầy đủ.
* Trạng thái đơn hàng được hiển thị đúng.
* Kitchen có thể thực hiện thao tác cập nhật trạng thái.
* Sau khi cập nhật, dữ liệu mới được phản ánh trên backend.

### Kiểm tra Manager Dashboard

Manager Dashboard được sử dụng để kiểm tra khả năng tổng hợp và trình bày dữ liệu cho người quản lý.

![Manager](/images/5-Workshop/AWS_CloudMenu_Manager.png)

Các thông tin có thể được kiểm tra gồm:

* Tổng số đơn hàng.
* Tổng doanh thu.
* Phân loại đơn theo trạng thái.
* Dữ liệu liên quan đến các bàn.
* Các món được gọi nhiều.
* Các dữ liệu thống kê khác được xây dựng từ thông tin đơn hàng.

Việc Manager Dashboard hiển thị đúng dữ liệu giúp xác nhận frontend có thể đọc dữ liệu backend và thực hiện các phép tổng hợp cần thiết cho giao diện quản lý.

### Kiểm tra kết nối Frontend và Backend

Hosting frontend chỉ được xem là hoàn chỉnh khi giao diện được phân phối thành công **và** frontend có thể giao tiếp với backend.

Luồng kiểm tra end-to-end có thể mô tả như sau:

**CloudFront Frontend → API Gateway → Lambda → DynamoDB**

Ví dụ:

1. Customer mở menu từ CloudFront.
2. Customer tạo một đơn hàng.
3. Frontend gửi request tới API Gateway.
4. Lambda xử lý và ghi đơn vào DynamoDB.
5. Kitchen tải danh sách đơn và thấy đơn vừa tạo.
6. Kitchen cập nhật trạng thái.
7. Customer hoặc Manager nhận dữ liệu trạng thái mới.

Nếu các bước trên hoạt động bình thường, frontend hosting và quá trình tích hợp với backend đã được triển khai thành công.

### Kết quả

Sau khi hoàn thành phần này, các giao diện Customer, Kitchen và Manager của CloudMenu có thể được truy cập thông qua Internet bằng CloudFront domain thay vì phụ thuộc vào local development server.

Amazon S3 đảm nhiệm lưu trữ frontend, trong khi Amazon CloudFront cung cấp lớp phân phối nội dung tới người dùng.

Thiết kế này phù hợp với kiến trúc serverless tổng thể của CloudMenu và tạo nền tảng để thực hiện các bước kiểm thử end-to-end, monitoring và tối ưu hệ thống ở các phần tiếp theo.
