# CONVERSATION_PARTICIPANTS

## Mục đích

Lưu thành viên của mỗi cuộc trò chuyện và trạng thái đọc/tắt thông báo.

## Logic

- Trong MVP mỗi conversation nên có đúng hai participant tương ứng hai user của match.
- `last_read_at` dùng để tính unread count và trạng thái đã đọc.
- Khi participant `left_at` có giá trị, người đó không nên tiếp tục nhận tin nhắn mới trong conversation.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `conversation_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `CONVERSATIONS`. |
| `user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `is_muted` | `boolean` | Không | Đánh dấu người tham gia đã tắt thông báo cuộc trò chuyện. |
| `joined_at` | `datetime` | Không | Thời điểm tham gia cuộc trò chuyện. |
| `last_read_at` | `datetime` | Không | Thời điểm đọc gần nhất trong conversation. |
| `left_at` | `datetime` | Không | Thời điểm rời khỏi conversation nếu có. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `CONVERSATIONS` | `CONVERSATIONS` \|\|--\|{ `CONVERSATION_PARTICIPANTS` (đúng 1 → 1..n) | Bảng `CONVERSATIONS` chứa `CONVERSATION_PARTICIPANTS`; nhìn từ `CONVERSATION_PARTICIPANTS`, đây là quan hệ ngược về `CONVERSATIONS`. | CONVERSATION_PARTICIPANTS.conversation_id -> CONVERSATIONS.id |
| `USERS` | `USERS` \|\|--o{ `CONVERSATION_PARTICIPANTS` (đúng 1 → 0..n) | Bảng `USERS` tham gia `CONVERSATION_PARTICIPANTS`; nhìn từ `CONVERSATION_PARTICIPANTS`, đây là quan hệ ngược về `USERS`. | CONVERSATION_PARTICIPANTS.user_id -> USERS.id |
