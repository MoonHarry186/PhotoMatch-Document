# CR-003 — Hợp nhất Nearby vào bộ lọc Discovery

| Version | Date | Author | Status |
|---------|------|--------|--------|
| — | 2026-07-30 | Codex | Approved for draft incorporation |

## 1. Change Request

| Mục | Nội dung |
|-----|----------|
| **CR ID** | CR-003 |
| **Date submitted** | 2026-07-30 |
| **Requester** | Product owner |
| **Approver** | Product owner |
| **Status** | Approved |
| **Change type** | Modify / remove |
| **Priority** | High |
| **Current baseline** | PhotoMatch MVP plan `1.0.0` — Draft |

### Description

Loại tab và màn Nearby độc lập khỏi mobile MVP. Khả năng tìm Photographer theo
vị trí được chuyển thành bộ lọc opt-in `Gần tôi` trong Discovery:

- Discovery mặc định không yêu cầu GPS hoặc discovery presence;
- chỉ khi bật `Gần tôi`, app mới xin foreground location, cập nhật owner location
  và gửi bán kính lên server trước pagination;
- kết quả gần chỉ hiển thị distance bucket, không trả tọa độ của target;
- navigation chính còn Discovery, Messages và Profile;
- endpoint `/nearby` được giữ để tương thích, nhưng mobile MVP không expose route
  hoặc tab riêng.

CR này thay thế phần “giữ Nearby list độc lập” của CR-002; quyết định loại map
provider, marker và public-coordinate contract trong CR-002 vẫn giữ nguyên.

## 2. Affected Requirements

| Requirement | Current | Proposed |
|-------------|---------|----------|
| Bottom navigation | Discovery, Nearby, Messages, Profile | Discovery, Messages, Profile |
| Discovery mặc định | Phụ thuộc location/presence vì dùng Nearby query | Không yêu cầu location; trả candidate đủ eligibility |
| Nearby experience | Màn danh sách riêng | Toggle `Gần tôi` và bán kính trong Discovery filter |
| Location permission | Mở từ tab Nearby | Chỉ hỏi khi user bật `Gần tôi`; quản lý provider location/presence trong Profile |
| Candidate distance | Luôn có | `null` ở feed global, distance bucket khi `Gần tôi` bật |

## 3. Affected Documents

| Artifact | Change summary |
|----------|----------------|
| `documents/1.0.0-mvp/plan/photomatch-mvp-plan.md` | Chuyển 4 tab thành 3 tab; hợp nhất Nearby vào Discovery filter |
| Mobile OpenSpec proposal/design/specs/tasks | Bỏ Nearby surface độc lập; thêm global-vs-nearby Discovery behavior |
| Backend OpenAPI | Cho phép candidate `distance` nullable |
| Mobile/API tests | Thêm coverage navigation, opt-in permission, global-vs-nearby và filter-before-pagination |

## 4. Impact Analysis

| Dimension | Impact | Detail |
|-----------|--------|--------|
| **Scope** | Reduced / consolidated | Bỏ một tab, screen, filter store và luồng danh sách trùng lặp |
| **Schedule** | Neutral | Có thêm nhánh query global nhưng giảm UI/navigation phải duy trì |
| **Cost** | Neutral | PostGIS/location vẫn phục vụ `Gần tôi`; không thêm provider |
| **Backend/API** | Additive-compatible | `/discovery/candidates` không có radius dùng feed global; có radius dùng nearby; `/nearby` giữ mặc định 20 km |
| **Database / ERD** | No change | Tiếp tục dùng user location và discovery presence cho `Gần tôi` |
| **Privacy** | Improved | GPS chỉ được yêu cầu khi user chủ động bật `Gần tôi` |
| **Quality / Regression** | Required | Navigation, filter persistence, permission states, global/nearby pagination, block/cooldown |
| **Risk** | Data readiness | Feed vẫn rỗng nếu không có Photographer đủ service, pricing, visibility và 6 portfolio items |

## 5. Regression Test Scope

- Bottom navigation không còn Nearby và static route không được export.
- Discovery mặc định không gửi radius hoặc yêu cầu location.
- Bật `Gần tôi` mới hiển thị bán kính, xin foreground permission và cập nhật owner
  location.
- Denied/restricted/services-disabled phải giữ feed mặc định khả dụng và hiển thị
  hướng xử lý.
- Global và nearby filters phải chạy server-side trước cursor pagination.
- Candidate DTO/local persistence/telemetry không chứa raw coordinates.
- LEFT cooldown, block exclusion, interest, match và unmatch không đổi.

## 6. Decision

| Mục | Nội dung |
|-----|----------|
| **Decision** | Approved |
| **Decision date** | 2026-07-30 |
| **Decision by** | Product owner |
| **Comments** | Loại tab Nearby; giữ khả năng tìm gần như bộ lọc opt-in trong Discovery |

## 7. Implementation

| Mục | Nội dung |
|-----|----------|
| **Target release** | MVP `1.0.0` draft |
| **Implementation status** | Implemented and verified |
| **New baseline** | Không tăng version cho tới khi draft `1.0.0` được sign-off |

### Verification

- Mobile lint, strict TypeScript và Prettier trên phạm vi thay đổi: đạt.
- Mobile unit tests: 121/121; integration: 3/3; contract: 15/15.
- API unit tests: 35/35; contract: 19/19; targeted E2E: 5/5.
- Expo public config và export iOS/Android/Web: đạt; static routes không còn Nearby.
