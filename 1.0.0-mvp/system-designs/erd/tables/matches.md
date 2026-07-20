# MATCHES

## Mục đích

Lưu kết nối giữa hai vai trò sau khi hai bên đạt điều kiện match.

## Logic

- Tạo khi hai bên đạt điều kiện match theo luồng Customer quan tâm và Photographer chấp nhận, hoặc khi Customer gửi direct booking hợp lệ.
- Một cặp user role chỉ nên có một match active; cần chuẩn hóa thứ tự A/B hoặc dùng unique logic.
- Khi match active được tạo, hệ thống mở `CONVERSATIONS` tương ứng. Direct booking tạo match, conversation, participants và booking `PENDING` trong cùng transaction.
- Unmatch chuyển lifecycle sang ended, đóng conversation cho message mới và giữ lịch sử; không hard-delete match.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_role_a_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `user_role_b_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `ended_by_user_id` | `uuid` | Tùy chọn, khóa ngoại | Participant kết thúc match; có thể trống khi system kết thúc do policy. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `end_reason` | `text` | Tùy chọn | Lý do/kind kết thúc match. |
| `matched_at` | `datetime` | Không | Thời điểm match được tạo. |
| `ended_at` | `datetime` | Không | Thời điểm match kết thúc nếu có. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `MATCHES` (đúng 1 → 0..n) | Bảng `USER_ROLES` tham gia với vai trò phía A `MATCHES`; nhìn từ `MATCHES`, đây là quan hệ ngược về `USER_ROLES`. | MATCHES.user_role_a_id -> USER_ROLES.id |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `MATCHES` (đúng 1 → 0..n) | Bảng `USER_ROLES` tham gia với vai trò phía B `MATCHES`; nhìn từ `MATCHES`, đây là quan hệ ngược về `USER_ROLES`. | MATCHES.user_role_b_id -> USER_ROLES.id |
| `USERS` | `USERS` o\|--o{ `MATCHES` (0..1 → 0..n) | User có thể kết thúc nhiều match; system-ended match có thể không có actor. | MATCHES.ended_by_user_id -> USERS.id |
| `CONVERSATIONS` | `MATCHES` \|\|--o\| `CONVERSATIONS` (đúng 1 → 0..1) | Một bản ghi `MATCHES` mở `CONVERSATIONS`. | CONVERSATIONS.match_id -> MATCHES.id |
| `BOOKINGS` | `MATCHES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Một match có thể dẫn tới nhiều booking; mỗi booking thuộc đúng một match. | BOOKINGS.match_id -> MATCHES.id |
| `NOTIFICATIONS` | `MATCHES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Một bản ghi `MATCHES` tham chiếu `NOTIFICATIONS`. | NOTIFICATIONS.match_id -> MATCHES.id |
| `USER_REPORTS` | `MATCHES` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Một bản ghi `MATCHES` tham chiếu `USER_REPORTS`. | USER_REPORTS.match_id -> MATCHES.id |

## Ghi chú nghiệp vụ

- Nên chuẩn hóa cặp `user_role_a_id`, `user_role_b_id` để tránh tạo hai match trùng chiều A-B và B-A.
