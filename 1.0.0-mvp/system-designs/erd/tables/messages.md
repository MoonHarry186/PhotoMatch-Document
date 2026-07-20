# MESSAGES

## Mục đích

Lưu nội dung tin nhắn trong cuộc trò chuyện.

## Logic

- Chỉ participant của conversation mới được gửi message.
- Trước khi lưu message phải kiểm tra match còn hiệu lực và không bên nào chặn bên còn lại.
- Xóa message nên là xóa mềm qua `deleted_at` để giữ lịch sử receipt/report.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `conversation_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `CONVERSATIONS`. |
| `sender_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `reply_to_message_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `MESSAGES`. |
| `message_type` | `varchar` | Không | Loại tin nhắn, ví dụ text hoặc system. |
| `content` | `text` | Không | Nội dung chính của tin nhắn hoặc tài liệu. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `edited_at` | `datetime` | Không | Thời điểm chỉnh sửa tin nhắn. |
| `deleted_at` | `datetime` | Không | Thời điểm xóa mềm bản ghi, nếu có. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `CONVERSATIONS` | `CONVERSATIONS` \|\|--o{ `MESSAGES` (đúng 1 → 0..n) | Bảng `CONVERSATIONS` chứa `MESSAGES`; nhìn từ `MESSAGES`, đây là quan hệ ngược về `CONVERSATIONS`. | MESSAGES.conversation_id -> CONVERSATIONS.id |
| `USERS` | `USERS` \|\|--o{ `MESSAGES` (đúng 1 → 0..n) | Bảng `USERS` gửi `MESSAGES`; nhìn từ `MESSAGES`, đây là quan hệ ngược về `USERS`. | MESSAGES.sender_user_id -> USERS.id |
| `MESSAGES` | `MESSAGES` o\|--o{ `MESSAGES` (0..1 → 0..n) | `MESSAGES` có quan hệ tự tham chiếu: một bản ghi trả lời bản ghi khác cùng bảng. | MESSAGES.reply_to_message_id -> MESSAGES.id |
| `MESSAGE_RECEIPTS` | `MESSAGES` \|\|--o{ `MESSAGE_RECEIPTS` (đúng 1 → 0..n) | Một bản ghi `MESSAGES` sở hữu hồ sơ `MESSAGE_RECEIPTS`. | MESSAGE_RECEIPTS.message_id -> MESSAGES.id |
| `NOTIFICATIONS` | `MESSAGES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Một bản ghi `MESSAGES` tham chiếu `NOTIFICATIONS`. | NOTIFICATIONS.message_id -> MESSAGES.id |
| `USER_REPORTS` | `MESSAGES` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Một bản ghi `MESSAGES` tham chiếu `USER_REPORTS`. | USER_REPORTS.message_id -> MESSAGES.id |
