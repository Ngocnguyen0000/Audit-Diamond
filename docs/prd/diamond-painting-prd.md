# PRD — Diamond Painting (Tô màu kim cương theo số)

| | |
|---|---|
| **Sản phẩm** | Diamond Painting — game tô màu pixel/kim cương theo số |
| **Phiên bản tài liệu** | 0.1 |
| **Ngày** | 2026-07-17 |
| **Trạng thái** | 🟡 Draft — chờ chốt các mục `[CẦN XÁC NHẬN]` |
| **Nguồn thiết kế** | [Figma — Audit Diamond](https://www.figma.com/design/lfGHbsUaDacwp80v1epi0g/Audit-Diamond) |
| **Tài liệu liên quan** | [Báo cáo Audit UI](../audit/ui-audit-report.md) |

---

## 1. Tổng quan sản phẩm

Diamond Painting là game mobile casual thể loại **color-by-number / diamond art**: người chơi chạm vào các ô được đánh số trên lưới pixel để "đính đá" hoàn thành bức tranh. App hướng tới trải nghiệm thư giãn (relax & create), có hệ thống sưu tầm tranh theo chủ đề, cho phép **upload ảnh cá nhân để tự tạo tranh**, và kiếm tiền qua quảng cáo + nội tệ (diamond).

**Điểm khác biệt chính (theo UI hiện có):**
1. Upload ảnh riêng → app chuyển thành tranh pixel theo số màu tùy chọn (Level 1–4).
2. Hệ thống Artwork gallery lưu và chia sẻ thành phẩm (Download/Share).
3. Effect trang trí thành phẩm (mua bằng diamond).
4. Vòng lặp retention: Daily check-in, Streak challenge 7 ngày, vòng quay may mắn.

## 2. Vấn đề & Cơ hội

- **Pain point:** người dùng casual cần hoạt động thư giãn ngắn, không áp lực, có cảm giác hoàn thành (completion satisfaction). `[CẦN XÁC NHẬN: có dữ liệu nghiên cứu người dùng không?]` Hiện tại tỷ lệ người dùng vào đến store tải app thì nhiều nhưng số người sẽ tải xuống thì thấp
- **Cơ hội thị trường:** thể loại color-by-number (Happy Color, Pixel Art by Easybrain, Diamond Painting ASMR) có DAU lớn, monetize chủ yếu bằng rewarded ads + remove-ads IAP. `[CẦN XÁC NHẬN: thị trường mục tiêu — global hay VN?]` tập trung thị trường global

## 3. Đối tượng người dùng

`[CẦN XÁC NHẬN — đề xuất dựa trên art direction hiện tại - Đối tương người dùng sẽ là thanh niên từ 12 đến trưởng thành]`

| Persona | Mô tả | Nhu cầu chính |
|---|---|---|
| **Người chơi thư giãn (chính)** | 18–40, nghiêng nữ, chơi 5–15 phút/phiên buổi tối hoặc lúc chờ | Thư giãn, hoàn thành tranh đẹp, không bị ép thao tác khó |
| **Người sưu tầm** | Chơi đều mỗi ngày, quan tâm bộ sưu tập theo chủ đề | Bộ sưu tập mới thường xuyên, streak/daily reward |
| **Người sáng tạo** | Muốn biến ảnh cá nhân (thú cưng, người thân) thành tranh | Upload ảnh, tùy chỉnh số màu, chia sẻ thành phẩm |

## 4. Phạm vi

### 4.1 Trong phạm vi MVP (đã có UI trong Figma)

- FTU: Splash, Onboarding 3 bước (có native ad)
- Home + điều hướng Daily / Shop / Streak
- Duyệt tranh: Category (filter All/Cartoon/Anime/Flower), Collection theo chủ đề
- Chọn level (số lượng màu/độ khó: Level 1–4)
- Gameplay tô màu: bảng màu đánh số, bộ công cụ (tìm ô, gợi ý, zoom…), thanh tiến độ có mốc thưởng, menu in-game (Music/Sound/Exit/Help)
- Màn kết quả: xem lại, Download, Share
- Artwork gallery: danh sách, chi tiết theo level, xóa (có popup xác nhận), empty state
- Upload ảnh: chọn nguồn (Camera/Photo) → crop → chọn level + số màu → tạo tranh
- Effect trang trí (mua bằng diamond)
- Kiếm thưởng: xem video ad, vòng quay, hoàn thành tranh, check-in, chia sẻ bạn bè
- Challenge: Daily check-in 7 ngày, Streak challenge, Today's Goal
- Quảng cáo: banner (in-game), native ad (onboarding, artwork, result), rewarded video

### 4.2 Ngoài phạm vi MVP / chưa thiết kế (theo Audit UI)

| Hạng mục | Lý do cần quyết định |
|---|---|
| Màn **Shop** + mua diamond (IAP paywall) | Tab Shop và nút "+" diamond đã tồn tại trên UI nhưng chưa có màn đích — **chặn release nếu có IAP** |
| Popup **không đủ diamond** | Nhánh else của Use Effect / Use Diamonds |
| **Settings**, **Help** | Đã có entry point (bánh răng ở Home, mục Help trong menu chơi) |
| Màn **vòng quay may mắn** | Mới có popup Congrats kết quả |
| Xác nhận **Exit Drawing** | Tránh mất tiến độ |
| Loading / Error states (upload, mất mạng) | Bước xử lý ảnh chắc chắn có độ trễ |
| Remove Ads / Premium | `[CẦN XÁC NHẬN: có bán gói remove-ads không?]` Không có gói bán remove ads nhưng có reward để người dùng xem ads |

## 5. Luồng người dùng chính

### 5.1 First-time user (FTU)
```
Splash (loading, "may contain ads")
  → Obs_1 "Diamond Painting" → Obs_2 "Relax & Create" → Obs_3 "Artwork" [Get Started]
  → Home
```

### 5.2 Vòng lặp chơi cốt lõi (core loop)
```
Home → Category/Collection → chọn tranh → chọn Level (1–4)
  → Màn chơi (tô màu, nhận mốc thưởng theo tiến độ)
  → Hoàn thành → popup "Finish your artwork?" → Màn kết quả (Download/Share)
  → [CẦN XÁC NHẬN: CTA quay lại chơi tiếp — hiện UI chưa có]
```

### 5.3 Upload ảnh tự tạo tranh
```
Category (ô "+") → popup Select Image (Camera/Photo)
  → Crop ảnh → Chọn Level + số màu → Save
  → [CẦN XÁC NHẬN: sau Save đi đâu — vào thẳng Màn chơi hay về Category?]
```

### 5.4 Kinh tế trong game
```
Kiếm: xem rewarded ad / vòng quay / hoàn thành tranh / check-in / share
Tiêu: Effect (10 💎) / unlock tranh premium 
Mua: [CHƯA THIẾT KẾ — Shop/IAP]
```
> ⚠️ **Câu hỏi mở quan trọng nhất:** màn "Kiếm coins" thưởng **coins** nhưng toàn bộ UI còn lại chỉ dùng **diamonds**. Cần chốt: 1 hay 2 loại tiền tệ, tỷ giá quy đổi, và ví Diamonds hiển thị ở đâu. Quyết định này ảnh hưởng header của mọi màn hình. Sử dụng coin để mua thêm các bút cho chức năng trong màn chơi như chức năng fill, tìm kiếm ô để chơi

## 6. Yêu cầu chức năng

Định dạng: user story + tiêu chí nghiệm thu (AC).

### F1 — FTU (Splash + Onboarding)
> Là người dùng mới, tôi muốn hiểu nhanh app làm gì để bắt đầu chơi ngay.

- AC1: Splash hiển thị ≤ 3s khi có mạng; có progress bar; hiện disclaimer "This action may contain ads".
- AC2: Onboarding 3 trang, có thể Next từng trang; trang cuối nút "Get Started" vào Home.
- AC3: Onboarding chỉ hiện lần đầu mở app. `[CẦN XÁC NHẬN: có nút Skip không — UI hiện chưa có]` Không có nút skip
- AC4: Native ad ở onboarding không che nút điều hướng và tuân thủ chính sách store (xem mục 8.2).

### F2 — Home
> Là người chơi, tôi muốn từ Home vào nhanh phần chơi hoặc xem lại thành phẩm.

- AC1: Card "Diamond Painting" (Start now) → Category; card "Artwork" (View Gallery) → Artwork gallery.
- AC2: Header hiển thị số diamond hiện có; nút "+" mở luồng nạp `[CHƯA THIẾT KẾ]`.
- AC3: Nút Settings mở màn Settings `[CHƯA THIẾT KẾ]`.
- AC4: Tab Daily - Checkin nhận coin hàng ngày / Shop - Mua diamond mới hoặc một số effect mới/ Streak điều hướng đúng đích - hiện tại chưa biết điều hướng như thế nào hãy gợi ý và làm cho tôi.

### F3 — Category & Collection
- AC1: Filter chip All / Cartoon / Anime / Flower lọc đúng danh sách.
- AC2: Tranh khóa (premium) hiển thị khóa và giá mở khóa `[CẦN XÁC NHẬN: giá]`Coi 1 ads là mở được tranh bị khóa.
- AC3: Ô "+" đầu grid mở luồng Upload (F8).
- AC4: Collection hiển thị theo chủ đề (Sparkle Like a Star, Princess World, Cartoon Besties…); badge "New" cho tranh mới.
- AC5: Empty state khi collection trống `[CHƯA THIẾT KẾ]`.

### F4 — Chọn level
- AC1: Mỗi tranh có 4 level tương ứng độ phân giải/số màu (ví dụ 60×60, 99×99).
- AC2: Level đã hoàn thành hiển thị tiến độ %; "Start Drawing" mở màn chơi với level đã chọn.

### F5 — Gameplay (Màn chơi)
> Là người chơi, tôi muốn tô màu mượt, biết còn bao nhiêu ô, và được thưởng theo tiến độ.

- AC1: Bảng màu đánh số (1–n), chọn màu → các ô tương ứng được highlight; đếm số ô còn lại trên từng màu.
- AC2: Bộ đếm tiến độ (vd 60/99) + progress bar có các mốc quà; đạt mốc → nhận thưởng.
- AC3: Công cụ hỗ trợ: tìm ô, gợi ý/auto-fill, zoom, xem ảnh gốc, magic wand `[CẦN XÁC NHẬN: hành vi + giá từng tool — một số tool có badge số lượt]` Đúng.
- AC4: Menu ☰: bật/tắt Music, Sound; Exit Drawing (có xác nhận nếu chưa lưu `[CHƯA THIẾT KẾ popup]`); Help `[CHƯA THIẾT KẾ]`.
- AC5: Tiến độ tự lưu khi thoát; mở lại tiếp tục đúng trạng thái.
- AC6: Banner ad đáy màn không che bảng màu/công cụ.
- AC7: Hoàn thành 100% → popup "Finish your artwork?" (Keep Creating / View Result).

### F6 — Màn kết quả
- AC1: Hiển thị tranh hoàn thành (có animation replay ).
- AC2: Download lưu ảnh vào máy (xin quyền Photos nếu cần); Share mở share sheet hệ điều hành.
- AC3: Đề xuất hành động tiếp theo (Next artwork) `[CHƯA THIẾT KẾ — khuyến nghị bổ sung để giữ retention]`.

### F7 — Artwork gallery
- AC1: Danh sách thành phẩm; empty state "Start Drawing" khi trống (đã có UI).
- AC2: Chi tiết artwork: các level đã chơi + tiến độ; Continue với level dở dang.
- AC3: Xóa artwork: popup xác nhận Delete/Cancel; xóa không khôi phục được → nút Delete phải là kiểu destructive (đỏ/outline).

### F8 — Upload ảnh tự tạo tranh
> Là người sáng tạo, tôi muốn biến ảnh của mình thành tranh diamond theo số.

- AC1: Chọn nguồn Camera / Photo (xin quyền tương ứng, xử lý khi bị từ chối `[CHƯA THIẾT KẾ]`).
- AC2: Crop vùng ảnh vuông; Cancel/Save.
- AC3: Chọn Level (1–4) + slider số màu; preview kết quả pixel hóa `[CẦN XÁC NHẬN: có preview realtime không]` không.
- AC4: Có loading state khi xử lý ảnh; lỗi xử lý → thông báo và cho thử lại `[CHƯA THIẾT KẾ]`.
- AC5: Tranh tạo xong xuất hiện ở đâu (Category tab riêng? Artwork?) `[CẦN XÁC NHẬN]`.

### F9 — Effect
- AC1: Danh sách effect (Effect 1–4) áp lên thành phẩm; preview trước khi mua.
- AC2: Mua effect: popup "Use Effect? — 10 💎" (Cancel / Use now); trừ diamond đúng.
- AC3: Không đủ diamond → popup dẫn tới Shop/rewarded ad `[CHƯA THIẾT KẾ]`.
- AC4: Entry point của Effect trong luồng chính `[CẦN XÁC NHẬN: từ màn kết quả hay artwork detail?]artwork detail`.

### F10 — Kinh tế & Shop
- AC1: Nguồn kiếm: rewarded video, vòng quay may mắn, hoàn thành tranh, check-in hằng ngày, chia sẻ bạn bè (mỗi mục +100 `[CẦN XÁC NHẬN đơn vị coins/diamonds]` Diamonds).
- AC2: Vòng quay: màn quay + popup Congrats (Claim) `[màn quay CHƯA THIẾT KẾ]`.
- AC3: Shop mua diamond bằng tiền thật `[CHƯA THIẾT KẾ toàn bộ]`.
- AC4: Số dư đồng bộ realtime trên header mọi màn.

### F11 — Challenge / Streak
- AC1: Daily check-in 7 ngày, ngày hiện tại highlight, Claim nhận thưởng.
- AC2: Streak challenge 7 ngày với các mốc thưởng lớn dần.
- AC3: Today's Goal: danh sách nhiệm vụ (spin ×3, vẽ 1 tranh…) + nút Go/Finished.
- AC4: Miss 1 ngày → hành vi streak `[CẦN XÁC NHẬN: reset hay cho phép hồi bằng ad/diamond]` cho phép hồi bằng ad/diamond.

### F12 — Quảng cáo (Monetization)
- AC1: Banner: đáy màn chơi. Native: onboarding, artwork detail, màn kết quả. Rewarded: kiếm diamond, hồi tool.
- AC2: Tần suất interstitial `[CẦN XÁC NHẬN: UI chưa thể hiện interstitial]`.
- AC3: Ad không load được → UI không vỡ layout, nút rewarded hiển thị trạng thái không khả dụng.
- AC4: Tuân thủ chính sách ad network + store (không đặt nút sát vùng ad gây misclick).

## 7. Yêu cầu phi chức năng

| Hạng mục | Yêu cầu |
|---|---|
| Hiệu năng | Render lưới tới 99×99 (≈10.000 ô) mượt 60fps khi pan/zoom; mở app → Home ≤ 4s |
| Lưu trữ | Tiến độ chơi lưu local, không mất khi kill app; `[CẦN XÁC NHẬN: có sync cloud/đăng nhập không?]` không có |
| Offline | Chơi tranh đã tải được khi offline; ads/upload yêu cầu mạng; xử lý ảnh upload `[CẦN XÁC NHẬN: on-device hay server?]` on-device |
| Bảo mật & riêng tư | Ảnh người dùng upload: nêu rõ trong privacy policy nơi xử lý/lưu; xóa theo yêu cầu |
| Ngôn ngữ | UI hiện tại tiếng Anh; `[CẦN XÁC NHẬN: có localize tiếng Việt và thị trường khác không]` |

## 8. Yêu cầu đặc thù mobile

### 8.1 Nền tảng
`[ iOS, Android hay cả hai; native (Swift/Kotlin)]`. UI thiết kế theo khung iPhone (375×812, notch) — nếu có Android cần thêm spec cho màn hình không notch, nút back hệ thống, và tỉ lệ màn khác.

### 8.2 App Store / Play Store compliance
- App có ads + nhân vật hoạt hình dễ hút trẻ em → cần khai báo đúng **age rating**; nếu nhắm "families/kids" thì ad network phải đạt chuẩn trẻ em (COPPA/GDPR-K) — native ad trong onboarding sẽ bị soi kỹ.
- iOS: ATT prompt trước khi tracking cho quảng cáo cá nhân hóa `[CHƯA THIẾT KẾ popup pre-ATT]`.
- IAP diamond phải qua In-App Purchase của store (không thanh toán ngoài).

### 8.3 Permissions
| Quyền | Khi nào xin | Màn liên quan |
|---|---|---|
| Camera | Lần đầu chọn "Camera" trong Select Image | F8 |
| Photo library (read) | Lần đầu chọn "Photo" | F8 |
| Photo library (add) | Lần đầu bấm Download kết quả | F6 |
| Notifications | Sau khi hoàn thành tranh đầu tiên (đề xuất) | F11 — nhắc streak/daily |
| ATT (iOS) | Sau onboarding, trước ad đầu tiên | F12 |

### 8.4 Push notification
` đề xuất: nhắc daily check-in, sắp mất streak, collection mới.

## 9. Success metrics

| Metric | Định nghĩa | Mục tiêu gợi ý |
|---|---|---|
| D1 / D7 retention | % quay lại sau 1/7 ngày | ≥ 35% / ≥ 12% |
| Completion rate | % tranh bắt đầu được hoàn thành | ≥ 45% |
| Session length | Thời gian chơi trung bình/phiên | ≥ 8 phút |
| Rewarded engagement | % DAU xem ≥ 1 rewarded ad/ngày | ≥ 30% |
| ARPDAU | Doanh thu (ads+IAP)/DAU | benchmark thể loại |
| Upload adoption | % người dùng tạo ≥ 1 tranh từ ảnh riêng | theo dõi từ MVP |

## 10. Câu hỏi mở (chặn chốt PRD)

1. **Coins vs Diamonds** — 1 hay 2 loại tiền? (mục 5.4)
2. Nền tảng phát hành & công nghệ (mục 8.1)
3. Có IAP không, gói nào (diamond pack, remove ads)? → quyết định scope màn Shop
4. Đích đến của 3 tab Daily/Shop/Streak; có chuyển sang bottom tab bar không?
5. Sau khi upload/Save, luồng đi tiếp thế nào?
6. Entry point của Effect
7. Xử lý ảnh upload: on-device hay server (ảnh hưởng privacy + offline)
8. Thị trường & ngôn ngữ mục tiêu

## 11. Roadmap đề xuất

| Phase | Nội dung | Ghi chú |
|---|---|---|
| **P0 — Chốt thiết kế** | Trả lời 8 câu hỏi mở; vẽ các màn `[CHƯA THIẾT KẾ]`; chuẩn hóa design system (xem [Audit](../audit/ui-audit-report.md)) | đang làm |
| **P1 — MVP** | F1–F8 + banner/rewarded ads + lưu local | core loop khép kín |
| **P2 — Kinh tế đầy đủ** | Shop/IAP, Effect, vòng quay, Challenge/Streak | cần P0 câu 1,3 |
| **P3 — Tăng trưởng** | Push notification, localize, sync cloud, collection theo mùa | sau khi có số liệu P1 |

---

*Tài liệu sinh từ audit UI Figma ngày 2026-07-17. Cập nhật khi có quyết định mới; đổi Status thành Final sau khi chốt checklist ở mục 10.*
