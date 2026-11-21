# Khai báo dùng AI — Trần Tuấn Anh · G10

Bài làm cá nhân, sau đó review chéo với một bạn cùng mã G10. Không đưa ảnh cabin, ảnh tài xế hay ảnh chụp màn hình cho AI. Mọi ca khó chỉ mô tả bằng chữ kèm tên ảnh. Nhãn trên CVAT do mình tự gán.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude (Claude Code) | Tóm tắt đề Day 12, các bước Task 0, cách chặn `G10.zip` khỏi git | Tự chạy lệnh giải nén, đếm ảnh (16 + 6), chạy `cabin_check --help`; thêm `G10.zip` vào `.git/info/exclude` | Có: `git status --short` không còn file nào lạ |
| 2 | Claude | Cách tạo project/task CVAT với 22 ảnh cốt lõi, cách export và lệnh checkpoint | Tự tạo project `Day12` (9 nhãn), task `Day12-G10` 22 ảnh; tự export và chạy checkpoint | Có: CVAT hiện 22 frame; `cabin_check` đọc được file |
| 3 | Claude | Ca khó G10 S01: ghế rear_right có vật phủ vải caro, gán thế nào (mô tả bằng chữ) | Gán `child_seat`, `seat = rear_right`, `occupied = unknown` | Có: đối chiếu GUIDE 4.4-1 ("ghế trẻ em bị phủ: unknown") và 4.8 |
| 4 | Claude | Đọc `annotations.xml` (chỉ tọa độ và thuộc tính) để soát lỗi mà `cabin_check` không bắt | Tự sửa trên CVAT: thêm `face13` cho D##, xóa 8 box `hand` trên S01/S04, xóa box cốc ở D06 | Có: đối chiếu GUIDE 4.1, 4.2, 4.3; chạy lại `cabin_check` 0 lỗi |
| 5 | Claude | Cách vẽ `face13` khi ảnh xoay 90°, mặt nghiêng hoặc bị che; khi nào dùng `occluded`/`outside`; `face` có vẽ trên ảnh IR không | Bỏ `face`/`face13` ở D01, D03 (không thấy mắt lẫn mũi); điểm đoán chắc = `occluded`, không đoán được = `outside` | Có: GUIDE 4.5, 4.6; tự xem lại từng khung trên CVAT |
| 6 | Claude | Kiểm tọa độ điểm `face13` (mắt trái có y lớn hơn, mí trên/dưới) | Giữ D05 sau khi xem lại; ghi vào tự đánh giá | Có: trái/phải đúng ở cả 4 mặt theo trục y |
| 7 | Claude | S04: nhãn ghi `unknown` nhưng mình thấy rõ không thắt dây, không đeo kính | Sửa S04_rgb và S04_ir thành `seatbelt = not_worn`, `glasses = none` | Có: GUIDE 4.5, 4.7; đọc lại XML sau khi export |
| 8 | Claude | Soạn ghi chú cuối `check_final.txt` (lý do bỏ mặt, lý do `gaze_zone = other`, số ghế trống) | Dùng câu chữ, lý do dựa trên nhãn và quan sát của mình | Có: tự đọc lại; số ghế trống đếm từ nhãn của mình |
| 9 | Claude | Soạn `ir_log.csv` từ 4 quan sát mình kể bằng lời (S01_ir, S04_ir, D02, D06) | Lưu file sau khi đọc lại | Có: tách `IR_sim` / `real_IR`; mỗi dòng có khung, hiện tượng, trường nhãn |
| 10 | Claude | Danh sách lớp ứng viên cho DMS/OMS và câu hỏi leo thang | Mình tự chọn 7 lớp DMS, 7 lớp OMS và 4 câu hỏi; Claude soạn câu chữ; mình đọc lại và chỉnh | Có: đối chiếu slide 54, GUIDE 4.1 và các khung đã gán |
| 11 | Claude | Soạn bản khai báo `prompts.md` này | Đọc lại, sửa cho đúng những gì đã làm | Có: đối chiếu từng lần hỏi trong buổi |
| 12 | Claude | Lập playbook và thư mục ghi chú lệnh cá nhân (không nộp) | Dùng để theo dõi các bước và lệnh | Không áp dụng (tài liệu cá nhân) |
| 13 | Claude | Soạn `self_review.md` từ các lỗi đã sửa trong bài | Mình tự chọn lỗi lặp lại (gán nhãn sai phần); Claude soạn câu chữ; mình đọc lại | Có: đối chiếu XML trước/sau khi sửa và GUIDE 4.1, 4.2 |
