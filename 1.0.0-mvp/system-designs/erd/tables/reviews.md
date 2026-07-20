# REVIEWS

## Mục đích

Lưu đánh giá sau khi booking hoàn thành.

## Logic

- Chỉ customer của booking được đánh giá photographer sau khi booking completed.
- Mỗi booking có tối đa một review để tránh spam đánh giá.
- Rating phải nằm trong khoảng 1-5 và review không được ẩn danh trong MVP.
- Review mới được tạo với trạng thái `PUBLISHED`; chỉ review `PUBLISHED` được tính vào rating tổng hợp và hiển thị công khai.
- Admin chỉ đổi trạng thái moderation, không sửa rating/comment; `HIDDEN`/`REMOVED` phải có moderator, lý do và thời điểm xử lý.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `booking_id` | `uuid` | Unique, khóa ngoại | Mỗi booking có tối đa một review; tham chiếu `BOOKINGS`. |
| `reviewer_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `reviewee_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `moderated_by_user_id` | `uuid` | Tùy chọn, khóa ngoại | Admin gần nhất thay đổi trạng thái moderation. |
| `rating` | `int` | Bắt buộc, check 1-5 | Điểm đánh giá từ 1 đến 5. |
| `comment` | `text` | Tùy chọn | Nội dung đánh giá. |
| `status` | `varchar` | Bắt buộc | `PUBLISHED`, `HIDDEN` hoặc `REMOVED`. |
| `moderation_reason` | `text` | Tùy chọn có điều kiện | Bắt buộc khi trạng thái chuyển sang `HIDDEN` hoặc `REMOVED`. |
| `moderated_at` | `datetime` | Tùy chọn | Thời điểm moderation gần nhất. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `BOOKINGS` | `BOOKINGS` \|\|--o\| `REVIEWS` (đúng 1 → 0..1) | Một booking completed nhận tối đa một review. | REVIEWS.booking_id -> BOOKINGS.id |
| `USERS` | `USERS` \|\|--o{ `REVIEWS` (đúng 1 → 0..n) | Bảng `USERS` viết `REVIEWS`; nhìn từ `REVIEWS`, đây là quan hệ ngược về `USERS`. | REVIEWS.reviewer_user_id -> USERS.id |
| `USERS` | `USERS` \|\|--o{ `REVIEWS` (đúng 1 → 0..n) | Bảng `USERS` được đánh giá `REVIEWS`; nhìn từ `REVIEWS`, đây là quan hệ ngược về `USERS`. | REVIEWS.reviewee_user_id -> USERS.id |
| `USERS` | `USERS` o\|--o{ `REVIEWS` (0..1 → 0..n) | Một Admin có thể moderate nhiều review; review chưa được xử lý có thể chưa có moderator. | REVIEWS.moderated_by_user_id -> USERS.id |

## Ghi chú nghiệp vụ

- Chỉ Customer của booking `COMPLETED` được đánh giá đúng Photographer trong booking; mỗi booking có tối đa một review.
- Admin moderation phải giữ nguyên rating/comment do Customer tạo và lưu metadata xử lý.
