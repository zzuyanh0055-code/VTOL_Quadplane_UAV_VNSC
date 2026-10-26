# Nhiễu roll tự động theo vị trí

[Hướng dẫn mô phỏng](../../simulation/README.md) · [English configuration reference](README.md)

Tài liệu này mô tả mã hiện tại trong [LocationTriggeredWrench.cc](src/LocationTriggeredWrench.cc) và cấu hình của [model chính](../../simulation/models/VTOL_Quadplane/model.sdf).

## Cấu hình hiện tại

| Thông số | Giá trị |
|---|---|
| Link chịu mô-men | `base_link` |
| x trong hệ world Gazebo | 371.5–416.5 m |
| y trong hệ world Gazebo | 925–965 m |
| z trong hệ world Gazebo | 35–70 m |
| Mô-men quanh trục +X thân | 1 N·m |

Model chính ghi đè mô-men mặc định 0.60 N·m của mã nguồn thành 1 N·m. Kiểm tra block plugin của đúng SDF được nạp trước mỗi ca chạy.

## Cơ chế kích hoạt

- Khi khởi tạo model, plugin chuyển sang trạng thái chờ và in `Armed`.
- Lần đầu link đi vào vùng, plugin in `ON` và bắt đầu tác động mô-men.
- Mô-men tiếp tục được áp dụng khi link còn trong vùng.
- Khi link ra khỏi vùng, plugin in `OFF`, ghi thời gian tác động và khóa trigger.
- Khởi động lại mô phỏng để kích hoạt được lần tiếp theo.

Mã hiện tại **không đọc tham số `duration_s`**. Mô tả cũ “0.08 N·m trong 0.50 s” không còn tương ứng với cấu hình này. Mô-men được biến đổi từ trục thân sang hệ world ở từng bước, nên hướng tác động đi theo trục roll của máy bay.

## Biên dịch và chạy

Chạy từ thư mục gốc repo:

```bash
cmake -S plugins/location_triggered_disturbance -B plugins/location_triggered_disturbance/build
cmake --build plugins/location_triggered_disturbance/build -j"$(nproc)"
```

Đặt đường dẫn plugin/resource và khởi động Gazebo theo [simulation/README.md](../../simulation/README.md). Model chính đã có block plugin; không cần chép đè bằng file SDF ví dụ để bật plugin.

## So sánh PID–INDI

- `RLL_INDI_EN=0`: PID.
- `RLL_INDI_EN=1`: PID điều khiển, INDI chỉ tính và ghi log.
- `RLL_INDI_EN=2`: INDI điều khiển trục roll fixed-wing.

Giữ cùng mission, model, tham số, vùng và mô-men giữa hai lượt. Khởi động lại Gazebo giữa các lượt. Nếu không thấy `Armed`, hoặc máy bay không đi qua vùng nên không có `ON`, không coi đó là ca thử nhiễu thành công.

Lưu cả thời gian chịu nhiễu thực tế: hai bộ điều khiển có thể ra khỏi vùng ở thời điểm khác nhau. Khi đánh giá, ghi rõ cửa sổ thời gian và định nghĩa metric cho sai số roll, roll rate, cross-track, altitude và control output.

[Flight_Test.plan](Flight_Test.plan) hiện chứa VTOL takeoff và hai waypoint tiếp theo. Bộ mission nghiên cứu đầy đủ cần bổ sung theo [mission archive](../../simulation/missions/README.md).
