# Project Charter: Enterprise SOC Infrastructure Lab

| Thuộc tính | Giá trị |
|------------|---------|
| Project ID | ESIL-001 |
| Tài liệu | Project Charter (điều lệ dự án) |
| Phiên bản | 0.3 |
| Trạng thái | Draft – Pending Final Owner Approval |
| Deliverable | D-01 |

Chi tiết metadata xem [mục 17](#17-metadata-tài-liệu).

> **Phạm vi hiệu lực của tài liệu.** Project Charter (điều lệ dự án) xác lập mục đích, phạm vi, mục tiêu và cơ chế quản trị của dự án. Tài liệu này không chốt các quyết định kiến trúc còn mở và không cho phép triển khai hạ tầng.
>
> Dự án theo dõi ba khía cạnh độc lập (chi tiết tại [mục 14.3](#143-định-nghĩa-trạng-thái)):
> - **Quyết định kiến trúc**: được chốt bằng ADR theo vòng đời ADR (Proposed / Accepted / Rejected / Superseded).
> - **Trạng thái thực hiện**: một thành phần hoặc cấu hình chuyển từ Proposed sang Implemented khi đã được thực hiện trong lab.
> - **Trạng thái xác minh**: một thành phần hoặc cấu hình chỉ đạt Verified khi đã được xác minh bằng phương pháp phù hợp và có bằng chứng.
>
> Một ADR có thể được Accepted **trước** khi thực hiện. Accepted không có nghĩa là Implemented; Implemented không có nghĩa là Verified. Tại thời điểm lập tài liệu này, mọi thành phần kỹ thuật được mô tả đều ở trạng thái thực hiện **Proposed**.

---

## 1. Bối cảnh dự án

| Thuộc tính | Giá trị |
|------------|---------|
| Tên dự án | Enterprise SOC Infrastructure Lab |
| Repository | `enterprise-soc-infrastructure-lab` |
| Thư mục tài liệu | `docs/01-planning/` |
| File tài liệu | `docs/01-planning/project-charter.md` |
| Mô hình dự án | Local-first, Cloud-ready / Hybrid-ready |
| Cách tiếp cận lập kế hoạch | Lấy chất lượng làm trọng tâm (quality-driven), theo giai đoạn (milestone-based) |
| Hạn hoàn thành | Không cố định |

**Local-first** nghĩa là mọi thành phần được xây dựng và xác minh trên máy cục bộ trước. **Cloud-ready / Hybrid-ready** nghĩa là thiết kế giữ trung lập với nhà cung cấp, để sau này có thể chuyển một phần hoặc toàn bộ lên cloud, hoặc vận hành theo mô hình hybrid (kết hợp hạ tầng cục bộ và cloud), mà không phải thiết kế lại từ đầu. Dự án hiện **không** cấp phát tài nguyên cloud.

## 2. Mô hình quản trị thực thi

Dự án vận hành theo mô hình do chủ sở hữu điều hành, có AI hỗ trợ (Owner-directed, AI-assisted execution model).

### 2.1. Project Owner – Doãn Hữu Nguyên

- Có thẩm quyền cuối cùng đối với mục tiêu và phạm vi dự án.
- Phê duyệt các quyết định kiến trúc và bảo mật.
- Phê duyệt deliverable (sản phẩm bàn giao) và baseline (phiên bản cơ sở đã được duyệt) của dự án.
- Cho phép thay đổi repository và các thao tác Git.
- Chấp nhận hoặc từ chối kết quả công việc.

### 2.2. ChatGPT – Planning and Review Assistant

- Nghiên cứu tiêu chuẩn kỹ thuật và tài liệu chính thức.
- Xây dựng kế hoạch dự án, yêu cầu và đề xuất kiến trúc.
- Soạn hướng dẫn thực thi cho Claude CLI.
- Rà soát kết quả do Claude CLI báo cáo.
- Phát hiện thiếu sót kỹ thuật, điểm không nhất quán và rủi ro.
- Đề xuất hành động khắc phục.

ChatGPT **không** tự cho phép thay đổi repository, triển khai hạ tầng hay thực hiện thao tác Git.

### 2.3. Claude CLI – Execution Agent

- Kiểm tra repository trong phạm vi được cho phép.
- Tạo và sửa file theo hướng dẫn đã được phê duyệt.
- Thực hiện cấu hình và script đã được cho phép.
- Chạy các thủ tục kiểm tra được yêu cầu.
- Báo cáo thay đổi, lệnh đã chạy, kết quả kiểm tra và các vấn đề chưa giải quyết.

Claude CLI **không** được tự mở rộng phạm vi dự án hoặc tự đưa ra quyết định kiến trúc chưa được phê duyệt.

### 2.4. Chu trình làm việc chuẩn

1. Owner xác định nhiệm vụ.
2. ChatGPT phân tích nhiệm vụ và chuẩn bị đề xuất.
3. Owner rà soát và cho phép thực hiện.
4. Claude CLI thực hiện công việc đã được cho phép.
5. Claude CLI báo cáo thay đổi thực tế và kết quả kiểm tra.
6. ChatGPT đối chiếu kết quả với tiêu chí chấp nhận (acceptance criteria).
7. Owner chấp nhận hoặc yêu cầu chỉnh sửa.
8. Deliverable đã được duyệt được ghi nhận vào repository.

Việc phê duyệt một nhiệm vụ **không** đồng nghĩa với việc cho phép các công việc không liên quan.

## 3. Các quyết định hiện có của dự án

Các thông tin dưới đây là bối cảnh dự án đã được chủ sở hữu thống nhất ở cấp định hướng và phạm vi. Cần phân biệt:

- **Chốt quyết định kiến trúc:** một phương án kỹ thuật cụ thể chỉ được chốt bằng ADR (Architecture Decision Record – bản ghi quyết định kiến trúc) ở trạng thái **Accepted**. ADR có thể được Accepted trước khi thực hiện; việc chốt quyết định không phải chờ đến khi đã thực hiện.
- **Trạng thái thực hiện:** các thành phần trong mục này hiện ở trạng thái **Proposed**, tức chưa được thực hiện trong lab. Chúng chỉ chuyển sang **Implemented** khi chủ sở hữu xác nhận đã áp dụng.
- **Trạng thái xác minh:** thành phần chỉ đạt **Verified** khi đáp ứng đủ điều kiện xác minh của repository.

Việc chủ sở hữu thống nhất định hướng ở mục này không đồng nghĩa với một ADR Accepted cho phương án kỹ thuật chi tiết.

### 3.1. Hạ tầng

| Hạng mục | Lựa chọn |
|----------|----------|
| Hypervisor (nền tảng ảo hóa) | VMware Workstation |
| Tường lửa (firewall) | pfSense |
| Hạ tầng web | Ubuntu Server |
| Bảo mật web | WAF Reverse Proxy và DVWA Backend chạy trong các container riêng biệt, trên cùng máy ảo Ubuntu Server |
| Hạ tầng định danh | Một Domain Controller Windows Server Active Directory |
| SIEM | Wazuh All-in-One |
| Mô hình triển khai | Local-first |
| Định hướng tương lai | Cloud-ready / Hybrid-ready |

Ghi chú:
- **WAF** (Web Application Firewall – tường lửa ứng dụng web) đóng vai trò reverse proxy (máy chủ trung gian nhận yêu cầu rồi chuyển tiếp tới backend) đứng trước DVWA.
- **DVWA** (Damn Vulnerable Web Application) là ứng dụng web có lỗ hổng cố ý, chỉ dùng cho kiểm thử trong lab.
- **SIEM** (Security Information and Event Management) là hệ thống thu thập, chuẩn hóa và phân tích sự kiện bảo mật tập trung.

### 3.2. Các vùng mạng logic

Dự án sử dụng bốn phân đoạn mạng (network segment):

| Vùng | Vai trò khái quát |
|------|-------------------|
| WAN | Phía bên ngoài của lab |
| DMZ | Vùng đặt các dịch vụ web hướng ra ngoài, tách biệt với mạng nội bộ |
| LAN | Mạng nội bộ, nơi đặt hạ tầng định danh |
| SOC | Vùng giám sát bảo mật |

Không thêm phân đoạn khác khi chưa có ADR ở trạng thái **Accepted**.

### 3.3. Vị trí đặt thành phần (đề xuất)

| Thành phần | Vùng đề xuất | Trạng thái |
|------------|--------------|------------|
| pfSense | Có interface (giao diện mạng) tới WAN / DMZ / LAN / SOC | Proposed |
| WAF Reverse Proxy | DMZ | Proposed |
| DVWA Backend | DMZ | Proposed |
| Active Directory Domain Controller | LAN | Proposed |
| Wazuh All-in-One | SOC | Proposed |

Vị trí đặt Wazuh All-in-One vẫn cần được đánh giá chi tiết về tài nguyên và kiến trúc trước khi chốt (xem mục 3.4).

### 3.4. Các quyết định chưa được chốt

Các quyết định dưới đây **không** được tự giải quyết. Mỗi mục chỉ được chốt bằng ADR ở trạng thái **Accepted**.

| Chủ đề | Trạng thái | Ghi chú |
|--------|------------|---------|
| Chế độ kết nối WAN trên VMware (VMware WAN networking mode) | **UNDECIDED** | NAT / Bridged / Host-only. Mức độ phơi nhiễm của từng chế độ phải được đánh giá trong ADR |
| Vị trí đặt cuối cùng của Wazuh All-in-One | **UNDECIDED** | Vùng SOC hiện chỉ là đề xuất; phương án local / cloud / hybrid chưa chốt |
| Cấu hình tài nguyên (sizing) cho Wazuh All-in-One | **TBD** | Phụ thuộc đánh giá tài nguyên thực tế |
| Có cần phân đoạn quản trị riêng (Management Segment) hay không | **UNDECIDED** | Chưa có phân đoạn quản trị riêng nào được phê duyệt. Không tạo subnet, kế hoạch địa chỉ IP hoặc cấu hình mạng quản trị riêng cho đến khi có ADR ở trạng thái Accepted |
| Nhà cung cấp cloud | **UNKNOWN** | Không giả định bất kỳ nhà cung cấp nào |
| Kiến trúc cloud trong tương lai | **TBD** | Ngoài phạm vi triển khai hiện tại |
| CIDR mạng cuối cùng cho từng phân đoạn | **TBD** | |
| Ánh xạ card mạng (NIC mapping) | **TBD** | |

Danh sách đầy đủ các quyết định còn mở của repository, bao gồm cả các mục không nêu ở đây, được theo dõi trong `CLAUDE.md`, mục 4.2.

### 3.5. Nguyên tắc bảo mật

| Nguyên tắc | Diễn giải |
|------------|-----------|
| Mặc định từ chối (Default Deny) | Mọi lưu lượng giữa các phân đoạn bị từ chối, trừ khi có rule tường minh được ghi lại |
| Đặc quyền tối thiểu (Least Privilege) | Mỗi người dùng, dịch vụ và luồng mạng chỉ có quyền tối thiểu cần thiết |
| Phân đoạn mạng (Network Segmentation) | Tách hạ tầng thành các vùng có ranh giới rõ ràng |
| Phòng thủ nhiều lớp (Defense in Depth) | Kết hợp nhiều lớp kiểm soát: tường lửa, WAF, định danh, giám sát |
| Ranh giới tin cậy tường minh (Explicit Trust Boundaries) | Ranh giới tin cậy giữa các vùng phải được xác định và ghi lại |
| Giám sát bảo mật tập trung (Centralized Security Monitoring) | Dữ liệu đo lường bảo mật (telemetry) được thu thập về SIEM |
| Xác minh dựa trên bằng chứng (Evidence-based Validation) | Kết quả chỉ được công nhận khi có bằng chứng đã làm sạch (sanitize) |
| Không phơi nhiễm giao diện quản trị | Giao diện quản trị nhạy cảm không được truy cập trực tiếp từ Internet |

**Không** để các thành phần sau truy cập trực tiếp từ Internet:
- Active Directory.
- Wazuh Dashboard.
- DVWA Backend.
- pfSense GUI.

Quy tắc bổ sung:
- Lưu lượng DMZ → LAN **mặc định bị từ chối**.
- Kết nối VPN **không** tự động thiết lập quan hệ tin cậy.

**Ranh giới truy cập DVWA Backend.**
- DVWA Backend **không** được công bố trực tiếp ra WAN hoặc Internet.
- Mọi yêu cầu tới DVWA Backend **phải** đi qua WAF Reverse Proxy.
- Truy cập tới WAF từ vùng WAN mô phỏng của lab phải tuân theo chính sách tường lửa và ADR đã được phê duyệt, bao gồm điều kiện mở lưu lượng WAN → DMZ dưới đây.
- Truy cập trực tiếp từ mạng bên ngoài thực tế (ngoài lab) tới DVWA Backend bị **cấm**.

Mục này không tạo rule tường lửa mới và không cho phép bất kỳ luồng lưu lượng WAN → DMZ nào.

**Điều kiện mở lưu lượng WAN → DMZ.** Lưu lượng WAN → DMZ mặc định bị từ chối và chỉ được cho phép khi đáp ứng **đồng thời cả hai** điều kiện:

1. Chế độ kết nối WAN trên VMware đã được chốt trong một ADR ở trạng thái **Accepted**; **và**
2. Luồng lưu lượng WAN → DMZ cụ thể đó đã được cho phép trong một ADR ở trạng thái **Accepted**.

Việc chốt chế độ kết nối WAN trên VMware **không** tự động cho phép lưu lượng WAN → DMZ. Thiếu bất kỳ điều kiện nào thì giữ nguyên trạng thái từ chối mặc định.

## 4. Bối cảnh và vấn đề cần giải quyết

Một môi trường hạ tầng SOC (Security Operations Center – trung tâm điều hành an ninh) cần nhiều hơn một nền tảng SIEM. Môi trường đó phải có hạ tầng tạo ra dữ liệu đo lường bảo mật (security telemetry), thực thi chính sách bảo mật, mô phỏng các dịch vụ doanh nghiệp và cung cấp dữ liệu cho giám sát bảo mật tập trung.

Một lab chỉ triển khai SIEM không thể thể hiện đầy đủ mối liên hệ giữa:

- Hạ tầng mạng (Network Infrastructure).
- Phân đoạn mạng (Network Segmentation).
- Thực thi bảo mật (Security Enforcement).
- Bảo mật ứng dụng web (Web Application Security).
- Hạ tầng định danh (Identity Infrastructure).
- Giám sát bảo mật (Security Monitoring).

Dự án giải quyết khoảng trống này bằng cách thiết kế và xây dựng một môi trường lab tích hợp, mô phỏng hạ tầng doanh nghiệp.

## 5. Mục đích dự án

Xây dựng một SOC Infrastructure Lab có tính thực tiễn, có thể kiểm thử và được tài liệu hóa đầy đủ, nhằm thể hiện mối liên hệ:

`Infrastructure → Network Security → Security Telemetry → SIEM`

Dự án ưu tiên:
- Tính đúng đắn của kiến trúc.
- Khả năng xác minh.
- Chất lượng tài liệu.
- Khả năng tái tạo (reproducibility).

## 6. Mục tiêu dự án

| ID | Mục tiêu | Mô tả |
|----|----------|-------|
| OBJ-01 | Infrastructure Architecture | Thiết kế và xây dựng hạ tầng mạng mô phỏng doanh nghiệp với các vùng WAN, DMZ, LAN và SOC |
| OBJ-02 | Network Security | Triển khai pfSense và thực hiện các chính sách bảo mật mạng đã được phê duyệt theo nguyên tắc Default Deny |
| OBJ-03 | Web Security | Triển khai WAF Reverse Proxy và DVWA Backend trong các container riêng biệt. Không công bố trực tiếp DVWA Backend ra bên ngoài |
| OBJ-04 | Identity Infrastructure | Triển khai một Domain Controller Windows Server Active Directory |
| OBJ-05 | Security Monitoring | Triển khai Wazuh All-in-One để thu thập và phân tích telemetry từ các hệ thống trong phạm vi |
| OBJ-06 | Security Validation | Kiểm thử mạng, tường lửa, WAF, Active Directory và Wazuh |
| OBJ-07 | Technical Documentation | Xây dựng tài liệu kiến trúc, cấu hình, thực hiện, kiểm thử, triển khai và vận hành |
| OBJ-08 | Future Extensibility | Tính đến khả năng mở rộng sang cloud và hybrid trong tương lai, nhưng không yêu cầu triển khai cloud trong phạm vi hiện tại |

**Ghi chú thuật ngữ.** Trong các nhãn OBJ-06 – Security Validation, M7 – Validation và nguyên tắc Evidence-based Validation, từ "Validation" được giữ nguyên làm nhãn đã được duyệt. Trong các nhãn này, "Validation" mang nghĩa kiểm thử và xác minh (verification) theo `CLAUDE.md` mục 6, không mang nghĩa kiểm tra tính hợp lệ (validation) theo bảng thuật ngữ.

## 7. Phạm vi dự án

### 7.1. Trong phạm vi

- Ảo hóa bằng VMware Workstation.
- Kiến trúc mạng và phân đoạn mạng.
- Tường lửa pfSense.
- Ubuntu Server.
- WAF Reverse Proxy.
- DVWA Backend.
- Windows Server Active Directory.
- Wazuh All-in-One.
- Thu thập telemetry bảo mật và log.
- Kiểm thử mạng và bảo mật.
- Tài liệu kỹ thuật.
- Bằng chứng kiểm thử đã được làm sạch.

### 7.2. Ngoài phạm vi

- Shuffle SOAR.
- TheHive.
- Tích hợp MISP và VirusTotal.
- Xây dựng AI Agent.
- Tự động hóa toàn bộ quy trình SOC.
- High Availability (tính sẵn sàng cao) cho production.
- Disaster Recovery (khôi phục sau thảm họa) cho production.
- Triển khai trên một nhà cung cấp cloud cụ thể.
- Dịch vụ production công khai ra Internet.

Các hạng mục này có thể được xem xét trong một giai đoạn mở rộng tương lai, với phê duyệt riêng.

### 7.3. Ranh giới phạm vi

Dự án xây dựng **hạ tầng SOC**, không xây dựng một SOC vận hành hoàn chỉnh.

Việc triển khai Wazuh **không** tự động có nghĩa là dự án đã thực hiện đầy đủ quy trình SOC, bố trí nhân sự SOC hay năng lực ứng phó sự cố (incident response) cho production.

## 8. Deliverable cấp cao

| ID | Deliverable |
|----|-------------|
| D-01 | Project Charter |
| D-02 | Requirements Specification |
| D-03 | Architecture Design |
| D-04 | Architecture Decision Records |
| D-05 | Infrastructure Configuration |
| D-06 | Firewall Policy and Rule Matrix |
| D-07 | WAF and DVWA Deployment |
| D-08 | Active Directory Deployment |
| D-09 | Wazuh Deployment |
| D-10 | Test Documentation |
| D-11 | Test Evidence |
| D-12 | Deployment Documentation |
| D-13 | Operations Documentation |
| D-14 | Final Project Report |

Ở các giai đoạn tiếp theo, mỗi deliverable phải truy vết được (traceable) tới yêu cầu và tiêu chí chấp nhận tương ứng.

## 9. Các bên liên quan và trách nhiệm

| Bên liên quan | Trách nhiệm |
|---------------|-------------|
| Project Owner | Phạm vi, kiến trúc, quyết định bảo mật, tài nguyên và chấp nhận kết quả |
| Infrastructure Engineer | Thực hiện VMware, mạng và tường lửa |
| Security Engineer | WAF, chính sách bảo mật và kiểm thử |
| SIEM Engineer | Wazuh, telemetry và kiểm thử SIEM |
| Reviewer / Supervisor | Rà soát và đánh giá deliverable của dự án |

Một người có thể đảm nhận nhiều vai trò kỹ thuật. Không mặc định mỗi vai trò cần một cá nhân riêng.

## 10. Ràng buộc của dự án

### 10.1. Ràng buộc hạ tầng

- Triển khai cục bộ trên VMware Workstation.
- CPU, RAM và dung lượng đĩa vật lý có giới hạn.
- Cấu hình tài nguyên máy ảo phải phù hợp với tài nguyên sẵn có.
- Phải kiểm soát dung lượng đĩa và thời gian lưu giữ log (log retention).

### 10.2. Ràng buộc phạm vi

- Không xây dựng SOAR hoặc AI đầy đủ.
- Không bắt buộc triển khai cloud.
- Không triển khai HA/DR ở cấp production.

### 10.3. Ràng buộc bảo mật

- Không bao giờ commit thông tin bí mật (secret) thật.
- Không commit log thô có thông tin nhạy cảm.
- Làm sạch bằng chứng trước khi lưu vào repository.
- Không để giao diện quản trị nhạy cảm hoặc backend truy cập trực tiếp từ Internet.

### 10.4. Ràng buộc tiến độ

- Không có hạn chót cố định.
- Lập kế hoạch theo giai đoạn (milestone).
- Tiến độ phụ thuộc vào chất lượng deliverable và các cổng chấp nhận (acceptance gate).

## 11. Giả định

Các giả định dưới đây **chưa được xác minh** và cần được kiểm tra ở các giai đoạn tiếp theo. Không được trình bày chúng như sự thật đã xác minh.

| ID | Giả định | Trạng thái |
|----|----------|------------|
| A-01 | VMware Workstation có thể hỗ trợ phân đoạn mạng như dự kiến | Chưa xác minh |
| A-02 | Các phiên bản phần mềm được chọn tương thích với tài nguyên sẵn có | Chưa xác minh |
| A-03 | Wazuh All-in-One có thể được cấu hình tài nguyên phù hợp với lab | Chưa xác minh |
| A-04 | WAF và DVWA có thể chạy trong các container riêng biệt trên một máy ảo Ubuntu | Chưa xác minh |
| A-05 | Windows Server có thể hoạt động như một Domain Controller độc lập | Chưa xác minh |
| A-06 | Có thể thu thập telemetry phù hợp từ các thành phần được chọn | Chưa xác minh |

## 12. Rủi ro cấp cao

| ID | Rủi ro | Biện pháp giảm thiểu ban đầu |
|----|--------|------------------------------|
| R-01 | Thiếu RAM và CPU | Tính toán tài nguyên và triển khai theo từng bước |
| R-02 | Cạn dung lượng đĩa do log và index | Chính sách lưu giữ dữ liệu và giám sát dung lượng đĩa |
| R-03 | Phân đoạn mạng sai | Rà soát kiến trúc và kiểm thử kết nối |
| R-04 | DVWA Backend bị truy cập trực tiếp | Kiểm tra mức độ phơi nhiễm và chính sách tường lửa |
| R-05 | Wazuh AIO quá tải tài nguyên | Tính toán tài nguyên và kiểm tra theo tải thực tế |
| R-06 | Mất cấu hình hoặc dữ liệu của lab | Chiến lược sao lưu (backup) và snapshot |
| R-07 | Phạm vi bị mở rộng ngoài kiểm soát (scope creep) | Kiểm soát phạm vi và phê duyệt thay đổi |
| R-08 | Tài liệu không khớp với thực tế đã thực hiện | Xác minh dựa trên bằng chứng |

Phân tích rủi ro chi tiết và kế hoạch quản lý rủi ro sẽ được thực hiện trong một hoạt động lập kế hoạch tiếp theo.

## 13. Tiêu chí thành công

Tài liệu này không đặt ngưỡng hiệu năng bằng số. Các ngưỡng định lượng (nếu cần) sẽ được xác định khi lập kế hoạch yêu cầu và kiểm thử.

### SC-01 – Architecture

- Thiết kế kiến trúc và thiết kế triển khai được tài liệu hóa.
- Có sơ đồ mạng (network topology), thiết kế máy ảo, ánh xạ card mạng (NIC mapping) và kế hoạch địa chỉ IP.
- Ranh giới tin cậy (trust boundary) được xác định.
- Các quyết định kiến trúc quan trọng được ghi trong ADR.

### SC-02 – Network Security

- Các vùng mạng được phân đoạn.
- Chính sách tường lửa tuân theo Default Deny.
- Các luồng lưu lượng được phép và bị từ chối đều được kiểm thử.
- Không còn truy cập liên vùng ngoài ý muốn.

### SC-03 – Web Security

- WAF Reverse Proxy hoạt động đúng.
- DVWA Backend chạy trong container riêng.
- Backend không bị truy cập trực tiếp từ WAN.
- Các kiểm thử bảo mật web liên quan có bằng chứng.

### SC-04 – Identity

- Domain Controller Active Directory hoạt động đúng.
- Các thao tác định danh cơ bản được kiểm thử.
- Telemetry liên quan được thu thập.

### SC-05 – SIEM

- Wazuh All-in-One hoạt động đúng.
- Log hoặc telemetry liên quan được tiếp nhận.
- Có thể tìm kiếm và phân tích sự kiện bảo mật.
- Việc sinh và thu thập cảnh báo (alert) có bằng chứng.

### SC-06 – Documentation and Evidence

- Tài liệu phản ánh đúng trạng thái thực hiện thực tế.
- Các cấu hình quan trọng có thể tái tạo.
- Kết quả kiểm thử và bằng chứng được ghi lại.
- Phân biệt rõ Proposed, Accepted, Implemented và Verified theo hai hệ trạng thái độc lập ở [mục 14.3](#143-định-nghĩa-trạng-thái).

### SC-07 – Resource Sustainability

- Lab hoạt động trong giới hạn tài nguyên đã xác định.
- Có quy trình khởi động, tắt và kiểm tra tình trạng (health check).
- Cách xử lý khi cạn tài nguyên được tài liệu hóa.

## 14. Quản trị dự án

### 14.1. Thẩm quyền quyết định

Project Owner phê duyệt:

- Thay đổi phạm vi.
- Quyết định kiến trúc.
- Ngoại lệ đối với chính sách bảo mật.
- Phân bổ tài nguyên.
- Chấp nhận milestone.
- Công khai các artifact (sản phẩm) của dự án.

### 14.2. Kiểm soát thay đổi

Mọi thay đổi ảnh hưởng tới phạm vi, kiến trúc, ranh giới bảo mật hoặc deliverable phải được ghi lại và đánh giá.

Các quyết định kiến trúc quan trọng bắt buộc có ADR, lưu tại `docs/03-architecture-design/ADR/`.

### 14.3. Định nghĩa trạng thái

Dự án dùng **hai hệ trạng thái độc lập**, nhất quán với `CLAUDE.md` (mục 4.3 và mục 6).

**a) Vòng đời ADR (ADR Lifecycle)**: áp dụng cho quyết định kiến trúc.

| Trạng thái ADR | Ý nghĩa |
|----------------|---------|
| Proposed | Quyết định đang được đề xuất, chưa được chấp thuận |
| Accepted | Quyết định đã được chủ sở hữu chấp thuận |
| Rejected | Đề xuất bị từ chối |
| Superseded | Quyết định đã được thay thế bởi ADR khác |

**b) Trạng thái thực hiện (Implementation Status)**: áp dụng cho thành phần, cấu hình và kết quả kiểm thử.

| Trạng thái | Ý nghĩa |
|------------|---------|
| Proposed | Được đề xuất trong thiết kế hoặc phạm vi, nhưng chưa được thực hiện hoặc cấu hình trong lab |
| Implemented | Đã thực hiện hoặc đã cấu hình trong lab; chưa mặc nhiên có nghĩa là đã kiểm thử thành công |
| Verified | Đã được xác minh bằng phương pháp phù hợp, có kết quả và bằng chứng tương ứng |

Quy tắc diễn giải:
- Hai hệ trạng thái **độc lập** với nhau. Không dùng Implemented hoặc Verified làm trạng thái ADR, và không dùng Accepted, Rejected hoặc Superseded làm trạng thái thực hiện.
- Một ADR có thể được **Accepted trước khi** thành phần tương ứng được thực hiện.
- **Accepted không có nghĩa là Implemented.** **Implemented không có nghĩa là Verified.**
- Chữ "Proposed" có ở cả hai hệ và phải hiểu theo ngữ cảnh: ADR Proposed là đề xuất quyết định chưa được chấp thuận; thành phần Proposed là hạng mục chưa được thực hiện trong lab.

### 14.4. Quy trình thực thi và rà soát

Dự án tuân theo mô hình Owner-directed, AI-assisted execution. Vai trò chi tiết và chu trình làm việc chuẩn được mô tả tại [mục 2](#2-mô-hình-quản-trị-thực-thi). Tóm tắt:

| Bên | Vai trò |
|-----|---------|
| Project Owner | Có thẩm quyền quyết định và chấp nhận cuối cùng |
| ChatGPT – Planning and Review Assistant | Nghiên cứu, phân tích, lập kế hoạch, soạn hướng dẫn và rà soát kết quả. Không tự cho phép thực thi |
| Claude CLI – Execution Agent | Thực hiện các nhiệm vụ repository và triển khai đã được cho phép tường minh; báo cáo thay đổi và kết quả kiểm tra; phải dừng lại khi gặp quyết định chưa được chốt hoặc tình huống bất thường |

**Quy tắc cho phép thực hiện**

- Phê duyệt một tài liệu **không** đồng nghĩa với cho phép triển khai hạ tầng.
- Phê duyệt một thao tác Git **không** đồng nghĩa với cho phép thao tác Git khác.
- Claude CLI không được mở rộng phạm vi khi chưa được phê duyệt.
- Khi repository ở trạng thái bất thường, phải dừng lại và báo cáo.
- Mọi thay đổi quan trọng phải truy vết được tới một yêu cầu hoặc quyết định đã được phê duyệt.

**Quy tắc chấp nhận**

Một nhiệm vụ **chưa** hoàn thành chỉ vì việc thực thi đã thành công. Nhiệm vụ chỉ được coi là hoàn thành khi:

- Deliverable mong đợi đã tồn tại.
- Tiêu chí chấp nhận được đáp ứng.
- Có bằng chứng kiểm tra, khi áp dụng.
- Các giới hạn và vấn đề chưa giải quyết được ghi lại.
- Đã có chấp nhận của Owner, khi cần.

## 15. Kế hoạch theo giai đoạn (milestone)

Dự án không có hạn chót cố định. Mỗi milestone chỉ kết thúc khi đáp ứng điều kiện hoàn thành (exit condition) tương ứng.

**Khởi tạo (Bootstrap) và M0.** Khởi tạo (Bootstrap) là hoạt động chuẩn bị repository và quy trình làm việc: cấu trúc thư mục, quy tắc quản lý và cơ chế thực thi. Bootstrap không nhất thiết là một milestone độc lập của dự án và không đồng nghĩa với việc M0 đã hoàn thành. Để tránh trùng nghĩa, M0 – Project Initiation được gọi bằng tiếng Việt là **khởi động dự án**, không gọi là "khởi tạo".

| Milestone | Điều kiện hoàn thành |
|-----------|----------------------|
| M0 – Project Initiation (khởi động dự án) | Project Charter được Owner phê duyệt và các điều kiện khởi động dự án ở [mục 15.1](#151-điều-kiện-khởi-động-dự-án-m0) được xác nhận |
| M1 – Planning Baseline | Các deliverable lập kế hoạch được rà soát và phê duyệt |
| M2 – Requirements Baseline | Yêu cầu và tiêu chí chấp nhận được phê duyệt |
| M3 – Architecture Baseline | Kiến trúc và các ADR cần thiết ở trạng thái Accepted |
| M4 – Infrastructure Foundation | Mạng VMware và pfSense được triển khai và kiểm thử |
| M5 – Security Infrastructure | WAF, DVWA và AD được triển khai và kiểm thử |
| M6 – SIEM Foundation | Wazuh được triển khai và tiếp nhận telemetry |
| M7 – Validation | Các test case được thực thi, có bằng chứng |
| M8 – Project Closure | Tài liệu và báo cáo cuối cùng được rà soát |

Tại thời điểm lập tài liệu này, **chưa** có milestone nào hoàn thành, kể cả M0 và M1.

### 15.1. Điều kiện khởi động dự án (M0)

M0 chỉ hoàn thành khi Owner xác nhận **toàn bộ** các điều kiện dưới đây. Bảng này chỉ định nghĩa điều kiện, không ghi nhận trạng thái hoàn thành.

| ID | Điều kiện | Ghi chú |
|----|-----------|---------|
| IC-01 | Repository được khởi tạo, có commit đầu tiên và được đồng bộ lên remote đã được Owner phê duyệt | |
| IC-02 | `CLAUDE.md`, `SECURITY.md` và `README.md` được Owner chấp nhận làm quy tắc nền của dự án | |
| IC-03 | Mô hình quản trị và chu trình làm việc chuẩn ở [mục 2](#2-mô-hình-quản-trị-thực-thi) được Owner xác nhận | |
| IC-04 | Danh sách quyết định còn mở được ghi nhận đầy đủ | [Mục 3.4](#34-các-quyết-định-chưa-được-chốt) và `CLAUDE.md` mục 4.2 |
| IC-05 | Project Charter được Owner phê duyệt cuối cùng (phiên bản 1.0) và được commit vào repository | Điều kiện chốt của M0 |
| IC-06 | Owner xác nhận chế độ hiển thị (visibility) của repository: public hoặc private | Theo [mục 14.1](#141-thẩm-quyền-quyết-định), công khai artifact phải được Owner phê duyệt |

Giấy phép (license) **không** là điều kiện của M0. License tiếp tục ở trạng thái **UNDECIDED** và được theo dõi như một quyết định còn mở trong `CLAUDE.md` mục 4.2.

## 16. Phê duyệt

| Vai trò | Họ tên | Trạng thái |
|---------|--------|------------|
| Project Owner | Doãn Hữu Nguyên | Đã phê duyệt định hướng, phạm vi và tiêu chí thành công; phê duyệt cuối cùng cho tài liệu đang chờ |
| Reviewer / Supervisor | TBD | Pending |

Việc phê duyệt Project Charter xác lập định hướng dự án và cho phép các hoạt động lập kế hoạch. Phê duyệt này **không** tự động chốt các quyết định kiến trúc còn mở và **không** cho phép triển khai hạ tầng.

## 17. Metadata tài liệu

| Thuộc tính | Giá trị |
|------------|---------|
| Project ID | ESIL-001 |
| Tài liệu | Project Charter |
| Phiên bản | 0.3 |
| Trạng thái | Draft – Pending Final Owner Approval |
| Đường dẫn | `docs/01-planning/project-charter.md` |
| Cách tiếp cận lập kế hoạch | Quality-driven, milestone-based |
| Ngày hoàn thành mục tiêu | Không cố định |

**Quy ước phiên bản.** `0.x` là bản nháp đang rà soát. `1.0` là bản được Owner phê duyệt cuối cùng. Thay đổi sau bản 1.0 phải qua kiểm soát thay đổi ([mục 14.2](#142-kiểm-soát-thay-đổi)).

**Lịch sử thay đổi**

| Phiên bản | Ngày | Nội dung |
|-----------|------|----------|
| 0.2 | 2026-11-23 | Bản nháp đầu tiên, tạo từ đặc tả nguồn của Owner |
| 0.3 | 2026-11-23 | Sửa theo kết quả rà soát C-01 → C-06: tách quyết định kiến trúc, trạng thái thực hiện và trạng thái xác minh; tách hai hệ trạng thái ở mục 14.3; làm rõ ranh giới truy cập DVWA Backend; làm rõ trạng thái Management Segment; bỏ tham chiếu WS6; phân biệt Bootstrap và M0. Bổ sung điều kiện khởi động dự án (mục 15.1), ghi chú thuật ngữ "Validation", quy ước phiên bản và lịch sử thay đổi |
