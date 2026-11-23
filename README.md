# Enterprise SOC Infrastructure Lab

Phòng lab xây dựng một hệ thống mạng doanh nghiệp được phân đoạn, có giám sát an ninh. Hệ thống dự kiến được xây dựng và triển khai trên máy cục bộ trước, còn thiết kế được giữ trung lập với nhà cung cấp để sau này có thể chuyển lên cloud hoặc mô hình hybrid.

> **Giai đoạn (milestone) hiện tại: M1 – Planning Baseline, chưa hoàn thành.** M0 – Project Initiation (khởi động dự án) đã hoàn thành khi Owner phê duyệt Project Charter v1.0. Hoạt động Khởi tạo (Bootstrap), tức chuẩn bị cấu trúc dự án và các quy tắc làm việc, đã được thực hiện; Bootstrap không phải là milestone. Chưa có thành phần nào được thực hiện (Implemented) hoặc xác minh (Verified). Kế hoạch milestone xem [`docs/01-planning/project-charter.md`](docs/01-planning/project-charter.md).

## Phạm vi

| # | Thành phần | Vai trò | Trạng thái |
|---|------------|---------|------------|
| 1 | VMware Workstation | Nền tảng ảo hóa cục bộ | Proposed |
| 2 | pfSense | Tường lửa (firewall) biên và định tuyến giữa các phân đoạn mạng | Proposed |
| 3 | Phân đoạn mạng | Bốn phân đoạn WAN, DMZ, LAN, SOC (không có phân đoạn quản trị riêng, xem quyết định còn mở) | Proposed |
| 4 | ModSecurity WAF | Reverse proxy và WAF đứng trước DVWA, chạy trong container riêng trên máy ảo Ubuntu | Proposed |
| 5 | DVWA | Ứng dụng web có lỗ hổng cố ý, chạy trong container riêng trên cùng máy ảo Ubuntu với WAF | Proposed |
| 6 | Windows Server AD DS | Một Domain Controller Active Directory | Proposed |
| 7 | Wazuh | SIEM dạng All-in-One (indexer, server, dashboard) | Proposed |

### Ngoài phạm vi

Shuffle SOAR, TheHive, MISP / VirusTotal, AI Agent, SOC hoàn chỉnh cho môi trường production, và HA/DR cho production.

## Nguyên tắc kiến trúc

- **Local-first:** mọi thành phần được xây dựng và xác minh trên VMware Workstation trước.
- **Cloud-ready / Hybrid-ready:** không có lựa chọn thiết kế nào ràng buộc dự án vào một nhà cung cấp cloud cụ thể.
- **Không cấp phát tài nguyên cloud** trong giai đoạn (milestone) hiện tại.
- **Tích hợp chung:** thiết kế phải cho phép tích hợp với thành phần của các thành viên khác trong nhóm.

Các quyết định kiến trúc được ghi lại dưới dạng ADR (Architecture Decision Record) trong [`docs/03-architecture-design/ADR`](docs/03-architecture-design/ADR). ADR có vòng đời riêng: **Proposed → Accepted / Rejected**, và **Superseded** khi bị ADR khác thay thế (mục 4.3 của [`CLAUDE.md`](CLAUDE.md)). ADR Accepted không có nghĩa cấu hình tương ứng đã Implemented hay Verified.

## Các quyết định còn mở

Các mục dưới đây chưa được chốt. Mỗi mục chỉ được chốt bằng ADR ở trạng thái Accepted. Danh sách đầy đủ ở mục 4.2 của [`CLAUDE.md`](CLAUDE.md).

| Quyết định | Tình trạng hiện tại |
|------------|---------------------|
| Nhà cung cấp cloud | UNKNOWN |
| Vị trí đặt Wazuh (local / cloud / hybrid) | UNDECIDED |
| VMware WAN connection mode (chế độ kết nối WAN: NAT / Bridged / Host-only) | UNDECIDED |
| Phân đoạn quản trị riêng | UNDECIDED (chỉ bổ sung khi có ADR được phê duyệt) |
| Vị trí cơ sở dữ liệu của DVWA (DVWA database placement) | UNDECIDED |
| Phương thức đăng ký Wazuh agent (Wazuh agent enrollment) | UNDECIDED |
| Kế hoạch địa chỉ IP cho từng phân đoạn | TBD |
| Phiên bản các thành phần | TBD |
| Giấy phép (license) | UNDECIDED (chờ chủ sở hữu quyết định) |
| Kênh tiếp nhận báo cáo lỗ hổng | TBD |
| Đường quản trị được kiểm soát | TBD |

Lưu lượng WAN → DMZ mặc định bị từ chối. Chỉ được mở khi **đồng thời**: (1) chế độ kết nối WAN trên VMware đã được chốt bằng ADR Accepted, **và** (2) lưu lượng WAN → DMZ cụ thể đó đã được cho phép bằng ADR Accepted (hai điều kiện có thể nằm trong cùng một ADR). Chốt chế độ kết nối WAN không tự động cho phép lưu lượng WAN → DMZ. Chi tiết ở mục 8 của [`CLAUDE.md`](CLAUDE.md) và [`SECURITY.md`](SECURITY.md).

## Cấu trúc repository

```
.claude/            Agent, command, rule, skill cho Claude Code
.specify/           Memory, script, template cho quy trình spec-driven
docs/
  01-planning/              Kế hoạch dự án, các giai đoạn (milestone)
  02-requirements/          Yêu cầu chức năng và yêu cầu bảo mật
  03-architecture-design/   Tài liệu kiến trúc, ADR/, diagrams/
  04-implementation/        Quy trình xây dựng
  05-testing/               Test strategy, test plan, test case, test procedure, tiêu chí pass/fail
  06-deployment/            Ghi chú triển khai và mức độ sẵn sàng cho cloud
  07-operations/            Runbook, bảo trì, rollback
configs/            Cấu hình đã làm sạch (sanitized) theo từng thành phần
  pfsense/  waf/  dvwa/  active-directory/  wazuh/
scripts/            setup/  validation/  utilities/
tests/              Test script, mã tự động hóa, fixture, dữ liệu kiểm thử đã làm sạch
  network/  firewall/  waf/  active-directory/  wazuh/
evidence/           Bằng chứng đã làm sạch từ các lần kiểm thử thực tế, theo từng thành phần
reports/            Báo cáo và tổng kết
```

Các thư mục đánh số trong `docs/` là **nhóm tài liệu**, không phải giai đoạn (milestone) của dự án. Bảng đối chiếu thư mục giữa `configs/`, `tests/` và `evidence/` ở mục 13.2 của [`CLAUDE.md`](CLAUDE.md).

## Mô hình trạng thái

Nhãn trạng thái được gắn cho thành phần, cấu hình và kết quả kiểm thử khi cần phân biệt mức độ hoàn thành. Quyết định kiến trúc (ADR) dùng vòng đời riêng, không dùng các nhãn này. Không bắt buộc gắn nhãn cho mọi nhận định trong tài liệu.

- **Proposed** (đề xuất): được đề xuất trong thiết kế hoặc phạm vi dự án, nhưng chưa được thực hiện hoặc cấu hình trong lab.
- **Implemented** (đã thực hiện): đã thực hiện hoặc cấu hình trong lab; không mặc nhiên có nghĩa là đã kiểm thử thành công.
- **Verified** (đã xác minh): đã được xác minh bằng phương pháp phù hợp, có kết quả và bằng chứng tương ứng. Bằng chứng kỹ thuật lưu trong `evidence/`; với kiểm thử kỹ thuật, liên kết đến test case trong `docs/05-testing/` và test script trong `tests/` nếu có.

Phê duyệt phạm vi dự án không đồng nghĩa với phê duyệt một quyết định kiến trúc cụ thể. Một thành phần có thể thuộc phạm vi đã được duyệt nhưng phương án kỹ thuật cụ thể vẫn mang trạng thái Proposed. Điều kiện chi tiết ở mục 6 của [`CLAUDE.md`](CLAUDE.md).

## Bắt đầu

1. Đọc [`CLAUDE.md`](CLAUDE.md) để nắm quy tắc làm việc và [`SECURITY.md`](SECURITY.md) để nắm yêu cầu bảo mật và cách xử lý thông tin bí mật.
2. Sao chép `.env.example` thành `.env` và điền giá trị trên máy cục bộ. Một số biến phụ thuộc vào quyết định còn mở; để trống nếu chưa xác định. **Tuyệt đối không commit `.env`.**

## Ngôn ngữ

Ngôn ngữ mặc định của repository là tiếng Việt. Tên công nghệ, lệnh, biến, file và nội dung code/log gốc giữ nguyên tiếng Anh. Xem chi tiết tại mục 2 của [`CLAUDE.md`](CLAUDE.md).

## Giấy phép

Chưa chọn.
