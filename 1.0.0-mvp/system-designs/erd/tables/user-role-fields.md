# USER_ROLE_FIELDS

## Mục đích

Lưu các lĩnh vực mà một người đã chọn cho vai trò cụ thể.

## Logic

- Tạo khi người dùng chọn lĩnh vực hoạt động cho một vai trò cụ thể.
- Chỉ cho phép chọn field nằm trong `ROLE_ACTIVITY_FIELDS` của role đó.
- Thay đổi field có thể làm thay đổi danh sách service được phép chọn hoặc cung cấp.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_role_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `activity_field_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `ACTIVITY_FIELDS`. |
| `selected_at` | `datetime` | Không | Thời điểm người dùng chọn giá trị này. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `USER_ROLE_FIELDS` (đúng 1 → 0..n) | Bảng `USER_ROLES` chọn `USER_ROLE_FIELDS`; nhìn từ `USER_ROLE_FIELDS`, đây là quan hệ ngược về `USER_ROLES`. | USER_ROLE_FIELDS.user_role_id -> USER_ROLES.id |
| `ACTIVITY_FIELDS` | `ACTIVITY_FIELDS` \|\|--o{ `USER_ROLE_FIELDS` (đúng 1 → 0..n) | Bảng `ACTIVITY_FIELDS` được chọn làm `USER_ROLE_FIELDS`; nhìn từ `USER_ROLE_FIELDS`, đây là quan hệ ngược về `ACTIVITY_FIELDS`. | USER_ROLE_FIELDS.activity_field_id -> ACTIVITY_FIELDS.id |
