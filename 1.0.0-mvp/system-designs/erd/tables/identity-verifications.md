# IDENTITY_VERIFICATIONS

## Mục đích

Lưu lịch sử gửi và xử lý xác minh danh tính qua provider bên thứ ba.

## Logic

- Tạo khi user gửi yêu cầu xác minh danh tính qua provider bên thứ ba.
- Kết quả xác minh cập nhật `status`, `verified_at` hoặc `rejected_reason`.
- Trạng thái tổng hợp trên `USERS.identity_verification_status` nên được cập nhật theo bản ghi mới nhất/hợp lệ nhất.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `provider` | `varchar` | Không | Nhà cung cấp liên quan, ví dụ provider đăng nhập hoặc provider xác minh danh tính. |
| `document_type` | `varchar` | Không | Loại tài liệu hoặc giấy tờ, tùy ngữ cảnh bảng. |
| `provider_reference` | `varchar` | Không | Mã tham chiếu phía provider. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `rejected_reason` | `text` | Không | Lý do bị từ chối nếu xử lý thất bại. |
| `submitted_at` | `datetime` | Không | Thời điểm gửi yêu cầu. |
| `verified_at` | `datetime` | Không | Thời điểm xác minh thành công. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `IDENTITY_VERIFICATIONS` (đúng 1 → 0..n) | Bảng `USERS` gửi yêu cầu xác minh `IDENTITY_VERIFICATIONS`; nhìn từ `IDENTITY_VERIFICATIONS`, đây là quan hệ ngược về `USERS`. | IDENTITY_VERIFICATIONS.user_id -> USERS.id |
