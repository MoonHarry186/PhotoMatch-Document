# USERS

## Mục đích

Lưu thông tin tài khoản cốt lõi, trạng thái hoạt động và trạng thái xác thực danh tính của người dùng.

## Logic

- Tạo bản ghi khi người dùng đăng ký thành công; các bảng hồ sơ, setting và role sẽ được tạo theo các bước tiếp theo của onboarding.
- `account_status` là cổng kiểm soát truy cập chính: tài khoản bị khóa hoặc bị phạt không được đăng nhập, chat, swipe hoặc booking.
- Không xóa cứng user trong luồng thông thường; dùng `deleted_at` để xóa mềm và giữ lại lịch sử giao dịch, report, booking.

## Các trường

| Trường | Kiểu | Ràng buộc | Giải thích |
| :---- | :---- | :---- | :---- |
| `id` | `uuid` | Khóa chính | Khóa chính định danh duy nhất của bản ghi. |
| `email` | `varchar` | Unique | Email đăng nhập hoặc liên hệ của tài khoản; cần unique khi có giá trị. |
| `phone` | `varchar` | Unique | Số điện thoại của tài khoản; cần unique khi có giá trị. |
| `account_status` | `varchar` | Không | Trạng thái hoạt động của tài khoản, ví dụ active, suspended hoặc banned. |
| `identity_verification_status` | `varchar` | Không | Trạng thái xác minh danh tính hiện tại của tài khoản. |
| `email_verified` | `boolean` | Không | Đánh dấu email đã được xác minh. |
| `phone_verified` | `boolean` | Không | Đánh dấu số điện thoại đã được xác minh. |
| `last_login_at` | `datetime` | Không | Thời điểm đăng nhập gần nhất. |
| `created_at` | `datetime` | Không | Thời điểm bản ghi được tạo. |
| `updated_at` | `datetime` | Không | Thời điểm bản ghi được cập nhật gần nhất. |
| `deleted_at` | `datetime` | Không | Thời điểm xóa mềm bản ghi, nếu có. |

## Quan hệ

| Bảng liên quan | Cardinality | Ý nghĩa | Khóa liên kết |
| :---- | :---- | :---- | :---- |
| `USER_PROFILES` | `USERS` \|\|--\|\| `USER_PROFILES` (đúng 1 → đúng 1) | Một bản ghi `USERS` sở hữu hồ sơ `USER_PROFILES`. | USER_PROFILES.user_id -> USERS.id |
| `AUTH_IDENTITIES` | `USERS` \|\|--o{ `AUTH_IDENTITIES` (đúng 1 → 0..n) | Một bản ghi `USERS` đăng nhập bằng `AUTH_IDENTITIES`. | AUTH_IDENTITIES.user_id -> USERS.id |
| `AUTH_SESSIONS` | `USERS` \|\|--o{ `AUTH_SESSIONS` (đúng 1 → 0..n) | Một bản ghi `USERS` tạo `AUTH_SESSIONS`. | AUTH_SESSIONS.user_id -> USERS.id |
| `USER_ROLES` | `USERS` \|\|--o{ `USER_ROLES` (đúng 1 → 0..n) | Một bản ghi `USERS` sở hữu `USER_ROLES`. | USER_ROLES.user_id -> USERS.id |
| `USER_SETTINGS` | `USERS` \|\|--\|\| `USER_SETTINGS` (đúng 1 → đúng 1) | Một bản ghi `USERS` cấu hình `USER_SETTINGS`. | USER_SETTINGS.user_id -> USERS.id |
| `USER_LOCATIONS` | `USERS` \|\|--o{ `USER_LOCATIONS` (đúng 1 → 0..n) | Một bản ghi `USERS` ghi nhận `USER_LOCATIONS`. | USER_LOCATIONS.user_id -> USERS.id |
| `CONVERSATION_PARTICIPANTS` | `USERS` \|\|--o{ `CONVERSATION_PARTICIPANTS` (đúng 1 → 0..n) | Một bản ghi `USERS` tham gia `CONVERSATION_PARTICIPANTS`. | CONVERSATION_PARTICIPANTS.user_id -> USERS.id |
| `MESSAGES` | `USERS` \|\|--o{ `MESSAGES` (đúng 1 → 0..n) | Một bản ghi `USERS` gửi `MESSAGES`. | MESSAGES.sender_user_id -> USERS.id |
| `MESSAGE_RECEIPTS` | `USERS` \|\|--o{ `MESSAGE_RECEIPTS` (đúng 1 → 0..n) | Một bản ghi `USERS` nhận `MESSAGE_RECEIPTS`. | MESSAGE_RECEIPTS.user_id -> USERS.id |
| `USER_BLOCKS` | `USERS` \|\|--o{ `USER_BLOCKS` (đúng 1 → 0..n) | Một bản ghi `USERS` chặn `USER_BLOCKS`. | USER_BLOCKS.blocker_user_id -> USERS.id |
| `USER_BLOCKS` | `USERS` \|\|--o{ `USER_BLOCKS` (đúng 1 → 0..n) | Một bản ghi `USERS` bị chặn `USER_BLOCKS`. | USER_BLOCKS.blocked_user_id -> USERS.id |
| `BOOKING_STATUS_HISTORY` | `USERS` \|\|--o{ `BOOKING_STATUS_HISTORY` (đúng 1 → 0..n) | Một bản ghi `USERS` thực hiện thay đổi `BOOKING_STATUS_HISTORY`. | BOOKING_STATUS_HISTORY.changed_by_user_id -> USERS.id |
| `REVIEWS` | `USERS` \|\|--o{ `REVIEWS` (đúng 1 → 0..n) | Một bản ghi `USERS` viết `REVIEWS`. | REVIEWS.reviewer_user_id -> USERS.id |
| `REVIEWS` | `USERS` \|\|--o{ `REVIEWS` (đúng 1 → 0..n) | Một bản ghi `USERS` được đánh giá `REVIEWS`. | REVIEWS.reviewee_user_id -> USERS.id |
| `REVIEWS` | `USERS` o\|--o{ `REVIEWS` (0..1 → 0..n) | Một Admin có thể moderate nhiều review. | REVIEWS.moderated_by_user_id -> USERS.id |
| `NOTIFICATIONS` | `USERS` \|\|--o{ `NOTIFICATIONS` (đúng 1 → 0..n) | Một bản ghi `USERS` nhận `NOTIFICATIONS`. | NOTIFICATIONS.recipient_user_id -> USERS.id |
| `NOTIFICATIONS` | `USERS` o\|--o{ `NOTIFICATIONS` (0..1 → 0..n) | Một bản ghi `USERS` kích hoạt `NOTIFICATIONS`. | NOTIFICATIONS.actor_user_id -> USERS.id |
| `IDENTITY_VERIFICATIONS` | `USERS` \|\|--o{ `IDENTITY_VERIFICATIONS` (đúng 1 → 0..n) | Một bản ghi `USERS` gửi yêu cầu xác minh `IDENTITY_VERIFICATIONS`. | IDENTITY_VERIFICATIONS.user_id -> USERS.id |
| `USER_REPORTS` | `USERS` \|\|--o{ `USER_REPORTS` (đúng 1 → 0..n) | Một bản ghi `USERS` báo cáo `USER_REPORTS`. | USER_REPORTS.reporter_user_id -> USERS.id |
| `USER_REPORTS` | `USERS` \|\|--o{ `USER_REPORTS` (đúng 1 → 0..n) | Một bản ghi `USERS` bị báo cáo `USER_REPORTS`. | USER_REPORTS.reported_user_id -> USERS.id |
| `USER_REPORTS` | `USERS` o\|--o{ `USER_REPORTS` (0..1 → 0..n) | Một bản ghi `USERS` xử lý `USER_REPORTS`. | USER_REPORTS.resolved_by_user_id -> USERS.id |
| `ACCOUNT_PENALTIES` | `USERS` \|\|--o{ `ACCOUNT_PENALTIES` (đúng 1 → 0..n) | Một bản ghi `USERS` nhận `ACCOUNT_PENALTIES`. | ACCOUNT_PENALTIES.user_id -> USERS.id |
| `ACCOUNT_PENALTIES` | `USERS` \|\|--o{ `ACCOUNT_PENALTIES` (đúng 1 → 0..n) | Một bản ghi `USERS` áp dụng hình phạt `ACCOUNT_PENALTIES`. | ACCOUNT_PENALTIES.imposed_by_user_id -> USERS.id |
| `ACCOUNT_PENALTIES` | `USERS` o\|--o{ `ACCOUNT_PENALTIES` (0..1 → 0..n) | Một Admin có thể thu hồi nhiều penalty. | ACCOUNT_PENALTIES.revoked_by_user_id -> USERS.id |
| `USER_CONSENTS` | `USERS` \|\|--o{ `USER_CONSENTS` (đúng 1 → 0..n) | Một bản ghi `USERS` chấp nhận `USER_CONSENTS`. | USER_CONSENTS.user_id -> USERS.id |

## Ghi chú nghiệp vụ

- `account_status` kết hợp với `ACCOUNT_PENALTIES` để xác định màn hình khóa/phạt.
- `identity_verification_status` là trạng thái hiện tại; lịch sử chi tiết nằm ở `IDENTITY_VERIFICATIONS`.
