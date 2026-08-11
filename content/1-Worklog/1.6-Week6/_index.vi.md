---
title: "Worklog Tuần 6"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6

- Phân tích yêu cầu và các chức năng chính của hệ thống CloudMenu.
- Thiết kế quy trình gọi món tại bàn bằng mã QR.
- Xây dựng giao diện khách hàng để xem thực đơn, chọn món và gửi đơn gọi món.
- Xây dựng giao diện bếp để theo dõi và cập nhật trạng thái đơn hàng.

**Thời gian:** 27/07/2026 - 31/07/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Phân tích yêu cầu hệ thống **CloudMenu** <br> + Xác định các đối tượng sử dụng chính: Customer, Kitchen và Manager/Admin <br> + Xác định các chức năng cần triển khai <br> + Phân tích quy trình gọi món từ khách hàng đến bếp | 27/07/2026 | 27/07/2026 | - |
| 2 | - Thiết kế chức năng gọi món bằng **QR Code** <br> + Mỗi bàn sử dụng một đường dẫn QR riêng <br> + Sử dụng tham số `table` trên URL để xác định số bàn <br> + Kiểm tra và xác thực số bàn trước khi hiển thị thực đơn | 28/07/2026 | 28/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) |
| 3 | - Xây dựng giao diện khách hàng của **CloudMenu** <br> + Hiển thị danh sách món ăn <br> + Tìm kiếm và lọc món theo danh mục <br> + Thêm món vào giỏ hàng và thay đổi số lượng <br> + Tính tổng giá trị đơn hàng | 29/07/2026 | 29/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) |
| 4 | - Hoàn thiện chức năng gửi và theo dõi đơn hàng <br> + Gửi thông tin đơn từ frontend đến API <br> + Gắn số bàn vào đơn hàng <br> + Hiển thị thông tin và trạng thái đơn cho khách hàng <br> + Xử lý các trạng thái `PENDING`, `PREPARING` và `COMPLETED` | 30/07/2026 | 30/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) |
| 5 | - Xây dựng giao diện **Kitchen** <br> + Hiển thị danh sách đơn hàng <br> + Hiển thị số bàn, món ăn, số lượng và tổng tiền <br> + Cập nhật trạng thái từ `PENDING` sang `PREPARING` và `COMPLETED` <br> + Kiểm thử quy trình Khách hàng → Gửi đơn → Bếp nhận đơn → Hoàn thành đơn | 31/07/2026 | 31/07/2026 | - |

---

### Thành tựu Tuần 6

- Hoàn thành bước phân tích yêu cầu và xác định các chức năng chính của hệ thống CloudMenu.
- Xây dựng được cơ chế nhận diện bàn thông qua mã QR và tham số `table` trên URL.
- Xây dựng giao diện khách hàng với các chức năng xem thực đơn, tìm kiếm, lọc món và quản lý giỏ hàng.
- Hoàn thiện chức năng gửi đơn và theo dõi các trạng thái `PENDING`, `PREPARING` và `COMPLETED`.
- Xây dựng giao diện Kitchen để nhân viên bếp xem và cập nhật trạng thái đơn hàng.
- Hoàn thiện luồng nghiệp vụ chính từ khách hàng gọi món đến bếp tiếp nhận và hoàn thành đơn.