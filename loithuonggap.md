# ⚠️ LỖI THƯỜNG GẶP & KINH NGHIỆM RÚT RA — Dự Án TKB

> Đọc file này TRƯỚC khi: (1) merge file JSON mới từ thầy, (2) thêm môn học/tính năng mới,
> (3) viết thuật toán xếp lịch/kiểm định. Mục tiêu: không lặp lại lỗi đã từng mất nhiều thời
> gian để tìm ra và sửa trong dự án này.

---

## 1. Thêm môn học mới → LUÔN nhớ cập nhật `pickSubject()` (auto-suggest GV)

**Lỗi đã gặp (2 lần, cùng 1 gốc):** Môn `NT(MT)` rồi sau đó 7 môn liên kết (`CDS`, `IC3`,
`CLB Stem`, `CLB KNS`, `CLB Toán TD`, `TA(BN)`, `TA(T-K)`) đều thiếu nhánh xử lý trong hàm
`pickSubject()` — khi Admin chọn môn trong modal Sửa Nhanh, ô chọn GV **giữ nguyên giá trị cũ**
(thường là GVCN) thay vì tự nhảy đúng GV, và không có gì cảnh báo cả (dễ lưu nhầm mà không
biết). Bug này còn tái diễn 2 lần trên chính dữ liệu thầy gửi (lớp 5.4 IC3 bị gán nhầm GV STEM).

**Quy tắc:** mỗi khi có môn mới cần auto-suggest GV theo lớp, PHẢI: (a) thêm field GV tương
ứng vào `CLASSES_CONFIG` mỗi lớp, (b) thêm nhánh `else if (code === '...')` trong
`pickSubject()`, (c) nếu môn không áp dụng cho khối đó, đảm bảo ô chọn GV **reset về rỗng**
(không giữ giá trị cũ) để bắt buộc chọn tay thật sự.

## 2. Nhiều tab/cửa sổ trình duyệt KHÔNG tự đồng bộ dữ liệu

**Lỗi:** `schedule` chỉ đọc từ `localStorage` 1 lần lúc `init()`. Sửa ở tab này không tự
phản ánh sang tab khác (kể cả khi cả 2 đang mở cùng lúc) — dễ khiến kiểm định trùng tiết báo
sai vì dựa trên dữ liệu lỗi thời.

**Đã fix:** thêm `window.addEventListener('storage', ...)` — chuẩn W3C, tự bắn ở các tab
KHÁC (không phải tab vừa ghi) mỗi khi `localStorage` thay đổi.

## 3. Publish Artifact KHÔNG tự xóa `localStorage` cũ của trình duyệt thầy

**Hiểu lầm cần tránh:** "Đã publish lại mà thầy F5 vẫn thấy lỗi cũ" — không phải do publish
thất bại. App ưu tiên đọc `localStorage` (nếu có dữ liệu thật) hơn `INITIAL_SCHEDULE` mới
publish. Nghĩa là: mọi lần Claude publish dữ liệu mới, **Artifact thầy đang mở vẫn giữ bản
cũ trong bộ nhớ trình duyệt** cho đến khi thầy tự sửa lại tay hoặc dùng "Khôi Phục Gốc".
→ Khi báo thầy đã publish xong, cần nói rõ: "F5 có thể chưa đủ, vì trình duyệt vẫn giữ dữ
liệu thầy đã lưu trước đó".

## 4. File JSON thầy gửi thường dựa trên baseline Artifact CŨ (chưa publish)

**Lỗi:** Vì Artifact của thầy chỉ cập nhật code khi Claude **chủ động publish**, nếu thầy
export JSON trước khi Claude kịp publish bản mới, file đó vẫn mang các lỗi ĐÃ FIX trong
repo (VD: 4 ô Thầy Tạo, 6 ô tên GV dạng đầy đủ). Nếu merge thẳng theo kiểu "lấy toàn bộ",
sẽ **hồi lại (regression)** các lỗi đã sửa.

**Quy tắc merge an toàn:**
1. Trước khi merge, kiểm tra vài ô "đã biết là đã fix" trong file mới gửi — nếu vẫn sai,
   xác nhận đây là baseline cũ.
2. Chỉ áp dụng ô THỰC SỰ khác giá trị so với `initial_schedule.json` hiện tại, **trừ** các
   ô nằm trong danh sách "đã biết đã fix" (giữ nguyên bản đã fix, không lấy từ file mới).
3. **Đừng quên phần XÓA:** ô có trong hiện tại nhưng KHÔNG có trong file mới = thầy đã xóa
   thật — phải áp dụng xóa đó, không chỉ xét ô thay đổi giá trị. (Từng bỏ sót việc này khiến
   thầy báo "xóa bên Lớp nhưng bên GV Trung tâm vẫn còn".)

## 5. Node `vm` sandbox: KHÔNG thể "tiêm" biến trước khi chạy script có `let x = {}`

**Lỗi nghiêm trọng (từng ghi đè `initial_schedule.json` thành rỗng!):** Gán
`sandbox.schedule = duLieuMoi` RỒI MỚI `vm.runInContext(toanBoScriptApp, sandbox)` — vô ích,
vì script app có `let schedule = {}` ở đầu, khai báo `let` sẽ **ghi đè** giá trị đã gán trước
đó trong cùng scope, không phải merge.

**Cách đúng:** chạy `vm.runInContext(scriptApp, sandbox)` trước (để `let schedule` được khai
báo bình thường bên trong sandbox), SAU ĐÓ mới chạy 1 đoạn code khác (`vm.runInContext` lần 2)
để **GÁN LẠI** (`schedule = duLieuMoi;`, không phải khai báo lại) rồi gọi hàm cần dùng.

## 6. LUÔN kiểm tra số lượng ô TRƯỚC KHI ghi đè file, đặc biệt sau bug #5

Sau sự cố #5, quy tắc bắt buộc: bất kỳ thao tác nào sắp ghi đè `initial_schedule.json` hay
`INITIAL_SCHEDULE`, PHẢI kiểm tra `Object.keys(sched).length === 29` và tổng số ô đúng kỳ
vọng TRƯỚC khi `fs.writeFileSync`. Nếu sai, dừng lại ngay, không ghi.

## 7. Thuật toán xếp lại: PHẢI snapshot/rollback theo từng đơn vị (lớp)

**Lỗi:** viết thuật toán "gỡ hết môn X rồi xếp lại" mà không snapshot trước — khi thất bại
giữa chừng (không tìm được chỗ đặt), môn X bị xóa mất mà không khôi phục, để lại lớp thiếu
tiết mà không hề báo lỗi rõ ràng (mất tới ~10 phút mới phát hiện qua kiểm định).

**Quy tắc:** trước khi biến đổi 1 đơn vị (VD: 1 lớp), luôn `JSON.parse(JSON.stringify(...))`
snapshot lại; nếu thất bại giữa chừng, `Object.assign` khôi phục snapshot rồi mới `continue`
sang đơn vị tiếp theo — không bao giờ để dữ liệu ở trạng thái dở dang.

## 8. Các bước sửa lỗi độc lập CÓ THỂ làm hỏng lẫn nhau — luôn kiểm tra chéo lần cuối

**Lỗi:** Sau khi B1 xếp đúng "CLB Toán TD vào tiết cuối", B2/B3 (xử lý IC3/TH, rồi TV) coi
Toán TD là "môn GVCN an toàn, dời đâu cũng được" (đúng về mặt tránh trùng GV) nhưng lại LÀM
SAI vị trí bắt buộc (tiết cuối) mà B1 vừa thiết lập. Tương tự: sửa "giãn cách môn 2 tiết/tuần"
(B mới) vô tình đưa thêm 1 tiết Toán vào ngày đã có Toán, phá lại quy tắc "tối đa 1 Toán/ngày"
(B trước).

**Quy tắc:** các ô có RÀNG BUỘC VỊ TRÍ CỐ ĐỊNH (không chỉ tránh trùng GV) phải được liệt kê
vào danh sách `PROTECTED`/loại trừ khi các bước SAU dùng làm "ô an toàn để hoán đổi". Sau
MỌI đợt sửa nhiều bước, luôn chạy lại **toàn bộ** các kiểm định cũ (không chỉ kiểm định của
bước vừa làm) để bắt lỗi hồi quy chéo giữa các bước.

## 9. Console Windows (cp1252) không in được tiếng Việt Unicode trực tiếp

**Lỗi:** `python -c "print(chuoi_tieng_viet)"` báo `UnicodeEncodeError` trên Windows dù nội
dung file/PDF vẫn đúng. Không phải lỗi dữ liệu — chỉ là giới hạn hiển thị console.

**Cách tránh:** khi cần xem/kiểm tra nội dung tiếng Việt qua Python hay Node trên Windows,
LUÔN ghi ra file (`encoding='utf-8'`) rồi đọc file đó, đừng `print()`/`console.log()` thẳng
ra console nếu có khả năng chứa dấu tiếng Việt phức tạp.

## 10. Artifact mới (URL khác) KHÔNG tự thừa hưởng `capabilities` của Artifact cũ

**Lỗi:** Publish 1 Artifact MỚI (khác URL) với cùng code có nút "Tải File Nén (.zip)" (dùng
API `downloads`) — bị cảnh báo link tải không hoạt động, vì `capabilities: {downloads: true}`
phải khai báo riêng cho từng Artifact, không tự động mang theo khi tạo bản mới.

## 11. `window.open()` bị chặn trong khung Artifact — dùng `window.print()` trực tiếp

Đã từng thử "mở cửa sổ mới rồi in" — thất bại vì Artifact chạy trong iframe, `window.open()`
trả về `null`. Giải pháp ổn định: gọi `window.print()` ngay trên trang hiện tại, dựa vào CSS
`@media print` đã tối ưu sẵn. Nút "Tải File Nén" tương tự — dùng `window.claude.downloads`
(khi có capability) thay vì thẻ `<a download>` truyền thống (bị chặn trong Artifact).

## 12. CSS `@page{size:A4 landscape}` không đáng tin cậy bằng kích thước tường minh

Một số trình duyệt/máy in ảo không áp dụng đúng hướng giấy với cú pháp từ khóa
`A4 landscape`. Đổi sang kích thước tường minh `297mm 210mm` (ngang) đáng tin cậy hơn.
Vẫn nên giữ hướng dẫn tay dự phòng (đổi Layout sang Landscape) vì trình duyệt có toàn quyền
quyết định cuối cùng, không trang web nào ép được 100%.

## 13. Google Drive đồng bộ `.git` giữa 3 máy — rủi ro nếu thao tác khi chưa sync xong

Xem chi tiết trong bộ nhớ Claude (`quy-trinh-3-may-tinh.md`) và `dongbophienlamviec.md`.
Tóm tắt: luôn đợi Drive báo đồng bộ xong + `git log`/`git status` trước khi commit/push trên
bất kỳ máy nào trong 3 máy.

## 14. Đổi tên hiển thị 1 môn học phải sửa ĐÚNG 2 CHỖ, không phải 1

**Bối cảnh (v3.23):** thầy nhờ đổi hiển thị `TA(BN)` thành "Tiếng Anh với người nước ngoài".
Tưởng chỉ cần sửa 1 chỗ nhưng thực ra có 2 cơ chế hiển thị độc lập:

1. **`SUBJECT_COLORS[code].name`** — tên đầy đủ, chỉ xuất hiện ở "🎨 Bảng Mã Màu" (Legend).
   KHÔNG dùng ở modal "Sửa Nhanh" (`renderSubjectGrid` hiển thị thẳng `code`, không phải `name`).
2. **`SUBJECT_DISPLAY` + hàm `displaySubj()`** — áp dụng cho ô lịch ở Tab "Xem Theo Lớp" và
   Tab "Xem Theo GV" (2 tab này gọi chung `displaySubj()`). Đây là cơ chế đã có sẵn từ trước
   cho `"KH"` → `"Khoa học"`. Muốn hiện nhiều dòng trong ô nhỏ, giá trị trong map này có thể là
   HTML (VD: `'Tiếng Anh<div style="...">Với người NN</div>'`) vì nó được nhúng thẳng vào
   `innerHTML`, không escape — an toàn vì chỉ 2 nơi gọi `displaySubj()`, cả 2 đều là ngữ cảnh
   `innerHTML` (không phải Excel/PDF export dùng text thuần).

**Cố ý KHÔNG đổi:** "Ma Trận TKB Toàn Trường" (`renderMasterMatrix`) không gọi `displaySubj()`
— giữ nguyên mã ngắn vì bảng quá dày, đổi tên dài sẽ vỡ layout. Đây là quyết định thiết kế có
chủ đích từ trước (áp dụng cho cả "KH"), không phải thiếu sót.

**Quy tắc:** trước khi đổi tên hiển thị 1 môn, `grep` toàn bộ chuỗi tên cũ trong file để tìm hết
các nơi nó xuất hiện (thường là 2: field `.name` cho Legend, và `SUBJECT_DISPLAY` cho ô lịch),
đừng chỉ sửa chỗ đầu tiên tìm thấy.

## 15. Sau khi thầy yêu cầu "cố định code" — KHÔNG tự sửa code khi không được nhờ

Từ 2026-08-23 thầy yêu cầu đóng băng code (`index.html`/`tkb-full-v2.html`) — xem
`dongbophienlamviec.md` mục 0. Việc merge DỮ LIỆU (JSON lịch dạy) vẫn bình thường. Chỉ sửa CODE
khi thầy nêu yêu cầu cụ thể cho đúng phần đó — không lan sang sửa/dọn dẹp/tối ưu chỗ khác dù
"tiện thể đang mở file ra sửa".

## 16. "Dòng khó coi khi in" — coi chừng nhầm giữa nội dung web và tiêu đề trang do TRÌNH DUYỆT tự thêm

**Bối cảnh (v3.25):** thầy phàn nàn có "1 dòng ví dụ 'tkb 1.1' rất thiếu thẩm mỹ" xuất hiện khi
in, ban đầu tưởng là dòng "Khối/GVCN/Tổng tiết" trong `.class-meta` (nội dung web tự sinh) —
HỎI LẠI mới biết KHÔNG PHẢI, mà là **tiêu đề trang do trình duyệt tự in thêm** (tùy chọn
"Headers and footers" trong hộp thoại in, mặc định lấy `document.title`) — đúng lúc code đang
chủ động đặt `document.title = 'TKB_Lop_' + selectedClass` (dạng "TKB_Lop_1.1", trông như code)
để gợi ý tên file khi "Save as PDF".

**Bài học:** khi thầy mô tả "1 dòng xấu xuất hiện khi in" mà dòng đó trông như 1 slug/code (có
gạch dưới, viết tắt) chứ không giống câu tiếng Việt bình thường trong trang, khả năng cao đó là
`document.title` bị trình duyệt in kèm, KHÔNG PHẢI nội dung HTML của trang. Cách xử lý: (1) đổi
`document.title` sang dạng tên đọc được, có dấu, có khoảng trắng (VD: `"Thời Khóa Biểu - Lớp
1.1"`) thay vì dạng slug; (2) không thể dùng CSS/JS để tắt hẳn "Headers and footers" của trình
duyệt (đây là tùy chọn phía người dùng trong hộp thoại in, ngoài tầm kiểm soát của trang web) —
chỉ có thể hướng dẫn thầy tự tắt nếu muốn trang in hoàn toàn sạch (giống bài học #12 về
`@page size`: trình duyệt luôn có tiếng nói cuối cùng).

## 25. Khi câu lệnh của thầy không khớp dữ liệu thực tế (VD nhầm GV/lớp tham chiếu) — hỏi lại ngay, đừng đoán; thầy có thể tự tính lại và gửi thẳng JSON

**Bối cảnh (v3.35-v3.36):** thầy nhắn "Lớp 3.6 em chuyển thành GV-TA(BN)_2 (dưới lớp 2.4)" —
tra dữ liệu thấy 2 điểm không khớp: lớp 3.6 vốn đã dùng đúng GV TA-BN (2) rồi (không cần đổi
GV), còn lớp 2.4 lại đang dùng GV TA-BN (1) khác hẳn. Câu lệnh mơ hồ đến mức không thể suy luận
an toàn ý thầy muốn dời sang slot nào. Đã dùng `AskUserQuestion` hỏi lại rõ, nêu đúng dữ liệu
tra được kèm 2-3 phương án cụ thể, thay vì tự chọn 1 phương án rồi làm liều.

**Kết quả:** thầy không trả lời trực tiếp câu hỏi mà tự tính toán lại phương án (không đụng
Âm nhạc, không đụng TA(BN)) rồi gửi thẳng 1 file JSON đầy đủ toàn trường. Khi nhận JSON như vậy,
**luôn diff với `initial_schedule.json` hiện tại trước** (không đọc thủ công file lớn) để xác
định đúng phạm vi thầy thực sự đổi — ở đây diff cho thấy JSON chỉ khác đúng 11 ô ở 2 lớp (3.4,
3.6), 27 lớp còn lại y hệt, khớp với lời thầy dặn "chỉ cập nhật 2 lớp 3 thôi". Diff-trước-khi-
đọc còn giúp phát hiện ngay nếu thầy gửi nhầm file cũ hoặc file có thay đổi ngoài ý muốn.

## 24. Khi thầy tự chốt phương án dời GV bộ môn/liên kết — vẫn PHẢI tự kiểm tra chéo lịch GV, đừng tin tưởng tuyệt đối

**Bối cảnh (v3.34):** thầy tự đề xuất cụ thể dời TA(BN) lớp 3.6 sang Thứ 2 tiết 5-6. Khi kiểm
tra lại, phát hiện GV đó (t_tabn_2) **đang dạy lớp KHÁC (3.2) đúng slot Thứ 2 tiết 5-6** — nếu
làm theo sẽ tạo trùng GV thật. Thầy không có đủ thông tin lịch dạy TOÀN TRƯỜNG của từng GV liên
kết (họ dạy nhiều lớp khác nhau) nên khó tự kiểm tra hết bằng mắt.

**Quy tắc:** dù thầy đã "chốt" rất cụ thể (ngày/tiết/lớp rõ ràng), vẫn LUÔN tự kiểm tra lại lịch
GV liên quan trên TOÀN TRƯỜNG trước khi ghi file — không tự tin phương án của thầy chắc chắn đã
loại trừ hết xung đột, vì thầy không có công cụ tra cứu lịch toàn trường theo GV như mình.

**Bài học kỹ thuật đi kèm:** khi tự động tìm slot thay thế cho 1 ô cần dời, phải kiểm tra AN
TOÀN THEO CẢ 2 CHIỀU của phép hoán đổi — không chỉ "ô đang muốn đưa đến có phù hợp cho GV mới
không", mà còn "môn đang bị đẩy NGƯỢC LẠI vị trí cũ có làm GV của nó (nếu là GV bộ môn, không
phải GVCN) trùng lịch nơi khác không". Cách an toàn tuyệt đối: chỉ hoán đổi với ô do CHÍNH
GVCN lớp đó dạy (GVCN chỉ có đúng 1 lớp, không bao giờ tự trùng lịch với ai) — nếu không đủ chỗ
GVCN mới cân nhắc phương án phức tạp hơn kèm kiểm tra chéo kỹ hơn.

## 23. Quy trình mới đã dùng thành công: Claude tự sửa `initial_schedule.json` trực tiếp theo lời nhắn của thầy — không cần JSON — khi giá trị đã biết chính xác 100%

**Bối cảnh (v3.31):** thầy nhờ Claude *chỉ đọc/quét/đề xuất* (không tự sửa), Claude đề xuất
phương án cụ thể (bảng hoán đổi từng ô), thầy "thống nhất" và chỉnh chi tiết bằng lời nhắn text
thuần (không gửi file JSON). Vì phương án đã đủ chi tiết (biết chính xác lớp nào, ô nào, giá
trị mới là gì), Claude áp dụng trực tiếp vào `initial_schedule.json` mà KHÔNG cần thầy thao tác
tay trên Artifact rồi xuất JSON gửi lại — rút ngắn 1 vòng lặp không cần thiết.

**Khi nào dùng cách này (không qua JSON):** chỉ khi TOÀN BỘ giá trị ô cần đổi đã rõ ràng, không
mơ hồ (đã biết chính xác lớp/ngày/tiết/môn/GV mới) — ví dụ qua đề xuất trước đó đã được thầy
duyệt cụ thể. Nếu thầy mô tả chung chung ("đổi vài chỗ cho hợp lý hơn") thì vẫn nên xin JSON
như thường, vì tự đoán giá trị dễ sai.

**Vẫn phải làm đủ:** mô phỏng thử trong bộ nhớ (chưa ghi file) → kiểm định đủ 8 tiêu chí → chỉ
ghi file thật khi kiểm định sạch → rồi mới commit/push/publish như quy trình chuẩn. Không được
bỏ qua bước kiểm định chỉ vì đây là lệnh trực tiếp từ thầy thay vì từ JSON.

## 22. `git status` báo file vừa STAGED vừa UNSTAGED cùng lúc — dấu hiệu `.git/index` bị Drive đè từ máy khác

**Bối cảnh (2026-08-24, sau khi thầy chuyển từ máy #3 sang máy #1/#2):** mở phiên mới, `git
status` báo 5 file "Changes to be committed" (staged) VÀ "Changes not staged for commit"
(unstaged) CÙNG LÚC — bất thường vì bình thường 1 file chỉ ở 1 trong 2 trạng thái tại 1 thời
điểm (trừ khi sửa thêm sau khi đã `git add`). Nội dung staged còn cho thấy dữ liệu CŨ hơn cả
HEAD hiện tại (như quay lại trước v3.26).

**Nguyên nhân xác định:** file `.git/index` (vùng staging, khác với working tree) bị Google
Drive đồng bộ đè từ 1 máy khác đang có 1 checkout/thao tác git cũ hơn — không phải do dữ liệu
trên đĩa (working tree) bị hỏng.

**Cách xử lý an toàn:**
1. TRƯỚC KHI làm bất cứ gì, so sánh file trên đĩa với HEAD: `diff <(git show HEAD:file) file`.
   Nếu khớp HEAD → working tree vẫn đúng, chỉ index bị lệch, an toàn để sửa.
2. Chạy `git reset` (KHÔNG có `--hard`) để đưa index về khớp HEAD — lệnh này CHỈ đụng vào
   index, không đụng working tree, nên không có rủi ro mất dữ liệu đang có trên đĩa.
3. Kiểm tra lại `git status` sạch, rồi mới tiếp tục thao tác bình thường.

**Tuyệt đối không** vội `git add -A && git commit` khi thấy trạng thái staged/unstaged lẫn lộn
này — có thể vô tình commit dữ liệu CŨ đè lên bản mới nếu không kiểm tra kỹ trước.

## 21. `window.print()` bọc trong `setTimeout` có thể bị trình duyệt âm thầm bỏ qua — luôn gọi đồng bộ trong sự kiện click

**Bối cảnh (v3.29):** nút "Xuất PDF"/"In" không tự mở hộp thoại in được, Thầy/Cô phải tự bấm
Ctrl+P mới in được. Code cũ gọi `window.print()` bên trong `setTimeout(..., 300)` (delay 300ms
sau khi bấm nút) — nhiều trình duyệt/khung hiển thị (đặc biệt trong sandbox như Artifact) chỉ
cho phép các API cần "user activation" (in, mở popup...) chạy khi được gọi TRỰC TIẾP, ĐỒNG BỘ,
trong cùng 1 tick với sự kiện click gốc. Một khi đã trì hoãn qua `setTimeout`/`Promise` dù chỉ
vài trăm mili-giây, "user activation" có thể bị coi là hết hạn ở 1 số trình duyệt/khung, khiến
lệnh bị bỏ qua HOÀN TOÀN MÀ KHÔNG BÁO LỖI GÌ (không throw exception, `catch` không bắt được).

**Cách xác nhận:** giả lập `window.print = () => { window.__called = true }` trước khi bấm nút,
rồi kiểm tra `window.__called` ngay sau lệnh gọi hàm xử lý — nếu `true` ngay lập tức (không cần
đợi timeout trôi qua) nghĩa là đã gọi đồng bộ, đúng yêu cầu.

**Quy tắc:** bất kỳ API nào cần "user gesture" (`window.print()`, `window.open()`,
`navigator.clipboard.writeText()`, xin quyền camera/mic...) PHẢI được gọi trực tiếp trong hàm
xử lý sự kiện `onclick`, không qua `setTimeout`/`await` trước đó. Việc hiển thị toast hướng dẫn
hay đổi `document.title` (không cần user gesture) vẫn có thể làm trước/sau tùy ý, chỉ riêng
lệnh gọi API nhạy cảm là phải đứng đầu, không trì hoãn.

## 20. Chuyển 1 tiết TH/IC3 sang slot mới — LUÔN kiểm tra lại tiêu chí (d) trùng phòng máy, kể cả khi JSON chỉ đụng 1 lớp

**Bối cảnh (v3.28, 2026-08-24):** thầy gửi JSON đổi tiết Tin học lớp 3.4 sang 1 slot mới —
nhìn qua tưởng chỉ ảnh hưởng lớp 3.4 (đúng như JSON diff chỉ hiện thay đổi ở 1 lớp), nhưng khi
kiểm định tiêu chí (d) mới phát hiện slot mới đó ĐÃ CÓ SẴN 2 lớp khác (1.4-TH, 3.6-IC3) đang
dùng phòng máy — cộng thêm 3.4 nữa thành 3 lớp, vượt quá 2 phòng máy hiện có của trường.

**Bài học:** "diff JSON chỉ động 1 lớp" KHÔNG có nghĩa "an toàn, không ảnh hưởng lớp khác" —
với các môn dùng TÀI NGUYÊN DÙNG CHUNG (phòng máy TH/IC3, GV bộ môn dạy nhiều lớp...), một thay
đổi ở 1 lớp vẫn có thể tạo xung đột với dữ liệu CÓ SẴN ở lớp khác tại đúng slot đó. Vì vậy tiêu
chí (d) (và (c) trùng GV) PHẢI luôn chạy kiểm định trên TOÀN BỘ 29 lớp sau merge, không chỉ xét
riêng lớp vừa đổi. Khi phát hiện vi phạm: dừng lại, KHÔNG tự ý ghi đè, báo rõ cho thầy (nêu cụ
thể slot nào, lớp nào đang xung đột) và hỏi hướng xử lý — không tự đoán slot thay thế.

## 19. "Tab Xem Theo GV không tự cập nhật" khi mở 2 máy khác nhau — giới hạn `localStorage`, không phải bug

**Bối cảnh (2026-08-24):** thầy báo khi sửa 1 ô ở Tab "Xem Theo Lớp" trên máy #1, Tab "Xem Theo
GV" đang mở sẵn trên **máy #3** không tự thấy thay đổi. Đây KHÔNG phải lỗi code — cơ chế
`window.addEventListener('storage', ...)` (tự làm mới khi có thay đổi từ tab/cửa sổ khác) và
`refreshTeacherViewIfOpen()` (tự làm mới Tab GV ngay sau khi lưu 1 ô) đều **chỉ hoạt động trong
CÙNG 1 trình duyệt, trên CÙNG 1 máy vật lý** — `localStorage` không đi qua mạng, 2 máy khác nhau
luôn có 2 bản dữ liệu độc lập hoàn toàn tách biệt.

**Cách xác nhận nhanh khi thầy báo hiện tượng tương tự:** hỏi ngay "2 tab đang mở trên cùng 1
máy hay 2 máy khác nhau?" — nếu 2 máy khác nhau, đây luôn là giới hạn thiết kế (không sửa được
bằng code), chỉ giải quyết được bằng quy trình xuất JSON → merge → publish → F5/Khôi Phục Gốc
ở máy còn lại (xem `dongbophienlamviec.md` mục 6).

## 18. "Artifact và Netlify hiển thị khác nhau" — thường do thầy sửa tay trên Artifact nhưng chưa xuất JSON gửi Claude

**Bối cảnh (v3.26, 2026-08-24):** thầy báo "Artifact và Netlify chưa hiển thị giống nhau".
Nguyên nhân thực tế: Netlify build tĩnh từ GitHub (chỉ cập nhật khi Claude commit+push), còn
Artifact hiển thị theo `localStorage` của trình duyệt thầy — nếu thầy tự sửa vài ô trực tiếp
trên Artifact (qua "Sửa Nhanh") mà CHƯA xuất JSON gửi Claude, thì `localStorage` (Artifact) đã
có thay đổi mới nhưng GitHub/Netlify vẫn ở bản cũ → 2 nơi lệch nhau. Đây KHÔNG phải lỗi merge
của Claude, mà là hệ quả tất yếu của kiến trúc "Artifact = nháp cục bộ, GitHub = bản chính thức"
đã ghi ở `dongbophienlamviec.md` mục 6. Cách xác nhận nhanh: so khớp JSON thầy mới xuất với
`initial_schedule.json` hiện tại — nếu số ô lệch nhỏ và tập trung ở đúng chỗ thầy vừa chỉnh tay,
đó chính là các ô chưa kịp đồng bộ, chỉ cần merge bình thường theo quy trình mục 5.

## 17. Ẩn 1 phần nội dung theo "chế độ" (VD: in nộp BGH vs in cho lớp) — dùng class trên `<body>` + `@media print`, đừng tạo 2 hàm render riêng

Khi cần hiển thị khác nhau tùy lựa chọn của người dùng (ở đây: ẩn tên GV khi in nộp BGH), cách
gọn nhất là: (1) gắn 1 class CSS lên phần tử cần ẩn ngay lúc render (`class="cell-teacher-note"`
cho dòng tên GV), (2) khi người dùng chọn chế độ, toggle 1 class trên `<body>`
(`print-mode-bgh`), (3) viết đúng 1 rule CSS bên trong `@media print`:
`body.print-mode-bgh .cell-teacher-note{display:none!important;}`. KHÔNG viết lại hàm render
để có 2 phiên bản HTML khác nhau — vừa trùng lặp code vừa dễ lệch dữ liệu giữa 2 phiên bản.
