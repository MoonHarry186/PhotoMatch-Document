# USER_SETTINGS

## Mục đích

Lưu thiết lập cá nhân, quyền riêng tư, giao diện và cấu hình hiển thị vị trí.

## Logic

- Tạo mặc định cùng hoặc ngay sau khi tạo user để app luôn có cấu hình đọc được.
- Các cờ `push_enabled`, `location_enabled`, `profile_discoverable` phải được kiểm tra trước khi gửi thông báo hoặc hiển thị hồ sơ.
- `location_visibility_duration_hours` là giá trị mặc định để tính thời hạn hiển thị vị trí khi user bật discovery.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `language` | `varchar` | Không | Ngôn ngữ giao diện người dùng chọn. |
| `theme` | `varchar` | Không | Giao diện sáng, tối hoặc theo hệ thống. |
| `map_type` | `varchar` | Không | Trường dormant tương thích dữ liệu cũ; map type deferred sau MVP theo `CR-002`. |
| `push_enabled` | `boolean` | Không | Cho phép nhận push notification. |
| `location_enabled` | `boolean` | Không | Cho phép sử dụng vị trí. |
| `profile_discoverable` | `boolean` | Không | Cho phép hồ sơ xuất hiện trong discovery. |
| `location_visibility_duration_hours` | `int` | Không | Thời lượng mặc định khi bật hiển thị vị trí. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--\|\| `USER_SETTINGS` (đúng 1 → đúng 1) | Bảng `USERS` cấu hình `USER_SETTINGS`; nhìn từ `USER_SETTINGS`, đây là quan hệ ngược về `USERS`. | USER_SETTINGS.user_id -> USERS.id |
