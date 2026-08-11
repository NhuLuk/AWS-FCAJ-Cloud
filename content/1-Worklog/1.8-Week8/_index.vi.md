---
title: "Worklog Tuần 8"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu Tuần 8

- Hoàn thiện các chức năng và giao diện của hệ thống CloudMenu.
- Xây dựng Dashboard thống kê và hoàn thiện quá trình kiểm thử hệ thống.
- Hoàn thiện các sơ đồ kiến trúc, luồng hoạt động và tài liệu kỹ thuật của dự án.
- Tổng kết quá trình thực tập và chuẩn bị báo cáo cuối kỳ.

**Thời gian:** 10/08/2026 - 15/08/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Xây dựng và hoàn thiện **Dashboard CloudMenu** <br> + Hiển thị tổng số đơn hàng và tổng doanh thu <br> + Thống kê số đơn theo trạng thái <br> + Thống kê doanh thu theo bàn <br> + Thống kê các món ăn được gọi nhiều nhất | 10/08/2026 | 10/08/2026 | - |
| 2 | - Hoàn thiện chức năng theo dõi thời gian đơn hàng <br> + Lưu thời gian tạo đơn `createdAt` <br> + Cập nhật thời gian thay đổi trạng thái `updatedAt` <br> + Lưu thời gian hoàn thành đơn `completedAt` <br> + Hiển thị thời gian và trạng thái phù hợp trên giao diện Kitchen | 11/08/2026 | 11/08/2026 | - |
| 3 | - Kiểm thử và rà soát toàn bộ hệ thống **CloudMenu** <br> + Kiểm thử chức năng gọi món bằng QR theo từng bàn <br> + Kiểm tra luồng Customer → API Gateway → Lambda → DynamoDB → Kitchen <br> + Kiểm tra việc cập nhật trạng thái đơn hàng <br> + Sửa các lỗi giao diện, API và dữ liệu phát sinh trong quá trình kiểm thử | 12/08/2026 | 12/08/2026 | - |
| 4 | - Hoàn thiện tài liệu và các sơ đồ của dự án <br> + System Architecture Diagram <br> + Order Workflow Diagram <br> + Use Case Diagram <br> + Data Model Diagram <br> + Deployment/Request Flow Diagram <br> + Rà soát README và tài liệu Workshop | 13/08/2026 | 13/08/2026 | [https://aws.amazon.com/architecture/](https://aws.amazon.com/architecture/) |
| 5 | - Hoàn thiện và tổng kết dự án **CloudMenu** <br> + Rà soát các dịch vụ AWS đã sử dụng <br> + Kiểm tra nội dung Worklog, Proposal và Workshop <br> + Tổng hợp kết quả đạt được và các hạn chế hiện tại của hệ thống <br> + Chuẩn bị báo cáo và thuyết trình cuối kỳ | 14/08/2026 | 14/08/2026 | - |

---

### Thành tựu Tuần 8

- Hoàn thiện Dashboard thống kê số lượng đơn hàng, doanh thu, trạng thái đơn, doanh thu theo bàn và món ăn được gọi nhiều.
- Hoàn thiện việc lưu trữ và hiển thị các thông tin thời gian của đơn hàng.
- Kiểm thử và hoàn thiện toàn bộ luồng hoạt động của hệ thống CloudMenu từ khách hàng đến bếp.
- Hoàn thiện các sơ đồ kiến trúc, luồng xử lý, mô hình dữ liệu và Use Case của hệ thống.
- Hoàn thiện README, Worklog, Proposal, Workshop và các tài liệu kỹ thuật liên quan.
- Tổng kết quá trình xây dựng CloudMenu và những kiến thức, kỹ năng đạt được trong suốt chương trình AWS First Cloud Journey.