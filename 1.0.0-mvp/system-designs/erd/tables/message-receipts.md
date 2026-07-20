# MESSAGE_RECEIPTS

## Mục đích

Lưu trạng thái giao và đọc tin nhắn theo từng người nhận.

## Logic

- Tạo/cập nhật theo từng message và từng người nhận để tracking delivered/read.
- `read_at` không được nhỏ hơn `delivered_at` nếu cả hai cùng tồn tại.
- Dùng để hiển thị trạng thái đã đọc và tính số tin chưa đọc trong inbox.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `message_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `MESSAGES`. |
| `user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `delivered_at` | `datetime` | Không | Thời điểm tin nhắn được giao tới người nhận. |
| `read_at` | `datetime` | Không | Thời điểm người nhận đọc tin nhắn hoặc thông báo. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `MESSAGES` | `MESSAGES` \|\|--o{ `MESSAGE_RECEIPTS` (đúng 1 → 0..n) | Bảng `MESSAGES` sở hữu hồ sơ `MESSAGE_RECEIPTS`; nhìn từ `MESSAGE_RECEIPTS`, đây là quan hệ ngược về `MESSAGES`. | MESSAGE_RECEIPTS.message_id -> MESSAGES.id |
| `USERS` | `USERS` \|\|--o{ `MESSAGE_RECEIPTS` (đúng 1 → 0..n) | Bảng `USERS` nhận `MESSAGE_RECEIPTS`; nhìn từ `MESSAGE_RECEIPTS`, đây là quan hệ ngược về `USERS`. | MESSAGE_RECEIPTS.user_id -> USERS.id |
