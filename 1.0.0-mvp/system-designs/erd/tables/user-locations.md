# USER_LOCATIONS

## Mục đích

Lưu vị trí GPS hiện tại hoặc lịch sử vị trí của người dùng.

## Logic

- Tạo mới khi app ghi nhận vị trí từ thiết bị; chỉ một vị trí nên có `is_current = true` cho mỗi user.
- Vị trí thật chỉ dùng cho tính toán backend; dữ liệu trả về công khai phải đi qua `DISCOVERY_PRESENCE` đã làm lệch.
- Không dùng bản ghi đã quá `expires_at` để hiển thị quanh đây hoặc swipe.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `city_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `CITIES`. |
| `latitude` | `decimal` | Không | Vĩ độ của địa điểm hoặc vị trí liên quan. |
| `longitude` | `decimal` | Không | Kinh độ của địa điểm hoặc vị trí liên quan. |
| `geohash` | `varchar` | Không | Geohash phục vụ tìm kiếm vị trí nhanh. |
| `accuracy_meters` | `decimal` | Không | Độ chính xác vị trí tính bằng mét. |
| `is_current` | `boolean` | Không | Đánh dấu đây là vị trí hiện tại của user. |
| `captured_at` | `datetime` | Không | Thời điểm thiết bị ghi nhận vị trí. |
| `expires_at` | `datetime` | Không | Thời điểm bản ghi hoặc token hết hạn. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--o{ `USER_LOCATIONS` (đúng 1 → 0..n) | Bảng `USERS` ghi nhận `USER_LOCATIONS`; nhìn từ `USER_LOCATIONS`, đây là quan hệ ngược về `USERS`. | USER_LOCATIONS.user_id -> USERS.id |
| `CITIES` | `CITIES` o\|--o{ `USER_LOCATIONS` (0..1 → 0..n) | Bảng `CITIES` thuộc về `USER_LOCATIONS`; nhìn từ `USER_LOCATIONS`, đây là quan hệ ngược về `CITIES`. | USER_LOCATIONS.city_id -> CITIES.id |
| `DISCOVERY_PRESENCE` | `USER_LOCATIONS` o\|--o{ `DISCOVERY_PRESENCE` (0..1 → 0..n) | Một bản ghi `USER_LOCATIONS` sử dụng vị trí hiện tại `DISCOVERY_PRESENCE`. | DISCOVERY_PRESENCE.current_location_id -> USER_LOCATIONS.id |
