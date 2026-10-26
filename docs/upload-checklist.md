# Checklist hoàn thiện repo QuadPlane UAV

Repo đã có mã mô phỏng, bản vá INDI và plugin gây nhiễu. Các thư mục mới hiện là nơi hướng dẫn bổ sung dữ liệu, chưa có CAD gốc, OpenVSP, ảnh máy bay thật hay bộ kết quả.

## 1. Bổ sung trước để người đọc hiểu ngay dự án

- [ ] **Ảnh máy bay thật:** đưa 1 ảnh tổng thể rõ nét và một số ảnh lắp ráp vào [media/photos](../media/photos/README.md). Ghi phiên bản máy bay và chú thích.
- [ ] **SolidWorks:** dùng Pack and Go để gom assembly, toàn bộ part/subassembly và drawing vào [cad/solidworks](../cad/solidworks/README.md).
- [ ] **STEP:** xuất assembly hoàn chỉnh vào [cad/exports](../cad/exports/README.md) để người không có SolidWorks vẫn xem được.
- [ ] **OpenVSP:** đưa file `.vsp3` đúng phiên bản đã phân tích vào [aerodynamics/openvsp](../aerodynamics/openvsp/README.md).
- [ ] **Kết quả nổi bật:** đưa các biểu đồ có dữ liệu nguồn và chú thích vào [results](../results/README.md).
- [ ] Sau khi có ảnh thật, thêm ảnh đại diện vào đầu README. Không dùng ảnh render thay cho ảnh chế tạo.

## 2. Hoàn thiện nội dung kỹ thuật

- [ ] **VSPAERO:** file cấu hình ca chạy, điều kiện phân tích, CL/CD/Cm, polar và dữ liệu CSV vào [aerodynamics/vspaero](../aerodynamics/vspaero/README.md).
- [ ] **Thông số máy bay:** chốt phiên bản cuối, kích thước đo thực tế, khối lượng, CG và inertia. Phân biệt sải cánh thực với bref dùng trong phân tích khí động.
- [ ] **Hardware:** danh sách linh kiện, sơ đồ điện, vật liệu, quy trình in/lắp ráp vào [hardware](../hardware/README.md).
- [ ] **Mô hình động lực học:** phương trình, giả thiết, hệ tọa độ và nguồn tham số trong [modeling.md](modeling.md).
- [ ] **Mission nghiên cứu:** bổ sung đúng file `.plan` cho từng case vào [simulation/missions](../simulation/missions/README.md). File Flight_Test.plan hiện có chỉ chứa VTOL takeoff và 2 waypoint tiếp theo; không phải bộ mission đầy đủ.
- [ ] **So sánh PID–INDI:** lưu mission, parameter snapshot, cấu hình model/world, log, script xử lý và định nghĩa metric cho từng cặp chạy.
- [ ] **Thử nghiệm thực:** ghi rõ mục tiêu, cấu hình, kết quả quan sát và giới hạn. Tách khỏi kết quả SITL.
- [ ] **Bài báo/luận văn:** bổ sung tác giả, tên chính xác, venue, trạng thái và bản được phép công khai vào [docs/publications](publications/README.md).
- [ ] **Đóng góp nhóm:** xác nhận tên và vai trò từng thành viên trong [attribution.md](attribution.md).

## 3. Quy ước lưu trữ

- Dùng tên rõ ràng, ví dụ `quadplane_v3_assembly.step` hoặc `case04_indi_cross_track.csv`; đây là ví dụ đặt tên, không phải file đã có.
- Lưu riêng V1/V2/V3 nếu cần trình bày quá trình phát triển; xác định bản nào tương ứng với model mô phỏng.
- Mỗi bộ dữ liệu cần ghi đơn vị, nguồn, phiên bản và điều kiện tạo dữ liệu.
- Không đưa thư mục build, cache, file tạm CAD hoặc toàn bộ log chạy hằng ngày vào repo.
- Với CAD/video/log lớn, chọn Git LFS hoặc nơi tải riêng trước khi upload; nếu dùng Git LFS cần cấu hình và kiểm tra trên máy upload. Chưa có thay đổi LFS tự động trong lần tổ chức này.
- Đối với dữ liệu tải riêng, thêm link ổn định, dung lượng và checksum vào README của phần tương ứng.

## 4. Những điểm đã phát hiện khi rà soát

- README cũ chỉ giới thiệu simulation và chưa liệt kê phần controller.
- Hướng dẫn plugin cũ mô tả xung 0.50 s / 0.08 N·m, trong khi mã hiện tại tác động đến khi ra khỏi vùng; model chính đặt 1 N·m. Tài liệu plugin đã được cập nhật theo mã.
- Repo có nhiều model/world ví dụ và asset trùng giữa các thư mục. Một số asset tham chiếu vẫn được model chính dùng, vì vậy chưa xóa hoặc chuyển đường dẫn runtime.
- Có mesh lớn, ví dụ My_Base_link.dae khoảng 41.2 MB; cần cân nhắc dung lượng khi bổ sung CAD.
- Chưa có bộ dữ liệu kết quả đủ để kiểm chứng độc lập các mức cải thiện PID–INDI.
- Cần chạy lại toàn bộ quy trình trên môi trường Ubuntu/Gazebo của dự án để xác nhận khả năng tái lập; lần tổ chức repo này kiểm tra tài liệu và đường dẫn, không thực hiện chuyến bay mô phỏng.
