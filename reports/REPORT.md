# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Lâm Bách   Nhóm: ______   Ngày: 16/09/2026

> Số liệu trong báo cáo được lấy từ `reports/visibility_report.md`,
> `outputs/visibility_report.json` và `outputs/eval_vs_gold.json`. Các mục chưa có nguồn dữ
> liệu được ghi rõ là **chưa có dữ liệu**, không tự ước lượng.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| Số keypoint có `v > 0` trung bình mỗi người | 15.93 |
| `v=2` / `v=1` / `v=0` | 309 / 153 / 31 |
| Thời gian trung bình mỗi ảnh | 5 phút 15 giây |

Ba khớp có `%v=1` cao nhất:

1. `left_ear`: 21/29, tương đương 72%.
2. `right_ear`: 15/29, tương đương 52%.
3. `left_hip`: 13/29, tương đương 45%.

Đây cũng là các vị trí dễ gây phân vân. Tai thường bị tóc hoặc mũ bảo hiểm che; hông có
thể bị áo dài, tư thế cơ thể hoặc vật phía trước che. Bằng chứng định lượng là cả ba có tỷ lệ
`v=1` cao nhất, đồng thời đánh giá với gold ghi nhận nhiều bất đồng cờ ở tai và hông.

## 2. Chấm với gold

Hiện chỉ có một lần đánh giá trong `outputs/eval_vs_gold.json`; kết quả này được xem là
**trước rework** vì vẫn còn các lỗi cần sửa. Cần chạy lại công cụ sau khi sửa nhãn để điền cột
“Sau rework”.

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| Số người gold / ghép đúng / thiếu / thừa | 29 / 29 / 0 / 0 | Chưa chạy lại |
| OKS trung bình | 0.9495 | Chưa chạy lại |
| OKS@0.50 | 1.0000 | Chưa chạy lại |
| OKS@0.75 | 1.0000 | Chưa chạy lại |
| Lỗi `dao_trai_phai` | 0 | Chưa chạy lại |
| Lỗi `nham_nguoi` | 1 | Chưa chạy lại |
| Lỗi `xoa_khop_bi_che` | 2 | Chưa chạy lại |
| Lỗi `lech_nhe` | 3 | Chưa chạy lại |
| Bất đồng cờ `v=1`/`v=2` với gold | 58 | Chưa chạy lại |
| Khớp gold có `v=0` (không tính OKS) | 72 | Chưa chạy lại |

**Các nhãn cần rework theo kết quả hiện tại:**

- `train_04.jpg`, người của bài số 1, `left_wrist`: kiểm tra và chuyển điểm đang gần cổ tay
  của người khác về đúng người.
- `train_12.jpg`, người số 1, `left_ankle` và `right_ankle`: đổi từ `v=0` sang `v=1`, đặt
  điểm ước lượng vì hai cổ chân bị xe/thùng carton che nhưng vẫn nằm trong khung.
- `train_02.jpg`, người số 1, `left_knee` và `right_knee`: chỉnh vị trí; mỗi điểm lệch 35 px,
  bằng 1.3 lần bán kính dung sai.
- `train_13.jpg`, bài người số 2 (ghép với gold người số 1), `right_wrist`: chỉnh vị trí lệch
  17 px, bằng 1.4 lần bán kính dung sai.

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh theo lần đánh giá hiện tại.

Ảnh có OKS theo người thấp nhất là `train_12.jpg` (0.8371), tiếp theo là người số 1 trong
`train_04.jpg` (0.8518). Hai lỗi xóa khớp bị che tại `train_12.jpg` có tác động trực tiếp đến
OKS; các bất đồng cờ `v=1`/`v=2` không trực tiếp làm giảm OKS trong báo cáo này.

## 3. Kiểm chéo

Bạn cùng nhóm: **Chưa có thông tin**

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| Chưa có visibility report của bạn cùng nhóm | — | — | — | Chưa thể kết luận |

Chưa thể xác định khớp lệch `%v=1` nhiều nhất giữa hai người vì chưa có báo cáo của bạn cùng
nhóm. Rule nội bộ đã bổ sung vào `GUIDELINE_MINI.md`: khớp bị vật che nhưng vị trí giải phẫu
ước lượng vẫn nằm trong ảnh phải dùng `v=1`; chỉ dùng `v=0` khi vị trí khớp nằm ngoài biên ảnh.

## 4. Model

năm câu hỏi về model mà không suy đoán.

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | Chưa có | Chưa có | Chưa có |
| pose_mAP50-95 | Chưa có | Chưa có | Chưa có |
| pose_precision | Chưa có | Chưa có | Chưa có |
| pose_recall | Chưa có | Chưa có | Chưa có |
| box_mAP50-95 | Chưa có | Chưa có | Chưa có |

### Năm câu hỏi cuối notebook

1. Chưa trả lời: cần `outputs/eval_model.json` để tính thay đổi `pose_mAP50-95`.
2. Chưa trả lời: cần `box_mAP` và `pose_mAP` của cùng lần đánh giá.
3. Chưa trả lời: cần kết quả dự đoán/ảnh trực quan của model.
4. Chưa trả lời: cần OKS giữa nhãn và model theo từng ảnh.
5. Chưa trả lời: cần đối chiếu ảnh có OKS gold thấp nhất với ảnh model dự đoán tệ nhất.

## 5. Một rule evidence đã dùng

Ở `train_12.jpg`, người số 1, cả `left_ankle` và `right_ankle` bị xe máy và các thùng carton
che. Dựa vào cẳng chân, bàn chân và tư thế ngồi, vị trí hai cổ chân vẫn có thể được ước lượng
và chắc chắn còn nằm trong khung ảnh. Vì vậy hai điểm phải dùng `v=1` và được đặt tại vị trí
ước lượng; dùng `v=0` sẽ xóa hai khớp bị che khỏi bài và làm chúng nhận 0 điểm khi gold có
`v=1`.

## 6. Việc còn thiếu để hoàn tất báo cáo

- Sửa các nhãn liệt kê ở mục 2, chạy lại đánh giá, rồi điền cột “Sau rework”.
- Cung cấp visibility report của bạn cùng nhóm để hoàn tất mục kiểm chéo.
- Chạy đánh giá model để tạo `outputs/eval_model.json` và hoàn tất mục 4.
