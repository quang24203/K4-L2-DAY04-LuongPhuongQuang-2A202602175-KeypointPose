# Mini guideline - nhóm: Cá nhân  |  người gán: ______  |  ngày: ______

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
| Hông của người mặc quần áo dài | Ước lượng tâm khớp theo giải phẫu cơ thể, đặt điểm và chọn `Occluded` (`v=1`). | Quần áo che cấu trúc trực tiếp, nhưng vai, thân và đầu gối vẫn giúp suy ra vị trí hông. |
| Tai bị tóc hoặc mũ che | Ước lượng vị trí tai theo mắt, mũi và đường hàm, đặt điểm và chọn `Occluded` (`v=1`). | Tai bị che nhưng vẫn nằm trong khung ảnh nên không được dùng `Outside`. |
| Người bị cắt ở mép ảnh | Với khớp đã ra ngoài mép, chọn `Outside` (`v=0`) và không đặt tọa độ. | `v=0` chỉ dùng khi khớp không còn pixel nào trong khung; không xóa keypoint khỏi skeleton. |
| Cổ tay nằm sau tay lái hoặc thân mình | Ước lượng theo hướng cẳng tay, đặt điểm và chọn `Occluded` (`v=1`). | Vật thể hoặc người khác che cổ tay, nhưng vị trí vẫn còn trong ảnh và có thể suy ra. |
| Hai người chồng lên nhau | Tạo một skeleton riêng cho mỗi người; khớp bị che của người phía sau vẫn đặt điểm và chọn `Occluded` (`v=1`). | Mỗi người là một đối tượng độc lập, không gộp khớp của người này sang người kia. |
| Người rất nhỏ hoặc quá mờ | Gán nếu còn phân biệt được đầu, vai và thân; chỉ bỏ qua khi không thể suy luận vị trí khớp một cách có căn cứ. | Gán mò khi ảnh không đủ thông tin sẽ tạo nhiễu cho dữ liệu huấn luyện. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_06`, người đứng giữa, khớp `left_hip`

- Mơ hồ ở chỗ nào: Hông bị quần áo che gần hết, không thấy rõ bề mặt khớp.
- Bạn quyết thế nào: Đặt điểm theo đường nối vai - gối và chọn `Occluded` (`v=1`).
- Vì sao: Hông vẫn nằm trong khung ảnh và vị trí có thể suy ra từ hình dáng cơ thể.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học rằng hông bị quần áo che thì không có khớp.

### Ca 2 - ảnh `train_01`, người #1, khớp `left_ear` và `right_ear`

- Mơ hồ ở chỗ nào: Tai bị tóc hoặc góc chụp che, nên khó phân biệt `v=1` và `v=2`.
- Bạn quyết thế nào: Giữ điểm ở vị trí giải phẫu ước lượng và chọn `Occluded` (`v=1`) khi tai còn trong ảnh.
- Vì sao: Mắt, mũi và đường hàm vẫn giúp xác định vị trí tai.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học sai visibility của tai và có thể bỏ qua tai trong các ảnh bị tóc che.

### Ca 3 - ảnh `train_03`, người #2, khớp `right_wrist` và `right_elbow`

- Mơ hồ ở chỗ nào: Cánh tay bị che một phần nên cờ visibility có thể khác giữa hai cách gán.
- Bạn quyết thế nào: Giữ điểm theo hướng cẳng tay và chọn `Occluded` (`v=1`).
- Vì sao: Khớp vẫn ở trong khung ảnh, chỉ bị che chứ không ra ngoài mép.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học cờ visibility không nhất quán cho các khớp tay bị che.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Không áp dụng: bài này được thực hiện cá nhân, không có thư mục nhãn của bạn cùng nhóm để đối chiếu.
- Không có số liệu `%v=1` đối chiếu và không bổ sung luật nhóm mới.
