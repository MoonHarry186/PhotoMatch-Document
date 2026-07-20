# PORTFOLIO_ITEMS

## Mục đích

Lưu ảnh hoặc hạng mục portfolio của thợ ảnh, có thể gắn với dịch vụ cụ thể.

## Logic

- Chỉ photographer role được tạo portfolio item.
- Portfolio item bị xóa nên dùng `deleted_at` để giữ lịch sử liên quan tới booking hoặc report.
- `sort_order` quyết định thứ tự hiển thị trong trang portfolio.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `photographer_role_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `USER_ROLES`. |
| `service_id` | `uuid` | Khóa ngoại | Khóa ngoại tham chiếu `SERVICES`. |
| `media_url` | `varchar` | Không | URL ảnh hoặc media chính trong portfolio. |
| `thumbnail_url` | `varchar` | Không | URL ảnh thumbnail để hiển thị danh sách. |
| `title` | `varchar` | Không | Tiêu đề hiển thị. |
| `description` | `text` | Không | Mô tả chi tiết. |
| `sort_order` | `int` | Không | Thứ tự sắp xếp khi hiển thị. |
| `status` | `varchar` | Không | Trạng thái hiện tại của bản ghi. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |
| `deleted_at` | `datetime` | Không | Thời điểm xóa mềm bản ghi, nếu có. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_ROLES` | `USER_ROLES` \|\|--o{ `PORTFOLIO_ITEMS` (đúng 1 → 0..n) | Bảng `USER_ROLES` sở hữu `PORTFOLIO_ITEMS`; nhìn từ `PORTFOLIO_ITEMS`, đây là quan hệ ngược về `USER_ROLES`. | PORTFOLIO_ITEMS.photographer_role_id -> USER_ROLES.id |
| `SERVICES` | `SERVICES` o\|--o{ `PORTFOLIO_ITEMS` (0..1 → 0..n) | Bảng `SERVICES` phân loại `PORTFOLIO_ITEMS`; nhìn từ `PORTFOLIO_ITEMS`, đây là quan hệ ngược về `SERVICES`. | PORTFOLIO_ITEMS.service_id -> SERVICES.id |
