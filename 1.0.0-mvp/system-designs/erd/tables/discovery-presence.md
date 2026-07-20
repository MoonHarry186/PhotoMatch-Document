# DISCOVERY_PRESENCE

## Mục đích

Xác định một vai trò có đang xuất hiện trên bản đồ hoặc màn hình quẹt hay không.

## Logic

- Được cập nhật khi user bật/tắt xuất hiện trên bản đồ hoặc luồng quẹt.
- Chỉ presence còn hạn `visible_until` và có cờ `show_on_map`/`show_in_swipe` phù hợp mới được đưa vào discovery.
- Tọa độ công khai phải được làm lệch từ vị trí thật để bảo vệ quyền riêng tư.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_role_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `current_location_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_LOCATIONS`. |
| `show_on_map` | `boolean` | Không | Cho phép vai trò này xuất hiện trên bản đồ quanh đây. |
| `show_in_swipe` | `boolean` | Không | Cho phép vai trò này xuất hiện trong luồng quẹt. |
| `available_now` | `boolean` | Không | Đánh dấu đang sẵn sàng tại thời điểm hiện tại. |
| `public_radius_meters` | `decimal` | Không | Bán kính công khai dùng để hiển thị vị trí mờ. |
| `public_latitude` | `decimal` | Không | Vĩ độ công khai đã được làm lệch. |
| `public_longitude` | `decimal` | Không | Kinh độ công khai đã được làm lệch. |
| `location_noise_meters` | `decimal` | Không | Độ lệch đã áp dụng để bảo vệ GPS thật. |
| `visible_until` | `datetime` | Không | Thời điểm hết hiển thị trong discovery. |
| `last_active_at` | `datetime` | Không | Lần cuối vai trò hoạt động trong discovery. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o\| `DISCOVERY_PRESENCE` (đúng 1 → 0..1) | Bảng `USER_ROLES` công bố trạng thái hiển thị `DISCOVERY_PRESENCE`; nhìn từ `DISCOVERY_PRESENCE`, đây là quan hệ ngược về `USER_ROLES`. | DISCOVERY_PRESENCE.user_role_id -> USER_ROLES.id |
| `USER_LOCATIONS` | `USER_LOCATIONS` o\|--o{ `DISCOVERY_PRESENCE` (0..1 → 0..n) | Bảng `USER_LOCATIONS` sử dụng vị trí hiện tại `DISCOVERY_PRESENCE`; nhìn từ `DISCOVERY_PRESENCE`, đây là quan hệ ngược về `USER_LOCATIONS`. | DISCOVERY_PRESENCE.current_location_id -> USER_LOCATIONS.id |

## Ghi chú nghiệp vụ

- Tọa độ công khai không nên là GPS thật; dùng `public_latitude`, `public_longitude`, `public_radius_meters` và `location_noise_meters` để bảo vệ riêng tư.
