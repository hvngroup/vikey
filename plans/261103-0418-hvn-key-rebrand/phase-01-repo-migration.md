# Phase 1 — Chuyển repo và thiết lập hai repo

## Mục tiêu

`hvngroup/universal_hvn_key` (Private) chứa toàn bộ lịch sử Vikey trên nhánh `main`. Repo `hvngroup/vikey` chỉ làm
mirror của upstream `tonyminibot/Vikey`. Repo phát hành public sẵn sàng nhận Release.

## Chủ sản phẩm làm (ASK FIRST: agent không tạo repo và không đổi nhánh mặc định)

1. Trả lời câu hỏi mở 3 (tên repo). Phần dưới giả định giữ tên `universal_hvn_key`.
2. Tạo `hvngroup/universal_hvn_key-releases` ở chế độ **Public**, tích **Add a README**. Tắt Wiki và Projects, giữ
   Issues, bật **Private vulnerability reporting**.
3. Thêm repo releases vào token dùng chung "HVN release publisher" (Contents: Read and write, chỉ repo `*-releases`).
   Lưu token làm secret `RELEASES_TOKEN` của `universal_hvn_key`. Không dán token vào chat.
4. Cho phép agent tạo nhánh `main` ở `universal_hvn_key`. Sau đó đặt `main` làm default branch.

## Agent làm

1. Lấy lịch sử: `git fetch https://github.com/hvngroup/vikey main`, rồi đẩy commit đó làm `main` của
   `universal_hvn_key` (sau khi được phép ở bước 4). **Không** đẩy tag `v1.0.x` của Vikey, vì chúng sẽ đụng tag của
   HVN Key.
2. Đặt remote `upstream` = `hvngroup/vikey` trong tài liệu phát triển. Ghi quy trình merge upstream vào
   `docs/development.md`: fetch, rồi merge vào nhánh riêng `chore/upstream-YYYYMMDD`, chạy `Tools/autoswitch/check.sh`,
   mở PR.
3. Với `hvngroup/vikey`: ghi vào README của mirror (hoặc mô tả repo) rằng đây là bản sao upstream, không phát triển
   ở đây. Đồng bộ bằng nút **Sync fork** của GitHub hoặc `git pull upstream main`.
4. Mở PR phát hành repo releases, hoặc gửi nội dung để chủ sản phẩm commit: `README.md` (theo
   `ab-app-release/references/readme-guide.md` §Repo releases), `LICENSE` (GPL-3.0), `SECURITY.md`,
   `.github/ISSUE_TEMPLATE/`.

## Kiểm tra

- `git log --oneline main | wc -l` ở `universal_hvn_key` bằng số commit của `vikey/main` (cộng commit plan).
- Repo nguồn trả 404 khi mở ở cửa sổ ẩn danh. Repo releases mở được.
- Secret `RELEASES_TOKEN` có trong Settings › Secrets (chỉ thấy tên).

## Rủi ro

- Repo mã nguồn mà lỡ đổi thành Public thì phải dừng lại và báo chủ sản phẩm (theo chuẩn).
- Upstream tag trùng `v1.0.0`: không đẩy tag upstream. Repo mới chỉ có tag của HVN Key.

---

HVN GROUP · hvn.vn
