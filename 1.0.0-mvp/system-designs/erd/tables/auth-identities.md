# AUTH_IDENTITIES

## Mục đích

Lưu các phương thức đăng nhập gắn với một tài khoản, ví dụ email/password, Google, Apple hoặc provider khác.

## Logic

- Mỗi user có thể có nhiều phương thức đăng nhập, nhưng một cặp `provider` và `provider_identifier` không được trùng giữa các tài khoản.
- `password_hash` chỉ có giá trị với provider email/password; OAuth provider không lưu mật khẩu nội bộ.
- Khi identity bị vô hiệu hóa, hệ thống phải chặn đăng nhập bằng identity đó nhưng không nhất thiết khóa toàn bộ user.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `provider` | `varchar` | Không | Nhà cung cấp liên quan, ví dụ provider đăng nhập hoặc provider xác minh danh tính. |
| `provider_identifier` | `varchar` | Không | Định danh người dùng phía provider, ví dụ email hoặc provider subject. |
| `password_hash` | `varchar` | Không | Mật khẩu đã hash; chỉ dùng cho provider email/password. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `last_used_at` | `datetime` | Không | Lần cuối phương thức đăng nhập này được sử dụng. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `AUTH_IDENTITIES` (đúng 1 → 0..n) | Bảng `USERS` đăng nhập bằng `AUTH_IDENTITIES`; nhìn từ `AUTH_IDENTITIES`, đây là quan hệ ngược về `USERS`. | AUTH_IDENTITIES.user_id -> USERS.id |
| `PASSWORD_RESET_TOKENS` | `AUTH_IDENTITIES` \|\|--o{ `PASSWORD_RESET_TOKENS` (đúng 1 → 0..n) | Một bản ghi `AUTH_IDENTITIES` nhận `PASSWORD_RESET_TOKENS`. | PASSWORD_RESET_TOKENS.auth_identity_id -> AUTH_IDENTITIES.id |
