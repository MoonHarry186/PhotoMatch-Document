# ROLE_ACTIVITY_FIELDS

## Mục đích

Quy định vai trò nào được phép chọn lĩnh vực hoạt động nào.

## Logic

- Quy định role nào được phép chọn activity field nào trong onboarding.
- Nếu `is_required = true`, user role tương ứng phải chọn field này trước khi hoàn tất onboarding.
- Bảng này giúp cấu hình khác nhau giữa customer và photographer mà không hard-code trong app.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `role_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `ROLES`. |
| `activity_field_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `ACTIVITY_FIELDS`. |
| `is_required` | `boolean` | Không | Đánh dấu giá trị này là bắt buộc trong ngữ cảnh cấu hình. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `ROLES` | `ROLES` \|\|--o{ `ROLE_ACTIVITY_FIELDS` (đúng 1 → 0..n) | Bảng `ROLES` cho phép `ROLE_ACTIVITY_FIELDS`; nhìn từ `ROLE_ACTIVITY_FIELDS`, đây là quan hệ ngược về `ROLES`. | ROLE_ACTIVITY_FIELDS.role_id -> ROLES.id |
| `ACTIVITY_FIELDS` | `ACTIVITY_FIELDS` \|\|--o{ `ROLE_ACTIVITY_FIELDS` (đúng 1 → 0..n) | Bảng `ACTIVITY_FIELDS` thuộc về `ROLE_ACTIVITY_FIELDS`; nhìn từ `ROLE_ACTIVITY_FIELDS`, đây là quan hệ ngược về `ACTIVITY_FIELDS`. | ROLE_ACTIVITY_FIELDS.activity_field_id -> ACTIVITY_FIELDS.id |
