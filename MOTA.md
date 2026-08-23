# 📚 Hệ Thống Thời Khóa Biểu Tự Động
## Trường Tiểu Học Nguyễn An Khương — Năm Học 2026-2027

> **Phiên bản:** V2.8 — Ngày cập nhật: 22/08/2026  
> **Artifact:** https://claude.ai/code/artifact/8552c45d-c843-4f04-aa02-ac0ed341b2b6  
> **GitHub:** https://github.com/hcmtranle/tkb20262027hoanchinh

---

## 🗂️ Cấu Trúc Dự Án

```
Dự án TKB mới/
├── index.html             ← Bản sao của tkb-full-v2.html, dùng cho GitHub Pages
├── tkb-full-v2.html       ← File chính — mở trình duyệt là dùng ngay (~160KB)
├── initial_schedule.json  ← Dữ liệu TKB gốc 29 lớp (nhúng sẵn trong HTML)
├── MOTA.md                ← Tài liệu mô tả dự án (file này)
├── .gitignore             ← Loại trừ desktop.ini, .claude/ khỏi git
└── link.txt               ← Link artifact Claude
```

---

## 🚀 Cách Sử Dụng

**Cách 1 — Mở trực tiếp:**
```
Nhấp đúp tkb-full-v2.html → mở bằng Chrome/Edge → chạy ngay
```

**Cách 2 — Deploy GitHub Pages:**
```
1. Đổi tên tkb-full-v2.html → index.html
2. Push lên GitHub repo
3. Bật GitHub Pages → có URL công khai
```

**Cách 3 — Claude Artifact (đã publish):**
```
https://claude.ai/code/artifact/8552c45d-c843-4f04-aa02-ac0ed341b2b6
```

---

## 🎯 Tính Năng Đầy Đủ (V2.7)

### 📋 Tab 1 — Xem Theo Lớp
| Tính năng | Mô tả |
|-----------|-------|
| Lọc khối | Tất cả / Khối 1 / Khối 2 / Khối 3 / Khối 4 / Khối 5 |
| Chọn lớp | 29 lớp — click vào pill chọn lớp |
| Bảng TKB | Header 2 cột (UBND trái, CHXHCNVN phải) + tiêu đề lớp căn giữa |
| Cột bảng | BUỔI (SÁNG/CHIỀU, rowspan đúng) \| TIẾT \| THỜI GIAN \| THỨ 2→6 |
| Ô môn học | Card bo tròn, màu sắc riêng từng môn, icon, tên giáo viên |
| Giờ nghỉ | Ra chơi sáng / Bán trú & Nghỉ trưa / Ra chơi chiều |
| Thống kê | Số tiết/môn tự tính, tổng tiết/tuần |
| Chữ ký | Hiệu trưởng Hồ Thị Ngọc Diễm + GVCN |
| Bảng màu | 23 môn học với màu và biểu tượng |

### 👩‍🏫 Tab 2 — Xem Theo Giáo Viên & Lịch Báo Giảng
| Tính năng | Mô tả |
|-----------|-------|
| Lọc nhóm | Tất cả / Bộ môn & TPT (9) / GV Liên kết (18) / Chủ nhiệm (29) |
| Chọn GV | Dropdown đầy đủ 57 giáo viên |
| Bảng lịch | Hiển thị dạy lớp nào, tiết nào, môn gì |

### 📊 Tab 3 — Ma Trận TKB Toàn Trường
| Tính năng | Mô tả |
|-----------|-------|
| Tổng quan | 29 lớp × 40 tiết/tuần |
| Lọc khối | Xem từng khối riêng |
| Tìm kiếm | Tìm theo lớp/GVCN |

### 🛡️ Kiểm Định Quy Tắc (mới — v2.8)
| Tính năng | Mô tả |
|-----------|-------|
| Phát hiện trùng tiết | Tự động quét toàn trường tìm GV bị xếp dạy ≥2 lớp cùng lúc |
| Loại trừ hợp lệ | Chào cờ (HĐTN(CC)) — TPT phụ trách 29 lớp cùng lúc → **không** tính là lỗi |
| Banner cảnh báo | Hiện ở đầu bảng Tab 2 khi GV đang xem có tiết trùng, liệt kê rõ Thứ/Tiết/Lớp |
| Ô trùng tiết | Viền đỏ + nhãn "⚠️ TRÙNG TIẾT!" ngay trên bảng lịch GV |
| Kiểm tra khi Lưu | Mỗi lần Lưu/Xóa 1 tiết → tự chạy kiểm định toàn trường |
| Bong bóng kết quả | ✅ Toast xanh "Dữ liệu ĐÚNG quy tắc" nếu sạch, hoặc modal đỏ liệt kê từng lỗi nếu có |
| Đồng bộ GV bộ môn | Sửa 1 tiết ở Tab 1 → nếu đang mở Tab 2 sẽ tự render lại ngay, không cần F5 |

---

## 🔒 Chế Độ Admin

**Mật khẩu: `Tram@0211`**

| Nút | Ai thấy | Chức năng |
|-----|---------|-----------|
| 🔒 Đăng Nhập Quản Trị | Tất cả | Mở hộp nhập mật khẩu |
| ✏️ Bật Sửa Nhanh Từng Tiết | **Tất cả thấy** — chỉ admin dùng được | Click → tự mở login; sau login → edit mode bật |
| 💾 Lưu TKB | Tất cả | Lưu vào localStorage |
| 🔄 Cập Nhật Lịch Báo Giảng | Admin | (đang phát triển) |
| 🖨️ In / Xuất PDF | Admin | `window.print()` |
| ↩️ Khôi Phục Gốc | Admin | Reset về `initial_schedule.json` |
| 🚪 Đăng Xuất Admin | Admin | Thoát admin mode |

### Luồng Sửa Nhanh Từng Tiết:
```
Click [✏️ Bật Sửa Nhanh Từng Tiết]
  ↓ Chưa đăng nhập → mở hộp login tự động
  ↓ Nhập Tram@0211 → Đăng nhập
  ↓ Edit mode tự bật (thanh vàng nhấp nháy xuất hiện)
  ↓ Hover ô tiết → viền tím + icon ✏️
  ↓ Click ô → Modal chỉnh sửa mở ra:
      • Chọn môn (23 môn có màu & icon)
      • Chọn giáo viên (dropdown 57 GV)
      • Nhấn 💾 Lưu → lưu ngay, cập nhật bảng
  ↓ Click lại nút → tắt edit mode
```

---

## 🎨 Danh Sách 23 Môn Học

| Mã | Tên Môn | Màu |
|----|---------|-----|
| TV | Tiếng Việt | 🟡 Vàng nhạt |
| Toán | Toán | 🔵 Xanh dương |
| ĐĐ | Đạo đức | 🟡 Vàng |
| TNXH | Tự nhiên và Xã hội | 🟢 Xanh lá |
| KH | Khoa học | 🩵 Ngọc |
| LSĐL | Lịch sử và Địa lí | 🟠 Cam |
| HĐTN(CC) | HĐTN Chào cờ | 🔴 Đỏ nhạt |
| HĐTN(CĐ) | HĐTN Chủ đề | 🌸 Hồng |
| HĐTN(SHL) | HĐTN SHL | 🟣 Tím nhạt |
| GDTC | Giáo dục thể chất | 🟢 Xanh lá đậm |
| NT(MT) | Mĩ thuật | 🩷 Hồng đậm |
| NT(ÂN) | Âm nhạc | 🩵 Cyan |
| CN | Công nghệ | 🟢 Xanh chanh |
| TA | Tiếng Anh | 💜 Tím xanh |
| TH | Tin học | 🔷 Xanh nhạt |
| IC3 | Tin học Quốc tế IC3 | 🟣 Tím |
| CDS | CLB Công dân số | 🟤 Hồng tím |
| CLB Stem | CLB Stem | 🩵 Ngọc |
| CLB KNS | CLB KNS | 🟡 Vàng |
| TA(BN) | Tiếng Anh người NN | 🔵 Xanh |
| TA(T-K) | Tiếng Anh Toán-Khoa | 🩵 Cyan |
| CLB Toán TD | CLB Toán Tư duy | 🟢 Xanh |
| Tự học | Tự học có hướng dẫn | ⬜ Xám nhạt |

---

## 📊 Dữ Liệu Trường

| Hạng mục | Chi tiết |
|----------|----------|
| Tổng lớp | **29 lớp** (Khối 1–5) |
| GV Bộ môn & TPT | 9 GV |
| GV Liên kết (đối tác) | 18 GV |
| GVCN (Chủ nhiệm) | 29 GVCN |
| Tổng GV | **57 GV** |
| Hiệu trưởng | Hồ Thị Ngọc Diễm |
| Cơ quan chủ quản | UBND Phường Đông Hưng Thuận |

---

## 💾 Lưu Trữ Dữ Liệu

```
localStorage key  : tkb_nak_schedule_v4
sessionStorage key: tkb_nak_admin  (phiên Admin)

Thứ tự ưu tiên load:
1. localStorage (nếu có data thực, không rỗng)
2. INITIAL_SCHEDULE (nhúng trong HTML từ initial_schedule.json)

Ô đã sửa thủ công: đánh dấu ✏️ ở góc trên phải cell
```

---

## 🛠️ Kiến Trúc Kỹ Thuật

```
tkb-full-v2.html (~160KB, standalone — không cần CDN, npm, build)
├── <style>  — CSS thuần: header, nav, table, modal, toast, legend
├── <body>   — HTML cấu trúc:
│   ├── <header>   Tiêu đề + nút hành động
│   ├── <nav>      3 tab chính
│   ├── <main>
│   │   ├── #view-class    Tab Xem theo lớp
│   │   ├── #view-teacher  Tab Xem theo GV
│   │   └── #view-master   Tab Ma trận toàn trường
│   ├── #loginModal   Modal đăng nhập Admin
│   ├── #editModal    Modal sửa nhanh từng tiết
│   └── #toast        Thông báo nhanh
└── <script>
    ├── DATA
    │   ├── SUBJECT_COLORS  (23 môn, hex colors + icons)
    │   ├── TIME_SLOTS      (11 slots: 8 tiết + 3 giờ nghỉ)
    │   ├── DAYS            (T2→T6, tên đầy đủ)
    │   ├── CLASSES_CONFIG  (29 lớp: grade, GVCN, GV bộ môn)
    │   ├── TEACHERS_CONFIG (57 GV: specialist + partner + gvcn)
    │   └── INITIAL_SCHEDULE (toàn bộ TKB 29 lớp — flat map)
    └── FUNCTIONS
        ├── init()                ← Load data, render all, set date
        ├── switchTab()           ← Chuyển tab
        ├── filterGrade()         ← Lọc khối lớp
        ├── renderClassGrid()     ← Danh sách lớp
        ├── selectClass()         ← Chọn lớp → cập nhật header + bảng
        ├── renderScheduleTable() ← Bảng TKB (rowspan đúng cho SÁNG/CHIỀU)
        ├── renderStats()         ← Thống kê tiết/môn
        ├── renderLegend()        ← Bảng màu 23 môn
        ├── openEdit()            ← Mở modal sửa tiết
        ├── pickSubject()         ← Chọn môn + auto-suggest GV
        ├── saveCell()            ← Lưu thay đổi
        ├── clearCell()           ← Xóa ô tiết
        ├── renderTeacherTable()  ← Bảng lịch GV
        ├── renderMasterMatrix()  ← Ma trận toàn trường
        ├── doLogin()/doLogout()  ← Admin authentication
        ├── toggleEditMode()      ← Bật/tắt sửa nhanh
        └── showToast()           ← Thông báo nổi
```

---

## 📅 Lịch Sử Phiên Bản

| Ver | Ngày | Thay đổi |
|-----|------|---------|
| v1.0 | 2026-08 | Khung React + localStorage, data mẫu |
| v2.0 | 2026-08-22 | Rewrite Vanilla JS, nhúng 29 lớp, 3 tab, modal sửa, màu 23 môn |
| v2.4 | 2026-08-22 | Teacher View, Master Matrix, Admin UI |
| v2.5 | 2026-08-22 | **Fix:** INITIAL_SCHEDULE load (`.schedule` → flat map) |
| v2.6 | 2026-08-22 | **Fix:** Nút Sửa Nhanh luôn hiện, auto-open login |
| v2.7 | 2026-08-22 | **Fix:** Header 2 cột căn giữa, rowspan SÁNG/CHIỀU đúng, colspan break đúng |
| v2.8 | 2026-08-22 | **Thêm:** Phát hiện trùng tiết GV, Kiểm định quy tắc khi lưu (toast/modal), đồng bộ GV bộ môn tức thời. Đồng bộ GitHub lần đầu. |
| v2.9 | 2026-08-22 | **Fix:** In/Xuất PDF gọn 1 trang A4 ngang, bỏ ghi chú chữ ký |
| v3.0 | 2026-08-22 | **Fix nghiêm trọng:** Bảng "Xem Theo Giáo Viên" trống với **mọi** GV do so khớp sai `teacher_name` (dữ liệu lưu tên ngắn, code so tên đầy đủ) — sửa lại so khớp qua `teacher_id` (chính xác 1-1, 1100/1100 ô đã kiểm chứng). **Fix nút In:** chuyển từ `window.print()` gọi trực tiếp (bị chặn trong iframe Artifact, không phản ứng) sang mở cửa sổ mới rồi in — đảm bảo hoạt động ổn định. |
| v3.1 | 2026-08-22 | **Fix:** Nút "Tải File Nén (.zip)" xuất JSON không tải được gì (Claude Artifact chặn tải file kiểu `<a download>` truyền thống). Chuyển sang dùng API `downloads` chính thức của Claude (`window.claude.use('downloads')`) khi chạy trong Artifact, tự động rơi về cách tải truyền thống khi chạy trên Netlify/local — hoạt động đúng ở cả 2 môi trường. |
| v3.2 | 2026-08-22 | **Fix nút In (lần 2):** Cách "mở cửa sổ mới rồi in" (v3.0) bị chặn ngay ở tầng khung Artifact (`window.open()` trả về `null`) — không phải trình duyệt chặn pop-up. Quay lại gọi `window.print()` trực tiếp trên trang hiện tại (dựa vào `@media print` đã tối ưu sẵn từ v2.9), kèm hướng dẫn rõ ràng bấm **Ctrl+P / Cmd+P** làm phương án dự phòng luôn hoạt động (phím tắt trình duyệt, không trang web nào chặn được). |
| v3.3 | 2026-08-22 | **Nhập dữ liệu thầy chỉnh trên Artifact:** Merge 32 ô thầy đã sửa (lớp 1.1, 3.2, 3.3, 4.3) từ file JSON xuất ra vào `initial_schedule.json` + `INITIAL_SCHEDULE` trong HTML. **Fix bug phát hiện qua đó:** môn "NT(MT)" (Mĩ thuật) không có trong danh sách auto-suggest GV → lưu tiết với GV trống (lớp 3.3, Thứ 3, Tiết 6) — đã sửa dữ liệu về đúng GVCN + thêm NT(MT) vào danh sách auto-suggest + **thêm kiểm tra bắt buộc chọn GV trước khi lưu** (chặn tận gốc lỗi này tái diễn). |
| v3.4 | 2026-08-23 | **Fix lệch cột Tiết 8 (Tab 2 — Xem Theo GV):** `rowspan` cột CA (SÁNG/CHIỀU) thiếu `+1` cho hàng "Ra chơi" nằm trong buổi → Tiết 8 (và Tiết 4) bị mất cột CA, cả hàng lệch trái. Đã sửa `morningSlots.length`/`afternoonSlots.length` → `+1`, khớp cách tính đã đúng ở Tab 1. |
| v3.5 | 2026-08-23 | **Thêm tính năng:** Hiển thị tổng số tiết/tuần của GV bộ môn ở Tab 2 (hàm mới `getTeacherWeeklyTotal()` — đếm số (Thứ\|Tiết) duy nhất, không đếm nhân môn dạy đồng loạt toàn trường như Chào cờ). **Điều chỉnh hiển thị header Tab 1:** (1) đổi định dạng ngày "Ngày xuất: D/M/YYYY" → thể thức công văn "ngày D tháng M năm YYYY" (chữ "ngày" viết thường, vẫn lấy theo thời điểm hiển thị thực tế); (2) bỏ phần tên GVCN rút gọn lặp lại sau tên đầy đủ; (3) đổi "Tổng số: N tiết / tuần" → "Lớp X.X: N tiết/tuần". |
| v3.6 | 2026-08-23 | **Nhập dữ liệu thầy sửa trên Artifact (máy #3 cơ quan):** Merge 33 ô từ file `tkb_nak_2026-2027.json` (exported_at 2026-08-23T01:54:10Z) vào `initial_schedule.json` + `INITIAL_SCHEDULE`. Lớp thay đổi: 1.1 (5 ô), 2.3 (2 ô), 2.5 (13 ô), 2.6 (3 ô), 4.1 (10 ô). Đã kiểm định trước khi merge: 1100/1100 ô khớp, 0 ô trống GV, 0 trùng tiết. |
| v3.7 | 2026-08-23 | **Xác nhận + kiểm chứng lại fix rowspan Tiết 8 (v3.4)** áp dụng đúng cho mọi nhóm GV (Bộ môn & TPT, Liên kết, Chủ nhiệm) — dùng chung 1 hàm `renderTeacherTable()` nên không cần sửa riêng lẻ; đã test trực tiếp qua server local (không phải static snapshot) cho GV Cô Tâm (TA) và GV Liên kết TA(BN)_1, xác nhận hết lệch. **Giãn bố cục trang:** tăng `max-width` của `.hdr-wrap`, `.nav-tabs`, `main` từ 1400px → 1680px để tận dụng không gian màn hình rộng, đỡ trống trải 2 bên. Đã publish lại Artifact (link cũ giữ nguyên) — bản Artifact trước đó vẫn là V2.4 rất cũ, giờ đã đồng bộ đúng code mới nhất. |
| v3.8 | 2026-08-23 | **Ẩn 3 nút/badge chỉ dành cho quản trị khỏi giao diện người dùng thường** (Netlify/Artifact khi chưa đăng nhập): badge "100% Tuân Thủ Quy Tắc", nút "💾 Lưu Thời Khóa Biểu", nút "📦 Tải File Nén (.zip)" — thêm class `hidden` mặc định + đưa vào danh sách `adminIds` trong `updateAdminUI()`, cùng cơ chế đã dùng cho các nút quản trị khác (Cập Nhật Báo Giảng, In/Xuất PDF, Khôi Phục Gốc...). Đăng nhập quản trị vẫn thấy đủ như cũ. Đã test qua server local: trạng thái mặc định ẩn đúng cả 3, đăng nhập admin hiện lại đúng cả 3, đăng xuất ẩn lại đúng. |
| v3.9 | 2026-08-23 | **Fix dữ liệu — thêm GV thứ 57 bị thiếu:** Thầy Lê Thành Tạo (Phó Hiệu trưởng) dạy Tin học TH(2018) cho 4 lớp 1.1-1.4, nhưng hệ thống trước đó gán nhầm 4 tiết này cho GVCN từng lớp. Đã: (1) thêm GV mới `t_pht_tao` vào `TEACHERS_CONFIG` (nhóm Bộ môn & TPT, 9→10 GV), (2) sửa `CLASSES_CONFIG` 4 lớp 1.1-1.4 (`thTeacher`), (3) sửa đúng 4 ô dữ liệu TH trong `initial_schedule.json`/`INITIAL_SCHEDULE` (1.1 T6-6, 1.2 T3-7, 1.3 T3-3, 1.4 T6-4) từ GVCN → Thầy Tạo (PHT). Tổng GV giảng dạy: 56→**57** (đúng số đã ghi sẵn ở header). Đã kiểm định: 1100/1100 ô, 0 trùng tiết. Phát hiện qua đối chiếu với tài liệu `QUY_TAC_VA_DU_LIEU_TKB_2026_2027.md` (thầy cung cấp, chưa đưa vào dự án — đang chờ thầy hoàn thiện xếp TKB theo đúng quy tắc). |

---

## 🔄 Quy Trình Cập Nhật Dữ Liệu (sau khi Thầy sửa trên Artifact)

> **Lưu ý kiến trúc:** Thầy chỉ thao tác/chỉnh sửa trên **Claude Artifact**
> (claude.ai). **Netlify chỉ là bản hiển thị tĩnh**, tự build lại từ GitHub —
> không phải nơi để chỉnh sửa. 3 hệ thống này tách biệt hoàn toàn:
> `Artifact (localStorage) → GitHub (khi Claude push) → Netlify (tự build lại)`.

Artifact chạy hoàn toàn phía trình duyệt (client-side) — mọi thay đổi qua
**"✏️ Bật Sửa Nhanh Từng Tiết"** chỉ lưu vào `localStorage` của trình duyệt đó,
**KHÔNG** tự động lên GitHub (vì lý do bảo mật, không thể nhúng token GitHub
vào file HTML công khai mà ai xem trang cũng đọc được mã nguồn).

### ✅ Cách 1 — Nhờ Claude cập nhật (khuyên dùng)
```
1. Sửa TKB trên Artifact như bình thường (Sửa Nhanh Từng Tiết)
2. Bấm nút "📦 Tải File Nén (.zip)" → trình duyệt tải về 1 file .json
3. Gửi file .json đó cho Claude trong khung chat
4. Claude sẽ:
   a. Lấy phần "schedule" trong file gửi lên
   b. Cập nhật vào initial_schedule.json (thư mục dự án)
   c. Cập nhật vào biến INITIAL_SCHEDULE trong tkb-full-v2.html / index.html
   d. git commit + git push lên GitHub
   e. Publish lại Artifact với dữ liệu mới
```

### 🛠️ Cách 2 — Tự cập nhật thủ công
```
1. Xuất file .json như Cách 1 (bước 1-2)
2. Mở file .json vừa tải, copy phần bên trong "schedule": { ... }
3. Dán đè vào initial_schedule.json (thư mục dự án) — thay toàn bộ nội dung
4. Nếu có Git cài sẵn trên máy:
   cd "Dự án TKB mới"
   git add . && git commit -m "Cập nhật TKB" && git push
```

### ⚠️ Nếu có lỗi / dữ liệu bị rối
Gửi lại cho Claude **toàn bộ thư mục dự án** (hoặc file `tkb-full-v2.html`
+ `initial_schedule.json`) **kèm link GitHub** —
`https://github.com/hcmtranle/tkb20262027hoanchinh` — Claude sẽ đối chiếu,
sửa lại và đồng bộ lại từ đầu.

### Định dạng file .json xuất ra từ Artifact
```json
{
  "version": "2.0",
  "school": "Trường TH Nguyễn An Khương",
  "year": "2026-2027",
  "schedule": { "1.1": { "T2": { "1": {...}, ... }, ... }, ... },
  "exported_at": "2026-08-22T..."
}
```
Lưu ý: `initial_schedule.json` trong dự án **chỉ chứa phần `schedule`**
(không có wrapper `version/school/year`) — khi cập nhật thủ công nhớ chỉ
lấy đúng nội dung bên trong `"schedule": { ... }`.

---

## 🔗 Liên Kết

- **Artifact:** https://claude.ai/code/artifact/8552c45d-c843-4f04-aa02-ac0ed341b2b6
- **GitHub:** *(thầy sẽ cung cấp link sau để đồng bộ)*
- **Nguồn tham khảo:** Google AI Studio app (Gemini) — `ai.studio/apps/b1a14e68-b122-4f25-aed3-410bc1831fbf`

---

*Xây dựng bởi Claude Sonnet 4.6 cùng thầy Trần Lê — Ngày 22/08/2026*  
*Dữ liệu thực: Trường TH Nguyễn An Khương, 29 lớp, 57 GV, năm học 2026-2027*
