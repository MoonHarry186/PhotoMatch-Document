# Photomatch 1.1.0 Monetization Plan

# DOCUMENT CONTROL

| Thuộc tính | Giá trị |
| :---- | :---- |
| Product document version | `1.1.0` |
| Release scope | Post-MVP, phiên bản đầu tiên sau MVP |
| Loại thay đổi | Minor version, tương thích với baseline `1.0.0` |
| Change request | `CR-001 - Photographer Monetization Platform` |
| Baseline tham chiếu | [Photomatch MVP Plan 1.0.0](../../1.0.0-mvp/plan/photomatch-mvp-plan.md) |
| ERD phiên bản này | [Photomatch ERD 1.1.0](../system-designs/erd.md) |
| API contract version | `/api/v1` |
| Trạng thái | Draft |
| Ngày cập nhật | `2026-07-18` |

Phiên bản `1.1.0` mở rộng Photomatch từ marketplace miễn phí thành nền tảng freemium dành cho photographer. Luồng cốt lõi từ MVP không đổi:

```text
Tìm người phù hợp → Match → Chat → Booking
```

Nguồn thu của `1.1.0` đến từ tính năng số dành cho photographer, không thu commission trên booking và chưa xử lý tiền dịch vụ chụp ảnh trong app.

# PRODUCT DECISION

## Thứ tự ưu tiên

| Ưu tiên | Sản phẩm | Mô hình | Vai trò trong doanh thu | Phạm vi |
| :---- | :---- | :---- | :---- | :---- |
| 1 ⭐⭐⭐⭐⭐ | PhotoMatch Pro | Thuê bao tháng/năm | Nguồn doanh thu định kỳ chính | `1.1.0` |
| 2 ⭐⭐⭐⭐⭐ | Boost Profile | Trả tiền theo từng lượt/thời hạn | Doanh thu theo nhu cầu tăng hiển thị | `1.1.0` |
| 3 ⭐⭐⭐⭐☆ | Featured Photographer | Mua vị trí quảng cáo | Doanh thu từ vị trí nổi bật theo khu vực/danh mục | `1.1.0`, rollout sau Boost |
| 4 ⭐⭐⭐⭐⭐ | SaaS for Photographer | Thuê bao công cụ vận hành | Mở rộng Photomatch thành hệ điều hành kinh doanh | Sau `1.1.0` |

## Người trả tiền

- Photographer là đối tượng trả phí chính.
- Customer tiếp tục dùng miễn phí đăng ký, discovery, match, chat và booking.
- Không thu phí membership từ customer trong `1.1.0`.
- Không thu commission theo booking trong `1.1.0`.
- Không bán quyền xác minh danh tính, quyền report, quyền block hoặc quyền vượt qua moderation.

## Nguyên tắc sản phẩm

- Free phải đủ dùng để photographer mới có thể nhận khách và chứng minh giá trị của marketplace.
- Pro giúp vận hành tốt hơn và có lợi thế hợp lý, nhưng không bảo đảm có khách.
- Boost/Featured chỉ tăng phân phối trong tập customer phù hợp; không bỏ qua filter, block hoặc relevance.
- Mọi vị trí quảng cáo phải được gắn nhãn rõ ràng.
- Giá, thời hạn, quota và điều kiện gia hạn phải hiển thị bằng tiền thật; không dùng coin/diamond.
- `VERIFIED` phản ánh kết quả xác minh thật; `PRO` phản ánh thuê bao. Hai badge không được gộp.

# SCOPE

## In scope `1.1.0`

- Product catalog cho Pro, Boost và Featured.
- Giá theo platform, khu vực, currency và billing period.
- Apple In-App Purchase và Google Play Billing cho tính năng số trên mobile.
- Backend verify purchase, subscription lifecycle và restore purchase.
- Entitlement service dùng chung cho Pro, portfolio, AI, analytics và promotion.
- Pro badge tách biệt Verified badge.
- Portfolio không giới hạn theo sản phẩm, có technical fair-use policy.
- AI hỗ trợ nội dung hồ sơ/portfolio và insight cơ bản.
- Photographer analytics, tách organic traffic và paid traffic.
- Boost 24 giờ, Boost 7 ngày và Boost theo khu vực.
- Featured Photographer theo khu vực, danh mục hoặc search placement.
- Admin cấu hình product, giá, entitlement, placement và campaign.
- Tracking impression, profile view, click, like, match và booking attribution.

## Out of scope `1.1.0`

- Thu commission booking.
- Thanh toán tiền chụp ảnh, đặt cọc, refund, payout hoặc escrow.
- Đấu giá quảng cáo theo thời gian thực.
- Bán Verified badge.
- Full CRM, hợp đồng điện tử và gallery giao ảnh production-ready.
- AI tự động gửi tin nhắn hoặc tự quyết định thay photographer.
- Customer subscription.

## Future foundation

ERD `1.1.0` chuẩn bị schema cho các module sau nhưng các bảng này chưa bắt buộc triển khai trong release:

- CRM khách hàng và lead.
- Lịch chụp và lịch làm việc.
- Báo giá và quote items.
- Hợp đồng và các bên ký.
- Client gallery và quyền truy cập ảnh.
- AI jobs và usage ledger có thể dùng cho nhiều tính năng SaaS.

# PRIORITY 1 — PHOTOMATCH PRO

## Mô hình

PhotoMatch Pro là thuê bao dành cho photographer, ưu tiên gói tháng và có thể bổ sung gói năm. Đây là nguồn Monthly Recurring Revenue chính.

## Free vs Pro

| Khả năng | Photographer Free | PhotoMatch Pro |
| :---- | :---- | :---- |
| Hồ sơ photographer | Có | Có |
| Verified badge | Có nếu xác minh thành công | Có nếu xác minh thành công |
| Pro badge | Không | Có trong thời gian subscription active |
| Portfolio | Giới hạn theo cấu hình, giả thuyết `30` item | Không có product hard-cap; áp dụng fair use |
| Analytics | Tổng quan 7 ngày | Chi tiết 30/90 ngày và funnel conversion |
| AI hỗ trợ | Trial/quota nhỏ | Quota hàng tháng cao hơn |
| Chủ động tìm customer | Không hoặc quota rất thấp | Có quota cao hơn theo ngày |
| Filter nâng cao | Không | Có |
| Khả năng hiển thị | Organic ranking | Pro ranking signal có trọng số giới hạn |
| Boost đi kèm | Không | Có thể tặng 1 Boost 24 giờ mỗi chu kỳ |
| Hỗ trợ | Tiêu chuẩn | Ưu tiên theo SLA nội bộ |

## Pro badge và Verified badge

```text
PRO       = user đang có subscription/entitlement Pro hợp lệ
VERIFIED  = user đã hoàn thành quy trình xác minh danh tính
```

- Photographer có thể chỉ có `PRO`, chỉ có `VERIFIED`, có cả hai hoặc không có badge nào.
- Không đặt tên badge trả phí là `Pro/Verified` vì dễ khiến customer hiểu nhầm rằng tiền có thể mua độ tin cậy.
- Khi subscription hết hạn, `PRO` biến mất sau khi entitlement hết hiệu lực; `VERIFIED` không bị ảnh hưởng.

## Portfolio không giới hạn

“Không giới hạn” là quyền lợi sản phẩm, không có nghĩa hệ thống chấp nhận sử dụng tài nguyên vô hạn.

- Không đặt hard-cap nhỏ cho Pro.
- Có giới hạn kỹ thuật theo dung lượng file, định dạng, tổng storage bất thường và chống abuse.
- Ảnh tiếp tục được kiểm tra an toàn, tối ưu thumbnail và lifecycle storage.
- Khi Pro hết hạn, không xóa portfolio. Photographer không thể thêm mới nếu đang vượt quota Free.

## AI hỗ trợ trong `1.1.0`

- Gợi ý headline và bio cho hồ sơ.
- Gợi ý title, caption và tag cho portfolio.
- Tóm tắt insight analytics bằng ngôn ngữ dễ hiểu.
- Gợi ý cải thiện hồ sơ dựa trên dữ liệu của chính photographer.
- Không auto-publish và không auto-send; photographer phải xác nhận kết quả.
- Mỗi lần sử dụng phải ghi usage để kiểm soát quota và chi phí.

## Analytics cho photographer

- Số lần xuất hiện trong discovery/search/nearby.
- Profile views.
- Lượt quan tâm nhận được.
- Match và booking được tạo.
- Funnel `impression → profile view → like → match → booking`.
- Tách `ORGANIC`, `PRO_SIGNAL`, `BOOST` và `FEATURED` để photographer hiểu nguồn traffic.
- Không tiết lộ dữ liệu cá nhân hoặc hành vi chi tiết của customer khác.

## Visibility advantage

Pro có thể là một tín hiệu ranking nhỏ nhưng không được trở thành điều kiện để xuất hiện.

```text
final_score = relevance_score
            + quality_score
            + availability_score
            + bounded_pro_weight
            + active_campaign_weight
```

`bounded_pro_weight` và `active_campaign_weight` phải có giới hạn để hồ sơ không phù hợp không vượt lên chỉ vì trả tiền.

# PRIORITY 2 — BOOST PROFILE

## Sản phẩm Boost

| Product | Thời lượng | Target | Mục đích |
| :---- | :---- | :---- | :---- |
| Boost 24H | 24 giờ | Mặc định theo khu vực hoạt động | Tăng hiển thị ngắn hạn |
| Boost 7D | 7 ngày | Mặc định theo khu vực hoạt động | Dùng cho thời điểm cần tìm khách liên tục |
| Regional Boost | 24 giờ hoặc 7 ngày | Thành phố/khu vực được chọn | Tập trung vào Hà Nội, TP.HCM hoặc khu vực cấu hình |
| Category Boost | 24 giờ hoặc 7 ngày | Dịch vụ/danh mục | Wedding, Portrait, Event hoặc category khác |

## Business rules

- Chỉ photographer có profile public, available và đủ điều kiện tối thiểu mới kích hoạt Boost.
- Photographer bị penalty hoặc hidden profile không được chạy campaign.
- Thời gian campaign được tính từ lúc backend kích hoạt thành công.
- Boost hết hạn tự động và không gia hạn nếu chưa có xác nhận mua mới.
- Một photographer không được cộng dồn vô hạn nhiều Boost trên cùng placement.
- Campaign vẫn phải tuân theo city, service, distance, block và discovery filter.
- Boost không bảo đảm số impression, match hoặc booking cụ thể trừ khi sau này có product guarantee riêng.
- Impression/click/profile view phải được ghi nhận để đo hiệu quả campaign.

## Pricing hypotheses

| Product | Khoảng giá thử nghiệm |
| :---- | :---- |
| Boost 24H | `29.000–49.000 VND` |
| Boost 7D | `99.000–199.000 VND` |
| Regional/Category targeting | Có thể cộng thêm `20–40%` tùy demand |

Giá là giả thuyết cần kiểm chứng, không phải bảng giá đã phê duyệt.

# PRIORITY 3 — FEATURED PHOTOGRAPHER

## Mô hình

Featured Photographer bán vị trí nổi bật trong các placement có giới hạn.

| Placement | Ví dụ | Cách hiển thị |
| :---- | :---- | :---- |
| Search Featured | Kết quả tìm photographer | Vị trí nổi bật có nhãn `Tài trợ` |
| Region Top | Photographer nổi bật tại Hà Nội | Khối nổi bật theo thành phố |
| Category Top | Top Wedding/Portrait/Event | Khối nổi bật theo service/category |
| Discovery Featured | Discovery feed | Chèn có frequency cap giữa organic results |

## Business rules

- Featured là quảng cáo; UI phải có nhãn phân biệt với kết quả organic.
- Placement có inventory và số slot tối đa theo thời gian.
- `1.1.0` dùng fixed package hoặc admin-configured price, chưa dùng auction.
- Nếu inventory hết, không được bán campaign trùng slot ngoài capacity.
- Photographer phải phù hợp target và đạt quality threshold.
- Admin có thể pause campaign vi phạm nhưng không sửa lịch sử purchase.
- Ranking trong slot Featured vẫn ưu tiên relevance giữa các campaign đủ điều kiện.
- Frequency cap bảo vệ customer khỏi thấy một advertiser quá nhiều lần.

## Pricing hypotheses

| Product | Khoảng giá thử nghiệm |
| :---- | :---- |
| Featured theo khu vực 7 ngày | `299.000–699.000 VND` |
| Featured theo danh mục 7 ngày | `399.000–999.000 VND` |
| Search Featured | Theo inventory và demand của placement |

# PRIORITY 4 — SAAS FOR PHOTOGRAPHER

## Tầm nhìn

Khi marketplace đủ lớn, Photomatch phát triển từ nơi tìm khách thành công cụ vận hành công việc cho photographer.

```text
Marketplace acquisition
→ Lead/CRM
→ Calendar
→ Quote
→ Contract
→ Shoot workflow
→ Client gallery
→ Repeat customer
```

## Module roadmap

| Module | Khả năng | Điều kiện triển khai |
| :---- | :---- | :---- |
| CRM | Contact, lead, pipeline, follow-up | Photographer có lượng lead lặp lại đủ lớn |
| Calendar | Lịch chụp, lịch bận, reminder | Booking usage ổn định |
| Quotes | Báo giá, line item, version, accept/reject | Có nhu cầu chuẩn hóa thương lượng |
| Contracts | Template, snapshot, parties, signature status | Có legal review và signature provider |
| Client Gallery | Gallery riêng tư, quyền truy cập, download | Có storage/CDN economics phù hợp |
| AI Copilot | Soạn nội dung, quote draft, workflow insight | Có quota, privacy và cost guardrail |

## Ranh giới với `1.1.0`

- `1.1.0` chỉ triển khai AI/analytics phục vụ Pro ở mức cơ bản.
- Các bảng SaaS được định nghĩa để giữ hướng mở rộng, nhưng API/UI SaaS chưa nằm trong release commitment.
- SaaS chỉ được bật sau khi marketplace đạt các product gate ở phần KPI.

# MONETIZATION CATALOG

## Product types

| ProductType | Ý nghĩa |
| :---- | :---- |
| `SUBSCRIPTION` | Gói Pro tháng/năm |
| `BOOST` | Campaign tăng hiển thị theo thời lượng/target |
| `FEATURED` | Vị trí quảng cáo có inventory |
| `AI_CREDIT` | Gói quota AI mua thêm trong tương lai |
| `SAAS_ADDON` | Add-on CRM/gallery/AI trong tương lai |

## Entitlements

| EntitlementCode | Giá trị |
| :---- | :---- |
| `PRO_BADGE` | Hiển thị badge Pro |
| `PORTFOLIO_LIMIT` | Số item portfolio; Pro có giá trị `UNLIMITED_FAIR_USE` |
| `CUSTOMER_DISCOVERY_DAILY_LIMIT` | Quota chủ động tìm customer |
| `ADVANCED_FILTERS` | Mở filter nâng cao |
| `ANALYTICS_RETENTION_DAYS` | Khoảng thời gian analytics |
| `AI_MONTHLY_CREDITS` | Quota AI theo chu kỳ |
| `PRO_RANKING_WEIGHT` | Trọng số Pro có giới hạn |
| `INCLUDED_BOOST_24H` | Số Boost tặng trong chu kỳ |
| `CRM_ACCESS` | Quyền dùng CRM tương lai |
| `GALLERY_STORAGE_GB` | Storage gallery tương lai |

Entitlement phải được cấu hình qua data, không hard-code logic theo tên plan trong mobile/backend.

# BILLING FLOW

## Kênh thanh toán

Pro, Boost và Featured là tính năng số sử dụng trong app. Trên mobile, purchase flow phải đi qua Apple In-App Purchase hoặc Google Play Billing theo chính sách của store tại thị trường phát hành.

- [Apple App Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)
- [Google Play Payments Policy](https://support.google.com/googleplay/android-developer/answer/9858738)

## Purchase flow

```text
Mobile lấy product catalog từ backend
→ Mobile khởi tạo purchase với Apple/Google
→ Store trả transaction cho mobile
→ Mobile gửi transaction reference lên backend
→ Backend xác minh trực tiếp với store
→ Backend lưu PURCHASE_TRANSACTIONS
→ Backend tạo/cập nhật SUBSCRIPTIONS hoặc USER_ENTITLEMENTS
→ Mobile refresh entitlement
```

## Subscription lifecycle

- Hỗ trợ purchase, renew, grace period, cancel, expire, refund/revoke và restore.
- Store transaction và billing event phải idempotent.
- Webhook/store notification là nguồn sự thật cho renewal/revoke.
- Mobile không tự cấp Pro chỉ dựa trên local receipt.
- Product price không sửa ngược lịch sử; tạo price version mới với thời gian hiệu lực.

# FEATURE SCOPE

## Mobile

| Feature | Actor | Mô tả |
| :---- | :---- | :---- |
| Pro overview | Photographer | Xem quyền lợi, giá, billing period và trial nếu có. |
| Purchase Pro | Photographer | Mua subscription qua store billing. |
| Restore purchase | Photographer | Khôi phục entitlement khi đổi/cài lại thiết bị. |
| Manage subscription | Photographer | Mở trang quản lý subscription của store. |
| Pro badge | User | Hiển thị trạng thái Pro tách khỏi Verified. |
| Portfolio entitlement | Photographer | Kiểm tra quota Free/Pro trước khi upload. |
| AI assistant | Photographer | Tạo gợi ý và xác nhận trước khi áp dụng. |
| Analytics dashboard | Photographer | Xem traffic, funnel và attribution. |
| Buy Boost | Photographer | Chọn 24H/7D và target hợp lệ. |
| Campaign status | Photographer | Xem scheduled/active/expired và kết quả. |
| Featured placement | Photographer | Mua placement đang còn inventory. |
| Sponsored result | Customer | Nhìn thấy nhãn quảng cáo rõ ràng. |

## Web admin

| Feature | Actor | Mô tả |
| :---- | :---- | :---- |
| Product catalog | Admin | Quản lý product, price, platform và thời gian hiệu lực. |
| Entitlement mapping | Admin | Map product sang quyền lợi có typed value. |
| Subscription lookup | Admin | Tra cứu lifecycle subscription. |
| Purchase lookup | Admin | Tra cứu store transaction và billing event. |
| Promotion placement | Admin | Cấu hình slot, capacity, frequency cap và status. |
| Campaign management | Admin | Xem/pause campaign Boost/Featured. |
| Campaign analytics | Admin | Impression, profile view, conversion và revenue. |
| AI usage dashboard | Admin | Theo dõi quota, cost và lỗi AI job. |
| Fair-use review | Admin | Xử lý storage/AI usage bất thường. |

# BUSINESS RULES

| ID | Quy tắc |
| :---- | :---- |
| `BR-MON-001` | Customer không cần subscription để sử dụng luồng cốt lõi. |
| `BR-MON-002` | Chỉ photographer role hợp lệ mới được nhận entitlement Pro/Boost/Featured. |
| `BR-MON-003` | `PRO` và `VERIFIED` là hai trạng thái độc lập. |
| `BR-MON-004` | Không bán hoặc tự động cấp `VERIFIED` từ purchase. |
| `BR-MON-005` | Entitlement chỉ được cấp sau khi backend xác minh purchase/store event. |
| `BR-MON-006` | Mỗi external transaction/event chỉ được xử lý một lần. |
| `BR-MON-007` | Hết Pro không xóa dữ liệu; chỉ áp quota Free cho thao tác mới. |
| `BR-MON-008` | “Unlimited portfolio” luôn chịu technical fair-use và safety policy. |
| `BR-MON-009` | Boost/Featured không bỏ qua relevance, block, visibility hoặc penalty. |
| `BR-MON-010` | Featured placement phải có nhãn quảng cáo. |
| `BR-MON-011` | Không cho campaign chạy ngoài target city/service đã mua. |
| `BR-MON-012` | Campaign đã pause/expired không đóng góp ranking weight. |
| `BR-MON-013` | Pro ranking weight và campaign weight phải có upper bound. |
| `BR-MON-014` | AI output không auto-publish hoặc auto-send. |
| `BR-MON-015` | AI usage phải được ghi nhận để enforce quota và kiểm soát chi phí. |
| `BR-MON-016` | Analytics không tiết lộ dữ liệu nhận dạng customer cho photographer. |
| `BR-MON-017` | SaaS future tables không đồng nghĩa feature đã được release. |

# ENUMS

## SubscriptionStatus

`PENDING`, `TRIALING`, `ACTIVE`, `GRACE_PERIOD`, `PAST_DUE`, `CANCELLED`, `EXPIRED`, `REVOKED`

## PurchaseProvider

`APPLE`, `GOOGLE`, `ADMIN_GRANT`

## PurchaseStatus

`PENDING`, `VERIFIED`, `FAILED`, `REFUNDED`, `REVOKED`

## CampaignType

`BOOST`, `FEATURED`

## CampaignStatus

`DRAFT`, `SCHEDULED`, `ACTIVE`, `PAUSED`, `COMPLETED`, `CANCELLED`, `REJECTED`

## TargetScope

`GLOBAL`, `REGION`, `CATEGORY`, `REGION_AND_CATEGORY`

## PromotionEventType

`IMPRESSION`, `CLICK`, `PROFILE_VIEW`, `LIKE`, `MATCH`, `BOOKING_CREATED`

## TrafficSource

`ORGANIC`, `PRO_SIGNAL`, `BOOST`, `FEATURED`

## AiJobStatus

`QUEUED`, `PROCESSING`, `COMPLETED`, `FAILED`, `CANCELLED`

# API CONTRACT DELTA

Base path tiếp tục là `/api/v1` vì thay đổi tương thích ngược.

## Mobile API

| Domain | Endpoint | Method | Actor | Mục đích |
| :---- | :---- | :---- | :---- | :---- |
| Catalog | `/monetization/products` | `GET` | Photographer | Product/price khả dụng theo platform/region. |
| Purchase | `/billing/store-transactions/verify` | `POST` | Photographer | Verify transaction và cấp entitlement. |
| Purchase | `/billing/store-transactions/restore` | `POST` | Photographer | Restore purchase. |
| Subscription | `/me/subscription` | `GET` | Photographer | Xem Pro lifecycle hiện tại. |
| Entitlement | `/me/entitlements` | `GET` | Photographer | Lấy quyền và quota đang hiệu lực. |
| Usage | `/me/feature-usage` | `GET` | Photographer | Xem quota đã dùng/còn lại. |
| Analytics | `/me/photographer-analytics` | `GET` | Photographer | Xem funnel và attribution. |
| AI | `/ai/jobs` | `POST` | Photographer | Tạo AI job theo entitlement/quota. |
| AI | `/ai/jobs/{jobId}` | `GET` | Photographer | Xem kết quả job. |
| Placement | `/promotion-placements` | `GET` | Photographer | Lấy inventory khả dụng. |
| Campaign | `/promotion-campaigns` | `POST` | Photographer | Tạo Boost/Featured từ purchase hợp lệ. |
| Campaign | `/promotion-campaigns` | `GET` | Photographer | Danh sách campaign của mình. |
| Campaign | `/promotion-campaigns/{campaignId}` | `GET` | Photographer | Chi tiết và analytics campaign. |
| Campaign | `/promotion-campaigns/{campaignId}/activate` | `POST` | Photographer | Kích hoạt campaign đã đủ điều kiện. |

## Store notification API

| Endpoint | Nguồn | Yêu cầu |
| :---- | :---- | :---- |
| `/webhooks/apple` | Apple | Verify signed payload và deduplicate event. |
| `/webhooks/google` | Google | Verify token/signature và deduplicate event. |

## Web admin API

| Domain | Endpoint | Method | Mục đích |
| :---- | :---- | :---- | :---- |
| Summary | `/admin/monetization/summary` | `GET` | MRR, subscription, campaign revenue và conversion. |
| Products | `/admin/monetization/products` | `GET/POST` | Quản lý product catalog. |
| Products | `/admin/monetization/products/{productId}` | `PATCH` | Cập nhật trạng thái/cấu hình future-effective. |
| Prices | `/admin/monetization/products/{productId}/prices` | `GET/POST` | Quản lý price versions. |
| Entitlements | `/admin/monetization/products/{productId}/entitlements` | `PUT` | Map product sang entitlement. |
| Subscriptions | `/admin/subscriptions` | `GET` | Tra cứu subscription. |
| Purchases | `/admin/purchase-transactions` | `GET` | Tra cứu purchase đã verify. |
| Placements | `/admin/promotion-placements` | `GET/POST` | Quản lý placement/inventory. |
| Campaigns | `/admin/promotion-campaigns` | `GET` | Tra cứu campaign. |
| Campaigns | `/admin/promotion-campaigns/{campaignId}/status` | `POST` | Pause/resume/cancel theo rule. |
| Analytics | `/admin/promotion-analytics` | `GET` | Hiệu quả Pro/Boost/Featured. |
| AI usage | `/admin/ai-usage` | `GET` | Theo dõi quota và cost. |

# DATA MODEL STRATEGY

ERD `1.1.0` gồm ba lớp:

1. Core marketplace: kế thừa 37 bảng từ `1.0.0`.
2. Monetization runtime: product, price, subscription, entitlement, promotion, analytics và AI usage.
3. SaaS future foundation: CRM, calendar, quote, contract và gallery; chưa bắt buộc migrate trong release `1.1.0`.

Không thêm cột `is_pro` vào `USERS` hoặc `PHOTOGRAPHER_PROFILES`. Pro được suy ra từ `USER_ENTITLEMENTS` đang hiệu lực để tránh trạng thái bị lệch với subscription.

# NFR

## Security & privacy

- Backend verify store transaction và signed notification.
- Không log receipt/token/secret ở dạng thô.
- Entitlement admin grant/revoke phải có audit trail.
- AI chỉ nhận dữ liệu cần thiết và không dùng private chat/file nếu chưa có consent riêng.
- CRM/gallery future data phải được phân quyền theo photographer tenant.
- Gallery private assets dùng signed URL có thời hạn.

## Reliability

- Store notification dùng inbox pattern, unique external event ID và idempotent processing.
- Có reconciliation job giữa store transaction, subscription và entitlement.
- Campaign activation/expiry chạy bằng scheduler có retry và lock chống chạy trùng.
- Entitlement cache phải có TTL ngắn và cơ chế invalidate khi renew/revoke.
- Analytics event có thể ghi bất đồng bộ; không làm chậm discovery response.

## Performance

| Chỉ số | Mục tiêu ban đầu |
| :---- | :---- |
| Entitlement check | p95 dưới `200 ms` với cache |
| Discovery có promotion ranking | p95 không tăng quá `100 ms` so với baseline |
| Campaign activation | Dưới `5 giây` sau khi đủ điều kiện |
| Analytics dashboard | p95 dưới `2 giây` cho khoảng 90 ngày |
| AI job submit | Dưới `1 giây`; xử lý async |

## Scalability

- `PROMOTION_EVENTS` partition theo thời gian khi volume tăng.
- Dùng aggregate `PHOTOGRAPHER_ANALYTICS_DAILY` cho dashboard, không query raw events trực tiếp.
- Product/entitlement được cấu hình bằng data để thêm plan/add-on không cần đổi schema.
- Placement, city và service target dùng quan hệ riêng; không lưu danh sách ID trong JSON.
- AI usage dùng ledger chung để thêm model/provider/feature mới.

# KPI & RELEASE GATES

## Revenue KPI

| KPI | Ý nghĩa |
| :---- | :---- |
| MRR | Doanh thu thuê bao Pro hàng tháng |
| Pro conversion | Photographer active chuyển sang Pro |
| Paid subscriber retention | Subscription renew qua các chu kỳ |
| Pro churn | Subscription không renew/cancel |
| ARPPU | Doanh thu trung bình trên photographer trả phí |
| Boost repeat rate | Photographer mua Boost lặp lại |
| Featured fill rate | Tỷ lệ slot Featured được bán |
| Promotion revenue | Doanh thu Boost + Featured |

## Product KPI

- Organic impressions, paid impressions và tỷ lệ paid/organic.
- Profile view rate theo traffic source.
- Like, match và booking conversion theo traffic source.
- AI activation, completion và repeat usage.
- Portfolio storage cost trên Free/Pro photographer.
- Tỷ lệ photographer đạt giá trị đầu tiên trước khi nhìn thấy paywall.

## Guardrails

- Organic discovery của Free không bị giảm quá `5%` do paid placements.
- Không để một photographer chiếm quá frequency cap trong một customer session.
- Paid result vẫn phải đạt relevance/quality threshold.
- Report/block rate từ paid traffic không vượt organic traffic quá ngưỡng cảnh báo.
- Không ra mắt Featured nếu chưa đo được impression và attribution tin cậy.
- Không quảng cáo “unlimited” nếu fair-use policy chưa được công bố rõ.

## SaaS activation gates

Chỉ chuyển CRM/calendar/quote/contract/gallery thành feature production khi:

- Có lượng photographer active và paid subscriber đủ lớn để phỏng vấn/đo nhu cầu lặp lại.
- Một tỷ lệ đáng kể photographer quản lý nhiều lead/booking mỗi tháng.
- Pro retention cho thấy nhu cầu công cụ vận hành, không chỉ nhu cầu tăng hiển thị.
- Storage, AI và support unit economics có thể kiểm soát.

# DELIVERY PLAN

## Complexity

| Chiều đánh giá | Điểm `0–4` | Lý do |
| :---- | :---- | :---- |
| Business rules | 4 | Subscription, entitlement, promotion inventory và fairness liên kết nhau. |
| Data model | 4 | Product catalog, campaign events, analytics và future SaaS foundation. |
| External integration | 3 | Apple/Google billing và AI provider. |
| Security/compliance | 3 | Store billing, AI privacy, quảng cáo minh bạch. |
| UI/operations | 4 | Mobile paywall/campaign/analytics và admin monetization. |
| Tổng | `18/20 - Enterprise` | Cần rollout theo phase và feature flag. |

## WBS & estimate

| Phase | Hạng mục | Owner chính | Effort dự kiến |
| :---- | :---- | :---- | :---- |
| 0 | Chốt pricing, entitlement, fair-use và placement policy | Product, Legal | 1–2 tuần |
| 1 | Product catalog, price versioning, store billing, subscription | Backend, Mobile | 2–3 tuần |
| 2 | Entitlement service, Pro badge, portfolio, filters | Backend, Mobile | 2–3 tuần |
| 3 | Analytics events, daily aggregates, photographer dashboard | Backend, Mobile, Data | 2–3 tuần |
| 4 | Boost campaign, regional/category target và campaign analytics | Backend, Mobile, Admin Web | 2–3 tuần |
| 5 | Featured placement, inventory, labeling và frequency cap | Backend, Mobile, Admin Web | 2–3 tuần |
| 6 | AI jobs, quota/usage, safety và cost dashboard | Backend, Mobile | 2–3 tuần |
| 7 | Sandbox/UAT, pilot cohort, store review và rollout | QA, Product, Operations | 2–3 tuần |

Các phase có thể chạy song song sau khi product catalog, entitlement contract và promotion ranking rule được baseline.

## Milestones

| Milestone | Kết quả | Exit criteria |
| :---- | :---- | :---- |
| `M1 - Commercial baseline` | Pro/Boost/Featured pricing draft | Product, entitlement và fair-use được duyệt. |
| `M2 - Billing sandbox` | Pro purchase lifecycle chạy đầy đủ | Purchase/renew/cancel/expire/revoke/restore pass. |
| `M3 - Pro pilot` | Pro badge, portfolio, analytics và AI hoạt động | Không cấp entitlement sai; Free flow không regression. |
| `M4 - Boost pilot` | Campaign 24H/7D/region hoạt động | Activation/expiry/target/attribution pass. |
| `M5 - Featured pilot` | Placement inventory và sponsored labeling hoạt động | Frequency/relevance guardrails pass. |
| `M6 - General availability` | Phát hành post-MVP `1.1.0` | KPI instrumentation và operational runbook sẵn sàng. |

# RISKS & MITIGATIONS

| Rủi ro | Tác động | Giảm thiểu |
| :---- | :---- | :---- |
| Bán Pro trước khi có đủ customer demand | Churn cao | Chỉ paywall sau khi photographer đã nhận giá trị; trial/pilot cohort. |
| Paid ranking làm Free mất cơ hội | Marketplace giảm thanh khoản | Bounded weight, paid inventory cap và organic guardrail. |
| Pro badge bị hiểu là Verified | Mất trust | Badge, màu, tooltip và logic tách biệt hoàn toàn. |
| Unlimited portfolio làm tăng storage cost | Unit economics xấu | Fair-use, compression, lifecycle và abuse detection. |
| Boost/Featured không tạo kết quả | Photographer không mua lại | Target chính xác, analytics minh bạch và không cam kết sai. |
| Featured làm search giống quảng cáo | Customer giảm tương tác | Sponsored label, slot cap, relevance và frequency cap. |
| AI cost vượt subscription revenue | Margin âm | Monthly quota, model routing, usage ledger và kill switch. |
| Store event replay/out-of-order | Entitlement sai | Signature verify, unique external ID, event inbox và reconciliation. |
| Xây SaaS quá sớm | Scope lớn nhưng ít dùng | Chỉ chuẩn bị schema; triển khai theo activation gate. |

# OPEN DECISIONS

- Giá chính thức của Pro tháng/năm và có trial hay không.
- Quota portfolio Free và định nghĩa fair-use cho Pro.
- Quota chủ động tìm customer của Free/Pro.
- Quota AI theo tháng và feature nào được tính bao nhiêu credit.
- Pro ranking weight tối đa.
- Boost 24H/7D có target region/category mặc định hay add-on.
- Số slot và capacity của từng Featured placement.
- Chính sách pause/refund digital campaign khi lỗi hệ thống.
- Provider/model AI và chính sách sử dụng dữ liệu.
- Điều kiện định lượng để bắt đầu SaaS roadmap.

# CHANGE RECORD

| Version | Ngày | Change request | Nội dung | Trạng thái |
| :---- | :---- | :---- | :---- | :---- |
| `1.1.0` | `2026-07-18` | `CR-001` | PhotoMatch Pro, Boost Profile, Featured Photographer và SaaS future foundation. | Draft |

# APPROVAL CHECKLIST

- [ ] Product duyệt scope và thứ tự ưu tiên.
- [ ] Product/Finance duyệt pricing hypotheses.
- [ ] Legal duyệt subscription terms, sponsored labeling và fair-use.
- [ ] Engineering duyệt entitlement/promotion architecture.
- [ ] Mobile duyệt Apple/Google billing lifecycle.
- [ ] Data duyệt analytics event và attribution model.
- [ ] Security/Privacy duyệt AI data flow và admin grant/revoke.
- [ ] QA có test matrix purchase/renew/expire/restore/campaign/entitlement.
- [ ] Pilot đạt revenue KPI và discovery guardrails.
- [ ] ERD/API/test delta được re-baseline cho `1.1.0`.
