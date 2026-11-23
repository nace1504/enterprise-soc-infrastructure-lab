# Chính sách bảo mật

## Tính chất của dự án

Đây là môi trường **lab** phục vụ học tập và trình diễn hạ tầng SOC. Lab cố ý chứa một ứng dụng có lỗ hổng (DVWA). Dự án **không** dành cho môi trường production.

## Quy tắc an toàn cho lab

1. **Cô lập:** mạng lab không được để lộ DVWA hay bất kỳ giao diện quản trị nào ra Internet hoặc các mạng không tin cậy.
2. **Chỉ truy cập qua WAF:** DVWA chỉ được truy cập thông qua ModSecurity WAF.
3. **Chỉ nhắm vào mục tiêu được phép:** các bài mô phỏng tấn công và quét chỉ được nhắm vào hệ thống trong lab do chủ sở hữu kiểm soát.
4. **Tường lửa mặc định từ chối (default-deny):** lưu lượng giữa các phân đoạn mạng bị chặn trừ khi được cho phép tường minh và có ghi lại.
5. **Giới hạn quản trị:** việc quản trị pfSense, Wazuh dashboard và DC chỉ được thực hiện từ phân đoạn SOC hoặc qua đường quản trị được kiểm soát nằm trong kiến trúc đã duyệt. Hiện không có phân đoạn quản trị riêng; việc bổ sung phân đoạn này là quyết định còn mở và chỉ được thực hiện khi có ADR được chủ sở hữu phê duyệt (trạng thái Accepted).
6. **Lưu lượng WAN → DMZ:** mặc định bị từ chối. Chỉ được mở khi đáp ứng **đồng thời cả hai** điều kiện: (1) chế độ kết nối WAN trên VMware đã được chốt bằng một ADR ở trạng thái Accepted, **và** (2) lưu lượng WAN → DMZ cụ thể đó đã được cho phép bằng một ADR ở trạng thái Accepted. Hai điều kiện có thể nằm trong cùng một ADR nếu nội dung quyết định thể hiện rõ cả hai vấn đề. Không mở lưu lượng dựa trên ADR Proposed, quyết định chưa được chấp thuận hoặc ghi chú thiết kế chưa có ADR Accepted; thiếu bất kỳ điều kiện nào thì giữ nguyên từ chối mặc định. ADR Accepted chỉ cho phép bắt đầu cấu hình; rule chỉ được coi là Verified sau khi được xác minh có bằng chứng.
7. **Chế độ kết nối WAN trên VMware:** hiện là UNDECIDED (NAT / Bridged / Host-only). Chốt chế độ này không tự động cho phép lưu lượng WAN → DMZ (xem quy tắc 6). Không được mặc định NAT hay Bridged là an toàn trong mọi tình huống. Mỗi chế độ có mức độ phơi nhiễm khác nhau đối với máy host và mạng xung quanh, nên phải được đánh giá trong ADR trước khi chọn.

## Xử lý thông tin bí mật (secret)

- Secret thật **chỉ** được lưu trong file `.env` cục bộ (đã bị git bỏ qua) hoặc trình quản lý mật khẩu.
- `.env.example` chỉ chứa tên biến, không có giá trị thật.
- Không bao giờ commit các nội dung sau:
  - mật khẩu, hash mật khẩu, API key, token
  - private key, chứng chỉ, keystore (`.pem`, `.key`, `.pfx`, `.p12`, …)
  - khóa đăng ký agent Wazuh hoặc file `client.keys`
  - bản export cấu hình pfSense chưa được làm sạch
  - bản sao lưu AD, `NTDS.dit` hoặc registry hive
- Làm sạch (sanitize) cấu hình và che (redact) thông tin nhạy cảm trong bằng chứng (ảnh chụp màn hình, log) trước khi đưa vào repository.

### Khi secret bị lộ

1. Dừng lại và báo ngay cho chủ sở hữu dự án.
2. Coi secret đó đã bị lộ và **thay mới (rotate)**.
3. Xóa secret khỏi repository. Khi secret đã được push, chỉ viết lại lịch sử Git là chưa đủ.

## Báo cáo lỗ hổng

Repository chưa được công bố trên nền tảng lưu trữ nào và chưa có kênh tiếp nhận báo cáo chính thức (TBD). Cho đến khi có kênh chính thức:

- Báo cáo riêng tư, trực tiếp cho chủ sở hữu dự án.
- Không công khai chi tiết lỗ hổng (ví dụ qua issue, pull request, diễn đàn hoặc kênh công khai khác trên bất kỳ nền tảng nào) trước khi chủ sở hữu xử lý.
- Nội dung báo cáo nên gồm: mô tả vấn đề, file hoặc thành phần liên quan, cách tái hiện trong lab và mức độ ảnh hưởng dự kiến.
- Không gửi kèm secret thật trong báo cáo. Nếu vấn đề là secret bị lộ, làm theo mục "Khi secret bị lộ".
- Phạm vi báo cáo: code, script, cấu hình và tài liệu trong repository. Các lỗ hổng cố ý của DVWA không thuộc phạm vi báo cáo.

## Các phiên bản được hỗ trợ

Chưa phát hành phiên bản nào. Phiên bản các thành phần chưa được cố định (TBD).
