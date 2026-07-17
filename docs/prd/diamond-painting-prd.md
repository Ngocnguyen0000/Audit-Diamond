# PRD — Diamond Painting (Tô màu kim cương theo số)

| | |
|---|---|
| **Sản phẩm** | Diamond Painting — game tô màu pixel/kim cương theo số |
| **Phiên bản tài liệu** | 0.2 — bổ sung mode Block Blast (mục 6/F13) |
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
5. **Mode Block Blast (v0.2)**: mini-game xếp khối kim cương vuông trên lưới 8×8 — vòng chơi thứ hai tăng session length và tạo thêm chỗ tiêu coin (xem F13).

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
| **Mode Block Blast** (toàn bộ UI) | Mode mới v0.2, chưa có màn nào trong Figma — cần vẽ: entry card ở Home, màn chơi, pause, Game Over/Revive, Result (xem F13.7) |

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

### 5.5 Mode Block Blast (v0.2)
```
Home (card "Block Blast") → Màn chơi Block Blast
  → đặt khối kim cương vuông từ khay 3 khối lên lưới 8×8
  → đầy hàng ngang/dọc → nổ kim cương, cộng điểm, combo
  → hết chỗ đặt → popup Revive (xem ad / 20 💎, tối đa 1 lần/ván)
  → Game Over → màn Result (điểm, best score, thưởng coin/diamond) → Play Again / Home
```

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
- AC4: Tab Daily - Checkin nhận coin hàng ngày / Shop - Mua diamond mới hoặc một số effect mới / Streak điều hướng đúng đích. **Đề xuất điều hướng:** giữ 3 chip ở Home như shortcut (không phải tab bar toàn cục): `Daily` → mở màn Daily Check-in (phần trên của màn Challenge hiện có, tách thành màn riêng); `Shop` → màn Shop (2 tab con: Diamonds, Effects); `Streak` → màn Challenge/Streak hiện có (phần 7-Day Challenge + Today's Goal). Bỏ 3 chip này khỏi màn Category để tránh hiểu nhầm là navigation toàn cục — Category chỉ giữ filter nội dung.
- AC5 (v0.2): Home có card thứ 3 **"Block Blast"** (cùng cỡ card Diamond Painting/Artwork, CTA "Play now") → mở màn chơi Block Blast (F13).

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

### F13 — Mode Block Blast (v0.2)
> Là người chơi, tôi muốn một mini-game nhanh, dễ hiểu để đổi gió giữa các bức tranh mà vẫn giữ cảm giác "kim cương" của app.

#### F13.1 Concept & mục tiêu
- Puzzle xếp khối kiểu Block Blast trên lưới **8×8**; mọi ô của khối là **viên kim cương vuông** (square gem) render theo art style của app — cùng bộ màu đá với màn tô tranh (dùng token `Primitives` đã chuẩn hóa).
- Mục tiêu sản phẩm: tăng session length, tạo thêm inventory quảng cáo (revive/booster), và là **chỗ tiêu coin** — cân bằng với nguồn coin từ Daily check-in (mục 5.4: diamond = tiền chính cho effect/unlock, coin = tiền tool).
- Phiên chơi mục tiêu 2–5 phút, không có timer — giữ tinh thần thư giãn, thua do hết chỗ chứ không do áp lực thời gian.

#### F13.2 Luật chơi (core rules)
- AC1: Lưới 8×8 trống khi bắt đầu ván mới. Khay (tray) hiển thị **3 khối** ngẫu nhiên; kéo-thả khối lên lưới; khối **không xoay được** (chuẩn thể loại — đơn giản cho casual).
- AC2: Bộ khối gồm 1–9 viên kim cương vuông: 1×1, 1×2…1×5 (ngang/dọc), 2×2, 3×3, chữ L/J (2×3), chữ T. Trọng số xuất hiện điều chỉnh được từ remote config để tinh chỉnh độ khó.
- AC3: Chỉ đặt được vào vị trí đủ ô trống; khi kéo, hiện **ghost preview** vị trí sẽ đặt + highlight hàng/cột sắp nổ; vị trí không hợp lệ → khối mờ đi và bật về khay.
- AC4: Đặt đủ 3 khối → khay sinh 3 khối mới. Lấp đầy **hàng ngang hoặc cột dọc** → hàng/cột nổ với hiệu ứng kim cương vỡ lấp lánh + rung nhẹ (haptic) — tận dụng cảm giác ASMR như khi đính đá.
- AC5: **Điểm**: +1 điểm/viên khi đặt khối; nổ 1 hàng = +10; nổ nhiều hàng cùng lúc nhân theo số hàng (2 hàng = ×2…). **Combo streak**: nổ ở các lượt đặt liên tiếp → nhân điểm lũy tiến (×2, ×3… tối đa ×8), hiện label "Combo ×N!". Đứt combo khi 1 lượt đặt không nổ hàng nào.
- AC6: **Game over** khi cả 3 khối trong khay đều không còn chỗ đặt hợp lệ → mờ lưới, hiện popup Revive (F13.4).
- AC7: Ván dở lưu tự động (lưới + khay + điểm + combo); thoát ra Home rồi quay lại tiếp tục đúng trạng thái — nhất quán với F5-AC5.

#### F13.3 Booster (tiêu coin — trả lời câu hỏi mở #1)
| Booster | Giá gợi ý | Hành vi |
|---|---|---|
| 🔨 Hammer | 100 coin | Đập 1 viên bất kỳ trên lưới |
| 🔄 Swap | 150 coin | Đổi 1 khối trong khay lấy khối ngẫu nhiên khác |
| 💥 Clear Row | 300 coin | Nổ 1 hàng/cột tự chọn |

- AC1: Booster đặt ở thanh dưới khay, hiển thị số lượng sở hữu; hết → bấm vào hiện giá coin, mua nhanh không rời màn chơi.
- AC2: Không đủ coin → popup gợi ý: xem rewarded ad nhận coin hoặc đi Daily check-in (không dẫn sang IAP vì coin không bán bằng tiền thật).
- AC3: Giá booster điều khiển bằng remote config.

#### F13.4 Revive & quảng cáo
- AC1: Game over lần đầu trong ván → popup **Revive**: "Continue?" với 2 lựa chọn: xem rewarded ad **hoặc** 20 💎; hiệu ứng revive = dọn 3 hàng ngẫu nhiên + khay mới. Tối đa **1 revive/ván**; từ chối → sang màn Result.
- AC2: Banner ad đáy màn chơi (vị trí như F5-AC6, không che khay/booster).
- AC3: Interstitial (nếu bật theo F12-AC2): chỉ sau màn Result, không bao giờ chen giữa ván.

#### F13.5 Thưởng & tích hợp kinh tế
- AC1: Màn **Result**: điểm ván, best score (kèm hiệu ứng "New Record!" nếu phá kỷ lục), thưởng quy đổi: mỗi 500 điểm = 50 coin; đạt mốc 1.000/2.500/5.000 điểm lần đầu trong ngày = +5/10/20 💎.
- AC2: Best score lưu local (không cần đăng nhập — nhất quán mục 7 "không có cloud sync").
- AC3: Tích hợp **Today's Goal** (F11-AC3): thêm nhiệm vụ dạng "Chơi 1 ván Block Blast", "Đạt 1.000 điểm Block Blast".
- AC4: Số dư coin/diamond trên header đồng bộ realtime như F10-AC4.

#### F13.6 Màn hình & UI cần vẽ (chưa có trong Figma)
1. **Card "Block Blast" ở Home** — cùng hệ card hiện có, art khối kim cương vuông.
2. **Màn chơi**: header (back, coin, diamond) + lưới 8×8 + khay 3 khối + thanh booster + điểm/combo + banner ad. Nền dùng gradient `color/bg/deep → color/bg/default` như màn tô tranh.
3. **Menu pause** (☰): Music/Sound/Exit/Help — tái dùng component menu của F5-AC4.
4. **Popup Revive** — theo hệ popup khiên vàng game-style đã thống nhất trong Audit.
5. **Màn Result** — bố cục kế thừa "Màn kết quả" hiện có (điểm thay cho tranh, CTA Play Again + Home).
6. Popup xác nhận Exit khi đang giữa ván (tái dùng popup confirm Exit Drawing sắp vẽ).

#### F13.7 Edge cases
- Khay sinh khối phải qua kiểm tra "có ít nhất 1 khối đặt được" ở **lượt sinh đầu ván** (tránh game over ngay khi mở); các lượt sau sinh thuần ngẫu nhiên theo trọng số.
- Kill app giữa animation nổ hàng → khi mở lại, trạng thái đã là sau-khi-nổ (commit điểm trước khi chạy animation).
- Rewarded ad revive không load được → nút ad disable, vẫn cho revive bằng 20 💎.
- Thiết bị màn nhỏ/không notch: lưới ưu tiên chiều rộng, khay không bị banner ad đè (safe area).

#### F13.8 Metrics riêng của mode
| Metric | Mục tiêu gợi ý |
|---|---|
| Mode adoption — % DAU chơi ≥ 1 ván | ≥ 25% |
| Số ván/người/ngày | ≥ 3 |
| Revive rate (% game over có revive) | 15–25% |
| Coin sink — % coin kiếm được tiêu vào booster | ≥ 40% |

#### F13.9 Câu hỏi mở của mode
1. Tên hiển thị chính thức: "Block Blast" hay tên riêng theo brand (vd "Diamond Blast") để tránh trùng thương hiệu game Block Blast hiện có trên store? **[Khuyến nghị: Diamond Blast]**
2. Có leaderboard (Game Center/Play Games) không, hay chỉ best score local?
3. Block Blast có được tính vào Streak challenge không, hay streak chỉ tính tranh hoàn thành?

## 7. Yêu cầu phi chức năng

| Hạng mục | Yêu cầu |
|---|---|
| Hiệu năng | Render lưới tới 99×99 (≈10.000 ô) mượt 60fps khi pan/zoom; mở app → Home ≤ 4s; Block Blast: kéo-thả khối phản hồi ≤ 16ms (ghost preview không giật), animation nổ hàng không chặn input |
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

1. **Coins vs Diamonds** — 1 hay 2 loại tiền? Diamonds (mục 5.4)
2. Nền tảng phát hành & công nghệ (mục 8.1) google play
3. Có IAP không, gói nào (diamond pack, remove ads)? → quyết định scope màn Shop: Hiện tại không có
4. Đích đến của 3 tab Daily/Shop/Streak; có chuyển sang bottom tab bar không?
5. Sau khi upload/Save, luồng đi tiếp thế nào? Cho người dùng crop ảnh chọn số pixel rồi vào màn chới
6. Entry point của Effect
7. Xử lý ảnh upload: on-device hay server (ảnh hưởng privacy + offline)
8. Thị trường & ngôn ngữ mục tiêu: Golbal
9. Block Blast: tên chính thức, leaderboard, quan hệ với Streak (xem F13.9)

## 11. Roadmap đề xuất

| Phase | Nội dung | Ghi chú |
|---|---|---|
| **P0 — Chốt thiết kế** | Trả lời 8 câu hỏi mở; vẽ các màn `[CHƯA THIẾT KẾ]`; chuẩn hóa design system (xem [Audit](../audit/ui-audit-report.md)) | đang làm |
| **P1 — MVP** | F1–F8 + banner/rewarded ads + lưu local | core loop khép kín |
| **P2 — Kinh tế đầy đủ** | Shop/IAP, Effect, vòng quay, Challenge/Streak | cần P0 câu 1,3 |
| **P2.5 — Mode Block Blast** | F13: UI (6 màn/popup, F13.6) → gameplay → booster/revive → tích hợp Today's Goal | sau P2 vì cần coin sink/source cân bằng; A/B đo tác động lên session length |
| **P3 — Tăng trưởng** | Push notification, localize, sync cloud, collection theo mùa | sau khi có số liệu P1 |

---

*Tài liệu sinh từ audit UI Figma ngày 2026-07-17. Cập nhật khi có quyết định mới; đổi Status thành Final sau khi chốt checklist ở mục 10.*
