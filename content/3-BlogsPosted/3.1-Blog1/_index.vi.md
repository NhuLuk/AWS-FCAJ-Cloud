---

title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
----------------------

# BIẾN HÀNG TRIỆU HỢP ĐỒNG THÀNH DỮ LIỆU HỮU ÍCH VỚI DOCZY.AI VÀ GENERATIVE AI TRÊN AWS

Trong các doanh nghiệp, đặc biệt ở lĩnh vực y tế và tài chính, rất nhiều thông tin quan trọng vẫn nằm trong những tài liệu phi cấu trúc như hợp đồng, thỏa thuận pháp lý hay hóa đơn.

Một hợp đồng có thể dài hàng chục hoặc hàng trăm trang với cấu trúc và điều khoản khác nhau. Khi số lượng tài liệu tăng lên hàng nghìn hoặc hàng triệu, việc đọc, trích xuất và kiểm tra thủ công vừa tốn thời gian vừa khó mở rộng.

Trong quá trình tìm hiểu các ứng dụng thực tế của Generative AI trên AWS, mình thấy một case khá thú vị: **Doczy.ai của AArete** – nền tảng sử dụng Generative AI và các dịch vụ AWS để tự động hóa quá trình phân tích hợp đồng.

Key points to know:

* Trước đây, AArete sử dụng phương pháp dựa trên rule để xử lý hợp đồng và đạt khoảng **55% accuracy** trong giai đoạn 2020–2023. Sau khi chuyển sang giải pháp AI trên AWS vào năm 2024, Doczy.ai đạt mức **99% accuracy**.
* Doczy.ai sử dụng kỹ thuật **Smart Chunking** để duy trì cấu trúc, ngữ cảnh và mối quan hệ giữa các phần của hợp đồng thay vì chỉ chia tài liệu thành những đoạn text cố định.
* Hệ thống kết hợp **semantic analysis** để hiểu ý nghĩa nội dung và **structural analysis** để nhận diện cấu trúc của tài liệu.
* **Amazon Cognito** hỗ trợ xác thực người dùng, trong khi **Amazon S3** được sử dụng để lưu trữ tài liệu.
* **AWS Lambda** kích hoạt quá trình xử lý và **Amazon Textract** thực hiện trích xuất nội dung từ tài liệu.
* Các workload tiếp theo được xử lý với **Amazon ECS**, trong khi **Amazon Bedrock** cung cấp khả năng Generative AI.
* Kết quả được đưa vào **Snowflake** để phục vụ phân tích dữ liệu; **AWS Secrets Manager** và **Amazon CloudWatch** hỗ trợ bảo mật và giám sát hệ thống.
* Trong khoảng 22 tháng, Doczy.ai đã xử lý **2,5 triệu tài liệu**, tương đương khoảng **50 triệu trang**, thực hiện **137 triệu API calls** tới Amazon Bedrock và xử lý khoảng **442 tỷ tokens**.
* Hệ thống có khả năng xử lý tới **250.000 tài liệu mỗi tuần**, đạt khoảng **99% accuracy** và giảm **97% thời gian xử lý thủ công**.
* Theo AWS, giải pháp đã mang lại khoảng **330 triệu USD tổng mức tiết kiệm trực tiếp và gián tiếp** cho khách hàng.

Điểm đáng chú ý của Doczy.ai là Generative AI không hoạt động độc lập. Amazon Bedrock chỉ là một phần trong một pipeline hoàn chỉnh gồm authentication, storage, document processing, compute, AI, data và monitoring.

Điều này cho thấy Generative AI không chỉ được sử dụng để xây chatbot hay tạo nội dung. Khi kết hợp với kiến trúc phù hợp, **Amazon Bedrock, Amazon Textract và các dịch vụ AWS khác có thể biến hàng triệu trang tài liệu phi cấu trúc thành dữ liệu hữu ích cho doanh nghiệp.**

![Architecture of Doczy.ai](/images/3-Blogs%20posted/Architecture%20of%20Doczy.ai.png)

*Kiến trúc Doczy.ai trên AWS*

[Automating contract intelligence with Doczy.ai™ on AWS](https://aws.amazon.com/vi/blogs/architecture/automating-contract-intelligence-with-doczy-ai-on-aws/)
