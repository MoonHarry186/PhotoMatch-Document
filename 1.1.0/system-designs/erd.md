# PhotoMatch 1.1.0 — ERD mở rộng Post-MVP

| Thuộc tính | Giá trị |
| :---- | :---- |
| Version | `1.1.0` |
| Baseline | [ERD MVP 1.0.0](../../1.0.0-mvp/system-designs/erd.md) |
| Plan liên quan | [Monetization Plan 1.1.0](../plan/photomatch-monetization-plan.md) |
| Change request | `CR-001 - Photographer Monetization Platform` |
| Trạng thái | Draft |

ERD này kế thừa toàn bộ core marketplace của `1.0.0` và bổ sung ba bounded context:

1. Monetization runtime cho Pro subscription và entitlement.
2. Promotion & analytics cho Boost/Featured.
3. SaaS foundation cho CRM, calendar, quote, contract và client gallery trong các phiên bản sau.

Các bảng SaaS foundation được mô hình hóa để bảo đảm khả năng mở rộng nhưng chưa bắt buộc triển khai trong release `1.1.0`.

```mermaid
erDiagram
    USERS ||--|| USER_PROFILES : has
    USERS ||--o{ AUTH_IDENTITIES : authenticates_with
    USERS ||--o{ AUTH_SESSIONS : creates
    AUTH_IDENTITIES ||--o{ PASSWORD_RESET_TOKENS : receives

    USERS ||--o{ USER_ROLES : owns
    ROLES ||--o{ USER_ROLES : assigns
    USER_ROLES ||--o| PHOTOGRAPHER_PROFILES : extends
    USERS ||--|| USER_SETTINGS : configures

    CITIES o|--o{ USER_PROFILES : is_home_city
    USERS ||--o{ USER_LOCATIONS : records
    CITIES o|--o{ USER_LOCATIONS : belongs_to
    USER_ROLES ||--o| DISCOVERY_PRESENCE : publishes
    USER_LOCATIONS o|--o{ DISCOVERY_PRESENCE : uses_current_location

    ROLES ||--o{ ROLE_ACTIVITY_FIELDS : allows
    ACTIVITY_FIELDS ||--o{ ROLE_ACTIVITY_FIELDS : belongs_to
    USER_ROLES ||--o{ USER_ROLE_FIELDS : selects
    ACTIVITY_FIELDS ||--o{ USER_ROLE_FIELDS : selected_as

    ACTIVITY_FIELDS ||--o{ SERVICES : contains
    USER_ROLES ||--o{ USER_ROLE_SERVICES : offers_or_needs
    SERVICES ||--o{ USER_ROLE_SERVICES : references

    USER_ROLES ||--o{ PORTFOLIO_ITEMS : owns
    SERVICES o|--o{ PORTFOLIO_ITEMS : categorizes

    USER_ROLES ||--o{ DISCOVERY_FILTERS : creates
    ROLES ||--o{ DISCOVERY_FILTERS : targets
    CITIES o|--o{ DISCOVERY_FILTERS : searches_in
    DISCOVERY_FILTERS ||--o{ DISCOVERY_FILTER_SERVICES : includes
    SERVICES ||--o{ DISCOVERY_FILTER_SERVICES : filters_by

    USER_ROLES ||--o{ SWIPES : performs
    USER_ROLES ||--o{ SWIPES : receives
    DISCOVERY_FILTERS o|--o{ SWIPES : generated_from

    USER_ROLES ||--o{ MATCHES : joins_as_side_a
    USER_ROLES ||--o{ MATCHES : joins_as_side_b
    MATCHES ||--o| CONVERSATIONS : opens

    CONVERSATIONS ||--|{ CONVERSATION_PARTICIPANTS : contains
    USERS ||--o{ CONVERSATION_PARTICIPANTS : participates
    CONVERSATIONS ||--o{ MESSAGES : contains
    USERS ||--o{ MESSAGES : sends
    MESSAGES o|--o{ MESSAGES : replies_to
    MESSAGES ||--o{ MESSAGE_RECEIPTS : has
    USERS ||--o{ MESSAGE_RECEIPTS : receives

    USERS ||--o{ USER_BLOCKS : blocks
    USERS ||--o{ USER_BLOCKS : is_blocked

    USER_ROLES ||--o{ SHOOT_REQUESTS : creates
    SERVICES ||--o{ SHOOT_REQUESTS : requests
    CITIES o|--o{ SHOOT_REQUESTS : located_in

    MATCHES o|--o{ BOOKINGS : results_in
    SHOOT_REQUESTS o|--o{ BOOKINGS : becomes
    USER_ROLES ||--o{ BOOKINGS : books_as_customer
    USER_ROLES ||--o{ BOOKINGS : receives_as_photographer
    SERVICES ||--o{ BOOKINGS : books_service

    BOOKINGS ||--o{ BOOKING_STATUS_HISTORY : has_history
    USERS ||--o{ BOOKING_STATUS_HISTORY : changes
    BOOKINGS ||--o{ REVIEWS : receives
    USERS ||--o{ REVIEWS : writes
    USERS ||--o{ REVIEWS : is_reviewed

    USERS ||--o{ NOTIFICATIONS : receives
    USERS o|--o{ NOTIFICATIONS : triggers
    MATCHES o|--o{ NOTIFICATIONS : references
    MESSAGES o|--o{ NOTIFICATIONS : references
    BOOKINGS o|--o{ NOTIFICATIONS : references
    ACCOUNT_PENALTIES o|--o{ NOTIFICATIONS : references

    USERS ||--o{ IDENTITY_VERIFICATIONS : submits

    USERS ||--o{ USER_REPORTS : reports
    USERS ||--o{ USER_REPORTS : is_reported
    USERS o|--o{ USER_REPORTS : resolves
    MATCHES o|--o{ USER_REPORTS : references
    MESSAGES o|--o{ USER_REPORTS : references

    USERS ||--o{ ACCOUNT_PENALTIES : receives
    USERS ||--o{ ACCOUNT_PENALTIES : imposes
    USER_REPORTS o|--o{ ACCOUNT_PENALTIES : leads_to

    LEGAL_DOCUMENTS ||--o{ USER_CONSENTS : accepted_as
    USERS ||--o{ USER_CONSENTS : accepts

    MONETIZATION_PRODUCTS ||--o{ PRODUCT_PRICES : has_prices
    MONETIZATION_PRODUCTS ||--o{ PRODUCT_ENTITLEMENTS : grants
    ENTITLEMENT_DEFINITIONS ||--o{ PRODUCT_ENTITLEMENTS : defines

    USERS ||--o{ PURCHASE_TRANSACTIONS : purchases
    PRODUCT_PRICES ||--o{ PURCHASE_TRANSACTIONS : purchased_as
    USER_ROLES ||--o{ SUBSCRIPTIONS : subscribes_as
    PRODUCT_PRICES ||--o{ SUBSCRIPTIONS : subscribes_to
    SUBSCRIPTIONS o|--o{ PURCHASE_TRANSACTIONS : generates

    USER_ROLES ||--o{ USER_ENTITLEMENTS : receives
    ENTITLEMENT_DEFINITIONS ||--o{ USER_ENTITLEMENTS : instantiates
    SUBSCRIPTIONS o|--o{ USER_ENTITLEMENTS : sources
    PURCHASE_TRANSACTIONS o|--o{ USER_ENTITLEMENTS : grants
    SUBSCRIPTIONS o|--o{ BILLING_EVENTS : receives_events
    PURCHASE_TRANSACTIONS o|--o{ BILLING_EVENTS : records_events

    PROMOTION_PLACEMENTS ||--o{ PROMOTION_CAMPAIGNS : hosts
    USER_ROLES ||--o{ PROMOTION_CAMPAIGNS : promotes
    PURCHASE_TRANSACTIONS o|--o{ PROMOTION_CAMPAIGNS : funds
    USER_ENTITLEMENTS o|--o{ PROMOTION_CAMPAIGNS : consumes
    PROMOTION_CAMPAIGNS ||--o{ PROMOTION_CAMPAIGN_CITIES : targets
    CITIES ||--o{ PROMOTION_CAMPAIGN_CITIES : selected_for
    PROMOTION_CAMPAIGNS ||--o{ PROMOTION_CAMPAIGN_SERVICES : targets
    SERVICES ||--o{ PROMOTION_CAMPAIGN_SERVICES : selected_for
    PROMOTION_CAMPAIGNS ||--o{ PROMOTION_EVENTS : produces
    USERS o|--o{ PROMOTION_EVENTS : views
    MATCHES o|--o{ PROMOTION_EVENTS : attributes_match
    BOOKINGS o|--o{ PROMOTION_EVENTS : attributes_booking
    USER_ROLES ||--o{ PHOTOGRAPHER_ANALYTICS_DAILY : aggregates

    USER_ROLES ||--o{ FEATURE_USAGE_RECORDS : consumes_features
    ENTITLEMENT_DEFINITIONS o|--o{ FEATURE_USAGE_RECORDS : meters
    USER_ROLES ||--o{ AI_JOBS : requests
    FEATURE_USAGE_RECORDS o|--o{ AI_JOBS : charges

    USER_ROLES ||--o{ PHOTOGRAPHER_CLIENTS : manages
    USERS o|--o{ PHOTOGRAPHER_CLIENTS : links_account
    USER_ROLES ||--o{ SALES_LEADS : owns
    PHOTOGRAPHER_CLIENTS o|--o{ SALES_LEADS : represents
    SERVICES o|--o{ SALES_LEADS : requests
    BOOKINGS o|--o{ SALES_LEADS : converts_to

    USER_ROLES ||--o{ CALENDAR_EVENTS : schedules
    PHOTOGRAPHER_CLIENTS o|--o{ CALENDAR_EVENTS : attends
    BOOKINGS o|--o{ CALENDAR_EVENTS : blocks_time

    USER_ROLES ||--o{ QUOTES : creates
    PHOTOGRAPHER_CLIENTS ||--o{ QUOTES : receives
    BOOKINGS o|--o{ QUOTES : references
    QUOTES ||--|{ QUOTE_ITEMS : contains
    SERVICES o|--o{ QUOTE_ITEMS : prices

    USER_ROLES ||--o{ CONTRACTS : issues
    PHOTOGRAPHER_CLIENTS ||--o{ CONTRACTS : receives
    QUOTES o|--o{ CONTRACTS : becomes
    BOOKINGS o|--o{ CONTRACTS : governs
    CONTRACTS ||--|{ CONTRACT_PARTIES : requires
    USERS o|--o{ CONTRACT_PARTIES : signs_as_user

    USER_ROLES ||--o{ CLIENT_GALLERIES : delivers
    PHOTOGRAPHER_CLIENTS ||--o{ CLIENT_GALLERIES : receives
    BOOKINGS o|--o{ CLIENT_GALLERIES : delivers_for
    CLIENT_GALLERIES ||--o{ GALLERY_ITEMS : contains
    CLIENT_GALLERIES ||--o{ GALLERY_ACCESS_GRANTS : authorizes
    USERS o|--o{ GALLERY_ACCESS_GRANTS : grants_user_access

    USERS {
        uuid id PK
        varchar email UK
        varchar phone UK
        varchar account_status
        varchar identity_verification_status
        boolean email_verified
        boolean phone_verified
        datetime last_login_at
        datetime created_at
        datetime updated_at
        datetime deleted_at
    }

    AUTH_IDENTITIES {
        uuid id PK
        uuid user_id FK
        varchar provider
        varchar provider_identifier
        varchar password_hash
        varchar status
        datetime last_used_at
        datetime created_at
        datetime updated_at
    }

    AUTH_SESSIONS {
        uuid id PK
        uuid user_id FK
        varchar refresh_token_hash
        varchar device_name
        varchar platform
        varchar ip_address
        datetime expires_at
        datetime revoked_at
        datetime created_at
    }

    PASSWORD_RESET_TOKENS {
        uuid id PK
        uuid auth_identity_id FK
        varchar token_hash UK
        datetime expires_at
        datetime used_at
        datetime created_at
    }

    USER_PROFILES {
        uuid user_id PK, FK
        uuid city_id FK
        varchar display_name
        varchar avatar_url
        text bio
        date date_of_birth
        varchar profile_status
        datetime created_at
        datetime updated_at
    }

    ROLES {
        uuid id PK
        varchar code UK
        varchar name
        varchar status
        datetime created_at
        datetime updated_at
    }

    USER_ROLES {
        uuid id PK
        uuid user_id FK
        uuid role_id FK
        varchar status
        boolean is_primary
        datetime selected_at
        datetime created_at
        datetime updated_at
    }

    PHOTOGRAPHER_PROFILES {
        uuid user_role_id PK, FK
        varchar headline
        varchar working_location
        decimal working_radius_km
        int years_experience
        varchar availability_status
        varchar verification_status
        datetime created_at
        datetime updated_at
    }

    USER_SETTINGS {
        uuid user_id PK, FK
        varchar language
        varchar theme
        varchar map_type
        boolean push_enabled
        boolean location_enabled
        boolean profile_discoverable
        int location_visibility_duration_hours
        datetime created_at
        datetime updated_at
    }

    CITIES {
        uuid id PK
        varchar province_code
        varchar name
        decimal center_latitude
        decimal center_longitude
        varchar status
        datetime created_at
        datetime updated_at
    }

    USER_LOCATIONS {
        uuid id PK
        uuid user_id FK
        uuid city_id FK
        decimal latitude
        decimal longitude
        varchar geohash
        decimal accuracy_meters
        boolean is_current
        datetime captured_at
        datetime expires_at
    }

    DISCOVERY_PRESENCE {
        uuid user_role_id PK, FK
        uuid current_location_id FK
        boolean show_on_map
        boolean show_in_swipe
        boolean available_now
        decimal public_radius_meters
        decimal public_latitude
        decimal public_longitude
        decimal location_noise_meters
        datetime visible_until
        datetime last_active_at
        datetime updated_at
    }

    ACTIVITY_FIELDS {
        uuid id PK
        varchar code UK
        varchar name
        text description
        varchar status
        datetime created_at
        datetime updated_at
    }

    ROLE_ACTIVITY_FIELDS {
        uuid role_id PK, FK
        uuid activity_field_id PK, FK
        boolean is_required
        datetime created_at
    }

    USER_ROLE_FIELDS {
        uuid user_role_id PK, FK
        uuid activity_field_id PK, FK
        datetime selected_at
    }

    SERVICES {
        uuid id PK
        uuid activity_field_id FK
        varchar code UK
        varchar name
        text description
        varchar status
        datetime created_at
        datetime updated_at
    }

    USER_ROLE_SERVICES {
        uuid id PK
        uuid user_role_id FK
        uuid service_id FK
        varchar service_mode
        decimal min_price
        decimal max_price
        varchar currency
        varchar price_unit
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    PORTFOLIO_ITEMS {
        uuid id PK
        uuid photographer_role_id FK
        uuid service_id FK
        varchar media_url
        varchar thumbnail_url
        varchar title
        text description
        int sort_order
        varchar status
        datetime created_at
        datetime updated_at
        datetime deleted_at
    }

    DISCOVERY_FILTERS {
        uuid id PK
        uuid owner_user_role_id FK
        uuid target_role_id FK
        uuid city_id FK
        varchar name
        decimal search_latitude
        decimal search_longitude
        decimal radius_km
        decimal min_price
        decimal max_price
        boolean available_only
        boolean verified_only
        boolean is_default
        datetime created_at
        datetime updated_at
    }

    DISCOVERY_FILTER_SERVICES {
        uuid discovery_filter_id PK, FK
        uuid service_id PK, FK
    }

    SWIPES {
        uuid id PK
        uuid actor_user_role_id FK
        uuid target_user_role_id FK
        uuid discovery_filter_id FK
        varchar direction
        varchar source
        datetime created_at
        datetime updated_at
    }

    MATCHES {
        uuid id PK
        uuid user_role_a_id FK
        uuid user_role_b_id FK
        varchar status
        datetime matched_at
        datetime ended_at
        datetime created_at
        datetime updated_at
    }

    CONVERSATIONS {
        uuid id PK
        uuid match_id UK, FK
        varchar status
        datetime last_message_at
        datetime created_at
        datetime updated_at
    }

    CONVERSATION_PARTICIPANTS {
        uuid conversation_id PK, FK
        uuid user_id PK, FK
        boolean is_muted
        datetime joined_at
        datetime last_read_at
        datetime left_at
    }

    MESSAGES {
        uuid id PK
        uuid conversation_id FK
        uuid sender_user_id FK
        uuid reply_to_message_id FK
        varchar message_type
        text content
        varchar status
        datetime created_at
        datetime edited_at
        datetime deleted_at
    }

    MESSAGE_RECEIPTS {
        uuid message_id PK, FK
        uuid user_id PK, FK
        datetime delivered_at
        datetime read_at
    }

    USER_BLOCKS {
        uuid blocker_user_id PK, FK
        uuid blocked_user_id PK, FK
        varchar reason_code
        text note
        datetime created_at
    }

    SHOOT_REQUESTS {
        uuid id PK
        uuid customer_role_id FK
        uuid service_id FK
        uuid city_id FK
        varchar title
        text description
        datetime scheduled_start
        datetime scheduled_end
        decimal budget_min
        decimal budget_max
        varchar currency
        varchar address_text
        decimal latitude
        decimal longitude
        varchar visibility
        varchar status
        datetime created_at
        datetime updated_at
        datetime closed_at
    }

    BOOKINGS {
        uuid id PK
        uuid match_id FK
        uuid shoot_request_id FK
        uuid customer_role_id FK
        uuid photographer_role_id FK
        uuid service_id FK
        datetime scheduled_start
        datetime scheduled_end
        varchar address_text
        decimal latitude
        decimal longitude
        decimal agreed_price
        varchar currency
        varchar status
        text cancellation_reason
        datetime created_at
        datetime updated_at
        datetime completed_at
    }

    BOOKING_STATUS_HISTORY {
        uuid id PK
        uuid booking_id FK
        uuid changed_by_user_id FK
        varchar previous_status
        varchar new_status
        text note
        datetime changed_at
    }

    REVIEWS {
        uuid id PK
        uuid booking_id FK
        uuid reviewer_user_id FK
        uuid reviewee_user_id FK
        int rating
        text comment
        varchar status
        datetime created_at
        datetime updated_at
    }

    NOTIFICATIONS {
        uuid id PK
        uuid recipient_user_id FK
        uuid actor_user_id FK
        uuid match_id FK
        uuid message_id FK
        uuid booking_id FK
        uuid penalty_id FK
        varchar notification_type
        varchar title
        text body
        boolean is_read
        datetime read_at
        datetime created_at
    }

    IDENTITY_VERIFICATIONS {
        uuid id PK
        uuid user_id FK
        varchar provider
        varchar document_type
        varchar provider_reference
        varchar status
        text rejected_reason
        datetime submitted_at
        datetime verified_at
        datetime created_at
        datetime updated_at
    }

    USER_REPORTS {
        uuid id PK
        uuid reporter_user_id FK
        uuid reported_user_id FK
        uuid match_id FK
        uuid message_id FK
        uuid resolved_by_user_id FK
        varchar reason_code
        text description
        varchar status
        datetime created_at
        datetime resolved_at
    }

    ACCOUNT_PENALTIES {
        uuid id PK
        uuid user_id FK
        uuid report_id FK
        uuid imposed_by_user_id FK
        varchar penalty_type
        text reason
        varchar status
        datetime starts_at
        datetime ends_at
        datetime created_at
        datetime revoked_at
    }

    LEGAL_DOCUMENTS {
        uuid id PK
        varchar document_type
        varchar version
        varchar content_url
        varchar status
        datetime effective_at
        datetime created_at
    }

    USER_CONSENTS {
        uuid user_id PK, FK
        uuid legal_document_id PK, FK
        varchar ip_address
        datetime accepted_at
    }

    MONETIZATION_PRODUCTS {
        uuid id PK
        varchar code UK
        varchar product_type
        varchar name
        text description
        varchar status
        datetime created_at
        datetime updated_at
    }

    PRODUCT_PRICES {
        uuid id PK
        uuid product_id FK
        varchar platform
        varchar region_code
        varchar currency
        bigint amount_minor
        varchar billing_period
        varchar store_product_id
        datetime active_from
        datetime active_to
        varchar status
        datetime created_at
        datetime updated_at
    }

    ENTITLEMENT_DEFINITIONS {
        uuid id PK
        varchar code UK
        varchar name
        varchar value_type
        varchar default_value
        varchar status
        datetime created_at
        datetime updated_at
    }

    PRODUCT_ENTITLEMENTS {
        uuid product_id PK, FK
        uuid entitlement_definition_id PK, FK
        varchar entitlement_value
        datetime created_at
    }

    SUBSCRIPTIONS {
        uuid id PK
        uuid photographer_role_id FK
        uuid product_price_id FK
        varchar provider
        varchar external_subscription_id
        varchar status
        boolean auto_renew
        datetime current_period_start
        datetime current_period_end
        datetime cancelled_at
        datetime created_at
        datetime updated_at
    }

    PURCHASE_TRANSACTIONS {
        uuid id PK
        uuid purchaser_user_id FK
        uuid product_price_id FK
        uuid subscription_id FK
        varchar provider
        varchar external_transaction_id
        varchar original_transaction_id
        bigint amount_minor
        varchar currency
        varchar status
        datetime purchased_at
        datetime expires_at
        datetime verified_at
        datetime created_at
    }

    USER_ENTITLEMENTS {
        uuid id PK
        uuid photographer_role_id FK
        uuid entitlement_definition_id FK
        uuid subscription_id FK
        uuid purchase_transaction_id FK
        varchar entitlement_value
        varchar source_type
        datetime starts_at
        datetime ends_at
        datetime revoked_at
        datetime created_at
    }

    BILLING_EVENTS {
        uuid id PK
        uuid subscription_id FK
        uuid purchase_transaction_id FK
        varchar provider
        varchar external_event_id UK
        varchar event_type
        varchar payload_hash
        varchar status
        datetime occurred_at
        datetime received_at
        datetime processed_at
        text processing_error
    }

    PROMOTION_PLACEMENTS {
        uuid id PK
        varchar code UK
        varchar placement_type
        varchar name
        int max_active_slots
        int frequency_cap_per_day
        varchar status
        datetime created_at
        datetime updated_at
    }

    PROMOTION_CAMPAIGNS {
        uuid id PK
        uuid photographer_role_id FK
        uuid placement_id FK
        uuid purchase_transaction_id FK
        uuid entitlement_id FK
        varchar campaign_type
        varchar target_scope
        varchar status
        decimal priority_weight
        datetime starts_at
        datetime ends_at
        datetime paused_at
        datetime created_at
        datetime updated_at
    }

    PROMOTION_CAMPAIGN_CITIES {
        uuid campaign_id PK, FK
        uuid city_id PK, FK
    }

    PROMOTION_CAMPAIGN_SERVICES {
        uuid campaign_id PK, FK
        uuid service_id PK, FK
    }

    PROMOTION_EVENTS {
        uuid id PK
        uuid campaign_id FK
        uuid viewer_user_id FK
        uuid match_id FK
        uuid booking_id FK
        varchar event_type
        varchar traffic_source
        varchar discovery_request_id
        int rank_position
        datetime occurred_at
    }

    PHOTOGRAPHER_ANALYTICS_DAILY {
        uuid photographer_role_id PK, FK
        date metric_date PK
        varchar traffic_source PK
        int impressions
        int profile_views
        int likes_received
        int matches_created
        int bookings_created
        datetime updated_at
    }

    FEATURE_USAGE_RECORDS {
        uuid id PK
        uuid photographer_role_id FK
        uuid entitlement_definition_id FK
        varchar feature_code
        decimal usage_quantity
        varchar usage_unit
        varchar source_type
        uuid source_id
        datetime occurred_at
        datetime created_at
    }

    AI_JOBS {
        uuid id PK
        uuid photographer_role_id FK
        uuid usage_record_id FK
        varchar feature_code
        varchar provider
        varchar model
        varchar prompt_version
        varchar input_reference_type
        uuid input_reference_id
        varchar output_reference
        varchar status
        int input_tokens
        int output_tokens
        varchar error_code
        datetime created_at
        datetime started_at
        datetime completed_at
    }

    PHOTOGRAPHER_CLIENTS {
        uuid id PK
        uuid photographer_role_id FK
        uuid linked_user_id FK
        varchar display_name
        varchar email_ciphertext
        varchar phone_ciphertext
        text notes_ciphertext
        varchar status
        datetime created_at
        datetime updated_at
        datetime deleted_at
    }

    SALES_LEADS {
        uuid id PK
        uuid photographer_role_id FK
        uuid client_id FK
        uuid service_id FK
        uuid converted_booking_id FK
        varchar source
        varchar title
        varchar status
        bigint estimated_value_minor
        varchar currency
        datetime next_follow_up_at
        datetime created_at
        datetime updated_at
    }

    CALENDAR_EVENTS {
        uuid id PK
        uuid photographer_role_id FK
        uuid client_id FK
        uuid booking_id FK
        varchar event_type
        varchar title
        datetime starts_at
        datetime ends_at
        varchar timezone
        varchar status
        text notes
        datetime created_at
        datetime updated_at
    }

    QUOTES {
        uuid id PK
        uuid photographer_role_id FK
        uuid client_id FK
        uuid booking_id FK
        varchar quote_number
        int version
        varchar currency
        bigint subtotal_minor
        bigint discount_minor
        bigint tax_minor
        bigint total_minor
        varchar status
        datetime valid_until
        datetime accepted_at
        datetime created_at
        datetime updated_at
    }

    QUOTE_ITEMS {
        uuid id PK
        uuid quote_id FK
        uuid service_id FK
        varchar description
        decimal quantity
        bigint unit_price_minor
        bigint amount_minor
        int sort_order
        datetime created_at
    }

    CONTRACTS {
        uuid id PK
        uuid photographer_role_id FK
        uuid client_id FK
        uuid quote_id FK
        uuid booking_id FK
        varchar contract_number
        int version
        varchar title
        varchar content_url
        varchar content_hash
        varchar status
        datetime sent_at
        datetime signed_at
        datetime voided_at
        datetime created_at
        datetime updated_at
    }

    CONTRACT_PARTIES {
        uuid id PK
        uuid contract_id FK
        uuid user_id FK
        varchar party_role
        varchar display_name
        varchar email
        varchar status
        varchar signature_provider_reference
        datetime signed_at
        datetime created_at
    }

    CLIENT_GALLERIES {
        uuid id PK
        uuid photographer_role_id FK
        uuid client_id FK
        uuid booking_id FK
        varchar name
        varchar slug UK
        varchar status
        datetime published_at
        datetime expires_at
        datetime created_at
        datetime updated_at
    }

    GALLERY_ITEMS {
        uuid id PK
        uuid gallery_id FK
        varchar storage_key
        varchar media_type
        varchar title
        int sort_order
        boolean downloadable
        text metadata_json
        datetime created_at
        datetime deleted_at
    }

    GALLERY_ACCESS_GRANTS {
        uuid id PK
        uuid gallery_id FK
        uuid user_id FK
        varchar email_hash
        varchar access_token_hash
        varchar permission
        datetime expires_at
        datetime revoked_at
        datetime created_at
    }
```

# Giải thích core entities kế thừa

## Tài khoản và phân quyền

| Entity                  | Vai trò                                                                 |
| ----------------------- | ----------------------------------------------------------------------- |
| `USERS`                 | Thông tin tài khoản cốt lõi, trạng thái hoạt động và trạng thái xác thực danh tính. |
| `AUTH_IDENTITIES`       | Các phương thức đăng nhập như email/password, Google, Apple hoặc provider khác. |
| `AUTH_SESSIONS`         | Quản lý phiên đăng nhập, thiết bị và việc đăng xuất hoặc thu hồi phiên. |
| `PASSWORD_RESET_TOKENS` | Token sử dụng cho luồng quên mật khẩu.                                  |
| `USER_PROFILES`         | Thông tin cá nhân dùng chung như tên, avatar, thành phố và giới thiệu.  |
| `ROLES`                 | Danh mục vai trò như `CUSTOMER`, `PHOTOGRAPHER`, `ADMIN`.               |
| `USER_ROLES`            | Bảng trung gian Many-to-Many giữa tài khoản và vai trò.                 |
| `PHOTOGRAPHER_PROFILES` | Các thông tin chỉ dành cho vai trò thợ ảnh.                             |
| `USER_SETTINGS`         | Ngôn ngữ, giao diện, loại bản đồ và các thiết lập riêng tư.             |

Không tạo bảng `CUSTOMER_PROFILES` riêng vì các thông tin hiện có của người dùng thường đã nằm trong `USER_PROFILES`. Điều này tránh lặp avatar, tên, thành phố và phần giới thiệu.

`USERS.identity_verification_status` là trạng thái xác thực hiện tại của tài khoản. `IDENTITY_VERIFICATIONS` lưu lịch sử từng lần gửi xác thực, provider bên thứ ba và kết quả xử lý.

## Lĩnh vực, dịch vụ và portfolio

| Entity                 | Vai trò                                                                  |
| ---------------------- | ------------------------------------------------------------------------ |
| `ACTIVITY_FIELDS`      | Lĩnh vực hoạt động cấp cao.                                              |
| `ROLE_ACTIVITY_FIELDS` | Quy định mỗi vai trò được phép chọn những lĩnh vực nào.                  |
| `USER_ROLE_FIELDS`     | Những lĩnh vực mà một người đã chọn cho một vai trò cụ thể.              |
| `SERVICES`             | Dịch vụ thuộc một lĩnh vực, ví dụ chân dung, cưới, sự kiện hoặc du lịch. |
| `USER_ROLE_SERVICES`   | Dịch vụ mà vai trò đang cung cấp hoặc đang có nhu cầu.                   |
| `PORTFOLIO_ITEMS`      | Ảnh portfolio của thợ ảnh, có thể gắn với một dịch vụ cụ thể.            |

`USER_ROLE_SERVICES.service_mode` có thể nhận:

* `OFFERED`: dịch vụ do thợ ảnh cung cấp.
* `WANTED`: dịch vụ mà người dùng đang quan tâm.

Cách này giúp cùng một cấu trúc dữ liệu hỗ trợ cả hai phía của marketplace.

## Vị trí và tìm kiếm quanh đây

| Entity                      | Vai trò                                                                     |
| --------------------------- | --------------------------------------------------------------------------- |
| `CITIES`                    | Danh mục tỉnh hoặc thành phố.                                               |
| `USER_LOCATIONS`            | Lưu vị trí GPS hiện tại và lịch sử vị trí nếu cần.                          |
| `DISCOVERY_PRESENCE`        | Xác định một vai trò có xuất hiện trên bản đồ hoặc màn hình quẹt hay không. |
| `DISCOVERY_FILTERS`         | Lưu tiêu chí tìm kiếm như bán kính, giá, thành phố và trạng thái sẵn sàng.  |
| `DISCOVERY_FILTER_SERVICES` | Quan hệ Many-to-Many giữa bộ lọc và dịch vụ.                                |

`USER_PROFILES.city_id` là thành phố cư trú hoặc thành phố khai báo.
`USER_LOCATIONS` là vị trí GPS hiện tại. Hai dữ liệu này có ý nghĩa khác nhau nên không bị xem là trùng lặp.

Vị trí trả về cho người khác không dùng trực tiếp tọa độ GPS thật. `DISCOVERY_PRESENCE.public_latitude` và `DISCOVERY_PRESENCE.public_longitude` là tọa độ công khai đã được làm lệch chủ động trong phạm vi cấu hình. `public_radius_meters` mô tả bán kính công khai hiển thị, còn `location_noise_meters` mô tả mức sai lệch đã áp dụng.

`DISCOVERY_PRESENCE.visible_until` xác định thời điểm hết hiển thị. Giá trị mặc định được tính từ `USER_SETTINGS.location_visibility_duration_hours`, nhưng người dùng có thể chọn một khoảng thời gian hiển thị ngắn hơn cho từng lần bật hiển thị.

Trong triển khai thật, nên dùng PostgreSQL cùng PostGIS với kiểu `geography(Point, 4326)` thay vì chỉ sử dụng hai cột latitude và longitude.

## Quẹt và match

| Entity    | Vai trò                                           |
| --------- | ------------------------------------------------- |
| `SWIPES`  | Lưu hành động bỏ qua, quan tâm, chấp nhận hoặc từ chối. |
| `MATCHES` | Được tạo khi người dùng quan tâm và thợ ảnh chấp nhận.  |

`SWIPES` liên kết tới `USER_ROLES` thay vì trực tiếp tới `USERS`. Vì một tài khoản có thể vừa là người dùng, vừa là thợ ảnh và hành vi quẹt phụ thuộc vào chế độ đang sử dụng.

Luồng match đã chốt:

```text
Customer quan tâm
→ Photographer chấp nhận
→ MATCH
```

Trong MVP, `Customer role RIGHT Photographer role` chỉ tạo tín hiệu quan tâm, chưa mở chat. Khi thợ ảnh bấm chấp nhận, hệ thống tạo `MATCH` và mở `CONVERSATION`. Nếu thợ ảnh từ chối, hệ thống ghi nhận hành động từ chối để không tiếp tục hiển thị cùng một tín hiệu quan tâm.

## Chat và chặn người dùng

| Entity                      | Vai trò                                                                |
| --------------------------- | ---------------------------------------------------------------------- |
| `CONVERSATIONS`             | Cuộc trò chuyện được tạo từ một match.                                 |
| `CONVERSATION_PARTICIPANTS` | Thành viên trong cuộc trò chuyện. Với MVP phải có đúng hai thành viên. |
| `MESSAGES`                  | Nội dung tin nhắn văn bản.                                             |
| `MESSAGE_RECEIPTS`          | Thời điểm tin nhắn được giao và được đọc.                              |
| `USER_BLOCKS`               | Danh sách chặn giữa hai tài khoản.                                     |

Một `MATCH` chỉ có tối đa một `CONVERSATION`. Vì vậy `CONVERSATIONS.match_id` cần unique.

Trước khi gửi tin nhắn, backend cần kiểm tra:

1. Hai người có match còn hiệu lực.
2. Người gửi là thành viên cuộc trò chuyện.
3. Không bên nào đã chặn bên còn lại.
4. Tài khoản không đang bị khóa hoặc đình chỉ.

## Yêu cầu chụp ảnh, đặt lịch và đánh giá

| Entity                   | Vai trò                                  |
| ------------------------ | ---------------------------------------- |
| `SHOOT_REQUESTS`         | Nhu cầu chụp ảnh do khách hàng đăng lên. |
| `BOOKINGS`               | Lịch chụp đã được hai bên thống nhất.    |
| `BOOKING_STATUS_HISTORY` | Lịch sử thay đổi trạng thái booking.     |
| `REVIEWS`                | Đánh giá sau khi booking hoàn thành.     |

`BOOKINGS` có thể được tạo:

* Từ một `MATCH`.
* Từ một `SHOOT_REQUEST`.
* Từ cả hai nếu người dùng đăng yêu cầu rồi match với thợ ảnh.

Chỉ khách hàng được đánh giá thợ ảnh sau khi booking hoàn thành. Không cho phép đánh giá ẩn danh và không cần admin duyệt review trước khi hiển thị. Database nên có unique constraint để mỗi booking chỉ có một review:

```text
UNIQUE (booking_id)
```

## Thông báo, xác thực và xử phạt

| Entity                   | Vai trò                                                           |
| ------------------------ | ----------------------------------------------------------------- |
| `NOTIFICATIONS`          | Thông báo match, tin nhắn, booking hoặc khóa tài khoản.           |
| `IDENTITY_VERIFICATIONS` | Lịch sử gửi và xác minh danh tính tự động qua dịch vụ bên thứ ba. |
| `USER_REPORTS`           | Báo cáo tài khoản hoặc tin nhắn vi phạm.                          |
| `ACCOUNT_PENALTIES`      | Cảnh cáo, khóa tạm thời hoặc khóa vĩnh viễn.                      |
| `LEGAL_DOCUMENTS`        | Các phiên bản điều khoản sử dụng và chính sách bảo mật.           |
| `USER_CONSENTS`          | Ghi nhận người dùng đã đồng ý với phiên bản điều khoản nào.       |

Màn hình phạt hoặc khóa tài khoản được xác định từ:

```text
USERS.account_status
+
ACCOUNT_PENALTIES đang có hiệu lực
```

# Các ràng buộc cần bổ sung trong database

Ngoài PK và FK, nên có các unique constraint sau:

```text
USER_ROLES:
UNIQUE (user_id, role_id)

USER_ROLE_SERVICES:
UNIQUE (user_role_id, service_id, service_mode)

SWIPES:
UNIQUE (actor_user_role_id, target_user_role_id)

MATCHES:
UNIQUE (canonical_user_role_a_id, canonical_user_role_b_id)

CONVERSATION_PARTICIPANTS:
UNIQUE (conversation_id, user_id)

MESSAGE_RECEIPTS:
UNIQUE (message_id, user_id)

USER_BLOCKS:
UNIQUE (blocker_user_id, blocked_user_id)

REVIEWS:
UNIQUE (booking_id)

USER_CONSENTS:
UNIQUE (user_id, legal_document_id)
```

Với `MATCHES`, hai ID cần được chuẩn hóa trước khi lưu:

```text
user_role_a_id = ID nhỏ hơn
user_role_b_id = ID lớn hơn
```

Cách này ngăn hai bản ghi trùng:

```text
A — B
B — A
```

Các ràng buộc nghiệp vụ khác:

* Một người không được swipe, block hoặc report chính mình.
* Chỉ một `USER_LOCATION` được có `is_current = true` cho mỗi user.
* Chỉ một bộ lọc mặc định cho mỗi vai trò và loại đối tượng tìm kiếm.
* Chỉ tài khoản có role `PHOTOGRAPHER` mới được tạo `PHOTOGRAPHER_PROFILES` và `PORTFOLIO_ITEMS`.
* Chỉ booking `COMPLETED` mới được đánh giá.
* Chỉ khách hàng của booking được đánh giá thợ ảnh của booking đó.
* Review không được ẩn danh và không cần trạng thái chờ admin duyệt.
* Rating phải nằm trong khoảng 1–5.
* `min_price` không được lớn hơn `max_price`.
* `scheduled_end` phải sau `scheduled_start`.
* Vị trí hiển thị công khai phải được làm lệch chủ động, không trả về GPS chính xác.
* Vị trí hiển thị tự động hết hạn theo `visible_until`; thời lượng mặc định do người dùng cấu hình.

# Giả định thiết kế

1. Một tài khoản có thể đồng thời là người dùng thường và thợ ảnh.
2. Người dùng chuyển đổi giữa các vai trò trong ứng dụng.
3. Trang cá nhân cơ bản dùng chung giữa các vai trò.
4. Portfolio chỉ thuộc vai trò thợ ảnh.
5. Chỉ người bật `show_on_map` hoặc `show_in_swipe` mới xuất hiện trong phần khám phá.
6. Thợ ảnh nhìn thấy toàn bộ người dùng đã bật hiển thị phù hợp với ngữ cảnh bản đồ hoặc quẹt.
7. Chat chỉ được mở sau khi có match.
8. Một cặp match có một conversation nhưng có thể phát sinh nhiều booking.
9. Booking và review chưa bắt buộc phải xuất hiện trong bản phát hành đầu tiên nhưng schema đã chuẩn bị sẵn.
10. VNeID không nằm trong MVP. Nếu cần sau này, VNeID chỉ là một provider trong nhóm identity verification.
11. Tất cả tài khoản cần xác thực danh tính, thực hiện tự động thông qua dịch vụ bên thứ ba.
12. Điều khoản sử dụng và chính sách bảo mật có phiên bản để có thể yêu cầu người dùng chấp nhận lại khi nội dung thay đổi.

# Các quyết định đã chốt

## Cơ chế match

Người dùng quẹt phải để thể hiện quan tâm. Thợ ảnh bấm chấp nhận để tạo match. Chat chỉ được mở sau khi match được tạo.

## Thợ ảnh nhìn thấy gì?

Thợ ảnh nhìn thấy toàn bộ người dùng đã bật hiển thị. Điều kiện hiển thị phụ thuộc vào `DISCOVERY_PRESENCE.show_on_map`, `DISCOVERY_PRESENCE.show_in_swipe` và thời hạn `visible_until`.

## Quyền riêng tư vị trí

* Hiển thị vị trí có sai lệch chủ động, không trả về GPS chính xác.
* Vị trí tự động hết hạn sau số giờ do người dùng cấu hình trong `USER_SETTINGS.location_visibility_duration_hours`.
* Người dùng được phép bật hiển thị trong một khoảng thời gian nhất định; giá trị cụ thể được lưu bằng `DISCOVERY_PRESENCE.visible_until`.

## Booking

Booking có các trạng thái:

```text
DRAFT
PENDING
ACCEPTED
REJECTED
CANCELLED
IN_PROGRESS
COMPLETED
DISPUTED
```

## Đánh giá

* Chỉ khách hàng đánh giá thợ ảnh.
* Không cho phép đánh giá ẩn danh.
* Không cần admin duyệt review.

## Xác thực danh tính

* VNeID không nằm trong MVP.
* Tất cả tài khoản cần xác thực.
* Xác thực tự động thông qua dịch vụ của bên thứ ba.

# Những màn hình không cần bảng riêng

Các màn hình sau chủ yếu là giao diện hoặc dữ liệu tĩnh nên không cần entity độc lập:

* Splash.
* Đăng nhập.
* Đăng ký.
* Quên mật khẩu.
* About application.
* Chọn loại bản đồ.
* Giao diện sáng hoặc tối.
* Giới thiệu bạn bè nếu chỉ sử dụng chức năng chia sẻ link.

Nếu cần theo dõi referral và thưởng giới thiệu, nên bổ sung bảng `REFERRALS`.

# Phạm vi mở rộng `1.1.0`

| Lớp dữ liệu | Số bảng | Trạng thái |
| :---- | :---- | :---- |
| Core marketplace kế thừa `1.0.0` | 37 | Tiếp tục sử dụng |
| Monetization, promotion, analytics và AI | 16 | Phạm vi triển khai `1.1.0` |
| SaaS foundation | 10 | Thiết kế mở rộng, chưa bắt buộc migrate/release |
| Tổng logical model | 63 | Core + active extension + future foundation |

Không thêm booking payment, commission, refund hoặc payout vào `1.1.0`. Doanh thu của phiên bản này chỉ đến từ digital products dành cho photographer.

# Monetization catalog và billing

| Entity | Vai trò |
| :---- | :---- |
| `MONETIZATION_PRODUCTS` | Định nghĩa product logic như Pro, Boost, Featured, AI credit hoặc SaaS add-on. |
| `PRODUCT_PRICES` | Version giá theo platform, region, currency và billing period. |
| `ENTITLEMENT_DEFINITIONS` | Danh mục quyền typed-value, ví dụ boolean, integer, decimal hoặc string. |
| `PRODUCT_ENTITLEMENTS` | Map product sang tập quyền và giá trị được cấp. |
| `SUBSCRIPTIONS` | Vòng đời thuê bao Pro của photographer role. |
| `PURCHASE_TRANSACTIONS` | Store transaction đã được backend xác minh. |
| `USER_ENTITLEMENTS` | Quyền thực tế của photographer tại một thời điểm. |
| `BILLING_EVENTS` | Inbox nhận Apple/Google server notification, chống xử lý trùng. |

## Tách product, price và entitlement

Không lưu toàn bộ logic Pro trong một cột `plan` hoặc `is_pro`.

```text
MONETIZATION_PRODUCTS
→ PRODUCT_PRICES
→ PURCHASE_TRANSACTIONS/SUBSCRIPTIONS
→ USER_ENTITLEMENTS
```

Cách tách này cho phép:

- Một product có giá khác nhau trên iOS, Android, region và currency.
- Đổi giá bằng price version mới mà không sửa lịch sử purchase.
- Tạo nhiều plan/add-on mà không thay schema.
- Thêm/bớt quyền lợi bằng mapping product-entitlement.
- Dùng cùng entitlement service cho portfolio, analytics, AI và SaaS sau này.

## Pro và Verified

- `PRO_BADGE` được suy ra từ `USER_ENTITLEMENTS` còn hiệu lực.
- `VERIFIED` tiếp tục được suy ra từ `PHOTOGRAPHER_PROFILES.verification_status` và lịch sử `IDENTITY_VERIFICATIONS`.
- Không tạo `is_pro` hoặc `is_verified_paid` trong `USERS`.
- Purchase không được tạo/cập nhật identity verification.

## Giá trị entitlement

`ENTITLEMENT_DEFINITIONS.value_type` xác định cách parse `entitlement_value`:

| value_type | Ví dụ |
| :---- | :---- |
| `BOOLEAN` | `ADVANCED_FILTERS=true` |
| `INTEGER` | `AI_MONTHLY_CREDITS=50` |
| `DECIMAL` | `PRO_RANKING_WEIGHT=0.15` |
| `STRING` | `PORTFOLIO_LIMIT=UNLIMITED_FAIR_USE` |

Backend phải validate value theo `value_type` trước khi publish product configuration.

## Subscription lifecycle

```text
PENDING
→ TRIALING hoặc ACTIVE
→ GRACE_PERIOD/PAST_DUE
→ ACTIVE hoặc EXPIRED

ACTIVE → CANCELLED → EXPIRED
ACTIVE/TRIALING/GRACE_PERIOD → REVOKED
```

- `CANCELLED` có nghĩa đã tắt auto-renew nhưng có thể còn quyền tới `current_period_end`.
- `EXPIRED` mới làm entitlement hết hiệu lực nếu không có source khác.
- `REVOKED` dùng khi store hoàn tiền/thu hồi hoặc phát hiện fraud.
- Một photographer có thể có nhiều entitlement source; effective entitlement được resolve theo rule ưu tiên và thời hạn.

## Purchase và billing event

- Unique `(provider, external_transaction_id)` trên `PURCHASE_TRANSACTIONS`.
- Unique `(provider, external_event_id)` trên `BILLING_EVENTS`.
- `payload_hash` dùng để phát hiện payload thay đổi bất thường; không thay thế signature verification.
- Persist `BILLING_EVENTS` trước khi acknowledge provider.
- Event out-of-order phải được xử lý dựa trên `occurred_at` và trạng thái store hiện tại.
- Không lưu receipt/token/secret ở log hoặc cột không mã hóa.

# Promotion engine

| Entity | Vai trò |
| :---- | :---- |
| `PROMOTION_PLACEMENTS` | Danh mục vị trí quảng cáo và capacity/frequency cap. |
| `PROMOTION_CAMPAIGNS` | Một campaign Boost hoặc Featured của photographer. |
| `PROMOTION_CAMPAIGN_CITIES` | Target một hoặc nhiều thành phố. |
| `PROMOTION_CAMPAIGN_SERVICES` | Target một hoặc nhiều dịch vụ/category. |
| `PROMOTION_EVENTS` | Raw attribution event của campaign. |
| `PHOTOGRAPHER_ANALYTICS_DAILY` | Aggregate hàng ngày cho dashboard photographer. |

## Dùng chung campaign cho Boost và Featured

Không tạo hai hệ thống ranking độc lập.

```text
BOOST
→ tăng trọng số có thời hạn trong placement cho phép

FEATURED
→ chiếm inventory quảng cáo cụ thể, có sponsored label
```

`PROMOTION_CAMPAIGNS.campaign_type` phân biệt hai loại. `placement_id`, target city/service và khoảng thời gian quyết định campaign được tham gia ở đâu.

## Placement

Các `PROMOTION_PLACEMENTS.code` gợi ý:

- `DISCOVERY_BOOST`
- `NEARBY_BOOST`
- `SEARCH_FEATURED`
- `REGION_TOP`
- `CATEGORY_TOP`

`max_active_slots` giới hạn số Featured campaign đồng thời. `frequency_cap_per_day` giới hạn số lần cùng campaign được hiển thị cho một viewer trong một ngày.

## Target rules

| target_scope | Quan hệ bắt buộc |
| :---- | :---- |
| `GLOBAL` | Không có city/service target |
| `REGION` | Có ít nhất một `PROMOTION_CAMPAIGN_CITIES` |
| `CATEGORY` | Có ít nhất một `PROMOTION_CAMPAIGN_SERVICES` |
| `REGION_AND_CATEGORY` | Có cả city và service target |

Không lưu danh sách `city_ids` hoặc `service_ids` trong JSON vì cần index, constraint và join hiệu quả khi discovery.

## Campaign state machine

```text
DRAFT → SCHEDULED → ACTIVE → COMPLETED
                  ↘ PAUSED ↗

DRAFT/SCHEDULED → CANCELLED
SCHEDULED       → REJECTED
ACTIVE          → CANCELLED
```

- Chỉ campaign có purchase/entitlement hợp lệ mới vào `SCHEDULED`.
- Scheduler chuyển `SCHEDULED` sang `ACTIVE` và `ACTIVE` sang `COMPLETED`.
- Campaign bị penalty hoặc profile hidden phải chuyển `PAUSED`.
- `priority_weight` bị chặn bởi upper bound theo placement; admin không được nhập giá trị tùy ý vượt ngưỡng.

## Promotion attribution

`PROMOTION_EVENTS` hỗ trợ:

```text
IMPRESSION → CLICK/PROFILE_VIEW → LIKE → MATCH → BOOKING_CREATED
```

- `traffic_source` phân biệt `ORGANIC`, `PRO_SIGNAL`, `BOOST`, `FEATURED`.
- `match_id` và `booking_id` chỉ có ở conversion event tương ứng.
- `discovery_request_id` liên kết các event trong cùng một lần xếp hạng mà không lưu raw filter nhạy cảm.
- Raw events nên partition theo `occurred_at` và có retention policy.
- Dashboard đọc từ `PHOTOGRAPHER_ANALYTICS_DAILY`, không aggregate raw event theo request.

# Feature usage và AI

| Entity | Vai trò |
| :---- | :---- |
| `FEATURE_USAGE_RECORDS` | Usage ledger dùng chung cho AI, quota discovery, storage hoặc SaaS feature. |
| `AI_JOBS` | Công việc AI bất đồng bộ, model/provider, token và kết quả. |

## Usage ledger

- Mỗi usage record là append-only trong luồng thông thường.
- Quota được tính theo entitlement window, không reset bằng local device time.
- Sai lệch quota được sửa bằng adjustment record, không sửa lịch sử usage.
- `source_type/source_id` trace usage về AI job, upload hoặc action tương ứng.

## AI job logic

```text
Validate entitlement/quota
→ Create FEATURE_USAGE_RECORDS reservation
→ Queue AI_JOBS
→ Process provider request
→ Persist output reference/usage
→ Complete hoặc release/adjust reservation khi failed
```

- Không lưu prompt chứa dữ liệu nhạy cảm nếu không cần thiết.
- `prompt_version` giúp tái hiện hành vi và A/B test an toàn.
- `input_reference_type/input_reference_id` liên kết tới profile/portfolio/analytics mà không tạo cột FK cho từng feature.
- Polymorphic reference chỉ dùng trong AI integration boundary; service phải kiểm tra ownership.
- AI output không tự publish hoặc gửi cho customer.

# SaaS foundation

Các entity dưới đây thuộc logical future model. Chỉ tạo migration khi module tương ứng được phê duyệt.

## CRM và lead

| Entity | Vai trò |
| :---- | :---- |
| `PHOTOGRAPHER_CLIENTS` | Contact riêng trong workspace của photographer, có thể link tới Photomatch user. |
| `SALES_LEADS` | Cơ hội công việc theo pipeline, source, service và estimated value. |

- Một customer ngoài Photomatch vẫn có thể là client, do đó `linked_user_id` optional.
- Contact PII phải mã hóa; tìm kiếm email/phone có thể dùng blind index riêng khi triển khai.
- Một user có thể là client của nhiều photographer, nhưng mỗi photographer chỉ thấy workspace của mình.
- `converted_booking_id` liên kết lead đã thắng với booking mà không ép mọi booking phải bắt đầu từ CRM.

## Calendar

| Entity | Vai trò |
| :---- | :---- |
| `CALENDAR_EVENTS` | Lịch chụp, lịch bận, follow-up hoặc reminder của photographer. |

- Booking có thể tạo calendar event nhưng event cũng có thể tồn tại độc lập.
- `timezone` bắt buộc để tránh sai lịch khi photographer di chuyển.
- Cần constraint `ends_at > starts_at`.
- Đồng bộ Google/Apple Calendar sau này cần thêm integration account và external event mapping, không nhét provider fields vào core table.

## Quote

| Entity | Vai trò |
| :---- | :---- |
| `QUOTES` | Header báo giá có version, tổng tiền, trạng thái và hạn hiệu lực. |
| `QUOTE_ITEMS` | Các dịch vụ/hạng mục trong báo giá. |

- Monetary value dùng integer minor unit để tránh floating-point error.
- `quote_number` unique trong phạm vi photographer, không cần unique toàn hệ thống.
- Khi gửi quote, snapshot item/price phải được giữ ổn định; sửa nội dung tạo version mới.
- `total_minor` phải bằng subtotal - discount + tax theo policy được cấu hình.

## Contract

| Entity | Vai trò |
| :---- | :---- |
| `CONTRACTS` | Snapshot hợp đồng, content hash, version và trạng thái. |
| `CONTRACT_PARTIES` | Các bên cần ký và trạng thái chữ ký. |

- Contract có thể sinh từ quote hoặc booking nhưng cả hai FK đều optional.
- `content_hash` phát hiện tài liệu bị thay đổi sau khi gửi/ký.
- Không ghi đè file hợp đồng đã gửi; thay đổi tạo version mới.
- Giá trị pháp lý của chữ ký phải được review theo thị trường và signature provider.

## Client gallery

| Entity | Vai trò |
| :---- | :---- |
| `CLIENT_GALLERIES` | Gallery riêng tư giao ảnh theo client/booking. |
| `GALLERY_ITEMS` | Media delivery item, tách khỏi public portfolio. |
| `GALLERY_ACCESS_GRANTS` | Quyền xem/download theo user, email hoặc access token. |

- `GALLERY_ITEMS` không dùng chung với `PORTFOLIO_ITEMS`: gallery là private delivery, portfolio là public marketing.
- Database lưu `storage_key`, không lưu public URL vĩnh viễn.
- API tạo signed URL ngắn hạn sau khi kiểm tra grant.
- `email_hash` hỗ trợ cấp quyền cho client chưa có tài khoản mà không lưu email plaintext trong grant.
- Xóa gallery cần retention/grace period; không hard-delete media ngay khi user bấm xóa.

# Constraints và indexes `1.1.0`

## Unique constraints

```text
MONETIZATION_PRODUCTS(code)
PRODUCT_PRICES(platform, store_product_id)
PURCHASE_TRANSACTIONS(provider, external_transaction_id)
SUBSCRIPTIONS(provider, external_subscription_id)
BILLING_EVENTS(provider, external_event_id)
PRODUCT_ENTITLEMENTS(product_id, entitlement_definition_id)
PROMOTION_CAMPAIGN_CITIES(campaign_id, city_id)
PROMOTION_CAMPAIGN_SERVICES(campaign_id, service_id)
PHOTOGRAPHER_ANALYTICS_DAILY(photographer_role_id, metric_date, traffic_source)
```

## Check constraints

- `PRODUCT_PRICES.amount_minor >= 0`.
- `PRODUCT_PRICES.active_to > active_from` khi có `active_to`.
- `SUBSCRIPTIONS.current_period_end > current_period_start`.
- `USER_ENTITLEMENTS.ends_at > starts_at` khi có `ends_at`.
- `PROMOTION_CAMPAIGNS.ends_at > starts_at`.
- `PROMOTION_CAMPAIGNS.priority_weight >= 0` và không vượt placement upper bound trong service rule.
- `PROMOTION_EVENTS.rank_position > 0` khi event là impression.
- Các analytics counters không âm.
- `CALENDAR_EVENTS.ends_at > starts_at`.
- Monetary minor-unit fields trong quote không âm, trừ adjustment được định nghĩa rõ sau này.

## Indexes

- `USER_ENTITLEMENTS(photographer_role_id, starts_at, ends_at, revoked_at)` cho entitlement check.
- `SUBSCRIPTIONS(photographer_role_id, status, current_period_end)`.
- `PROMOTION_CAMPAIGNS(placement_id, status, starts_at, ends_at)` cho ranking query.
- `PROMOTION_CAMPAIGN_CITIES(city_id, campaign_id)`.
- `PROMOTION_CAMPAIGN_SERVICES(service_id, campaign_id)`.
- `PROMOTION_EVENTS(campaign_id, occurred_at)` và partition theo thời gian khi cần.
- `PHOTOGRAPHER_ANALYTICS_DAILY(photographer_role_id, metric_date)`.
- `FEATURE_USAGE_RECORDS(photographer_role_id, feature_code, occurred_at)`.
- SaaS tables luôn index `photographer_role_id` trước để bảo vệ tenant query pattern.

# Quy tắc triển khai theo release

## Bắt buộc trong `1.1.0`

- `MONETIZATION_PRODUCTS`
- `PRODUCT_PRICES`
- `ENTITLEMENT_DEFINITIONS`
- `PRODUCT_ENTITLEMENTS`
- `SUBSCRIPTIONS`
- `PURCHASE_TRANSACTIONS`
- `USER_ENTITLEMENTS`
- `BILLING_EVENTS`
- `PROMOTION_PLACEMENTS`
- `PROMOTION_CAMPAIGNS`
- `PROMOTION_CAMPAIGN_CITIES`
- `PROMOTION_CAMPAIGN_SERVICES`
- `PROMOTION_EVENTS`
- `PHOTOGRAPHER_ANALYTICS_DAILY`
- `FEATURE_USAGE_RECORDS`
- `AI_JOBS`

## Chỉ triển khai khi SaaS module được approve

- `PHOTOGRAPHER_CLIENTS`
- `SALES_LEADS`
- `CALENDAR_EVENTS`
- `QUOTES`
- `QUOTE_ITEMS`
- `CONTRACTS`
- `CONTRACT_PARTIES`
- `CLIENT_GALLERIES`
- `GALLERY_ITEMS`
- `GALLERY_ACCESS_GRANTS`

Không tạo bảng trống chỉ để “dành chỗ”. Logical ERD giữ contract tương lai; migration vật lý đi theo release thực tế để tránh schema chết và chi phí bảo trì không cần thiết.

# Quyết định kiến trúc `1.1.0`

1. Pro là entitlement, không phải cột boolean trên user.
2. Verified độc lập hoàn toàn với purchase/subscription.
3. Product và price được version hóa riêng.
4. Boost và Featured dùng chung promotion campaign engine.
5. Target city/service được chuẩn hóa bằng relation tables.
6. Raw promotion events tách khỏi daily analytics aggregate.
7. Feature usage là ledger dùng chung cho AI/quota/future SaaS.
8. Public portfolio và private client gallery là hai domain khác nhau.
9. SaaS data có tenant key `photographer_role_id` để phân quyền theo workspace.
10. Booking payment/commission không thuộc phạm vi `1.1.0` và không xuất hiện trong ERD extension này.
