# 🔄 ĐỒNG BỘ PHIÊN LÀM VIỆC — Dự Án TKB Trường TH Nguyễn An Khương

> **Cách dùng:** Thầy copy TOÀN BỘ nội dung file này, dán làm tin nhắn đầu tiên khi mở
> Claude Code trên máy #1 (MacBook), máy #2 (bàn ở nhà), hoặc máy #3 (cơ quan) — hoặc bất
> kỳ phiên Claude nào khác cùng tài khoản. Claude đọc xong sẽ nắm ngay bối cảnh, không cần
> giải thích lại từ đầu.
>
> **Cập nhật lần cuối:** 2026-08-24 (phiên làm việc máy #1 — MacBook, thầy có mở song song máy #3)

---

## 0. ⚠️ CHÍNH SÁCH MỚI: CỐ ĐỊNH CODE (từ 2026-08-23)

Thầy yêu cầu **đóng băng code** của app (`index.html`, `tkb-full-v2.html`) — Claude KHÔNG được
tự ý sửa/refactor/"cải thiện" bất kỳ phần code nào (HTML/CSS/JS, cấu trúc, logic) cho đến khi
thầy chủ động nhờ đổi cụ thể. Việc merge **DỮ LIỆU** (JSON lịch dạy mới từ thầy vào
`INITIAL_SCHEDULE`/`initial_schedule.json`) vẫn làm bình thường — đó không phải là sửa code.
Khi thầy nhờ sửa 1 phần hiển thị/tính năng cụ thể, chỉ làm ĐÚNG phạm vi đó, không lan sang sửa
thêm chỗ khác dù "tiện thể".

## 1. TRẠNG THÁI HIỆN TẠI

- **Phiên bản đang chạy chính thức:** v3.27
- **3 nơi đã đồng bộ khớp nhau:**
  - GitHub: `github.com/hcmtranle/tkb20262027hoanchinh` — commit `d61c27c`
  - Artifact chính: `https://claude.ai/code/artifact/8552c45d-c843-4f04-aa02-ac0ed341b2b6` — đã publish lại bản mới nhất (v3.27)
  - Netlify: tự build lại từ GitHub (không cần thao tác gì thêm)
- **Dữ liệu:** 29 lớp, 57 giáo viên, 1100 ô tiết học (con số "1098" ghi ở bản cũ của file này đã
  lỗi thời — v3.19 đã điền đủ lại 2 ô thiếu ở lớp 5.1, về đúng 1100)

## 2. NHỮNG GÌ ĐÃ HOÀN THÀNH (tóm tắt các đợt lớn, mới nhất trước)

0000. **v3.27 (2026-08-24, máy #1, thầy tiếp tục điều chỉnh khi mở song song máy #3):** thầy
   báo "Tab Xem Theo GV không tự cập nhật" khi đang mở 2 máy khác nhau — đã giải thích rõ đây là
   giới hạn kỹ thuật (localStorage không đồng bộ qua mạng giữa 2 máy vật lý), không phải lỗi
   code, xem thêm `loithuonggap.md` mục 19. Sau đó thầy gửi JSON mới (gửi trùng 2 lần, nội dung
   giống hệt nhau — chỉ merge 1 lần): 7 ô đổi ở lớp 4.1 và 5.2 (hoán đổi Toán/KH/GDTC/TH của 4.1,
   GDTC/Toán/NT(MT) của 5.2). Kiểm định đủ 8 tiêu chí, không lệch quota, không hồi quy tên GV.

000. **v3.26 (2026-08-24, máy #1, thầy có mở song song máy #3 — đã kiểm tra `git status`/`git
   fetch` trước khi thao tác, không có xung đột):** thầy báo "chiều qua update json chưa đúng,
   Artifact và Netlify chưa hiển thị giống nhau". So khớp JSON thầy gửi với `initial_schedule.json`
   hiện tại → chỉ lệch đúng **8 ô ở lớp 3.4** (chuyển Tiếng Anh từ Thứ 4 sang Thứ 2, hoán đổi
   Toán/TV/ĐĐ để lấp chỗ trống Thứ 4). Không hồi quy tên GV, không thêm/xóa ô. Merge xong, kiểm
   định đủ 8 tiêu chí (không lệch quota, không trùng GV/phòng máy...), push GitHub + publish lại
   Artifact. → Netlify sẽ tự build lại từ GitHub sau vài phút, giờ 3 nơi khớp nhau.

00. **v3.24 → v3.25 (2026-08-23, máy #1, sau lệnh "cố định code"):**
   - v3.24: GVCN lớp 2.6 và 3.6 tự đổi lịch dạy nội bộ (18 ô, chỉ trong cùng lớp/cùng GVCN,
     không ảnh hưởng lớp khác) — merge theo JSON thầy gửi, kiểm định đủ 8 tiêu chí.
   - v3.25 (SỬA CODE — có yêu cầu cụ thể từ thầy, không vi phạm chính sách cố định):
     - Thêm hộp thoại chọn hình thức in mỗi khi bấm bất kỳ nút In/Xuất PDF nào (`exportPDF()` →
       mở `#printChoiceModal` → `doPrint('bgh'|'class')`): "Nộp Ban Giám Hiệu" (ẩn tên GV dạy
       dưới môn học, qua class CSS `body.print-mode-bgh` + `.cell-teacher-note{display:none}`
       trong `@media print`) hoặc "In Cho Lớp" (giữ nguyên, hiện đủ tên GV).
     - Đổi `document.title` lúc in từ dạng code `TKB_Lop_1.1` sang `Thời Khóa Biểu - Lớp 1.1`
       (đẹp hơn nếu trình duyệt in kèm tiêu đề trang qua tùy chọn "Headers and footers") + thêm
       hướng dẫn tắt tùy chọn đó trong toast nếu muốn trang in hoàn toàn sạch.
     - `SUBJECT_DISPLAY` mở rộng thêm `TV→Tiếng Việt, TA→Tiếng Anh, CN→Công nghệ, ĐĐ→Đạo đức,
       TH→Tin học` (giống cơ chế `KH→Khoa học`/`TA(BN)` đã có) — chỉ ảnh hưởng ô lịch Tab 1&2,
       Ma Trận Toàn Trường vẫn giữ mã ngắn.
     - Footer: bỏ dòng "Phiên bản V2.4 (Branch: tkbnak2627V2)", thêm dòng "© Bản quyền thuộc về
       Trường Tiểu học Nguyễn An Khương" (thuần trình bày, không đụng logic sinh TKB).
     - **Lưu ý quan trọng đã hỏi lại và làm rõ với thầy:** KHÔNG ẩn dòng "Khối/GVCN/Tổng tiết"
       hay tiêu đề lớp ở đầu trang — thầy giữ nguyên. Vấn đề "dòng khó coi khi in" thực ra là do
       trình duyệt tự thêm tiêu đề trang, không phải nội dung web sinh ra.

0. **v3.20 → v3.23 (2026-08-23, máy #1):**
   - v3.21: Merge JSON thầy gửi (95 ô) — hết buổi lẻ Thầy Thái & Thầy Tạo, fix quota LSĐL lớp
     5.2 (1→2 tiết/tuần, đúng quy định 2 tiết/tuần), điền tên GV còn thiếu (hiển thị trống) ở
     lớp 3.4/3.6/4.6.
   - v3.22: Merge JSON thầy gửi (21 ô) — hết buổi lẻ NỐT cho GV STEM(2), GV STEM(3), GV CDS(1).
     **→ Nguyên tắc 3 (GV bộ môn tránh buổi lẻ) coi như HOÀN TẤT 100%.**
   - v3.23: Đổi tên hiển thị môn `TA(BN)` thành "Tiếng Anh với người nước ngoài" (đúng quy định)
     — Bảng Mã Màu hiện tên đầy đủ, ô lịch Tab "Xem Theo Lớp" & "Xem Theo GV" hiện 2 dòng
     ("Tiếng Anh" / "Với người NN"), Ma Trận Toàn Trường vẫn giữ mã ngắn `TA(BN)` (bảng dày,
     giữ nguyên như cách đã làm với "KH"→"Khoa học" trước đây). Sửa ở CẢ `SUBJECT_COLORS.name`
     (cho Legend) LẪN `SUBJECT_DISPLAY` (cho ô lịch) — xem thêm `loithuonggap.md`.
   - Cùng đợt: thầy yêu cầu **cố định code** kể từ nay (xem mục 0 phía trên).

## 2b. NHỮNG GÌ ĐÃ HOÀN THÀNH TRƯỚC ĐÓ (v3.17 trở về trước)

1. **Fix lỗi hiển thị/dữ liệu nền tảng:** lệch cột Tiết 8 bảng GV, auto-suggest GV thiếu cho 7 môn liên kết (CDS, IC3, CLB Stem, CLB KNS, CLB Toán TD, TA(BN), TA(T-K)), chuẩn hóa tên GV về dạng rút gọn, thêm GV thứ 57 (Thầy Lê Thành Tạo — Phó Hiệu trưởng, dạy Tin học 4 lớp 1.1-1.4), đồng bộ dữ liệu real-time giữa nhiều tab trình duyệt (sự kiện `storage`).
2. **Xếp lại TKB theo 3 quy tắc mới của thầy** (144 ô thay đổi, kiểm định đầy đủ):
   - Phòng máy 1 (Tin học TH 2018) + Phòng máy 2 (IC3) phối hợp dùng chung, không trùng phòng
   - CLB Toán Tư duy (29 lớp) + CLB Kỹ năng sống (17 lớp Khối 3-5) chuyển từ GV trung tâm sang **GVCN dạy**, xếp đúng **tiết cuối** (Tiết 7 Khối 1-2, Tiết 8 Khối 3-5)
   - CLB Công dân số + CLB Stem giữ nguyên (đã đúng buổi chiều, GV trung tâm)
3. **2 quy tắc bổ sung sau đó:**
   - Tối đa 1 tiết Toán/ngày (đã sửa 10 ô vi phạm)
   - Các môn 2 tiết/tuần (GDTC, TNXH, Khoa học, LSĐL) phải cách nhau ≥1 ngày, tránh nhàm chán (đã sửa 4 ô vi phạm)
4. **Hiển thị:** "Khoa học" viết đầy đủ thay vì "KH" (ở Tab Xem Theo Lớp & Tab Xem Theo GV — Ma Trận Toàn Trường vẫn giữ mã ngắn vì bảng quá dày)
5. **Fix CSS in:** `@page size` đổi từ từ khóa `A4 landscape` sang kích thước tường minh `297mm 210mm` — đáng tin cậy hơn qua nhiều trình duyệt/máy in ảo; thông báo hướng dẫn khi in cũng rõ hơn (nhắc đổi Layout sang Landscape nếu hộp thoại vẫn hiện khổ dọc).

## 3. CÔNG VIỆC CÒN DANG DỞ — CHƯA LÀM

**Nguyên tắc 3 (GV bộ môn tránh "buổi lẻ 1 tiết"):** ✅ **ĐÃ XONG HOÀN TOÀN** (v3.21 + v3.22,
2026-08-23) — thầy tự sửa thủ công rồi gửi JSON 2 lần, Claude merge + kiểm định đủ 8 tiêu chí
cả 2 lần. Không còn GV nào bị buổi lẻ (Thầy Thái, Thầy Tạo, GV STEM(2)/(3), GV CDS(1) đều hết).

Hiện tại **không có việc gì dang dở**. Code vẫn đang ở chính sách cố định (mục 0) — v3.25 là
ngoại lệ hợp lệ vì thầy nêu yêu cầu cụ thể (hộp thoại in, tên môn học, chân trang), không phải
Claude tự ý sửa. Việc kế tiếp sẽ là thầy gửi JSON dữ liệu mới (merge theo quy trình mục 5) hoặc
tiếp tục nêu yêu cầu sửa 1 phần hiển thị/tính năng cụ thể khác.

→ **Khi thầy gửi JSON mới:** làm đúng quy trình cũ — merge cẩn thận (đối chiếu ô nào là chỉnh
sửa thật vs dữ liệu baseline cũ của Artifact do chưa reload), kiểm định đủ 8 tiêu chí (xem
mục 5), rồi mới cập nhật.

## 4. FILE `tkb-V1.1-xep-lai.html` VÀ ARTIFACT V1.1 RIÊNG

Không còn là bản "thử nghiệm" nữa — nội dung của nó đã được **đè hoàn toàn** lên bản chính
thức (`tkb-full-v2.html`/`index.html`/`initial_schedule.json`). File + Artifact V1.1 riêng
(`1150a615-6300-4363-b2b7-269baf249ed7`) giờ chỉ còn giá trị lưu trữ lịch sử, KHÔNG cần
cập nhật thêm trừ khi thầy yêu cầu cụ thể.

## 5. QUY TRÌNH BẮT BUỘC KHI NHẬN FILE JSON MỚI TỪ THẦY

1. Đọc file, so sánh với `initial_schedule.json` hiện tại — tìm ô THAY ĐỔI thật (khác giá
   trị) **VÀ** ô BỊ XÓA thật (có ở hiện tại, mất trong file mới — đừng bỏ sót phần xóa!).
2. Kiểm tra file mới có dựa trên **baseline Artifact cũ** không (dấu hiệu: các ô đã biết là
   đã fix — VD tên GV dạng đầy đủ "Trần Thị Diễm Linh" thay vì rút gọn — vẫn xuất hiện sai
   trong file). Nếu có, **lọc bỏ** các ô này khi merge, đừng để hồi lại lỗi cũ.
3. Quét lỗi GV sai chuyên môn (dùng `TEACHERS_CONFIG[teacher_id].subject` đối chiếu với
   `cell.subject`, bỏ qua GV loại `gvcn` vì họ dạy nhiều môn).
4. Sau merge: chạy `normalizeTeacherNames()` (đã có sẵn trong code, tự động chuẩn hóa tên).
5. Kiểm định đủ: (a) tổng số ô đúng, (b) không lệch quota môn/lớp, (c) không trùng GV,
   (d) không trùng Phòng máy 1&2, (e) IC3 luôn giữ cặp 2 tiết liền nhau, (f) CLB Toán TD/KNS
   đúng tiết cuối + GVCN, (g) tối đa 1 Toán/ngày, (h) GDTC/TNXH/Khoa học/LSĐL cách nhau ≥1 ngày.
6. Đồng bộ `initial_schedule.json` ↔ `INITIAL_SCHEDULE` trong cả `tkb-full-v2.html` VÀ
   `index.html` (2 file phải **giống hệt nhau**, dùng `diff` kiểm tra).
7. Commit + push GitHub, publish lại Artifact chính — **chỉ khi thầy xác nhận đồng ý**, trừ
   khi thầy đã cho phép trước (như lần "đè lên bản đang dùng" vừa rồi).

## 6. QUY TRÌNH 3 MÁY (đã ghi trong bộ nhớ Claude, nhắc lại ở đây cho session khác đọc)

- **Máy #1 = MacBook cá nhân** | **Máy #2 = máy bàn ở nhà** | **Máy #3 = máy cơ quan**
- Cả 3 máy dùng Google Drive Desktop đồng bộ TOÀN BỘ thư mục dự án (kể cả `.git`) — nghĩa là
  chia sẻ chung 1 lịch sử Git qua cơ chế đồng bộ file của Drive, không phải 3 bản clone riêng.
- **Rủi ro:** nếu chạy lệnh git ngay khi Drive chưa đồng bộ xong từ máy vừa dùng trước đó, có
  thể lỗi (`.git/index.lock`, file "xung đột" do Drive tạo). → Luôn đợi Drive sync xong (tick
  xanh) + chạy `git log -5 --oneline` và `git status` trước khi thao tác.
- **Kiến trúc dữ liệu:** Artifact (localStorage trình duyệt, thầy sửa trực tiếp) → GitHub (khi
  Claude push) → Netlify (tự build lại, chỉ hiển thị tĩnh). localStorage trên Artifact **ưu
  tiên hơn** dữ liệu mới publish — nghĩa là nếu thầy F5 mà vẫn thấy dữ liệu cũ, đó là vì trình
  duyệt giữ bản đã lưu cục bộ, không phải do publish thất bại.
- **Sửa trên Artifact có tự lưu không?** CÓ — mỗi lần sửa 1 ô qua modal "Sửa Nhanh", hàm
  `saveCell()` tự gọi `saveSchedule()` ngay, ghi vào `localStorage` lập tức, không cần bấm nút
  "💾 Lưu Thời Khóa Biểu" riêng. NHƯNG đó chỉ là lưu CỤC BỘ (1 trình duyệt, 1 máy) — không tự
  đẩy về GitHub/không đồng bộ máy khác. Muốn đưa vào bản chính thức: thầy xuất JSON từ Artifact
  gửi cho Claude → Claude merge thủ công theo quy trình mục 5.

## 7. LIÊN KẾT QUAN TRỌNG

- **Artifact chính:** https://claude.ai/code/artifact/8552c45d-c843-4f04-aa02-ac0ed341b2b6
- **Artifact V1.1 (lịch sử):** https://claude.ai/code/artifact/1150a615-6300-4363-b2b7-269baf249ed7
- **GitHub:** https://github.com/hcmtranle/tkb20262027hoanchinh
- **Tài liệu kỹ thuật đầy đủ:** `MOTA.md` (lịch sử phiên bản chi tiết từng dòng)
- **Lỗi thường gặp / kinh nghiệm rút ra:** `loithuonggap.md` (đọc trước khi merge JSON hoặc
  sửa code, để tránh lặp lại các lỗi đã từng gặp)
