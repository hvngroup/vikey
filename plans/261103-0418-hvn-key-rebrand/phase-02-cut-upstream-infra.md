# Phase 2 — Cắt hạ tầng upstream (an toàn trước tiên)

## Mục tiêu

Không bản build nào của HVN Key lấy cập nhật, gửi crash report, gửi feedback hay phát hành qua hạ tầng của Vikey. Phase
này phải xong **trước** mọi bản build đưa cho người dùng.

## Hiện trạng (đã kiểm)

| Chỗ | Đang trỏ về | Hậu quả nếu giữ |
|---|---|---|
| `App/Info.plist` `SUFeedURL`, `SUPublicEDKey` | `vikey.app/appcast.xml`, khoá của Vikey | Máy HVN nhận bản Vikey đè lên HVN Key |
| `App/Model/CrashReporter.swift` | `https://vikey.app/api/crash` | Crash report của HVN gửi cho bên thứ ba |
| `App/Model/Updater.swift`, `App/Views/MainView.swift` | Mô tả "kiểm tra bản mới trên vikey.app" | Thông tin sai cho người dùng |
| `project.yml` `DEVELOPMENT_TEAM` | `3H6967F79Y` | Ký nhầm team, CI lỗi |
| `Tools/release/release.sh` | Keychain và khoá notarize ở `~/.secrets` của tác giả gốc, deploy `vikey.app` | Script không chạy được, lộ cấu trúc khoá của người khác |
| `.github/workflows/{linux,windows,android}.yml` | Upload gói Vikey lên Release khi có tag `v*` | Tag `v1.0.0` của HVN Key kéo theo gói Vikey |
| `site/wrangler.toml` | Cloudflare Pages `vikey`, D1 và R2 của Vikey | Deploy nhầm sang hạ tầng của người khác |

## Việc làm

1. `App/Info.plist`: đặt `SUFeedURL` theo repo phát hành. Bỏ trống `SUPublicEDKey` cho tới Phase 6. Theo mẫu
   `ab-macos-native/assets/AppUpdater.swift`, Sparkle chỉ bật khi có khoá, nên không có khoá thì updater tắt.
2. `CrashReporter.swift`: tắt gửi mạng mặc định khi chưa có endpoint của HVN. Vẫn giữ lưu crash log cục bộ để người
   dùng đính kèm vào Issue.
3. Link feedback trong app trỏ về `https://github.com/hvngroup/universal_hvn_key-releases/issues/new/choose`.
4. `project.yml`: `DEVELOPMENT_TEAM: TVZNQ5W8NG`, `bundleIdPrefix: vn.hvn`. Cách ký số chốt ở Phase 6.
5. Bỏ nhánh macOS của `Tools/release/release.sh`, thay bằng CI (Phase 6). Giữ các lane `--windows/--linux/--android`
   cho đợt 2, nhưng chặn bằng một dòng báo "chưa chuyển sang HVN Key".
6. Workflow `linux.yml`, `windows.yml`, `android.yml`: bỏ trigger `tags: ['v*']` và job `release`. Giữ build và test
   trên PR để engine không bị hỏng ở các nền tảng khác.
7. `site/`: không deploy trong đợt 1. Ghi chú ở đầu `site/wrangler.toml` và `Tools/release/deploy_site.sh` rằng
   website thuộc đợt 2.

## Kiểm tra

- `git grep -n "vikey.app" -- App project.yml .github` chỉ còn link ghi công trong Acknowledgements (nếu giữ).
- `git grep -n "3H6967F79Y\|\.secrets" -- project.yml Tools .github` trả về rỗng.
- CI `Engine rules` vẫn xanh vì phase này không chạm engine.

## Rủi ro

- Tắt crash report thì mất tín hiệu lỗi. Bù bằng mẫu Issue "Báo lỗi" có hướng dẫn đính kèm crash log (Phase 5).

---

HVN GROUP · hvn.vn
