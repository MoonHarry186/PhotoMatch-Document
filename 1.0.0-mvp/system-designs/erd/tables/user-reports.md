# USER_REPORTS

## Mục đích

Lưu báo cáo vi phạm giữa người dùng với người dùng hoặc tin nhắn cụ thể.

## Logic

- Tạo khi user báo cáo người khác hoặc context được phép như match, conversation, message hoặc booking.
- Admin hoặc moderator cập nhật `resolved_by_user_id`, `status`, `admin_note`, `resolution`, `updated_at`, `resolved_at` khi xử lý.
- Report nghiêm trọng có thể dẫn tới `ACCOUNT_PENALTIES`.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `reporter_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `reported_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `match_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `MATCHES`. |
| `conversation_id` | `uuid` | Tùy chọn, khóa ngoại | Context conversation mà reporter được phép truy cập. |
| `message_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `MESSAGES`. |
| `booking_id` | `uuid` | Tùy chọn, khóa ngoại | Context booking mà reporter tham gia. |
| `resolved_by_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `reason_code` | `varchar` | Không | Mã lý do phục vụ phân loại xử lý. |
| `description` | `text` | Không | Mô tả chi tiết. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `admin_note` | `text` | Tùy chọn | Ghi chú nội bộ, không serialize cho normal user. |
| `resolution` | `text` | Tùy chọn | Kết quả xử lý có thể dùng để thông báo cho reporter. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm report được cập nhật gần nhất. |
| `resolved_at` | `datetime` | Không | Thời điểm report được xử lý. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `USER_REPORTS` (đúng 1 → 0..n) | Bảng `USERS` báo cáo `USER_REPORTS`; nhìn từ `USER_REPORTS`, đây là quan hệ ngược về `USERS`. | USER_REPORTS.reporter_user_id -> USERS.id |
| `USERS` | `USERS` \|\|--o{ `USER_REPORTS` (đúng 1 → 0..n) | Bảng `USERS` bị báo cáo `USER_REPORTS`; nhìn từ `USER_REPORTS`, đây là quan hệ ngược về `USERS`. | USER_REPORTS.reported_user_id -> USERS.id |
| `USERS` | `USERS` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Bảng `USERS` xử lý `USER_REPORTS`; nhìn từ `USER_REPORTS`, đây là quan hệ ngược về `USERS`. | USER_REPORTS.resolved_by_user_id -> USERS.id |
| `MATCHES` | `MATCHES` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Bảng `MATCHES` tham chiếu `USER_REPORTS`; nhìn từ `USER_REPORTS`, đây là quan hệ ngược về `MATCHES`. | USER_REPORTS.match_id -> MATCHES.id |
| `CONVERSATIONS` | `CONVERSATIONS` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Conversation context của report. | USER_REPORTS.conversation_id -> CONVERSATIONS.id |
| `MESSAGES` | `MESSAGES` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Bảng `MESSAGES` tham chiếu `USER_REPORTS`; nhìn từ `USER_REPORTS`, đây là quan hệ ngược về `MESSAGES`. | USER_REPORTS.message_id -> MESSAGES.id |
| `BOOKINGS` | `BOOKINGS` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Booking context của report. | USER_REPORTS.booking_id -> BOOKINGS.id |
| `ACCOUNT_PENALTIES` | `USER_REPORTS` o\|--o{ `ACCOUNT_PENALTIES` (0..1 → 0..n) | Một bản ghi `USER_REPORTS` dẫn tới `ACCOUNT_PENALTIES`. | ACCOUNT_PENALTIES.report_id -> USER_REPORTS.id |
