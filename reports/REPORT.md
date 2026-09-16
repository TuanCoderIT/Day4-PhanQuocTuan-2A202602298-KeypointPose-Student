# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phan Quốc Tuấn   Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 328 / 139 / 26 |
| Thời gian trung bình mỗi ảnh | Tầm 2-3 phút cho mỗi ảnh tùy vào số đối tượng trong ảnh |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` – 62%
2. `right_ear` – 45%
3. `left_eye` – 34%

Các khớp ở vùng đầu, đặc biệt là hai tai, thường khó xác định chính xác do bị tóc,
góc quay hoặc phần đầu của người khác che khuất. Mắt cũng có thể khó gán khi người
trong ảnh quay nghiêng hoặc có tư thế không thuận lợi. Vì vậy, việc các khớp này có
tỷ lệ `v=1` cao tương đối phù hợp với khó khăn thực tế khi annotation, trong đó nguyên
nhân chủ yếu là bị che khuất hơn là không xác định được vị trí giải phẫu.


## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.853 | |
| OKS@0.50 | 0.897 | |
| OKS@0.75 | 0.828 | |
| Lỗi `dao_trai_phai` | 4 | |
| Lỗi `nham_nguoi` | 1 | |
| Lỗi `xoa_khop_bi_che` | 0 | |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_06.jpg` người #1: đổi lại toàn bộ cặp Trái/Phải; sửa lỗi trượt hẳn ở `left_elbow`, `left_wrist`, `left_ankle`.
- `train_09.jpg` người #1: đổi lại toàn bộ cặp Trái/Phải; sửa lỗi trượt hẳn ở `left_ankle`.
- `train_13.jpg` người #1: đổi lại toàn bộ cặp Trái/Phải.
- `train_14.jpg` người #1: đổi lại toàn bộ cặp Trái/Phải; sửa lỗi nhầm người ở `left_elbow`; sửa lỗi trượt hẳn ở `right_elbow`.
- `train_15.jpg` người #1: sửa lỗi trượt hẳn ở `nose` và `right_eye`.
- `train_04.jpg` người #1 & `train_10.jpg` người #1: đổi 4 khớp bị gán nhầm từ `v=0` thành `v=1` do người nằm gọn giữa ảnh.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xảy ra ở 4 ảnh: `train_06.jpg` (người #1), `train_09.jpg` (người #1), `train_13.jpg` (người #1) và `train_14.jpg` (người #1).
Các ảnh này có dáng người quay lưng hoặc nhìn nghiêng. Nguyên nhân sai sót xuất phát từ việc nhầm lẫn giữa hướng trái/phải theo góc nhìn của bức ảnh thay vì căn cứ theo cơ thể của đối tượng trong ảnh.


## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-


## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` tăng từ **0.6853 lên 0.6908**, tương ứng tăng **0.0055**.
   Điều này cho thấy sau fine-tune, kết quả pose trên tập test có cải thiện nhẹ.
   Tuy nhiên, với kết quả hiện có chưa thể kết luận cụ thể 20 ảnh đã giúp model học
   thêm đặc điểm nào hoặc làm hỏng điều gì; cần đối chiếu thêm hình ảnh dự đoán và
   kết quả đánh giá chi tiết.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm **người** dễ hơn hay tìm
   **khớp** dễ hơn? Vì sao?

   Sau fine-tune, `box_mAP50-95` là **0.8041**, trong khi `pose_mAP50-95` là
   **0.6908**, chênh **0.1133**. Kết quả cho thấy việc xác định bounding box của
   người có kết quả cao hơn việc xác định chính xác các keypoint. Điều này phù hợp
   với việc xác định vị trí toàn bộ người thường đơn giản hơn so với xác định chính
   xác từng khớp trên cơ thể.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ảnh `train_10.jpg`: Model dự đoán bị lỗi **Nhầm người** (model phát hiện 2 người trong khi nhãn thực tế chỉ gán 1 người, dẫn đến phát sinh skeleton thừa).

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh `train_06.jpg` có OKS thấp nhất giữa nhãn của tôi và model (chỉ đạt **0.046**). Trong trường hợp này, **model đúng còn nhãn của tôi sai**. Dựa trên đối chiếu với tập gold, nhãn gán của tôi trên `train_06.jpg` mắc lỗi nghiêm trọng về đảo trái/phải và trượt hẳn 3 khớp (OKS vs gold chỉ đạt 0.133), do đó dẫn đến sai lệch lớn với dự đoán của model.

5. Ảnh bạn gán tệ nhất có **cũng** là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   **Có.** Ảnh tôi gán tệ nhất khi chấm với gold là `train_06.jpg` (OKS = 0.133), và đây cũng chính là ảnh có OKS bất đồng nhất giữa tôi và model (0.046). Điều này cho thấy `train_06.jpg` là một bức ảnh có góc quay khó, độ che khuất phức tạp làm cho cả người gán nhãn lẫn mô hình đều gặp khó khăn khi xác định bộ khung giải phẫu.


## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Trong ảnh `train_01.jpg`, người thứ 2 (`your_person` 1), tôi phải xác định trạng thái của keypoint `right_wrist`. Phần cổ tay phải của đối tượng bị phần thân và tay của người bên cạnh che khuất một phần, không nhìn thấy trực tiếp điểm khớp. Dựa vào hướng của cẳng tay và vị trí khuỷu tay `right_elbow`, tôi vẫn ước lượng được vị trí giải phẫu của cổ tay nằm hoàn toàn bên trong khung hình. Vì vậy, tôi chọn trạng thái `v=1` thay vì `v=0`, giúp phân biệt trường hợp khớp bị che khuất với trường hợp khớp thực sự nằm ngoài rìa ảnh.