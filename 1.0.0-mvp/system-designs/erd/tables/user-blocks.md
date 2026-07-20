# USER_BLOCKS

## Mục đích

Lưu quan hệ chặn giữa hai tài khoản.

## Logic

- Tạo khi một user chặn user khác; không cho phép tự chặn chính mình.
- Khi có block giữa hai user, hệ thống phải chặn chat, match interaction và discovery tương ứng.
- Cặp `(blocker_user_id, blocked_user_id)` nên unique để tránh trùng bản ghi.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `blocker_user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `blocked_user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `reason_code` | `varchar` | Không | Mã lý do phục vụ phân loại xử lý. |
| `note` | `text` | Không | Ghi chú bổ sung. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `USER_BLOCKS` (đúng 1 → 0..n) | Bảng `USERS` chặn `USER_BLOCKS`; nhìn từ `USER_BLOCKS`, đây là quan hệ ngược về `USERS`. | USER_BLOCKS.blocker_user_id -> USERS.id |
| `USERS` | `USERS` \|\|--o{ `USER_BLOCKS` (đúng 1 → 0..n) | Bảng `USERS` bị chặn `USER_BLOCKS`; nhìn từ `USER_BLOCKS`, đây là quan hệ ngược về `USERS`. | USER_BLOCKS.blocked_user_id -> USERS.id |
