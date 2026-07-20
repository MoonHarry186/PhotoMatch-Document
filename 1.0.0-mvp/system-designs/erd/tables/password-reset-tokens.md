# PASSWORD_RESET_TOKENS

## Mục đích

Lưu token đã hash cho luồng quên mật khẩu và đặt lại mật khẩu.

## Logic

- Token chỉ được dùng một lần; sau khi reset mật khẩu thành công phải cập nhật `used_at`.
- Token lưu ở dạng hash, không lưu plaintext để tránh lộ token khi database bị đọc trái phép.
- Chỉ token chưa hết hạn và chưa sử dụng mới được phép đặt lại mật khẩu.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `auth_identity_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `AUTH_IDENTITIES`. |
| `token_hash` | `varchar` | Unique | Token đặt lại mật khẩu đã hash; cần unique. |
| `expires_at` | `datetime` | Không | Thời điểm bản ghi hoặc token hết hạn. |
| `used_at` | `datetime` | Không | Thời điểm token đã được sử dụng. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `AUTH_IDENTITIES` | `AUTH_IDENTITIES` \|\|--o{ `PASSWORD_RESET_TOKENS` (đúng 1 → 0..n) | Bảng `AUTH_IDENTITIES` nhận `PASSWORD_RESET_TOKENS`; nhìn từ `PASSWORD_RESET_TOKENS`, đây là quan hệ ngược về `AUTH_IDENTITIES`. | PASSWORD_RESET_TOKENS.auth_identity_id -> AUTH_IDENTITIES.id |
