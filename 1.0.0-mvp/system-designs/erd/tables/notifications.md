# NOTIFICATIONS

## Mục đích

Lưu thông báo gửi tới người dùng, có thể tham chiếu match, message, booking hoặc penalty.

## Logic

- Tạo khi có sự kiện cần báo cho user như match, message, booking, report hoặc penalty.
- Notification có thể tham chiếu một entity nguồn để app điều hướng đúng màn hình khi bấm vào.
- `is_read` và `read_at` dùng để đồng bộ trạng thái đã đọc giữa thiết bị.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `recipient_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `actor_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `match_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `MATCHES`. |
| `message_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `MESSAGES`. |
| `booking_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `BOOKINGS`. |
| `penalty_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `ACCOUNT_PENALTIES`. |
| `notification_type` | `varchar` | Không | Loại thông báo, ví dụ match, message, booking hoặc penalty. |
| `title` | `varchar` | Không | Tiêu đề hiển thị. |
| `body` | `text` | Không | Nội dung thông báo. |
| `is_read` | `boolean` | Không | Đánh dấu thông báo đã đọc. |
| `read_at` | `datetime` | Không | Thời điểm người nhận đọc tin nhắn hoặc thông báo. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `NOTIFICATIONS` (đúng 1 → 0..n) | Bảng `USERS` nhận `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `USERS`. | NOTIFICATIONS.recipient_user_id -> USERS.id |
| `USERS` | `USERS` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Bảng `USERS` kích hoạt `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `USERS`. | NOTIFICATIONS.actor_user_id -> USERS.id |
| `MATCHES` | `MATCHES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Bảng `MATCHES` tham chiếu `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `MATCHES`. | NOTIFICATIONS.match_id -> MATCHES.id |
| `MESSAGES` | `MESSAGES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Bảng `MESSAGES` tham chiếu `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `MESSAGES`. | NOTIFICATIONS.message_id -> MESSAGES.id |
| `BOOKINGS` | `BOOKINGS` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Bảng `BOOKINGS` tham chiếu `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `BOOKINGS`. | NOTIFICATIONS.booking_id -> BOOKINGS.id |
| `ACCOUNT_PENALTIES` | `ACCOUNT_PENALTIES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Bảng `ACCOUNT_PENALTIES` tham chiếu `NOTIFICATIONS`; nhìn từ `NOTIFICATIONS`, đây là quan hệ ngược về `ACCOUNT_PENALTIES`. | NOTIFICATIONS.penalty_id -> ACCOUNT_PENALTIES.id |
