# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Hoàng Nhật Anh
- MSSV: 2A202603183
- Email: 26ai.anhhn2@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/nhatanhhoang2905/K5-L2-Day02-HoangNhatAnh-2A202603183-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1FnxC7Udw-VAIeabmqx0SJXwOHJoJr35H#scrollTo=3e0508d8

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

Tóm tắt 3 ca mơ hồ đã quyết định và cách áp dụng schema:

1. **Ca 1 (`drive_025.jpg`):** Một xe con màu trắng ở góc dưới bên phải bị cắt bởi cạnh dưới ảnh. **Quyết định:** Gán `car`; vẽ box quanh phần xe nhìn thấy và để cạnh dưới box chạm mép ảnh. Chỉ đánh dấu phần nằm trong ảnh, không ước lượng phần xe bên ngoài.
2. **Ca 2 (`drive_042.jpg`):** Xe buýt hai tầng và xe chở hàng chất cao xuất hiện gần nhau; một phần xe bị hàng hóa che khuất. **Quyết định:** Vẽ box riêng cho từng xe; gán `bus` cho xe buýt và `truck` cho xe chở hàng nếu phần thân/cabin còn nhìn thấy đủ để nhận diện. Không gộp hai xe thành một box.
3. **Ca 3 (`drive_034.jpg`):** Ở giữa ảnh có xe chuyên dụng màu trắng với cabin và sàn kéo; hình dáng có thể gây phân vân giữa `car` và `truck`. **Quyết định:** Không chốt lớp chỉ dựa vào mục đích sử dụng; cần xác nhận với giảng viên nếu chưa phân biệt chắc. Nhãn hiện tại của ảnh là `truck`, nên cần đối chiếu lại khi self-QC.



## 3. Annotation và self-QC

- **Số file nhãn train:** 50 file (`drive_001.txt`–`drive_050.txt`) trong `dataset/labels/train/`, khớp với 50 ảnh trong `dataset/images/train/`.
- **Kết quả checker cấu trúc:** Đã chạy `python tools/check_yolo_labels.py`; checker đạt **50/50** file hợp lệ. Không phát hiện thiếu/thừa file, sai số cột, class ID ngoài phạm vi, tọa độ sai chuẩn hóa hoặc box vượt biên. Kết quả này chỉ kiểm tra format, không đánh giá chất lượng so với gold.
- **Gold-set QC:** [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json) hiện ghi `passed: false`; chỉ số sau rework được trình bày ở mục 4. Đây là kết quả chất lượng annotation, tách biệt với checker cấu trúc.
- **Danh sách 10 ảnh đã xem để rà nội dung và điểm cần đối chiếu:**
  1. `drive_001.jpg` — Đường nhiều làn, xe ở gần và xa; rà xe nhỏ ở xa và xe bị cắt ở mép ảnh.
  2. `drive_003.jpg` — Ngã tư đông, xe nối đuôi và che khuất nhau; rà box riêng cho từng xe và phần thân nhìn thấy.
  3. `drive_004.jpg` — Cảnh ban đêm có người đi bộ, xe máy và ô tô; rà phạm vi class, không gán người/xe máy.
  4. `drive_005.jpg` — Mặt đường ướt, giao lộ có nhiều xe và phản chiếu; rà box không lấn nhiều vào mặt đường hoặc bóng phản chiếu.
  5. `drive_008.jpg` — Giao lộ đông có xe con, xe buýt và xe tải; rà phân lớp, che khuất và box riêng.
  6. `drive_009.jpg` — Dòng xe đông với xe buýt lớn, xe tải và xe con gần nhau; rà tách box và ranh giới `bus`/`truck`.
  7. `drive_012.jpg` — Giao lộ rộng với nhiều xe con và xe van; rà xe cùng lớp không bị gộp và class van nhất quán.
  8. `drive_014.jpg` — Xe cứu hộ kéo xe ở tiền cảnh cùng các xe khác; rà lớp xe chuyên dụng và box không nhập với xe lân cận.
  9. `drive_020.jpg` — Đường trên cao, xe chuyển động có nhòe; rà box bám thân xe, không lấy nền đường.
  10. `drive_028.jpg` — Đường nhiều làn với xe nhỏ ở xa và xe bị cắt ở cạnh dưới; rà completeness và box chạm mép ảnh.
- **Lỗi và rework:** Kết quả gold QC cho thấy còn nhiều box không khớp; riêng `drive_042` có 6 student box, 4 gold box nhưng chỉ 1 match. JSON chỉ có số lượng box/matches theo ảnh, không ghi nguyên nhân từng FP/FN, nên không thể kết luận lỗi cụ thể hoặc khẳng định các box nào đã được sửa chỉ từ file này.
- **Commit:** Lịch sử repo có commit `0173b33` (tạo bộ bài cá nhân) và `acfa174` (`fix: refine annotations after gold QC`). Các file `dataset/labels/train/` hiện có thay đổi chưa commit so với `acfa174`; vì vậy chưa có commit hash cho bộ nhãn hiện đang được đánh giá.

## 4. Gold QC và rework

| Chỉ số | Trước rework | Sau rework / hiện tại |
|---|---:|---:|
| TP | 468 | 464 |
| FP | 487 | 457 |
| FN | 185 | 189 |
| Precision | 0.4901 | 0.5038 |
| Recall | 0.7167 | 0.7106 |
| F1 | 0.5821 | 0.5896 |
| mean_iou_of_matches | 0.8627 | 0.8632 |
| passed | `false` | `false` |

Dùng ngưỡng ghép cùng lớp IoU ≥ 0.50. Rework giảm 30 FP nhưng cũng giảm 4 TP; F1 chỉ tăng nhẹ từ 0.5821 lên 0.5896, vẫn dưới ngưỡng 0.80 nên kết quả chưa đạt. Mean IoU đạt 0.8632, nhưng chỉ tính các box đã ghép; cần ưu tiên rà `drive_009` (6/21 match) và `drive_042` (1/4 match). JSON không chỉ ra nguyên nhân từng FP/FN, nên cần đối chiếu trực tiếp ảnh và box trước khi kết luận.

## 5. Kết quả YOLO

- **Runtime và cấu hình train:** `args.yaml` xác nhận dùng `yolo26n.pt`, 50 epochs, `imgsz=640`, `batch=-1`, `seed=20260909`, cấu hình `device=0` (GPU index 0). Không có log ghi model GPU cụ thể của phiên train trên Colab.
- **Checkpoint và lần đánh giá:** Đã lưu [`best.pt`](assets/yolo26_runs/vehicle_annotation/weights/best.pt) và [`last.pt`](assets/yolo26_runs/vehicle_annotation/weights/last.pt). Bảng dưới là kết quả chạy `best.pt` trực tiếp trên 10 ảnh eval (113 đối tượng) bằng Ultralytics 8.4.208, PyTorch 2.7.1+cu118, CPU Intel Core i5-1235U; đây là lần đánh giá lại trong VS Code, không phải hàng epoch trong `results.csv`.

| **Chỉ số trên 10 ảnh eval** | **Giá trị thực tế** |
| --------------------------- | -------------------: |
| Precision | 0.2021 |
| Recall | 0.3337 |
| mAP50 | 0.2890 |
| mAP50–95 | 0.2060 |

Artifact hiện có [`results.csv`](assets/yolo26_runs/vehicle_annotation/results.csv), [`args.yaml`](assets/yolo26_runs/vehicle_annotation/args.yaml), confusion matrix và các đường cong precision/recall trong [thư mục vehicle_eval](assets/yolo26_runs/vehicle_eval/), cùng 10 ảnh dự đoán trong [eval_predictions/visuals](assets/eval_predictions/visuals/). Theo confusion matrix normalized, `truck` và `van` cùng có recall thấp nhất (đường chéo bằng 0 trên biểu đồ); đây là tín hiệu cần ưu tiên xem lại, nhưng tập eval nhỏ nên không khái quát hóa quá mức.

Trả lời bốn câu hỏi cuối notebook:

1. **Lớp recall thấp nhất:** `truck` và `van` cùng bằng 0 theo confusion matrix normalized. Xem [drive_053.jpg](assets/eval_predictions/visuals/drive_053.jpg) cùng các ảnh eval còn lại để đối chiếu dự đoán với nhãn; drive_053 có hai box van trong ground truth.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** Kiểm tra độ khít và vị trí bốn cạnh box: box có lấy thừa nền, cắt mất xe hoặc lệch khỏi vật thể không. IoU cao nhạy hơn với sai lệch hình học nhỏ.
3. **Lỗi class có thể liên quan guideline:** Confusion matrix cho thấy có nhầm lẫn giữa `truck` và `van` (một phần ground truth `truck` bị dự đoán thành `van`). Điều này đáng để đối chiếu với guideline phân biệt loại xe, nhưng không đủ để kết luận nguyên nhân là guideline; nên xem ảnh và box cụ thể trước.
4. **Vì sao không train trên 10 ảnh eval:** Giữ eval tách khỏi tập train để không rò rỉ dữ liệu. Tuy nhiên, notebook dùng chính 10 ảnh này làm validation trong quá trình train và chọn `best.pt`, nên chúng không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Self-QC cấu trúc | Cần phân biệt lỗi format với lỗi chất lượng annotation. | Chạy `python tools/check_yolo_labels.py`; đọc riêng `outputs/annotation_evaluation.json`. | Checker format đạt 50/50; gold QC sau rework vẫn chưa đạt vì F1=0.5896 < 0.80. |
| Gold QC và rework | Sau rework, FP giảm nhưng FN tăng; F1 chỉ tăng nhẹ và vẫn dưới ngưỡng. | So sánh hai lần evaluation và dùng danh sách `per_image` để ưu tiên mở ảnh có ít matches, ví dụ `drive_042` (1/4 gold match), `drive_018` (1/2), `drive_017` (2/4). | Đã giảm FP 30; cần tiếp tục rà từng ảnh/box để xác định nguyên nhân, vì JSON không nêu chi tiết lỗi từng box. |
| Train/eval YOLO | Ban đầu thiếu file metrics tổng hợp cho lần eval riêng. | Đánh giá lại `best.pt` trực tiếp trên 10 ảnh eval bằng Ultralytics trong VS Code; lưu thông số runtime và kết quả vào báo cáo. | Có checkpoint, `results.csv`, biểu đồ, 10 ảnh prediction và metrics eval trực tiếp. GPU cụ thể của phiên Colab vẫn chưa xác minh được. |

## 7. Tự đánh giá theo rubric

| Tiêu chí | Kết quả đối chiếu với bài làm | Tự đánh giá |
|---|---|---:|
| Hoàn thành và định dạng YOLO (10) | Có đủ 50 file nhãn cùng basename; checker đạt 50/50, đúng 5 cột, normalized và class ID trong phạm vi. | **10/10** |
| Đầy đủ vật thể (20) | Có nhãn cho 50 ảnh train. Gold QC sau rework có recall 0.7106, cho thấy vẫn còn khả năng bỏ sót vật thể; JSON tổng hợp không chỉ rõ từng box thiếu. | **12/20** |
| Độ bám sát và hình học box (20) | `mean_iou_of_matches=0.8632`, cao hơn ngưỡng 0.70; tuy vậy chỉ tính các box ghép được, còn F1 gold QC thấp nên chưa thể xem là hoàn tất. | **16/20** |
| Phân lớp nhất quán (15) | Schema có đủ `car/truck/bus/van` và checker xác nhận ID hợp lệ. Gold QC vẫn có FP/FN; ca `drive_034.jpg` còn điểm cần rà soát giữa mô tả guideline và nhãn `truck`. | **10/15** |
| Mini guideline và quyết định mơ hồ (10) | Có định nghĩa lớp, quy tắc vẽ box và ba case có lý do; `drive_034.jpg` vẫn cần làm rõ để quyết định lớp nhất quán hơn. | **8/10** |
| Gold-set annotation QC (15) | Đã thực hiện và giải thích vòng rework: FP giảm 30, F1 tăng 0.5821 lên 0.5896, mean IoU đạt 0.8632. F1 vẫn dưới 0.80 và `passed=false`; chưa phân tích được từng FP/FN từ JSON. | **13/15** |
| Pipeline YOLO và phân tích (10) | Có notebook/Colab, checkpoint, metrics trên 10 ảnh eval và ảnh dự đoán; eval không dùng làm train nhưng đã dùng làm validation để chọn `best.pt`, nên không phải test độc lập. Có confusion matrix và nhận xét lỗi lớp. | **8/10** |
| **Tổng tự đánh giá** | Điểm tự ước lượng theo bằng chứng hiện có; giảng viên chấm theo rubric chính thức. | **77/100** |

## 8. Công cụ AI và tài liệu tham khảo

Sử dụng công cụ Antigravity (Gemini) để hỗ trợ tra cứu chuẩn định dạng YOLO, cấu hình môi trường PowerShell UTF-8 và đối chiếu các tiêu chí Rubric.
Sử dụng công cụ CVAT để gán nhãn và rà soát trực quan bounding box trên ảnh.
Không sử dụng AI để can thiệp hay thay đổi file nhãn ground truth của giảng viên.
