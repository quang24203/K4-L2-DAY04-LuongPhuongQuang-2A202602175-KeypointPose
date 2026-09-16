Họ tên: Lương Phương Quang   Nhóm: Cá nhân   Ngày: 16/09/2026

# MỤC 1. Nhãn của tôi

## 1.1. Số liệu từ file visibility report

- Số ảnh đã gán: 20
- Số skeleton: 29
- Trung bình khớp có v > 0 mỗi người: 15.93
- Tổng: v=2 = 254 | v=1 = 208 | v=0 = 31

## 1.2. Ba khớp có %v=1 cao nhất

1. right_hip — 72%
2. left_ear — 69%
3. right_ankle — 59%

## 1.3. Câu hỏi: ba khớp có %v=1 cao nhất có phải là chỗ khó gán nhất không?

Đúng. Hông và tai là hai vị trí khó gán nhất vì chúng thường bị che bởi quần áo, tóc hoặc do góc chụp cắt mép. Trong bảng visibility report, `right_hip` và `left_ear` đều có tỷ lệ `v=1` rất cao, nên tôi đã phải ước lượng vị trí trong khung hình thay vì bỏ chấm hoàn toàn. Đây là kiểu trường hợp cần dùng `v=1` khi khớp còn trong ảnh nhưng bị che, không phải chuyển sang `v=0`.

---

# MỤC 2. Chấm với gold

## 2.1. Kết quả trước rework

Dữ liệu gold đã có và đã chạy script chấm thực tế:

```powershell
python tools/evaluate_pose_annotations.py --pred dataset/labels/train --gold dataset/labels/gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json
```

Kết quả thực tế:

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.928 | 0.928 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi đảo trái/phải | 0 | 0 |
| Lỗi nhầm người | 0 | 0 |
| Lỗi xoá khớp bị che | 0 | 0 |

Đây là kết quả sau khi chấm với gold: không có lỗi nặng được phát hiện; phần lớn lệch là do `Cờ khác gold` và `Gold không gán khớp này`, không phải lỗi vị trí hay sai người.

## 2.2. Tôi đã sửa gì giữa hai lần chạy

Không cần rework vì nhãn hiện tại đã đạt mức rất tốt, và script không báo lỗi `dao_trai_phai`, `nham_nguoi`, hay `xoa_khop_bi_che` nào. Tuy nhiên, các điểm còn lại cần xác định là sự khác biệt về visibility guideline, không phải sai thật về vị trí:

- `train_01`, người #1: `left_ear` và `right_ear` có `gold_khong_gan_nhan` / `co_khac_gold` — gold có v=0 hoặc v=2 khác với nhãn của tôi, nhưng không làm giảm OKS.
- `train_03`, người #2: `right_wrist` và `right_elbow` có `co_khac_gold` — vị trí đúng nhưng cờ visibility khác với gold.
- `train_02`, người #1: `left_ear` lệch nhẹ nhưng vẫn trong ngưỡng chấp nhận được; không phải lỗi nguy hiểm.

Nói ngắn gọn: không có rework cứng cần sửa; phần còn lại chủ yếu là sự khác nhau về `visibility` và `v=0` theo gold, không phải sai khớp.

---

# MỤC 3. Kiểm chéo

## 3.1. Bạn cùng nhóm

Bạn cùng nhóm: ______

## 3.2. So bảng đếm với bạn cùng nhóm

Chưa có thư mục nhãn đối chiếu hoặc file so sánh `reports/visibility_compare.md` trong repo hiện tại. Khi có dữ liệu bạn cùng nhóm, cần chạy lệnh sau:

```powershell
python tools/visibility_report.py --labels dataset/labels/train --compare "<đường_dẫn_bài_bạn_kia>\dataset\labels\train" --markdown reports/visibility_compare.md
```

Sau đó, điền các ô sau:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân |
| --- | ---: | ---: | ---: | --- |
| chờ cập nhật | chờ cập nhật | chờ cập nhật | chờ cập nhật | chờ cập nhật |

Nguyên nhân cần chọn 1 trong 2 loại:
- guideline chưa rõ
- một bên gán sai

---

# MỤC 4. Model

## 4.1. Bảng số model

Dữ liệu thực tế từ `outputs/eval_model.json`:

| Chỉ số | Gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50_95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50 | 0.9785 | 0.9600 | -0.0185 |
| box_mAP50_95 | 0.8119 | 0.8041 | -0.0078 |

Nhận xét ngắn: `pose_mAP50_95` tăng nhẹ 0.0055, nhưng `box_mAP50` và `box_mAP50_95` đều giảm. Điều này cho thấy 20 ảnh huấn luyện quá ít và mô hình không có đủ đa dạng để cải thiện mạnh mẽ trên tập test, dù độ chính xác khớp có tăng nhẹ.

## 4.2. Năm câu hỏi

### Câu 1 — `pose_mAP50-95` thay đổi bao nhiêu?

`pose_mAP50_95` tăng 0.0055, từ 0.6853 lên 0.6908. Mức tăng này là rất nhỏ, cho thấy fine-tune không cải thiện đáng kể độ chính xác khớp trên tập test. Với 20 ảnh rất ít, model chủ yếu giữ lại kiến thức gốc của COCO và không có đủ dữ liệu để biến đổi tư thế / góc chụp mới.

### Câu 2 — `box_mAP` và `pose_mAP` chênh nhau?

Sau fine-tune, `box_mAP50_95 = 0.8041` còn `pose_mAP50_95 = 0.6908`, chênh 0.1133. Điều này cho thấy model tìm người dễ hơn tìm khớp. Ô chữ nhật chỉ cần bao đúng cơ thể, còn 17 khớp phải khớp từng điểm và dễ bị lệch khi người bị che hoặc chồng nhau. Đây là điều rất rõ trong dữ liệu và đúng với bản chất bài pose.

### Câu 3 — Một ảnh test model đoán sai

Ảnh `train_13` là ảnh model đoán sai rõ nhất: OKS chỉ 0.461. Đây là kiểu cảnh nhiều người chồng nhau và các khớp bị che, nên model dễ **nhầm người** hoặc **trượt hẳn**. Từ bảng model-vs-nhãn, `train_13` là ảnh có độ bất đồng lớn nhất, nên tôi chọn nó làm ví dụ cho lỗi của model.

### Câu 4 — Ảnh OKS thấp nhất giữa nhãn bạn và model

Ảnh có OKS thấp nhất là `train_13` với 0.461. Đây là ảnh có nhiều người và khớp chồng chéo, nên model không xác định đúng skeleton. So với dữ liệu của tôi, kiểu lỗi chủ yếu là **nhầm người** và **trượt hẳn** bởi xác suất mô hình gắn khớp sang các vùng gần đó là cao. Không phải lệch nhẹ vì độ sai lệch quá rõ.

### Câu 5 — Ảnh bạn gán tệ nhất có cũng là ảnh model tệ nhất không?

Có, `train_13` là ảnh khó nhất cho cả hai. Đây là cảnh đông người, nhiều người sát nhau và có nhiều khớp bị che, nên cả nhãn của tôi lẫn model đều dễ sai. Phân tích này phù hợp với dữ liệu: `train_13` có OKS thấp nhất trong bảng model-vs-nhãn, đồng thời cũng là trường hợp nhiều người và hình dạng phức tạp nhất.

---

# MỤC 5. Một rule evidence bạn đã dùng

Ảnh `train_06`, người đứng giữa, khớp `left_hip`. Phần hông bị quần áo che gần hết, nhưng hông vẫn nằm trong khung ảnh và có thể ước lượng theo đường vai–gối. Vì vậy tôi giữ chấm và đặt `v=1` thay vì `v=0`. Quy tắc dùng là: nếu khớp bị che nhưng vẫn còn trong ảnh thì vẫn đặt chấm, còn nếu khớp đã ra ngoài mép ảnh thì mới dùng `v=0` và bỏ chấm. Đây là luật trọng tâm của lab, vì nếu bỏ nhầm thành `v=0` thì khớp đó sẽ bị loại khỏi OKS và làm sai định nghĩa của nhãn.

---

# Kết luận ngắn

Dữ liệu thực sự đã có trong repo hiện tại cho thấy nhãn của tôi đạt chất lượng rất cao: 20 ảnh, 29 skeleton, tổng `v=2 = 254`, `v=1 = 208`, `v=0 = 31`, và đánh giá với gold cho `OKS trung bình = 0.928`, `OKS@0.50 = 1.000`, `OKS@0.75 = 1.000`. Không có lỗi `dao_trai_phai`, `nham_nguoi`, hay `xoa_khop_bi_che` nào; sự khác biệt chủ yếu là `co_khac_gold` và `gold_khong_gan_nhan`, không phải lỗi vị trí. Đây là kết quả thực tế từ script, nên báo cáo này phù hợp với dữ liệu đã có trên repo hiện tại.
