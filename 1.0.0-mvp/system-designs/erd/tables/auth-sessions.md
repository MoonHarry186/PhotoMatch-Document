# AUTH_SESSIONS

## Mục đích

Quản lý phiên đăng nhập, refresh token, thiết bị và trạng thái thu hồi session.

## Logic

- Tạo session mới sau khi đăng nhập thành công và lưu refresh token ở dạng hash.
- Khi logout, đổi mật khẩu hoặc phát hiện rủi ro bảo mật, cập nhật `revoked_at` để thu hồi session.
- Session hết hạn theo `expires_at`; job nền có thể dọn hoặc bỏ qua session đã hết hạn.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `refresh_token_hash` | `varchar` | Không | Refresh token đã hash để đối chiếu khi gia hạn phiên. |
| `device_name` | `varchar` | Không | Tên thiết bị hoặc trình duyệt của phiên đăng nhập. |
| `platform` | `varchar` | Không | Nền tảng đăng nhập như iOS, Android hoặc Web. |
| `ip_address` | `varchar` | Không | Địa chỉ IP liên quan tới hành động. |
| `expires_at` | `datetime` | Không | Thời điểm bản ghi hoặc token hết hạn. |
| `revoked_at` | `datetime` | Không | Thời điểm session hoặc penalty bị thu hồi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `AUTH_SESSIONS` (đúng 1 → 0..n) | Bảng `USERS` tạo `AUTH_SESSIONS`; nhìn từ `AUTH_SESSIONS`, đây là quan hệ ngược về `USERS`. | AUTH_SESSIONS.user_id -> USERS.id |
