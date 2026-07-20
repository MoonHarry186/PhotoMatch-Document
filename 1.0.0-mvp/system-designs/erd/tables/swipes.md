# SWIPES

## Mục đích

Lưu hành động quẹt hoặc phản hồi giữa hai vai trò trong luồng discovery.

## Logic

- Ghi nhận hành động của actor role lên target role trong discovery.
- Không cho phép actor và target là cùng một user role; cũng nên tránh swipe trùng cùng một cặp.
- Customer quan tâm và photographer chấp nhận là tín hiệu để tạo `MATCHES` theo rule MVP.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `actor_user_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `target_user_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `discovery_filter_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `DISCOVERY_FILTERS`. |
| `direction` | `varchar` | Không | Hướng hoặc loại hành động quẹt, ví dụ left, right, accept hoặc reject. |
| `source` | `varchar` | Không | Nguồn tạo hành động, ví dụ discovery feed hoặc nearby map. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `SWIPES` (đúng 1 → 0..n) | Bảng `USER_ROLES` thực hiện `SWIPES`; nhìn từ `SWIPES`, đây là quan hệ ngược về `USER_ROLES`. | SWIPES.actor_user_role_id -> USER_ROLES.id |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `SWIPES` (đúng 1 → 0..n) | Bảng `USER_ROLES` nhận `SWIPES`; nhìn từ `SWIPES`, đây là quan hệ ngược về `USER_ROLES`. | SWIPES.target_user_role_id -> USER_ROLES.id |
| `DISCOVERY_FILTERS` | `DISCOVERY_FILTERS` o\|--o{ `SWIPES` (0..1 → 0..n) | Bảng `DISCOVERY_FILTERS` được tạo từ `SWIPES`; nhìn từ `SWIPES`, đây là quan hệ ngược về `DISCOVERY_FILTERS`. | SWIPES.discovery_filter_id -> DISCOVERY_FILTERS.id |
