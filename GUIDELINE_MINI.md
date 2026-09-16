# Mini guideline - nhóm: ______ | người gán: Nguyễn Lâm Bách | ngày: 16/09/2026

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên và đúng thứ tự; lấy từ file `.SVG` chung.
- Mọi người được gán đều có đủ 17 điểm. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo cơ thể người, không theo phía của bức ảnh.
- Bị che nhưng vị trí khớp còn trong khung: `v=1`, vẫn đặt chấm ở vị trí ước lượng.
- Vị trí khớp nằm ngoài mép ảnh: `v=0`, không đặt chấm.
- Không dùng `Hidden` (`h`) vì trạng thái này không được lưu vào file.

## 2. Luật áp dụng thống nhất

| Tình huống | Luật áp dụng | Căn cứ kiểm chứng |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Nếu hông nằm trong khung nhưng bị áo che, đặt tại tâm khớp háng ước lượng và dùng `v=1`; chỉ dùng `v=2` khi mốc khớp nhìn thấy trực tiếp. | Theo đường thân, mép quần/áo và trục đùi; không quyết định chỉ dựa vào việc nhìn thấy lớp vải. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Thấy trực tiếp mốc tai thì `v=2`; không thấy mốc tai nhưng tai còn trong khung và suy ra được từ mắt/đầu thì `v=1`; vị trí tai ngoài khung mới là `v=0`. | Tai là hai khớp có tỷ lệ `v=1` cao nhất (72% trái, 52% phải) và nhiều bất đồng cờ với gold. |
| Người bị cắt ở mép ảnh, chỉ thấy từ hông trở lên | Các khớp có vị trí giải phẫu nằm ngoài biên ảnh dùng `v=0`; khớp còn trong ảnh nhưng bị vật/cơ thể che dùng `v=1`. | Kiểm tra vị trí khớp, không kiểm tra toàn bộ chi; không mặc định mọi khớp không nhìn thấy là `v=0`. |
| Cổ tay nằm sau tay lái hoặc sau thân mình | Nếu cổ tay vẫn nằm trong khung, dùng `v=1` và ước lượng từ cẳng tay/bàn tay; nếu thấy trực tiếp tâm cổ tay thì `v=2`. | Bám theo trục khuỷu–cẳng tay–bàn tay và hình dạng vật che. |
| Hai người chồng lên nhau | Chọn một người, lần theo vai–khuỷu–cổ tay và hông–gối–cổ chân của đúng người đó trước khi chuyển sang người khác; điểm bị người kia che dùng `v=1`. | Không gán điểm chỉ vì nó là điểm gần nhất; `train_04.jpg` đã có một `left_wrist` gần cổ tay của người khác hơn. |
| Người nhỏ đến mức nào thì không gán nữa | Gán nếu vẫn xác định được một người và có thể đặt nhất quán skeleton 17 điểm; nếu các bộ phận không đủ phân giải để xác định trái/phải và vị trí khớp thì chuyển ca đó cho người kiểm duyệt, không tự đặt ngưỡng pixel. | Ngưỡng pixel chưa được lớp/nhóm cung cấp; cần quy tắc chung trước khi loại người khỏi tập. |

### Rule rút ra từ lần đánh giá hiện tại

- Vật che không phải là biên ảnh: một khớp bị xe, thùng carton, tay lái, quần áo hoặc người
  khác che nhưng vị trí giải phẫu vẫn nằm trong ảnh phải là `v=1`, không phải `v=0`.
- Trước khi đặt cổ tay/cổ chân trong ảnh nhiều người, khóa chuỗi chi theo đúng skeleton:
  vai → khuỷu → cổ tay hoặc hông → gối → cổ chân.
- Cờ `v=1`/`v=2` diễn tả khả năng nhìn thấy mốc khớp, không diễn tả độ chắc chắn chủ quan.
  Nếu mốc khớp nhìn thấy trực tiếp thì dùng `v=2`; nếu phải suy ra qua vật che thì dùng `v=1`.

> Còn thiếu: chèn screenshot CVAT minh họa cho từng luật trong bảng sau khi mở lại project.
> Ảnh gốc không thể hiện vị trí chấm/cờ nên không thay thế hoàn toàn screenshot CVAT.

## 3. Ba ca mơ hồ/lỗi đã gặp

### Ca 1 - ảnh `train_12.jpg`, người thứ 1, khớp `left_ankle` và `right_ankle`

- Mơ hồ ở chỗ nào: hai cổ chân không nhìn thấy trực tiếp vì bị xe máy và thùng carton che.
- Quyết định thống nhất: dùng `v=1` và đặt chấm tại vị trí ước lượng, không dùng `v=0`.
- Vì sao: hai cẳng chân/bàn chân và tư thế ngồi cho thấy vị trí cổ chân vẫn nằm trong ảnh.
- Nếu quyết ngược lại: model học rằng khớp bị vật che tương đương khớp nằm ngoài ảnh; đánh giá
  hiện tại đã ghi nhận hai lỗi `xoa_khop_bi_che` và tính 0 điểm cho hai khớp này.

### Ca 2 - ảnh `train_04.jpg`, người của bài số 1, khớp `left_wrist`

- Mơ hồ ở chỗ nào: ảnh có hai người, các tay và xe chồng lấn nên dễ lấy cổ tay gần nhất của
  người khác.
- Quyết định thống nhất: lần theo vai → khuỷu → cổ tay của cùng một người rồi mới đặt điểm;
  dùng `v=1` nếu cổ tay bị người hoặc xe che.
- Vì sao: quan hệ hình học của cả chi đáng tin hơn khoảng cách đến một điểm đơn lẻ.
- Nếu quyết ngược lại: model học một chi nối sang skeleton khác; đánh giá hiện tại đã ghi nhận
  một lỗi `nham_nguoi` tại đúng keypoint này.

### Ca 3 - ảnh `train_02.jpg`, người thứ 1, khớp `left_knee` và `right_knee`

- Mơ hồ ở chỗ nào: tư thế đứng cạnh xe đạp và góc nhìn làm tâm gối dễ bị đặt lệch.
- Quyết định thống nhất: đặt tâm gối trên trục hông–gối–cổ chân và đối chiếu đường viền chân,
  không lấy mép quần làm tâm khớp.
- Vì sao: đánh giá cho thấy cả hai gối lệch 35 px, bằng 1.3 lần bán kính dung sai.
- Nếu quyết ngược lại: model học trục chân bị dịch có hệ thống dù người vẫn được ghép đúng.

## 4. Kết quả visibility hiện tại

- 20 ảnh, 29 người, trung bình 15.93 keypoint có `v>0` mỗi người.
- Tổng trạng thái: `v=2`: 309; `v=1`: 153; `v=0`: 31.
- Ba tỷ lệ `v=1` cao nhất: `left_ear` 72%, `right_ear` 52%, `left_hip` 45%.
- Gold comparison: 58 bất đồng `v=1`/`v=2`, tập trung nhiều ở tai và hông. Đây là tín hiệu
  phải áp dụng thống nhất tiêu chí “thấy trực tiếp mốc khớp” thay vì cảm giác chắc/không chắc.

## 5. Sau khi so với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: **chưa xác định** (chưa có visibility report của bạn cùng nhóm).
- Nguyên nhân là guideline chưa rõ hay một bên gán sai: **chưa thể kết luận**.
- Khi có báo cáo thứ hai, so từng tỷ lệ trên cùng mẫu số người và ghi lại khớp lệch lớn nhất.
- Luật mới sau khi thống nhất phải có dạng kiểm chứng được: điều kiện nhìn thấy/vị trí trong
  khung → trạng thái `v`, kèm ít nhất một screenshot CVAT.
