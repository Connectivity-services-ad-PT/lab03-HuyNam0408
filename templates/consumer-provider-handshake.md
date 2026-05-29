# Biên bản bắt tay kỹ thuật — Consumer-Provider Handshake

Dựa trên bản đồ phụ thuộc (Dependency Map) của nền tảng Smart Campus Operations Platform, dưới đây là đặc tả chi tiết các mối quan hệ tích hợp liên thông của dịch vụ **Camera Stream**.

---

## 1. Mối quan hệ giữa Camera Stream (Consumer) và AI Vision (Provider)

* **Bên yêu cầu (Consumer):** `team-camera` (Dịch vụ Camera Stream)
* **Bên cung cấp (Provider):** `team-vision` (Dịch vụ AI Vision)
* **Mục đích:** Gửi các khung hình (frames) thu được từ camera khi phát hiện chuyển động (motion) sang để phân tích và nhận diện đối tượng.
* **Cơ chế tích hợp:** `REST sync` (Đồng bộ qua HTTP REST API)
* **Kịch bản kiểm thử liên thông (Consumer-side Smoke Test):**
  - Đã được tự động hóa tại file cấu hình kiểm thử `05_Consumer_side_Smoke.js`.
  - Thực hiện gọi giả lập sang Mock Server của dịch vụ AI Vision chạy tại cổng `4011` (`{{aiVisionMockUrl}}`).
  - Điểm tiếp nhận ký kết: `POST /api/v1/vision/detect`.
  - Kết quả kỳ vọng: Mã phản hồi `200 OK` thông suốt.

---

## 2. Mối quan hệ giữa Camera Stream (Consumer) và Analytics (Provider)

* **Bên yêu cầu (Consumer):** `team-camera` (Dịch vụ Camera Stream)
* **Bên cung cấp (Provider):** `team-analytics` (Dịch vụ Analytics)
* **Mục đích:** Đẩy dữ liệu sự kiện (event camera) về cho phân hệ Analytics để phục vụ việc tổng hợp dữ liệu (aggregate) và tính toán chỉ số thống kê.
* **Cơ chế tích hợp:** `Queue async` (Bất đồng bộ qua hàng đợi tin nhắn / Message Queue)
* **Kịch bản kiểm thử biên và độ tin cậy:**
  - Đã được bao phủ trong luồng xử lý tại file `04_Boundary_Reliability.js`.
  - Khi hệ thống truyền dữ liệu bất đồng bộ ở ngưỡng ranh giới tải cao, API trả về trạng thái phản hồi `202 Accepted` để đảm bảo hàng đợi không bị nghẽn mạch.