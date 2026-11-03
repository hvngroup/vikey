# Kế hoạch: Vikey → HVN Key, chuẩn hoá theo HVN App Builder Suite

- Trạng thái: **Chờ duyệt** (cổng ① Phạm vi đã chốt 4 quyết định; còn 5 câu hỏi mở)
- Hợp đồng phạm vi: [`plans/reports/brainstorm-261103-hvn-key-rebrand.md`](../reports/brainstorm-261103-hvn-key-rebrand.md)
- Repo đích: `hvngroup/universal_hvn_key` (Private) · Phát hành: `hvngroup/universal_hvn_key-releases` (Public)
- Đợt 1: **macOS**. Đợt 2 (iOS, Android, Windows, Linux, website) có kế hoạch riêng sau khi đợt 1 phát hành.

## Định danh đề xuất

| Mục | Giá trị cũ | Giá trị mới |
|---|---|---|
| Tên hiển thị | Vikey | **HVN Key** |
| `PRODUCT_NAME` / target / scheme | `Vikey` | `HVNKey` (đổi `.app` thành `HVN Key.app` khi đóng gói) |
| Bundle ID macOS | `com.devuap.vikey` | `vn.hvn.hvnkey` |
| Apple team | `3H6967F79Y` (Typing Technology) | `TVZNQ5W8NG` (HVN) |
| Phiên bản | 1.0.31 (33) | 1.0.0 (1) |
| Feed cập nhật | `https://vikey.app/appcast.xml` | `https://github.com/hvngroup/universal_hvn_key-releases/releases/latest/download/appcast.xml` |
| Khoá Sparkle | khoá của Vikey | khoá EdDSA mới của HVN (GitHub Secret) |
| Thư mục dữ liệu | `~/Library/Application Support/Vikey` | `~/Library/Application Support/HVN Key` |
| File phát hành | `Vikey-X.Y.Z.dmg` | `HVNKey.dmg`, `HVNKey.zip`, `*.sha256`, `appcast.xml`, `HVNKey-X.Y.Z-source.tar.gz` |

## Các phase

| # | Phase | Cổng | Ai làm | Phụ thuộc |
|:-:|---|:-:|---|---|
| 1 | [Chuyển repo và thiết lập hai repo](phase-01-repo-migration.md) | — | Chủ sản phẩm + agent | Câu hỏi mở 3 |
| 2 | [Cắt hạ tầng upstream (an toàn)](phase-02-cut-upstream-infra.md) | — | Agent | 1 |
| 3 | [Đổi tên lớp app macOS](phase-03-rename-macos.md) | — | Agent | 2 |
| 4 | [Brand HVN](phase-04-hvn-brand.md) | — | Agent + artwork từ CMO | 3 |
| 5 | [Chuẩn hoá repo, tiếng Việt mặc định, docs](phase-05-standard-compliance.md) | — | Agent | 3 |
| 6 | [CI `Build`, ký số, Sparkle, gói GPL](phase-06-ci-signing-release.md) | ② CI | Agent + chủ sản phẩm (secrets) | 2–5, câu hỏi mở 1–2 |
| 7 | [Chạy thử Mac thật và phát hành v1.0.0](phase-07-verify-release.md) | ③ ④ | Chủ sản phẩm + agent | 6 |

Phase 4 và 5 chạy song song được vì chạm các file khác nhau (4: `App/Model/Brand.swift`, icon, `design/`; 5: chuỗi
giao diện, `docs/`, `CHANGELOG.md`). Phase 3 phải xong trước vì có đổi tên file.

## Quy tắc PR

- Mỗi phase là một PR vào `main` của `universal_hvn_key` (Phase 4–5 có thể chung một PR nếu nhỏ). Phát hành v1.0.0 là
  một PR riêng "chore: release 1.0.0".
- Merge upstream không đi chung PR với đổi tên.
- Không tắt test trong `Tools/autoswitch/check.sh` để CI xanh. Đổi tên không được làm thay đổi hành vi engine, và golden
  snapshot phải giữ nguyên.

## Tiêu chí hoàn thành

Xem 7 tiêu chí nghiệm thu trong hợp đồng phạm vi. Tóm tắt: bản build HVN Key 1.0.0 có CI xanh, chủ sản phẩm xác nhận
trên Mac thật, Release public có đủ file (cả gói mã nguồn GPL), và app không còn chạm vào hạ tầng `vikey.app`.

## Rollback

- Phase 1–5 chỉ là commit trên nhánh hoặc PR, nên revert PR là đủ. Repo `vikey` không bị sửa (chỉ làm mirror).
- Phase 6–7: Release lỗi thì xoá Release và tag ở repo phát hành rồi tạo lại (theo `pipeline.md` Phase 6). Khoá Sparkle
  bị lộ thì phát hành bản mới với khoá mới và hướng dẫn cài tay (Sparkle không đổi khoá được cho máy đã cài).

---

HVN GROUP · hvn.vn
