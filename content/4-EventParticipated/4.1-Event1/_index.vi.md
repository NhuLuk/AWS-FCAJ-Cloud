---
title: "AgentForge Day 2"
date: 2026-08-08
weight: 1
chapter: false
pre: "<b>4.1. </b>"
---

# Bài thu hoạch “AgentForge Day 2 – Advanced Amazon Bedrock AgentCore”

**Thời gian tham gia:** 08/08/2026  
**Chủ đề:** Personalization, Evaluation & Optimization  
**Nội dung chính:** Advanced Amazon Bedrock AgentCore

---

### Tổng quan sự kiện

Ngày **08/08/2026**, tôi có cơ hội tham gia **AgentForge Day 2**, với chủ đề **Personalization, Evaluation & Optimization**. Đây là một sự kiện giúp tôi tiếp cận thêm các kiến thức liên quan đến việc xây dựng và vận hành AI Agent trên nền tảng AWS.

Nội dung chính của chương trình tập trung vào **Advanced Amazon Bedrock AgentCore**, đặc biệt là các khả năng hỗ trợ AI Agent trong quá trình duy trì ngữ cảnh, theo dõi hoạt động, đánh giá hiệu suất và tối ưu cách Agent vận hành.

Chương trình được chia thành hai phần chính:

- Phiên chia sẻ về **Amazon Bedrock AgentCore** từ **09:00 – 10:00**.
- Phiên **Hands-on Lab** từ **10:00 – 11:00**, tập trung vào việc tìm hiểu và thực hành một số khả năng của AgentCore.

Trong phiên đầu tiên, người tham gia được giới thiệu về các nội dung nâng cao của AgentCore như **Memory, Evaluations và Observability**. Ngoài ra, chương trình còn đề cập đến một số thành phần và khái niệm khác gồm **Registry, Harness, Tools, Payments, Optimization và Policy**.

Phần Hands-on Lab giúp người tham gia có cơ hội tiếp cận thực tế hơn với một số nội dung đã được trình bày, bao gồm thêm Memory để hỗ trợ hành vi Agent có tính cá nhân hóa, tìm hiểu Agent Observability, sử dụng AgentCore Evaluations để đánh giá hiệu suất Agent và khám phá AgentCore Harness.

Thông qua sự kiện, tôi hiểu rõ hơn rằng việc xây dựng một AI Agent không chỉ dừng lại ở khả năng nhận yêu cầu và tạo phản hồi. Khi đưa Agent vào các ứng dụng thực tế, cần quan tâm đến khả năng duy trì thông tin ngữ cảnh, theo dõi quá trình hoạt động, đánh giá chất lượng phản hồi và liên tục tối ưu Agent.

![Tham gia AgentForge Day 2](/images/4-EventParticipated/4.1-Event1/agentforge-day2.png)

*Hình 1. Hình ảnh trong quá trình tham gia AgentForge Day 2 ngày 08/08/2026.*

---

### Nội dung chính của sự kiện

#### Amazon Bedrock AgentCore

Phần đầu của chương trình giới thiệu các nội dung nâng cao liên quan đến **Amazon Bedrock AgentCore**.

Trong đó, ba nội dung được tập trung giới thiệu gồm:

- **Memory**
- **Evaluations**
- **Observability**

Các khả năng này hỗ trợ những khía cạnh khác nhau trong vòng đời của một AI Agent. Memory hỗ trợ Agent duy trì thông tin cần thiết từ các tương tác, Observability hỗ trợ theo dõi quá trình hoạt động, trong khi Evaluations cung cấp phương pháp để đánh giá hiệu quả của Agent.

Qua phần trình bày này, tôi có cái nhìn rõ hơn về quá trình phát triển AI Agent theo hướng hoàn chỉnh hơn, không chỉ tập trung vào việc xây dựng logic ban đầu mà còn phải quan tâm đến quá trình vận hành và cải tiến Agent sau đó.

---

#### AgentCore Memory

Một trong những nội dung tôi quan tâm trong sự kiện là **AgentCore Memory**.

Memory hỗ trợ Agent duy trì và sử dụng thông tin từ các tương tác để tạo ra trải nghiệm có tính cá nhân hóa hơn. Thay vì mỗi request đều được xử lý như một tương tác hoàn toàn độc lập, Agent có thể sử dụng các thông tin ngữ cảnh phù hợp để đưa ra phản hồi sát với nhu cầu của người dùng hơn.

Trong phần Hands-on Lab, người tham gia được tìm hiểu cách:

- Thêm Memory cho Agent.
- Sử dụng Memory để hỗ trợ hành vi Agent có tính cá nhân hóa.
- Quan sát cách thông tin ngữ cảnh có thể ảnh hưởng đến quá trình tương tác của Agent.

Qua nội dung này, tôi hiểu rõ hơn vai trò của Memory đối với các ứng dụng AI Agent cần tương tác với người dùng trong nhiều phiên hoặc nhiều bước khác nhau.

Memory cũng giúp tôi nhận ra sự khác biệt giữa một hệ thống chỉ xử lý từng câu hỏi riêng biệt và một Agent có khả năng sử dụng thông tin từ quá trình tương tác để tạo ra trải nghiệm phù hợp hơn.

---

#### AgentCore Evaluations

Một nội dung quan trọng khác của chương trình là **AgentCore Evaluations**.

Trong quá trình phát triển AI Agent, việc Agent có thể tạo ra câu trả lời chưa đủ để kết luận hệ thống đang hoạt động hiệu quả. Nhà phát triển còn cần có phương pháp để đánh giá chất lượng và hiệu suất của Agent trong các trường hợp sử dụng khác nhau.

AgentCore Evaluations hỗ trợ quá trình đánh giá này, giúp người phát triển có thêm cơ sở để xác định Agent đang hoạt động như thế nào và những phần nào cần tiếp tục được cải thiện.

Trong phần Hands-on Lab, người tham gia được tìm hiểu cách sử dụng **AgentCore Evaluations** để đo lường hiệu suất của Agent.

Qua nội dung này, tôi hiểu rằng việc phát triển AI Agent có thể được xem như một quá trình liên tục:

```text
Build Agent
     ↓
Run Agent
     ↓
Observe Behavior
     ↓
Evaluate Performance
     ↓
Optimize Agent