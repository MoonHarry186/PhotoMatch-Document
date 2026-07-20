# PHOTOGRAPHER_PROFILES

## Mục đích

Lưu thông tin mở rộng chỉ dành cho vai trò thợ ảnh.

## Logic

- Chỉ tạo cho `USER_ROLES` có role photographer; không tạo cho customer role.
- `availability_status` ảnh hưởng tới discovery, filter available-only và khả năng nhận booking.
- `verification_status` có thể dùng để ưu tiên hoặc hạn chế hiển thị thợ ảnh chưa xác minh.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_role_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `headline` | `varchar` | Không | Dòng giới thiệu ngắn của thợ ảnh. |
| `working_location` | `varchar` | Không | Khu vực hoạt động chính dạng text. |
| `working_radius_km` | `decimal` | Không | Bán kính nhận việc tính theo kilomet. |
| `years_experience` | `int` | Không | Số năm kinh nghiệm. |
| `availability_status` | `varchar` | Không | Trạng thái sẵn sàng nhận việc. |
| `verification_status` | `varchar` | Không | Trạng thái xác minh của hồ sơ hoặc vai trò. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o\| `PHOTOGRAPHER_PROFILES` (đúng 1 → 0..1) | Bảng `USER_ROLES` mở rộng thành `PHOTOGRAPHER_PROFILES`; nhìn từ `PHOTOGRAPHER_PROFILES`, đây là quan hệ ngược về `USER_ROLES`. | PHOTOGRAPHER_PROFILES.user_role_id -> USER_ROLES.id |
