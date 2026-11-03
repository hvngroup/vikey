# Phase 4 — Brand HVN

## Mục tiêu

HVN Key mang nhận diện HVN GROUP, gồm màu `#EA4445`, logo HVN GROUP bằng SVG chuẩn và footer "HVN GROUP · hvn.vn". App
vẫn giữ ghi công Vikey và OpenKey.

## Việc làm

1. **Màu:** `App/Model/Brand.swift` đổi màu nhấn indigo `#4F46E5` / `#8C87FF` sang Brand Red `#EA4445` (hover
   `#C93939`, nền nhạt `#FDECEC`). Ở chế độ tối, kiểm độ tương phản của chữ trắng trên nền đỏ (≥ 4.5:1 cho chữ thường).
   Màu trạng thái: xanh `#28A745`, cảnh báo `#FFC107`. Màu đỏ "gõ sai" trong demo đổi sang tông khác đỏ brand (ví dụ
   gạch chân vàng `#FFC107`) để không trùng nghĩa.
2. **Wordmark:** "HVN Key" chữ đậm, "HVN" màu chữ chính và "Key" màu `#EA4445`. Dùng ở header cửa sổ, About và
   onboarding.
3. **Icon app:** CMO (#Archer / #Minion) cung cấp artwork vuông 1024 px. Ví dụ: chữ "Vi" hoặc biểu tượng phím trên nền
   `#EA4445`. Không để AI vẽ logo. Sinh bộ AppIcon bằng `ab-app-release/assets/scripts/make-app-icon.py` vào
   `App/Resources/Assets.xcassets/AppIcon.appiconset/`, rồi sinh `design/HVNKey.icns` và xoá `design/Vikey.icns`.
4. **Icon thanh menu** (`App/Views/MenuBarIcon.swift`): giữ dạng template (đơn sắc) để macOS tự đổi màu. Chỉ đổi ký
   tự hoặc hình nếu CMO yêu cầu.
5. **About:** logo HVN GROUP (SVG chuẩn, bản dark trên nền sáng và bản light trên nền tối), dòng "HVN GROUP · hvn.vn",
   link `https://hvn.vn`, mục "Dựa trên Vikey (Tony Dinh) và OpenKey (Tuyen Mai), GPL-3.0" có link mã nguồn upstream.
6. **DMG:** `Tools/release/dmg_background.py` (hoặc script mới ở `scripts/`) vẽ nền DMG theo brand HVN.
7. **`design/BRAND.md`:** viết lại thành brand guideline của HVN Key. Tham chiếu brand guideline HVN GROUP, giữ quy tắc
   viết tên ("HVN Key", không viết "Hvn Key" hay "HVNkey").

## Kiểm tra

- Ảnh chụp cửa sổ chính, About, onboarding và menu ở cả hai chế độ sáng/tối, gửi chủ sản phẩm duyệt. Đây là một phần
  của cổng ③.
- CI kiểm bundle có `AppIcon` đủ kích thước.

## Phụ thuộc

- Artwork icon từ CMO. Nếu chưa có, dùng icon tạm (chữ "HK" trắng trên nền `#EA4445`, sinh bằng script) và ghi
  "chờ artwork" trong PR.

---

HVN GROUP · hvn.vn
