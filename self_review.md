# Tự đánh giá — Trần Tuấn Anh · G10

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_rgb, S01_ir | `hand` ×2 mỗi ảnh; `child_seat` rear_right | `extra`; `guess` | Vẽ tay hành khách trên ảnh SVIRO, nhưng `hand` chỉ dùng cho DMS (GUIDE 4.1, 4.3). Ghế trẻ em bị vải caro phủ mà ghi `occupied = no`, phải là `unknown` (4.4-1) | Có: xóa 4 box `hand`, đổi `occupied = unknown` |
| S04_rgb, S04_ir | `hand` ×2 mỗi ảnh; `person.seatbelt`, `face.glasses` | `extra`; `ok` sau khi sửa | Lại vẽ `hand` trên SVIRO. `seatbelt` và `glasses` ghi `unknown` dù thấy rõ không thắt dây, không đeo kính, phải ghi `not_worn` / `none` (4.5, 4.7) | Có: xóa 4 box `hand`, đổi `not_worn` / `none` |
| D05 | `face13` mắt trái | `box` (vị trí điểm) | Khung 13 điểm CVAT đặt dựng đứng; 4 điểm mắt trái còn theo kiểu mặt nhìn thẳng (mí trên/dưới lệch theo y) trên ảnh xoay 90°. Trái/phải đúng: mắt trái có y lớn hơn | Chưa: xem lại trên ảnh nhưng không chắc vị trí mép mí, giữ nguyên để không đoán (4.6) |
| D06 | `object` (ly nước) | `extra` | Vẽ ly tài xế đang uống thành `object`, nhưng DMS không vẽ chai, cốc trên tay (4.2). Điểm mũi-miệng bị ly che đã có cờ `occluded` | Có: xóa box `object` |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Gán nhãn không thuộc phần đang làm. Vẽ `hand` trên ảnh OMS (S01, S04: 8 box) và vẽ `object` trên ảnh DMS (D06). Lặp ở 3 cảnh.

Vì sao mình mắc: Mình gán theo những gì nhìn thấy trong ảnh (thấy tay thì vẽ tay, thấy ly thì vẽ ly) mà không tra bảng GUIDE 4.1 xem nhãn đó dùng cho OMS hay DMS. `cabin_check` không báo lỗi này nên lần kiểm đầu vẫn ra 0 lỗi.

Lần sau tránh bằng cách: Trước khi vẽ mỗi box, hỏi "nhãn này có trong cột *Dùng ở* của phần đang làm không?" (OMS: person, face, child_seat, infant_seat, object; DMS: person, face, face13, hand, phone). Sau khi export, đếm số đối tượng theo từng phần thay vì chỉ nhìn dòng "0 lỗi".

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: (điền sau review chéo)
