# Service Boundary của nhóm

## 1. Thông tin nhóm

- Tên nhóm: 1A
- Lớp: CNTT17-10
- Thành viên: Đinh Thế Mạnh
- Service nhóm phụ trách: Camera
- Sản phẩm tổng thể của lớp: Hệ thống giám sát thông minh tích hợp nhiều dịch vụ microservice

## 2. Actor

Ai tương tác với hệ thống/service?

- Người dùng cuối (user) muốn xem video trực tiếp hoặc bản ghi
- Hệ thống quản trị viên để cấu hình camera và giám sát tình trạng
- Các service khác trong hệ thống (ví dụ: service phân tích hình ảnh, service cảnh báo)

## 3. System Boundary

Nhóm em xây phần nào?

Phần nhóm kiểm soát:

- Service Camera chịu trách nhiệm thu thập dữ liệu video từ camera,
  lưu trữ metadata, và cung cấp API truy vấn luồng video, ảnh chụp và trạng thái camera.
- Quản lý kết nối camera, xác thực camera, và kiểm tra sức khỏe thiết bị.

Phần nhóm chỉ tích hợp:

- Nhận yêu cầu từ service cảnh báo để bật/tắt camera hoặc thay đổi cấu hình ghi hình.
- Cung cấp dữ liệu cho service phân tích hình ảnh / nhận diện để xử lý tiếp.

## 4. Service Boundary

Service của nhóm có trách nhiệm gì?

- Kết nối và quản lý các thiết bị camera trong phạm vi hệ thống.
- Cung cấp API để lấy video trực tiếp, ảnh chụp, và trạng thái camera.
- Thu thập, lưu metadata camera, bao gồm trạng thái kết nối, trạng thái ghi hình và nhật ký sự kiện.
- Kiểm tra và báo cáo tình trạng hoạt động của camera.

Service KHÔNG làm gì?

- Không xử lý nhận diện khuôn mặt, phân loại vật thể hoặc phân tích video sâu.
- Không lưu trữ dữ liệu video dài hạn trong hệ thống lưu trữ phân tán.
- Không thực hiện thông báo người dùng cuối trực tiếp; chỉ cung cấp dữ liệu cho service cảnh báo.

## 5. Input / Output

### Input

- Yêu cầu API từ người dùng hoặc service khác để lấy video, ảnh chụp hoặc trạng thái camera.
- Dữ liệu cấu hình camera từ service quản lý cấu hình.
- Sự kiện yêu cầu bật/tắt camera từ service cảnh báo.

### Output

- Trả về luồng video trực tiếp hoặc ảnh chụp từ camera.
- Trả về trạng thái kết nối, tình trạng ghi hình và metadata camera.
- Gửi thông tin sức khỏe camera cho service giám sát.

## 6. API dự kiến

| Method | Endpoint | Mục đích |
|---|---|---|
| GET | /health | Kiểm tra trạng thái service camera |
| GET | /cameras | Lấy danh sách camera và trạng thái |
| GET | /cameras/{id}/live | Lấy luồng video trực tiếp hoặc URL stream |
| GET | /cameras/{id}/snapshot | Lấy ảnh chụp hiện tại từ camera |
| GET | /cameras/{id}/status | Lấy trạng thái chi tiết của camera |
| POST | /cameras/{id}/config | Cập nhật cấu hình camera |
| POST | /cameras/{id}/action | Thực hiện hành động bật/tắt hoặc reset camera |

## 7. Phụ thuộc service khác

Service này gọi đến service nào?

- Service cấu hình để lấy thông tin cấu hình camera.
- Service lưu trữ nếu cần ghi metadata hoặc logs.
- Service cảnh báo để nhận lệnh bật/tắt hoặc thay đổi trạng thái khi có sự kiện.

Service nào gọi đến service này?

- Service phân tích hình ảnh / nhận diện để lấy luồng camera và xử lý.
- Service giám sát để kiểm tra trạng thái camera và sức khỏe thiết bị.
- Ứng dụng người dùng cuối để xem video trực tiếp, ảnh chụp và trạng thái camera.

## 8. Sơ đồ minh họa

https://lucid.app/lucidchart/91e13ba9-91da-4c17-a93a-1ceeb7b76d40/edit?viewport_loc=-1588%2C-379%2C3488%2C1911%2C0_0&invitationId=inv_5f6f8537-e8d1-49b4-9e63-27ef05970c36
