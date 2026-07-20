# ACCOUNT_PENALTIES

## Mục đích

Lưu cảnh cáo, khóa tạm thời hoặc khóa vĩnh viễn áp dụng lên tài khoản.

## Logic

- Tạo khi admin/moderator áp dụng cảnh cáo, khóa tạm thời hoặc khóa vĩnh viễn.
- Penalty active trong khoảng `starts_at` đến `ends_at` phải ảnh hưởng tới quyền đăng nhập và thao tác của user.
- Khi thu hồi penalty, cập nhật moderator, lý do, `revoked_at` và `status` thay vì xóa bản ghi.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `report_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_REPORTS`. |
| `imposed_by_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `revoked_by_user_id` | `uuid` | Tùy chọn, khóa ngoại | Admin thu hồi penalty. |
| `penalty_type` | `varchar` | Không | Loại hình phạt, ví dụ warning, temporary_ban hoặc permanent_ban. |
| `reason` | `text` | Không | Lý do áp dụng hành động. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `starts_at` | `datetime` | Không | Thời điểm hình phạt bắt đầu. |
| `ends_at` | `datetime` | Không | Thời điểm hình phạt kết thúc nếu có. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `revoke_reason` | `text` | Tùy chọn có điều kiện | Bắt buộc khi penalty được thu hồi. |
| `revoked_at` | `datetime` | Không | Thời điểm session hoặc penalty bị thu hồi. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `NOTIFICATIONS` | `ACCOUNT_PENALTIES` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Một bản ghi `ACCOUNT_PENALTIES` tham chiếu `NOTIFICATIONS`. | NOTIFICATIONS.penalty_id -> ACCOUNT_PENALTIES.id |
| `USERS` | `USERS` \|\|--o{ `ACCOUNT_PENALTIES` (đúng 1 → 0..n) | Bảng `USERS` nhận `ACCOUNT_PENALTIES`; nhìn từ `ACCOUNT_PENALTIES`, đây là quan hệ ngược về `USERS`. | ACCOUNT_PENALTIES.user_id -> USERS.id |
| `USERS` | `USERS` \|\|--o{ `ACCOUNT_PENALTIES` (đúng 1 → 0..n) | Bảng `USERS` áp dụng hình phạt `ACCOUNT_PENALTIES`; nhìn từ `ACCOUNT_PENALTIES`, đây là quan hệ ngược về `USERS`. | ACCOUNT_PENALTIES.imposed_by_user_id -> USERS.id |
| `USERS` | `USERS` o\|--o{ `ACCOUNT_PENALTIES` (0..1 → 0..n) | Một Admin có thể thu hồi nhiều penalty; penalty chưa thu hồi chưa có revoker. | ACCOUNT_PENALTIES.revoked_by_user_id -> USERS.id |
| `USER_REPORTS` | `USER_REPORTS` o\|--o{ `ACCOUNT_PENALTIES` (0..1 → 0..n) | Bảng `USER_REPORTS` dẫn tới `ACCOUNT_PENALTIES`; nhìn từ `ACCOUNT_PENALTIES`, đây là quan hệ ngược về `USER_REPORTS`. | ACCOUNT_PENALTIES.report_id -> USER_REPORTS.id |
