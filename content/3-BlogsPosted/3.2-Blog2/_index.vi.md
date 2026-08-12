---

title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
----------------------

# XÂY DỰNG AI ASSISTANT ĐA GIAO DIỆN VỚI AMAZON Q BUSINESS VÀ SLACK TRÊN AWS

Trong doanh nghiệp, thông tin thường nằm rải rác ở nhiều nơi như tài liệu nội bộ, hướng dẫn, chính sách hay các cuộc trao đổi trên Slack. Khi cần tìm một thông tin cụ thể, nhân viên có thể phải mở nhiều tài liệu hoặc tìm lại hàng loạt tin nhắn cũ.

Trong quá trình tìm hiểu về Generative AI trên AWS, mình thấy một giải pháp khá thú vị: xây dựng **AI Assistant đa giao diện với Amazon Q Business và Slack**, giúp người dùng tìm kiếm kiến thức nội bộ ngay từ những công cụ họ đang sử dụng.

Key points to know:

* Người dùng có thể đặt câu hỏi thông qua **Amazon Q Business** hoặc trực tiếp trong **Slack**, nhưng cả hai giao diện đều có thể khai thác nguồn kiến thức của tổ chức.
* Hệ thống ứng dụng **Retrieval Augmented Generation (RAG)**. Thay vì chỉ dựa vào kiến thức có sẵn của AI, hệ thống tìm kiếm thông tin liên quan trong dữ liệu doanh nghiệp và sử dụng thông tin đó làm ngữ cảnh để tạo câu trả lời.
* **Amazon S3** được sử dụng để lưu trữ tài liệu và dữ liệu phục vụ hệ thống.
* **Amazon Kendra** hỗ trợ tìm kiếm và truy xuất những thông tin liên quan từ nguồn kiến thức của doanh nghiệp.
* Người dùng có thể tương tác với AI Assistant thông qua nhiều giao diện, bao gồm **Amazon Q Business** và **Slack**.
* Dữ liệu hội thoại từ Slack có thể được xử lý bằng **AWS Lambda** và tự động đưa vào nguồn kiến thức theo lịch với **Amazon EventBridge**.
* Giải pháp hỗ trợ **clickable references**, cho phép người dùng truy cập tài liệu nguồn được AI sử dụng khi tạo câu trả lời.
* **Amazon CloudFront** hỗ trợ phân phối các tài liệu được lưu trữ trong Amazon S3 để các reference có thể được truy cập thuận tiện.
* **Amazon CloudWatch** được sử dụng để theo dõi hoạt động của hệ thống sau khi triển khai.
* **User feedback** cũng có thể được thu thập để đánh giá và cải thiện chất lượng câu trả lời của AI Assistant.

Một điểm đáng chú ý của kiến trúc này không chỉ là việc xây dựng thêm một chatbot, mà là **đưa AI đến nơi người dùng đang làm việc thay vì yêu cầu người dùng phải tìm đến một giao diện AI riêng biệt**.

AI Assistant có thể khai thác dữ liệu nội bộ, hoạt động trên nhiều giao diện và quan trọng hơn là cung cấp reference để người dùng có thể kiểm tra lại nguồn thông tin được sử dụng trong câu trả lời.

Điều này cho thấy khi kết hợp **Amazon Q Business, Slack, Amazon Kendra, Amazon S3 và Amazon CloudFront**, Generative AI có thể được tích hợp trực tiếp vào workflow của doanh nghiệp, giúp việc tìm kiếm và truy cập kiến thức nội bộ trở nên nhanh chóng và thuận tiện hơn.

![Multi-interface AI Assistant Architecture](/images/3-Blogs%20posted/AI%20Assistant.png)

*Kiến trúc AI Assistant đa giao diện với Amazon Q Business và Slack trên AWS*

[Build a multi-interface AI assistant using Amazon Q and Slack with Amazon CloudFront clickable references from an Amazon S3 bucket](https://aws.amazon.com/vi/blogs/machine-learning/build-a-multi-interface-ai-assistant-using-amazon-q-and-slack-with-amazon-cloudfront-clickable-references-from-an-amazon-s3-bucket/)
