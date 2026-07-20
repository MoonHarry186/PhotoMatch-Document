# CITIES

## Mục đích

Danh mục tỉnh/thành phố phục vụ hồ sơ, vị trí, bộ lọc và yêu cầu chụp.

## Logic

- Là dữ liệu danh mục phục vụ hồ sơ, vị trí, bộ lọc và shoot request.
- Chỉ city có `status` active mới nên xuất hiện trong form chọn hoặc filter.
- Tọa độ trung tâm dùng làm fallback cho map/search khi chưa có GPS chính xác.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `province_code` | `varchar` | Không | Mã tỉnh/thành phố theo quy ước nội bộ hoặc dữ liệu hành chính. |
| `name` | `varchar` | Không | Tên hiển thị. |
| `center_latitude` | `decimal` | Không | Vĩ độ trung tâm thành phố. |
| `center_longitude` | `decimal` | Không | Kinh độ trung tâm thành phố. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_PROFILES` | `CITIES` o\|--o{ `USER_PROFILES` (0..1 → 0..n) | Một bản ghi `CITIES` là thành phố cư trú của `USER_PROFILES`. | USER_PROFILES.city_id -> CITIES.id |
| `USER_LOCATIONS` | `CITIES` o\|--o{ `USER_LOCATIONS` (0..1 → 0..n) | Một bản ghi `CITIES` thuộc về `USER_LOCATIONS`. | USER_LOCATIONS.city_id -> CITIES.id |
| `DISCOVERY_FILTERS` | `CITIES` o\|--o{ `DISCOVERY_FILTERS` (0..1 → 0..n) | Một bản ghi `CITIES` tìm kiếm trong `DISCOVERY_FILTERS`. | DISCOVERY_FILTERS.city_id -> CITIES.id |
| `SHOOT_REQUESTS` | `CITIES` o\|--o{ `SHOOT_REQUESTS` (0..1 → 0..n) | Một bản ghi `CITIES` diễn ra tại `SHOOT_REQUESTS`. | SHOOT_REQUESTS.city_id -> CITIES.id |
