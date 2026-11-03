# Phase 7 — Chạy thử trên Mac thật và phát hành v1.0.0 → Cổng ③ Máy thật, ④ Phát hành

## Cổng ③ — checklist trên Mac thật (chủ sản phẩm chạy, agent không tự đánh dấu)

Dùng artifact của commit cuối PR. Chạy trên Mac chip Apple, macOS 13 trở lên (app chỉ build arm64).

- [ ] Mở DMG không bị Gatekeeper chặn (đã ký Developer ID và notarize).

- [ ] Mở DMG, kéo vào Applications. App tên "HVN Key", icon nền đỏ HVN.
- [ ] Onboarding hướng dẫn cấp quyền Accessibility. Sau khi cấp, gõ được tiếng Việt.
- [ ] Telex: `treen` → `trên`, `truowfng` → `trường`. Gõ `more` vẫn giữ tiếng Anh.
- [ ] Auto-switch đúng trong TextEdit, Chrome và Terminal. Không can thiệp ô mật khẩu.
- [ ] Nhớ ngôn ngữ theo app và theo trang web (Chrome, Safari): có lời nhắc quyền Apple Events ghi "HVN Key".
- [ ] Phím tắt chuyển ngôn ngữ và tiếng bíp tuỳ chọn hoạt động.
- [ ] Menu bar, cửa sổ chính, About: brand HVN, dòng "HVN GROUP · hvn.vn", ghi công Vikey/OpenKey + GPL-3.0.
- [ ] Giao diện mặc định là tiếng Việt. Chuyển sang English rồi khởi động lại thì toàn bộ là tiếng Anh.
- [ ] Không có kết nối tới `vikey.app`. Kiểm bằng `nettop -p "HVN Key"` hoặc Little Snitch trong 10 phút sử dụng, kể cả
      lúc bấm "Kiểm tra cập nhật".
- [ ] Gỡ Vikey (nếu đang cài) không ảnh hưởng HVN Key, vì hai app chạy độc lập.

## Cổng ④ — phát hành v1.0.0

1. PR riêng "chore: release 1.0.0": `[Unreleased]` → `[1.0.0] - YYYY-MM-DD`, `MARKETING_VERSION: "1.0.0"`,
   `CURRENT_PROJECT_VERSION: "1"`. Chạy thử `scripts/release-notes.sh 1.0.0`.
2. Merge vào `main`.
3. Chủ sản phẩm tạo Release `v1.0.0` trên GitHub UI của repo mã nguồn, target `main`, để trống mô tả. Đây là bước
   ASK FIRST, agent không tạo tag.
4. Workflow của tag xanh. Release `v1.0.0` ở `app-hvn-key-releases` có DMG, ZIP, SHA-256, `appcast.xml`,
   `HVNKey-1.0.0-source.tar.gz`, và release notes lấy từ CHANGELOG.
5. Agent tải file, chạy `sha256sum -c`, rồi đọc `Info.plist` trong ZIP để kiểm đúng `1.0.0 (1)` và
   `vn.hvn.vnkey`. Agent cũng giải nén gói source và đối chiếu commit.
6. Chủ sản phẩm mở cửa sổ ẩn danh: tải được `releases/latest/download/appcast.xml`, còn repo mã nguồn trả 404.

## Thử cập nhật thật (sau 1.0.0, trước khi thông báo rộng)

- Phát hành 1.0.1 (thay đổi nhỏ). Trên máy đang chạy 1.0.0, bấm "Kiểm tra cập nhật" → cài → khởi động lại.
- [ ] Lên 1.0.1 thành công.
- [ ] **Quyền Accessibility vẫn còn sau cập nhật.** Nếu mất, kiểm lại chữ ký của hai bản (`codesign -dr -`): designated
      requirement phải giống nhau.

## Sau phát hành

- Thông báo nội bộ (COO/CCO) kèm link repo phát hành và `docs/user-guide.md`.
- Lập kế hoạch đợt 2 (iOS, Android, Windows, Linux, website) với cùng định danh và brand.

---

HVN GROUP · hvn.vn
