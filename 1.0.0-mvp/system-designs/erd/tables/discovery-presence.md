# DISCOVERY_PRESENCE

## Mục đích

Xác định một vai trò có đang xuất hiện trong Nearby hoặc màn hình quẹt hay không.

## Logic

- Được cập nhật khi user bật/tắt discovery presence.
- Chỉ presence còn hạn `visible_until` và đáp ứng eligibility mới được đưa vào Nearby/discovery.
- Public point đã làm lệch chỉ được backend dùng để query và tính distance bucket; không trả tọa độ cho mobile trong MVP.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_role_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `current_location_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_LOCATIONS`. |
| `is_visible` | `boolean` | Không | Cho phép vai trò này xuất hiện trong Nearby/discovery khi còn hạn và đủ eligibility. |
| `available_now` | `boolean` | Không | Đánh dấu đang sẵn sàng tại thời điểm hiện tại. |
| `public_radius_meters` | `decimal` | Không | Bán kính làm mờ chỉ backend sử dụng để tạo distance bucket trong MVP. |
| `public_latitude` | `decimal` | Không | Vĩ độ đã làm lệch, chỉ backend sử dụng trong MVP. |
| `public_longitude` | `decimal` | Không | Kinh độ đã làm lệch, chỉ backend sử dụng trong MVP. |
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

- Public point không được là GPS thật. Mobile MVP chỉ nhận distance bucket, không nhận `public_latitude` hoặc `public_longitude`.
