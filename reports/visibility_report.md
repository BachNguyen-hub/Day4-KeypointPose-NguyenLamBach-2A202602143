# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 309 | v=1 153 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 20 | 9 | 0 | 31% |
| 3 | left_ear | 8 | 21 | 0 | 72% |
| 4 | right_ear | 14 | 15 | 0 | 52% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 18 | 11 | 0 | 38% |
| 10 | right_wrist | 18 | 10 | 1 | 34% |
| 11 | left_hip | 15 | 13 | 1 | 45% |
| 12 | right_hip | 19 | 9 | 1 | 31% |
| 13 | left_knee | 16 | 9 | 4 | 31% |
| 14 | right_knee | 16 | 9 | 4 | 31% |
| 15 | left_ankle | 13 | 6 | 10 | 21% |
| 16 | right_ankle | 11 | 8 | 10 | 28% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
