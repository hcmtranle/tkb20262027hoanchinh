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

---

## 🔄 Quy Trình Cập Nhật Dữ Liệu (sau khi Thầy sửa trên Artifact)

Artifact chạy hoàn toàn phía trình duyệt (client-side) — mọi thay đổi qua
**"✏️ Bật Sửa Nhanh Từng Tiết"** chỉ lưu vào `localStorage` của trình duyệt đó,
**KHÔNG** tự động lên GitHub (vì lý do bảo mật, không thể nhúng token GitHub
vào file HTML công khai).

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
