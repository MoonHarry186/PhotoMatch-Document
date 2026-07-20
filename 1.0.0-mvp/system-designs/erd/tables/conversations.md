# CONVERSATIONS

## Mục đích

Lưu cuộc trò chuyện được mở từ một match.

## Logic

- Được tạo từ một match hợp lệ phát sinh qua Photographer accept hoặc Customer direct booking; chỉ mở chat khi match còn hiệu lực.
- `last_message_at` cập nhật khi có message mới để sắp xếp inbox.
- Nếu match bị kết thúc hoặc có block, conversation có thể chuyển trạng thái hạn chế gửi tin.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `match_id` | `uuid` | Unique, Khóa ngoại | Khóa ngoại tham chiếu `MATCHES`. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `last_message_at` | `datetime` | Không | Thời điểm tin nhắn gần nhất. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `MATCHES` | `MATCHES` \|\|--o\| `CONVERSATIONS` (đúng 1 → 0..1) | Bảng `MATCHES` mở `CONVERSATIONS`; nhìn từ `CONVERSATIONS`, đây là quan hệ ngược về `MATCHES`. | CONVERSATIONS.match_id -> MATCHES.id |
| `CONVERSATION_PARTICIPANTS` | `CONVERSATIONS` \|\|--\|{ `CONVERSATION_PARTICIPANTS` (đúng 1 → 1..n) | Một bản ghi `CONVERSATIONS` chứa `CONVERSATION_PARTICIPANTS`. | CONVERSATION_PARTICIPANTS.conversation_id -> CONVERSATIONS.id |
| `MESSAGES` | `CONVERSATIONS` \|\|--o{ `MESSAGES` (đúng 1 → 0..n) | Một bản ghi `CONVERSATIONS` chứa `MESSAGES`. | MESSAGES.conversation_id -> CONVERSATIONS.id |

## Ghi chú nghiệp vụ

- Trong MVP, một match chỉ mở tối đa một conversation; vì vậy `match_id` nên unique.
