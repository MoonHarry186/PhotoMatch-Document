# USER_CONSENTS

## Mục đích

Ghi nhận người dùng đã đồng ý với phiên bản tài liệu pháp lý nào.

## Logic

- Tạo khi user chấp nhận một version tài liệu pháp lý.
- Cặp `(user_id, legal_document_id)` nên unique để tránh ghi nhận trùng một lần chấp nhận.
- `accepted_at` và `ip_address` phục vụ audit khi cần chứng minh consent.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `user_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `USERS`. |
| `legal_document_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `LEGAL_DOCUMENTS`. |
| `ip_address` | `varchar` | Không | Địa chỉ IP liên quan tới hành động. |
| `accepted_at` | `datetime` | Không | Thời điểm người dùng chấp nhận tài liệu pháp lý. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `LEGAL_DOCUMENTS` | `LEGAL_DOCUMENTS` \|\|--o{ `USER_CONSENTS` (đúng 1 → 0..n) | Bảng `LEGAL_DOCUMENTS` được chấp nhận dưới dạng `USER_CONSENTS`; nhìn từ `USER_CONSENTS`, đây là quan hệ ngược về `LEGAL_DOCUMENTS`. | USER_CONSENTS.legal_document_id -> LEGAL_DOCUMENTS.id |
| `USERS` | `USERS` \|\|--o{ `USER_CONSENTS` (đúng 1 → 0..n) | Bảng `USERS` chấp nhận `USER_CONSENTS`; nhìn từ `USER_CONSENTS`, đây là quan hệ ngược về `USERS`. | USER_CONSENTS.user_id -> USERS.id |

## Ghi chú nghiệp vụ

- Khi nội dung pháp lý đổi version, hệ thống có thể yêu cầu user chấp nhận lại bằng bản ghi mới.
