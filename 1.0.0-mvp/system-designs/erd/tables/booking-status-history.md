# BOOKING_STATUS_HISTORY

## Mục đích

Lưu lịch sử thay đổi trạng thái của booking.

## Logic

- Tạo một dòng mới cho mỗi lần booking đổi trạng thái.
- `previous_status` và `new_status` phải phản ánh đúng transition được backend cho phép.
- Dùng làm audit trail để admin hoặc hai bên xem lại lịch sử xử lý booking.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `booking_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `BOOKINGS`. |
| `changed_by_user_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `previous_status` | `varchar` | Không | Trạng thái trước khi thay đổi. |
| `new_status` | `varchar` | Không | Trạng thái mới sau khi thay đổi. |
| `note` | `text` | Không | Ghi chú bổ sung. |
| `changed_at` | `datetime` | Không | Thời điểm thay đổi trạng thái. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `BOOKINGS` | `BOOKINGS` \|\|--o{ `BOOKING_STATUS_HISTORY` (đúng 1 → 0..n) | Bảng `BOOKINGS` có lịch sử trạng thái `BOOKING_STATUS_HISTORY`; nhìn từ `BOOKING_STATUS_HISTORY`, đây là quan hệ ngược về `BOOKINGS`. | BOOKING_STATUS_HISTORY.booking_id -> BOOKINGS.id |
| `USERS` | `USERS` \|\|--o{ `BOOKING_STATUS_HISTORY` (đúng 1 → 0..n) | Bảng `USERS` thực hiện thay đổi `BOOKING_STATUS_HISTORY`; nhìn từ `BOOKING_STATUS_HISTORY`, đây là quan hệ ngược về `USERS`. | BOOKING_STATUS_HISTORY.changed_by_user_id -> USERS.id |
