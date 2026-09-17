# Mini guideline - nhóm: SOLO  |  người gán: Phan Quốc Tuấn  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Chọn `v=1`, đặt chấm tại vị trí ước lượng giải phẫu dựa trên đường gấp nép nếp may/thắt lưng. | Quần áo rộng làm che bớt điểm mốc giải phẫu chính xác, nhưng khớp vẫn nằm trong khung hình và suy luận được vị trí. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Chọn `v=1`, chấm tại vị trí giao giữa vành tai uốn cong và đầu. | Dù bị phủ một phần bởi vật thể/tóc nhưng vị trí sụn tai vẫn xác định rõ phạm vi hình học. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp phần dưới (chân/đầu gối/cổ chân) bị cắt hẳn ngoài viền ảnh chọn `v=0`, không đặt chấm. | Khớp hoàn toàn nằm ngoài khung ảnh nên theo quy tắc chuẩn COCO phải đặt `v=0`. |
| Cổ tay nằm sau tay lái / sau thân mình | Chọn `v=1`, đặt chấm tại vị trí ước lượng dựa theo hướng cẳng tay và điểm nối bàn tay. | Cổ tay bị vật thể khác che nhưng thân người nằm gọn trong ảnh, không bị lọt ra ngoài mép. |
| Hai người chồng lên nhau | Khớp người phía sau bị lấp chọn `v=1`, ước lượng dựa trên tỉ lệ cơ thể; nếu vượt rìa ảnh thì dùng `v=0`. | Phân biệt rõ giữa che khuất bởi đối tượng khác (Occluded - `v=1`) và lọt ra khỏi khung hình (Outside - `v=0`). |
| Người nhỏ đến mức nào thì không gán nữa | Bounding box có chiều cao < 20px hoặc bị nhòe không nhận diện rõ các bộ phận thì không gán. | Tránh nhiễu dữ liệu huấn luyện do không đủ độ phân giải cho mô hình học đặc trưng keypoint. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04.jpg`, người thứ `1`, khớp `left_knee` / `right_knee`

- Mơ hồ ở chỗ nào: Người nằm trọn trong khung hình nhưng phần đầu gối và cổ chân bị đồ vật che khuất hoàn toàn, phân vân giữa chọn `v=0` hay `v=1`.
- Bạn quyết thế nào: Ban đầu chọn `v=0` nhưng sau đó chỉnh lại thành `v=1` và ước lượng vị trí khớp.
- Vì sao: Theo luật bắt buộc, `v=0` chỉ dùng khi khớp ra ngoài mép ảnh. Đối tượng nằm gọn trong ảnh nên bị che khuất bắt buộc phải gán `v=1`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai rằng cứ bị che khuất là xem như không tồn tại (Outside), dẫn đến triệt tiêu keypoint đó khi tính toán OKS trong huấn luyện.

### Ca 2 - ảnh `train_14.jpg`, người thứ `1`, khớp `left_shoulder` / `right_shoulder`

- Mơ hồ ở chỗ nào: Người trong ảnh quay lưng nghiêng về phía sau, dễ nhầm lẫn vai trái/phải theo hướng mắt nhìn của bức ảnh.
- Bạn quyết thế nào: Căn cứ theo cấu trúc giải phẫu của cơ thể người trong ảnh chứ không dựa theo góc nhìn camera.
- Vì sao: Quy định bắt buộc số 3 nêu rõ: Trái/phải tính theo cơ thể người, không theo bức ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model bị đảo lộn nhãn Trái/Phải (error `dao_trai_phai`), làm giảm mAP trầm trọng khi thực hiện nhiệm vụ ước lượng tư thế.

### Ca 3 - ảnh `train_14.jpg`, người thứ `1`, khớp `left_elbow`

- Mơ hồ ở chỗ nào: Vùng tay của người thứ 1 đè sát và chồng lấn vào tay của người thứ 2 đứng bên cạnh.
- Bạn quyết thế nào: Dóng theo đường xương cánh tay của đúng đối tượng để đặt chấm, tránh bị hút sang cẳng tay người bên cạnh.
- Vì sao: Nếu chấm không chính xác sẽ dẫn đến lỗi `nham_nguoi` (chấm nhầm sang cơ thể đối tượng kề bên).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ dự đoán bộ xương bị biến dạng, kéo các điểm keypoint của hai người lại gần nhau khi có tương tác gần.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `62%` / họ `35%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline chưa thống nhất rõ quy định tai bị tóc che mất 50% thì đánh `v=1` hay `v=2`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
  - **Luật bổ sung về Tai/Mắt bị che:** Chỉ cần thấy được một phần sụn tai hoặc đuôi mắt (dù < 50%) vẫn chọn `v=1`. Trường hợp tóc phủ kín hoàn toàn không còn điểm mốc nhìn thấy được thì chọn `v=1` và chấm theo vị trí giải phẫu ước lượng dựa vào đỉnh đầu và góc hàm.