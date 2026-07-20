# ROLES

## Mục đích

Danh mục vai trò trong hệ thống như customer, photographer hoặc admin.

## Logic

- Là dữ liệu danh mục nên thường được seed sẵn, ví dụ CUSTOMER, PHOTOGRAPHER, ADMIN.
- `code` nên ổn định và không đổi tùy tiện vì được dùng trong phân quyền và điều hướng.
- Role bị inactive không được chọn mới, nhưng vẫn cần giữ để đọc dữ liệu lịch sử.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `code` | `varchar` | Unique | Mã định danh nghiệp vụ, thường dùng trong logic hoặc cấu hình. |
| `name` | `varchar` | Không | Tên hiển thị. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `ROLES` \|\|--o{ `USER_ROLES` (đúng 1 → 0..n) | Một bản ghi `ROLES` gán vào `USER_ROLES`. | USER_ROLES.role_id -> ROLES.id |
| `ROLE_ACTIVITY_FIELDS` | `ROLES` \|\|--o{ `ROLE_ACTIVITY_FIELDS` (đúng 1 → 0..n) | Một bản ghi `ROLES` cho phép `ROLE_ACTIVITY_FIELDS`. | ROLE_ACTIVITY_FIELDS.role_id -> ROLES.id |
| `DISCOVERY_FILTERS` | `ROLES` \|\|--o{ `DISCOVERY_FILTERS` (đúng 1 → 0..n) | Một bản ghi `ROLES` nhắm tới `DISCOVERY_FILTERS`. | DISCOVERY_FILTERS.target_role_id -> ROLES.id |
