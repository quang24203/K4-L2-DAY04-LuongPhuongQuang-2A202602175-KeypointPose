# Mini guideline - nhóm: ______  |  người gán: ______  |  ngày: ______

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
| Hông của người mặc quần áo dài |Ước lượng vị trí tâm khớp hông theo giải phẫu cơ thể, đặt điểm và chọn trạng thái Occluded (v=1). | Lớp quần áo dài/rộng che khuất cấu trúc trực tiếp của tâm khớp, nhưng dựa vào tỷ lệ cơ thể (gióng từ vai, đầu gối) ta vẫn suy ra được vị trí. Tuân theo luật: "Bị che nhưng vẫn suy ra vị trí -> Vẫn đặt điểm ước lượng, bật Occluded|
| Tai bị tóc hoặc mũ bảo hiểm che một phần |Ước lượng vị trí lỗ tai (dưới lớp tóc/mũ), đặt điểm và chọn trạng thái Occluded (v=1) |Dù bị lấp một phần hay toàn bộ, dựa vào khung xương hàm và vị trí mắt/mũi, ta hoàn toàn có thể đoán được chính xác tai nằm ở đâu. Áp dụng luật "bị che nhưng vẫn suy ra vị trí|
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) |Các điểm bị cắt ra ngoài mép ảnh bắt buộc gạt sang trạng thái Outside (v=0) và không đặt tọa độ. Không được xóa keypoint |Quy tắc bắt buộc: "Mỗi người nhìn thấy là một skeleton và luôn có đủ 17 keypoint; không xoá keypoint khó". Khớp đã nằm ngoài khung hình thì dùng Outside (v=0) để bảo toàn cấu trúc 17 điểm của file xuất ra |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí cổ tay dựa theo đường thẳng của cẳng tay, đặt điểm và chọn trạng thái Occluded (v=1).|Việc cổ tay bị vật thể (tay lái) hoặc cơ thể người khác che khuất cũng được tính là occlusion. Vẫn có cơ sở để suy ra vị trí nên phải đặt điểm ước lượng (v=1 |
| Hai người chồng lên nhau |Tạo đủ 2 skeleton riêng biệt. Các khớp của người đứng sau bị người đứng trước che khuất thì đặt điểm ước lượng và chọn Occluded (v=1). | Mỗi người là một đối tượng độc lập và "luôn có đủ 17 keypoint". Cơ thể người phía trước đóng vai trò như vật cản che khuất, người phía sau vẫn phải được gán đủ điểm dựa trên suy luận giải phẫu|
| Người nhỏ đến mức nào thì không gán nữa |Gán mọi người thuộc phạm vi trong ảnh. Chỉ bỏ qua khi người quá nhỏ hoặc quá mờ đến mức không thể phân biệt được hình dáng cơ thể (không thể suy luận nổi vị trí đầu, vai, hông) |Bài lab yêu cầu "Gán mọi người thuộc phạm vi trong ảnh". Tuy nhiên, nếu số lượng pixel quá ít khiến việc ước lượng giải phẫu trở thành "đoán mò vô căn cứ", việc cố gán sẽ sinh ra nhiễu (noise data) làm giảm chất lượng mô hình |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

### Ca 2 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

### Ca 3 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
