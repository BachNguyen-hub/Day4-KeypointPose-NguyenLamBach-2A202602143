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

Kết quả sau rework giữ nguyên các chỉ số của lần đánh giá trước, riêng hai lỗi
`xoa_khop_bi_che` đã được sửa nên giảm từ 2 xuống 0.

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| Số người gold / ghép đúng / thiếu / thừa | 29 / 29 / 0 / 0 | 29 / 29 / 0 / 0 |
| OKS trung bình | 0.9495 | 0.9495 |
| OKS@0.50 | 1.0000 | 1.0000 |
| OKS@0.75 | 1.0000 | 1.0000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 2 | 0 |
| Lỗi `lech_nhe` | 3 | 3 |
| Bất đồng cờ `v=1`/`v=2` với gold | 58 | 58 |
| Khớp gold có `v=0` (không tính OKS) | 72 | 72 |

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

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Năm câu hỏi cuối notebook

1. `pose_mAP50-95` tăng từ 0.6853 lên 0.6908, tức tăng 0.0055 (0.55 điểm phần trăm).
   Mức tăng nhỏ cho thấy fine-tune cải thiện nhẹ độ chính xác định vị keypoint trên tập test;
   `pose_mAP50` và recall không đổi nên chưa có bằng chứng model phát hiện thêm nhiều pose.

2. Trước fine-tune, `box_mAP50-95` cao hơn `pose_mAP50-95` 0.1266; sau fine-tune, chênh
   lệch là 0.1133. Model tìm và định vị hộp người dễ hơn định vị chính xác 17 khớp, vì box chỉ
   cần bao phủ người còn pose phải đặt đúng nhiều điểm, kể cả các điểm nhỏ hoặc bị che. Sau
   fine-tune, khoảng cách này thu hẹp 0.0133 do pose tăng nhẹ trong khi box giảm nhẹ.

3. Chưa thể gọi tên một ảnh test model đoán sai: `eval_model.json` không chứa dự đoán hoặc
   kết quả theo từng ảnh. Cần ảnh trực quan prediction để phân loại lệch nhẹ, đảo trái/phải,
   nhầm người hay trượt hẳn.

4. Chưa thể xác định ảnh có OKS thấp nhất giữa nhãn và model vì file chỉ có metric tổng hợp,
   không có OKS theo ảnh.

5. Ảnh gán có OKS với gold thấp nhất là `train_12.jpg` (0.8371), nhưng chưa thể biết đây có
   đồng thời là ảnh model đoán tệ nhất hay không vì thiếu kết quả model theo ảnh.

## 5. Một rule evidence đã dùng

Ở `train_12.jpg`, người số 1, cả `left_ankle` và `right_ankle` bị xe máy và các thùng carton
che. Dựa vào cẳng chân, bàn chân và tư thế ngồi, vị trí hai cổ chân vẫn có thể được ước lượng
và chắc chắn còn nằm trong khung ảnh. Vì vậy hai điểm phải dùng `v=1` và được đặt tại vị trí
ước lượng; dùng `v=0` sẽ xóa hai khớp bị che khỏi bài và làm chúng nhận 0 điểm khi gold có
`v=1`.
