# LEGAL_DOCUMENTS

## Mục đích

Lưu các phiên bản điều khoản sử dụng và chính sách bảo mật.

## Logic

- Mỗi lần thay đổi điều khoản hoặc chính sách nên tạo version mới thay vì sửa đè version cũ.
- Chỉ document active/effective mới được yêu cầu user chấp nhận.
- Khi có version mới, hệ thống có thể yêu cầu tạo lại `USER_CONSENTS` cho user.
- `(document_type, version)` phải unique; mỗi document type có tối đa một version `ACTIVE` tại một thời điểm.
- Version `ACTIVE` đã effective là bất biến; chỉ version `INACTIVE` mới được sửa, còn xóa dùng trạng thái `ARCHIVED`.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `document_type` | `varchar` | Không | Loại tài liệu hoặc giấy tờ, tùy ngữ cảnh bảng. |
| `version` | `varchar` | Không | Phiên bản tài liệu. |
| `content_url` | `varchar` | Không | URL nội dung tài liệu. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `effective_at` | `datetime` | Không | Thời điểm tài liệu bắt đầu có hiệu lực. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_CONSENTS` | `LEGAL_DOCUMENTS` \|\|--o{ `USER_CONSENTS` (đúng 1 → 0..n) | Một bản ghi `LEGAL_DOCUMENTS` được chấp nhận dưới dạng `USER_CONSENTS`. | USER_CONSENTS.legal_document_id -> LEGAL_DOCUMENTS.id |
