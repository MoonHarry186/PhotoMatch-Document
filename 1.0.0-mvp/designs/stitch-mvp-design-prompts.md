# Photomatch MVP - Stitch design prompts

Tài liệu này chuyển scope trong `photomatch-mvp-plan.md` thành các prompt có thể dùng trực tiếp trong Google Stitch. Prompt được viết bằng tiếng Anh để mô tả layout rõ hơn, nhưng toàn bộ nội dung hiển thị trong sản phẩm phải là tiếng Việt.

## Cách sử dụng

1. Tạo một project mobile mới trong Stitch và chạy `Prompt 0` để thiết lập visual language.
2. Trong cùng project đó, chạy lần lượt `Prompt 1` đến `Prompt 5` để Stitch tái sử dụng component và token đã tạo.
3. Tạo một project web riêng và chạy `Prompt 6` cho admin dashboard. Không trộn mobile app và web admin trong cùng một canvas.
4. Tạo một project/logo board riêng và chạy `Prompt 7` để thiết kế hệ thống logo Photomatch.
5. Nếu Stitch bỏ sót màn hình hoặc state, dùng `Prompt 5` như một vòng kiểm tra và bổ sung.

## Prompt 0 - Mobile foundation

```text
Design the visual foundation and main navigation shell for "Photomatch", a production-ready cross-platform mobile marketplace that helps customers find, match with, chat with, and book photographers in Vietnam.

This is the actual product UI, not a landing page or a marketing concept. Use a 390 x 844 mobile frame, respect iOS and Android safe areas, and make all screens feel implementable in React Native. All visible UI copy must be natural Vietnamese. Do not use lorem ipsum.

Product personality:
- Trustworthy, modern, calm, visual-first, and photography-focused.
- It should feel like a professional services marketplace, not a dating game. The raspberry pink brand color must feel confident and editorial, never romantic, sugary, or childish.
- Use authentic portfolio photography as the primary visual signal.
- Make verification, availability, price, distance, safety actions, and booking status easy to scan.
- Avoid decorative gradients, neon colors, floating blobs, oversized marketing typography, glassmorphism, excessive pills, and nested cards.

Design tokens:
- Primary raspberry pink: #D81B60; pressed/active: #C2185B; soft selected backgrounds: #FDF2F8 and #FCE7F3.
- Accent: #F59E0B, used sparingly for photographer role, rating, and portfolio highlights.
- Success: #16A34A; warning: #F97316; danger: #DC2626; info: #0891B2.
- Light background: #F8FAFC; surface: #FFFFFF; muted surface: #F1F5F9.
- Primary text: #0F172A; secondary text: #334155; muted text: #64748B; border: #E2E8F0.
- Dark background: #020617; dark surface: #0F172A; dark muted surface: #1E293B; dark border: #334155; dark primary text: #F8FAFC.
- Use Inter. Text styles: display 32/40 bold, H1 28/36 bold, H2 24/32 bold, H3 20/28 semibold, body 16/24 regular, small 14/20 regular, caption 12/16 medium, button 16/24 semibold.
- Use a 4-point spacing system with 16px default horizontal padding and 24px between sections.
- Radius: 6px for chips and inputs, 8px for buttons and compact cards, 12px only for sheets, modals, and image cards.
- Touch targets must be at least 44 x 44px. Use subtle shadows only for floating actions, sheets, and swipe cards.

Create a reusable mobile component language:
- Do not create a global app header. Use only minimal contextual top controls such as back, close, progress, or overflow when the current screen requires them.
- Bottom navigation with four destinations: "Khám phá", "Quanh đây", "Tin nhắn", "Hồ sơ".
- Primary, secondary, text, and destructive buttons with default, pressed, disabled, and loading states.
- Text input, password input, select, multi-select, date/time field, price range field, switch, checkbox, filter chip, status badge, avatar, image uploader, and file uploader.
- Photographer profile summary, portfolio grid, rating summary, review card, 1-5 star rating input, swipe card, chat bubble, booking status badge, skeleton, empty state, inline error, modal, and bottom sheet.
- Use familiar outline icons. Icon-only buttons need clear visual meaning and accessible labels.
- Status must never rely on color alone; always include a label or icon.

Navigation and interaction rules:
- Logged-in screens must not repeat the Photomatch wordmark, search icon, and notification bell in a persistent top bar.
- Begin primary content immediately below the safe area. Put titles, search, filters, and actions inside the relevant content area only when needed.
- The Photomatch logo may appear on Splash and Auth screens, but never as a repeated navigation header after sign-in.
- The main tabs are always stable and do not shift when labels or badges change.
- Use bottom sheets for filters, quick actions, and short confirmations.
- Use a dedicated screen for long forms, booking creation, profile editing, and safety reports.
- Place the primary action within easy thumb reach without covering content.
- Include realistic loading, empty, validation error, network error, disabled, permission denied, and success feedback patterns.
- Support dynamic text sizing without clipping or overlap.

Create one compact design-foundation board plus the four-tab app shell in light mode. Also provide a dark-mode sample of the app shell to confirm semantic token mapping. Use a simple temporary Photomatch wordmark only on Splash/Auth; do not place it in the logged-in app shell and do not invent a complex final logo.
```

## Prompt 1 - Auth and onboarding

```text
Continue the existing Photomatch mobile project. Reuse the established tokens, components, typography, spacing, and icon style. Design the complete authentication and onboarding flow as connected, production-ready mobile screens. All visible copy must be Vietnamese and concise.

Global mobile chrome rule for every screen in this batch:
- Do not create a persistent global header, branded app bar, centered wordmark, global search icon, notification bell, bottom divider, or reserved blank space at the top.
- Start page content immediately below the safe area. Use only minimal contextual controls such as back, close, or step progress when functionally required.
- On Splash and Auth screens, the logo may appear inside the page content, never inside a separate navigation header.
- Put each screen title at the start of its scrollable content instead of inside a tall fixed bar.

Create these screens and important variants:

1. Splash / session loading
- Centered temporary Photomatch wordmark on a clean background.
- Small loading indicator and no unnecessary promotional copy.
- Show a network retry variant and keep startup visually calm.

2. Sign in - "Đăng nhập"
- Email and password fields, show/hide password action, "Quên mật khẩu?", and primary "Đăng nhập" CTA.
- Divider with Google and Apple sign-in options using recognizable icons.
- Link to "Tạo tài khoản".
- Include inline validation, wrong-credentials error, loading, and disabled states.

3. Sign up - "Tạo tài khoản"
- Email, password, confirm password, password requirements, consent checkbox with links to Terms and Privacy.
- Primary "Tiếp tục" CTA plus Google and Apple options.
- Link back to sign in.
- Include duplicate-email and weak-password validation states.

4. Verify email - "Xác thực email"
- Confirm that a verification email was sent, display the masked email address, "Mở ứng dụng email", "Gửi lại email", and "Đổi email" actions.
- Include resend countdown, success confirmation, expired-link, and retry states.

5. Forgot password - "Quên mật khẩu"
- Email field, short explanation, "Gửi liên kết đặt lại" CTA, and sent-success state.
- Add a reset-password screen with new password, confirm password, token-expired error, and completion confirmation.

6. Onboarding progress pattern
- A subtle step indicator and back action.
- When onboarding resumes, show completed progress and the next missing step without losing previous data.
- Avoid a heavy wizard look; keep one clear task per screen.

7. Personal information - "Thông tin cá nhân"
- Fields: display name, date of birth, city, and short bio.
- Explain only where needed that minimum profile information is required before appearing in discovery.
- Include field validation and keyboard-safe layout.

8. Avatar - "Thêm ảnh đại diện"
- Large avatar preview, choose from library, take photo, replace, remove, upload progress, failed-upload retry, and optional skip action.
- Crop controls should keep the face visible.

9. Role selection - "Bạn tham gia với vai trò nào?"
- Explain that every account can use Customer mode and may add Photographer during onboarding.
- Two clear role choices: "Khách hàng" and "Thợ ảnh". Photographer is an additional role, not a replacement for Customer.
- Include a confirmation dialog stating that the initially selected additional role cannot be changed after onboarding.

10. Activity fields - "Lĩnh vực hoạt động"
- Searchable multi-select list whose options depend on the selected role.
- Use realistic categories such as Chân dung, Cưới hỏi, Gia đình, Sự kiện, Sản phẩm, Thời trang, Kiến trúc.
- Selected items remain easy to review and remove.

11. Services and pricing - "Dịch vụ của bạn"
- Service list depends on selected activity fields and current role.
- Photographer mode includes offered service, minimum price, maximum price, VND currency, and price unit.
- Customer mode captures wanted services without provider pricing fields.
- Include validation that maximum price cannot be below minimum price.

12. Location permission and discovery presence
- Explain that location is used to find people nearby and that other users only see an intentionally offset public location, never exact GPS coordinates.
- Actions: "Cho phép vị trí", "Để sau", and open system settings after denial.
- Include default search radius 20 km, public-location duration default 24 hours, and a clear visibility switch.

13. Photographer portfolio requirement
- For Photographer onboarding, show an upload grid and progress toward the minimum of 6 real portfolio images required for discovery visibility.
- Each item can have image, title, short description, and related service.
- Include upload progress, reorder, replace, delete confirmation, and failed-upload retry.

14. Onboarding completion
- Summarize role, profile, selected services, visibility, and portfolio readiness.
- Primary CTA "Bắt đầu khám phá".
- If requirements are incomplete, clearly identify what remains and keep discovery visibility disabled.

Use realistic Vietnamese names, cities, services, and prices. Keep legal and privacy explanations readable but brief. Show both Customer and Photographer paths where they materially differ. Do not turn onboarding into decorative cards; forms should remain direct, spacious, and easy to complete.
```

## Prompt 2 - Discovery, nearby, swipe, and match

```text
Continue the existing Photomatch mobile project and reuse its design system. Design the complete discovery experience for both Customer and Photographer roles. All visible UI copy must be Vietnamese.

Global mobile chrome rule for every screen in this batch:
- Do not create a persistent global header with the Photomatch wordmark, search icon, notification bell, divider, or reserved top space.
- Start useful content immediately below the safe area with 8-12px breathing room.
- Search and filters must be compact local controls inside Discovery content. Map controls must overlay the map safely.
- Use only contextual back and overflow controls on detail screens; never replace the removed header with another full-width app bar.

Create these connected screens and states:

1. Discovery / swipe - "Khám phá"
- A large visual profile card showing an authentic portfolio image, name, verified badge when applicable, distance, availability, primary services, and a clear price range such as "1.500.000 - 3.000.000 đ / buổi".
- Add image pagination and a subtle way to open the full profile.
- Stable action controls below the card: skip, view profile, and interested/accept depending on current role.
- Customer behavior: swipe right means interested; it does not create a match immediately.
- Photographer behavior: incoming customer interest can be accepted or rejected. A photographer initiating interest in a new customer is a paid capability, so show it as locked with a small premium label and an explanatory sheet, without designing payment checkout.
- Do not make the interface look like a dating game. Emphasize service fit, trust, and booking intent.

2. Swipe feedback
- Direction-aware visual feedback for "Bỏ qua", "Quan tâm", "Chấp nhận", and "Từ chối" before submission.
- Include submit loading, recoverable failure with retry, and undo only if clearly marked as unavailable for MVP.

3. Discovery filters bottom sheet
- Services multi-select, price range, radius, "Chỉ đang sẵn sàng", and "Chỉ đã xác minh".
- Actions: "Xóa bộ lọc" and "Áp dụng".
- Make selected filters visible as removable chips on the discovery screen.

4. No-candidate states
- Empty result from strict filters with "Điều chỉnh bộ lọc".
- End-of-stack state with a calm message that new profiles may appear later.
- Location unavailable, offline, and server-error states with a direct recovery action.

5. Nearby map - "Quanh đây"
- Full, useful map experience with intentionally approximate profile markers; never imply exact user coordinates.
- Overlay compact local controls for filter, map type, and recenter directly on the map; do not add a separate header band.
- A compact result carousel or bottom sheet connected to selected markers.
- Profile summary must show image, name, distance, service, price, verified, and availability.
- Support Standard, Satellite, and Hybrid map types in a compact selector.
- Include default 20 km radius, location permission request, denied state, expired public-location state, and refresh action.

6. Public profile detail
- Large portfolio imagery with clear subject framing, avatar and identity summary, verified badge, availability, distance, city, bio, services, price ranges, and portfolio grid.
- Show the Photographer's average rating, total published review count, and a paginated preview of recent Customer reviews. Do not show a zero-star score when no review exists; use "Chưa có đánh giá".
- Customer viewing Photographer: primary actions "Quan tâm" and "Đặt lịch". "Đặt lịch" opens the direct booking flow without requiring a prior match.
- Photographer viewing interested Customer: primary actions "Chấp nhận" and "Từ chối".
- Overflow menu includes "Báo cáo" and "Chặn" with confirmation patterns.
- Never show exact GPS coordinates, private email, or private contact details.

7. Incoming interests - "Khách hàng quan tâm"
- A paginated Photographer-only list of unresolved Customer interests with avatar, name, city, wanted service, request time, and compact profile preview.
- Each row supports "Xem hồ sơ", "Chấp nhận", and "Từ chối" with submit loading, already-resolved, stale-profile, empty, pagination, and retry states.
- Accepting creates the match/conversation and opens the same success flow below; rejecting removes the item without pretending it can be edited later.

8. Match success
- A concise success modal or screen after the Photographer accepts.
- Show both avatars, "Hai bạn đã kết nối", and actions "Nhắn tin ngay" and "Tiếp tục khám phá".
- Keep celebration tasteful and brief; no confetti-heavy or romantic dating visual language.

9. Role switch
- A compact, clear role switcher between Customer and Photographer for users who have both roles.
- Explain current context through labels and service-specific actions, not only color.
- Preserve the same navigation structure after switching role.

Use authentic Vietnamese profile data and varied photography categories. Include light mode for all screens and one representative dark-mode version of the swipe screen. Ensure every overlay remains readable over photography with a subtle dark scrim, and never crop away the main subject.
```

## Prompt 3 - Messages and booking

```text
Continue the existing Photomatch mobile project and reuse all existing components and tokens. Design the complete messaging and booking journeys, including Customer direct booking before match, message list, conversation, booking detail, and booking status actions. All visible copy must be Vietnamese.

Global mobile chrome rule for every screen in this batch:
- Do not create a persistent global header with logo, global search, notification bell, divider, or reserved top space.
- Message list content begins directly below the safe area; its title and search belong inside the content flow.
- Conversation detail may use one compact contextual identity row up to 56px tall for back, avatar, participant name/status, and overflow only.
- Booking and form titles belong inside scrollable content, with only minimal back or close controls above them when required.

Create these screens and states:

1. Message list - "Tin nhắn"
- Search field, conversation rows with avatar, name, verified indicator where relevant, last message, timestamp, unread count, and muted/system indicators when needed.
- Show a small "Kết nối mới" area only if it improves scanning; keep the page operational rather than card-heavy.
- Include skeleton loading, no matches yet, no conversations, offline, and pagination loading states.

2. Conversation detail
- Compact contextual identity row with back button, avatar, name, online/availability context, and overflow menu; no global brand header.
- Message bubbles for outgoing and incoming text, image, file, and system events.
- Show sent, delivered, and read states while respecting the user's read-receipt setting.
- Composer supports text, image, file attachment, send, upload progress, failed-upload retry, and disabled state when blocked or conversation is closed.
- System messages announce match creation and booking status changes without looking like user messages.
- Overflow actions: view profile, create/view booking, report, block, and "Kết thúc kết nối". Unmatch requires confirmation, keeps prior messages read-only, disables new messages, and explains the 30-day rematch window.

3. Attachment actions
- Bottom sheet for photo library, camera, and file.
- Image preview and file metadata before sending.
- Show size/type validation errors and a clear retry path.

4. Create booking - "Tạo lịch chụp"
- Customer can open this flow directly from a Photographer public profile without a prior match or conversation. Photographer can initiate it only from an existing active conversation.
- Fields: service, start date/time, end date/time, address, agreed price, VND currency, and optional note.
- Summarize the other participant and selected service at the top without nesting cards.
- Validation: end must be after start, price is required, required fields are explicit, and double-submit is prevented.
- Primary action: "Gửi yêu cầu đặt lịch". A successful Customer submission atomically creates or reuses the match, creates or reuses the conversation, and creates the booking with status "Chờ xác nhận" (`PENDING`).
- After success, show "Yêu cầu đặt lịch đã được gửi" and provide a direct action to open the newly available conversation.
- Include keyboard-safe layout, date/time picker, address entry, submit loading, duplicate-submit prevention, full transaction error, and success confirmation.

5. Booking detail
- Clear status at the top using label plus icon: Nháp, Chờ xác nhận, Đã chấp nhận, Đã từ chối, Đã hủy, Đang diễn ra, Hoàn tất, Tranh chấp.
- Show participants, service, agreed price, schedule, address, note, creator, and status history timeline.
- The participant who did not create a Pending booking sees "Chấp nhận" and "Từ chối".
- Contextual actions by status: the creator may edit permitted fields only while Pending; otherwise show accept, reject, cancel, start, complete, or report dispute according to actor/state. Never offer hard delete.
- For the Customer of a `COMPLETED` booking with no existing review, show the primary action "Đánh giá thợ ảnh". After a review exists, show the submitted review read-only in the booking detail.
- Destructive or irreversible actions require a clear confirmation dialog and reason where appropriate.

6. Booking list
- Accessible from Profile and relevant chat contexts.
- Segmented control or tabs for "Sắp tới", "Đang chờ", and "Lịch sử".
- Rows show counterpart, service, schedule, price, and status.
- Include empty, loading, filter, and network-error states.

7. Booking decision sheets
- Accept confirmation, reject with optional reason, cancel with reason, start shoot, complete shoot, and open dispute.
- Each sheet clearly explains the resulting status and does not use color as the only signal.

8. Blocked and reported conversation states
- When a participant is blocked, preserve prior messages as read-only evidence but disable new messages.
- Show a direct explanation and available actions without exposing internal moderation details.

9. Review Photographer - "Đánh giá thợ ảnh"
- This screen is available only to the Customer of a completed booking.
- Show a compact booking and Photographer context, a required 1-5 star rating input with accessible labels, and an optional comment field.
- Primary action: "Gửi đánh giá". Explain that each booking can be reviewed only once and the review is public under the Customer's display identity.
- Include invalid/missing rating, bounded comment, submit loading, duplicate review, network error, success, and already-reviewed states.
- After success, return to booking detail and update the Photographer rating summary without layout shift.

Use realistic Vietnamese chat messages, names, dates, locations, service names, and VND prices. Keep chat fast and familiar. Keep booking information structured and auditable, with no decorative hero sections. Provide a representative dark-mode version of conversation detail and booking detail.
```

## Prompt 4 - Profile, portfolio, settings, and safety

```text
Continue the existing Photomatch mobile project and reuse its established visual language. Design the self-profile, profile editing, photographer portfolio management, settings, and user-safety flows. All visible UI copy must be Vietnamese.

Global mobile chrome rule for every screen in this batch:
- Do not create a persistent global header with centered logo, global search, notification bell, divider, or reserved top space.
- Self Profile begins with identity content directly below the safe area, with Settings as a compact local action.
- Detail screens use only contextual back/close/overflow controls. Titles begin inside scrollable content rather than inside a fixed bar.
- Public visual profiles may place back and overflow controls over the hero image using readable 44px surfaces.

Create these screens and variants:

1. Self profile - "Hồ sơ"
- Begin the content with avatar, display name, verification state, city, current role, profile visibility, and completion state; place settings as a compact local action without a separate header bar.
- For users with Customer and Photographer roles, include a compact role switcher.
- Customer view emphasizes wanted services and discovery preferences.
- Photographer view emphasizes availability, offered services, price ranges, and portfolio readiness.
- Photographer view also shows average rating and published review count, with access to the public review list.
- Actions: "Chỉnh sửa hồ sơ", "Xem hồ sơ công khai", "Quản lý portfolio", and "Lịch chụp" where applicable.
- Show a clear progress checklist if the profile is not eligible for discovery.

2. Edit profile - "Chỉnh sửa hồ sơ"
- Editable/removable avatar, display name, date of birth, city, bio, activity fields, services, service mode, price range, and photographer availability where applicable.
- Profile visibility switch and public-location expiry controls.
- The initial additional role is visible but cannot be changed after onboarding; explain this as disabled helper text.
- Include unsaved-changes confirmation, inline validation, save loading, success, and network-error states.

3. Portfolio management - "Portfolio"
- 3-column image grid with upload tile, image count, and visible requirement "Tối thiểu 6 ảnh để xuất hiện trong Khám phá".
- Reorder mode, add, edit metadata, replace, and delete with confirmation.
- Portfolio item editor includes image, title, description, and related service.
- Include empty, 1-5 image incomplete, 6+ ready, upload progress, and upload failure states.

4. Settings hub - "Cài đặt"
- Organize as clear list sections, not cards inside cards.
- Account and privacy: profile visibility, public-location duration, read receipts, blocked users.
- Notifications: match and booking notification switches.
- Preferences: language, theme, map type.
- Support: invite friends and about the app.
- Legal: Terms of Service and Privacy Policy.
- Session: sign out as a clear secondary/destructive action.

5. Language
- Options "Tiếng Việt" and "English" with current selection and save behavior.

6. Theme
- Segmented selection for "Theo hệ thống", "Sáng", and "Tối" with small accurate previews.

7. Notification settings
- Independent switches for new matches and booking creation/status changes.
- Include system-permission-disabled state with "Mở cài đặt thiết bị".

8. Privacy settings
- Read-receipt switch.
- Discovery visibility switch.
- Public-location duration selector, default 24 hours.
- Plain-language note that public location is intentionally offset and exact GPS is never shown to other users.

9. Blocked users - "Danh sách chặn"
- Searchable list with avatar, name, block date, and "Bỏ chặn" action.
- Unblock requires confirmation.
- Include empty and loading states.

10. Report flow - "Báo cáo"
- Can be opened from profile, chat, message, or booking.
- Reason choices: Spam, Quấy rối, Hồ sơ giả mạo, Nội dung không phù hợp, Lừa đảo, Khác.
- Description field, optional evidence upload, target-context summary, and submit confirmation.
- Show upload progress, validation, success, and submission error.

11. Block confirmation
- Clearly explain that blocking removes the person from discovery and match interactions and prevents further messages.
- Confirm and cancel actions with restrained danger styling.

12. Penalty / account restricted
- Separate states for warning, temporary suspension with start/end time, permanent ban, and feature restriction.
- Explain the reason, affected features, effective period, and support/help action when available.
- Prevent navigation into unavailable features while keeping sign out and permitted support actions accessible.

13. About and legal
- About screen with app version and essential support links.
- Terms and Privacy are readable document screens with title, effective date, scroll content, and external-link behavior.
- Invite friends uses a native-share style sheet, not a marketing page.

Use realistic data and concise explanations. Show a representative dark-mode version of Profile and Settings. Ensure danger actions are confirmed, disabled controls explain why, and every list or form includes loading, empty, error, and success feedback where relevant.
```

## Prompt 5 - Mobile coverage and state audit

```text
Audit the existing Photomatch mobile designs as a complete production MVP. Do not redesign the visual language. Reuse the same components, tokens, navigation, and Vietnamese terminology, then add any missing screen or state.

Enforce this rule across every audited frame and variant: no logged-in mobile screen may use a persistent global header with centered Photomatch logo, global search icon, notification bell, divider, or reserved blank space. Content starts immediately below the safe area, with only minimal contextual back, close, progress, identity, or overflow controls when functionally necessary. Logo placement is limited to page content on Splash/Auth screens.

Required route coverage:
- Splash and session loading.
- Sign in, sign up, email verification, forgot password, reset password, and account penalty.
- Personal information, avatar, role selection, activity fields, services/pricing, location permission, portfolio requirement, onboarding progress, and completion.
- Discovery swipe, incoming-interest list/decision, filters, nearby map, map type, public profile, role switch, and match success.
- Message list, conversation detail, attachments, unmatch confirmation/read-only history, report/block, booking create/list/detail, and booking status decisions.
- Customer review form after completed booking, already-reviewed state, Photographer rating summary, and public review list.
- Self profile, edit profile, portfolio management, settings, notifications, privacy, language, theme, blocked users, about, invite, Terms, and Privacy.

Required reusable states:
- Initial loading skeleton.
- Pull-to-refresh or refresh feedback where appropriate.
- Empty result with one useful next action.
- Offline and server error with retry.
- Inline form validation and submit failure.
- Button loading and disabled states that prevent duplicate submission.
- Location and notification permission denied.
- Image/file upload progress, failure, retry, and removal.
- Profile incomplete and hidden from discovery.
- No discovery candidates due to filters and end of candidate stack.
- Conversation active, closed, and blocked.
- Message sent, delivered, read, and failed.
- Match active, unmatch confirmation, ended, and 30-day rematch explanation.
- All booking statuses: Draft, Pending, Accepted, Rejected, Cancelled, In progress, Completed, and Disputed.
- Review states: eligible after completed booking, missing/invalid rating, submitting, published, duplicate/already reviewed, empty public list, and hidden/removed exclusion.
- Account warning, temporary suspension, permanent ban, and feature restriction.

Quality checks:
- All copy is natural Vietnamese and terminology is consistent across screens.
- No logged-in mobile screen uses a persistent global header with centered logo, search icon, notification bell, separator, or reserved blank space.
- No exact location or private contact information is exposed on public profiles.
- Photographer discovery eligibility clearly requires at least 6 portfolio images.
- Customer interest does not immediately create a match; Photographer acceptance creates the match and opens chat.
- Sent messages, completed reviews, booking history, reports, and penalties never show generic edit or hard-delete actions; use only their permitted lifecycle actions.
- Customer direct booking must create or reuse exactly one match and conversation, then create a `PENDING` booking atomically. Photographer-created booking still requires an active match/conversation.
- Only the Customer of a `COMPLETED` booking can submit one review for its Photographer; rating is 1-5 and only published reviews appear in public rating aggregates.
- Verification, availability, status, and errors always use label or icon in addition to color.
- Touch targets are at least 44 x 44px and text does not clip at larger system font sizes.
- Main navigation dimensions stay stable across badges, labels, and role changes.
- Photography remains inspectable and is not hidden by dark crops or excessive overlays.
- No landing pages, feature-explanation banners, nested cards, decorative gradients, or dating-game visual language.

Finish with a screen inventory board grouped by Auth, Onboarding, Discovery, Messaging, Booking, Profile, Settings, and Safety so missing coverage is visible at a glance.
```

## Prompt 6 - Web admin MVP

```text
Create a separate responsive web admin project for "Photomatch Admin". This is a quiet, dense, operational dashboard for a single admin role, not a marketing site. Design at 1440px desktop first and provide a usable 1024px responsive layout. All visible UI copy must be Vietnamese. Reuse the Photomatch brand tokens and Inter typography, but optimize the interface for repeated scanning, filtering, investigation, and safe administrative actions.

Visual system:
- Primary raspberry pink #D81B60, pressed/active #C2185B, soft selected backgrounds #FDF2F8 and #FCE7F3; accent #F59E0B, success #16A34A, warning #F97316, danger #DC2626, info #0891B2.
- Use raspberry pink with restraint so the admin remains professional, operational, and easy to scan rather than romantic or decorative.
- Background #F8FAFC, surfaces #FFFFFF, primary text #0F172A, secondary #334155, muted #64748B, borders #E2E8F0.
- Use 8px maximum radius for admin panels, tables, fields, and dialogs.
- Use tabular numbers for metrics, prices, and dates where useful.
- No gradients, oversized hero sections, decorative illustrations, floating page sections, or cards inside cards.
- Keep data tables, filters, pagination, status tags, and row actions stable and aligned.

Global admin shell:
- Fixed sidebar: "Tổng quan", "Người dùng", "Thợ ảnh", "Đánh giá", "Báo cáo", "Hình phạt", "Booking", "Lĩnh vực", "Dịch vụ", "Tài liệu pháp lý".
- Header with breadcrumb, current admin, and sign out.
- Reusable filter bar, data table, status tag, pagination, detail page/drawer, evidence viewer, status history, confirmation dialog, empty state, skeleton, and retry state.
- Destructive actions always require confirmation and a reason where appropriate.

Design these screens:

1. Admin sign in
- Email, password, show/hide password, submit loading, invalid credentials, and session-expired state.

2. Dashboard - "Tổng quan"
- Compact operational metrics: active users, photographers, active matches, pending bookings, open reports, active penalties.
- Small trend summaries and actionable queues for open reports, disputed bookings, and recent penalties.
- Use restrained metric panels and useful tables; do not create a promotional dashboard.

3. Users list and detail
- Search and filters for role, account status, and verification status.
- Columns: user, email, role, account status, verification, city, created date, last activity, actions.
- Detail shows identity, profile, roles, status, verification, discovery visibility, related reports, penalties, bookings, and relevant history.
- Actions: suspend/lock, restore/unlock, with reason and confirmation.

4. Photographers list and detail
- Filters for status, verification, availability, city, activity field, and service.
- Detail shows profile, services and prices, availability, portfolio grid, reports, penalties, and booking summary.
- Admin can inspect but does not pre-approve portfolio in MVP.

5. Reviews list and moderation
- Filters for status, rating, date, Customer, and Photographer.
- Columns: review ID, Customer, Photographer, rating, short comment, status, booking ID, created date, action.
- Detail shows canonical rating/comment as read-only, permitted booking context, and current moderation metadata.
- Status action supports Published, Hidden, and Removed with a required reason and confirmation. Admin must never edit Customer-authored rating/comment.

6. Reports list and review
- Filters for status, reason, date, reporter, and target type.
- Columns: report ID, reporter, target, reason, status, created date, updated date, action.
- Detail shows report description, linked user/message/conversation/booking, attachments, relevant evidence, and timeline.
- Review form supports In review, Resolved, Rejected, admin note, resolution, notify user, and optional creation of a penalty.

7. Penalties list, create, detail, and revoke
- Filters for type, status, user, and effective date.
- Types: Warning, Temporary suspension, Permanent ban, Feature restriction.
- Create form: user, type, reason, starts at, ends at when relevant, affected feature when relevant, notify user.
- Detail includes effective period, status, reason, linked report, creator, and history.
- Revoke requires reason and confirmation.

8. Bookings list and detail
- Filters for status, date range, user, photographer, and service.
- Columns: booking ID, customer, photographer, service, schedule, agreed price, status, updated date.
- Detail shows both participants, service, schedule, address, price, note, linked conversation context, and complete status-history timeline.
- Disputed bookings visually surface evidence without allowing admin to alter the normal booking flow.

9. Activity fields configuration
- Dense table with code, name, description, status, updated date, and actions.
- Create/edit form includes allowed roles and status change confirmation for Active, Inactive, Archived.

10. Services configuration
- Table and filters by activity field and status.
- Create/edit form with activity field, code, name, description, and status.

11. Legal documents
- Table for Terms of Service and Privacy Policy with version, effective date, status, URL, and updated date.
- Create/edit form with document type, version, content URL, effective date, and status. Only Inactive versions are editable; Active versions are read-only and changes require a new version.
- Clearly indicate that a version change may require renewed user consent.

For every list, include loading, no data, no filter results, API error, pagination, and selected-row states. For every form, include labels, helper text where necessary, validation errors, disabled/loading submit, success feedback, and unsaved-changes protection. Use realistic Vietnamese data, dates, IDs, VND prices, and moderation examples. Keep all content readable and never communicate account, report, penalty, or booking state with color alone.
```

## Prompt 7 - Photomatch camera logo design

```text
Design a distinctive, production-ready logo system for "Photomatch", a Vietnamese mobile marketplace that connects customers with professional photographers for discovery, matching, chat, and booking.

Brand idea:
- The core logo symbol must be an instantly recognizable camera viewed from the front. It must still read as a camera without the Photomatch wordmark.
- Build the camera from a simplified geometric body, a clear circular lens, and one subtle top detail such as a shutter button or viewfinder bump.
- Combine the camera with the idea of matching by using two focus corners, two aligned shapes, or a subtle connection detail around the lens. The matching idea must remain secondary to the camera silhouette.
- The result must communicate a trusted creative-services marketplace, not a dating app.
- Keep the camera bold, minimal, and custom rather than using a stock outline icon. It should have a distinctive lens or framing detail that belongs specifically to Photomatch.
- Do not use hearts, dating symbols, handshakes, chain-link icons, realistic camera rendering, literal photo mountains, initials inside a generic circle, or stock-logo shapes.

Personality:
- Modern, confident, visual, human, precise, and premium enough for professional photographers.
- Friendly without becoming cute, romantic, sugary, or feminine-coded.
- Simple enough to remain recognizable at small app-icon and favicon sizes.

Color system:
- Primary raspberry pink: #D81B60.
- Dark/pressed pink: #C2185B.
- Soft pink backgrounds: #FDF2F8 and #FCE7F3.
- Supporting dark neutral: #0F172A.
- White: #FFFFFF.
- Use flat solid color only. No gradients, shadows, bevels, glow, transparency effects, or 3D mockup styling in the actual logo artwork.

Wordmark:
- Use the exact name "Photomatch" with a capital P and lowercase remaining letters.
- Create a clean custom geometric sans-serif wordmark that feels compatible with Inter but has one memorable detail related to framing, focus, or alignment.
- Prioritize legibility. Do not distort letters, tighten tracking excessively, or turn the two "o" characters into obvious camera lenses.
- The symbol and wordmark must feel balanced in a horizontal lockup and must also work independently.

Required logo outputs on a clean brand board:
1. Primary horizontal lockup: camera symbol plus "Photomatch" on a light background.
2. Reversed horizontal lockup in white on #D81B60 and on #0F172A.
3. Symbol-only app icon for iOS and Android, tested in square and rounded-square masks.
4. Wordmark-only version.
5. One-color black, one-color white, and one-color raspberry pink versions.
6. Small-size tests at 16px, 24px, 32px, and 48px showing that the symbol remains identifiable.
7. Clear-space and minimum-size guidance.
8. A compact usage example on Splash and Sign in, but do not place the logo in logged-in mobile headers.

App icon constraints:
- Use the bold front-facing camera symbol as the single centered app-icon graphic with generous optical padding.
- The camera body and lens must remain recognizable at first glance, even at 24px.
- Avoid tiny internal gaps that disappear at 24px.
- Let the camera occupy approximately 60-68% of the safe icon area so it does not feel too small or touch the mask edges.
- It must be distinguishable from dating, social media, and generic camera apps through its custom focus/matching detail.
- Do not place the full wordmark inside the app icon.

Present one resolved logo direction, not a page of loosely related sketches. Keep the brand board flat, precise, and suitable for handoff to a vector designer. All explanatory labels on the board should be in Vietnamese.
```

## Checklist đầu ra mong đợi

- Mobile và admin là hai project Stitch riêng nhưng dùng chung brand tokens.
- Mobile có đầy đủ hai ngữ cảnh `CUSTOMER` và `PHOTOGRAPHER`.
- Nội dung hiển thị là tiếng Việt; prompt mô tả bằng tiếng Anh.
- Mọi flow quan trọng đều có happy path, empty, loading, error, disabled và permission state phù hợp.
- Luồng nghiệp vụ hỗ trợ cả: quan tâm -> photographer chấp nhận -> match -> chat -> booking; và Customer đặt lịch trực tiếp -> match + conversation + booking `PENDING`.
- Vị trí công khai luôn được mô tả là vị trí làm lệch, không phải GPS thật.
- Photographer cần tối thiểu 6 ảnh portfolio để đủ điều kiện xuất hiện trong discovery.
- Customer chỉ review Photographer sau booking `COMPLETED`; mỗi booking tối đa một review và rating từ 1 đến 5.
- Admin tập trung vào dashboard, users, photographers, reviews, reports, penalties, bookings và system configuration.
- Customer review đã gửi là read-only; Admin chỉ publish/hide/remove kèm lý do và không sửa rating/comment.
- Mobile không có global header lặp lại logo/search/notification; chỉ dùng contextual top controls khi cần.
- Logo có đủ horizontal lockup, wordmark, symbol, app icon, monochrome và small-size tests.
