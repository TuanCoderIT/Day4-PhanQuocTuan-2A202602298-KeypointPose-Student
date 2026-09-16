# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 16.1 khớp có v > 0 mỗi người
- Tổng: v=2 328 | v=1 139 | v=0 26

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 6 | 0 | 21% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 20 | 9 | 0 | 31% |
| 3 | left_ear | 11 | 18 | 0 | 62% |
| 4 | right_ear | 16 | 13 | 0 | 45% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 20 | 9 | 0 | 31% |
| 10 | right_wrist | 20 | 8 | 1 | 28% |
| 11 | left_hip | 20 | 9 | 0 | 31% |
| 12 | right_hip | 21 | 8 | 0 | 28% |
| 13 | left_knee | 18 | 8 | 3 | 28% |
| 14 | right_knee | 19 | 6 | 4 | 21% |
| 15 | left_ankle | 11 | 9 | 9 | 31% |
| 16 | right_ankle | 11 | 9 | 9 | 31% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
