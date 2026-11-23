# CLAUDE.md: Enterprise SOC Infrastructure Lab

Hướng dẫn dành cho Claude Code (và mọi trợ lý AI) khi làm việc trong repository này. Các quy tắc dưới đây được ưu tiên hơn hành vi mặc định.

## 1. Định danh dự án

- **Tên:** Enterprise SOC Infrastructure Lab
- **Repository:** `enterprise-soc-infrastructure-lab`
- **Kiến trúc:** Local-first, Cloud-ready / Hybrid-ready
- **Giai đoạn (milestone) hiện tại:** M1 – Planning Baseline, **chưa hoàn thành**. M0 – Project Initiation (khởi động dự án) đã hoàn thành ngày 2026-11-23 khi Owner phê duyệt Project Charter v1.0. Hoạt động Khởi tạo (Bootstrap), tức chuẩn bị cấu trúc repository và quy tắc quản lý, đã được thực hiện; Bootstrap không phải là milestone. Chưa có thành phần nào được thực hiện trong lab. Kế hoạch milestone xem `docs/01-planning/project-charter.md`, mục 15.

Workspace này hoàn toàn độc lập. Mọi workspace hoặc dự án cũ nằm ngoài thư mục repository này là **dự án riêng** và phải được bảo vệ: không truy cập (đọc, sửa, di chuyển, xóa hoặc đổi tên) và không dùng lại thiết lập hay cấu hình Claude của chúng nếu chủ sở hữu chưa yêu cầu hoặc cho phép rõ ràng. Không ghi đường dẫn tuyệt đối của máy cá nhân vào tài liệu được chia sẻ.

## 2. Ngôn ngữ và chuẩn tài liệu

Ngôn ngữ mặc định của toàn bộ repository là **tiếng Việt**, bao gồm:
- `README.md`, `CLAUDE.md`, `SECURITY.md`.
- Toàn bộ tài liệu trong `docs/`.
- Hướng dẫn cài đặt, cấu hình và kiểm thử.
- Báo cáo, ghi chú kỹ thuật và chú thích (comment) trong file cấu hình mẫu.

Giữ nguyên tiếng Anh đối với:
- Tên công nghệ, sản phẩm và giao thức (pfSense, ModSecurity, Wazuh, Active Directory, HTTP, LDAP, …).
- Tên lệnh, tham số, biến môi trường và khóa cấu hình.
- Thuật ngữ chuyên ngành không có bản dịch tương đương, hoặc bản dịch dễ gây hiểu nhầm.
- Tên file và thư mục.
- Nội dung code và log gốc.

Cách viết:
- Khi một thuật ngữ chuyên ngành xuất hiện lần đầu trong tài liệu, giải thích ngắn bằng tiếng Việt và ghi thuật ngữ tiếng Anh trong ngoặc nếu cần, ví dụ: "tường lửa (firewall)".
- Không dịch máy móc. Không viết cả câu bằng tiếng Anh khi có thể diễn đạt rõ bằng tiếng Việt.
- Dùng thuật ngữ nhất quán trong toàn repository theo bảng dưới. Bổ sung vào bảng khi xuất hiện thuật ngữ mới.

| Thuật ngữ tiếng Anh | Cách dùng trong repository |
|---------------------|----------------------------|
| lab | lab (môi trường thử nghiệm của dự án); giữ nguyên |
| cloud, hybrid | giữ nguyên; hybrid là mô hình kết hợp hạ tầng cục bộ và cloud |
| production | môi trường vận hành thực tế (production); dự án này không dành cho production |
| firewall | tường lửa (firewall); trong tên thư mục và ngữ cảnh kỹ thuật giữ `firewall` |
| network segmentation / segment | phân đoạn mạng / phân đoạn |
| rule (firewall) | rule (quy tắc tường lửa) |
| default-deny | mặc định từ chối (default-deny) |
| least privilege | đặc quyền tối thiểu (least privilege) |
| reverse proxy | reverse proxy (máy chủ trung gian nhận yêu cầu rồi chuyển tiếp tới backend) |
| container | giữ nguyên (container) |
| dashboard | giữ nguyên, ví dụ Wazuh dashboard (giao diện web giám sát) |
| agent | agent (phần mềm cài trên máy được giám sát, gửi dữ liệu về máy chủ, ví dụ Wazuh agent). "AI Agent" là hạng mục ngoài phạm vi |
| secret | thông tin bí mật (secret) |
| sanitize | làm sạch (sanitize): loại bỏ thông tin nhạy cảm |
| redact | che (redact): che thông tin nhạy cảm trong bằng chứng |
| rotate (secret) | thay mới (rotate) |
| evidence | bằng chứng |
| rollback | rollback (khôi phục về trạng thái trước) |
| snapshot, backup | snapshot, bản sao lưu (backup) |
| deploy / deployment | triển khai: **chỉ** dùng cho hoạt động đưa thành phần vào môi trường chạy; không dùng thay cho nhãn Implemented |
| implement | thực hiện, cấu hình; tương ứng nhãn Implemented |
| validation / validate | kiểm tra tính hợp lệ (validation): đánh giá tính hợp lệ hoặc sự phù hợp với yêu cầu. Tên thư mục `scripts/validation/` giữ nguyên và là tên nhóm script (xem mục 13) |
| verification / verify | xác minh (verification): xác nhận kết quả bằng phương pháp kiểm tra và bằng chứng; đạt đủ điều kiện mới gắn nhãn Verified. Không dùng "validate" theo nghĩa này |
| Validation (trong nhãn đã duyệt) | Trong các nhãn OBJ-06 – Security Validation, M7 – Validation và nguyên tắc Evidence-based Validation của Project Charter, giữ nguyên "Validation" làm nhãn; nghĩa là kiểm thử và xác minh theo mục 6, không phải kiểm tra tính hợp lệ |
| test, test case | kiểm thử; "test case" giữ nguyên |
| Proposed / Implemented / Verified | giữ nguyên làm nhãn trạng thái của thành phần, cấu hình và kết quả kiểm thử (xem mục 6); không dùng làm vòng đời ADR |
| Proposed / Accepted / Rejected / Superseded | giữ nguyên làm trạng thái vòng đời ADR (xem mục 4.3) |
| test strategy, test plan, test procedure | giữ nguyên; là tài liệu kiểm thử trong `docs/05-testing/` |
| test script, fixture | giữ nguyên; là mã và dữ liệu phục vụ thực thi kiểm thử trong `tests/` |
| UNKNOWN / UNDECIDED / TBD | giữ nguyên: UNKNOWN là chưa có thông tin; UNDECIDED là quyết định chưa chốt; TBD là giá trị sẽ xác định sau |
| Proposed convention | quy ước đề xuất, chưa được chủ sở hữu phê duyệt |
| milestone | giai đoạn (milestone): mốc tiến độ của dự án, ví dụ M0 – Project Initiation. Danh sách milestone xem `docs/01-planning/project-charter.md`, mục 15 |
| Bootstrap | Khởi tạo (Bootstrap): hoạt động chuẩn bị repository và quy trình làm việc; không phải milestone |
| Project Initiation (M0) | khởi động dự án (Project Initiation); không gọi là "khởi tạo" để tránh trùng với Bootstrap |
| Project Charter | Project Charter (điều lệ dự án) |
| Project Owner, Owner | chủ sở hữu (Project Owner) |
| deliverable | deliverable (sản phẩm bàn giao); mã `D-NN` theo Project Charter |
| baseline | baseline (phiên bản cơ sở đã được duyệt) |
| acceptance criteria | tiêu chí chấp nhận (acceptance criteria) |
| exit condition | điều kiện hoàn thành (exit condition) của milestone |
| telemetry | dữ liệu đo lường (telemetry); "security telemetry" là dữ liệu đo lường bảo mật |
| Defense in Depth | phòng thủ nhiều lớp (Defense in Depth) |
| trust boundary | ranh giới tin cậy (trust boundary) |
| NIC mapping | ánh xạ card mạng (NIC mapping) |
| (thư mục `docs/NN-*`) | nhóm tài liệu, ví dụ `docs/03-architecture-design/`. Không gọi là "giai đoạn" |
| Domain Controller (DC) | giữ nguyên |
| WAF, SIEM, SOC, ADR, VM | giữ nguyên; VM có thể viết "máy ảo" |

## 3. Phạm vi

**Trong phạm vi:**
1. Hạ tầng ảo hóa VMware Workstation
2. Tường lửa pfSense
3. Phân đoạn mạng WAN, DMZ, LAN và SOC
4. ModSecurity WAF
5. Backend DVWA chạy trong container riêng, trên cùng máy ảo Ubuntu với WAF; WAF cũng chạy trong container riêng
6. Một Domain Controller Windows Server Active Directory
7. Wazuh SIEM All-in-One

**Ngoài phạm vi:** Shuffle SOAR, TheHive, MISP / VirusTotal, AI Agent, SOC hoàn chỉnh cho production, HA/DR cho production. Không thêm thành phần, cấu hình hay tài liệu cho các mục này trừ khi chủ sở hữu thay đổi phạm vi bằng văn bản.

## 4. Quyết định kiến trúc

### 4.1. Quyết định đã thống nhất

| Chủ đề | Quyết định |
|--------|------------|
| Môi trường triển khai | Cục bộ trước, trên VMware Workstation |
| Mục tiêu tích hợp | Tích hợp chung với các thành viên khác trong nhóm |
| Cấp phát tài nguyên cloud | **Không** trong giai đoạn (milestone) hiện tại |
| Phân đoạn mạng | Bốn phân đoạn: WAN, DMZ, LAN, SOC. Không thêm phân đoạn khác khi chưa có ADR được chủ sở hữu phê duyệt |
| Mô hình WAF/DVWA | WAF và DVWA trên một máy ảo Ubuntu, mỗi thành phần chạy trong container riêng; DVWA đứng sau WAF và chỉ được truy cập qua WAF |
| Domain Controller | Đúng một DC Windows Server AD |
| Mô hình triển khai SIEM | Wazuh All-in-One |

### 4.2. Quyết định còn mở

Không được tự chốt các mục dưới đây. Mỗi mục chỉ được chốt bằng ADR ở trạng thái **Accepted** (xem mục 4.3).

| Chủ đề | Tình trạng | Ghi chú |
|--------|------------|---------|
| Nhà cung cấp cloud | **UNKNOWN** | Không được giả định AWS, Azure, GCP hay bất kỳ nhà cung cấp nào |
| Vị trí đặt Wazuh | **UNDECIDED** | local / cloud / hybrid |
| Chế độ kết nối WAN trên VMware (VMware WAN connection mode) | **UNDECIDED** | NAT / Bridged / Host-only. Chốt chế độ này không tự động cho phép lưu lượng WAN → DMZ; điều kiện mở WAN → DMZ xem mục 8 |
| Phân đoạn quản trị riêng | **UNDECIDED** | Hiện không tồn tại. Chỉ bổ sung khi có ADR được phê duyệt; không tạo subnet hoặc IP cho phân đoạn này |
| Vị trí cơ sở dữ liệu của DVWA (DVWA database placement) | **UNDECIDED** | Chưa chọn phương án nào; không giả định container cơ sở dữ liệu riêng hay vị trí lưu trữ khác |
| Phương thức đăng ký Wazuh agent (Wazuh agent enrollment) | **UNDECIDED** | Chưa chọn phương thức nào; không giả định password-based enrollment hay phương thức khác |
| Kế hoạch địa chỉ IP cho từng phân đoạn | **TBD** | |
| Phiên bản các thành phần | **TBD** | |
| Giấy phép (license) | **UNDECIDED** | Chờ chủ sở hữu quyết định (xem mục 10) |
| Kênh tiếp nhận báo cáo lỗ hổng | **TBD** | Xem [`SECURITY.md`](SECURITY.md) |
| Đường quản trị được kiểm soát | **TBD** | Xem mục 8 |
| Quy ước đặt tên ADR và bằng chứng | **Proposed convention** | Xem mục 13.1 |

Mọi quyết định mới hoặc thay đổi được ghi thành ADR trong `docs/03-architecture-design/ADR/`. Quy ước đặt tên và nội dung ADR xem mục 13.1.

### 4.3. Vòng đời ADR

ADR dùng vòng đời riêng, tách biệt với nhãn trạng thái của thành phần và cấu hình (mục 6):

| Trạng thái ADR | Ý nghĩa |
|----------------|---------|
| **Proposed** | Đang được đề xuất, chưa được chấp thuận |
| **Accepted** | Quyết định đã được chấp thuận |
| **Rejected** | Đề xuất bị từ chối |
| **Superseded** | Đã được thay thế bởi ADR khác |

- Trong repository, "ADR được (chủ sở hữu) phê duyệt" nghĩa là ADR ở trạng thái **Accepted**.
- ADR Accepted không có nghĩa cấu hình tương ứng đã **Implemented**. Cấu hình Implemented không có nghĩa đã **Verified**.
- Không dùng Implemented hoặc Verified làm trạng thái ADR. Chữ "Proposed" xuất hiện ở cả hai hệ thống nhưng phải hiểu theo ngữ cảnh: ADR Proposed là đề xuất quyết định chưa được chấp thuận; thành phần Proposed là hạng mục chưa được thực hiện trong lab.
- Phê duyệt phạm vi dự án (mục 3) không đồng nghĩa với phê duyệt một quyết định kiến trúc cụ thể; phương án kỹ thuật cụ thể chỉ được chốt bằng ADR Accepted.

## 5. Nguyên tắc Local-first, Cloud-ready

- Xây dựng và xác minh mọi thứ trên máy cục bộ trước khi lên kế hoạch chuyển lên cloud.
- Giữ thiết kế trung lập với nhà cung cấp: mô tả nhu cầu (ví dụ "một phân đoạn có định tuyến", "một máy ảo N vCPU"), không mô tả dịch vụ của nhà cung cấp cụ thể.
- Tham số hóa các giá trị môi trường (địa chỉ, hostname, port) qua `.env` hoặc biến có tài liệu, không hardcode trong script.
- Giữ ranh giới rõ ràng giữa các thành phần (tường lửa, WAF/web, định danh, SIEM) để mỗi thành phần có thể di chuyển độc lập.
- Mô tả mọi điểm tích hợp (chuyển tiếp log, đăng ký agent, xác thực) như một giao diện mà thành viên khác trong nhóm có thể sử dụng.

## 6. Quy tắc trạng thái: Proposed / Implemented / Verified

Nhãn giữ nguyên tiếng Anh để thống nhất trong toàn repository.

**Phạm vi gắn nhãn:** gắn đúng một nhãn cho thành phần, cấu hình và kết quả kiểm thử khi cần phân biệt mức độ hoàn thành. Quyết định kiến trúc (ADR) dùng vòng đời riêng ở mục 4.3, không dùng các nhãn này. Không bắt buộc gắn nhãn cho mọi câu văn hay nhận định thông thường trong tài liệu.

| Nhãn | Ý nghĩa | Điều kiện |
|------|---------|-----------|
| **Proposed** | Được đề xuất trong thiết kế hoặc phạm vi dự án, nhưng chưa được thực hiện hoặc cấu hình trong lab | Mặc định cho mọi hạng mục chưa thực hiện |
| **Implemented** | Đã thực hiện hoặc cấu hình trong lab; không mặc nhiên có nghĩa là đã kiểm thử thành công | Chủ sở hữu xác nhận đã áp dụng |
| **Verified** | Đã được xác minh bằng phương pháp phù hợp, có kết quả và bằng chứng tương ứng | Đáp ứng đủ các điều kiện Verified dưới đây |

Phê duyệt phạm vi dự án không làm thay đổi nhãn: một thành phần có thể thuộc phạm vi đã được duyệt nhưng phương án kỹ thuật cụ thể vẫn mang trạng thái Proposed.

**Điều kiện Verified:**
1. Có phương pháp kiểm tra phù hợp, được mô tả trước khi kiểm tra.
2. Có kết quả kiểm tra được ghi lại.
3. Có bằng chứng tương ứng, đã làm sạch. Bằng chứng kỹ thuật lưu trong `evidence/<component>/`.
4. Với kiểm thử kỹ thuật (kết nối, rule tường lửa, WAF chặn, xác thực, sinh cảnh báo, …), liên kết đến test case tương ứng trong `docs/05-testing/` và, nếu có, test script trong `tests/<component>/`. Không bắt buộc mọi test case phải có script tự động. Việc xác minh không phải kiểm thử kỹ thuật (ví dụ rà soát tài liệu) không bắt buộc có test case.

**Cách dùng từ:**
- Dùng "đã thực hiện" hoặc "đã cấu hình" để mô tả trạng thái Implemented.
- Chỉ dùng "triển khai" khi nói đến hoạt động deployment, không dùng làm đồng nghĩa với Implemented.
- Chỉ dùng "đã xác minh" cho trạng thái Verified.
- Không nâng trạng thái khi chưa đủ điều kiện tương ứng.
- Không mô tả một hạng mục là đã thực hiện hoặc đã cấu hình khi nó chưa ở trạng thái **Implemented**. Không mô tả là đang hoạt động đúng, đã kiểm thử thành công hay đã xác minh khi nó chưa ở trạng thái **Verified**.

## 7. Không bịa đặt thông tin

Không được tự đặt ra:
- Địa chỉ IP, subnet, VLAN ID, gateway hoặc DNS server
- Port hoặc giao thức ngoài những gì chủ sở hữu hoặc tài liệu chính thức chỉ định
- Tên đăng nhập, mật khẩu, khóa, token hay bất kỳ thông tin xác thực nào
- Phiên bản phần mềm, số build hoặc ngày phát hành
- Kết quả kiểm thử, log, ảnh chụp màn hình, đầu ra cảnh báo hay bất kỳ bằng chứng nào

Nếu chưa biết giá trị, ghi `TBD` hoặc `UNKNOWN` và đưa vào danh sách việc còn mở. Nếu giá trị lấy từ tài liệu của nhà cung cấp, ghi rõ nguồn.

## 8. Yêu cầu bảo mật

- Đặc quyền tối thiểu (least privilege): rule tường lửa mặc định từ chối (default-deny), chỉ cho phép lưu lượng giữa các phân đoạn khi có rule tường minh và được ghi lại.
- DVWA có lỗ hổng cố ý. Không bao giờ để DVWA truy cập được từ bên ngoài lab, và chỉ được truy cập qua WAF.
- Lưu lượng WAN → DMZ: **mặc định bị từ chối**. Chỉ được mở lưu lượng WAN → DMZ khi đáp ứng **đồng thời cả hai** điều kiện:
  1. Chế độ kết nối WAN trên VMware (VMware WAN connection mode) đã được chốt bằng một ADR ở trạng thái **Accepted**; **và**
  2. Lưu lượng WAN → DMZ cụ thể đó đã được cho phép bằng một ADR ở trạng thái **Accepted**.

  Hai điều kiện có thể nằm trong cùng một ADR nếu nội dung quyết định thể hiện rõ cả hai vấn đề. Chốt chế độ kết nối WAN không tự động cho phép lưu lượng WAN → DMZ. Không mở lưu lượng dựa trên ADR ở trạng thái Proposed, quyết định chưa được chấp thuận hoặc ghi chú thiết kế chưa có ADR Accepted. Thiếu bất kỳ điều kiện nào thì giữ nguyên trạng thái từ chối mặc định.
- Mọi rule khác cho phép lưu lượng từ WAN phải có lý do được ghi trong ADR hoặc tài liệu thiết kế.
- Không mặc định NAT hay Bridged là an toàn trong mọi tình huống. Mức độ phơi nhiễm của từng chế độ phải được đánh giá trong ADR trước khi chọn.
- Giao diện quản trị (pfSense, Wazuh dashboard, quản trị DC) chỉ được truy cập từ phân đoạn SOC hoặc qua đường quản trị được kiểm soát nằm trong kiến trúc đã duyệt; cách thức cụ thể phải được ghi trong tài liệu thiết kế (TBD). Hiện không có phân đoạn quản trị riêng (xem mục 4.2).
- Lưu lượng tấn công thử nghiệm (ví dụ nhắm vào DVWA) phải nằm trong lab. Không bao giờ nhắm vào hệ thống không thuộc quyền sở hữu.
- Tuân thủ [`SECURITY.md`](SECURITY.md).

## 9. Xử lý thông tin bí mật (secret)

- Secret **chỉ** được lưu trong file `.env` cục bộ (đã bị git bỏ qua) hoặc trình quản lý mật khẩu, không bao giờ nằm trong file được theo dõi.
- `.env.example` chỉ chứa **tên** biến với giá trị trống hoặc giá trị giữ chỗ. Không đưa secret thật hoặc trông giống thật vào đó.
- Không in, ghi log, echo hay dán secret vào tài liệu, commit, bằng chứng, báo cáo hoặc đầu ra trò chuyện.
- Làm sạch cấu hình trước khi commit: xóa hoặc thay thế mật khẩu, hash mật khẩu, pre-shared key, private key, chứng chỉ, API key và khóa đăng ký agent.
- Che (redact) thông tin nhạy cảm trong bằng chứng (ảnh chụp màn hình, log) trước khi lưu vào `evidence/`.
- Nếu lỡ commit secret: dừng lại, báo cho chủ sở hữu và coi secret đó đã bị lộ (phải thay mới). Viết lại lịch sử Git không xóa được việc đã bị lộ.

## 10. Quy tắc thực thi an toàn

- **Kiểm tra trước khi sửa:** đọc file hoặc cấu hình hiện có, hoặc kiểm tra trạng thái hiện tại, trước khi thay đổi bất cứ điều gì.
- **Không thực hiện hành động phá hủy khi chưa được chấp thuận rõ ràng:** xóa file hoặc thư mục, các thao tác force, reset hoặc revert máy ảo và snapshot, xóa ổ đĩa, xóa dữ liệu, gỡ rule tường lửa, hạ cấp DC, `git reset --hard`, `git push --force` và các thao tác tương tự.
- Không cài đặt package, tạo hoặc sửa máy ảo, thay đổi thiết lập VMware hay triển khai bất kỳ thành phần nào trừ khi chủ sở hữu yêu cầu đúng bước đó.
- Không chạy lệnh trên hạ tầng đang hoạt động (pfSense, DC, Wazuh, máy chủ WAF) khi chưa được chấp thuận rõ ràng cho hành động đó.
- Không khởi tạo Git, thêm remote, push hay công khai khi chưa được chủ sở hữu chấp thuận.
- Không tự chọn hoặc thêm LICENSE. Hỏi chủ sở hữu.
- Ưu tiên lệnh chỉ đọc hoặc chạy thử (dry-run) trước. Nêu rõ lệnh sẽ làm gì trước khi chạy.
- Chỉ làm việc trong thư mục của repository này.

## 11. Xác minh mọi thay đổi cấu hình

Với mỗi thay đổi cấu hình:
1. Ghi lại **trạng thái trước thay đổi** (bản export hoặc bản sao lưu, đã làm sạch).
2. Mô tả thay đổi và kết quả mong đợi.
3. Chỉ áp dụng sau khi chủ sở hữu chấp thuận.
4. **Xác minh** bằng phương pháp kiểm tra đã định nghĩa (ví dụ kết nối, rule được khớp, WAF chặn, xác thực, sinh cảnh báo), liên kết test case trong `docs/05-testing/` (và test script trong `tests/<component>/` nếu có) khi là kiểm thử kỹ thuật, và ghi lại kết quả.
5. Lưu bằng chứng đã làm sạch vào `evidence/<component>/` và cập nhật trạng thái.

Thay đổi đã áp dụng nhưng chưa được xác minh chỉ ở trạng thái **Implemented**, không bao giờ là **Verified**.

## 12. Hướng dẫn rollback (bắt buộc với thay đổi hạ tầng)

Mỗi quy trình thay đổi hạ tầng trong `docs/04-implementation/` hoặc `docs/07-operations/` phải có mục **Rollback** gồm:
- **Sao lưu trước thay đổi:** ví dụ export cấu hình pfSense, snapshot máy ảo (khi được chấp thuận), bản sao file cấu hình WAF, sao lưu system state của AD, sao lưu cấu hình Wazuh.
- **Các bước rollback:** các bước cụ thể để quay về trạng thái trước.
- **Xác minh rollback:** cách xác nhận trạng thái trước đã được khôi phục.
- **Điểm không thể quay lại:** chỉ rõ bước nào không thể hoàn tác (ví dụ thay đổi forest hoặc schema của AD).

## 13. Quy ước repository

- Tài liệu: Markdown, viết bằng tiếng Việt (xem mục 2), đặt trong các nhóm tài liệu được đánh số của `docs/` (ví dụ `docs/03-architecture-design/`). Số thứ tự thể hiện nhóm nội dung, không phải giai đoạn (milestone) của dự án.
- Cấu hình: đã làm sạch, mỗi thành phần một thư mục trong `configs/`.
- Kiểm thử và bằng chứng được tách theo vai trò, không chồng lấn:

  | Vị trí | Vai trò |
  |--------|---------|
  | `docs/05-testing/` | Test Strategy, Test Plan, Test Case, Test Procedure, tiêu chí pass/fail và báo cáo tổng hợp. Mỗi test case nêu mục đích, điều kiện tiên quyết, các bước và kết quả mong đợi |
  | `tests/<component>/` | Test script, mã tự động hóa, fixture và dữ liệu kiểm thử đã làm sạch phục vụ thực thi |
  | `evidence/<component>/` | Bằng chứng thực tế đã làm sạch, ví dụ log, ảnh chụp, output và kết quả kiểm tra |

  Test case thủ công có thể chỉ được mô tả trong `docs/05-testing/`. Không bắt buộc mọi test case phải có script tự động. Tài liệu mô tả test case không đồng nhất với mã thực thi kiểm thử.
- Script: `scripts/validation/` là tên nhóm script, không giới hạn script trong thư mục này chỉ được dùng cho validation (kiểm tra tính hợp lệ); script xác minh (verification) cũng có thể đặt ở đây. Mỗi script cần có tên và mô tả thể hiện rõ mục đích.
- File lớn hoặc dữ liệu thô (ổ đĩa máy ảo, log thô, packet capture) không được commit. Xem `.gitignore`.

### 13.1. Quy ước đặt tên

**Quy ước đã được chủ sở hữu phê duyệt** (bản làm sạch `.sanitized.*` và template):

| Loại | Mẫu tên | Ghi chú |
|------|---------|---------|
| Cấu hình pfSense đã làm sạch | `*.sanitized.xml` trong `configs/pfsense/` | `.gitignore` chỉ cho phép file `.xml` có hậu tố này trong `configs/pfsense/` |
| CSV bằng chứng đã làm sạch | `*.sanitized.csv` trong `evidence/` | `.gitignore` chặn mọi file CSV khác trong `evidence/` |
| Log bằng chứng đã làm sạch | `*.sanitized.log` trong `evidence/` | `.gitignore` chặn mọi file `.log` khác |
| Bản làm sạch của cấu hình khác | `<tên>.sanitized.<đuôi>`, ví dụ `internal_users.sanitized.yml` | Tên khác với file gốc nên không trùng pattern chặn |
| Template | Đuôi `.example` (ví dụ `.env.example`, `*.tfvars.example`), `.dist` (ví dụ `config.inc.php.dist`) hoặc `.template` (ví dụ `*.reg.template`) | Template chỉ chứa giá trị trống hoặc giá trị giữ chỗ, không chứa secret |
| Sao lưu AD và registry (chỉ lưu cục bộ, không commit) | Thư mục `ad-backup/`; registry hive xuất bằng `reg save` dùng đuôi `.hiv` | Luôn bị `.gitignore` chặn |

Quy tắc đi kèm:
- Hậu tố `.sanitized.` luôn viết thường. Ngoại lệ trong `.gitignore` chỉ khớp chữ thường; trên Linux, tên viết hoa sẽ bị chặn.
- `.sanitized.*` là **quy ước đặt tên**, **không phải bằng chứng** dữ liệu đã được làm sạch. Nội dung vẫn phải được review (mục 13.3).
- Thư mục `raw/` trong `evidence/` luôn bị chặn, kể cả file có hậu tố `.sanitized`.
- File có tên chứa credentials, private key hoặc các loại luôn bị chặn khác (khối D của `.gitignore`) vẫn bị chặn dù có hậu tố `.sanitized`.

**Quy ước đề xuất (Proposed convention), chưa được chủ sở hữu phê duyệt.** Dùng làm hướng dẫn nhưng chưa bắt buộc và có thể thay đổi. (Vòng đời ADR ở mục 4.3 là quy định đã được xác nhận, không thuộc phần này.)

| Loại | Mẫu tên | Ghi chú |
|------|---------|---------|
| ADR | `ADR-NNNN-short-title.md` trong `docs/03-architecture-design/ADR/` | Nội dung gồm: bối cảnh, quyết định, phương án thay thế, hệ quả, trạng thái (theo vòng đời ADR, mục 4.3) |
| Bằng chứng | `YYYY-MM-DD_<test-id>_<short-desc>.<ext>` trong `evidence/<component>/` | Định dạng `<test-id>` chưa được quy định (TBD) |

### 13.2. Đối chiếu thư mục theo thành phần

Bảng giúp xác định vị trí lưu file. Tài liệu test case nằm trong `docs/05-testing/` (mục 13), không nằm trong bảng này. Tên thư mục giữa các khu vực không hoàn toàn trùng nhau; không đổi cấu trúc thư mục khi chưa được chủ sở hữu chấp thuận.

Không tạo thư mục mới chỉ để các nhánh `configs/`, `tests/`, `evidence/` đối xứng nhau. Chỉ xem xét tạo thư mục khi xuất hiện artifact thực tế cần lưu và có căn cứ về vị trí lưu phù hợp.

| Thành phần | Cấu hình (`configs/`) | Test script (`tests/`) | Bằng chứng (`evidence/`) |
|------------|-----------------------|------------------------|--------------------------|
| Tường lửa pfSense | `configs/pfsense/` | `tests/firewall/` | `evidence/firewall/` |
| Phân đoạn mạng | Không có thư mục riêng | `tests/network/` | `evidence/network/` |
| ModSecurity WAF | `configs/waf/` | `tests/waf/` | `evidence/waf/` |
| DVWA | `configs/dvwa/` | Không có thư mục riêng | Không có thư mục riêng |
| Active Directory (DC) | `configs/active-directory/` | `tests/active-directory/` | `evidence/active-directory/` |
| Wazuh SIEM | `configs/wazuh/` | `tests/wazuh/` | `evidence/wazuh/` |
| VMware Workstation | Không có thư mục riêng | Không có thư mục riêng | Không có thư mục riêng |

### 13.3. Quy tắc trước khi staging

1. **Review nội dung mọi file trước khi staging** (`git add`), kể cả file có hậu tố `.sanitized`.
2. **File không bị ignore không đồng nghĩa với file an toàn để commit.** `.gitignore` chỉ lọc theo tên và vị trí, không kiểm tra nội dung.
3. Không commit private key, chứng chỉ kèm private key, credentials, password thật, hash mật khẩu, registry hive, cơ sở dữ liệu hoặc bản sao lưu AD, bản sao lưu cấu hình chưa làm sạch, database dump, packet capture, memory dump, log thô hoặc dữ liệu nhạy cảm khác (xem mục 9 và [`SECURITY.md`](SECURITY.md)).
4. Tài liệu viết bằng tiếng Việt; giữ nguyên tiếng Anh cho tên kỹ thuật, lệnh, tên file, tên thư mục và tên biến (mục 2).
5. Phân biệt Proposed, Implemented và Verified (mục 6). Không gắn Verified khi chưa có bằng chứng tương ứng.
6. Quét secret tự động hiện được hoãn, nên review thủ công là bắt buộc.
