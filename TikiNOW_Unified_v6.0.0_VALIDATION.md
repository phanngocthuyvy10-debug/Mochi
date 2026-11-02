# Kiểm tra bản hợp nhất 6.0.0

## Đã đạt

- `node --check` toàn bộ JavaScript ngoài vendor; manifest và các file được tham chiếu tồn tại, một service worker, một entry point.
- Chromium 154 qua server localhost, API được mô phỏng và mọi request ngoài fixture bị chặn: khởi động app, tra chi tiết/video, nhãn Pre-order, chuyển 11 màn hình phân tích, không có uncaught page error.
- Năng suất/cycle/kết quả giao đọc đúng số đơn fixture đã lưu; chuyển các màn hình phân tích không phát sinh request tải đơn mới.
- Lọc seller chung áp dụng cache và phân tích; nhập JSON và JSON.GZ, không ghi đè đơn Hub đã có, giữ dữ liệu account khác; Excel chính tạo file XLSX hợp lệ.
- Chạy toàn bộ worker qua VM với Chrome API mock: các import không xung đột global, một action/cookie observer/webRequest observer, mở app chính, GET_TOKENS trả một phản hồi.
- Kiểm tra shared auth: xác minh token/seller qua user API mô phỏng, alias Bearer, từ chối seller không khớp, bỏ alias của tài khoản đã xóa/hết hiệu lực; không relay token ra backend khác. Legacy token chưa xác minh không bị xóa chỉ vì bước xác minh tạm thời thất bại; không được dùng như tài khoản đã xác minh.
- Media: phân loại video_pack, giữ nguyên URL có chữ ký, loại media gắn seller/mã đơn khác. Migration loại dữ liệu không hợp lệ/seller không khớp/chi tiết sai mã.
- Hồi quy Hub: seller ID riêng với partner_code, FD/CR Excel, completed pagination, giới hạn tự chọn, mô phỏng tải 304.478 đơn khi server cắt total tại 10.000.
- Mã lưu thực tế chạy với fake-indexeddb 6.2.4: lưu 280.229 đơn giả; progress đúng; rollback khi hủy hoặc DataError; giữ seller khác; không nhầm lỗi dừng lượt trước với trạng thái lưu hiện tại.

## Giới hạn

- Chưa dùng tài khoản TikiNOW thật hoặc gọi API thật trong lần hợp nhất. Không chứng minh phát được video thật hay tồn tại endpoint video khác.
- Đã thử nạp unpacked extension trong Chromium của cloud; chính sách quản trị `ExtensionInstallBlocklist: ["*"]` ngăn cài extension mới, nên không hoàn tất kiểm tra native installation/service worker trong browser. Worker được kiểm tra bằng VM; app/frame bằng Chromium với Chrome API mock. Không thay đổi/bỏ qua chính sách quản trị đó.
- Chưa benchmark RAM, thời gian dựng KPI hoặc xuất Excel với hàng trăm nghìn đơn trên Chrome của người dùng. Mô phỏng tải/lưu lớn không phải đo hiệu năng máy thật.
- Dữ liệu của extension ID khác cần backup/import; chỉ nhập orders và catalog metadata, không tự nhập resource/snapshot lịch sử.
