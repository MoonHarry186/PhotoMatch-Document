# USER_ROLES

## Mục đích

Liên kết tài khoản với vai trò cụ thể, cho phép một user có nhiều vai trò.

## Logic

- Được tạo khi user chọn vai trò trong onboarding hoặc bật thêm vai trò sau này.
- Các hành vi phụ thuộc vai trò như swipe, portfolio, booking phải đi qua `USER_ROLES` để biết user đang hành động ở ngữ cảnh nào.
- Mỗi user không nên có trùng cùng một role; nên có unique constraint trên `(user_id, role_id)`.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `ROLES`. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `is_primary` | `boolean` | Không | Đánh dấu vai trò chính khi user có nhiều vai trò. |
| `selected_at` | `datetime` | Không | Thời điểm người dùng chọn giá trị này. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `USER_ROLES` (đúng 1 → 0..n) | Bảng `USERS` sở hữu `USER_ROLES`; nhìn từ `USER_ROLES`, đây là quan hệ ngược về `USERS`. | USER_ROLES.user_id -> USERS.id |
| `ROLES` | `ROLES` \|\|--o{ `USER_ROLES` (đúng 1 → 0..n) | Bảng `ROLES` gán vào `USER_ROLES`; nhìn từ `USER_ROLES`, đây là quan hệ ngược về `ROLES`. | USER_ROLES.role_id -> ROLES.id |
| `PHOTOGRAPHER_PROFILES` | `USER_ROLES` \|\|--o\| `PHOTOGRAPHER_PROFILES` (đúng 1 → 0..1) | Một bản ghi `USER_ROLES` mở rộng thành `PHOTOGRAPHER_PROFILES`. | PHOTOGRAPHER_PROFILES.user_role_id -> USER_ROLES.id |
| `DISCOVERY_PRESENCE` | `USER_ROLES` \|\|--o\| `DISCOVERY_PRESENCE` (đúng 1 → 0..1) | Một bản ghi `USER_ROLES` công bố trạng thái hiển thị `DISCOVERY_PRESENCE`. | DISCOVERY_PRESENCE.user_role_id -> USER_ROLES.id |
| `USER_ROLE_FIELDS` | `USER_ROLES` \|\|--o{ `USER_ROLE_FIELDS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` chọn `USER_ROLE_FIELDS`. | USER_ROLE_FIELDS.user_role_id -> USER_ROLES.id |
| `USER_ROLE_SERVICES` | `USER_ROLES` \|\|--o{ `USER_ROLE_SERVICES` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` cung cấp hoặc có nhu cầu `USER_ROLE_SERVICES`. | USER_ROLE_SERVICES.user_role_id -> USER_ROLES.id |
| `PORTFOLIO_ITEMS` | `USER_ROLES` \|\|--o{ `PORTFOLIO_ITEMS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` sở hữu `PORTFOLIO_ITEMS`. | PORTFOLIO_ITEMS.photographer_role_id -> USER_ROLES.id |
| `DISCOVERY_FILTERS` | `USER_ROLES` \|\|--o{ `DISCOVERY_FILTERS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` tạo `DISCOVERY_FILTERS`. | DISCOVERY_FILTERS.owner_user_role_id -> USER_ROLES.id |
| `SWIPES` | `USER_ROLES` \|\|--o{ `SWIPES` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` thực hiện `SWIPES`. | SWIPES.actor_user_role_id -> USER_ROLES.id |
| `SWIPES` | `USER_ROLES` \|\|--o{ `SWIPES` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` nhận `SWIPES`. | SWIPES.target_user_role_id -> USER_ROLES.id |
| `MATCHES` | `USER_ROLES` \|\|--o{ `MATCHES` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` tham gia với vai trò phía A `MATCHES`. | MATCHES.user_role_a_id -> USER_ROLES.id |
| `MATCHES` | `USER_ROLES` \|\|--o{ `MATCHES` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` tham gia với vai trò phía B `MATCHES`. | MATCHES.user_role_b_id -> USER_ROLES.id |
| `SHOOT_REQUESTS` | `USER_ROLES` \|\|--o{ `SHOOT_REQUESTS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` tạo `SHOOT_REQUESTS`. | SHOOT_REQUESTS.customer_role_id -> USER_ROLES.id |
| `BOOKINGS` | `USER_ROLES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` đặt lịch với vai trò khách hàng `BOOKINGS`. | BOOKINGS.customer_role_id -> USER_ROLES.id |
| `BOOKINGS` | `USER_ROLES` \|\|--o{ `BOOKINGS` (đúng 1 → 0..n) | Một bản ghi `USER_ROLES` nhận lịch với vai trò thợ ảnh `BOOKINGS`. | BOOKINGS.photographer_role_id -> USER_ROLES.id |

## Ghi chú nghiệp vụ

- Các hành vi phụ thuộc vai trò như swipe, booking, portfolio nên tham chiếu `USER_ROLES` thay vì trực tiếp tới `USERS`.
