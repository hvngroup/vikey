# Phase 5 — Chuẩn hoá repo, tiếng Việt mặc định, tài liệu

## Mục tiêu

Repo và app đạt các mục "Mặc định của nhà" trong `ab-macos-native` và `ab-app-release`.

## Đối chiếu với chuẩn

| Mục chuẩn | Hiện trạng Vikey | Việc làm |
|---|---|---|
| XcodeGen, `.xcodeproj` không commit | Đã đúng | Giữ |
| macOS 13+ | Đã đúng | Giữ |
| `ARCHS: arm64` | Upstream build universal | Đặt `ARCHS: arm64` trong `project.yml` (đã chốt: chỉ chip Apple) |
| Bundle `vn.hvn.<app>` | `com.devuap.vikey` | Phase 3 |
| Chuỗi giao diện tiếng Anh, bản dịch Việt trong `App/Localizable.xcstrings`, **tiếng Việt mặc định** | Chuỗi tiếng Việt viết thẳng trong code, không có String Catalog | Chuyển sang String Catalog (mục 1 bên dưới) |
| `Shared/` cho logic thuần + XCTest | Logic engine ở `Engine/`, test bằng `Tools/autoswitch` | Giữ `Engine/` (tương đương `Shared/`), ghi lý do vào `docs/development.md`. Test hiện có mạnh hơn yêu cầu |
| `CHANGELOG.md` Keep a Changelog tiếng Việt | Chưa có (changelog nằm trong `site/pages/changelog.html`) | Tạo mới, mục `[1.0.0]` |
| `docs/user-guide.md`, `docs/development.md`, `docs/release.md` | Chưa có | Tạo mới |
| `SECURITY.md`, `.github/ISSUE_TEMPLATE/`, `dependabot.yml`, PR template | Chỉ có PR template | Lấy từ `ab-app-release/assets/github/` |
| README theo `readme-guide.md` | README của Vikey (tiếng Anh, tác giả Tony) | Viết lại cho HVN Key, giữ mục ghi công + GPL |
| Footer "HVN GROUP · hvn.vn" ở tài liệu | Không có | Thêm vào mọi file trong `docs/`, README, CHANGELOG |

## Việc làm

1. **String Catalog:** tạo `App/Localizable.xcstrings`, khai báo `knownRegions: [vi, en]` và
   `developmentRegion: vi`, hoặc theo mẫu `AppLanguage.swift` (tiếng Việt mặc định). Chuyển chuỗi trong `App/Views`
   sang khoá tiếng Anh kèm bản dịch tiếng Việt. Làm theo từng màn hình (Onboarding → MainView → MenuBarContent → các
   cửa sổ còn lại) để dễ review. CI kiểm có `vi.lproj` trong bundle.
   *Trade-off:* `App/` sẽ lệch xa upstream. Từ đây, bản sửa giao diện của upstream phải chép tay. Bản sửa engine
   không bị ảnh hưởng.
2. `CHANGELOG.md` gồm mục `[Unreleased]` và `[1.0.0]`: "Phát hành đầu tiên của HVN Key, dựa trên Vikey 1.0.31". Không
   ghi tên khách hàng hay link private (CHANGELOG là thông tin Công khai).
3. `docs/user-guide.md`: cài đặt, cấp quyền Accessibility và Apple Events, cách gõ, auto-switch, phím tắt, cập nhật, gỡ
   cài đặt, báo lỗi. Ghi rõ app chỉ chạy trên Mac chip Apple. Bản đã ký Developer ID và notarize nên không cần
   hướng dẫn **Open Anyway**, trừ bản build thử từ PR (vẫn ký ad-hoc khi CI chưa có chứng chỉ).
4. `docs/development.md`: build bằng XcodeGen, chạy `Tools/autoswitch/check.sh`, cấu trúc repo, quy trình merge
   upstream (Phase 1), lý do giữ `Engine/`.
5. `docs/release.md`: quy trình phát hành theo `ab-app-release`.
6. `SECURITY.md`, issue templates (báo lỗi có mục phím đã gõ / chữ hiện ra / chữ mong muốn và crash log), Dependabot
   cho GitHub Actions và Swift Package (Sparkle). Placeholder `{{RELEASES_REPO}}` = `hvngroup/app-hvn-key-releases`.
7. `README.md` viết lại. Chuyển `CONTRIBUTING.md`, `TRIAGE.md` và `.coderabbit.yaml` sang ngữ cảnh HVN hoặc ghi rõ
   chúng thuộc upstream.

## Kiểm tra

- Chuyển ngôn ngữ app sang English rồi khởi động lại: mọi màn hình hiện tiếng Anh. Để mặc định thì hiện tiếng Việt
  (cổng ③).
- `scripts/release-notes.sh 1.0.0` in đúng mục CHANGELOG.

---

HVN GROUP · hvn.vn
