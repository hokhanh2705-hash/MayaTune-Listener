---
title: "MayaTune Listener MVP"
subtitle: "Product Requirements Document"
date: "Phiên bản 0.1 · 17/08/2026"
lang: vi
---

> **Tuyên bố sản phẩm**  
> MayaTune giúp người nghe phổ thông biến âm nhạc thành một trải nghiệm thích nghi với sở thích, hoạt động và thời điểm nghe — mà không cần biết sản xuất âm nhạc.

| Thuộc tính | Giá trị |
|---|---|
| Trạng thái | Draft baseline — sẵn sàng cho review liên chức năng |
| Phạm vi | MayaTune Listener MVP |
| Use case ra mắt | Workout Personalization |
| Use case kế tiếp | Focus Personalization |
| Khả năng trọng tâm | Adjust, Transform / Music Style Transfer, Create New |
| Đối tượng đọc | Product, Design, Engineering, AI/ML, Data, Rights, Security, QA |

**North Star Metric:** Số phút nghe các phiên bản được cá nhân hóa và được người dùng chủ động chọn lại.

# 1. Kiểm soát tài liệu

## 1.1 Metadata

| Trường | Nội dung |
|---|---|
| Tên tài liệu | MayaTune Listener MVP — Product Requirements Document |
| Phiên bản | 0.1 |
| Ngày cập nhật | 17/08/2026 |
| Chủ sở hữu | MayaTune Product |
| Trạng thái | Draft baseline |
| Mức bảo mật | Nội bộ dự án |
| Tài liệu liên quan | `MayaTune_Project_Log.docx`, `README.md` |
| Bước kế tiếp sau PRD | UX wireframes và technical feasibility spike cho audio pipeline |

## 1.2 Lịch sử phiên bản

| Phiên bản | Ngày | Nội dung | Trạng thái |
|---|---|---|---|
| 0.1 | 17/08/2026 | Khóa baseline cho Listener MVP; bổ sung Music Style Transfer, user stories, requirements, analytics, data model, rollout và acceptance gates. | Hiện hành |

## 1.3 Vai trò review

| Nhóm | Trách nhiệm review | Điều kiện chấp thuận |
|---|---|---|
| Product | Scope, ưu tiên, metric, rollout | Không có mâu thuẫn giữa mục tiêu, phạm vi và success criteria |
| Design | Flow, màn hình, trạng thái, accessibility | Người dùng phổ thông có thể hoàn tất flow không cần thuật ngữ sản xuất nhạc |
| Engineering | Kiến trúc, API, dữ liệu, vận hành | Có thể chia thành epics và triển khai theo milestone |
| AI/ML & Audio | Analysis, generation, ranking, QC | Có phương pháp đo chất lượng và fallback rõ ràng |
| Rights & Trust | Quyền nguồn, derivative use, export, provenance | Không có flow xử lý audio trước khi rights policy cho phép |
| Security & Privacy | Lưu trữ, xóa dữ liệu, quyền truy cập | Dữ liệu audio và hành vi nghe có kiểm soát truy cập, retention và audit |
| QA | Acceptance criteria và test matrix | P0 có testable criteria, dữ liệu test và expected outcome |

## 1.4 Quy ước ưu tiên

| Mức | Ý nghĩa | Tiêu chí |
|---|---|---|
| P0 | Bắt buộc cho Closed Beta / MVP | Thiếu chức năng sẽ phá vỡ giá trị cốt lõi, quyền hoặc khả năng vận hành |
| P1 | Nên có trong MVP hoặc ngay sau beta | Tăng retention và hoàn thiện trải nghiệm nhưng có thể rollout sau P0 |
| P2 | Roadmap sau MVP | Chưa cần để kiểm chứng giả thuyết Listener-first |

## 1.5 Giả định làm việc của PRD v0.1

Các giả định dưới đây giúp đội ngũ có một baseline để thiết kế và ước lượng. Chúng chưa thay thế quyết định kinh doanh, pháp lý hoặc technical spike.

| ID | Giả định | Trạng thái |
|---|---|---|
| A-001 | Prototype và Closed Beta dùng trải nghiệm mobile-first trên responsive web/PWA; native app có thể theo sau. | Cần xác nhận |
| A-002 | Giao diện ra mắt ưu tiên tiếng Việt, nhưng schema và UI phải i18n-ready. | Cần xác nhận |
| A-003 | Pilot catalog có khoảng 50–200 track với quyền cá nhân hóa rõ ràng; ưu tiên track có stems. | Phụ thuộc đối tác |
| A-004 | First playable preview có mục tiêu tạm thời p50 ≤ 90 giây và p95 ≤ 240 giây. | Cần technical spike |
| A-005 | Preview dùng pipeline chi phí thấp; high-quality render chỉ chạy sau khi người dùng chọn. | Đề xuất |
| A-006 | Workout là funnel kiểm chứng đầu tiên; Focus được mở sau khi P0 ổn định. | Đã khóa |
| A-007 | Music Style Transfer chỉ P0 cho track có quyền derivative/transform hoặc file người dùng có quyền tương ứng. | Đã khóa về sản phẩm; cần rà soát hợp đồng |

# 2. Tóm tắt điều hành

## 2.1 Vấn đề

Người nghe phổ thông có rất nhiều nhạc nhưng ít khả năng làm cho bài hát phù hợp với hoàn cảnh cụ thể. Một bài họ yêu thích có thể có intro quá dài cho buổi chạy, năng lượng không đủ, vocal gây phân tâm khi làm việc hoặc thể loại không phù hợp với tâm trạng hiện tại. Các công cụ sản xuất nhạc truyền thống quá phức tạp; các công cụ generative thường bắt đầu từ prompt trắng và không hiểu đủ sâu sở thích đã hình thành của người dùng.

MayaTune giải quyết khoảng trống giữa **nghe thụ động** và **sản xuất âm nhạc chuyên nghiệp** bằng một trải nghiệm điều khiển đơn giản, có quyền rõ ràng và học dần từ hành vi.

## 2.2 Giải pháp

Người dùng chọn một bối cảnh, một nguồn nhạc hợp lệ và mô tả điều họ muốn bằng ngôn ngữ tự nhiên hoặc preset. MayaTune phân tích track, tạo một Edit Recipe có thể kiểm tra, sinh ba ứng viên và cho phép nghe A/B:

- **Familiar:** thay đổi nhẹ, ưu tiên cảm giác quen thuộc.
- **Personal:** tối ưu theo yêu cầu hiện tại và Preference Profile.
- **Explore:** sáng tạo hơn nhưng vẫn tuân thủ preserve locks, quyền và quality gates.

Sản phẩm hỗ trợ ba khả năng nền tảng:

| Khả năng | Mục đích | Ví dụ |
|---|---|---|
| **Adjust** | Điều chỉnh thuộc tính nhưng không thay đổi đáng kể bản sắc hoặc thể loại. | Tăng energy, rút intro, giảm vocal, tăng bass, đổi thời lượng. |
| **Transform** | Tạo arrangement hoặc ngữ cảnh sử dụng mới; bao gồm **Music Style Transfer**. | Pop → Rock, Ballad → EDM, Hip-hop → Jazz, acoustic, instrumental, workout. |
| **Create New** | Tạo tác phẩm độc lập từ Preference Profile hoặc thuộc tính trừu tượng. | Giữ mood và mức năng lượng nhưng tạo melody, lời, hook và cấu trúc mới. |

## 2.3 Giả thuyết MVP

> Nếu MayaTune có thể tạo ba preview có chất lượng chấp nhận được từ một track được cấp quyền, trong thời gian chờ hợp lý, và để người nghe chọn bằng các điều khiển dễ hiểu, thì người dùng sẽ lưu hoặc nghe lại ít nhất một phiên bản cá nhân hóa thay vì chỉ tạo thử một lần.

## 2.4 Wedge ra mắt

**Workout Personalization** là use case đầu tiên vì nhu cầu có thể chuyển thành tham số rõ ràng: BPM, energy curve, beat strength, intro length, chorus timing, duration và transition. P0 cũng gồm Music Style Transfer cho track đủ quyền, vì đây là biểu hiện mạnh nhất của giá trị “bài hát thích nghi với bạn”.

## 2.5 Phạm vi baseline

| Hạng mục | Baseline v0.1 |
|---|---|
| Người dùng | Người nghe phổ thông, không yêu cầu kiến thức sản xuất nhạc |
| Nguồn audio | Catalog được cấp quyền; file người dùng sở hữu/quản lý; reference-only cho nguồn khác |
| Bối cảnh P0 | Workout |
| Bối cảnh P1 | Focus |
| Đầu ra | Ba preview, một bản được chọn, Personal Station, controlled export khi quyền cho phép |
| Cá nhân hóa | Pairwise onboarding + Preference Profile + ranking từ hành vi |
| Style Transfer | Giữ các thành phần được khóa và chuyển arrangement/genre cho nguồn đủ quyền |
| Creator Voice | Ngoài phạm vi Listener MVP |

# 3. Mục tiêu, non-goals và thước đo thành công

## 3.1 Mục tiêu sản phẩm

| ID | Mục tiêu | Kết quả mong đợi |
|---|---|---|
| G-001 | Cho người dùng đạt “first personalized value” nhanh | Hoàn tất onboarding, chọn track và nghe preview đầu tiên trong một phiên |
| G-002 | Làm điều khiển âm nhạc dễ hiểu | Người dùng hoàn tất flow bằng bối cảnh, preset và ngôn ngữ tự nhiên thay vì thuật ngữ DAW |
| G-003 | Chứng minh giá trị của ba ứng viên | Người dùng có thể phân biệt, chọn và giải thích ngắn gọn vì sao thích một ứng viên |
| G-004 | Kiểm chứng Music Style Transfer | Tạo được chuyển thể loại thuyết phục mà vẫn giữ các thành phần được khóa |
| G-005 | Học sở thích theo bối cảnh | Lựa chọn sau có xếp hạng tốt hơn lựa chọn đầu theo từng context |
| G-006 | Vận hành rights-by-design | Không có generation hoặc export vượt quá quyền đã ghi nhận |
| G-007 | Xây nền tảng mở rộng | Recipe, job, analysis, rights và adapter đủ tổng quát cho Focus và Creator roadmap |

## 3.2 Kết quả người dùng

- Có một phiên bản phù hợp hơn với hoạt động hiện tại.
- Không cần hiểu BPM, stems hoặc hòa âm để sử dụng.
- Biết hệ thống đang giữ nguyên và thay đổi thành phần nào.
- Có thể nghe so sánh, hoàn tác, tạo lại và lưu công thức.
- Không bị đánh lừa về quyền sử dụng hoặc khả năng xuất file.
- Có thể xóa nguồn, phiên bản và dữ liệu cá nhân hóa liên quan.

## 3.3 Non-goals

| ID | Không thuộc MVP |
|---|---|
| NG-001 | Remix không giới hạn mọi bài hát thương mại trên thị trường |
| NG-002 | Tải trực tiếp audio từ dịch vụ streaming để chỉnh sửa khi chưa có quyền |
| NG-003 | Sao chép lời, melody, hook, bản ghi hoặc giọng từ nguồn reference-only |
| NG-004 | Prompt “giống hệt nghệ sĩ X” hoặc mô phỏng giọng người khác |
| NG-005 | DAW đầy đủ với timeline đa track, MIDI editor và plugin ecosystem |
| NG-006 | Social feed công khai cho remix hoặc phát hành thương mại một chạm |
| NG-007 | Marketplace mua bán voice model |
| NG-008 | Huấn luyện foundation model bằng audio người dùng theo mặc định |
| NG-009 | Bảo đảm thành công nghệ thuật cho mọi cặp thể loại hoặc mọi chất lượng file |
| NG-010 | Native iOS/Android bắt buộc cho Closed Beta nếu responsive web đáp ứng thử nghiệm |

## 3.4 Metric tree

**North Star Metric:** số phút nghe các phiên bản được cá nhân hóa và được người dùng chủ động chọn lại.

| Lớp | Metric |
|---|---|
| Acquisition | Người bắt đầu onboarding; nguồn traffic; chi phí mời beta |
| Activation | Hoàn tất onboarding; chọn context; generation đầu tiên; first playable preview |
| Value | Tỷ lệ chọn một trong ba ứng viên; nghe ≥ 70% bản được chọn; lưu station |
| Retention | Nghe lại trong 7/28 ngày; quay lại cùng station; tạo trên track thứ hai |
| Quality | Audio artifact rate; regenerate rate; “quá khác” / “chưa đủ khác” |
| Rights & Trust | Job bị chặn đúng; export có provenance; deletion hoàn tất |
| Economics | GPU seconds/job; cost/accepted version; cost/personalized listening minute |

## 3.5 Success thresholds tạm thời

Các ngưỡng là baseline để thiết kế alpha; phải hiệu chỉnh sau technical spike và user test.

| Metric | Mục tiêu alpha/beta | Loại gate |
|---|---|---|
| Generation job technical success | ≥ 95% với bộ track chuẩn | Go/no-go kỹ thuật |
| Rights decision có snapshot và audit event | 100% generation/export | Go/no-go trust |
| First preview latency | p50 ≤ 90 giây; p95 ≤ 240 giây | Mục tiêu tạm thời |
| Người nhận preview chọn ít nhất một ứng viên | ≥ 50% | Product signal |
| Người chọn nghe ≥ 70% phiên bản | ≥ 35% | Value signal |
| Lưu station hoặc nghe lại trong 7 ngày | ≥ 20% | Retention signal |
| Báo lỗi artifact nghiêm trọng | ≤ 10% job thành công | Quality gate |
| Xóa dữ liệu theo yêu cầu | 100% trong SLA nội bộ được định nghĩa | Privacy gate |

# 4. Người dùng và use cases

## 4.1 Persona chính — Everyday Active Listener

| Thuộc tính | Mô tả |
|---|---|
| Độ tuổi định hướng | Khoảng 18–35; không phải ràng buộc cứng |
| Hành vi | Nghe nhạc hằng ngày khi tập, đi lại, học hoặc làm việc |
| Kỹ năng | Không biết hoặc không muốn dùng công cụ sản xuất nhạc |
| Nhu cầu | Nhạc phù hợp hơn với từng hoạt động mà không phải tìm playlist thủ công |
| Friction | Intro dài, energy không đúng, vocal quá nhiều, playlist thiếu nhất quán |
| Tiêu chí tin tưởng | Nghe thử trước, kiểm soát mức thay đổi, biết quyền và có thể xóa dữ liệu |

## 4.2 Persona thứ hai — Focus Listener

Người dùng cần nhạc ít gây phân tâm, có mức năng lượng ổn định và thời lượng phù hợp cho một phiên làm việc. Persona này sử dụng cùng nền tảng nhưng ưu tiên giảm vocal, transition mượt và ít biến động hơn Workout.

## 4.3 Job to be Done

> Khi chuẩn bị thực hiện một hoạt động, tôi muốn âm nhạc tự thích nghi với trạng thái và sở thích của mình để không phải tìm kiếm hoặc chỉnh playlist thủ công.

### JTBD phụ

- Khi một bài hát yêu thích không phù hợp với hoàn cảnh hiện tại, tôi muốn chuyển nó sang phiên bản phù hợp mà vẫn giữ phần tôi yêu thích.
- Khi muốn khám phá phong cách mới, tôi muốn nghe bài quen thuộc dưới một thể loại khác trước khi tạo hoặc tìm bài hoàn toàn mới.
- Khi hệ thống tạo nhiều phương án, tôi muốn hiểu khác biệt và chọn nhanh mà không cần thuật ngữ kỹ thuật.
- Khi sử dụng file cá nhân, tôi muốn biết file được lưu, xử lý và xóa như thế nào.

## 4.4 Context matrix

| Context | Mục tiêu | Điều chỉnh mặc định | Trạng thái |
|---|---|---|---|
| Workout | Tăng động lực, nhịp rõ, energy tiến triển | BPM/energy tăng, intro ngắn, beat mạnh, chorus sớm | P0 |
| Focus | Giảm phân tâm, ổn định | Vocal thấp, transition mượt, energy ít biến động | P1 |
| Commute | Liền mạch, vừa năng lượng | Loudness ổn định, intro/outro vừa phải | P2 |
| Relax | Nhẹ, ít kích thích | Tempo/energy thấp, nhạc cụ mềm | P2 |
| Party | Năng lượng cao, hook rõ | Beat/bass mạnh, transition ngắn | P2 |

## 4.5 User stories cấp sản phẩm

| ID | User story | Ưu tiên |
|---|---|---|
| US-001 | Là người dùng mới, tôi muốn thiết lập sở thích bằng lựa chọn A/B để không phải hiểu thuật ngữ âm nhạc. | P0 |
| US-002 | Tôi muốn chọn Workout và thời lượng dự kiến để MayaTune biết mục tiêu nghe. | P0 |
| US-003 | Tôi muốn chọn một bài trong catalog có quyền rõ ràng và biết mình được phép làm gì. | P0 |
| US-004 | Tôi muốn tải file mình có quyền sử dụng và khai báo loại quyền. | P0 |
| US-005 | Tôi muốn đọc AI Music Brief trước khi tạo để biết hệ thống hiểu bài như thế nào. | P0 |
| US-006 | Tôi muốn nói “mạnh hơn, intro ngắn hơn” và thấy yêu cầu được chuyển thành các thay đổi rõ ràng. | P0 |
| US-007 | Tôi muốn chuyển một bài Pop thành Rock hoặc Ballad thành EDM trong khi giữ lời và melody chính khi quyền cho phép. | P0 |
| US-008 | Tôi muốn khóa vocal, lyrics, melody, hook hoặc structure trước khi transform. | P0 |
| US-009 | Tôi muốn nghe Familiar, Personal và Explore để so sánh. | P0 |
| US-010 | Tôi muốn yêu cầu “gần bản gốc hơn”, “mạnh hơn” hoặc “ít vocal hơn” mà không bắt đầu lại. | P0 |
| US-011 | Tôi muốn lưu phiên bản và recipe thành Workout Station. | P0 |
| US-012 | Tôi muốn hệ thống ưu tiên lựa chọn phù hợp hơn dựa trên hành vi trước. | P1 |
| US-013 | Tôi muốn tạo bài hoàn toàn mới từ sở thích khi nguồn chỉ là reference. | P1 |
| US-014 | Tôi muốn xuất file khi quyền cho phép và hiểu rõ giới hạn sử dụng. | P0 |
| US-015 | Tôi muốn xóa track nguồn, phiên bản hoặc toàn bộ dữ liệu liên quan. | P0 |
| US-016 | Tôi muốn nhận thông báo và hướng xử lý khi generation thất bại hoặc quyền không đủ. | P0 |

# 5. Phạm vi MVP và chiến lược phát hành

## 5.1 Ba khả năng nền tảng

### Adjust

Giữ bản sắc và thể loại tương đối ổn định trong khi thay đổi tempo, energy, intro/outro, vocal presence, bass, drums, duration hoặc structure emphasis.

### Transform — bao gồm Music Style Transfer

Giữ các thành phần được người dùng khóa và được quyền cho phép, đồng thời thay đổi arrangement, harmony treatment, groove, rhythm, instrumentation, sound design, production style và có thể cả thể loại.

### Create New

Tạo một tác phẩm độc lập dựa trên Preference Profile, context hoặc thuộc tính trừu tượng. Create New không giữ nguyên lời, melody, hook, bản ghi hoặc giọng của track reference-only.

## 5.2 Release slices

| Slice | Phạm vi | Mục tiêu |
|---|---|---|
| P0-A — Foundation | Account, onboarding, catalog, rights snapshot, analysis, playback | Có nguồn hợp lệ và hồ sơ sở thích |
| P0-B — Workout Adjust | Prompt/preset, recipe, ba ứng viên, compare, feedback | Chứng minh flow cá nhân hóa cơ bản |
| P0-C — Style Transfer | Target genre, preserve locks, transform intensity, QC | Chứng minh Transform có giá trị khác biệt |
| P0-D — Save & Trust | Station, version history, controlled export, deletion, provenance | Hoàn thiện vòng giá trị và niềm tin |
| P1-A — Focus | Focus preset, vocal reduction/instrumental, smoother transitions | Mở context thứ hai |
| P1-B — Create New | Original generation từ profile/reference attributes | Mở giá trị khi không có quyền derivative |
| P2 — Adaptive Sessions | Multi-track session, continuous energy curve, native apps | Mở rộng sau khi MVP được kiểm chứng |

## 5.3 Nguồn audio và quyền thao tác

| Rights mode | Analyze | Adjust | Style Transfer | Create New từ thuộc tính | Export |
|---|---:|---:|---:|---:|---:|
| Licensed catalog — derivative allowed | Có | Có | Có | Có | Theo giấy phép |
| User-owned / authorized upload | Có | Có | Có nếu declaration phù hợp | Có | Theo declaration và policy |
| Reference-only | Chỉ thuộc tính cần thiết | Không | Không | Có | Chỉ output độc lập |
| Rights unknown / rejected | Không hoặc metadata-only | Không | Không | Không dùng audio | Không |

## 5.4 In scope P0

- Đăng ký, đăng nhập và guest-to-account conversion trước khi lưu/export.
- Pairwise onboarding để khởi tạo Preference Profile.
- Catalog có badge quyền rõ ràng.
- Upload file có declaration và validation.
- Chọn Workout, thời lượng và cường độ.
- Track analysis và AI Music Brief.
- Prompt/preset → Edit Recipe có thể xem và chỉnh.
- Adjust cơ bản.
- Music Style Transfer với target genre và preserve locks.
- Ba ứng viên Familiar, Personal, Explore.
- A/B listening, feedback nhanh, regenerate và version history.
- Personal Station.
- Controlled export khi policy cho phép.
- Analytics, cost tracking, audit, deletion và support diagnostics.

## 5.5 Out of scope P0

- Upload URL hoặc rip audio từ streaming.
- User-to-user public sharing.
- Collaborative editing.
- Commercial distribution workflow.
- Full multi-track DAW.
- Creator voice cloning.
- Real-time live style transfer.
- Guaranteed transformation cho mọi cặp genre.
- Offline generation trên thiết bị.

# 6. Trải nghiệm cốt lõi

## 6.1 End-to-end flow

```text
Khởi tạo tài khoản / guest
        ↓
Pairwise onboarding
        ↓
Chọn Workout + thời lượng
        ↓
Chọn track có quyền / upload / reference-only
        ↓
Rights eligibility check
        ↓
Track Analysis + AI Music Brief
        ↓
Chọn Adjust hoặc Transform
        ↓
Prompt/preset + preserve locks + intensity
        ↓
Preview Edit Recipe
        ↓
Generate Familiar / Personal / Explore
        ↓
Nghe A/B + feedback + regenerate
        ↓
Lưu version / Personal Station
        ↓
Controlled export hoặc tiếp tục nghe trong app
```

## 6.2 Onboarding

Onboarding phải mất ít thao tác, ưu tiên lựa chọn âm thanh hơn biểu mẫu. Người dùng nghe các cặp đoạn ngắn và chọn phiên bản phù hợp hơn. Hệ thống tạo profile ban đầu nhưng cho phép bỏ qua từng câu.

**P0 dimensions:** energy, vocal presence, bass strength, intro tolerance, familiarity/novelty và Workout BPM range.

**Fallback:** nếu người dùng bỏ qua toàn bộ, dùng profile trung tính theo context và học từ lượt chọn đầu tiên.

## 6.3 Chọn context

Màn hình Context Picker hiển thị Workout là lựa chọn chính. Người dùng chọn:

- loại buổi tập;
- thời lượng dự kiến;
- energy mong muốn;
- có muốn energy tăng dần hay ổn định;
- mức thay đổi: gần gốc, cân bằng, khám phá.

## 6.4 Chọn nguồn

Nguồn được phân thành ba tab: **MayaTune Catalog**, **My Uploads**, **Use as Reference**. Mỗi track hiển thị badge cho biết chức năng được phép: Adjust, Style Transfer, Export.

Không cho phép người dùng bắt đầu generation trước khi policy engine trả về quyền thao tác tại thời điểm đó.

## 6.5 AI Music Brief

Brief diễn giải track bằng ngôn ngữ phổ thông:

> “Bài có energy trung bình, 96 BPM, vocal nổi bật và chorus bắt đầu ở 00:47. Với Workout, MayaTune đề xuất intro 8 giây, energy tăng dần và beat mạnh hơn.”

Brief phải cho phép người dùng sửa các nhận định có ảnh hưởng trực tiếp đến recipe, nhưng không biến thành màn hình kỹ thuật quá phức tạp.

## 6.6 Edit Recipe Preview

Trước khi tạo, MayaTune hiển thị tóm tắt:

- điều gì được giữ nguyên;
- điều gì sẽ thay đổi;
- target context/genre;
- mức sáng tạo;
- thời lượng ước tính;
- quyền nghe, lưu và export;
- cảnh báo chất lượng hoặc giới hạn nếu có.

## 6.7 Generation và progress

Generation là asynchronous job. UI phải:

- xác nhận job đã nhận;
- hiển thị các bước Analyze → Arrange → Render → Quality Check;
- cho phép rời màn hình và quay lại;
- phát ứng viên ngay khi từng ứng viên sẵn sàng, không chờ cả ba;
- có retry/fallback khi một candidate thất bại;
- không trừ credit hoặc quota cho lỗi hệ thống theo policy kinh doanh sau này.

## 6.8 Compare và feedback

Compare Player phải hỗ trợ:

- chuyển giữa ba ứng viên ở cùng timestamp hợp lý;
- hiển thị khác biệt bằng ngôn ngữ đơn giản;
- like/dislike;
- “gần gốc hơn”, “mạnh hơn”, “ít vocal hơn”, “giữ chorus này”;
- chọn ứng viên làm base cho lần tiếp theo;
- nghe source khi quyền cho phép;
- hiển thị warning nếu preview có chất lượng thấp.

## 6.9 Save và Personal Station

Khi người dùng chọn phiên bản, họ có thể:

- lưu version;
- đặt tên station;
- lưu recipe;
- áp dụng recipe cho track khác đủ quyền;
- xem version history;
- xóa hoặc restore trong thời gian retention được định nghĩa.

# 7. Music Style Transfer — đặc tả trọng tâm

## 7.1 Định nghĩa chuẩn

**Music Style Transfer** là khả năng giữ lại các thành phần được khóa của một bài hát được cấp quyền — chẳng hạn lời, giai điệu chính, vocal, hook hoặc cấu trúc — đồng thời tái tạo arrangement, hòa âm, nhịp điệu, groove, nhạc cụ, sound design và production style để chuyển bài sang một thể loại hoặc phong cách khác.

Ví dụ:

| Bản nguồn | Thể loại đích | Preserve mặc định | Thay đổi chính |
|---|---|---|---|
| Pop | Rock | Lyrics, melody, vocal, hook | Live drums, guitars, bass, rock dynamics |
| Ballad | EDM | Lyrics, melody, vocal, verse/chorus | 124 BPM, build-up, drop, synth bass, electronic drums |
| Hip-hop | Jazz | Lyrics/flow, vocal, structure | Swing groove, upright bass, piano/horns, reharmonization có kiểm soát |
| Acoustic | Lo-fi | Melody, vocal, structure | Dusty drums, warm keys, texture, lower energy |

## 7.2 Preserve Locks

| Lock | Ý nghĩa | P0 default |
|---|---|---|
| Lyrics Lock | Giữ nội dung lời và thứ tự câu | Bật nếu quyền cho phép |
| Main Melody Lock | Giữ đường nét melody vocal chính trong dung sai | Bật |
| Vocal Lock | Giữ bản thu vocal gốc hoặc stem vocal | Bật khi stem/quyền/chất lượng cho phép |
| Hook Lock | Giữ hook nhận diện được cấp quyền | Bật |
| Structure Lock | Giữ verse/chorus/bridge và thứ tự tương đối | Bật |
| Harmony Lock | Giữ progression; chỉ đổi voicing/instrumentation | Tắt mặc định; người dùng có thể bật |
| Duration Lock | Giữ thời lượng gần nguồn | Tắt; context có thể thay đổi |

Mỗi lock phải có trạng thái: `available`, `unavailable`, `required_by_policy`, `selected`, `not_selected`.

## 7.3 Transform Controls

- Target genre/style.
- Transform intensity: Light, Balanced, Strong.
- Target BPM hoặc Auto for context.
- Energy curve.
- Instrumentation preference.
- Harmony change: low/medium/high, chỉ khi quyền và lock cho phép.
- Vocal treatment: original, cleaned, background, instrumental — tùy quyền.
- Structure adaptation: none, context-optimized, genre-optimized.
- Production era/mood bằng descriptor trừu tượng; không nhận tên nghệ sĩ làm mục tiêu sao chép.

## 7.4 Candidate behavior

| Candidate | Style intensity | Preserve priority | Mục tiêu |
|---|---|---|---|
| Familiar | Light | Cao nhất | Cho thấy genre mới nhưng vẫn rất quen |
| Personal | Balanced | Cân bằng | Tối ưu theo context và profile |
| Explore | Strong | Vẫn tuân thủ locks | Thể hiện rõ target genre và arrangement mới |

## 7.5 Eligibility và rights gating

Style Transfer chỉ khả dụng khi:

1. `rights_state` cho phép tạo derivative/arrangement;
2. quyền đối với composition, lyrics, master/vocal và export được xác định theo operation;
3. user upload có declaration phù hợp và không bị policy hold;
4. target use không vượt phạm vi giấy phép;
5. rights snapshot được lưu vào generation job.

Nếu chỉ có quyền composition nhưng không có quyền master/vocal, hệ thống phải vô hiệu hóa Vocal Lock và giải thích lựa chọn thay thế. Nếu nguồn chỉ reference-only, nút Style Transfer không được hiển thị như một hành động khả dụng; hệ thống chuyển sang Create New.

## 7.6 Recipe schema tối thiểu

```json
{
  "operation": "style_transfer",
  "source_track_id": "track_123",
  "rights_snapshot_id": "rights_456",
  "context": "workout",
  "source_style": "pop_ballad",
  "target_style": "edm",
  "transform_intensity": 0.65,
  "preserve": {
    "lyrics": true,
    "main_melody": true,
    "vocal": true,
    "hook": true,
    "structure": true,
    "harmony": false
  },
  "target_bpm": 124,
  "energy_curve": "progressive",
  "instrumentation": ["electronic_drums", "synth_bass", "pads"],
  "structure_adaptation": "context_optimized",
  "candidate_profiles": ["familiar", "personal", "explore"]
}
```

## 7.7 Quality rubric

Mỗi candidate Style Transfer được chấm theo các chiều:

| Chiều | Câu hỏi kiểm tra |
|---|---|
| Lock adherence | Lời, melody, vocal, hook và structure được giữ trong dung sai đã định nghĩa? |
| Genre recognizability | Người nghe có nhận ra target genre mà không cần xem nhãn? |
| Musical coherence | Harmony, rhythm, arrangement và transition có tự nhiên? |
| Audio quality | Có clipping, phasing, warble, timing drift hoặc vocal artifact nghiêm trọng? |
| Context fit | Phiên bản có phù hợp Workout/Focus và duration mong muốn? |
| Personal fit | Candidate ranking có phản ánh profile và feedback? |
| Rights compliance | Operation và output có nằm trong rights snapshot? |

Candidate không vượt quality threshold sẽ không hiển thị như kết quả “hoàn tất”; hệ thống retry, thay đổi strategy hoặc trả về partial success có giải thích.

## 7.8 Acceptance criteria Style Transfer

| ID | Tiêu chí nghiệm thu |
|---|---|
| ST-AC-001 | Người dùng chỉ thấy Style Transfer là enabled khi rights policy cho phép operation. |
| ST-AC-002 | UI hiển thị rõ target genre, transform intensity và mọi preserve lock trước khi tạo. |
| ST-AC-003 | Recipe lưu chính xác lock state, rights snapshot và model/pipeline version. |
| ST-AC-004 | Với bộ test được cấp quyền, ≥ 90% candidate hiển thị không vi phạm lock bắt buộc. |
| ST-AC-005 | Ít nhất hai trong ba candidate phải vượt quality threshold hoặc job được đánh dấu partial/failed. |
| ST-AC-006 | A/B player cho phép chuyển candidate mà không mất vị trí nghe vượt quá dung sai UX. |
| ST-AC-007 | Người dùng có thể tạo lại từ một candidate và thay đổi đúng một control mà không mất các lock khác. |
| ST-AC-008 | Export bị chặn khi rights snapshot không cho phép, kể cả generation đã hoàn tất. |
| ST-AC-009 | Reference-only source không thể đi vào Style Transfer; flow đề xuất Create New. |
| ST-AC-010 | Hệ thống ghi quality scores, user feedback, cost và latency cho từng candidate. |

## 7.9 Edge cases và fallback

- **Không có stem vocal:** tắt Vocal Lock hoặc dùng strategy giữ mixed vocal nếu policy và chất lượng cho phép.
- **Tempo target gây méo vocal:** giới hạn target BPM, dùng tempo map hoặc khuyến nghị giữ BPM gần nguồn.
- **Genre pair chưa được hỗ trợ:** chỉ hiển thị genre có model capability; không nhận yêu cầu rồi thất bại âm thầm.
- **Lyrics alignment lỗi:** chặn candidate nếu word timing drift vượt threshold.
- **Một candidate thất bại:** phát các candidate đã sẵn sàng và cho retry candidate còn thiếu.
- **Rights thay đổi giữa lúc tạo và export:** export re-check dùng rights state hiện tại, không chỉ snapshot cũ.
- **User yêu cầu tên nghệ sĩ:** chuyển thành descriptor trung tính hoặc yêu cầu chọn thuộc tính âm thanh, không mô phỏng nghệ sĩ.

# 8. Functional requirements

## 8.1 Epic A — Account và onboarding

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-ONB-001 | Cho phép guest bắt đầu onboarding và nghe sample. | P0 | Guest có session ID; dữ liệu được chuyển vào account khi đăng ký. |
| FR-ONB-002 | Hỗ trợ đăng ký/đăng nhập và khôi phục tài khoản. | P0 | Người dùng truy cập lại library và jobs trên thiết bị khác. |
| FR-ONB-003 | Pairwise onboarding tối thiểu 4 dimension. | P0 | Mỗi câu có audio A/B, skip và replay. |
| FR-ONB-004 | Sinh Preference Profile ban đầu. | P0 | Profile có version, source signals và confidence. |
| FR-ONB-005 | Cho phép sửa hoặc reset sở thích. | P0 | Reset không xóa library; tạo audit event. |
| FR-ONB-006 | Hiển thị consent cho analytics và personalization cần thiết. | P0 | Consent state được lưu và có thể thay đổi. |
| FR-ONB-007 | Không yêu cầu chọn thể loại quá chi tiết để hoàn tất. | P0 | Có neutral/default path. |
| FR-ONB-008 | Hỗ trợ onboarding lại theo context. | P1 | Workout và Focus có sub-profile riêng. |

## 8.2 Epic B — Catalog, upload và rights

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-LIB-001 | Catalog hiển thị rights badges theo operation. | P0 | Track card nêu Adjust, Transform, Export availability. |
| FR-LIB-002 | Search/filter theo genre, mood, BPM range và quyền. | P0 | Kết quả cập nhật và giữ filter state. |
| FR-LIB-003 | Upload các định dạng audio được hỗ trợ. | P0 | Validate type, size, duration, corruption và malware policy. |
| FR-LIB-004 | Thu rights declaration trước khi analysis nâng cao. | P0 | Không tạo job nếu declaration thiếu. |
| FR-LIB-005 | Lưu rights snapshot cho mỗi generation. | P0 | Snapshot immutable và liên kết job/output. |
| FR-LIB-006 | Hỗ trợ reference-only mode. | P1 | Chỉ trích thuộc tính cần thiết và không mở Adjust/Transform. |
| FR-LIB-007 | Cho phép xóa source và derived assets. | P0 | UI mô tả tác động; backend thực thi theo retention policy. |
| FR-LIB-008 | Re-check rights trước export. | P0 | Export denied có reason code và hướng xử lý. |

## 8.3 Epic C — Context và Workout setup

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-CTX-001 | Workout là context mặc định được ưu tiên. | P0 | Có entry point rõ trên home. |
| FR-CTX-002 | Người dùng chọn target duration. | P0 | Recipe nhận duration hoặc session goal. |
| FR-CTX-003 | Chọn energy curve: steady/progressive/interval. | P0 | Preview summary phản ánh lựa chọn. |
| FR-CTX-004 | Chọn familiarity level. | P0 | Mapping vào candidate ranking/creativity. |
| FR-CTX-005 | Lưu context settings thành preset cá nhân. | P1 | Có thể rename, edit, delete. |
| FR-CTX-006 | Focus context dùng cùng framework. | P1 | Có vocal and distraction controls. |

## 8.4 Epic D — Track analysis và AI Music Brief

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-AN-001 | Phân tích BPM, key, loudness, sections, energy và instrumentation. | P0 | Analysis có confidence và pipeline version. |
| FR-AN-002 | Tách stem khi operation yêu cầu và capability cho phép. | P0 | Stem status được lưu; không giả định stem luôn khả dụng. |
| FR-AN-003 | Tạo waveform và section markers. | P0 | Player hiển thị markers nhất quán. |
| FR-AN-004 | Tạo AI Music Brief dễ hiểu. | P0 | Brief tránh thuật ngữ không giải thích. |
| FR-AN-005 | Người dùng có thể báo analysis sai. | P1 | Feedback liên kết analysis field. |
| FR-AN-006 | Cache analysis theo source fingerprint. | P0 | Không chạy lại không cần thiết; vẫn kiểm tra rights riêng. |

## 8.5 Epic E — Intent và Edit Recipe

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-INT-001 | Nhận prompt tiếng Việt và preset. | P0 | Intent parser trả structured recipe hoặc clarification UI. |
| FR-INT-002 | Hiển thị recipe summary trước generation. | P0 | User thấy giữ/thay đổi/context/rights. |
| FR-INT-003 | Cho phép sửa control mà không sửa JSON. | P0 | UI cập nhật recipe deterministically. |
| FR-INT-004 | Validate recipe với capability và rights. | P0 | Invalid field có reason và suggestion. |
| FR-INT-005 | Version recipe sau mỗi regenerate. | P0 | Có parent_recipe_id và diff. |
| FR-INT-006 | Lưu user-friendly label cho recipe. | P1 | Recipe xuất hiện trong Station/History. |

## 8.6 Epic F — Adjust

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-ADJ-001 | Điều chỉnh energy, intro, duration và drum intensity. | P0 | Output nằm trong supported range và recipe. |
| FR-ADJ-002 | Điều chỉnh vocal presence khi stem/capability cho phép. | P0 | Disabled state có giải thích nếu không khả dụng. |
| FR-ADJ-003 | Điều chỉnh tempo trong quality-safe range. | P0 | UI cảnh báo hoặc clamp range. |
| FR-ADJ-004 | Giữ source identity theo Familiar profile. | P0 | Quality rubric có identity score. |
| FR-ADJ-005 | Hỗ trợ instrumental nếu quyền và stem cho phép. | P1 | Không để vocal residue vượt threshold định nghĩa. |

## 8.7 Epic G — Transform / Style Transfer

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-TR-001 | Chọn target genre từ danh sách capability-supported. | P0 | Danh sách phụ thuộc source/capability/rights. |
| FR-TR-002 | Hiển thị và quản lý Preserve Locks. | P0 | Lock state đầy đủ, không mất khi regenerate. |
| FR-TR-003 | Chọn transform intensity. | P0 | Familiar/Personal/Explore có mapping rõ. |
| FR-TR-004 | Chọn hoặc auto target BPM/energy. | P0 | Recipe và summary hiển thị giá trị. |
| FR-TR-005 | Tạo ba candidate với arrangement khác nhau. | P0 | Candidate IDs, lineage và scores được lưu. |
| FR-TR-006 | Chặn target style không được hỗ trợ. | P0 | Không tạo job doomed-to-fail; có alternative. |
| FR-TR-007 | Kiểm tra lock adherence và audio quality. | P0 | Candidate fail không được trình bày như success. |
| FR-TR-008 | Re-check rights trước save/share/export. | P0 | Policy decision có timestamp và reason. |
| FR-TR-009 | Cho phép base-on-candidate iteration. | P0 | Child version giữ lineage và selected locks. |
| FR-TR-010 | Thu genre-recognition feedback. | P1 | Người dùng có thể chọn “không giống target style”. |

## 8.8 Epic H — Create New

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-NEW-001 | Tạo bài độc lập từ Preference Profile + context. | P1 | Output không dùng protected components từ reference-only. |
| FR-NEW-002 | Cho phép reference attributes như mood/BPM/energy. | P1 | UI giải thích nguồn chỉ dùng để hiểu thuộc tính. |
| FR-NEW-003 | Chạy similarity safeguards. | P1 | Candidate vượt threshold được regenerate hoặc review. |
| FR-NEW-004 | Ghi provenance cho prompt/profile/model. | P1 | Metadata gắn output. |

## 8.9 Epic I — Generation jobs và quality control

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-GEN-001 | Job chạy asynchronous và resumable. | P0 | Refresh/đổi màn hình không mất trạng thái. |
| FR-GEN-002 | Candidate sẵn sàng được phát ngay. | P0 | Partial results có trạng thái rõ. |
| FR-GEN-003 | Có progress stages và ETA dạng khoảng, không hứa chính xác. | P0 | UI không bị treo khi worker retry. |
| FR-GEN-004 | Retry theo lỗi có phân loại. | P0 | Không retry vô hạn; có dead-letter path. |
| FR-GEN-005 | Quality checks gồm clipping, loudness, timing, artifact, lock adherence. | P0 | Scores và failure reasons được lưu. |
| FR-GEN-006 | Cost telemetry theo stage/model/candidate. | P0 | Có cost dashboard nội bộ. |
| FR-GEN-007 | Generation Adapter tách provider/model khỏi product API. | P0 | Có contract test cho adapter. |
| FR-GEN-008 | Hỗ trợ cancel khi stage cho phép. | P1 | UI và billing/quota phản ánh đúng. |

## 8.10 Epic J — Compare, feedback và personalization

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-CMP-001 | A/B switch giữa candidates. | P0 | Playback position được map trong dung sai. |
| FR-CMP-002 | Hiển thị khác biệt chính bằng copy dễ hiểu. | P0 | Không chỉ hiển thị thông số kỹ thuật. |
| FR-CMP-003 | Thu like/dislike và quick refinements. | P0 | Event có candidate, recipe, context. |
| FR-CMP-004 | Chọn candidate làm accepted version. | P0 | Cập nhật job outcome và profile signal. |
| FR-CMP-005 | Ranking dùng Preference Profile và quality score. | P1 | Không xếp candidate fail quality lên đầu. |
| FR-CMP-006 | Cho phép undo feedback gần nhất. | P1 | Profile signal được đảo đúng. |
| FR-CMP-007 | Giải thích ngắn “vì sao đề xuất bản này”. | P1 | Dùng signal category, không lộ dữ liệu nhạy cảm. |

## 8.11 Epic K — Playback, save, station và export

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-OUT-001 | Streaming playback cho preview và accepted version. | P0 | Signed URL/token có thời hạn. |
| FR-OUT-002 | Lưu accepted version và recipe. | P0 | Version xuất hiện trong Library. |
| FR-OUT-003 | Tạo Personal Station từ context + recipe. | P0 | Station có name, context, default settings. |
| FR-OUT-004 | Version history và lineage. | P0 | User xem source → recipe → candidate → accepted. |
| FR-OUT-005 | Controlled export theo rights. | P0 | Export file chứa provenance metadata khả dụng. |
| FR-OUT-006 | Hiển thị usage terms trước download. | P0 | User acknowledge khi policy yêu cầu. |
| FR-OUT-007 | Sharing private link. | P2 | Chỉ triển khai sau rights review. |

## 8.12 Epic L — Privacy, settings và support

| ID | Requirement | Pri | Acceptance |
|---|---|---:|---|
| FR-PRI-001 | Cho phép xem/xóa source, version, station và profile. | P0 | Xóa có confirmation và audit. |
| FR-PRI-002 | Không dùng upload để train model chung theo mặc định. | P0 | Training consent là lựa chọn riêng. |
| FR-PRI-003 | Export dữ liệu tài khoản ở mức khả dụng. | P1 | Có job và thông báo hoàn tất. |
| FR-PRI-004 | Support diagnostic ID cho job lỗi. | P0 | User copy được ID, không lộ secret. |
| FR-PRI-005 | Preference privacy controls. | P1 | User tắt một số behavioral signals. |
| FR-PRI-006 | Account deletion. | P0 | Tạo deletion workflow và retention exceptions hợp lệ. |

# 9. UX và màn hình

## 9.1 Information architecture

```text
Home
├── Start a Workout Mix
├── Continue Station
├── Library
│   ├── Catalog
│   ├── My Uploads
│   └── Saved Versions
├── Create
│   ├── Adjust
│   ├── Change Style
│   └── Create New (P1)
├── Jobs / Activity
└── Profile & Privacy
```

## 9.2 Screen inventory

| ID | Màn hình | Mục tiêu | Trạng thái bắt buộc |
|---|---|---|---|
| S-01 | Welcome / Auth | Bắt đầu nhanh, guest hoặc account | default, loading, auth error |
| S-02 | Pairwise Onboarding | Khởi tạo profile | audio loading, skip, complete |
| S-03 | Home | Entry vào Workout và recent items | empty, returning, job in progress |
| S-04 | Context Setup | Chọn duration/energy/familiarity | valid, incomplete, preset loaded |
| S-05 | Source Picker | Catalog/upload/reference | search, filter, no rights, upload error |
| S-06 | Track Detail & Rights | Hiểu track và thao tác khả dụng | rights allowed/limited/blocked |
| S-07 | AI Music Brief | Xác nhận analysis | ready, low confidence, analysis failed |
| S-08 | Adjust / Change Style Studio | Chọn controls và locks | capability disabled, warning, valid |
| S-09 | Recipe Review | Xem giữ/thay đổi/quyền | ready, policy conflict, cost/quota warning |
| S-10 | Generation Progress | Theo dõi job | queued, running, partial, failed, canceled |
| S-11 | Compare Player | Nghe và chọn candidates | 1/2/3 ready, quality warning, source unavailable |
| S-12 | Refinement Sheet | Quick feedback/regenerate | valid, conflicting control |
| S-13 | Save to Station | Lưu recipe/version | new, existing station, naming conflict |
| S-14 | Library / Version History | Quản lý assets | empty, processing, expired preview |
| S-15 | Export & Rights | Download theo quyền | allowed, limited, denied, rights changed |
| S-16 | Privacy & Data | Consent, deletion, export data | confirmation, pending deletion |

## 9.3 UX principles

- **Context first:** hỏi người dùng muốn làm gì trước khi hỏi thông số.
- **Plain language:** “mạnh hơn”, “vào bài nhanh hơn”, “giữ giọng hát” thay vì chỉ “transient”, “LUFS”, “stem”.
- **Progressive disclosure:** slider kỹ thuật nằm sau advanced controls.
- **Explain constraints:** disabled control luôn có lý do và lựa chọn thay thế.
- **Trust before action:** rights và preserve summary xuất hiện trước generation/export.
- **Compare, do not guess:** luôn cho nghe nhiều candidate khi chi phí cho phép.
- **Recoverable:** mọi refinement tạo version mới; không phá hủy bản đã chọn.

## 9.4 Accessibility

- Mọi control có label, value và keyboard navigation.
- Không dùng màu là tín hiệu duy nhất cho rights/status.
- Transcript/lyrics hiển thị khi quyền cho phép; không bắt buộc để vận hành audio.
- Player có focus state, shortcut được công bố và target size phù hợp mobile.
- Motion/progress animation tôn trọng reduced-motion preference.
- Copy lỗi có hành động khắc phục, không chỉ error code.
- Contrast và text scaling phải đạt chuẩn accessibility mục tiêu của sản phẩm.

## 9.5 Copy guidelines

| Tránh | Dùng |
|---|---|
| “Stem separation failed” | “MayaTune chưa tách được giọng hát đủ sạch cho tùy chọn này.” |
| “Derivative rights missing” | “Bài này chưa được cấp quyền để đổi thể loại.” |
| “Similarity threshold exceeded” | “Bản mới còn quá gần một mẫu tham chiếu; MayaTune đang tạo phương án khác.” |
| “Generation error” | “Không thể hoàn tất bản này. Hai bản còn lại vẫn có thể nghe.” |

# 10. AI/ML và audio requirements

## 10.1 Pipeline logic

```text
Ingest
  → Rights pre-check
  → Audio normalization & fingerprint
  → Analysis / stem capability
  → Intent-to-Recipe
  → Capability + rights validation
  → Candidate planning
  → Generation / transformation
  → Mixing & mastering
  → Quality + lock adherence + similarity checks
  → Ranking
  → Preview delivery
  → Feedback learning
```

## 10.2 Analysis outputs

| Output | Mục đích | Yêu cầu |
|---|---|---|
| BPM / beat grid | Tempo, sync, workout mapping | Có confidence và downbeat alignment |
| Key / harmony | Transpose, arrangement | Không dùng như sự thật tuyệt đối khi confidence thấp |
| Structure | Intro, verse, chorus, bridge, outro | Time ranges, confidence, user correction path |
| Energy curve | Context adaptation | Chuẩn hóa theo track và catalog |
| Vocal/instrumentation | Locks và controls | Capability state, không giả định stem hoàn hảo |
| Loudness / peaks | QC và consistent playback | Lưu measurement version |
| Embedding/fingerprint | Search, cache, safeguards | Access-controlled và purpose-limited |

## 10.3 Intent-to-Recipe

Parser phải:

1. nhận tiếng Việt tự nhiên và preset;
2. trích context, preserve intent, target style, intensity, duration và constraints;
3. không tự bật operation trái quyền;
4. trả confidence và danh sách ambiguity;
5. ưu tiên UI choice thay vì hội thoại dài khi thiếu một trường;
6. lưu input, normalized intent và recipe version cho debugging có kiểm soát.

## 10.4 Candidate planning

Candidate planner không chỉ thay một tham số creativity. Mỗi profile có strategy:

- **Familiar:** lock adherence cao, arrangement change vừa đủ, tempo gần nguồn.
- **Personal:** tối ưu context/profile, chấp nhận structure adaptation vừa phải.
- **Explore:** target genre rõ, instrumentation mạnh hơn, nhưng không được vi phạm required locks.

## 10.5 Ranking

Ranking score có thể kết hợp:

```text
candidate_score =
  quality_score
  × rights_validity
  + context_fit
  + preference_fit
  + requested_change_adherence
  + diversity_bonus
  - artifact_penalty
  - policy_risk
```

`rights_validity` là hard gate, không phải trọng số có thể bù trừ. Candidate không hợp lệ không được xếp hạng cho người dùng.

## 10.6 Quality gates

P0 quality checks:

- clipping/peak;
- loudness range;
- severe separation artifact;
- vocal timing drift;
- beat/grid discontinuity;
- empty/silent output;
- lock adherence;
- duration tolerance;
- target genre recognizability proxy;
- corrupted file;
- provenance completeness.

## 10.7 Model/provider abstraction

Generation Adapter tối thiểu:

```text
analyze(track, operation_context)
separate_stems(track, requested_stems)
plan_candidates(analysis, recipe, profile)
generate_variant(track, recipe, candidate_profile)
master(version, target_profile)
score_quality(source, version, recipe)
score_similarity(reference_set, version)
```

Mỗi adapter phải trả capability metadata, latency, cost, model version, failure reason và reproducibility fields ở mức nhà cung cấp hỗ trợ.

## 10.8 Feedback learning

- P0: rule-based profile update và contextual ranking.
- P1: learning-to-rank khi đủ dữ liệu và consent.
- Không dùng một skip đơn lẻ để thay đổi mạnh profile.
- Tách global preference và context-specific preference.
- Có decay hoặc confidence để preference cũ không khóa trải nghiệm mãi mãi.

# 11. Quyền, an toàn và riêng tư

## 11.1 Rights-by-design

Mọi operation phải đi qua Rights Policy Engine trước generation và trước export. Policy decision gồm:

- source asset;
- user/account;
- operation;
- target use;
- territory/time nếu áp dụng;
- composition/master/lyrics/vocal permissions;
- output restrictions;
- decision, reason và policy version.

## 11.2 User uploads

Upload flow phải thu declaration rõ ràng, không dùng một checkbox mơ hồ. Tối thiểu:

- “Tôi sở hữu hoặc kiểm soát quyền cần thiết.”
- “Tôi có giấy phép cho phép chỉnh sửa/tạo derivative.”
- “Tôi chỉ muốn dùng làm reference thuộc tính.”
- “Tôi không chắc” → không mở generation có rủi ro.

Declaration không tự động chứng minh quyền. Hệ thống có thể áp dụng hold, giới hạn export hoặc yêu cầu tài liệu bổ sung theo policy.

## 11.3 Reference-only safeguards

- Chỉ trích thuộc tính cần thiết và được policy cho phép.
- Không lưu hoặc tái sử dụng component nhận diện ngoài mục đích đã công bố.
- Create New phải sinh lyrics/melody/hook/arrangement độc lập.
- Similarity checks và review path cho candidate rủi ro.
- UI không gọi output là “remix” của bài reference-only.

## 11.4 Prohibited requests

- Mô phỏng giọng người khác hoặc nghệ sĩ nổi tiếng.
- Tạo bản “giống hệt” một nghệ sĩ cụ thể.
- Giữ nguyên protected components khi quyền không cho phép.
- Dùng output để gây hiểu nhầm là bản phát hành chính thức.
- Bỏ qua provenance hoặc rights restrictions.
- Upload nội dung bất hợp pháp hoặc xâm phạm quyền theo policy áp dụng.

## 11.5 Provenance

Mỗi output lưu tối thiểu:

- source asset/fingerprint;
- recipe và lineage;
- rights snapshot/policy version;
- model/pipeline version;
- timestamp;
- user/workspace;
- quality scores;
- AI-assisted disclosure state;
- export restrictions.

## 11.6 Data handling

- Mã hóa dữ liệu khi truyền và lưu.
- Object access qua signed URL/token ngắn hạn.
- Tách quyền truy cập production audio khỏi quyền quản trị thông thường.
- Audit truy cập nhạy cảm.
- Không train model chung bằng upload theo mặc định.
- Có deletion workflow, retention schedule và exception record.
- Behavioral data được purpose-bound cho personalization/analytics theo consent.

## 11.7 Trust UX

User phải biết:

- vì sao một chức năng bị khóa;
- file nào đang được xử lý;
- output được phép nghe/lưu/export như thế nào;
- dữ liệu nào đang cải thiện personalization;
- cách xóa dữ liệu hoặc báo cáo vấn đề quyền.

# 12. Data model

## 12.1 Core entities

| Entity | Vai trò | Trường quan trọng |
|---|---|---|
| User | Chủ tài khoản | id, locale, consent_state, status |
| PreferenceProfile | Sở thích toàn cục/context | dimensions, confidence, version, signals |
| Track | Đơn vị nội dung logic | title, artist/owner metadata, source_type |
| SourceAsset | File audio gốc | storage_key, fingerprint, duration, format |
| RightsDeclaration | Tuyên bố người dùng/đối tác | basis, scope, evidence_state |
| RightsSnapshot | Quyền tại thời điểm operation | operation, decision, restrictions, policy_version |
| TrackAnalysis | Kết quả phân tích | bpm, key, sections, energy, stems, confidence |
| EditRecipe | Ý định có cấu trúc | operation, context, locks, controls, version |
| GenerationJob | Tiến trình async | state, stages, model, cost, errors |
| CandidateVersion | Mỗi Familiar/Personal/Explore | profile, asset, quality scores, lineage |
| Feedback | Tín hiệu người dùng | action, candidate, context, timestamp |
| PersonalStation | Recipe/context được lưu | name, defaults, accepted_version |
| Export | File xuất và quyền | format, policy decision, provenance, status |
| AuditEvent | Dấu vết trust/ops | actor, action, object, reason, timestamp |

## 12.2 Relationship summary

```text
User 1──N PreferenceProfile
User 1──N Track / PersonalStation
Track 1──N SourceAsset / TrackAnalysis / EditRecipe
EditRecipe 1──N GenerationJob
GenerationJob 1──N CandidateVersion
CandidateVersion 1──N Feedback / Export
RightsSnapshot N──1 Track or SourceAsset
Every sensitive operation 1──N AuditEvent
```

## 12.3 Job state machine

```text
created
  → rights_check
  → queued
  → analyzing
  → planning
  → generating
  → rendering
  → quality_check
  → partial_ready / ready
  → accepted / archived

Any active state → cancel_requested → canceled
Any active state → retryable_failed → queued
Any active state → terminal_failed
```

## 12.4 Rights state examples

```text
allowed
allowed_with_restrictions
reference_only
pending_review
expired
revoked
denied
```

## 12.5 Preference dimensions P0

| Dimension | Global | Context-specific | Signal examples |
|---|---:|---:|---|
| Energy | Có | Có | A/B choice, selected candidate |
| BPM range | Không bắt buộc | Có | Workout setup, completion |
| Vocal presence | Có | Có | slider, “less vocal”, skip |
| Bass/drum intensity | Có | Có | pairwise choice, accepted version |
| Intro tolerance | Có | Có | “chorus sooner”, seek behavior |
| Novelty tolerance | Có | Có | Familiar vs Explore choice |
| Preferred duration | Không | Có | session goal, replay |

# 13. API và service contracts

## 13.1 Endpoint baseline

| Method | Endpoint | Mục đích |
|---|---|---|
| POST | `/v1/tracks` | Tạo track/upload session |
| POST | `/v1/tracks/{id}/rights-declarations` | Khai báo quyền |
| POST | `/v1/tracks/{id}/analyze` | Khởi tạo/reuse analysis |
| GET | `/v1/tracks/{id}` | Track, capability và rights summary |
| POST | `/v1/recipes` | Tạo/validate Edit Recipe |
| POST | `/v1/generation-jobs` | Tạo job và ba candidate profiles |
| GET | `/v1/generation-jobs/{id}` | Progress, candidates, errors |
| POST | `/v1/candidates/{id}/feedback` | Like/refinement/accept |
| POST | `/v1/candidates/{id}/iterate` | Tạo child recipe/job |
| POST | `/v1/stations` | Lưu Personal Station |
| GET | `/v1/users/me/preferences` | Đọc profile đã tổng hợp |
| PATCH | `/v1/users/me/preferences` | Sửa/reset dimensions |
| POST | `/v1/exports` | Controlled export |
| DELETE | `/v1/assets/{id}` | Xóa asset theo policy |

## 13.2 API principles

- Idempotency key cho create job/export.
- Async operation trả job ID ngay.
- Errors có `code`, `user_message`, `retryable`, `support_id`.
- Không trả signed URL lâu dài.
- Version API và recipe schema.
- Rights decision không được override từ client.
- Candidate metadata không làm lộ internal safety thresholds không cần thiết.

## 13.3 Error taxonomy tối thiểu

| Code | Ý nghĩa | UX |
|---|---|---|
| `RIGHTS_OPERATION_DENIED` | Operation không được phép | Giải thích và đề xuất Create New/reference mode |
| `CAPABILITY_UNAVAILABLE` | Model/pipeline không hỗ trợ | Đề xuất style/control khác |
| `SOURCE_AUDIO_INVALID` | File lỗi/không hỗ trợ | Hướng dẫn upload lại |
| `QUALITY_GATE_FAILED` | Candidate không đạt chất lượng | Retry hoặc partial result |
| `JOB_CAPACITY_DELAY` | Hàng đợi quá tải | Hiển thị trạng thái, không mất job |
| `EXPORT_RECHECK_FAILED` | Quyền thay đổi hoặc restriction | Chặn download, nêu bước tiếp theo |

# 14. Analytics và experimentation

## 14.1 Event taxonomy

| Event | Khi nào | Properties chính |
|---|---|---|
| `onboarding_started` | Bắt đầu onboarding | source, locale, guest/account |
| `preference_pair_answered` | Chọn A/B/skip | dimension, choice, latency |
| `onboarding_completed` | Profile tạo xong | answered_count, profile_confidence |
| `context_selected` | Chọn Workout/Focus | context, duration, energy_curve |
| `source_selected` | Chọn track | source_type, rights_mode, capabilities |
| `upload_completed` | Upload thành công | format, duration_bucket, declaration_type |
| `rights_decision_made` | Policy trả quyết định | operation, decision, policy_version |
| `analysis_completed` | Analysis sẵn sàng | latency, cache_hit, confidence |
| `music_brief_viewed` | Xem brief | fields, low_confidence_flags |
| `operation_selected` | Adjust/Transform/Create New | operation |
| `preserve_lock_changed` | Bật/tắt lock | lock, state, reason |
| `target_style_selected` | Chọn style | source_style, target_style |
| `recipe_validated` | Recipe hợp lệ/lỗi | errors, warnings, operation |
| `generation_started` | Tạo job | job_id, candidate_profiles, model_route |
| `candidate_ready` | Mỗi candidate sẵn sàng | profile, latency, quality_score, cost |
| `generation_completed` | Job ready/partial/failed | outcome, candidate_count, total_cost |
| `candidate_played` | Play candidate | profile, start_position |
| `candidate_switch` | A/B switch | from, to, position_delta |
| `candidate_feedback` | Like/refinement | action, reason, profile |
| `candidate_accepted` | Chọn bản | profile, listen_ratio, iteration_count |
| `station_saved` | Lưu station | context, recipe_version |
| `version_replayed` | Nghe lại | days_since_create, source_surface |
| `export_requested` | Yêu cầu export | format, rights_state |
| `export_completed` | Export xong/bị chặn | outcome, reason |
| `asset_deleted` | Xóa dữ liệu | asset_type, cascade_count |

## 14.2 Funnels

### Activation funnel

```text
Onboarding started
→ Onboarding completed
→ Context selected
→ Source selected
→ Recipe validated
→ First candidate ready
→ Candidate accepted
```

### Retention funnel

```text
Candidate accepted
→ Station saved
→ Version/station replayed within 7 days
→ Second track personalized
→ Context-specific repeat usage
```

### Style Transfer funnel

```text
Transform selected
→ Target style selected
→ Preserve locks confirmed
→ Rights allowed
→ Candidate ready
→ Target style recognized
→ Candidate accepted
```

## 14.3 Dashboards

- Product activation and retention.
- Style Transfer quality by genre pair.
- Generation latency and failure by stage/model.
- Cost per accepted version.
- Rights decision and export denial reasons.
- Artifact reports and regenerate behavior.
- Preference cold-start and ranking lift.

## 14.4 Experiment backlog

| ID | Thử nghiệm | Primary metric |
|---|---|---|
| EXP-001 | Pairwise onboarding 4 vs 7 câu | Completion + first candidate acceptance |
| EXP-002 | Hiển thị 3 candidates cùng lúc vs progressive reveal | Time to first play + acceptance |
| EXP-003 | Default preserve locks khác nhau | Lock violations + acceptance |
| EXP-004 | Target genre cards vs text prompt | Successful recipe validation |
| EXP-005 | Familiar được phát đầu vs Personal được phát đầu | Accepted profile + replay |
| EXP-006 | Progress stage copy | Abandonment during generation |
| EXP-007 | “Why this version” explanation | Trust rating + selection confidence |

# 15. Non-functional requirements

## 15.1 Performance

| Requirement | Target v0.1 |
|---|---|
| Standard API p95 không gồm generation | ≤ 800 ms trong điều kiện beta mục tiêu |
| Playback start p95 | ≤ 2.5 giây trên kết nối mục tiêu |
| First candidate preview | p50 ≤ 90 giây; p95 ≤ 240 giây, provisional |
| UI progress freshness | Cập nhật trong ≤ 5 giây sau state change |
| Upload resume | Hỗ trợ file lớn theo multipart/resumable strategy |

## 15.2 Reliability

- Generation jobs không mất khi client refresh.
- Idempotent create operations.
- Candidate-level partial success.
- Dead-letter queue và replay tooling.
- Backup metadata; object durability theo hạ tầng lựa chọn.
- Runbook cho queue overload, provider outage, storage issue và rights service degradation.

## 15.3 Security

- Strong authentication và session controls.
- Least-privilege service identities.
- Encryption at rest/in transit.
- Secrets không nằm trong client/log.
- Signed URLs ngắn hạn.
- Malware/file validation cho uploads.
- Rate limiting và abuse detection.
- Audit cho admin access và policy override nếu có.

## 15.4 Privacy

- Data minimization.
- Consent versioning.
- User deletion/export workflow.
- Retention schedule theo asset type.
- Không lưu raw prompt/audio trong log telemetry không cần thiết.
- Redact identifiers khỏi diagnostics dùng rộng rãi.

## 15.5 Scalability và cost

- Queue tách analysis, generation, render và export.
- Autoscaling worker theo workload class.
- Cache analysis theo fingerprint.
- Low-cost preview trước high-quality render.
- Budget guard per job/account.
- Provider/model routing theo cost-quality-latency.

## 15.6 Observability

Mỗi job có trace xuyên suốt: API → rights → queue → model → render → QC → storage → player. Metric tối thiểu: latency stage, queue wait, retry, GPU time, cost, error code, quality score và acceptance outcome.

## 15.7 Internationalization

- UI string tách resource.
- Recipe enum không phụ thuộc ngôn ngữ hiển thị.
- Prompt parser hỗ trợ tiếng Việt P0; fallback rõ cho ngôn ngữ khác.
- Date/time/number/units theo locale.

# 16. Rollout, QA và launch gates

## 16.1 Phases

| Phase | Đối tượng | Phạm vi | Exit criteria |
|---|---|---|---|
| Technical Prototype | Nội bộ | 10–20 track, 2–3 genre pairs | Pipeline tạo được outputs và đo quality/cost/latency |
| Internal Alpha | Đội dự án + testers | P0 flow, 50+ track | Job success ≥ 95%, rights audit 100%, critical bugs đóng |
| Closed Beta | Nhóm người nghe mời | Workout + Style Transfer | Product signals đạt ngưỡng hoặc có learning rõ |
| Public MVP | Thị trường được chọn | Catalog mở rộng, support, privacy ops | Launch checklist và legal/security review hoàn tất |

## 16.2 Test dataset

Bộ test phải có quyền rõ ràng và bao phủ:

- vocal nam/nữ/nhóm;
- tempo chậm/trung bình/nhanh;
- track có/không có stems;
- nhiều cấu trúc verse/chorus;
- genre pairs P0;
- file chất lượng thấp và edge formats;
- track ngắn/dài;
- language variation;
- rights states allowed/restricted/reference-only/revoked.

## 16.3 QA matrix

| Lớp | Test |
|---|---|
| Unit | Recipe validation, policy rules, profile updates, state transitions |
| Contract | Generation Adapter, storage, rights provider, analytics schema |
| Integration | Upload → analysis → generation → playback → export |
| Audio regression | Golden set, objective metrics, expert listening |
| UX | First-time user, disabled states, errors, accessibility |
| Security | Auth, signed URL, upload abuse, admin access |
| Privacy | Delete, consent change, data export, retention |
| Load | Queue burst, concurrent playback, provider throttling |

## 16.4 Launch gates P0

- [ ] Catalog pilot có quyền và operation matrix được duyệt.
- [ ] Workout end-to-end hoàn tất trên thiết bị mục tiêu.
- [ ] Music Style Transfer hoạt động cho genre pairs được công bố.
- [ ] Rights pre-check và export re-check có audit.
- [ ] Không có blocker security/privacy.
- [ ] Generation success, latency và artifact rate nằm trong ngưỡng beta.
- [ ] Analytics events và dashboards đã kiểm chứng.
- [ ] Support runbook, diagnostic ID và incident owner sẵn sàng.
- [ ] Deletion workflow được test end-to-end.
- [ ] User-facing copy không hứa khả năng ngoài capability/rights thực tế.

# 17. Delivery plan và technical backlog

## 17.1 Epics

| Epic | Nội dung | Phụ thuộc | Outcome |
|---|---|---|---|
| E1 Identity & Consent | Auth, guest session, consent | — | Có user/session hợp lệ |
| E2 Rights Foundation | Declaration, policy engine, snapshot, audit | E1 | Operation gating |
| E3 Audio Ingest & Analysis | Upload, normalize, fingerprint, analysis | E2 | Track ready |
| E4 Preference Onboarding | Pairwise UX, profile schema | E1 | Cold-start profile |
| E5 Recipe & Intent | Prompt/preset, validation, versioning | E2, E3 | Structured generation request |
| E6 Generation Orchestrator | Queue, adapter, progress, retry | E3, E5 | Async candidates |
| E7 Workout Adjust | P0 controls and candidate profiles | E6 | First core value |
| E8 Style Transfer | Genre, locks, transform strategies, QC | E2, E3, E6 | Differentiated Transform value |
| E9 Compare & Feedback | Player, A/B, acceptance, ranking signals | E6 | User selection loop |
| E10 Stations & Library | Save, lineage, version history | E9 | Repeat value |
| E11 Export & Provenance | Rights re-check, render, metadata | E2, E10 | Controlled output |
| E12 Analytics & Ops | Events, dashboards, cost, runbooks | All | Learn and operate |
| E13 Privacy Lifecycle | Delete, consent change, data export | E1–E11 | Trust readiness |

## 17.2 Suggested implementation sequence

1. Rights schema + operation matrix.
2. Audio fixture set và evaluation harness.
3. Ingest/analysis prototype.
4. Generation Adapter technical spike.
5. Style Transfer spike cho 2–3 genre pairs.
6. Recipe schema và policy validation.
7. Mobile-first UX wireframes và clickable prototype.
8. Orchestrator + job state + progressive candidate delivery.
9. Compare Player và analytics.
10. Station, export, provenance, deletion.
11. Internal alpha và closed beta.

## 17.3 Technical spikes bắt buộc

| Spike | Câu hỏi cần trả lời | Output |
|---|---|---|
| TS-001 Style Transfer quality | Có thể giữ vocal/melody và chuyển genre đến mức nào? | Benchmark + supported genre matrix |
| TS-002 Stem strategy | Khi nào dùng stems có sẵn, separation hoặc full regeneration? | Decision tree + artifact data |
| TS-003 Latency/cost | Pipeline nào đạt preview target? | p50/p95 + cost/candidate |
| TS-004 Lock adherence | Đo lyrics/melody/structure preservation thế nào? | Metrics + human rubric |
| TS-005 Rights operations | Hợp đồng/catalog biểu diễn operation ra sao? | Rights schema + sample policy |
| TS-006 Playback comparison | Map timestamp giữa structure variants thế nào? | A/B UX prototype |

# 18. Rủi ro, phụ thuộc và câu hỏi mở

## 18.1 Risk register

| ID | Rủi ro | Mức | Giảm thiểu |
|---|---|---|---|
| R-001 | Không có đủ catalog với quyền derivative rõ | Cao | Bắt đầu nhỏ, hợp tác nghệ sĩ indie, operation matrix theo track |
| R-002 | Style Transfer không giữ được vocal/melody đủ sạch | Cao | Preserve locks, supported pairs, stems-first, quality gate, fallback |
| R-003 | Output có artifact hoặc không nhận ra target genre | Cao | Evaluation harness, human listening, candidate diversity, hide failed outputs |
| R-004 | Latency/cost làm người dùng bỏ cuộc | Cao | Progressive candidate delivery, preview tier, caching, routing |
| R-005 | Người dùng kỳ vọng chỉnh mọi bài thương mại | Cao | Catalog badge, reference-only explanation, Create New alternative |
| R-006 | Cold-start profile không chính xác | Trung bình | Pairwise onboarding, neutral defaults, fast feedback loop |
| R-007 | Rights thay đổi sau generation | Cao | Re-check trước export, revocation state, audit |
| R-008 | Prompt yêu cầu sao chép nghệ sĩ | Trung bình | Policy parser, descriptor conversion, unsupported request UX |
| R-009 | Metrics tối ưu generation thay vì listening value | Trung bình | North Star dựa trên accepted/replayed listening minutes |
| R-010 | Provider lock-in | Trung bình | Generation Adapter, capability matrix, contract tests |
| R-011 | Dữ liệu audio nhạy cảm bị truy cập sai | Cao | Encryption, signed URL, least privilege, access audit |
| R-012 | Scope quá rộng cho MVP | Cao | Release slices, P0 gates, Create New/Focus sau core stability |

## 18.2 External dependencies

- Catalog/licensing partners.
- Audio generation/transformation capabilities hoặc mô hình nội bộ.
- Object storage/CDN và GPU compute.
- Identity/auth provider nếu sử dụng.
- Legal review theo thị trường.
- Payment/quota system ở giai đoạn monetization.
- Human audio evaluators và test participants.

## 18.3 Open questions

| ID | Câu hỏi | Owner đề xuất | Thời điểm cần khóa |
|---|---|---|---|
| OQ-001 | Responsive web/PWA hay native app cho Closed Beta? | Product + Engineering | Trước UX hi-fi |
| OQ-002 | Việt Nam-first hay thị trường khác? | Product + GTM + Rights | Trước catalog contract |
| OQ-003 | Pilot catalog cụ thể và operation rights theo track? | Partnerships + Rights | Trước alpha |
| OQ-004 | Genre pairs P0 nào có chất lượng tốt nhất? | AI/Audio + Product | Sau TS-001 |
| OQ-005 | Preview latency/cost thực tế? | Engineering + AI/Audio | Sau TS-003 |
| OQ-006 | Export có trong Closed Beta hay chỉ in-app listening? | Product + Rights | Trước beta scope freeze |
| OQ-007 | Monetization subscription/credits/kết hợp? | Product + Finance | Không chặn prototype |
| OQ-008 | Retention period cho source/preview/deleted assets? | Privacy + Engineering | Trước alpha |
| OQ-009 | Success thresholds cần điều chỉnh theo cohort nào? | Data + Product | Sau alpha |
| OQ-010 | Tech stack cuối cùng và năng lực đội ngũ? | Engineering | Trước implementation plan |

# 19. Definition of Done cho Listener MVP v0.1

MVP được coi là đạt baseline khi:

1. Người dùng mới hoàn tất onboarding và tạo được profile.
2. Người dùng chọn Workout, track hợp lệ và operation.
3. Rights engine cho phép/chặn đúng và có audit.
4. Analysis và Music Brief sẵn sàng hoặc có fallback rõ.
5. Người dùng tạo được ba candidate cho Adjust hoặc supported Style Transfer.
6. Preserve locks được lưu và kiểm tra.
7. Compare Player hoạt động với partial results.
8. Người dùng chọn, refine, lưu version và station.
9. Export chỉ khả dụng theo rights; provenance được gắn.
10. Analytics, cost, quality và error telemetry đầy đủ.
11. User có thể xóa dữ liệu và thay đổi consent.
12. Closed Beta đạt technical gates và tạo đủ learning để quyết định public MVP.

# 20. Hành động kế tiếp

Sau khi PRD v0.1 được review, đội dự án thực hiện hai luồng song song:

1. **UX Specification & Wireframes v0.1:** thiết kế mobile-first cho Workout, Change Style, Compare Player, rights states và error recovery.
2. **Technical Feasibility Spike:** benchmark Style Transfer, stem strategy, lock adherence, latency, cost và supported genre pairs trên bộ audio có quyền rõ ràng.

Kết quả của hai luồng sẽ tạo đầu vào cho **System Design v0.1**, sprint plan và cập nhật `LOG-003`.

# Phụ lục A — Glossary

| Thuật ngữ | Định nghĩa |
|---|---|
| Adjust | Điều chỉnh thuộc tính trong khi giữ bản sắc/thể loại tương đối ổn định |
| Transform | Tạo arrangement hoặc style mới từ source đủ quyền |
| Music Style Transfer | Chuyển thể loại/phong cách trong khi giữ các component được khóa |
| Create New | Tạo tác phẩm độc lập từ profile/thuộc tính trừu tượng |
| Preserve Lock | Ràng buộc bắt buộc hoặc do người dùng chọn để giữ component |
| Edit Recipe | Biểu diễn có cấu trúc của intent, constraints, rights và controls |
| Candidate | Một trong các output Familiar/Personal/Explore |
| Rights Snapshot | Quyết định quyền immutable tại thời điểm operation |
| Personal Station | Context + recipe + preference được lưu để sử dụng lại |
| Provenance | Metadata về nguồn, quyền, recipe, model và lịch sử tạo |
| Quality Gate | Kiểm tra quyết định candidate có đủ điều kiện hiển thị/export |

# Phụ lục B — Sample acceptance test

**Scenario:** Người dùng chuyển Ballad → EDM cho Workout.

**Given** track nằm trong catalog, có quyền giữ lyrics/vocal/melody và tạo derivative.  
**And** stem vocal đạt capability threshold.  
**When** người dùng chọn Workout 20 phút, target EDM, 124 BPM, giữ Lyrics + Melody + Vocal + Structure, intensity Balanced.  
**Then** recipe được validate và lưu rights snapshot.  
**And** job tạo Familiar/Personal/Explore.  
**And** mỗi candidate chạy quality/lock checks.  
**And** người dùng có thể A/B, chọn Personal, yêu cầu “drop mạnh hơn” và tạo child version.  
**And** export chỉ được mở nếu rights state hiện tại cho phép.  
**And** mọi event, cost, latency và lineage được ghi nhận.

# Phụ lục C — PRD review checklist

- [ ] Product xác nhận mục tiêu, P0/P1/P2 và success thresholds.
- [ ] Design xác nhận flow, screen inventory và accessibility.
- [ ] Engineering xác nhận epics, service boundaries và NFR.
- [ ] AI/Audio xác nhận supported capabilities và evaluation plan.
- [ ] Rights xác nhận operation matrix và user-facing copy.
- [ ] Security/Privacy xác nhận data lifecycle.
- [ ] QA xác nhận acceptance criteria có thể test.
- [ ] Data xác nhận event taxonomy và dashboards.
- [ ] Chủ dự án ghi quyết định review vào MayaTune Project Log.
