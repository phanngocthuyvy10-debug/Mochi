# TikiNOW Unified Operations 6.0.0

Hợp nhất TikiNOW Ops Command Center 2.8.0 và Operations Hub 5.0.6 thành **một extension**, một giao diện chính, một bộ tài khoản và một nguồn đơn đã lưu.

## Cài / nâng cấp

1. Giải nén. Trong `chrome://extensions`, bật Developer mode và Load unpacked thư mục **TikiNOW_Unified_v6.0.0** có manifest.json.
2. Nếu đang dùng Hub 5.0.6 và muốn giữ cache/extension ID, chép toàn bộ nội dung thư mục mới vào **thư mục Hub đang cài**, rồi Reload extension. Không gỡ bản cũ trước. Đợi lượt đồng bộ đang chạy kết thúc rồi mới cập nhật.
3. Bấm icon extension để mở `app.html`. Đăng nhập TikiNOW Center trên cùng Chrome, rồi Tài khoản → Quét lại đăng nhập nếu cần.
4. Chrome dùng dữ liệu riêng cho từng extension ID. Một bản cài mới không tự đọc được cache của hai extension cũ. Từ Command Center cũ, xuất backup RAW JSON/JSON.GZ; trong bản mới vào **Cài đặt → Nhập dữ liệu Command Center cũ**. Chỉ thêm đơn chưa có và metadata catalog, không ghi đè đơn Hub hiện tại hoặc nhập token trong file. Resource/snapshot lịch sử của bản cũ không được nhập bởi chức năng này; giữ bản backup nếu cần đối soát.
5. Các phiên Command Center cũ trong cùng extension ID được xác minh seller qua `/api/user` rồi dùng chung. Quét lại đăng nhập có thể thử lại bước này. Nếu tài khoản hết phiên, đăng nhập lại bằng chính tài khoản đó.

## Phần được giữ và hợp nhất

| Nguồn | Chức năng sử dụng |
|---|---|
| Hub 5.0.6 | Đơn hàng, đơn hoàn tất, ngày tạo, seller ID, FD/CR, KPI PDF 2026, cảnh báo hậu mãi, tài khoản, tra mã TikiNOW / đối tác, chẩn đoán video, Excel |
| Command Center 2.8.0 | Backlog, luồng xử lý, năng suất kho, cycle-time, kết quả giao, prefix/brand mapping, Field Explorer, catalog/schema API, sơ đồ Center, mục tài nguyên đã học và nối chéo |
| Hợp nhất | Bộ đọc media rộng hơn với kiểm tra seller/mã đơn; giữ URL gốc để phát; hiển thị nhãn thực tế do API trả về; một luồng tải đơn, không tải lại khi mở các màn hình phân tích |

Không chạy đồng thời hai bộ đồng bộ đơn. Các module phân tích được nạp khi mở; tái sử dụng cache Hub qua adapter cùng origin, giữ namespace/CSS riêng. Kho tài nguyên API và catalog của Command Center được giữ cho dữ liệu bổ sung, không dùng làm nguồn đơn thứ hai.

KPI chính dùng engine PDF của Hub. Các dự báo năng lực/luồng xử lý là phân tích theo cấu hình vận hành, không thay KPI chính. Bảng xu hướng được tính cục bộ theo **ngày tạo đơn và trạng thái hiện tại**, thay luồng WBR backend cũ. Không cần backend relay token; không tự gửi cảnh báo/webhook của bản cũ.

## Sử dụng

- Đồng bộ ở thanh chung. Chọn seller ở góc trên; nút đồng bộ chính áp dụng seller đang chọn hoặc tất cả nếu không lọc.
- Muốn cả đơn đang mở và hoàn tất: Theo ngày → Đơn hàng: Tất cả trạng thái → chọn khoảng ngày tạo → Đồng bộ.
- Muốn chỉ hoàn tất: Danh sách đơn → Tải đơn hàng hoàn tất.
- Mặc định `Giới hạn tự chọn = 0`: không chặn tổng ở 60.000. Vẫn phân trang/chia khoảng khi chạm trần truy vấn. Lưu theo đợt trong một transaction; lỗi hoặc hủy lúc lưu giữ danh sách cũ.
- Phân tích chỉ phản ánh **tập đơn đã tải của seller đang chọn**. Nếu chỉ tải delivered, không thể dùng tập đó làm backlog toàn kho. Phạm vi được ghi rõ phía trên. Cycle-time chỉ dùng histories/mốc đã có; không tự tải detail của hàng trăm nghìn đơn. Field Explorer quét mẫu tối đa 1.500 đơn để tránh treo Chrome.
- Catalog/API: khi dùng Center, observer ghi endpoint, method và schema, không lưu toàn bộ response body vào catalog. Một endpoint chưa được quan sát không được coi là đã biết. Mục Center đã học có thể nạp tài nguyên GET khi người dùng bấm; không tự replay POST tạo/đổi dữ liệu hoặc yêu cầu video tính phí.
- Video: đọc hai API bằng chứng đã có, media nhúng trong chi tiết và các cấu trúc media hỗ trợ. API trả `data:null` không được biến thành URL đoán từ tên bucket. Chưa xác định endpoint tạo/đăng ký video mới; bản hợp nhất không tuyên bố giải quyết việc đó. Có link không đồng nghĩa trình duyệt phát được mọi codec/quyền truy cập.

## Dữ liệu và quyền

Cần cookies, scripting, storage, unlimitedStorage và webRequest (kế thừa hai bản). Host cố định: api.tikinow.vn và tikinowcenter.tikinow.vn. Token giữ trong Chrome của người dùng; alias tương thích cho engine cũ được lấy từ tài khoản đã xác minh. Không gửi token lên backend riêng. File ZIP không chứa phiên đăng nhập hoặc dữ liệu đơn cá nhân của người dùng.

Xem VALIDATION.md để biết những kiểm tra đã chạy và giới hạn kiểm chứng.
