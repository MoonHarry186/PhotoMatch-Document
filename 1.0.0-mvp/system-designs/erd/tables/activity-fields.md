# ACTIVITY_FIELDS

## Mục đích

Danh mục lĩnh vực chụp ảnh cấp cao như cưới, sự kiện, chân dung hoặc du lịch.

## Logic

- Là danh mục lĩnh vực cấp cao, ví dụ cưới, chân dung, sự kiện.
- Field inactive không được chọn mới, nhưng vẫn giữ để đọc hồ sơ hoặc booking cũ.
- Các service con phải thuộc một activity field hợp lệ.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `code` | `varchar` | Unique | Mã định danh nghiệp vụ, thường dùng trong logic hoặc cấu hình. |
| `name` | `varchar` | Không | Tên hiển thị. |
| `description` | `text` | Không | Mô tả chi tiết. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `ROLE_ACTIVITY_FIELDS` | `ACTIVITY_FIELDS` \|\|--o{ `ROLE_ACTIVITY_FIELDS` (đúng 1 → 0..n) | Một bản ghi `ACTIVITY_FIELDS` thuộc về `ROLE_ACTIVITY_FIELDS`. | ROLE_ACTIVITY_FIELDS.activity_field_id -> ACTIVITY_FIELDS.id |
| `USER_ROLE_FIELDS` | `ACTIVITY_FIELDS` \|\|--o{ `USER_ROLE_FIELDS` (đúng 1 → 0..n) | Một bản ghi `ACTIVITY_FIELDS` được chọn làm `USER_ROLE_FIELDS`. | USER_ROLE_FIELDS.activity_field_id -> ACTIVITY_FIELDS.id |
| `SERVICES` | `ACTIVITY_FIELDS` \|\|--o{ `SERVICES` (đúng 1 → 0..n) | Một bản ghi `ACTIVITY_FIELDS` chứa `SERVICES`. | SERVICES.activity_field_id -> ACTIVITY_FIELDS.id |
