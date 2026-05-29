# Reliability Checklist — FIT4110 Lab 03

Điền checklist này trước khi nộp Lab 03.

## 1. Functional tests

- [x] Có test cho endpoint health. (Cấu hình kiểm tra trạng thái hoạt động của hệ thống)
- [x] Có test happy path cho endpoint chính. (Đã viết tại `01_Functional.js` cho API `/api/v1/camera/frames`)
- [x] Có kiểm tra status code 2xx. (Đã dùng `pm.expect([200, 201]).to.include(...)`)
- [x] Có kiểm tra field quan trọng trong response. (Đã kiểm tra trường `status` trong dữ liệu trả về)
- [x] Có ít nhất 1 test đọc dữ liệu danh sách hoặc chi tiết. (Đã viết test lấy cấu hình thiết bị tại endpoint `/api/v1/camera/devices`)

## 2. Auth tests

- [x] Có test thiếu token. (Đã cấu hình chặn tại file `02_Auth.js`)
- [x] Có test sai token hoặc token rỗng. (Đã giả lập gửi token rác để Gateway chặn lỗi)
- [x] Endpoint public được khai báo rõ nếu không cần auth. (Mọi endpoint nghiệp vụ của camera đều bắt buộc an ninh)
- [x] Test thể hiện đúng expected status 401/403. (Đã dùng `pm.expect([401, 403]).to.include(...)`)

## 3. Negative tests

- [x] Có test thiếu field bắt buộc. (Đã viết test xóa trường `cameraId` tại file `03_Negative.js`)
- [x] Có test sai kiểu dữ liệu. (Kiểm thử gửi chuỗi rác không đúng định dạng ảnh)
- [x] Có test sai enum hoặc giá trị ngoài miền. (Kiểm thử truyền sai thông số luồng stream)
- [x] Lỗi trả về theo cùng một error model. (Đã xác thực cấu trúc lỗi đạt chuẩn `ProblemDetails` có đủ `type`, `title`, `status`)

## 4. Boundary tests

- [x] Có test min/max hoặc dữ liệu sát ngưỡng. (Đã cấu hình tại file `04_Boundary_Reliability.js` cho API `/analyze`)
- [x] Có test limit/pagination nếu endpoint có danh sách. (Áp dụng cho API danh sách thiết bị camera)
- [x] Có test payload lớn hoặc metadata thiếu. (Kiểm thử truyền tải kích thước khung hình ở ngưỡng giới hạn)
- [x] Có ghi chú kỳ vọng xử lý dữ liệu biên. (Hệ thống trả ra mã `202 Accepted` để xử lý hàng đợi ngầm)

## 5. Reliability tests cơ bản

- [x] Có kiểm tra response time. (Đã viết script đo hiệu năng tại file `06_Local_only_NonFunctional.js`)
- [x] Có mô tả timeout mong muốn. (Đặt ngưỡng cam kết SLA dưới máy local là 1000ms)
- [x] Có test hoặc ghi chú retry/idempotency nếu phù hợp. (Đảm bảo việc gửi trùng khung hình cũ sẽ được phân loại bằng mã hash)
- [x] Có consumer-side smoke test với ít nhất 1 mock của nhóm khác. (Đã bắt tay gọi sang dịch vụ đối tác AI Vision tại file `05_Consumer_side_Smoke.js`)
s
## 6. Evidence

- [x] Collection export JSON. (Được lưu tại thư mục `postman/collections/`)
- [x] Environment mock export JSON. (Đã hoàn thiện file `FIT4110_lab03_mock.postman_environment.json`)
- [x] Environment local export JSON. (Đã hoàn thiện file `FIT4110_lab03_local.postman_environment.json`)
- [x] Newman report XML/HTML. (Tự động sinh ra tại thư mục `reports/` sau khi chạy lệnh test)
- [x] Test-case matrix đã điền. (Đã điền đầy đủ thông tin tại file `templates/test-case-matrix.csv`)
- [x] Biên bản handshake đã điền. (Đã đặc tả tại file `templates/consumer-provider-handshake.md`)