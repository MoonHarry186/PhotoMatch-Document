# BOOKINGS

## Mục đích

Lưu yêu cầu/lịch chụp giữa khách hàng và thợ ảnh, bắt đầu ở trạng thái `PENDING` khi Customer gửi đặt lịch.

## Logic

- Customer có thể tạo booking trực tiếp mà không cần match trước; trong cùng transaction hệ thống tạo hoặc tái sử dụng match và conversation, sau đó tạo booking `PENDING`.
- Mọi booking sau khi tạo đều phải tham chiếu một match. Nếu đã có active match thì tái sử dụng; nếu chưa có thì tạo mới theo policy và không tạo trùng.
- Photographer chỉ được tạo booking khi đã có active match/conversation.
- Mọi thay đổi `status` nên tạo thêm dòng trong `BOOKING_STATUS_HISTORY`.
- Chỉ booking completed mới đủ điều kiện tạo review.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `match_id` | `uuid` | Bắt buộc, khóa ngoại | Match được tạo/tái sử dụng atomically khi booking được tạo; tham chiếu `MATCHES`. |
| `shoot_request_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `SHOOT_REQUESTS`. |
| `customer_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `photographer_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `service_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `SERVICES`. |
| `scheduled_start` | `datetime` | Không | Thời điểm bắt đầu lịch hoặc nhu cầu chụp. |
| `scheduled_end` | `datetime` | Không | Thời điểm kết thúc lịch hoặc nhu cầu chụp. |
| `address_text` | `varchar` | Không | Địa chỉ dạng text. |
| `latitude` | `decimal` | Không | Vĩ độ của địa điểm hoặc vị trí liên quan. |
| `longitude` | `decimal` | Không | Kinh độ của địa điểm hoặc vị trí liên quan. |
| `agreed_price` | `decimal` | Không | Giá cuối cùng hai bên thống nhất. |
| `currency` | `varchar` | Không | Đơn vị tiền tệ. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `cancellation_reason` | `text` | Không | Lý do hủy nếu booking bị hủy. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |
| `completed_at` | `datetime` | Không | Thời điểm booking hoàn thành. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `MATCHES` | `MATCHES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Mỗi booking thuộc đúng một match; match có thể được tạo từ direct booking hoặc tái sử dụng nếu đã active. | BOOKINGS.match_id -> MATCHES.id |
| `SHOOT_REQUESTS` | `SHOOT_REQUESTS` o\|--o{ `BOOKINGS` (0..1 → 0..n) | Bảng `SHOOT_REQUESTS` trở thành `BOOKINGS`; nhìn từ `BOOKINGS`, đây là quan hệ ngược về `SHOOT_REQUESTS`. | BOOKINGS.shoot_request_id -> SHOOT_REQUESTS.id |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Bảng `USER_ROLES` đặt lịch với vai trò khách hàng `BOOKINGS`; nhìn từ `BOOKINGS`, đây là quan hệ ngược về `USER_ROLES`. | BOOKINGS.customer_role_id -> USER_ROLES.id |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Bảng `USER_ROLES` nhận lịch với vai trò thợ ảnh `BOOKINGS`; nhìn từ `BOOKINGS`, đây là quan hệ ngược về `USER_ROLES`. | BOOKINGS.photographer_role_id -> USER_ROLES.id |
| `SERVICES` | `SERVICES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Bảng `SERVICES` đặt dịch vụ `BOOKINGS`; nhìn từ `BOOKINGS`, đây là quan hệ ngược về `SERVICES`. | BOOKINGS.service_id -> SERVICES.id |
| `BOOKING_STATUS_HISTORY` | `BOOKINGS` \|\|--o{ `BOOKING_STATUS_HISTORY` (đúng 1 → 0..n) | Một bản ghi `BOOKINGS` có lịch sử trạng thái `BOOKING_STATUS_HISTORY`. | BOOKING_STATUS_HISTORY.booking_id -> BOOKINGS.id |
| `REVIEWS` | `BOOKINGS` \|\|--o\| `REVIEWS` (đúng 1 → 0..1) | Một booking completed nhận tối đa một review từ Customer. | REVIEWS.booking_id -> BOOKINGS.id |
| `NOTIFICATIONS` | `BOOKINGS` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Một bản ghi `BOOKINGS` tham chiếu `NOTIFICATIONS`. | NOTIFICATIONS.booking_id -> BOOKINGS.id |
