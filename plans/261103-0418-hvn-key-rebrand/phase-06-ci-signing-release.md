# Phase 6 — CI `Build`, ký số, Sparkle, gói mã nguồn GPL → Cổng ② CI

## Mục tiêu

Một workflow `Build` chứng minh build, đóng gói và (khi có tag) đăng Release lên repo phát hành public, kèm gói mã nguồn
cho GPL.

## Workflow `.github/workflows/build.yml` (theo `ab-app-release/assets/github/build.yml.template`)

- Trigger: `pull_request`, `push` lên `main`, tag `v*`, `workflow_dispatch`.
- Job `engine` (macos-15): `zsh Tools/autoswitch/check.sh`, tức engine rule lock. Có thể gộp vào `engine-rules.yml`
  hiện có. Tránh chạy hai lần.
- Job `app` (macos-15, arm64):
  1. `brew install xcodegen create-dmg` → `xcodegen generate`.
  2. Kiểm phiên bản: trên tag, `MARKETING_VERSION` phải bằng tag và CHANGELOG phải có mục `## [X.Y.Z]`. Lệch thì dừng.
  3. `xcodebuild` Release, rồi ký (xem mục ký số).
  4. **Verify bundle:** `CFBundleIdentifier = vn.hvn.hvnkey`, `CFBundleDisplayName = HVN Key`, có
     `Sparkle.framework`, `vi.lproj`, `AppIcon`, kiến trúc đúng (`lipo -archs`), `codesign --verify --deep --strict`.
     Thêm kiểm "không còn `vikey.app`": `! grep -r "vikey.app" "HVN Key.app/Contents/Info.plist"`.
  5. Đổi tên thành `HVN Key.app`, rồi tạo `HVNKey.dmg` (có lối tắt Applications) và `HVNKey.zip` (cho Sparkle), kèm
     `*.sha256`.
  6. Upload artifact (PR và main).
- Job `publish` (chỉ khi có tag):
  1. `scripts/release-notes.sh X.Y.Z` lấy release notes từ CHANGELOG.
  2. `scripts/make-appcast.sh` ký EdDSA bằng secret `SPARKLE_PRIVATE_KEY`.
  3. `git archive --format=tar.gz --prefix=HVNKey-X.Y.Z/ $GITHUB_SHA > HVNKey-X.Y.Z-source.tar.gz` là **gói mã nguồn
     GPL**. Kèm `LICENSE`.
  4. `gh release create vX.Y.Z -R hvngroup/universal_hvn_key-releases` với `GH_TOKEN: ${{ secrets.RELEASES_TOKEN }}`,
     đính kèm DMG, ZIP, SHA-256, `appcast.xml` và gói source.

## Ký số (theo câu hỏi mở 1)

| Phương án | Cấu hình | Hệ quả |
|---|---|---|
| **Developer ID + notarize (đề xuất)** | `ENABLE_HARDENED_RUNTIME: YES`, `CODE_SIGN_IDENTITY: "Developer ID Application"`, team `TVZNQ5W8NG`. Secrets: `MACOS_CERT_P12_BASE64`, `MACOS_CERT_PASSWORD`, khoá API notarize | Người dùng mở app bình thường. Quyền Accessibility được giữ qua các bản cập nhật vì chữ ký ổn định |
| Ad-hoc (mặc định của chuẩn) | `CODE_SIGN_IDENTITY: "-"`, tắt hardened runtime | Gatekeeper chặn lần đầu (hướng dẫn Open Anyway). Có rủi ro phải cấp lại quyền Accessibility sau mỗi lần cập nhật |

Workflow viết sao cho thiếu chứng chỉ thì tự lùi về ký ad-hoc (mẫu của `signing-distribution.md`). Nhờ vậy CI vẫn xanh
trong lúc chờ chứng chỉ.

## Sparkle

1. Chủ sản phẩm tạo cặp khoá EdDSA mới (`generate_keys` của Sparkle) trên máy của mình (ASK FIRST). Khoá bí mật lưu
   vào secret `SPARKLE_PRIVATE_KEY`, phân loại Tuyệt mật. Khoá công khai gửi agent.
2. Agent đặt `SUPublicEDKey` vào `App/Info.plist`. `SUFeedURL` đã đặt ở Phase 2.
3. `App/Model/Updater.swift` chỉ bật updater khi có `SUPublicEDKey` (mẫu `AppUpdater.swift`).

## Cổng ② CI

- [ ] Workflow `Build` xanh trên **commit hiện tại** của PR.
- [ ] Bước Verify qua (bundle ID, tên, Sparkle, `vi.lproj`, icon, kiến trúc, chữ ký, không còn `vikey.app`).
- [ ] Artifact tải được: `HVNKey.dmg`, `HVNKey.zip`, `*.sha256`.
- [ ] `Engine rules` xanh, golden snapshot không đổi.
- [ ] Lỗi đỏ đã được tìm nguyên nhân gốc và sửa. Không tắt test.

---

HVN GROUP · hvn.vn
