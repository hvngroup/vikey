# Phase 3 — Đổi tên lớp app macOS

## Mục tiêu

App macOS mang tên HVN Key ở mọi chỗ người dùng nhìn thấy và trong định danh build. `Engine/` giữ nguyên ký hiệu để
merge upstream dễ.

## Phạm vi đổi tên

| Nhóm | File / mục | Đổi |
|---|---|---|
| Dự án | `project.yml` | `name: HVNKey`, target `HVNKey`, `PRODUCT_NAME: HVNKey`, `PRODUCT_BUNDLE_IDENTIFIER: vn.hvn.vnkey`, `MARKETING_VERSION: "1.0.0"`, `CURRENT_PROJECT_VERSION: "1"`, đường dẫn entitlements và bridging header |
| Đổi tên file | `App/VikeyApp.swift` → `App/HVNKeyApp.swift`; `App/Vikey.entitlements` → `App/HVNKey.entitlements`; `App/Core/Vikey-Bridging-Header.h` → `App/Core/HVNKey-Bridging-Header.h`; `App/Core/VikeyCore.{h,mm}` → `App/Core/HVNKeyCore.{h,mm}` | Dùng `git mv` để giữ lịch sử |
| Ký hiệu Swift/ObjC | `struct VikeyApp`, lớp `VikeyCore` và các chỗ gọi trong `App/` | Đổi theo tên file |
| Info.plist | `CFBundleDisplayName`/`CFBundleName` = "HVN Key"; mô tả quyền (`NSAppleEventsUsageDescription`...) đổi "Vikey" thành "HVN Key"; `NSHumanReadableCopyright` = "© 2026 HVN GROUP. Dựa trên Vikey © Tony Dinh và OpenKey © Tuyen Mai (GPL-3.0)" |
| Chuỗi giao diện | Khoảng 45 chuỗi chứa "Vikey" trong `App/Views/*`, `App/Model/*` (Onboarding, MainView, MenuBarContent, Windows…) | "HVN Key". Riêng trang Acknowledgements giữ "Vikey" để ghi công |
| Dữ liệu cục bộ | Đường dẫn `Application Support/Vikey` (nhật ký gõ, crash log) | `Application Support/HVN Key` |
| Engine | `Engine/*` | **Không đổi** ký hiệu và comment bản quyền. Chỉ cập nhật comment `Driver.h` dòng 9 nếu nó nhắc `VikeyCore.mm` (đổi thành `HVNKeyCore.mm`) |
| Công cụ test | `Tools/autoswitch/check.sh` và `.github/workflows/engine-rules.yml` job `app` | Đổi `-project Vikey.xcodeproj -scheme Vikey` thành `HVNKey` |

## Việc làm

1. `git mv` các file ở bảng trên, sửa `project.yml`, sửa import và tên ký hiệu.
2. Thay chuỗi theo danh sách lấy bằng `git grep -n '"[^"]*Vikey' -- App`. Xem từng chỗ, không sed hàng loạt: các chỗ
   ghi công phải giữ "Vikey".
3. `.gitignore`: đổi `Vikey.xcodeproj` thành `HVNKey.xcodeproj`.
4. Tự rà diff theo `ab-macos-native/references/concurrency-pitfalls.md` trước khi đẩy, vì agent không có Xcode.

## Kiểm tra

- CI `Engine rules` (job `check` + `app`) xanh, và golden snapshot không đổi.
- `git grep -n -i vikey -- App project.yml` chỉ còn dòng ghi công.
- Trong artifact build có `Contents/Info.plist`, với `CFBundleIdentifier = vn.hvn.vnkey`.

## Rủi ro

- Bridging header đổi tên mà quên cập nhật `SWIFT_OBJC_BRIDGING_HEADER` thì CI báo lỗi biên dịch ngay. Sửa theo log.
- Đổi bundle ID làm macOS coi đây là app mới: người dùng phải cấp lại quyền Accessibility và Apple Events. Đây là hành vi
  đúng với bản cài mới.

---

HVN GROUP · hvn.vn
