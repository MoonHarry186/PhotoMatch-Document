# USER_ROLE_SERVICES

## Mục đích

Lưu dịch vụ mà một vai trò đang cung cấp hoặc đang có nhu cầu.

## Logic

- Tạo khi một role chọn dịch vụ mình cung cấp hoặc đang có nhu cầu.
- `service_mode` phân biệt OFFERED và WANTED, giúp dùng chung cấu trúc cho cả thợ ảnh và khách hàng.
- Cần validate `min_price <= max_price` và currency/price_unit nhất quán.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `service_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `SERVICES`. |
| `service_mode` | `varchar` | Không | Chế độ dịch vụ: OFFERED nếu cung cấp, WANTED nếu đang cần. |
| `min_price` | `decimal` | Không | Giá tối thiểu mong muốn hoặc cung cấp. |
| `max_price` | `decimal` | Không | Giá tối đa mong muốn hoặc cung cấp. |
| `currency` | `varchar` | Không | Đơn vị tiền tệ. |
| `price_unit` | `varchar` | Không | Đơn vị tính giá, ví dụ per_hour, per_session hoặc per_day. |
| `is_active` | `boolean` | Không | Đánh dấu bản ghi hoặc cấu hình còn đang hoạt động. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `USER_ROLE_SERVICES` (đúng 1 → 0..n) | Bảng `USER_ROLES` cung cấp hoặc có nhu cầu `USER_ROLE_SERVICES`; nhìn từ `USER_ROLE_SERVICES`, đây là quan hệ ngược về `USER_ROLES`. | USER_ROLE_SERVICES.user_role_id -> USER_ROLES.id |
| `SERVICES` | `SERVICES` \|\|--o{ `USER_ROLE_SERVICES` (đúng 1 → 0..n) | Bảng `SERVICES` tham chiếu `USER_ROLE_SERVICES`; nhìn từ `USER_ROLE_SERVICES`, đây là quan hệ ngược về `SERVICES`. | USER_ROLE_SERVICES.service_id -> SERVICES.id |

## Ghi chú nghiệp vụ

- `service_mode` phân biệt dịch vụ do thợ ảnh cung cấp (`OFFERED`) và dịch vụ khách hàng đang cần (`WANTED`).
