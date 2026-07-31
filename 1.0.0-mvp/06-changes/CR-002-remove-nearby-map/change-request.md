# CR-002 — Loại Nearby map khỏi MVP

| Version | Date | Author | Status |
|---------|------|--------|--------|
| — | 2026-07-30 | Codex | Approved for draft incorporation |

## 1. Change Request

| Mục | Nội dung |
|-----|----------|
| **CR ID** | CR-002 |
| **Date submitted** | 2026-07-30 |
| **Requester** | Product owner |
| **Approver** | Product owner |
| **Status** | Approved |
| **Change type** | Remove |
| **Priority** | Medium |
| **Current baseline** | PhotoMatch MVP plan `1.0.0` — Draft |

### Description

Loại trải nghiệm bản đồ và marker khỏi phạm vi MVP. Tiếp tục giữ:

- quyền vị trí foreground theo ngữ cảnh;
- owner cập nhật/xóa exact location;
- discovery presence và thời hạn hiển thị;
- Nearby dạng danh sách với khoảng cách gần đúng do backend trả;
- filter server-side theo dịch vụ, giá, bán kính, available và verified.

MVP không yêu cầu `react-native-maps`, Google Maps API key, map type, marker clustering hoặc API trả public coordinates. Backend không được trả exact GPS của người khác.

## 2. Affected Requirements

| Requirement | Current | Proposed |
|-------------|---------|----------|
| Nearby presentation | Bản đồ và danh sách/fallback | Danh sách Nearby với khoảng cách gần đúng |
| Location privacy | Mobile nhận public coordinates đã làm lệch | Mobile không nhận tọa độ người khác; chỉ nhận distance bucket |
| Map type | `STANDARD`, `SATELLITE`, `HYBRID` trong MVP | Deferred sau MVP |
| Google Maps | Native dependency, key, quota và device validation | Deferred sau MVP |

## 3. Affected Documents

| Artifact | Change summary |
|----------|----------------|
| `documents/1.0.0-mvp/plan/photomatch-mvp-plan.md` | Loại map UI, map type, Google Maps dependency/config/cost; giữ Nearby list |
| Mobile OpenSpec proposal/design | Đổi Nearby map thành Nearby list và distance bucket |
| `mobile-profiles-and-discovery/spec.md` | Thay map privacy bằng list privacy |
| Mobile OpenSpec tasks | Scope out map task/map-type task; cập nhật test/performance task |

## 4. Impact Analysis

| Dimension | Impact | Detail |
|-----------|--------|--------|
| **Scope** | Reduced | Không còn map, marker, clustering hoặc map provider fallback |
| **Schedule** | Reduced | Bỏ native map integration và device/provider QA |
| **Cost** | Reduced | Không cần Google Maps Platform cho MVP |
| **Backend/API** | No additive coordinate contract | `/nearby` hiện tại với distance bucket đủ cho list |
| **Database** | No change | PostGIS, exact location và discovery presence vẫn phục vụ Nearby/filter |
| **Privacy** | Improved | Mobile không nhận exact hoặc public coordinates của target |
| **Regression** | Required | Location permission, clear location, presence expiry và server-side filter |
| **Post-MVP** | Deferred | Map có thể được đề xuất lại bằng change request riêng |

## 5. Regression Test Scope

- Permission: undetermined, granted, denied/restricted và services disabled.
- Exact-location update/clear; clear phải tắt presence.
- Presence enable/disable/refresh/expiry.
- Nearby list loading/empty/error/offline và location-unavailable fallback.
- Filter phải được gửi lên server trước pagination; đổi filter/role/location phải reset cursor.
- DTO, local persistence và telemetry không chứa raw coordinates.

## 6. Decision

| Mục | Nội dung |
|-----|----------|
| **Decision** | Approved |
| **Decision date** | 2026-07-30 |
| **Decision by** | Product owner |
| **Comments** | Giữ Nearby dạng danh sách; map deferred khỏi MVP |

## 7. Implementation

| Mục | Nội dung |
|-----|----------|
| **Target release** | MVP `1.0.0` draft |
| **Implementation status** | Implemented and verified |
| **New baseline** | Không tăng version cho tới khi draft `1.0.0` được sign-off |

### Verification

- Mobile lint và strict TypeScript: đạt.
- Unit tests: 105/105 đạt, gồm permission states, presence expiry, filter bounds, filter-before-pagination, candidate allow-list và local-persistence allow-list không có coordinates.
- Integration tests: 3/3 đạt.
- OpenAPI contract tests: 9/9 đạt.
- Expo public config và export iOS/Android/Web: đạt, không còn native map plugin/config trong public app config.
