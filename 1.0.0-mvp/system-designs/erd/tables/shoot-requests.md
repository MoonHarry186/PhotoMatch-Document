# SHOOT_REQUESTS

## Mục đích

Lưu nhu cầu chụp ảnh do khách hàng đăng lên.

## Logic

- Tạo bởi customer role khi đăng nhu cầu chụp ảnh.
- Chỉ request còn open/active mới được dùng để tạo booking hoặc hiển thị cho thợ ảnh.
- Cần validate thời gian, ngân sách và vị trí trước khi publish request.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `customer_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `service_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `SERVICES`. |
| `city_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `CITIES`. |
| `title` | `varchar` | Không | Tiêu đề hiển thị. |
| `description` | `text` | Không | Mô tả chi tiết. |
| `scheduled_start` | `datetime` | Không | Thời điểm bắt đầu lịch hoặc nhu cầu chụp. |
| `scheduled_end` | `datetime` | Không | Thời điểm kết thúc lịch hoặc nhu cầu chụp. |
| `budget_min` | `decimal` | Không | Ngân sách tối thiểu. |
| `budget_max` | `decimal` | Không | Ngân sách tối đa. |
| `currency` | `varchar` | Không | Đơn vị tiền tệ. |
| `address_text` | `varchar` | Không | Địa chỉ dạng text. |
| `latitude` | `decimal` | Không | Vĩ độ của địa điểm hoặc vị trí liên quan. |
| `longitude` | `decimal` | Không | Kinh độ của địa điểm hoặc vị trí liên quan. |
| `visibility` | `varchar` | Không | Phạm vi hiển thị của yêu cầu. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |
| `closed_at` | `datetime` | Không | Thời điểm đóng yêu cầu. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `SHOOT_REQUESTS` (đúng 1 → 0..n) | Bảng `USER_ROLES` tạo `SHOOT_REQUESTS`; nhìn từ `SHOOT_REQUESTS`, đây là quan hệ ngược về `USER_ROLES`. | SHOOT_REQUESTS.customer_role_id -> USER_ROLES.id |
| `SERVICES` | `SERVICES` \|\|--o{ `SHOOT_REQUESTS` (đúng 1 → 0..n) | Bảng `SERVICES` yêu cầu dịch vụ `SHOOT_REQUESTS`; nhìn từ `SHOOT_REQUESTS`, đây là quan hệ ngược về `SERVICES`. | SHOOT_REQUESTS.service_id -> SERVICES.id |
| `CITIES` | `CITIES` o\|--o{ `SHOOT_REQUESTS` (0..1 → 0..n) | Bảng `CITIES` diễn ra tại `SHOOT_REQUESTS`; nhìn từ `SHOOT_REQUESTS`, đây là quan hệ ngược về `CITIES`. | SHOOT_REQUESTS.city_id -> CITIES.id |
| `BOOKINGS` | `SHOOT_REQUESTS` o\|--o{ `BOOKINGS` (0..1 → 0..n) | Một bản ghi `SHOOT_REQUESTS` trở thành `BOOKINGS`. | BOOKINGS.shoot_request_id -> SHOOT_REQUESTS.id |
