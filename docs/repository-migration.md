# Tách YangDocs thành repository riêng

Ngày kiểm tra: **04/11/2026**.

## Nguồn và lịch sử

- Repository nguồn: [nguyenhonggiang2407/DTDD](https://github.com/nguyenhonggiang2407/DTDD).
- Nhánh nguồn: [`yangdocs-preview`](https://github.com/nguyenhonggiang2407/DTDD/tree/yangdocs-preview).
- Snapshot nguồn: [`29c218b69767592dc405594774f931c23c420c1e`](https://github.com/nguyenhonggiang2407/DTDD/commit/29c218b69767592dc405594774f931c23c420c1e).
- Repository đích: [nguyenhonggiang2407/YangDocs](https://github.com/nguyenhonggiang2407/YangDocs), nhánh `main`.

Bản tách được tạo bằng clone `--single-branch --branch yangdocs-preview` **không
dùng `--depth`**, giữ cả chín commit reachable từ snapshot nguồn với nguyên SHA,
tác giả và thời gian. Lịch sử bao gồm các commit tiền thân YangCareer vì chúng là
tổ tiên của nhánh; không lọc, squash hoặc viết lại để tạo một lịch sử mới.
Commit migration bổ sung nằm sau snapshot nguồn.

Chỉ đổi tên nhánh trong bản copy thành `main`. Remote nguồn được đặt tên
`upstream` và vô hiệu hóa push trong bản copy để tránh ghi nhầm vào DTDD.
Không sửa hoặc xóa branch nguồn trong thao tác này. LICENSE, NOTICE.md và
`upstream/workflow.py` giữ nguyên byte so với snapshot nguồn; các liên kết tới
tác giả và ví dụ upstream trong NOTICE vẫn giữ để truy vết.

## Các thay đổi phục vụ repository mới

- README dùng URL clone của YangDocs, thêm link CI và hồ sơ migration.
- Workflow `.github/workflows/ci.yml` chỉ theo dõi push vào `main`; bước commit
  screenshot cũng chỉ áp dụng `main` của repo mới.
- Browser test đọc file Markdown export bằng UTF-8 rõ ràng. Bản test cũ phụ thuộc
  encoding mặc định của Windows (`cp1252`) và thất bại dù file export UTF-8 đúng.
  Sửa này chỉ ở test, không đổi logic export hoặc runtime ứng dụng.
- Tạo lại ba screenshot từ browser tests chạy thực tế và kiểm tra trực quan;
  giữ đường dẫn tương đối `docs/screenshots/` để README chạy trên repo mới.

Không thay schema, API, giới hạn upload, dữ liệu mẫu hoặc attribution.

## Kiểm tra thực sự đã chạy

Môi trường local Windows, Python **3.12.14**, Chromium **156.0.8078.4** từ
Playwright **1.64.1**. Tái sử dụng virtualenv đã có, không tuyên bố đã thực hiện
một lượt cài sạch mới.

| Lệnh | Kết quả |
| --- | --- |
| `python -m pip check` | Không có dependency bị hỏng. |
| `ruff check app tests` | Đạt. |
| `ruff format --check app tests` | Tám file đúng định dạng. |
| `python -m pytest tests/test_documents.py -q` | 28 kiểm thử đạt. |
| `python -m pytest tests/test_ui.py -q` | Ba kiểm thử đạt sau sửa encoding test. |
| `python -m compileall -q app` | Biên dịch cú pháp Python đạt. |
| `node --check app/static/app.js` | Cú pháp JavaScript đạt. |

Browser suite khởi động Uvicorn và SQLite thử riêng, kiểm tra tìm kiếm tiếng Việt,
trích dẫn/trang nguồn/export, upload/ghim/xóa, render HTML như văn bản, điều khiển
bàn phím và overflow mobile. Ảnh desktop dùng viewport 1440 × 1050, mobile
390 × 844; ảnh full-page có thể cao hơn viewport.

Có một cảnh báo deprecation từ Starlette về TestClient/httpx. Kết quả test đạt
không đồng nghĩa dependency không có rủi ro hoặc ứng dụng đã được kiểm thử tải lớn.

## Phần chưa xác minh

- Không chạy Docker vì máy kiểm tra chưa có Docker. Hướng dẫn Compose giữ nguyên.
- Chưa có live demo công khai được xác minh; chạy local theo README. Không gắn
  homepage hoặc nhãn LIVE dựa trên giả định repo mới tự triển khai.
- Kết quả CI trên GitHub của **repo mới** chỉ có sau khi publish/push. Các kết quả
  local ở trên không được dùng thay trạng thái CI hoặc deployment.
- Workspace theo cookie chưa phải đăng nhập tài khoản hay đồng bộ nhiều thiết bị;
  giữ các giới hạn bảo mật/vận hành công khai đã nêu ở README.
