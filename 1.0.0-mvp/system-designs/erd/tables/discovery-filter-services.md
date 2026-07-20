# DISCOVERY_FILTER_SERVICES

## Mục đích

Bảng nối nhiều-nhiều giữa bộ lọc discovery và dịch vụ.

## Logic

- Là bảng nối giữa filter và service; mỗi cặp filter-service chỉ nên tồn tại một lần.
- Khi cập nhật service trong filter, có thể xóa và tạo lại các dòng nối theo payload mới.
- Nếu filter không có service nào, hệ thống có thể hiểu là không lọc theo service.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `discovery_filter_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `DISCOVERY_FILTERS`. |
| `service_id` | `uuid` | Khóa chính, Khóa ngoại | Khóa ngoại tham chiếu `SERVICES`. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `DISCOVERY_FILTERS` | `DISCOVERY_FILTERS` \|\|--o{ `DISCOVERY_FILTER_SERVICES` (đúng 1 → 0..n) | Bảng `DISCOVERY_FILTERS` bao gồm `DISCOVERY_FILTER_SERVICES`; nhìn từ `DISCOVERY_FILTER_SERVICES`, đây là quan hệ ngược về `DISCOVERY_FILTERS`. | DISCOVERY_FILTER_SERVICES.discovery_filter_id -> DISCOVERY_FILTERS.id |
| `SERVICES` | `SERVICES` \|\|--o{ `DISCOVERY_FILTER_SERVICES` (đúng 1 → 0..n) | Bảng `SERVICES` lọc theo `DISCOVERY_FILTER_SERVICES`; nhìn từ `DISCOVERY_FILTER_SERVICES`, đây là quan hệ ngược về `SERVICES`. | DISCOVERY_FILTER_SERVICES.service_id -> SERVICES.id |
