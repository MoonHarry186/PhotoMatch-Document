# DISCOVERY_FILTERS

## Mục đích

Lưu bộ lọc tìm kiếm/quẹt của một vai trò, gồm vị trí, bán kính, giá và trạng thái.

## Logic

- Lưu bộ lọc do user tạo hoặc bộ lọc mặc định cho một role.
- Khi `is_default = true`, nên đảm bảo mỗi owner/target context chỉ có một bộ lọc mặc định.
- Filter phải validate bán kính, khoảng giá và target role trước khi dùng để lấy candidate.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `owner_user_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `target_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `ROLES`. |
| `city_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `CITIES`. |
| `name` | `varchar` | Không | Tên hiển thị. |
| `search_latitude` | `decimal` | Không | Vĩ độ tâm tìm kiếm. |
| `search_longitude` | `decimal` | Không | Kinh độ tâm tìm kiếm. |
| `radius_km` | `decimal` | Không | Bán kính tìm kiếm tính theo kilomet. |
| `min_price` | `decimal` | Không | Giá tối thiểu mong muốn hoặc cung cấp. |
| `max_price` | `decimal` | Không | Giá tối đa mong muốn hoặc cung cấp. |
| `available_only` | `boolean` | Không | Chỉ lấy hồ sơ đang available. |
| `verified_only` | `boolean` | Không | Chỉ lấy hồ sơ đã xác minh. |
| `is_default` | `boolean` | Không | Đánh dấu đây là cấu hình mặc định. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `DISCOVERY_FILTERS` (đúng 1 → 0..n) | Bảng `USER_ROLES` tạo `DISCOVERY_FILTERS`; nhìn từ `DISCOVERY_FILTERS`, đây là quan hệ ngược về `USER_ROLES`. | DISCOVERY_FILTERS.owner_user_role_id -> USER_ROLES.id |
| `ROLES` | `ROLES` \|\|--o{ `DISCOVERY_FILTERS` (đúng 1 → 0..n) | Bảng `ROLES` nhắm tới `DISCOVERY_FILTERS`; nhìn từ `DISCOVERY_FILTERS`, đây là quan hệ ngược về `ROLES`. | DISCOVERY_FILTERS.target_role_id -> ROLES.id |
| `CITIES` | `CITIES` o\|--o{ `DISCOVERY_FILTERS` (0..1 → 0..n) | Bảng `CITIES` tìm kiếm trong `DISCOVERY_FILTERS`; nhìn từ `DISCOVERY_FILTERS`, đây là quan hệ ngược về `CITIES`. | DISCOVERY_FILTERS.city_id -> CITIES.id |
| `DISCOVERY_FILTER_SERVICES` | `DISCOVERY_FILTERS` \|\|--o{ `DISCOVERY_FILTER_SERVICES` (đúng 1 → 0..n) | Một bản ghi `DISCOVERY_FILTERS` bao gồm `DISCOVERY_FILTER_SERVICES`. | DISCOVERY_FILTER_SERVICES.discovery_filter_id -> DISCOVERY_FILTERS.id |
| `SWIPES` | `DISCOVERY_FILTERS` o\|--o{ `SWIPES` (0..1 → 0..n) | Một bản ghi `DISCOVERY_FILTERS` được tạo từ `SWIPES`. | SWIPES.discovery_filter_id -> DISCOVERY_FILTERS.id |
