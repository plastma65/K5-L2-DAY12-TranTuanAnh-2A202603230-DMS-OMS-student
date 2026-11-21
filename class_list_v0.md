# Danh sách lớp v0 — Trần Tuấn Anh · G10

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_state` (từng mắt) | open / closed / unknown | Phát hiện buồn ngủ: mắt nhắm kéo dài qua nhiều khung (PERCLOS) thì cảnh báo | Có (D02, D06 thấy rõ mí khép); `unknown` khi kính phản xạ LED hoặc mặt quay đi |
| 2 | `gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Cảnh báo mất tập trung: nhìn khỏi đường (lap, center) quá lâu | Có khi mắt mở; mắt nhắm hoặc khuất thì không xác định được (D02, D05, D06 → other) |
| 3 | `face13` (13 điểm mặt) | tọa độ + cờ occluded / outside | Đo độ mở mắt, phát hiện ngáp, ước lượng hướng đầu cho eye_state và gaze | Có; điểm bị tay, ly che phải có cờ (D06 5 điểm mũi-miệng occluded) |
| 4 | `phone` | held / mounted / lap | Cảnh báo dùng điện thoại khi lái | Có theo hình dạng (D04 held); màn hình có thể không sáng trên NIR |
| 5 | `hand` + `on_wheel` | whose, side, on_wheel yes / no / unknown | Kiểm tra tài xế sẵn sàng giành lại lái (takeover) khi xe tự lái | Có; `on_wheel = no` là bình thường khi xe tự lái (Drive&Act) |
| 6 | `glasses` | none / clear / sun / unknown | Biết khi nào kết quả eye_state kém tin cậy (kính phản xạ, kính râm) | Một phần: kính râm có thể trông trong suốt trên NIR, dễ ghi nhầm |
| 7 | `seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc tài xế thắt dây, phát hiện thắt sai (dưới nách, sau lưng) | Một phần: dây cùng tông với áo tối khó thấy → unknown |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Đếm người theo ghế (kể cả trẻ ngồi trong ghế trẻ em/ghế em bé), nhắc thắt dây theo ghế, điều chỉnh túi khí | Có (hình dạng); trẻ trong ghế ở S02, S03, S05, S07 |
| 2 | `child_seat` + `occupied` | seat, occupied no / unknown / yes (BTC thêm `yes` ngày 21/11) | Nhắc quên trẻ trong xe; điều chỉnh túi khí ghế có ghế trẻ em | Có theo hình dạng: có trẻ ngồi thì yes, kèm box person + face cho trẻ (S02, S03 rear_middle, coach xác nhận); thấy rõ ghế trống thì no (S01, S08) |
| 3 | `infant_seat` + `facing` | seat, facing rear / front | Tắt túi khí ghế trước khi có ghế em bé quay ngược chiều | Có (hình dạng nôi, quai xách) |
| 4 | `seatbelt` (ghế sau) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây cho hành khách hàng sau | Một phần: áo tối khó thấy; SVIRO không ai thắt dây nên chưa dạy được worn_correct |
| 5 | `seat_occupancy` (từng ghế) | trống / người / ghế trẻ em / ghế em bé / đồ vật | Quyết định túi khí và nhắc dây theo từng ghế; đếm ghế trống | Có |
| 6 | `face` (OMS, chỉ box) | box | Đếm người, phân biệt người lớn với trẻ em, gắn tính năng theo từng người | Có nếu không bị che (S01, S04; mặt trẻ ở S02, S05, S07) |
| 7 | `object` + `type` | bag / box / cup / other | Nhắc quên đồ khi rời xe | Chỉ theo hình dạng, mất màu (S01 cup, S03 bag, S05 cup) |

## Câu hỏi leo thang

1. Câu hỏi: Khi tài xế nhắm cả hai mắt, `gaze_zone` nên là `other` hay cần thêm giá trị riêng (`unknown` / `eyes_closed`)?
   Khung ví dụ: G10 D02.
   Bạn tạm gán thế nào và vì sao: `gaze_zone = other` kèm lý do trong ghi chú, vì GUIDE 4.5 dùng `other` cho trường hợp không chắc. Nhưng `other` đang gộp chung "nhìn chỗ khác" với "không xác định được", nên model học lẫn hai nghĩa.

2. Câu hỏi: DMS có cần lớp hành vi ăn uống (ly, chai trên tay) không? Hiện GUIDE 4.2 cấm vẽ `object` trong DMS.
   Khung ví dụ: G10 D06 (tài xế đang uống nước).
   Bạn tạm gán thế nào và vì sao: không vẽ ly; 5 điểm mũi-miệng bị ly che đặt `outside` vì không đoán được vị trí. Hành vi uống nước là nguồn mất tập trung nhưng không có trường nào ghi lại.

3. Câu hỏi: Coach xác nhận trẻ ngồi trong ghế thì vẽ `person` + `face`. Nhưng `infant_seat` không có thuộc tính `occupied`, và `person` không phân biệt người lớn với trẻ em. Có cần thêm `infant_seat.occupied` và thuộc tính `age_group` (adult/child) cho `person` không?
   Khung ví dụ: G10 S05, S07 (trẻ trong infant_seat rear_right), G10 S02, S03 (trẻ trong child_seat rear_middle).
   Bạn tạm gán thế nào và vì sao: vẽ `person` + `face` cho trẻ theo xác nhận của coach; `child_seat` ghi `occupied = yes`; `infant_seat` không ghi được là có trẻ. GUIDE 3 vẫn ghi "bộ ảnh không có trẻ em", cần cập nhật guideline.

4. Câu hỏi: Mặt nghiêng tới mức nào thì bỏ box `face` và `face13`? Có nên thêm `head_pose` (yaw/pitch)?
   Khung ví dụ: G10 D01, D03 (có box face nhưng không vẽ face13), G10 D05 (vẽ, mắt phải `outside`).
   Bạn tạm gán thế nào và vì sao: sau review chéo, D01 và D03 có box `face` nhưng không vẽ `face13` vì không thấy rõ mắt, mũi, miệng; D05 thấy một mắt và mũi nên vẽ `face13`, mắt xa đặt `outside`. GUIDE 4.5 (bỏ face khi không thấy mắt lẫn mũi) và 4.6 (mặt ≥ 60 px thì vẽ face13) chưa nói ca "có mặt nhưng không đặt được điểm".
