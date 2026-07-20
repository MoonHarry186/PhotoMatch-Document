# SERVICES

## Mục đích

Danh mục dịch vụ thuộc một lĩnh vực hoạt động cụ thể.

## Logic

- Là danh mục dịch vụ con thuộc một activity field.
- Chỉ service active mới xuất hiện trong onboarding, filter và booking mới.
- Không xóa cứng service đã được tham chiếu bởi portfolio, filter hoặc booking.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `activity_field_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `ACTIVITY_FIELDS`. |
| `code` | `varchar` | Unique | Mã định danh nghiệp vụ, thường dùng trong logic hoặc cấu hình. |
| `name` | `varchar` | Không | Tên hiển thị. |
| `description` | `text` | Không | Mô tả chi tiết. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `ACTIVITY_FIELDS` | `ACTIVITY_FIELDS` \|\|--o{ `SERVICES` (đúng 1 → 0..n) | Bảng `ACTIVITY_FIELDS` chứa `SERVICES`; nhìn từ `SERVICES`, đây là quan hệ ngược về `ACTIVITY_FIELDS`. | SERVICES.activity_field_id -> ACTIVITY_FIELDS.id |
| `USER_ROLE_SERVICES` | `SERVICES` \|\|--o{ `USER_ROLE_SERVICES` (đúng 1 → 0..n) | Một bản ghi `SERVICES` tham chiếu `USER_ROLE_SERVICES`. | USER_ROLE_SERVICES.service_id -> SERVICES.id |
| `PORTFOLIO_ITEMS` | `SERVICES` o\|--o{ `PORTFOLIO_ITEMS` (0..1 → 0..n) | Một bản ghi `SERVICES` phân loại `PORTFOLIO_ITEMS`. | PORTFOLIO_ITEMS.service_id -> SERVICES.id |
| `DISCOVERY_FILTER_SERVICES` | `SERVICES` \|\|--o{ `DISCOVERY_FILTER_SERVICES` (đúng 1 → 0..n) | Một bản ghi `SERVICES` lọc theo `DISCOVERY_FILTER_SERVICES`. | DISCOVERY_FILTER_SERVICES.service_id -> SERVICES.id |
| `SHOOT_REQUESTS` | `SERVICES` \|\|--o{ `SHOOT_REQUESTS` (đúng 1 → 0..n) | Một bản ghi `SERVICES` yêu cầu dịch vụ `SHOOT_REQUESTS`. | SHOOT_REQUESTS.service_id -> SERVICES.id |
| `BOOKINGS` | `SERVICES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Một bản ghi `SERVICES` đặt dịch vụ `BOOKINGS`. | BOOKINGS.service_id -> SERVICES.id |
