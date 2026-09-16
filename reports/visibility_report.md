# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 254 | v=1 208 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 6 | 0 | 21% |
| 1 | left_eye | 20 | 9 | 0 | 31% |
| 2 | right_eye | 19 | 10 | 0 | 34% |
| 3 | left_ear | 9 | 20 | 0 | 69% |
| 4 | right_ear | 13 | 16 | 0 | 55% |
| 5 | left_shoulder | 22 | 7 | 0 | 24% |
| 6 | right_shoulder | 25 | 4 | 0 | 14% |
| 7 | left_elbow | 16 | 13 | 0 | 45% |
| 8 | right_elbow | 19 | 10 | 0 | 34% |
| 9 | left_wrist | 16 | 13 | 0 | 45% |
| 10 | right_wrist | 16 | 12 | 1 | 41% |
| 11 | left_hip | 14 | 14 | 1 | 48% |
| 12 | right_hip | 7 | 21 | 1 | 72% |
| 13 | left_knee | 13 | 10 | 6 | 34% |
| 14 | right_knee | 9 | 14 | 6 | 48% |
| 15 | left_ankle | 9 | 12 | 8 | 41% |
| 16 | right_ankle | 4 | 17 | 8 | 59% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
