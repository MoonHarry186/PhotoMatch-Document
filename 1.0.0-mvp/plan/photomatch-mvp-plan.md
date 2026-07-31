**Photomatch MVP plan**

# VERSION MANAGEMENT

## Current version

| Thuộc tính | Giá trị |
| :---- | :---- |
| Product document version | `1.0.0` |
| Release scope | MVP |
| Document folder | `documents/1.0.0-mvp/` |
| API contract version | `/api/v1` |
| Status | Draft cho tới khi được review/approve; sau approve thì xem như baseline. |

Version `1.0.0` là baseline đầu tiên cho MVP. Tất cả thay đổi sau khi baseline cần được ghi nhận để tránh việc scope, ERD, API contract, mobile/web/admin và chi phí vận hành bị lệch nhau.

## Versioning rules

| Loại thay đổi | Cách tăng version | Ví dụ |
| :---- | :---- | :---- |
| Patch | `1.0.0` → `1.0.1` | Sửa typo, làm rõ câu chữ, thêm ghi chú không đổi requirement, API, database hoặc luồng nghiệp vụ. |
| Minor | `1.0.0` → `1.1.0` | Thêm feature nhỏ tương thích với MVP, thêm endpoint không breaking, thêm field optional, mở rộng admin config. |
| Major | `1.0.0` → `2.0.0` | Đổi luồng nghiệp vụ chính, đổi role model, đổi database lớn, breaking API, thay đổi scope sản phẩm sau MVP. |

## Change process after baseline

```text
Change request
→ Impact analysis
→ Update affected docs
→ Update API/ERD/NFR/test scope nếu liên quan
→ Review
→ Approve
→ Re-baseline version mới
```

## Change record

| Version | Ngày | Loại thay đổi | Nội dung | Trạng thái |
| :---- | :---- | :---- | :---- | :---- |
| `1.0.0` | TBD | Initial baseline | MVP scope đầu tiên cho Photomatch. | Draft |
| `1.0.0` | `2026-07-20` | Documentation update | Bổ sung section `DESIGNS` cho màu sắc, font, token và nguyên tắc giao diện. | Draft |
| `1.0.0` | `2026-07-20` | Design prompt | Bổ sung bộ prompt Google Stitch cho toàn bộ màn hình mobile và web admin trong MVP. | Draft |
| `1.0.0` | `2026-07-20` | Design update | Đổi màu chủ đạo từ xanh sang hồng raspberry trong design tokens và prompt Google Stitch. | Draft |
| `1.0.0` | `2026-07-20` | Mobile UI update | Loại bỏ global header trên mobile, chuyển sang contextual top controls và bổ sung prompt thiết kế logo. | Draft |
| `1.0.0` | `2026-07-20` | Booking flow update | Cho Customer đặt lịch trực tiếp; hệ thống tạo/reuse match, conversation và booking `PENDING` trong một transaction. | Draft |
| `1.0.0` | `2026-07-20` | MVP scope update | Chọn Cloudflare R2 để lưu ảnh/file và bổ sung Customer review Photographer sau booking hoàn tất. | Draft |
| `1.0.0` | `2026-07-23` | Brand alignment | Đồng bộ section `DESIGNS` với PhotoMatch Brand Identity Guidelines: Royal Blue/Vivid Purple, Plus Jakarta Sans, logo pin–shutter, layout, component, motion và accessibility. | Draft |
| `1.0.0` | `2026-07-30` | `CR-002` scope reduction | Loại Nearby map, marker, map type và Google Maps khỏi MVP; giữ Nearby list, location, presence và server-side filters. | Approved for draft incorporation |
| `1.0.0` | `2026-07-30` | `CR-003` navigation consolidation | Loại tab/màn Nearby độc lập; tích hợp `Gần tôi` và bán kính vào Discovery filter. | Approved for draft incorporation |

## Quy tắc quản lý thay đổi

- Trước khi `1.0.0` được approve, có thể sửa trực tiếp tài liệu nhưng vẫn nên cập nhật `Change record`.
- Sau khi `1.0.0` được approve, thay đổi requirement/API/ERD/NFR phải có change request.
- Nếu thay đổi chỉ sửa diễn đạt, typo hoặc format, tạo patch version như `1.0.1`.
- Nếu thay đổi thêm khả năng tương thích ngược, tạo minor version như `1.1.0`.
- Nếu thay đổi breaking hoặc đổi scope lớn, tạo major version như `2.0.0`.
- Khi tạo version mới sau MVP, tạo thư mục version không mang hậu tố MVP, ví dụ `documents/1.1.0/`, rồi cập nhật tài liệu trong thư mục mới. Chỉ baseline `1.0.0` dùng thư mục `documents/1.0.0-mvp/` để thể hiện đây là phạm vi MVP.
- API version `/api/v1` chỉ tăng khi có breaking change ở contract; document version có thể tăng độc lập với API version.

# FEATURES

## Mục tiêu MVP

Photomatch MVP ưu tiên giải quyết hai bài toán chính:

- Giúp customer tìm thợ ảnh nhanh theo vị trí, dịch vụ, giá và trạng thái sẵn sàng.
- Giúp customer kết nối với photographer qua luồng quan tâm hoặc đặt lịch trực tiếp, sau đó chat và quản lý booking trong app.

Luồng thành công chính:

```text
Đăng ký
→ Xác thực email
→ Chọn vai trò
→ Hoàn tất hồ sơ tối thiểu
→ Tìm photographer phù hợp
  ├─ Quan tâm → Photographer chấp nhận → Match → Chat → Booking
  └─ Customer đặt lịch trực tiếp → Match + Conversation + Booking PENDING
→ Photographer hoàn tất booking → Booking COMPLETED
→ Customer review Photographer
```

## Nguyên tắc MVP

- User chính gồm cả customer và photographer.
- Tất cả user mặc định có vai trò `CUSTOMER`.
- User có thể chọn thêm một vai trò ban đầu khi onboarding, ví dụ `PHOTOGRAPHER`.
- Sau onboarding, user không được thay đổi vai trò ban đầu đã chọn.
- User có thêm vai trò khác `CUSTOMER` được phép switch giữa `CUSTOMER` và vai trò đã chọn.
- Hồ sơ không đáp ứng điều kiện tối thiểu thì không được hiển thị trong Discovery.
- Admin MVP không duyệt trước nội dung, hồ sơ hoặc portfolio.
- Admin MVP vẫn xử lý report, penalty, user, photographer, booking và các cấu hình hệ thống.

## Auth & Onboarding

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Đăng ký | Guest | Tạo tài khoản mới bằng email/password, Google hoặc Apple. | Email/password bắt buộc xác thực email trước khi dùng app. |
| Đăng nhập | User | Đăng nhập bằng email/password, Google hoặc Apple. | Tài khoản bị phạt/khóa sẽ chuyển tới màn hình phạt. |
| Đăng xuất | User | Kết thúc session hiện tại. | Thu hồi session/token hiện tại. |
| Quên mật khẩu | User | Gửi email đặt lại mật khẩu. | Token reset chỉ dùng một lần và có thời hạn. |
| Xác thực email | User | Xác minh email sau đăng ký. | Chưa xác thực email thì không hoàn tất luồng sử dụng chính. |
| Đổi email chờ xác thực | Unverified user | Sửa email nhập sai trước khi tài khoản được xác thực. | Kiểm tra unique, vô hiệu token cũ và gửi verification cho email mới; không dùng endpoint này sau khi verified. |
| Chọn vai trò | User | Chọn vai trò bổ sung ngoài customer trong onboarding. | Không được đổi vai trò ban đầu sau khi onboarding hoàn tất. |
| Đồng ý tài liệu pháp lý | User | Ghi nhận Terms/Privacy version mà user đã chấp nhận. | Chỉ consent version đang active/effective; consent là append-only và không sửa/xóa. |
| Tiếp tục onboarding | User | Nếu bỏ dở onboarding, lần sau quay lại trang chủ và thấy tiến độ đã hoàn tất x/phần. | App phải nhắc phần còn thiếu; hồ sơ chưa đủ chuẩn thì không hiển thị. |

## Profile & Role

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Customer profile | Customer | Hồ sơ thể hiện nhu cầu dịch vụ, khoảng cách và danh tính cơ bản. | Chỉ hiển thị khi đủ thông tin tối thiểu. |
| Photographer profile | Photographer | Hồ sơ thợ ảnh gồm danh tính rõ ràng, portfolio thực tế và giá. | Bắt buộc có ít nhất 6 ảnh portfolio để được hiển thị. |
| Xem hồ sơ | User | Xem đầy đủ hồ sơ của chính mình hoặc hồ sơ public theo role. | Self DTO có dữ liệu chỉnh sửa; public DTO loại email, GPS chính xác và dữ liệu riêng tư. |
| Portfolio | Photographer | Quản lý ảnh portfolio. | Portfolio phải là ảnh thực tế; admin không duyệt trước trong MVP. |
| Giá dịch vụ | Photographer | Khai báo khoảng giá theo dịch vụ. | Giá dùng cho discovery filter và booking. |
| Trạng thái sẵn sàng | Photographer | Bật/tắt trạng thái đang sẵn sàng nhận việc. | Có thể dùng để lọc `available_only`. |
| Ẩn/hiện hồ sơ | User | User quyết định hồ sơ có xuất hiện trong Discovery và kết quả `Gần tôi` không. | Chỉ người bật hiển thị mới xuất hiện. |
| Switch role | User có nhiều role | Chuyển giữa `CUSTOMER` và vai trò đã chọn khi onboarding. | Không cho thêm/sửa vai trò sau onboarding trong MVP. |

## Bộ lọc Gần tôi trong Discovery

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Gần tôi | Customer | Lọc feed Discovery theo vị trí và khoảng cách gần đúng. | Opt-in; chỉ hỏi GPS khi user bật và chỉ hiển thị người có presence phù hợp. |
| Khoảng cách gần đúng | System | Backend dùng vị trí công khai đã làm lệch để xếp hạng và tạo distance bucket. | Mobile không nhận GPS thật hoặc tọa độ công khai của user khác. |
| Quản lý vị trí chính xác | User | Cập nhật hoặc xóa vị trí chính xác của chính mình. | Chỉ owner/system truy cập; xóa exact location đồng thời tắt public presence. |
| Bán kính mặc định | Customer | Giới hạn kết quả khi bật `Gần tôi`. | Mặc định 20 km; không áp dụng khi `Gần tôi` tắt. |
| Thời hạn hiển thị vị trí | User | Vị trí hiển thị có thời hạn. | Mặc định 24h và user có thể điều chỉnh. |
| Filter quanh đây | User | Lọc theo dịch vụ, giá, trạng thái available và xác minh. | Filter có thể lưu local trên thiết bị trong MVP. |

## Quẹt & Match

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Customer quẹt photographer | Customer | Customer quẹt phải để thể hiện quan tâm photographer. | Không tạo match ngay. |
| Danh sách quan tâm đến | Photographer | Photographer xem các Customer đã thể hiện quan tâm để ra quyết định. | Chỉ trả interest chưa xử lý thuộc đúng Photographer, có phân trang. |
| Photographer chấp nhận | Photographer | Photographer chấp nhận customer đã quan tâm. | Chỉ khi photographer accept thì mới tạo match. |
| Photographer quẹt customer | Photographer | Photographer chủ động quẹt customer. | Đây là tính năng trả phí; cơ chế thanh toán/entitlement cần thiết kế riêng. |
| Quẹt trái | User | Bỏ qua người đang xem. | Người bị bỏ qua có thể xuất hiện lại sau 7 ngày. |
| Match lại | User | Một cặp user có thể match lại sau khi unmatched. | Chỉ cho phép sau 30 ngày. |
| Unmatch | Matched user | Một participant chủ động kết thúc kết nối. | Dùng lifecycle action, đóng conversation cho tin nhắn mới và bắt đầu cửa sổ rematch 30 ngày; không xóa lịch sử. |
| Mở chat sau match | System | Tạo conversation sau khi match thành công. | Chat chỉ mở khi có match. |

## Chat

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Chat 1-1 | Matched users | Hai bên chat sau match. | Không cho nhắn trước match. |
| Text message | Matched users | Gửi tin nhắn văn bản. | Bắt buộc MVP. |
| Image message | Matched users | Gửi ảnh trong chat. | Cần giới hạn dung lượng và định dạng. |
| File message | Matched users | Gửi file trong chat. | Cần giới hạn dung lượng và loại file an toàn. |
| Read receipt | Matched users | Hiển thị trạng thái đã đọc. | User có config quyền riêng tư để bật/tắt read receipt. |
| Block trong chat | User | Chặn người dùng. | Bên bị block không được tiếp tục nhắn tin. |
| Report từ chat | User | Report vấn đề phát sinh trong hội thoại. | Cần lưu reason code và liên kết message/conversation nếu có. |
| Tính bất biến của tin nhắn | System | Giữ nguyên nội dung đã gửi để hỗ trợ retry, block và tranh chấp. | MVP không cho sửa hoặc hard-delete message; chỉ có thể ẩn theo policy moderation sau này nhưng vẫn giữ bằng chứng được phép. |

## Booking

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Customer đặt lịch trực tiếp | Customer | Customer tạo yêu cầu đặt lịch từ hồ sơ Photographer mà không cần match/chat trước. | Hệ thống tạo hoặc tái sử dụng active match, tạo hoặc tái sử dụng conversation, rồi tạo booking `PENDING` trong cùng một transaction. |
| Tạo booking từ conversation | Customer hoặc Photographer | Một participant có thể tạo booking trong conversation đang hoạt động. | Dùng active match/conversation hiện có; booking gửi đi có trạng thái `PENDING`. Photographer không được tạo booking trực tiếp khi chưa có match. |
| Khởi tạo quan hệ khi đặt lịch | System | Direct booking đồng thời mở quan hệ và kênh trao đổi cho hai bên. | Nếu chưa có active match/conversation thì tạo đúng một match và một conversation; nếu đã có thì tái sử dụng, không tạo trùng. Nếu bất kỳ bước nào lỗi thì rollback toàn bộ. |
| Xác nhận booking | Bên còn lại | Người không tạo booking xác nhận hoặc từ chối. | Booking mới gửi đi có trạng thái `PENDING`. |
| Giá thỏa thuận | Customer/Photographer | Lưu giá đã thống nhất. | Bắt buộc có trong booking. |
| Thời gian và địa điểm | Customer/Photographer | Lưu lịch chụp và địa điểm. | `scheduled_end` phải sau `scheduled_start`. |
| Trạng thái booking | System/User | Quản lý vòng đời booking. | `DRAFT`, `PENDING`, `ACCEPTED`, `REJECTED`, `CANCELLED`, `IN_PROGRESS`, `COMPLETED`, `DISPUTED`. |
| Lịch sử trạng thái | System | Lưu mọi lần đổi trạng thái. | Bắt buộc để phục vụ audit/tranh chấp. |
| Cập nhật booking | Booking creator | Sửa các điều khoản khi booking còn `PENDING`. | Chỉ creator được sửa trường cho phép trước khi accept; booking đã accept phải cancel và tạo yêu cầu mới nếu cần đổi điều khoản cốt lõi. Không hard-delete booking. |
| Bằng chứng giao dịch | System | Booking, chat và status history có thể làm dữ liệu bằng chứng khi có scam/tranh chấp. | Nên bổ sung snapshot/consent nếu muốn tăng giá trị pháp lý. |

## Review Photographer

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Tạo review | Customer | Customer đánh giá Photographer sau buổi chụp. | Chỉ Customer của booking `COMPLETED` được review Photographer thuộc booking đó. |
| Rating | Customer | Chấm điểm chất lượng dịch vụ. | Bắt buộc là số nguyên từ 1 đến 5. |
| Nội dung review | Customer | Viết nhận xét về Photographer. | Nội dung là tùy chọn, review không ẩn danh. |
| Một review mỗi booking | System | Ngăn spam hoặc đánh giá trùng cho cùng giao dịch. | `booking_id` phải unique trong `REVIEWS`; request retry phải idempotent. |
| Tính bất biến của review | System | Giữ đánh giá gắn với giao dịch đã hoàn tất. | Customer được đọc lại review theo booking nhưng không sửa hoặc xóa trong MVP; Admin chỉ đổi trạng thái moderation và phải lưu lý do. |
| Hiển thị review | User | Xem rating tổng hợp và danh sách review trên hồ sơ Photographer. | Chỉ review `PUBLISHED` được tính vào rating và hiển thị công khai. |
| Moderation review | Admin/System | Ẩn hoặc gỡ review vi phạm khi xử lý report. | Không duyệt trước review trong MVP; dùng `HIDDEN` hoặc `REMOVED` khi có quyết định xử lý. |

## Report, Block & Penalty

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Report | User | Báo cáo hành vi hoặc nội dung vi phạm. | Reason code: `SPAM`, `HARASSMENT`, `FAKE_PROFILE`, `INAPPROPRIATE_CONTENT`, `SCAM`, `OTHER`. |
| Block | User | Chặn user khác. | Block chặn discovery, match interaction và chat. |
| Xử lý report | Admin | Admin xem xét report và thông báo kết quả. | Admin có thể đưa ra penalty. |
| Penalty screen | Penalized user | App hiển thị màn hình phạt khi vi phạm chính sách. | Tài khoản bị giới hạn theo penalty đang hiệu lực. |
| Penalty có thời hạn | Admin | Tạo hình phạt có thời gian bắt đầu/kết thúc. | Không chỉ khóa vĩnh viễn; có temporary suspension. |

## Notification

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Match notification | User | Push notification khi có match mới từ luồng quan tâm. | Bắt buộc MVP; nếu match được tạo bởi direct booking thì không gửi thêm push match riêng. |
| Booking notification | User | Push notification khi booking được tạo hoặc đổi trạng thái. | Bắt buộc MVP; direct booking gửi một booking-focused push tới Photographer. |
| Notification settings | User | Tắt/bật từng loại notification. | Có config riêng cho từng loại. |
| Deep link | User | Bấm notification mở đúng màn hình liên quan. | Match notification mở match/chat; booking notification mở booking detail. |
| Notification inbox | User | Danh sách thông báo trong app. | Không cần trong MVP. |

## Admin

| Feature | Actor | Mô tả | Rule MVP |
| :---- | :---- | :---- | :---- |
| Dashboard | Admin | Xem thống kê vận hành cơ bản. | Bắt buộc MVP. |
| Quản lý config | Admin | Quản lý cấu hình hệ thống như lĩnh vực, dịch vụ, tài liệu pháp lý. | Không cần phân quyền nhiều cấp. |
| Quản lý users | Admin | Xem, khóa, mở khóa tài khoản. | Admin có quyền khóa/mở khóa. |
| Quản lý photographers | Admin | Xem hồ sơ photographer và portfolio. | Không duyệt trước trong MVP. |
| Quản lý reports | Admin | Xem và xử lý report. | Có thể thông báo kết quả và tạo penalty. |
| Quản lý reviews | Admin | Xem review và thay đổi trạng thái moderation. | Chỉ chuyển `PUBLISHED`, `HIDDEN`, `REMOVED` kèm lý do; không sửa rating/comment của Customer. |
| Quản lý penalties | Admin | Tạo, xem, thu hồi penalty. | Penalty có thời hạn. |
| Quản lý bookings | Admin | Xem booking để hỗ trợ xử lý tranh chấp. | Không cần can thiệp luồng booking mặc định. |
| Audit log admin | Admin/System | Ghi log thao tác admin. | Không cần trong MVP. |
| Phân quyền admin nhiều cấp | Admin | Nhiều nhóm quyền admin. | Không cần trong MVP. |

# SCREENS

## Splash (Loading)

## Màn hình phạt (Khoá tài khoản)

## Đăng nhập

## Đăng ký

## Thông tin cá nhân (Sau đăng ký)

## Ảnh đại diện (Sau đăng ký)

## Chọn vai trò

## Lĩnh vực hoạt động (Danh sách này phụ thuộc vào vai trò)

## Dịch vụ \[X\] (Phụ thuộc vào lĩnh vực đã chọn)

## Thông báo

## Quên mật khẩu

## Quanh đây

## Quẹt

## Chat

## Chi tiết chat

## Profile

## Edit Profile

## Portfolio (danh sách ảnh, cho thợ ảnh)

## Đánh giá thợ ảnh (sau booking completed)

## Danh sách đánh giá (trong hồ sơ thợ ảnh)

## Settings

- ### Về ứng dụng

- ### Ngôn ngữ

- ### Giao diện

- ### Giới thiệu bạn bè

- ### Danh sách chặn

- ### Điều khoản sử dụng

- ### Chính sách bảo mật

# DESIGNS

Nguồn sự thật cho nhận diện và product design là [PhotoMatch Brand Identity & Product Design Guidelines](../../brand/photomatch-brand-identity-guidelines.md). Khi có khác biệt về logo, màu sắc, typography, hình ảnh hoặc giọng điệu, brand guideline được ưu tiên hơn prompt/mockup cũ.

## Design direction

PhotoMatch là marketplace nhiếp ảnh gia theo vị trí, được định vị là cách nhanh, đơn giản và đáng tin cậy để tìm đúng nhiếp ảnh gia. Trải nghiệm phải hiện đại nhưng mang tính con người, ưu tiên portfolio thật, thông tin minh bạch và quyết định nhanh mà không biến sản phẩm thành giao diện dating hoặc studio cao cấp.

| Nguyên tắc | Mô tả |
| :---- | :---- |
| Simple first | Mỗi màn hình có một hành động chính rõ ràng, CTA dùng động từ trực tiếp và tránh thuật ngữ kỹ thuật. |
| Trust by design | Xác minh, rating, số lượt đặt, vị trí, giá, booking, report và block phải minh bạch, không dùng màu gây hiểu nhầm. |
| Photography first | Portfolio và khoảnh khắc thật là nội dung chính; UI hỗ trợ chứ không cạnh tranh với hình ảnh. |
| Mobile-first | Ưu tiên thao tác một tay, nội dung dễ quét, vùng chạm lớn và phản hồi tức thời trên iOS/Android. |
| Fast decision-making | Card Discovery hiển thị nhanh phong cách, rating, địa điểm, giá, chuyên môn, lịch trống và trạng thái xác minh; thêm distance bucket khi bật `Gần tôi`. |
| Human & friendly | Copy ngắn gọn, tự nhiên, khuyến khích hành động, không phán xét hoặc quá corporate. |
| Professional & consistent | Bố cục sạch, ít chi tiết thừa và dùng chung semantic token giữa mobile với Web Admin. |

PhotoMatch không được trở thành giao diện quá xa xỉ, quá nghệ thuật đến khó dùng, quá trẻ con, quá giống studio chụp ảnh hoặc quá giống Tinder về hình ảnh và ngôn ngữ.

## Brand voice and CTA

- Copy phải ngắn gọn, rõ ràng, tự nhiên và hướng tới hành động.
- Dùng các CTA cụ thể như `Tìm nhiếp ảnh gia`, `Xem hồ sơ`, `Gửi tin nhắn`, `Đặt lịch`, `Xác nhận`, `Tiếp tục`.
- Không dùng CTA chung chung như `OK`, `Đồng ý`, `Thực hiện`, `Xử lý` nếu có thể gọi đúng hành động.
- Không hiển thị thuật ngữ kỹ thuật như `matching`, `submit`, `khởi tạo provider` cho user.
- Thông báo lỗi phải nói rõ vấn đề và cách khắc phục, không đổ lỗi hoặc phán xét user.

## Logo system

Logo PhotoMatch kết hợp **location pin** và **camera shutter**. Không tự vẽ lại, kéo giãn, xoay, đổi cấu trúc shutter, thêm shadow/3D mạnh hoặc đổi sang màu ngoài hệ thống.

| Phiên bản | Dùng cho | Quy tắc |
| :---- | :---- | :---- |
| Primary/horizontal | Auth, email, tài liệu, Web Admin | Icon bên trái wordmark; `Photo` dùng Deep Navy, `Match` dùng Royal Blue hoặc gradient xanh–tím. |
| Stacked | Hero, poster, cover khi đủ không gian | Icon trên wordmark; chỉ dùng tagline khi kích thước đủ đọc. |
| Icon only | App icon, splash, favicon, avatar, badge nhỏ | Chỉ dùng pin + shutter; không có chữ hoặc tagline. |
| Monochrome | Nền tối, in đơn sắc | Deep Navy trên nền sáng, trắng trên nền tối hoặc đen khi in đơn sắc. |

- Khoảng an toàn tối thiểu quanh logo bằng một cánh shutter hoặc `25%` chiều cao biểu tượng.
- Logo có wordmark trên màn hình rộng tối thiểu `120px`; icon-only tối thiểu `24px`.
- App icon nguồn dùng canvas `1024x1024`, nền gradient Royal Blue → Vivid Purple, biểu tượng trắng, không chữ, không tagline, không viền trắng.
- Logo chỉ xuất hiện có chủ đích ở splash/auth hoặc điểm chạm thương hiệu; không lặp lại trên mọi màn hình đã đăng nhập.

## Brand palette

| Token | Hex | Dùng cho | Ghi chú |
| :---- | :---- | :---- | :---- |
| `primary.500` / Royal Blue | `#2563EB` | CTA chính, link, focus, active state | Button chính ưu tiên màu đặc để ổn định và dễ đọc. |
| `secondary.500` / Vivid Purple | `#7C3AED` | Gradient, creative accent, selected highlight | Không cạnh tranh với Orange trên cùng một màn hình. |
| `accent.500` / Vibrant Orange | `#F97316` | Badge, điểm nhấn có kiểm soát | Không thay thế màu warning/error theo ngữ nghĩa. |
| `neutral.950` / Deep Navy | `#0F172A` | Heading, text chính, nền tối | Màu chữ chính của thương hiệu. |
| `neutral.500` / Slate Gray | `#64748B` | Text phụ, metadata | Kiểm tra contrast khi dùng ở cỡ nhỏ. |
| `neutral.200` / Light Border | `#E2E8F0` | Border, divider, disabled background | Border mặc định trên nền sáng. |
| `neutral.50` / Soft White | `#F8FAFC` | App background light | Chiếm phần lớn diện tích giao diện. |
| `white` / Pure White | `#FFFFFF` | Card, modal, sheet, input | Surface chính. |
| `success.500` | `#16A34A` | Verified, available, accepted, completed | Chỉ dùng cho trạng thái thực sự tích cực. |
| `warning.500` | `#F59E0B` | Pending, cần chú ý, sắp hết hạn | Luôn đi kèm text hoặc icon. |
| `danger.500` | `#DC2626` | Error, report, block, rejected, penalty | Dùng cho lỗi hoặc hành động rủi ro. |
| `info.500` | `#2563EB` | System information | Dùng Royal Blue theo ngữ nghĩa info. |
| `disabled.text` | `#94A3B8` | Text/icon disabled | Không dùng cho nội dung cần đọc chính. |

Tỷ lệ màu định hướng: `60%` nền trắng/neutral, `25%` Royal Blue, `10%` Deep Navy và tối đa `5%` Purple hoặc Orange accent.

## Brand gradient

```css
linear-gradient(135deg, #2563EB 0%, #7C3AED 100%)
```

- Được dùng cho app icon, splash, hero, selected card, promotional banner hoặc brand illustration.
- Không dùng gradient cho mọi button, toàn bộ bottom navigation hoặc card thông thường.
- Purple và Orange không được cùng trở thành điểm nhấn chính trên một màn hình.

## Dark palette

| Token | Hex | Dùng cho |
| :---- | :---- | :---- |
| `dark.background` | `#020617` | Nền chính dark mode. |
| `dark.surface` | `#0F172A` | Card, bottom sheet, input. |
| `dark.surfaceMuted` | `#1E293B` | Surface phụ, selected chip. |
| `dark.border` | `#334155` | Divider và border. |
| `dark.textPrimary` | `#F8FAFC` | Text chính. |
| `dark.textSecondary` | `#CBD5E1` | Text phụ. |
| `dark.textMuted` | `#94A3B8` | Metadata, helper text. |

## Semantic colors

| Semantic token | Light | Dark | Dùng cho |
| :---- | :---- | :---- | :---- |
| `background` | `neutral.50` | `dark.background` | Nền app/web. |
| `surface` | `white` | `dark.surface` | Card, panel, sheet. |
| `surfaceMuted` | `neutral.100` | `dark.surfaceMuted` | Chip, empty state, skeleton. |
| `border` | `neutral.200` | `dark.border` | Border/divider. |
| `textPrimary` | `neutral.950` | `dark.textPrimary` | Heading/body chính. |
| `textSecondary` | `neutral.500` | `dark.textSecondary` | Nội dung phụ. |
| `textMuted` | `neutral.500` | `dark.textMuted` | Placeholder/helper/metadata. |
| `ctaPrimary` | `primary.500` | `primary.500` | CTA chính. |
| `ctaSecondary` | `secondary.500` | `secondary.500` | Accent/selected state có kiểm soát. |
| `ctaDanger` | `danger.500` | `danger.500` | Hành động nguy hiểm. |
| `focusRing` | `primary.500` | `primary.500` | Focus rõ trên keyboard/screen reader flow. |

## Role and status colors

| Trường hợp | Token | Ghi chú UI |
| :---- | :---- | :---- |
| Customer | `primary.500` | Dùng cho badge role hoặc icon nhỏ. |
| Photographer | `secondary.500` | Dùng cho badge role/selected accent, không thay thế CTA chính. |
| Verified | `success.500` | Luôn đi cùng icon/tên `Verified`, không chỉ dựa vào màu. |
| Available | `success.500` | Dot/status text `Sẵn sàng`. |
| Busy | `warning.500` | Dot/status text `Đang bận`. |
| Unavailable | `neutral.500` | Không dùng màu đỏ nếu user chỉ tắt nhận việc. |
| Match active | `primary.500` | Dùng cho trạng thái match/chat mở. |
| Booking pending | `warning.500` | Chờ xác nhận. |
| Booking accepted/completed | `success.500` | Đã xác nhận/hoàn tất. |
| Booking rejected/cancelled/disputed | `danger.500` | Trạng thái tiêu cực hoặc cần xử lý. |
| Report/penalty | `danger.500` | Hành động nhạy cảm phải có confirm. |

## Typography and fonts

| Nền tảng | Font chính | Font fallback | Ghi chú |
| :---- | :---- | :---- | :---- |
| Mobile iOS/Android | `Plus Jakarta Sans` | `Inter`, system font | Bundle và preload trước màn hình chính. |
| Web Admin | `Plus Jakarta Sans` | `Inter`, `system-ui`, `Segoe UI`, `sans-serif` | Cấu hình thống nhất trong theme. |
| Numeric/price | `Plus Jakarta Sans` tabular numbers | System fallback | Giá, rating, thời gian và số liệu phải dễ quét. |

Weight được phép: `300`, `400`, `500`, `600`, `700`. Không dùng quá ba cấp độ chữ trên một card, không dùng chữ in hoa cho đoạn dài và tránh font quá mảnh trên ảnh.

## Text styles

| Token | Size | Line height | Weight | Dùng cho |
| :---- | :---- | :---- | :---- | :---- |
| `display` | 32 | 40 | 700 | Splash/onboarding headline quan trọng. |
| `h1` | 28 | 36 | 700 | Title màn hình chính. |
| `h2` | 24 | 32 | 600 | Section lớn, modal title. |
| `h3` | 20 | 28 | 600 | Card title, form section. |
| `body-lg` | 16 | 24 | 400 | Nội dung chính, onboarding copy. |
| `body` | 14 | 22 | 400 | Body phổ biến. |
| `label` | 13 | 18 | 600 | Label input, chip, badge. |
| `caption` | 12 | 16 | 400 | Timestamp, metadata. |
| `button` | 14 | 20 | 600 | CTA chính/phụ. |

## Spacing, radius and elevation

| Token | Giá trị | Dùng cho |
| :---- | :---- | :---- |
| `space.1` | 4 | Khoảng cách icon/text nhỏ. |
| `space.2` | 8 | Gap trong chip, badge, row compact. |
| `space.3` | 12 | Padding item nhỏ. |
| `space.4` | 16 | Padding màn hình mobile mặc định. |
| `space.5` | 20 | Gap giữa form fields. |
| `space.6` | 24 | Section spacing. |
| `space.8` | 32 | Khoảng cách block lớn/onboarding. |
| `space.10` | 40 | Hero hoặc block lớn. |
| `radius.sm` | 8 | Badge, chip, control nhỏ. |
| `radius.md` | 12 | Button, input. |
| `radius.lg` | 16 | Card, image card. |
| `radius.xl` | 20 | Modal. |
| `radius.sheet` | 24 | Hai góc trên của bottom sheet. |
| `radius.full` | 999 | Avatar, pill, status dot. |
| `shadow.card` | `0 4px 16px rgba(15, 23, 42, 0.08)` | Card, sheet, lớp nổi cần phân tách nhẹ. |

Không dùng shadow dày, glow xanh hoặc hiệu ứng 3D trên card thông thường.

## Layout rules

- Mobile dùng base grid `4pt`, safe area đầy đủ, padding ngang mặc định `16`, khoảng cách section `24–32` và padding card `12–16`.
- Mobile không dùng global header chứa logo, search và notification trên mọi màn hình. Nội dung chính bắt đầu ngay sau safe area để tận dụng chiều cao.
- Logo chỉ xuất hiện có chủ đích ở splash/auth, không lặp lại trong thanh điều hướng của các màn hình đã đăng nhập.
- Màn hình cấp cao dùng title trong content khi thật sự cần; search/filter được đặt trong vùng nội dung liên quan thay vì thanh app-wide.
- Màn hình chi tiết và form chỉ giữ contextual top controls tối thiểu như back, close, step progress hoặc overflow. Với màn hình giàu hình ảnh, các control này có thể overlay an toàn trên nội dung.
- Touch target tối thiểu `44x44` trên mobile.
- CTA chính nằm trong vùng dễ với tới ở mobile, đặc biệt ở discovery, booking và form submit.
- Bottom sheet dùng cho filter, quick action và confirm nhẹ; màn riêng dùng cho form dài hoặc flow nhiều bước.
- Web admin ưu tiên layout dense, dễ scan: sidebar cố định, table có filter bar, detail mở theo page hoặc drawer tùy độ phức tạp.
- Không lồng nhiều card trong card; chỉ dùng card cho item lặp lại, modal/sheet hoặc khối dữ liệu cần đóng khung.
- Mỗi màn hình chỉ có một primary action; destructive action chỉ nổi bật tại điểm xác nhận.

## Imagery and media

- Hình ảnh tập trung vào con người thật, nhiếp ảnh gia đang làm việc, khoảnh khắc tự nhiên, ánh sáng chân thực, cảm xúc tích cực và bối cảnh Việt Nam khi phù hợp.
- Ưu tiên cặp đôi, gia đình, chân dung, sự kiện, không gian ngoài trời, quá trình chuẩn bị và hậu trường.
- Không dùng ảnh stock quá giả, filter nặng, retouch phi thực tế, ảnh quá tối/buồn, watermark bên thứ ba hoặc chỉ tập trung vào thiết bị.
- Photographer card ưu tiên tỷ lệ `4:5`; portfolio dùng `1:1` hoặc `4:5`; hero/banner dùng `16:9` hoặc `3:2`; avatar dùng `1:1`.
- Portfolio phải ưu tiên ảnh thật, tỷ lệ hiển thị nhất quán và crop không làm mất chủ thể.
- Discovery card dùng ảnh hero lớn, overlay text phải có scrim tối nhẹ để đảm bảo đọc được.
- Avatar có fallback bằng initials và màu nền trung tính nếu user chưa upload ảnh.
- Empty state có thể dùng illustration nhẹ, nhưng không được che CTA chính hoặc thay thế hướng dẫn hành động.
- Ảnh upload cần thumbnail, blurhash/placeholder và trạng thái retry khi lỗi mạng.

## Iconography

- Dùng một bộ icon outline bo tròn, stroke đồng nhất `1.75–2px`, kích thước chuẩn `20`, `24`, `28`.
- Bộ icon cốt lõi gồm Camera, Location, Search, Heart/Match, Message, Calendar/Booking, User, Shield/Verification, Star/Rating, Filter, Portfolio và Notification.
- Icon mặc định dùng Deep Navy hoặc Slate Gray; active dùng Royal Blue; selected/match dùng Royal Blue hoặc gradient; disabled dùng `#94A3B8`; error dùng `#DC2626`.
- Không trộn icon outline và filled tùy ý trong cùng navigation.
- Badge quan trọng như `Verified`, booking status, report/penalty phải có cả icon hoặc label, không chỉ dùng màu.
- Icon button cần accessible label và hit area đủ lớn.
- Không hiển thị icon chuông global vì MVP không có notification inbox; notification được xử lý qua push/deep link và settings.

## Core component styling

| Component | Quy chuẩn MVP |
| :---- | :---- |
| Primary button | Royal Blue, chữ trắng, cao `48px`, radius `12px`, weight `600`; không dùng gradient mặc định. |
| Secondary button | Nền trắng/surface, border Light Border, chữ Deep Navy. |
| Tertiary button | Không background, chữ Royal Blue. |
| Destructive button | Error Red; chỉ dùng cho thao tác rủi ro và phải có confirm khi cần. |
| Input | Label luôn hiển thị; cao tối thiểu `48px`; radius `12px`; focus Royal Blue; error Red kèm hướng sửa. |
| Photographer card | Ảnh/portfolio nổi bật, tên, loại, rating + số review, khu vực, giá khởi điểm, badge xác minh và action phù hợp; không chứa mô tả dài. |
| Photographer profile | Ưu tiên Portfolio → tên/xác minh → rating → chuyên môn → giá → khu vực → giới thiệu → review → CTA nhắn tin/đặt lịch. |
| Bottom navigation | Tối đa năm mục; active dùng Royal Blue; không phủ gradient toàn thanh và không dựa chỉ vào màu. |

## Motion and interaction

- Micro-interaction dùng `150–200ms`; screen transition dùng `200–300ms`; bottom sheet dùng `250–350ms`.
- Motion phải hỗ trợ hiểu trạng thái, không dùng animation chỉ để trang trí.
- Swipe card cần phản hồi trực tiếp theo hướng kéo; trạng thái accept/reject/like phải rõ trước khi submit.
- Match animation phải ngắn và có thể bỏ qua.
- Loading chính dùng skeleton cho list/card; spinner chỉ dùng cho action ngắn hoặc submit.
- Error state phải có retry hoặc hướng xử lý tiếp theo.
- Tôn trọng reduced motion khi platform/browser yêu cầu.
- Haptic nhẹ chỉ dùng cho match thành công, booking xác nhận, thao tác lưu hoặc lỗi quan trọng; không dùng cho mọi lần bấm.

## Accessibility

- Text thường đạt contrast tối thiểu `4.5:1`; text lớn tối thiểu `3:1` trên light và dark mode.
- Không truyền đạt trạng thái chỉ bằng màu; badge/status cần text hoặc icon.
- Vùng chạm tối thiểu `44x44`; button chính ưu tiên cao `48–52`.
- Form field có label rõ, error message cụ thể và focus state dễ nhận biết.
- Nội dung quan trọng phải hỗ trợ dynamic type/font scaling ở mobile.
- Ảnh có alt/accessibility description phù hợp; icon-only button bắt buộc có accessibility label.
- Form có thứ tự focus hợp lý và không mất CTA khi keyboard mở.
- Hành động nguy hiểm như block, report, reject booking hoặc khóa user cần confirm rõ.

## Design tokens implementation

| Nền tảng | Vị trí đề xuất | Nội dung |
| :---- | :---- | :---- |
| Mobile | `photomatch-mobile/src/theme/colors.ts` | Brand, semantic, role/status colors. |
| Mobile | `photomatch-mobile/src/theme/typography.ts` | Font family, text styles, weights, line heights. |
| Mobile | `photomatch-mobile/src/theme/spacing.ts` | Spacing, radius, shadow/elevation tokens. |
| Mobile | `photomatch-mobile/tailwind.config.js` | Map token sang NativeWind class. |
| Web admin | `photomatch-web/src/theme/tokens.ts` | Token dùng chung cho dashboard. |
| Web admin | `photomatch-web/src/theme/antd-theme.ts` | Map token sang Ant Design theme. |

Token tối thiểu:

```ts
export const colors = {
  primary: "#2563EB",
  secondary: "#7C3AED",
  accent: "#F97316",
  navy: "#0F172A",
  white: "#FFFFFF",
  background: "#F8FAFC",
  textPrimary: "#0F172A",
  textSecondary: "#64748B",
  border: "#E2E8F0",
  disabled: "#94A3B8",
  success: "#16A34A",
  warning: "#F59E0B",
  error: "#DC2626",
  info: "#2563EB",
} as const;

export const gradients = {
  brand: ["#2563EB", "#7C3AED"],
} as const;
```

## Design generation prompt

- [Google Stitch prompts cho mobile và web admin](../designs/stitch-mvp-design-prompts.md) phải dùng brand guideline và section này làm nguồn sự thật; mọi prompt cũ dùng raspberry hoặc Inter không còn hiệu lực.

# COMPONENTS

## Mobile common UI components

| Component | Dùng cho | Mô tả | Ghi chú |
| :---- | :---- | :---- | :---- |
| `AppScreen` | Mobile | Wrapper chuẩn cho mỗi màn hình. | Xử lý safe area, background, padding, keyboard avoidance. |
| `ContextualTopControls` | Mobile | Điều khiển tối thiểu theo ngữ cảnh. | Không tạo global header; chỉ gồm back/close/progress/overflow khi màn hình thực sự cần. |
| `BottomTabs` | Mobile | Điều hướng tab chính. | Discovery, Messages, Profile. |
| `Button` | Mobile | Nút hành động chính/phụ. | Có loading, disabled, icon. |
| `IconButton` | Mobile | Nút icon nhỏ. | Dùng cho back, close, filter, settings, more. |
| `TextField` | Mobile | Input text. | Hỗ trợ error, helper text, secure text. |
| `SelectField` | Mobile | Chọn một giá trị. | Dùng cho role, service và city. |
| `MultiSelectField` | Mobile | Chọn nhiều giá trị. | Dùng cho activity fields, services, filter. |
| `DateTimeField` | Mobile | Chọn ngày/giờ. | Dùng trong booking. |
| `PriceRangeField` | Mobile | Nhập khoảng giá. | Dùng trong filter và dịch vụ. |
| `AvatarPicker` | Mobile | Chọn/tải avatar. | Dùng onboarding/profile. |
| `ImageUploader` | Mobile | Upload ảnh. | Dùng portfolio, chat image, report evidence nếu cần. |
| `FileUploader` | Mobile | Upload file. | Dùng chat file. |
| `LocationPermissionCard` | Mobile | Xin quyền vị trí theo ngữ cảnh. | Dùng khi bật `Gần tôi` và trong quản lý profile/presence. |
| `SwipeCard` | Mobile | Card hồ sơ trong discovery. | Hiển thị ảnh, tên, khoảng cách, dịch vụ, giá. |
| `SwipeActions` | Mobile | Nút bỏ qua/quan tâm/chấp nhận/từ chối. | Theo role hiện tại. |
| `ProfileSummaryCard` | Mobile | Tóm tắt hồ sơ user/photographer. | Dùng Discovery và match. |
| `PortfolioGrid` | Mobile | Lưới ảnh portfolio. | Photographer cần tối thiểu 6 ảnh để hiển thị. |
| `ChatBubble` | Mobile | Bong bóng tin nhắn. | Hỗ trợ text, ảnh, file, trạng thái gửi/đọc. |
| `BookingStatusBadge` | Mobile | Badge trạng thái booking. | Theo `BookingStatus`. |
| `RatingInput` | Mobile | Customer chấm điểm Photographer. | Chọn bắt buộc từ 1 đến 5 sao, có accessible label. |
| `RatingSummary` | Mobile | Tóm tắt rating Photographer. | Hiển thị điểm trung bình và tổng số review. |
| `ReviewCard` | Mobile | Hiển thị review công khai. | Gồm rating, tên Customer, nội dung và thời gian. |
| `EmptyState` | Mobile | Trạng thái rỗng. | Dùng khi không có candidate, match, chat, booking. |
| `ErrorState` | Mobile | Trạng thái lỗi. | Có retry action. |
| `LoadingState` | Mobile | Trạng thái loading. | Skeleton hoặc spinner. |

## Web admin common UI components

| Component | Dùng cho | Mô tả | Ghi chú |
| :---- | :---- | :---- | :---- |
| `AdminShell` | Web admin | Layout chính của admin dashboard. | Gồm sidebar, header, content. |
| `AdminHeader` | Web admin | Header admin. | Hiển thị admin hiện tại, logout, quick action. |
| `AdminSidebar` | Web admin | Menu điều hướng admin. | Dashboard, users, photographers, reports, penalties, bookings, configs. |
| `AdminBreadcrumb` | Web admin | Breadcrumb theo trang. | Dùng trong màn detail. |
| `DataTable` | Web admin | Bảng dữ liệu admin. | Sort, filter, pagination, row action. |
| `StatusTag` | Web admin | Tag trạng thái. | Dùng account, booking, report, penalty. |
| `FilterBar` | Web admin | Thanh filter danh sách. | Search, status, date range, role. |
| `DetailPanel` | Web admin | Panel chi tiết bản ghi. | Dùng user/report/booking detail. |
| `ConfirmDialog` | Web admin | Dialog xác nhận hành động. | Khóa user, thu hồi penalty, reject report. |
| `AuditSummary` | Web admin | Tóm tắt lịch sử liên quan. | Không phải audit log đầy đủ; dùng status history/report history nếu có. |
| `EvidenceViewer` | Web admin | Xem bằng chứng liên quan. | Chat/message/booking/report attachment nếu có. |
| `ConfigTable` | Web admin | Quản lý danh mục cấu hình. | Activity fields, services, legal documents. |
| `AdminEmptyState` | Web admin | Trạng thái rỗng trong dashboard. | Dùng cho bảng dữ liệu. |
| `AdminErrorState` | Web admin | Trạng thái lỗi trong dashboard. | Có retry action. |

# FORMS

## Mobile forms

| Form | Dùng cho | Fields chính | Rule MVP |
| :---- | :---- | :---- | :---- |
| `SignInForm` | Mobile | Email, password; Google; Apple. | Tài khoản bị penalty chuyển tới màn hình phạt. |
| `SignUpForm` | Mobile | Email, password, confirm password; Google; Apple; consent Terms/Privacy. | Email/password bắt buộc verify email; chỉ gửi legal document id/version đang effective. |
| `ForgotPasswordForm` | Mobile | Email. | Gửi reset token có thời hạn. |
| `PersonalInformationForm` | Mobile onboarding | Display name, date of birth, city, bio. | Thiếu thông tin tối thiểu thì profile không hiển thị. |
| `AvatarForm` | Mobile onboarding | Avatar image. | Có thể bỏ qua nếu chưa bắt buộc, nhưng profile hiển thị nên có avatar. |
| `RoleSelectionForm` | Mobile onboarding | Role bổ sung ngoài customer. | Sau onboarding không được đổi role ban đầu. |
| `ActivityFieldsForm` | Mobile onboarding | Activity fields theo role. | Chỉ chọn field được role cho phép. |
| `ServicesForm` | Mobile onboarding | Services, service mode, min/max price, currency, price unit. | Photographer cần dịch vụ và giá để hiển thị. |
| `PortfolioForm` | Mobile photographer | Ảnh portfolio, title, description, service. | Photographer cần tối thiểu 6 ảnh thực tế để hiển thị. |
| `DiscoveryFilterForm` | Mobile | Services, price range, `Gần tôi`, radius, available only, verified only. | Filter có thể lưu local; radius/location chỉ gửi khi `Gần tôi` bật. |
| `BookingForm` | Mobile | Photographer, service, scheduled start/end, address, agreed price, currency, note. | Customer có thể đặt trực tiếp; submit thành công tạo/reuse match, conversation và booking `PENDING`. |
| `PhotographerReviewForm` | Mobile customer | Rating 1-5, comment tùy chọn, booking context. | Chỉ Customer của booking `COMPLETED`; mỗi booking tối đa một review. |
| `ReportForm` | Mobile | Reason code, description, target user/message/booking, evidence optional. | Reason code theo `ReportReasonCode`. |
| `ReadReceiptPrivacyForm` | Mobile settings | Enable/disable read receipt. | Ảnh hưởng hiển thị trạng thái đã đọc trong chat. |
| `NotificationSettingsForm` | Mobile settings | Match notification, booking notification. | MVP có push notification cho match và booking. |

## Web admin forms

| Form | Dùng cho | Fields chính | Rule MVP |
| :---- | :---- | :---- | :---- |
| `AdminSignInForm` | Web admin | Email, password. | Không cần phân quyền nhiều cấp trong MVP. |
| `UserSearchFilterForm` | Web admin | Search, role, account status, verification status. | Dùng trong danh sách users/photographers. |
| `UserAccountActionForm` | Web admin | Action, reason, note. | Admin có quyền khóa/mở khóa tài khoản. |
| `ReviewModerationForm` | Web admin | Status, moderation reason. | Chỉ đổi `PUBLISHED`, `HIDDEN`, `REMOVED`; rating/comment là read-only. |
| `ReportReviewForm` | Web admin | Status, admin note, resolution, notify user. | Admin xem xét report và thông báo kết quả. |
| `PenaltyActionForm` | Web admin | Penalty type, reason, starts at, ends at, status, notify user. | Dùng để tạo/thu hồi penalty có thời hạn. |
| `BookingAdminFilterForm` | Web admin | Search, status, date range, user, photographer. | Admin xem booking để hỗ trợ tranh chấp. |
| `ActivityFieldConfigForm` | Web admin | Code, name, description, allowed roles, status. | Quản lý lĩnh vực và mapping role được phép chọn. |
| `ServiceConfigForm` | Web admin | Activity field, code, name, description, status. | Quản lý dịch vụ theo lĩnh vực. |
| `LegalDocumentForm` | Web admin | Document type, version, content URL, effective at, status. | Khi đổi version có thể yêu cầu user consent lại. |

# [ERD](../system-designs/erd.md)

# ENUMS

Các enum nên được định nghĩa thống nhất ở backend, mobile và web admin. Backend là nguồn sự thật chính; mobile/web nên dùng type sinh từ OpenAPI hoặc generated API client để tránh lệch giá trị.

## AccountStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Tài khoản hoạt động bình thường. |
| `PENDING_VERIFICATION` | Tài khoản đã tạo nhưng chưa hoàn tất xác thực cần thiết. |
| `SUSPENDED` | Tài khoản bị tạm khóa hoặc hạn chế thao tác. |
| `BANNED` | Tài khoản bị khóa vĩnh viễn. |
| `DELETED` | Tài khoản đã xóa mềm. |

## IdentityVerificationStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `NOT_SUBMITTED` | Chưa gửi yêu cầu xác minh danh tính. |
| `PENDING` | Đã gửi và đang chờ provider xử lý. |
| `VERIFIED` | Đã xác minh thành công. |
| `REJECTED` | Bị từ chối xác minh. |
| `EXPIRED` | Kết quả xác minh hết hạn hoặc cần xác minh lại. |

## RoleCode

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `CUSTOMER` | Người dùng có nhu cầu thuê/chụp ảnh. |
| `PHOTOGRAPHER` | Thợ ảnh cung cấp dịch vụ. |
| `ADMIN` | Quản trị viên hệ thống. |

## RoleStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Vai trò đang hoạt động. |
| `INACTIVE` | Vai trò tạm không hoạt động. |
| `DISABLED` | Vai trò bị vô hiệu hóa bởi hệ thống/admin. |

## ProfileStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `DRAFT` | Hồ sơ chưa hoàn tất. |
| `ACTIVE` | Hồ sơ đủ điều kiện hiển thị. |
| `HIDDEN` | Hồ sơ bị người dùng ẩn. |
| `BLOCKED` | Hồ sơ bị hệ thống/admin chặn hiển thị. |

## PhotographerAvailabilityStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `AVAILABLE` | Sẵn sàng nhận việc. |
| `BUSY` | Đang bận, vẫn có thể hiển thị nhưng không ưu tiên. |
| `UNAVAILABLE` | Không nhận việc trong thời điểm hiện tại. |

## CatalogStatus

Dùng chung cho `CITIES`, `ACTIVITY_FIELDS`, `SERVICES`, `LEGAL_DOCUMENTS` nếu cần trạng thái danh mục.

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Đang được sử dụng. |
| `INACTIVE` | Tạm ngưng sử dụng, không cho chọn mới. |
| `ARCHIVED` | Lưu lịch sử, không hiển thị ở luồng chính. |

## ServiceMode

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `OFFERED` | Dịch vụ do thợ ảnh cung cấp. |
| `WANTED` | Dịch vụ mà người dùng đang có nhu cầu. |

## SwipeDirection

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `LEFT` | Bỏ qua. |
| `RIGHT` | Quan tâm. |
| `ACCEPT` | Thợ ảnh chấp nhận tín hiệu quan tâm. |
| `REJECT` | Thợ ảnh từ chối tín hiệu quan tâm. |

## SwipeSource

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `DISCOVERY` | Hành động phát sinh từ màn hình quẹt. |
| `NEARBY` | Hành động phát sinh từ danh sách quanh đây. |
| `PROFILE` | Hành động phát sinh từ trang chi tiết hồ sơ. |

## MatchStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Match còn hiệu lực và có thể mở chat. |
| `ENDED` | Match đã kết thúc. |
| `BLOCKED` | Match bị vô hiệu do một bên chặn bên còn lại. |

## ConversationStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Cuộc trò chuyện hoạt động bình thường. |
| `CLOSED` | Cuộc trò chuyện đã đóng. |
| `BLOCKED` | Cuộc trò chuyện bị chặn do block/report/penalty. |

## MessageType

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `TEXT` | Tin nhắn văn bản. |
| `SYSTEM` | Tin nhắn hệ thống, ví dụ thông báo match hoặc booking. |

## MessageStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `SENT` | Đã gửi lên server. |
| `DELIVERED` | Đã giao tới người nhận/thiết bị. |
| `READ` | Người nhận đã đọc. |
| `DELETED` | Tin nhắn đã bị xóa mềm. |

## ShootRequestStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `OPEN` | Yêu cầu đang mở. |
| `MATCHED` | Yêu cầu đã có match phù hợp. |
| `BOOKED` | Yêu cầu đã chuyển thành booking. |
| `CLOSED` | Yêu cầu đã đóng. |
| `CANCELLED` | Yêu cầu bị hủy. |

## BookingStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `DRAFT` | Booking mới tạo nháp. |
| `PENDING` | Đang chờ xác nhận. |
| `ACCEPTED` | Đã được chấp nhận. |
| `REJECTED` | Bị từ chối. |
| `CANCELLED` | Đã hủy. |
| `IN_PROGRESS` | Buổi chụp đang diễn ra. |
| `COMPLETED` | Booking hoàn thành. |
| `DISPUTED` | Có tranh chấp. |

## ReviewStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `PUBLISHED` | Review đang hiển thị. |
| `HIDDEN` | Review bị ẩn. |
| `REMOVED` | Review bị gỡ do vi phạm. |

## NotificationType

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `MATCH_CREATED` | Có match mới. |
| `MESSAGE_RECEIVED` | Có tin nhắn mới. |
| `BOOKING_CREATED` | Có booking mới. |
| `BOOKING_STATUS_CHANGED` | Trạng thái booking thay đổi. |
| `REPORT_RESOLVED` | Báo cáo đã được xử lý. |
| `ACCOUNT_PENALTY_CREATED` | Tài khoản nhận hình phạt/cảnh cáo. |
| `SYSTEM` | Thông báo hệ thống chung. |

## ReportReasonCode

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `SPAM` | Spam/quảng cáo không phù hợp. |
| `HARASSMENT` | Quấy rối hoặc xúc phạm. |
| `FAKE_PROFILE` | Hồ sơ giả mạo. |
| `INAPPROPRIATE_CONTENT` | Nội dung không phù hợp. |
| `SCAM` | Có dấu hiệu lừa đảo. |
| `OTHER` | Lý do khác. |

## ReportStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `OPEN` | Báo cáo mới tạo. |
| `IN_REVIEW` | Admin/moderator đang xử lý. |
| `RESOLVED` | Đã xử lý xong. |
| `REJECTED` | Báo cáo không hợp lệ hoặc không đủ căn cứ. |

## PenaltyType

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `WARNING` | Cảnh cáo. |
| `TEMPORARY_SUSPENSION` | Khóa tạm thời. |
| `PERMANENT_BAN` | Khóa vĩnh viễn. |
| `FEATURE_RESTRICTION` | Hạn chế một số tính năng. |

## PenaltyStatus

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `ACTIVE` | Hình phạt đang hiệu lực. |
| `EXPIRED` | Hình phạt đã hết hạn. |
| `REVOKED` | Hình phạt đã được thu hồi. |

## LegalDocumentType

| Giá trị | Ý nghĩa |
| :---- | :---- |
| `TERMS_OF_SERVICE` | Điều khoản sử dụng. |
| `PRIVACY_POLICY` | Chính sách bảo mật. |
| `COMMUNITY_GUIDELINES` | Quy định cộng đồng nếu cần sau MVP. |

## UserPreference

| Enum | Giá trị đề xuất |
| :---- | :---- |
| `Language` | `VI`, `EN` |
| `Theme` | `SYSTEM`, `LIGHT`, `DARK` |

# NFR

## Security

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| Authentication | Bắt buộc | Dùng JWT access token + refresh token; refresh token lưu dạng hash. |
| Email verification | Bắt buộc | Email/password phải xác thực email trước khi dùng luồng chính. |
| Password storage | Bắt buộc | Hash bằng thuật toán mạnh như Argon2 hoặc bcrypt. |
| Authorization | Bắt buộc | Guard theo user, role và ownership. |
| Admin access | Bắt buộc | Admin dashboard yêu cầu đăng nhập riêng; MVP không cần nhiều cấp quyền. |
| Rate limit | Bắt buộc | Áp dụng cho auth, forgot password, swipe/interest decision, upload, report, booking, review và admin moderation action. |
| Input validation | Bắt buộc | DTO + Zod/class-validator; không tin dữ liệu từ client. |
| File upload safety | Bắt buộc | Giới hạn dung lượng, MIME type, extension; scan hoặc quarantine nếu cần sau MVP. |
| Object storage access | Bắt buộc | Cloudflare R2 bucket mặc định private; upload/download qua pre-signed URL có thời hạn và policy theo owner/purpose. |
| Secrets management | Bắt buộc | Không commit secret; dùng env vars và secret manager của platform. |

## Privacy & data protection

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| Vị trí và khoảng cách gần đúng | Bắt buộc | Không trả GPS thật hoặc tọa độ công khai của user khác; client chỉ nhận distance bucket do backend tính. |
| Thời hạn hiển thị vị trí | Bắt buộc | Mặc định 24h, user có thể chỉnh; hết hạn thì không xuất hiện trong kết quả `Gần tôi`. |
| Read receipt privacy | Bắt buộc | User có thể bật/tắt trạng thái đã đọc. |
| Block privacy | Bắt buộc | Người bị block không được nhắn tiếp và không xuất hiện trong discovery/match interaction. |
| Data minimization | Bắt buộc | API chỉ trả field cần cho màn hình hiện tại. |
| Soft delete | Nên có | User/content quan trọng nên dùng `deleted_at` để giữ lịch sử tranh chấp. |

## Performance

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| API response time | P95 < 500ms cho API thường | Không tính upload file lớn hoặc truy vấn map phức tạp. |
| Discovery global/nearby query | P95 < 1s | Feed global dùng eligibility/filter index; `Gần tôi` dùng thêm PostGIS. |
| Chat send message | P95 < 500ms | Tin nhắn text phải phản hồi nhanh; upload ảnh/file xử lý riêng. |
| Mobile startup | < 3s ở mạng bình thường | Splash không giữ quá lâu nếu token/session đã có. |
| Pagination | Bắt buộc | Áp dụng cho list users, messages, bookings, reports, notifications nếu có. |
| Image optimization | Bắt buộc | Resize/compress portfolio và chat image trước/sau upload. |

## Reliability & availability

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| Availability target | 99%+ cho MVP production | Chưa cần HA phức tạp nhưng cần health check và restart policy. |
| Backup database | Bắt buộc | Backup daily; có quy trình restore thử nghiệm. |
| Migration safety | Bắt buộc | Prisma migration chạy qua CI/CD, không sửa database production thủ công. |
| Background jobs | Bắt buộc | Dùng cho hết hạn vị trí, hết hạn penalty, gửi notification, cleanup. |
| Idempotency | Bắt buộc theo command | Áp dụng cho swipe/interest decision, unmatch, booking action, review create, upload completion, notification send và penalty action. |

## Observability

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| Error monitoring | Bắt buộc | Sentry cho mobile, web admin và backend. |
| API logging | Bắt buộc | Log request id, user id, status code, latency; không log secret/token. |
| Audit-like history | Bắt buộc theo domain | Không cần admin audit log đầy đủ, nhưng cần booking status history, report resolution, penalty history. |
| Product analytics | Nên có | Track sign up, onboarding completed, profile published, match created, booking created/accepted. |
| Alerting | Nên có | Alert cho error rate, API latency, database CPU/storage, cost budget. |

## Compatibility

| Yêu cầu | Mức MVP | Ghi chú triển khai |
| :---- | :---- | :---- |
| Mobile platform | Bắt buộc | iOS và Android qua Expo. |
| Web admin browser | Bắt buộc | Chrome, Safari, Edge bản mới. |
| API versioning | Bắt buộc | Prefix `/api/v1`; breaking change phải tăng version hoặc giữ backward compatibility. |
| Timezone | Bắt buộc | Lưu UTC ở backend; client hiển thị theo timezone thiết bị. |
| Language | MVP | `VI`, `EN`; tiếng Việt là mặc định. |

# API CONTRACT

Backend dùng **Option B: NestJS API + Prisma + PostgreSQL/PostGIS**. API contract được xuất bằng OpenAPI từ backend và sinh client/type cho mobile + web admin.

## API conventions

| Quy ước | Giá trị |
| :---- | :---- |
| Base path | `/api/v1` |
| Auth header | `Authorization: Bearer <access_token>` |
| Content type | `application/json`; upload dùng `multipart/form-data` hoặc pre-signed URL |
| Pagination | `page`, `limit` hoặc cursor theo từng endpoint |
| Date/time | ISO 8601 UTC |
| Error shape | `{ code, message, details?, requestId }` |
| Idempotency | Header `Idempotency-Key` cho action dễ double submit như swipe decision, booking, review, upload completion và penalty |

## CRUD và lifecycle coverage

Không phải entity nào cũng mở đủ bốn verb CRUD. Dữ liệu giao dịch, bằng chứng và state machine dùng command/lifecycle action để tránh sửa hoặc xóa lịch sử ngoài ý muốn.

| Resource/feature | Create | Read | Update/action | Delete/archive | Quyết định MVP |
| :---- | :---- | :---- | :---- | :---- | :---- |
| Profile và avatar | Onboarding upsert/avatar attach | Self + public role profile | Patch profile, replace avatar | Remove avatar | Không hard-delete profile/account trong scope hiện tại. |
| Role | Chọn một role bổ sung | List owned roles | Switch current role | Không | Role ban đầu bất biến sau onboarding. |
| Reference roles/cities | Seed/Admin | Available active lists | Không | Không | Mobile chỉ đọc để render onboarding/filter; MVP quản trị bằng seed/config vận hành. |
| User legal consent | Accept active version | Get current consent state | Không | Không | Append-only; version mới tạo consent record mới. |
| Activity field/service selection | `PUT` replace selection set | `GET` theo owned role | Cùng `PUT` atomically replace | Bỏ item khỏi replacement set | Không expose CRUD từng relation để tránh partial invalid state. |
| Portfolio | Create item | Self/public list | Patch item + reorder | Soft-delete item | Đủ CRUD nghiệp vụ, mọi thay đổi recalculates discovery eligibility. |
| Exact location/presence | `PUT` exact + publish | `GET` current presence | `PUT` refresh/configure | Clear exact + disable presence | Không expose GET exact-location history; public chỉ dùng tọa độ đã làm lệch. |
| Interest/swipe | Create decision | Incoming interest list | Photographer accept/reject action | Không | Swipe là event bất biến; quyết định thay thế update/delete. |
| Match | System create/reuse | List/detail | Unmatch action | Không | Unmatch đóng lifecycle nhưng giữ lịch sử và rematch window. |
| Conversation/message | Send message | Conversation/message history | Receipt action | Không | Message không sửa/hard-delete trong MVP vì retry và evidence. |
| Booking | Create | List/detail/history | Patch khi `PENDING` + status actions | Cancel action | Không hard-delete booking hoặc history. |
| Photographer review | Create một lần | Booking review + public list/summary | Admin moderation status | Không | Customer review bất biến; Admin không sửa rating/comment. |
| Block | Create | List | Không | Unblock | Đủ lifecycle cần thiết. |
| Report | Create | Admin list/detail | Resolve/reject action | Không | Report/evidence bất biến với user. |
| Penalty | Admin create | User active restrictions + Admin list/detail | Revoke/expire action | Không | Giữ history phục vụ enforcement và dispute. |
| Settings | System create default | Get | Patch | Không | Một settings record mỗi user. |
| Upload/media | Create intent + complete | Authorized access URL | Attach vào domain record | System orphan cleanup/soft replacement | Client không được xóa object tùy ý khỏi R2. |
| Device token | Register/upsert | Không cần UI list | Rotate bằng register/upsert | Remove | Đủ lifecycle push token. |
| Activity field/service catalog | Admin create | Public/admin list + admin detail | Admin patch/status | Archive status | Không hard-delete vì có historical references. |
| Legal document | Admin tạo version | Current public + admin list/detail | Patch bản `INACTIVE` + status action | Archive status | Version đã effective bất biến; tạo version mới thay vì sửa đè/xóa. |

## Mobile API endpoints

| Module | Endpoint | Method | Actor | Mục đích |
| :---- | :---- | :---- | :---- | :---- |
| Auth | `/auth/sign-up` | `POST` | Guest | Đăng ký email/password. |
| Auth | `/auth/sign-in` | `POST` | Guest | Đăng nhập email/password. |
| Auth | `/auth/oauth` | `POST` | Guest | Đăng nhập Google/Apple. |
| Auth | `/auth/verify-email` | `POST` | User | Xác thực email. |
| Auth | `/auth/resend-verification` | `POST` | User | Gửi lại email xác thực với cooldown và response chống enumeration. |
| Auth | `/auth/change-pending-email` | `POST` | Unverified user | Đổi email chưa xác thực, thu hồi token cũ và gửi verification mới. |
| Auth | `/auth/forgot-password` | `POST` | Guest | Gửi email reset password. |
| Auth | `/auth/reset-password` | `POST` | Guest | Đặt lại mật khẩu. |
| Auth | `/auth/refresh-token` | `POST` | User | Gia hạn access token. |
| Auth | `/auth/sign-out` | `POST` | User | Thu hồi session hiện tại. |
| Current user | `/me` | `GET` | User | Lấy user hiện tại, role, onboarding status. |
| Onboarding | `/me/onboarding/progress` | `GET` | User | Lấy tiến độ onboarding. |
| Profile | `/me/profile` | `GET` | User | Lấy đầy đủ hồ sơ của chính mình để hiển thị/chỉnh sửa. |
| Onboarding | `/me/profile` | `PATCH` | User | Cập nhật hồ sơ cá nhân. |
| Onboarding | `/me/avatar` | `POST` | User | Upload/cập nhật avatar. |
| Onboarding | `/me/avatar` | `DELETE` | User | Gỡ avatar và tính lại onboarding/discovery eligibility. |
| Roles | `/me/roles` | `GET` | User | Lấy các role của user. |
| Roles | `/me/roles` | `POST` | User | Chọn role trong onboarding. |
| Roles | `/roles/available` | `GET` | User | Lấy role bổ sung đang được phép chọn trong onboarding. |
| Roles | `/me/current-role` | `PATCH` | User | Switch giữa customer và role đã chọn. |
| Role fields | `/me/roles/{userRoleId}/activity-fields` | `GET` | Role owner | Lấy lĩnh vực đã chọn cho role thuộc user. |
| Role fields | `/me/roles/{userRoleId}/activity-fields` | `PUT` | Role owner | Thay thế atomically tập lĩnh vực đã chọn. |
| Role services | `/me/roles/{userRoleId}/services` | `GET` | Role owner | Lấy dịch vụ/pricing đã chọn cho role thuộc user. |
| Role services | `/me/roles/{userRoleId}/services` | `PUT` | Role owner | Thay thế atomically tập dịch vụ/pricing hợp lệ. |
| Catalog | `/activity-fields` | `GET` | User | Lấy lĩnh vực hoạt động. |
| Catalog | `/services` | `GET` | User | Lấy dịch vụ theo lĩnh vực/role. |
| Catalog | `/cities` | `GET` | Guest/User | Lấy danh sách city active dùng cho onboarding/filter. |
| Legal | `/legal-documents/current` | `GET` | Guest/User | Lấy Terms/Privacy đang có hiệu lực theo `type` để hiển thị và consent. |
| Legal consent | `/me/consents` | `GET` | User | Lấy trạng thái consent của user đối với các legal version hiện hành. |
| Legal consent | `/me/consents` | `POST` | User | Ghi nhận append-only consent cho legal document đang active/effective. |
| Public profile | `/profiles/{userRoleId}` | `GET` | User | Lấy hồ sơ public theo role với dữ liệu riêng tư đã loại bỏ. |
| Photographer | `/me/photographer-profile` | `GET` | Photographer | Lấy dữ liệu Photographer của chính mình để chỉnh sửa. |
| Photographer | `/me/photographer-profile` | `PATCH` | Photographer | Cập nhật hồ sơ thợ ảnh. |
| Portfolio | `/me/portfolio` | `GET` | Photographer | Lấy portfolio của chính mình. |
| Portfolio | `/me/portfolio` | `POST` | Photographer | Tạo portfolio item. |
| Portfolio | `/me/portfolio/{portfolioItemId}` | `GET` | Photographer | Lấy chi tiết item thuộc owner để edit/deep link. |
| Portfolio | `/me/portfolio/{portfolioItemId}` | `PATCH` | Photographer | Cập nhật portfolio item. |
| Portfolio | `/me/portfolio/{portfolioItemId}` | `DELETE` | Photographer | Xóa mềm portfolio item. |
| Portfolio | `/me/portfolio/order` | `PUT` | Photographer | Thay đổi thứ tự các portfolio item thuộc owner. |
| Portfolio | `/photographers/{photographerRoleId}/portfolio` | `GET` | User | Lấy portfolio public có phân trang của Photographer. |
| Discovery | `/discovery/candidates` | `GET` | User | Lấy candidate cho màn hình quẹt; không có radius là global, có radius là `Gần tôi` và trả distance bucket. |
| Nearby compatibility | `/nearby` | `GET` | User | Giữ tương thích API với bán kính mặc định 20 km; mobile MVP không có route/tab riêng. |
| Location | `/me/location` | `PUT` | User | Ghi đè vị trí chính xác mới nhất của owner; không trả cho user khác. |
| Location | `/me/location` | `DELETE` | User | Xóa vị trí chính xác đã lưu và tắt discovery presence. |
| Presence | `/me/discovery-presence` | `GET` | User | Lấy trạng thái/vòng đời presence hiện tại của role đang active. |
| Presence | `/me/discovery-presence` | `PUT` | User | Bật/tắt hiển thị trong kết quả `Gần tôi` và thời hạn vị trí. |
| Swipes | `/swipes` | `POST` | User | Tạo quyết định candidate như `LEFT` hoặc Customer `RIGHT`; accept/reject incoming interest dùng endpoint riêng. |
| Interests | `/interests/incoming` | `GET` | Photographer | Lấy Customer interests chưa xử lý có phân trang. |
| Interests | `/interests/{interestId}/decision` | `POST` | Photographer | Chấp nhận hoặc từ chối đúng interest đang pending. |
| Matches | `/matches` | `GET` | User | Lấy danh sách match. |
| Matches | `/matches/{matchId}` | `GET` | User | Lấy chi tiết match. |
| Matches | `/matches/{matchId}/unmatch` | `POST` | Match participant | Kết thúc match, đóng conversation cho message mới và giữ lịch sử. |
| Chat | `/conversations` | `GET` | User | Lấy danh sách hội thoại. |
| Chat | `/conversations/{conversationId}` | `GET` | User | Lấy chi tiết hội thoại. |
| Chat | `/conversations/{conversationId}/messages` | `GET` | User | Lấy tin nhắn. |
| Chat | `/conversations/{conversationId}/messages` | `POST` | User | Gửi text/image/file message. |
| Chat | `/messages/{messageId}/receipts` | `PUT` | User | Cập nhật delivered/read. |
| Bookings | `/bookings` | `GET` | User | Lấy danh sách booking của user. |
| Bookings | `/bookings` | `POST` | Customer hoặc matched user | Customer đặt trực tiếp hoặc participant tạo từ conversation; tạo/reuse match và conversation rồi tạo booking `PENDING` atomically. |
| Bookings | `/bookings/{bookingId}` | `GET` | User | Lấy chi tiết booking. |
| Bookings | `/bookings/{bookingId}` | `PATCH` | User | Cập nhật thông tin được phép theo trạng thái và actor policy. |
| Bookings | `/bookings/{bookingId}/status` | `POST` | User | Accept/reject/cancel/start/complete/dispute. |
| Reviews | `/bookings/{bookingId}/review` | `POST` | Customer | Tạo một review cho Photographer sau booking `COMPLETED`. |
| Reviews | `/bookings/{bookingId}/review` | `GET` | User | Lấy review gắn với booking nếu caller có quyền hoặc review đang public. |
| Reviews | `/photographers/{photographerRoleId}/reviews` | `GET` | User | Lấy rating summary và danh sách review `PUBLISHED` có phân trang. |
| Blocks | `/blocks` | `GET` | User | Lấy danh sách đã chặn. |
| Blocks | `/blocks` | `POST` | User | Chặn user. |
| Blocks | `/blocks/{blockedUserId}` | `DELETE` | User | Bỏ chặn user. |
| Reports | `/reports` | `POST` | User | Tạo report từ profile/chat/booking. |
| Restrictions | `/me/restrictions` | `GET` | User | Lấy account/feature penalty đang hiệu lực để render penalty screen. |
| Settings | `/me/settings` | `GET` | User | Lấy settings. |
| Settings | `/me/settings` | `PATCH` | User | Cập nhật theme, language, notification và read receipt. |
| Uploads | `/uploads/presign` | `POST` | User | Lấy pre-signed URL để upload trực tiếp lên Cloudflare R2 theo owner/purpose. |
| Uploads | `/uploads/{uploadId}/complete` | `POST` | Upload owner | Xác nhận upload R2 hoàn tất, verify metadata và chuyển asset sang usable. |
| Uploads | `/uploads/{assetId}/access-url` | `POST` | Authorized user | Cấp signed download URL ngắn hạn sau domain authorization. |
| Devices | `/devices` | `POST` | User | Đăng ký push token. |
| Devices | `/devices/{deviceId}` | `DELETE` | User | Gỡ push token. |

## Web admin API endpoints

| Module | Endpoint | Method | Actor | Mục đích |
| :---- | :---- | :---- | :---- | :---- |
| Admin auth | `/admin/auth/sign-in` | `POST` | Admin | Đăng nhập admin. |
| Admin auth | `/admin/auth/sign-out` | `POST` | Admin | Đăng xuất admin. |
| Dashboard | `/admin/dashboard/summary` | `GET` | Admin | Lấy thống kê tổng quan. |
| Users | `/admin/users` | `GET` | Admin | Danh sách users có filter. |
| Users | `/admin/users/{userId}` | `GET` | Admin | Chi tiết user. |
| Users | `/admin/users/{userId}/status` | `POST` | Admin | Khóa/mở user qua account-status/penalty lifecycle, bắt buộc lý do và giữ history. |
| Photographers | `/admin/photographers` | `GET` | Admin | Danh sách photographer. |
| Photographers | `/admin/photographers/{photographerId}` | `GET` | Admin | Chi tiết photographer. |
| Reports | `/admin/reports` | `GET` | Admin | Danh sách report. |
| Reports | `/admin/reports/{reportId}` | `GET` | Admin | Chi tiết report. |
| Reports | `/admin/reports/{reportId}/resolve` | `POST` | Admin | Xử lý report. |
| Reviews | `/admin/reviews` | `GET` | Admin | Danh sách review có filter theo status, rating, Customer và Photographer. |
| Reviews | `/admin/reviews/{reviewId}` | `GET` | Admin | Chi tiết review và booking context được phép. |
| Reviews | `/admin/reviews/{reviewId}/status` | `POST` | Admin | Publish/hide/remove review kèm lý do; không sửa rating/comment. |
| Penalties | `/admin/penalties` | `GET` | Admin | Danh sách penalty. |
| Penalties | `/admin/penalties` | `POST` | Admin | Tạo penalty. |
| Penalties | `/admin/penalties/{penaltyId}` | `GET` | Admin | Chi tiết penalty. |
| Penalties | `/admin/penalties/{penaltyId}/revoke` | `POST` | Admin | Thu hồi penalty. |
| Bookings | `/admin/bookings` | `GET` | Admin | Danh sách booking. |
| Bookings | `/admin/bookings/{bookingId}` | `GET` | Admin | Chi tiết booking và status history. |
| Config | `/admin/activity-fields` | `GET` | Admin | Danh sách lĩnh vực có filter và phân trang. |
| Config | `/admin/activity-fields` | `POST` | Admin | Tạo lĩnh vực hoạt động. |
| Config | `/admin/activity-fields/{activityFieldId}` | `GET` | Admin | Lấy chi tiết lĩnh vực. |
| Config | `/admin/activity-fields/{activityFieldId}` | `PATCH` | Admin | Cập nhật metadata hoặc chuyển `ACTIVE`/`INACTIVE`/`ARCHIVED`; không hard-delete. |
| Config | `/admin/services` | `GET` | Admin | Danh sách dịch vụ có filter và phân trang. |
| Config | `/admin/services` | `POST` | Admin | Tạo dịch vụ thuộc lĩnh vực. |
| Config | `/admin/services/{serviceId}` | `GET` | Admin | Lấy chi tiết dịch vụ. |
| Config | `/admin/services/{serviceId}` | `PATCH` | Admin | Cập nhật metadata hoặc status; không hard-delete. |
| Legal | `/admin/legal-documents` | `GET` | Admin | Danh sách các version tài liệu pháp lý. |
| Legal | `/admin/legal-documents` | `POST` | Admin | Tạo version Terms/Privacy mới. |
| Legal | `/admin/legal-documents/{legalDocumentId}` | `GET` | Admin | Lấy chi tiết một version. |
| Legal | `/admin/legal-documents/{legalDocumentId}` | `PATCH` | Admin | Chỉ sửa version `INACTIVE` chưa effective; version `ACTIVE` bất biến. |
| Legal | `/admin/legal-documents/{legalDocumentId}/status` | `POST` | Admin | Activate/archive version theo state rule và effective date. |

## Realtime/WebSocket contract

| Event | Direction | Payload chính | Ghi chú |
| :---- | :---- | :---- | :---- |
| `conversation.message.created` | Server → Client | `conversationId`, `message` | Gửi khi có tin nhắn mới. |
| `conversation.message.receipt_updated` | Server → Client | `messageId`, `userId`, `deliveredAt`, `readAt` | Tôn trọng read receipt privacy. |
| `match.created` | Server → Client | `matchId`, `conversationId` | Có thể kèm push notification. |
| `booking.created` | Server → Client | `bookingId`, `status` | Có thể kèm push notification. |
| `booking.status_changed` | Server → Client | `bookingId`, `previousStatus`, `newStatus` | Đồng bộ booking detail/list. |

# TECH STACKS

Thống nhất chọn **Option B: NestJS API + Prisma + PostgreSQL/PostGIS**. Không dùng Supabase làm backend/auth/storage/realtime chính trong MVP.

| Thành phần | Công nghệ đề xuất |
| :---- | :---- |
| Mobile Application | React Native, Expo, TypeScript |
| Navigation | Expo Router |
| UI styling | NativeWind |
| Client state | Zustand |
| Server state | TanStack Query |
| Form | React Hook Form |
| Validation | Zod |
| Swipe gesture | React Native Gesture Handler |
| Animation | React Native Reanimated |
| Device location | Expo Location |
| Backend API | NestJS, TypeScript |
| ORM | Prisma |
| Database | PostgreSQL |
| Geospatial database | PostGIS |
| Authentication | JWT access token, refresh token, NestJS Guards |
| File storage | Cloudflare R2 qua S3-compatible API và pre-signed URL |
| Realtime chat | NestJS WebSocket Gateway |
| Cache/queue | Redis |
| Push notification | Expo Notifications và Firebase Cloud Messaging |
| Admin dashboard | Next.js, TypeScript, Ant Design |
| Error monitoring | Sentry |
| Product analytics | PostHog hoặc Firebase Analytics |
| Mobile deployment | Expo Application Services — EAS |
| Admin deployment | Vercel |
| API deployment | Docker container trên Cloud Run, Render, Railway, Fly.io hoặc VPS |
| Source control | Gitlab |
| CI/CD | Gitlab CI |

# FOLDER STRUCTURE

## Tổng quan monorepo

## Mobile

| Thành phần | Công nghệ sử dụng |
| :---- | :---- |
| Framework | React Native, Expo |
| Language | TypeScript |
| Routing | Expo Router |
| UI styling | NativeWind |
| Client state | Zustand |
| Server state | TanStack Query |
| Form & validation | React Hook Form, Zod |
| Gesture & animation | React Native Gesture Handler, React Native Reanimated |
| Device location | Expo Location |
| Notification | Expo Notifications, Firebase Cloud Messaging |
| Monitoring & analytics | Sentry, PostHog hoặc Firebase Analytics |
| Build/deploy | Expo Application Services (EAS) |

```text
photomatch-mobile/                     # Thư mục gốc của ứng dụng mobile Photomatch
|-- app/                               # Khai báo route theo Expo Router
|   |-- _layout.tsx                    # Layout gốc, bọc toàn bộ navigation của app
|   |-- index.tsx                      # Màn hình entry, xử lý điều hướng ban đầu
|   |-- (public)/                      # Nhóm route công khai, chưa cần đăng nhập
|   |   |-- _layout.tsx                # Layout cho các màn hình public
|   |   |-- splash.tsx                 # Màn hình splash/loading khi mở ứng dụng
|   |   `-- penalty.tsx                # Màn hình thông báo tài khoản bị khóa hoặc phạt
|   |-- (auth)/                        # Nhóm route xác thực tài khoản
|   |   |-- _layout.tsx                # Layout cho luồng đăng nhập và đăng ký
|   |   |-- sign-in.tsx                # Màn hình đăng nhập
|   |   |-- sign-up.tsx                # Màn hình đăng ký
|   |   |-- forgot-password.tsx        # Màn hình yêu cầu đặt lại mật khẩu
|   |   |-- reset-password.tsx         # Màn hình nhập mật khẩu mới
|   |   `-- verify-email.tsx           # Màn hình xác thực email
|   |-- (onboarding)/                  # Nhóm route hoàn thiện hồ sơ sau đăng ký
|   |   |-- _layout.tsx                # Layout cho luồng onboarding
|   |   |-- personal-information.tsx   # Màn hình nhập thông tin cá nhân
|   |   |-- avatar.tsx                 # Màn hình chọn hoặc tải ảnh đại diện
|   |   |-- role.tsx                   # Màn hình chọn vai trò người dùng hoặc thợ ảnh
|   |   |-- activity-fields.tsx        # Màn hình chọn lĩnh vực hoạt động
|   |   |-- services.tsx               # Màn hình chọn dịch vụ theo lĩnh vực
|   |   |-- location-permission.tsx    # Màn hình xin quyền truy cập vị trí
|   |   `-- complete.tsx               # Màn hình hoàn tất onboarding
|   |-- (tabs)/                        # Nhóm route chính hiển thị trong bottom tabs
|   |   |-- _layout.tsx                # Layout cấu hình tab navigation
|   |   |-- discovery/                 # Tab quẹt và lọc khám phá hồ sơ
|   |   |   |-- index.tsx              # Màn hình danh sách thẻ quẹt
|   |   |   |-- filters.tsx            # Bộ lọc service/price/Gần tôi/radius
|   |   |   `-- interests.tsx          # Danh sách Customer đang quan tâm cho Photographer
|   |   |-- messages/                  # Tab danh sách cuộc trò chuyện
|   |   |   `-- index.tsx              # Màn hình inbox và danh sách chat
|   |   `-- profile/                   # Tab hồ sơ cá nhân
|   |       `-- index.tsx              # Màn hình profile của tài khoản hiện tại
|   |-- users/                         # Route chi tiết hồ sơ người dùng
|   |   `-- [userRoleId].tsx           # Màn hình hồ sơ public theo role
|   |-- photographers/                 # Route chi tiết hồ sơ thợ ảnh
|   |   `-- [photographerRoleId]/      # Route hồ sơ public theo Photographer role
|   |       |-- index.tsx              # Màn hình chi tiết Photographer
|   |       `-- reviews.tsx            # Danh sách review public
|   |-- matches/                       # Route chi tiết kết nối giữa hai bên
|   |   `-- [matchId].tsx              # Màn hình chi tiết match theo matchId
|   |-- chat/                          # Route hội thoại trực tiếp
|   |   `-- [conversationId].tsx       # Màn hình chat theo conversationId
|   |-- bookings/                      # Route quản lý đặt lịch
|   |   |-- index.tsx                  # Màn hình danh sách booking
|   |   |-- create.tsx                 # Màn hình tạo booking mới
|   |   `-- [bookingId]/               # Route theo booking
|   |       |-- index.tsx              # Màn hình chi tiết booking
|   |       `-- review.tsx             # Form/read-only review theo booking
|   |-- portfolio/                     # Route quản lý portfolio của thợ ảnh
|   |   |-- index.tsx                  # Màn hình danh sách ảnh trong portfolio
|   |   |-- create.tsx                 # Màn hình thêm ảnh hoặc bộ ảnh mới
|   |   |-- edit.tsx                   # Màn hình chỉnh sửa portfolio item
|   |   `-- [portfolioItemId].tsx      # Màn hình chi tiết portfolio item
|   |-- reports/                       # Route báo cáo vi phạm
|   |   `-- create.tsx                 # Màn hình tạo báo cáo người dùng hoặc nội dung
|   |-- edit-profile.tsx               # Màn hình chỉnh sửa hồ sơ cá nhân
|   `-- settings/                      # Route cài đặt ứng dụng và tài khoản
|       |-- index.tsx                  # Màn hình tổng quan cài đặt
|       |-- appearance.tsx             # Màn hình tùy chỉnh giao diện
|       |-- language.tsx               # Màn hình chọn ngôn ngữ
|       |-- notifications.tsx          # Cài đặt push match/booking, không phải notification inbox
|       |-- blocked-users.tsx          # Màn hình danh sách người dùng đã chặn
|       |-- privacy-settings.tsx       # Màn hình cài đặt vị trí/read receipt/visibility
|       |-- privacy-policy.tsx         # Màn hình chính sách bảo mật hiện hành
|       |-- terms.tsx                  # Màn hình điều khoản sử dụng
|       |-- about.tsx                  # Màn hình thông tin về ứng dụng
|       `-- invite-friends.tsx         # Màn hình giới thiệu bạn bè
|-- src/                               # Mã nguồn nghiệp vụ và thành phần dùng chung
|   |-- features/                      # Chia module theo từng domain tính năng
|   |   |-- auth/                      # Logic và UI cho xác thực
|   |   |-- onboarding/                # Logic và UI cho onboarding
|   |   |-- profile/                   # Logic và UI cho hồ sơ cá nhân
|   |   |-- photographer/              # Logic và UI riêng cho thợ ảnh
|   |   |-- portfolio/                 # Logic và UI cho portfolio
|   |   |-- discovery/                 # Logic và UI cho luồng quẹt khám phá
|   |   |-- location/                  # Logic xử lý vị trí và quyền truy cập
|   |   |-- swipe/                     # Logic gesture quẹt trái/phải
|   |   |-- match/                     # Logic kết nối sau khi hai bên quan tâm nhau
|   |   |-- chat/                      # Logic và UI cho nhắn tin
|   |   |-- booking/                   # Logic và UI cho đặt lịch
|   |   |-- review/                    # Logic review booking và rating public
|   |   |-- notification/              # Xử lý push/deep link; không có notification inbox
|   |   |-- report/                    # Logic và UI cho báo cáo vi phạm
|   |   |-- block/                     # Logic chặn và bỏ chặn người dùng
|   |   |-- restriction/               # Read model và màn hình penalty đang hiệu lực
|   |   |-- legal-consent/             # Legal document hiện hành và consent append-only
|   |   `-- settings/                  # Logic và UI cho cài đặt
|   |-- components/                    # Component dùng chung toàn ứng dụng
|   |   |-- ui/                        # Component UI cơ bản như button, input, modal
|   |   |-- layout/                    # Component bố cục như screen, container, section
|   |   |-- feedback/                  # Component trạng thái như loading, empty, error
|   |   `-- navigation/                # Component hỗ trợ navigation
|   |-- services/                      # Lớp giao tiếp hạ tầng và API
|   |   |-- api/                       # Cấu hình API client và kiểu response
|   |   |   |-- api-client.ts          # Client HTTP dùng chung cho backend
|   |   |   |-- api-error.ts           # Chuẩn hóa lỗi API
|   |   |   `-- api-response.ts        # Chuẩn hóa kiểu dữ liệu response
|   |   |-- auth-storage.service.ts    # Lưu và đọc token hoặc session cục bộ
|   |   |-- location.service.ts        # Xử lý quyền và dữ liệu vị trí thiết bị
|   |   |-- notification.service.ts    # Xử lý push notification và local notification
|   |   |-- upload.service.ts          # Xử lý upload ảnh và file
|   |   `-- websocket.service.ts       # Kết nối realtime hoặc websocket
|   |-- generated/                     # Mã sinh tự động, không chỉnh sửa thủ công
|   |   `-- api/                       # Client và type được sinh từ API spec
|   |       |-- types.ts               # TypeScript type sinh từ backend contract
|   |       `-- client.ts              # API client sinh tự động
|   |-- stores/                        # Store client state bằng Zustand
|   |   |-- auth.store.ts              # Trạng thái đăng nhập và session
|   |   |-- role.store.ts              # Trạng thái vai trò hiện tại
|   |   |-- discovery.store.ts         # Trạng thái bộ lọc và feed discovery
|   |   |-- location.store.ts          # Trạng thái vị trí và quyền vị trí
|   |   `-- settings.store.ts          # Trạng thái cài đặt cục bộ
|   |-- hooks/                         # React hooks dùng chung
|   |   |-- use-current-user.ts        # Hook lấy người dùng hiện tại
|   |   |-- use-current-role.ts        # Hook lấy vai trò hiện tại
|   |   |-- use-location-permission.ts # Hook kiểm tra và xin quyền vị trí
|   |   |-- use-network-status.ts      # Hook theo dõi trạng thái mạng
|   |   |-- use-debounce.ts            # Hook trì hoãn giá trị nhập liệu
|   |   `-- use-app-state.ts           # Hook theo dõi trạng thái foreground/background
|   |-- providers/                     # Provider bọc ứng dụng
|   |   |-- app-provider.tsx           # Provider tổng hợp cho toàn app
|   |   |-- auth-provider.tsx          # Provider quản lý auth session
|   |   |-- query-provider.tsx         # Provider cấu hình TanStack Query
|   |   |-- theme-provider.tsx         # Provider theme và appearance
|   |   `-- websocket-provider.tsx     # Provider quản lý kết nối realtime
|   |-- constants/                     # Hằng số dùng chung
|   |   |-- account-status.ts          # Trạng thái tài khoản
|   |   |-- booking-status.ts          # Trạng thái booking
|   |   |-- match-status.ts            # Trạng thái match
|   |   |-- notification-type.ts       # Loại thông báo
|   |   |-- query-keys.ts              # Khóa cache cho TanStack Query
|   |   |-- roles.ts                   # Vai trò trong hệ thống
|   |   |-- routes.ts                  # Tên route dùng chung
|   |   |-- storage-keys.ts            # Khóa lưu trữ local storage
|   |   `-- swipe-direction.ts         # Hằng số hướng quẹt
|   |-- schemas/                       # Schema validate dữ liệu
|   |   |-- common.schema.ts           # Schema dùng chung cho nhiều feature
|   |   `-- pagination.schema.ts       # Schema phân trang
|   |-- utils/                         # Hàm tiện ích thuần
|   |   |-- format-date.ts             # Định dạng ngày giờ
|   |   |-- format-distance.ts         # Định dạng khoảng cách hiển thị
|   |   |-- format-price.ts            # Định dạng giá tiền
|   |   |-- image.ts                   # Xử lý URL ảnh và metadata ảnh
|   |   `-- validation.ts              # Helper validate dữ liệu
|   |-- types/                         # TypeScript type dùng chung
|   |   |-- environment.ts             # Type cho biến môi trường
|   |   |-- file.ts                    # Type cho file upload và asset
|   |   `-- navigation.ts              # Type cho route params và navigation
|   |-- theme/                         # Token giao diện của ứng dụng
|   |   |-- colors.ts                  # Bảng màu dùng chung
|   |   |-- spacing.ts                 # Khoảng cách và kích thước chuẩn
|   |   |-- typography.ts              # Cấu hình font và text style
|   |   `-- index.ts                   # Export theme tổng hợp
|   `-- config/                        # Cấu hình runtime và app
|       |-- app.config.ts              # Cấu hình tên app, phiên bản và metadata
|       `-- env.ts                     # Đọc và validate biến môi trường
|-- assets/                            # Tài nguyên tĩnh của ứng dụng
|   |-- animations/                    # File animation như Lottie
|   |-- fonts/                         # Font chữ nhúng trong app
|   |-- icons/                         # Icon tĩnh hoặc asset icon riêng
|   `-- images/                        # Ảnh tĩnh dùng trong UI
|-- tests/                             # Bộ kiểm thử của ứng dụng mobile
|   |-- unit/                          # Unit test cho hàm và component nhỏ
|   |-- integration/                   # Integration test cho flow nhiều module
|   `-- e2e/                           # End-to-end test cho luồng người dùng chính
|-- scripts/                           # Script hỗ trợ phát triển và build
|   `-- generate-api-types.ts          # Script sinh type và client từ API spec
|-- .github/                           # Cấu hình GitHub cho mobile repo nếu tách riêng
|   `-- workflows/                     # Workflow CI/CD
|       |-- ci.yml                     # Workflow kiểm tra lint, test và typecheck
|       `-- build.yml                  # Workflow build ứng dụng mobile
|-- .env.example                       # Mẫu biến môi trường cho developer
|-- app.json                           # Cấu hình Expo dạng JSON
|-- eas.json                           # Cấu hình build và submit bằng EAS
|-- babel.config.js                    # Cấu hình Babel cho React Native và Expo
|-- metro.config.js                    # Cấu hình Metro bundler
|-- nativewind-env.d.ts                # Type declaration cho NativeWind
|-- tsconfig.json                      # Cấu hình TypeScript
|-- package.json                       # Khai báo dependency và script npm
`-- README.md                          # Tài liệu hướng dẫn chạy và phát triển mobile app
```

### Nguyên tắc mobile

Các file trong app/ chỉ chịu trách nhiệm:

- Khai báo route.
- Nhận route parameter.
- Kiểm tra authentication.
- Kiểm tra onboarding.
- Kiểm tra vai trò.
- Render feature screen.

Không đặt trực tiếp API call, schema hoặc business logic phức tạp trong route file.

### Cấu trúc một mobile feature

Ví dụ feature swipe:

```text
src/features/swipe/
├── api/
│   ├── create-swipe.ts
│   ├── get-swipe-candidates.ts
│   ├── get-incoming-interests.ts
│   └── decide-interest.ts
│
├── components/
│   ├── swipe-actions.tsx
│   ├── swipe-card.tsx
│   ├── swipe-card-stack.tsx
│   └── swipe-empty-state.tsx
│
├── hooks/
│   ├── use-create-swipe.ts
│   ├── use-swipe-candidates.ts
│   └── use-swipe-gesture.ts
│
├── schemas/
│   └── swipe-filter.schema.ts
│
├── types/
│   └── swipe.types.ts
│
├── utils/
│   └── calculate-swipe-direction.ts
│
├── screens/
│   └── discovery-screen.tsx
│
└── index.ts
```

## Web

| Thành phần | Công nghệ sử dụng |
| :---- | :---- |
| Framework | Next.js App Router, React |
| Language | TypeScript |
| UI styling | Tailwind CSS |
| Admin UI | Ant Design |
| Server state | TanStack Query |
| Form & validation | React Hook Form, Zod |
| Routing & middleware | Next.js App Router, middleware.ts |
| Auth | Admin auth provider, protected routes |
| API integration | Generated API client, admin API client |
| Monitoring & analytics | Sentry, PostHog hoặc Firebase Analytics |
| Build/deploy | Vercel |

```text
photomatch-web/                             # Thư mục gốc của ứng dụng web Photomatch
|-- src/                                    # Mã nguồn chính của web app
|   |-- app/                                # App Router của Next.js
|   |   |-- layout.tsx                      # Layout gốc cho toàn bộ web app
|   |   |-- globals.css                     # CSS global và khai báo Tailwind base
|   |   |-- not-found.tsx                   # Trang 404 dùng chung
|   |   |-- error.tsx                       # Trang lỗi dùng chung cho route segment
|   |   |-- admin/                          # Route khu vực quản trị
|   |   |   |-- layout.tsx                  # Layout ngoài cho admin
|   |   |   |-- sign-in/                    # Route đăng nhập admin
|   |   |   |   `-- page.tsx                # Trang đăng nhập admin
|   |   |   `-- (protected)/                # Nhóm route admin yêu cầu đăng nhập
|   |   |       |-- layout.tsx              # Layout bảo vệ và shell admin
|   |   |       |-- page.tsx                # Trang dashboard admin
|   |   |       |-- users/                  # Quản lý người dùng
|   |   |       |   |-- page.tsx            # Trang danh sách người dùng
|   |   |       |   `-- [userId]/           # Route chi tiết người dùng
|   |   |       |       `-- page.tsx        # Trang chi tiết người dùng
|   |   |       |-- photographers/          # Quản lý hồ sơ thợ ảnh
|   |   |       |   |-- page.tsx            # Trang danh sách thợ ảnh
|   |   |       |   `-- [photographerId]/   # Route chi tiết thợ ảnh
|   |   |       |       `-- page.tsx        # Trang chi tiết thợ ảnh
|   |   |       |-- reviews/                # Quản lý moderation review
|   |   |       |   |-- page.tsx            # Trang danh sách review
|   |   |       |   `-- [reviewId]/         # Route chi tiết review
|   |   |       |       `-- page.tsx        # Trang đổi trạng thái review
|   |   |       |-- reports/                # Quản lý báo cáo vi phạm
|   |   |       |   |-- page.tsx            # Trang danh sách report
|   |   |       |   `-- [reportId]/         # Route chi tiết report
|   |   |       |       `-- page.tsx        # Trang xử lý report
|   |   |       |-- penalties/              # Quản lý hình phạt tài khoản
|   |   |       |   |-- page.tsx            # Trang danh sách penalty
|   |   |       |   `-- [penaltyId]/        # Route chi tiết penalty
|   |   |       |       `-- page.tsx        # Trang chi tiết penalty
|   |   |       |-- bookings/               # Quản lý booking
|   |   |       |   |-- page.tsx            # Trang danh sách booking
|   |   |       |   `-- [bookingId]/        # Route chi tiết booking
|   |   |       |       `-- page.tsx        # Trang chi tiết booking
|   |   |       |-- activity-fields/        # Quản lý lĩnh vực hoạt động
|   |   |       |   `-- page.tsx            # Trang cấu hình lĩnh vực hoạt động
|   |   |       |-- services/               # Quản lý dịch vụ
|   |   |       |   `-- page.tsx            # Trang cấu hình dịch vụ
|   |   |       `-- legal-documents/        # Quản lý tài liệu pháp lý
|   |   |           `-- page.tsx            # Trang cập nhật điều khoản và chính sách
|   |   `-- api/                            # Route handler nội bộ của Next.js
|   |       `-- health/                     # Endpoint kiểm tra tình trạng web
|   |           `-- route.ts                # Handler trả về trạng thái health check
|   |-- features/                           # Module nghiệp vụ theo domain
|   |   `-- admin/                          # Feature cho admin dashboard
|   |       |-- auth/                       # Logic đăng nhập và phân quyền admin
|   |       |-- dashboard/                  # Logic và UI dashboard tổng quan
|   |       |-- users/                      # Logic quản lý người dùng
|   |       |-- photographers/              # Logic quản lý thợ ảnh
|   |       |-- reviews/                    # Logic moderation review
|   |       |-- reports/                    # Logic xử lý báo cáo
|   |       |-- penalties/                  # Logic xử lý khóa và phạt tài khoản
|   |       |-- bookings/                   # Logic quản lý booking
|   |       |-- activity-fields/            # Logic quản lý lĩnh vực hoạt động
|   |       |-- services/                   # Logic quản lý dịch vụ
|   |       `-- legal-documents/            # Logic quản lý tài liệu pháp lý
|   |-- components/                         # Component dùng chung
|   |   |-- ui/                             # Component UI cơ bản như button, input, badge
|   |   `-- admin/                          # Component dùng trong admin dashboard
|   |       |-- admin-header.tsx            # Header của admin dashboard
|   |       |-- admin-sidebar.tsx           # Sidebar điều hướng admin
|   |       |-- admin-breadcrumb.tsx        # Breadcrumb cho admin pages
|   |       |-- data-table/                 # Component bảng dữ liệu
|   |       |-- dialogs/                    # Dialog xác nhận và form modal
|   |       |-- feedback/                   # Component loading, empty và error state
|   |       `-- forms/                      # Component form dùng lại trong admin
|   |-- services/                           # Lớp giao tiếp API và service ngoài
|   |   |-- api/                            # Cấu hình API client
|   |   |   |-- api-client.ts               # Client API dùng chung
|   |   |   |-- admin-api-client.ts         # Client API kèm auth admin
|   |   |   |-- api-error.ts                # Chuẩn hóa lỗi API
|   |   |   `-- api-response.ts             # Chuẩn hóa response API
|   |   |-- auth.service.ts                 # Service đăng nhập và session admin
|   |   |-- analytics.service.ts            # Service tracking analytics
|   |   `-- upload.service.ts               # Service upload file
|   |-- generated/                          # Mã sinh tự động từ API spec
|   |   `-- api/                            # Type và client sinh tự động
|   |       |-- types.ts                    # TypeScript type từ backend contract
|   |       `-- client.ts                   # API client sinh tự động
|   |-- hooks/                              # React hooks dùng chung cho web
|   |   |-- use-current-admin.ts            # Hook lấy admin hiện tại
|   |   |-- use-debounce.ts                 # Hook debounce input
|   |   |-- use-pagination.ts               # Hook quản lý phân trang
|   |   `-- use-permission.ts               # Hook kiểm tra quyền admin
|   |-- providers/                          # Provider bọc ứng dụng web
|   |   |-- query-provider.tsx              # Provider TanStack Query
|   |   |-- theme-provider.tsx              # Provider theme sáng tối
|   |   `-- admin-auth-provider.tsx         # Provider session admin
|   |-- constants/                          # Hằng số dùng chung
|   |   |-- admin-navigation.ts             # Cấu hình menu admin
|   |   |-- permissions.ts                  # Danh sách quyền admin
|   |   |-- query-keys.ts                   # Khóa cache TanStack Query
|   |   |-- roles.ts                        # Vai trò trong hệ thống
|   |   `-- routes.ts                       # Hằng số route web
|   |-- schemas/                            # Schema validate dữ liệu
|   |   |-- pagination.schema.ts            # Schema phân trang
|   |   `-- search-params.schema.ts         # Schema query params và filter
|   |-- types/                              # TypeScript type dùng chung
|   |   |-- environment.ts                  # Type biến môi trường
|   |   |-- navigation.ts                   # Type điều hướng và route config
|   |   `-- table.ts                        # Type cho data table
|   |-- utils/                              # Hàm tiện ích
|   |   |-- download-file.ts                # Helper tải file từ trình duyệt
|   |   |-- format-date.ts                  # Định dạng ngày giờ
|   |   |-- format-status.ts                # Định dạng trạng thái hiển thị
|   |   `-- metadata.ts                     # Helper tạo metadata SEO
|   `-- config/                             # Cấu hình runtime của web
|       |-- env.ts                          # Đọc và validate biến môi trường
|       |-- metadata.ts                     # Cấu hình metadata mặc định
|       `-- navigation.ts                   # Cấu hình điều hướng admin
|-- public/                                 # Static assets public của Next.js
|   |-- fonts/                              # Font public nếu không bundle qua src
|   |-- icons/                              # Icon public
|   |-- images/                             # Ảnh public dùng trong admin
|   |-- logos/                              # Logo Photomatch
|   `-- manifest/                           # Manifest PWA và metadata asset
|-- tests/                                  # Bộ kiểm thử web
|   |-- unit/                               # Unit test cho hàm và component nhỏ
|   |-- integration/                        # Integration test cho flow admin
|   `-- e2e/                                # End-to-end test bằng browser
|-- scripts/                                # Script hỗ trợ phát triển
|   `-- generate-api-types.ts               # Script sinh type và client từ API spec
|-- .github/                                # Cấu hình GitHub cho web repo nếu tách riêng
|   `-- workflows/                          # Workflow CI/CD
|       |-- ci.yml                          # Workflow kiểm tra lint, test và typecheck
|       `-- deploy.yml                      # Workflow deploy web
|-- .env.example                            # Mẫu biến môi trường cho web
|-- components.json                         # Cấu hình shadcn/ui
|-- middleware.ts                           # Middleware auth, redirect hoặc locale
|-- next.config.ts                          # Cấu hình Next.js
|-- postcss.config.js                       # Cấu hình PostCSS
|-- tailwind.config.ts                      # Cấu hình Tailwind CSS
|-- tsconfig.json                           # Cấu hình TypeScript
|-- package.json                            # Dependency và script npm
`-- README.md                               # Tài liệu chạy và phát triển web app
```

## Backend

| Thành phần | Công nghệ sử dụng |
| :---- | :---- |
| Framework | NestJS |
| Language | TypeScript |
| ORM | Prisma |
| Database | PostgreSQL, PostGIS |
| Authentication | JWT, Guards, role-based permission |
| Cache/queue | Redis |
| Realtime | WebSocket module, custom WebSocket adapter |
| API contract | OpenAPI |
| File storage | Cloudflare R2, S3-compatible API |
| Background jobs | NestJS jobs module, scheduled/background processors |
| Containerization | Docker, Docker Compose |
| Reverse proxy & monitoring | Nginx, monitoring configuration |

```text
photomatch-api/                     # Thư mục gốc của backend API Photomatch
|-- src/                            # Mã nguồn chính của NestJS API
|   |-- main.ts                     # Entry point khởi động NestJS application
|   |-- app.module.ts               # Root module liên kết toàn bộ module backend
|   |-- config/                     # Cấu hình ứng dụng theo từng nhóm hạ tầng
|   |   |-- app.config.ts           # Cấu hình chung như port, environment và app metadata
|   |   |-- auth.config.ts          # Cấu hình JWT, session và chiến lược xác thực
|   |   |-- database.config.ts      # Cấu hình kết nối database
|   |   |-- email.config.ts         # Cấu hình provider gửi email
|   |   |-- notification.config.ts  # Cấu hình push notification
|   |   |-- redis.config.ts         # Cấu hình Redis cache hoặc queue
|   |   |-- storage.config.ts       # Cấu hình lưu trữ file và ảnh
|   |   `-- validation.ts           # Validate biến môi trường và cấu hình runtime
|   |-- common/                     # Thành phần dùng chung toàn backend
|   |   |-- constants/              # Hằng số dùng chung
|   |   |-- decorators/             # Custom decorator cho controller và guard
|   |   |-- dto/                    # DTO dùng chung giữa nhiều module
|   |   |-- enums/                  # Enum dùng chung trong domain
|   |   |-- exceptions/             # Custom exception chuẩn hóa lỗi nghiệp vụ
|   |   |-- filters/                # Exception filter xử lý lỗi response
|   |   |-- guards/                 # Guard xác thực và phân quyền
|   |   |-- interceptors/           # Interceptor logging, transform và response
|   |   |-- middleware/             # Middleware xử lý request trước controller
|   |   |-- pipes/                  # Pipe validate và transform dữ liệu
|   |   |-- serializers/            # Serializer chuẩn hóa dữ liệu trả về
|   |   |-- types/                  # TypeScript type dùng chung
|   |   `-- utils/                  # Helper thuần dùng chung
|   |-- database/                   # Tầng cấu hình database và dữ liệu khởi tạo
|   |   |-- database.module.ts      # Module cung cấp database client cho NestJS
|   |   |-- migrations/             # Migration bổ sung ngoài Prisma nếu cần
|   |   |-- seeds/                  # Seed data theo từng domain
|   |   |-- factories/              # Factory tạo dữ liệu test hoặc seed
|   |   `-- extensions/             # SQL extension và cấu hình database mở rộng
|   |       `-- postgis.sql         # Script bật và cấu hình PostGIS
|   |-- modules/                    # Module nghiệp vụ chính của hệ thống
|   |   |-- auth/                   # Đăng nhập, đăng ký, token và xác thực
|   |   |-- users/                  # Quản lý tài khoản người dùng
|   |   |-- roles/                  # Quản lý vai trò và quyền
|   |   |-- profiles/               # Quản lý hồ sơ cá nhân
|   |   |-- photographers/          # Quản lý hồ sơ thợ ảnh
|   |   |-- activity-fields/        # Quản lý lĩnh vực hoạt động
|   |   |-- services/               # Quản lý dịch vụ theo lĩnh vực
|   |   |-- portfolios/             # Quản lý portfolio của thợ ảnh
|   |   |-- locations/              # Quản lý vị trí và dữ liệu quanh đây
|   |   |-- discovery/              # API feed khám phá và đề xuất
|   |   |-- swipes/                 # Xử lý lượt quẹt trái/phải
|   |   |-- matches/                # Xử lý kết nối khi hai bên quan tâm nhau
|   |   |-- conversations/          # Quản lý cuộc trò chuyện
|   |   |-- messages/               # Quản lý tin nhắn
|   |   |-- blocks/                 # Chặn và bỏ chặn người dùng
|   |   |-- reports/                # Báo cáo vi phạm
|   |   |-- bookings/               # Đặt lịch và quản lý booking
|   |   |-- reviews/                # Đánh giá sau booking
|   |   |-- notifications/          # Push/outbox delivery, không có notification inbox API
|   |   |-- penalties/              # Khóa, phạt và hạn chế tài khoản
|   |   |-- legal-documents/        # Điều khoản, chính sách và tài liệu pháp lý
|   |   |-- consents/               # Consent append-only theo legal document version
|   |   |-- settings/               # Cài đặt hệ thống và người dùng
|   |   |-- uploads/                # Upload ảnh, file và asset
|   |   |-- devices/                # Quản lý thiết bị và push token
|   |   |-- admin/                  # Controller/read model/action cho web admin MVP
|   |   `-- health/                 # Health check cho API
|   |-- integrations/               # Tích hợp dịch vụ bên ngoài
|   |   |-- analytics/              # Tích hợp analytics
|   |   |-- email/                  # Tích hợp gửi email
|   |   |-- oauth-providers/        # Adapter xác minh Google/Apple identity assertion
|   |   |-- push-notification/      # Tích hợp gửi push notification
|   |   `-- storage/                # Tích hợp storage provider
|   |-- jobs/                       # Background jobs và scheduled tasks
|   |   |-- jobs.module.ts          # Module đăng ký queue và job processor
|   |   |-- account-penalty/        # Job xử lý hết hạn hoặc áp dụng penalty
|   |   |-- cleanup/                # Job dọn dữ liệu tạm và tài nguyên hết hạn
|   |   |-- location-expiration/    # Job hết hạn vị trí quanh đây
|   |   `-- notification/           # Job gửi notification bất đồng bộ
|   `-- websocket/                  # Realtime gateway và websocket adapter
|       |-- websocket.module.ts     # Module cấu hình websocket
|       |-- websocket-auth.guard.ts # Guard xác thực kết nối websocket
|       `-- websocket.adapter.ts    # Adapter websocket tùy chỉnh
|-- prisma/                         # Prisma schema, migration và seed chính
|   |-- schema.prisma               # Data model và Prisma datasource
|   |-- migrations/                 # Migration do Prisma quản lý
|   `-- seed.ts                     # Seed data chính cho môi trường dev hoặc staging
|-- test/                           # Bộ kiểm thử backend
|   |-- unit/                       # Unit test cho service, util và guard
|   |-- integration/                # Integration test giữa module và database
|   |-- e2e/                        # End-to-end test cho API endpoint
|   |-- fixtures/                   # Dữ liệu mẫu phục vụ test
|   `-- helpers/                    # Helper setup test và mock
|-- scripts/                        # Script vận hành và phát triển
|   |-- create-admin.ts             # Script tạo tài khoản admin đầu tiên
|   |-- export-openapi.ts           # Script xuất OpenAPI spec cho frontend
|   |-- migrate.ts                  # Script chạy migration có kiểm soát
|   `-- seed.ts                     # Script seed dữ liệu thủ công
|-- docs/                           # Tài liệu kỹ thuật của backend
|   |-- architecture/               # Tài liệu kiến trúc backend
|   |-- business-rules/             # Tài liệu business rule
|   `-- database/                   # Tài liệu database và ERD
|-- infrastructure/                 # Cấu hình hạ tầng triển khai
|   |-- docker/                     # Docker config phụ trợ
|   |-- nginx/                      # Cấu hình reverse proxy
|   `-- monitoring/                 # Cấu hình logging, metrics và alerting
|-- .github/                        # Cấu hình GitHub cho backend repo nếu tách riêng
|   `-- workflows/                  # Workflow CI/CD
|       |-- ci.yml                  # Workflow kiểm tra lint, test và build
|       `-- deploy.yml              # Workflow deploy backend
|-- .env.example                    # Mẫu biến môi trường cho backend
|-- Dockerfile                      # Docker image definition cho API
|-- docker-compose.yml              # Stack local gồm API, database, Redis và service phụ trợ
|-- nest-cli.json                   # Cấu hình Nest CLI
|-- tsconfig.build.json             # Cấu hình TypeScript khi build
|-- tsconfig.json                   # Cấu hình TypeScript cho development
|-- package.json                    # Dependency và script npm
`-- README.md                       # Tài liệu chạy và phát triển backend API
```

### Cấu trúc một backend module

Ví dụ module swipes:

```text
src/modules/swipes/
├── swipes.module.ts
├── swipes.controller.ts
├── swipes.service.ts
│
├── dto/
│   ├── create-swipe.dto.ts
│   ├── get-swipe-candidates.dto.ts
│   └── swipe-response.dto.ts
│
├── repositories/
│   ├── swipe.repository.ts
│   └── swipe.repository.interface.ts
│
├── policies/
│   └── swipe.policy.ts
│
├── validators/
│   └── swipe-target.validator.ts
│
├── events/
│   ├── mutual-interest-found.event.ts
│   └── swipe-created.event.ts
│
├── listeners/
│   └── create-match.listener.ts
│
├── mappers/
│   └── swipe.mapper.ts
│
├── enums/
│   └── swipe-direction.enum.ts
│
└── tests/
    ├── swipe.policy.spec.ts
    └── swipes.service.spec.ts
```

## Environment variables

### Mobile

```env
EXPO_PUBLIC_API_URL=             # Base URL của backend API cho mobile app
EXPO_PUBLIC_WEBSOCKET_URL=       # URL WebSocket hoặc realtime backend
EXPO_PUBLIC_SENTRY_DSN=          # DSN Sentry để ghi nhận lỗi mobile
EXPO_PUBLIC_POSTHOG_KEY=         # Project key PostHog cho analytics mobile
EXPO_PUBLIC_POSTHOG_HOST=        # Host PostHog khi dùng self-host hoặc region riêng
```

### Web

```env
NEXT_PUBLIC_API_URL=       # Base URL backend API mà web admin gọi tới
NEXT_PUBLIC_WEB_URL=       # URL public của web admin dùng cho redirect/callback
NEXT_PUBLIC_POSTHOG_KEY=   # Project key PostHog nếu bật analytics trên web
ADMIN_SESSION_COOKIE_NAME= # Tên cookie lưu session đăng nhập admin
SENTRY_DSN=                # DSN Sentry cho lỗi server-side hoặc SSR của web
```

### Backend

```env
NODE_ENV=               # Môi trường chạy API: development, staging hoặc production
PORT=                   # Port HTTP mà backend API lắng nghe
APP_URL=                # URL public của backend API
WEB_URL=                # URL web admin được phép redirect hoặc gọi callback
DATABASE_URL=           # Connection string PostgreSQL dùng bởi Prisma
REDIS_URL=              # Connection string Redis cho cache, queue hoặc rate limit
JWT_ACCESS_SECRET=      # Secret ký access token
JWT_REFRESH_SECRET=     # Secret ký refresh token
JWT_ACCESS_EXPIRES_IN=  # Thời gian sống của access token
JWT_REFRESH_EXPIRES_IN= # Thời gian sống của refresh token
R2_ENDPOINT=            # S3-compatible endpoint của Cloudflare R2
R2_ACCESS_KEY_ID=       # Access key id của R2 API token
R2_SECRET_ACCESS_KEY=   # Secret access key của R2 API token
R2_BUCKET=              # Bucket private lưu avatar, portfolio, chat và evidence
R2_SIGNED_URL_TTL_SEC=  # Thời hạn pre-signed upload/download URL
R2_PUBLIC_BASE_URL=     # Custom domain/public base URL nếu bật public media delivery
FCM_PROJECT_ID=         # Firebase project ID dùng gửi push notification
FCM_CLIENT_EMAIL=       # Client email của Firebase service account
FCM_PRIVATE_KEY=        # Private key của Firebase service account
SMTP_HOST=              # SMTP host để gửi email hệ thống
SMTP_PORT=              # SMTP port
SMTP_USER=              # SMTP username
SMTP_PASSWORD=          # SMTP password
SENTRY_DSN=             # DSN Sentry để ghi nhận lỗi backend
```

# CHI PHÍ VẬN HÀNH SẢN PHẨM

Ước tính dưới đây dùng cho MVP của Photomatch, chưa bao gồm nhân sự, marketing, thuế, phí thanh toán, chi phí pháp lý, SMS OTP, KYC provider, hoặc overage vượt quota. Giá là tham khảo theo USD và cần kiểm tra lại tại thời điểm thanh toán.

## Giả định MVP

- Mobile app dùng Expo/EAS để build, submit và update.
- Web chỉ phục vụ admin dashboard, không có landing page.
- Backend có API riêng, database PostgreSQL/PostGIS, Cloudflare R2 cho ảnh/file và Redis/cache/queue.
- Push notification dùng Firebase Cloud Messaging.
- Monitoring dùng Sentry; analytics dùng PostHog hoặc Firebase Analytics.
- Bộ lọc `Gần tôi` dùng Expo Location và backend PostGIS; mobile không tích hợp map provider hoặc tab Nearby.

## Chi phí ban đầu

| Hạng mục | Chi phí tham khảo | Ghi chú |
| :---- | :---- | :---- |
| Apple Developer Program | 99 USD/năm | Bắt buộc nếu phát hành iOS lên App Store/TestFlight production. |
| Google Play Console | 25 USD một lần | Bắt buộc nếu phát hành Android lên Google Play. |
| Domain | 10-30 USD/năm | Tùy tên miền và registrar. |
| Email tên miền | 0-20 USD/tháng | Có thể dùng email provider miễn phí/gói thấp ở MVP. |

## Chi phí vận hành hàng tháng

| Nhóm chi phí | Dịch vụ đề xuất | MVP tiết kiệm | MVP production | Ghi chú kiểm soát |
| :---- | :---- | :---- | :---- | :---- |
| Managed PostgreSQL/PostGIS | Neon, Render PostgreSQL, Railway PostgreSQL, Cloud SQL hoặc RDS | 0-30 USD | 30-150+ USD | Chọn PostgreSQL có PostGIS; tăng theo compute, storage, connection, backup/PITR. |
| Backend API compute | Cloud Run, Render, Railway, Fly.io hoặc VPS | 5-30 USD | 30-100 USD | Chọn 1 container nhỏ trước; scale theo CPU/RAM/request. |
| Redis/cache/queue | Upstash, Redis Cloud hoặc self-host | 0-10 USD | 10-30 USD | Dùng cho queue, rate limit, session/cache nếu backend cần. |
| Web admin hosting | Vercel | 0 USD | 20+ USD | Hobby đủ thử nghiệm; Pro phù hợp team và production. |
| Mobile build/update | Expo EAS | 0-19 USD | 19-199 USD | Starter đủ giai đoạn launch nhỏ; Production khi cần concurrency, quota update cao và support. |
| Push notification | Firebase Cloud Messaging | 0 USD | 0 USD | FCM là nhóm no-cost; chi phí phát sinh nếu dùng thêm paid Firebase/Google Cloud services. |
| Error monitoring | Sentry | 0 USD | 26-80+ USD | Free đủ dev; Team/Business khi cần team workflow, quota và retention tốt hơn. |
| Product analytics | PostHog hoặc Firebase Analytics | 0 USD | 0-50+ USD | PostHog có free tier lớn; chi phí tăng theo event/session replay/feature flag. |
| CI/CD repository | GitLab/GitHub | 0 USD | 0-50+ USD | Phát sinh khi vượt CI minutes, cần private runner hoặc thêm seat. |
| Email transactional | Resend, SendGrid, Postmark hoặc SMTP | 0-20 USD | 20-100 USD | Dùng cho verify email, forgot password, cảnh báo tài khoản. |
| Object storage/media | Cloudflare R2 | 0-25 USD | 25-100+ USD | Dùng S3-compatible API và pre-signed URL; portfolio ảnh là nguồn tăng storage chính, cần resize/compress và lifecycle cleanup. |

## Tổng chi phí dự kiến

| Giai đoạn | Chi phí/tháng tham khảo | Phù hợp khi |
| :---- | :---- | :---- |
| Development nội bộ | 0-40 USD/tháng | Chưa public, ít build, ít dữ liệu, dùng free tier. |
| MVP launch tiết kiệm | 50-150 USD/tháng | Có user thật, dùng managed PostgreSQL nhỏ, backend container nhỏ, web admin production, EAS Starter, monitoring cơ bản. |
| MVP production ổn định | 150-350 USD/tháng | Có traffic đều, cần EAS Production, Sentry paid và backend compute riêng. |
| Growth sau MVP | 400+ USD/tháng | Tăng MAU, nhiều ảnh portfolio và truy vấn `Gần tôi`, cần scale database/compute. |

## Hạng mục dễ phát sinh chi phí

- Portfolio ảnh làm tăng storage và bandwidth; cần giới hạn dung lượng upload, tạo thumbnail và nén ảnh.
- Sentry có thể tăng bill khi app lỗi hàng loạt; cần sampling, rate limit và alert theo quota.
- PostHog tăng theo event/session replay; chỉ tracking event thật sự cần cho quyết định sản phẩm.
- EAS Build phát sinh nếu build nhiều nhánh hoặc build lại liên tục; nên dùng preview build có kiểm soát.
- Database tăng theo connection, storage, backup, compute và truy vấn geospatial; cần index đúng và theo dõi slow query.

## Nguyên tắc kiểm soát ngân sách

- Đặt billing alert cho database provider, API hosting, object storage, Vercel, Google Cloud/Firebase, Sentry, PostHog và Expo.
- Tắt hoặc sampling session replay, performance tracing và analytics event không cần thiết.
- Resize ảnh phía client/server trước khi lưu storage.
- Dùng môi trường dev/staging tách biệt với production nhưng đặt quota thấp.
- Review chi phí mỗi tuần trong 1 tháng đầu sau launch, sau đó ít nhất mỗi tháng một lần.

## Nguồn giá tham khảo

- Vercel pricing: https://vercel.com/pricing
- Expo EAS pricing: https://expo.dev/pricing
- Firebase pricing: https://firebase.google.com/pricing
- Sentry pricing: https://sentry.io/pricing/
- PostHog pricing: https://posthog.com/pricing
- Neon pricing: https://neon.com/pricing
- Render pricing: https://render.com/pricing
- Cloudflare R2 pricing: https://www.cloudflare.com/developer-platform/products/r2/
- Apple Developer Program: https://developer.apple.com/programs/enroll/
- Google Play Console: https://support.google.com/googleplay/android-developer/answer/6112435
