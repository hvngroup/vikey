# Brainstorm — HVN Key (đổi tên từ Vikey, chuẩn hoá theo HVN App Builder Suite)

- Ngày: 2026-11-03
- Repo mã nguồn: `hvngroup/universal_hvn_key`, Private, đang trống (chưa có commit)
- Repo nguồn gốc: `hvngroup/vikey`, bản sao của `tonyminibot/Vikey` (GPL-3.0, dựa trên OpenKey)
- Repo phát hành: `hvngroup/universal_hvn_key-releases`, Public (chủ sản phẩm tạo, xem câu hỏi mở 3)
- Trạng thái: **Đã chốt** 4 quyết định chặn (mục "Hướng đã chọn"); còn câu hỏi mở ở cuối

## Kết quả cần đạt (Outcome)

Nhân viên và khách hàng HVN cài **HVN Key** trên Mac (chip Apple) từ một Release ở repo phát hành public. App mang
nhận diện HVN GROUP, gõ tiếng Việt và tự chuyển Việt/Anh giống Vikey. Bản cập nhật lấy từ feed của HVN, không gửi bất
kỳ dữ liệu nào về hạ tầng của Vikey (`vikey.app`). Mã nguồn nằm trong repo private của HVN, vẫn kéo được bản sửa engine
từ upstream, và mỗi Release có kèm gói mã nguồn tương ứng để đáp ứng GPL-3.0.

## Ràng buộc (Constraints)

- **Giấy phép GPL-3.0:** giữ `LICENSE`, ghi công Tony Dinh (Vikey) và Tuyen Mai (OpenKey) trong About,
  `ACKNOWLEDGEMENTS.md` và README. Không xoá các dòng bản quyền ở đầu file `Engine/`. Mỗi Release ở repo public phải
  có `HVNKey-X.Y.Z-source.tar.gz` đúng với commit đã build.
- **Chuẩn App Builder Suite:** XcodeGen, macOS 13+, bundle `vn.hvn.<app>`, CI workflow `Build`, `CHANGELOG.md`
  theo Keep a Changelog (tiếng Việt), `docs/`, hai repo (mã nguồn private, phát hành public), brand HVN, giao diện
  tiếng Việt mặc định.
- **ATTT HVN 01-01/11/2024:** khoá Sparkle, chứng chỉ ký và `RELEASES_TOKEN` chỉ nằm trong GitHub Secrets, được phân loại
  Tuyệt mật. Không commit đường dẫn khoá cá nhân (script `Tools/release/release.sh` hiện đang trỏ tới
  `~/.secrets/...` của tác giả gốc).
- **Quyền hệ thống:** app cần quyền Accessibility (event tap) và Apple Events (đọc URL tab trình duyệt).
- **Môi trường agent:** container Linux không có Xcode, nên build do CI trên runner macOS chứng minh và chủ sản phẩm
  chạy thử trên Mac thật.
- **Đồng bộ upstream:** `Engine/` dùng chung cho cả 5 nền tảng và đổi nhiều ở upstream. Đợt này giữ nguyên tên ký hiệu
  trong `Engine/` (ví dụ `VikeyDriver`) để merge từ upstream ít xung đột. Chỉ đổi tên ở phần người dùng nhìn thấy và
  ở lớp app macOS.

## Ngoài phạm vi (Non-goals) của đợt 1

- iOS, Android, Windows, Linux và website `site/`. Các phần này để đợt 2. Mã nguồn vẫn giữ trong repo, nhưng CI của
  chúng không được phát hành gì (chặn tag-publish).
- Đổi engine gõ, luật Telex hoặc auto-switch.
- Hạ tầng thu crash report và feedback của HVN (endpoint, D1, R2). Đợt 1 tắt crash report gửi mạng, còn feedback trỏ về
  Issues của repo phát hành.
- Chuyển cài đặt từ Vikey sang HVN Key (xem câu hỏi mở 5).

## Tiêu chí nghiệm thu (Acceptance criteria)

1. Repo `universal_hvn_key` có nhánh `main` chứa toàn bộ lịch sử Vikey cùng các commit đổi tên. Remote `upstream`
   trỏ `hvngroup/vikey`, và quy trình merge upstream được ghi trong `docs/development.md`.
2. Tìm `git grep -i vikey -- App project.yml .github docs README.md` chỉ còn các dòng ghi công hoặc đường dẫn
   upstream có chủ đích.
3. App build ra `HVN Key.app`: bundle `vn.hvn.hvnkey`, tên hiển thị "HVN Key", phiên bản `1.0.0`, icon nền `#EA4445`.
   Cửa sổ About ghi HVN GROUP · hvn.vn và ghi công Vikey/OpenKey kèm GPL-3.0.
4. `Info.plist` không còn `vikey.app`. `SUFeedURL` trỏ appcast ở repo phát hành, `SUPublicEDKey` là khoá của HVN, và
   app không gọi mạng tới `vikey.app` (kiểm trên máy thật).
5. Workflow `Build` xanh trên commit hiện tại, gồm engine check (`Tools/autoswitch/check.sh`), build app, kiểm tra
   bundle và artifact `.dmg` + `.zip` + `.sha256`.
6. Tag `v1.0.0` đăng Release ở repo phát hành, gồm DMG, ZIP, SHA-256, `appcast.xml`, gói mã nguồn, cùng release notes lấy
   từ `CHANGELOG.md`.
7. Chủ sản phẩm chạy checklist trên Mac thật và xác nhận. Checklist gồm cấp quyền, gõ Telex, auto-switch, nhớ ngôn ngữ
   theo app hoặc web, phím tắt, cập nhật qua Sparkle từ 1.0.0 lên 1.0.1 và giữ quyền Accessibility sau cập nhật.

## Hướng đã chọn

| Thành phần | Lựa chọn |
|---|---|
| Repo | Đẩy lịch sử `hvngroup/vikey` sang `universal_hvn_key` (Private). `vikey` thành bản mirror upstream |
| Phạm vi | Đợt 1 làm macOS và đạt đủ 4 cổng. Đợt 2 làm iOS, Android, Windows, Linux và website |
| Brand | Brand HVN đầy đủ: `#EA4445`, logo HVN GROUP SVG chuẩn, footer HVN GROUP · hvn.vn |
| GPL | Mã nguồn để Private. Mỗi Release public đính kèm gói source và LICENSE |
| Vỏ app | Giữ SwiftUI + AppKit, event tap và engine C++ hiện có (đã là Swift gốc, đúng chuẩn) |
| Build / CI | `.github/workflows/build.yml` theo mẫu `ab-app-release`, runner `macos-15` |

## Trade-offs

| Hướng | Phụ thuộc lớn nhất | Hỏng đầu tiên khi |
|---|---|---|
| Đổi tên lớp app, giữ ký hiệu Engine (chọn) | Upstream giữ cấu trúc `Engine/` | Upstream đổi tên file trong `Engine/`, khi đó vẫn merge được nhưng phải sửa tay |
| Đổi tên toàn bộ, kể cả Engine | Không còn theo upstream | Mỗi lần merge upstream xung đột hàng trăm chỗ |
| Chuyển chuỗi giao diện sang String Catalog (chọn, Phase 5) | `App/` lệch xa upstream | Upstream sửa giao diện, khi đó phải chép tay bản sửa |

Better approaches: none. Fork kèm mirror upstream là cách rẻ nhất để vừa mang brand HVN vừa nhận bản sửa engine
(dựa trên nhịp commit của upstream: 132 commit trên `main`, trong đó 10 commit sửa `Engine/` từ 01/10/2026).

## Rủi ro cần kiểm chứng

1. **Quyền Accessibility sau cập nhật:** nếu ký ad-hoc, mỗi bản build có chữ ký khác nhau nên macOS có thể bỏ quyền đã
   cấp, buộc người dùng cấp lại sau mỗi lần cập nhật. Chỉ kiểm được trên Mac thật qua hai bản liên tiếp (câu hỏi mở 1).
2. Apple Events (đọc URL tab Chrome/Safari) hoạt động đúng khi bundle ID đổi (lời nhắc quyền mới).
3. Sparkle chuyển từ feed của Vikey sang feed của HVN: bản 1.0.0 là bản cài mới (bundle mới), không cập nhật đè Vikey.

## Câu hỏi còn mở

1. **Ký số macOS:** đề xuất dùng Developer ID + notarize (team HVN `TVZNQ5W8NG`) thay cho ad-hoc mặc định của chuẩn, vì
   app phụ thuộc quyền Accessibility (rủi ro 1). Cần Account Holder tạo chứng chỉ "Developer ID Application" và khoá API
   notarize.
2. **Kiến trúc:** chuẩn quy định `arm64`, trong khi upstream build universal (arm64 + x86_64). HVN còn máy Mac Intel
   cần dùng không?
3. **Tên repo phát hành:** dùng `universal_hvn_key-releases` cho khớp repo hiện có, hay đổi cả hai sang `app-hvn-key`
   / `app-hvn-key-releases` theo quy ước đặt tên của chuẩn?
4. **Bundle ID:** đề xuất `vn.hvn.hvnkey` (macOS). Nếu đồng ý thì đợt 2 dùng `vn.hvn.hvnkey.ios`,
   `vn.hvn.hvnkey.ios.keyboard`, Android `vn.hvn.hvnkey`, app group `group.vn.hvn.hvnkey`.
5. Có cần nhập cài đặt từ Vikey (`com.devuap.vikey`) cho người đang dùng Vikey không? Mặc định là không.

---

HVN GROUP · hvn.vn
