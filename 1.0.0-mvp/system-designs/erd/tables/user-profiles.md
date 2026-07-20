# USER_PROFILES

## Mục đích

Lưu hồ sơ cá nhân dùng chung như tên hiển thị, avatar, thành phố và giới thiệu.

## Logic

- Tạo sau khi user bắt đầu onboarding và cập nhật dần qua các màn hình thông tin cá nhân, avatar, thành phố.
- Thông tin trong bảng này dùng chung cho cả customer và photographer, tránh nhân đôi tên, avatar, bio.
- `profile_status` quyết định hồ sơ đã đủ điều kiện hiển thị hay vẫn đang draft/hidden.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `city_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `CITIES`. |
| `display_name` | `varchar` | Không | Tên hiển thị trên hồ sơ và trong giao tiếp. |
| `avatar_url` | `varchar` | Không | URL ảnh đại diện. |
| `bio` | `text` | Không | Phần giới thiệu ngắn của người dùng. |
| `date_of_birth` | `date` | Không | Ngày sinh; dùng cho kiểm tra tuổi hoặc hồ sơ. |
| `profile_status` | `varchar` | Không | Trạng thái hồ sơ, ví dụ draft, active hoặc hidden. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USERS` | `USERS` \|\|--\|\| `USER_PROFILES` (đúng 1 → đúng 1) | Bảng `USERS` sở hữu hồ sơ `USER_PROFILES`; nhìn từ `USER_PROFILES`, đây là quan hệ ngược về `USERS`. | USER_PROFILES.user_id -> USERS.id |
| `CITIES` | `CITIES` o\|--o{ `USER_PROFILES` (0..1 → 0..n) | Bảng `CITIES` là thành phố cư trú của `USER_PROFILES`; nhìn từ `USER_PROFILES`, đây là quan hệ ngược về `CITIES`. | USER_PROFILES.city_id -> CITIES.id |
