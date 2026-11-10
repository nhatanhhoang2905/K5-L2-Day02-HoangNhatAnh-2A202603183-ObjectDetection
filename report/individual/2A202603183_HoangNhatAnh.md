# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Hoàng Nhật Anh
- MSSV: 2A202603183
- Email: 26ai.anhhn2@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/nhatanhhoang2905/K5-L2-Day02-HoangNhatAnh-2A202603183-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1FnxC7Udw-VAIeabmqx0SJXwOHJoJr35H#scrollTo=8e715548

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

Dùng ngưỡng ghép cùng lớp IoU ≥ 0.50. Sau rework, FP giảm 30, nhưng TP giảm 4 và FN tăng 4; precision tăng từ 0.4901 lên 0.5038, recall giảm từ 0.7167 xuống 0.7106, F1 tăng nhẹ từ 0.5821 lên 0.5896, mean IoU tăng từ 0.8627 lên 0.8632. Mean IoU đạt ngưỡng 0.70 nhưng F1 vẫn thấp hơn ngưỡng 0.80, nên kết quả hiện tại vẫn `passed: false`. Số liệu trước rework lấy từ lần evaluation trước đã lưu trong báo cáo; JSON hiện tại là lần chạy sau rework. Cần căn cứ ảnh cụ thể để giải thích FP/FN, không suy nguyên nhân chỉ từ tổng số.

## 5. Kết quả YOLO

- **Runtime:** Link Colab đã ghi tại mục 1. Repo không lưu log phiên chạy, nên chưa xác nhận được loại GPU/CPU thực tế, phiên bản Torch hoặc phiên bản Ultralytics. Notebook cài Ultralytics mới nhất tại thời điểm chạy và tự chọn CUDA nếu có, nếu không thì CPU.
- **Cấu hình theo notebook:** Dùng `yolo26n.pt`, `epochs=50`, `imgsz=640`, `batch=-1`, `seed=20260909`; không thấy bằng chứng trong repo về thay đổi tham số.
- **Đường dẫn checkpoint theo cấu hình:** `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`. Không có checkpoint hoặc log để xác nhận file đã được tạo/lưu.

| Chỉ số trên 10 ảnh eval | Giá trị thực tế có thể xác minh |
|---|---:|
| Precision | Chưa có file metrics trong repo |
| Recall | Chưa có file metrics trong repo |
| mAP50 | Chưa có file metrics trong repo |
| mAP50–95 | Chưa có file metrics trong repo |

Workspace không có `outputs/model_eval_metrics.json`, `day2_colab_outputs.zip`, ảnh dự đoán, `results.csv` hoặc biểu đồ. Thư mục `report/individual/assets/` hiện chỉ có `.gitkeep`, nên chưa có ảnh eval, confusion matrix hay PR curve để dẫn hoặc phân tích. Không sử dụng các số liệu của mẫu báo cáo.

Trả lời bốn câu hỏi cuối notebook:

1. **Lớp recall thấp nhất và ảnh minh họa:** Chưa xác định được vì không có per-class metrics hoặc prediction outputs. Trong nhãn train hiện tại có 921 box: `car` 773 (83.9%), `truck` 21 (2.3%), `bus` 78 (8.5%), `van` 49 (5.3%). Số lượng lệch mạnh về `car` là dữ kiện về mất cân bằng tập train, nhưng không đủ để kết luận recall lớp nào thấp nhất.
2. **Nếu mAP50 cao nhưng mAP50–95 thấp:** Kiểm tra độ khít và vị trí bốn cạnh box: box có lấy thừa nền, cắt mất xe hoặc lệch khỏi vật thể không. IoU cao nhạy hơn với sai lệch hình học nhỏ.
3. **Lỗi class có thể liên quan guideline:** Chưa có confusion matrix/per-class output để xác định model nhầm lớp nào. Guideline ghi `drive_034.jpg` là ca cần phân biệt `car` và `truck`; đây là điểm cần đối chiếu với confusion matrix khi có, chưa phải bằng chứng model đã phân loại sai.
4. **Vì sao không train trên 10 ảnh eval:** Giữ eval tách khỏi tập train để không rò rỉ dữ liệu. Theo notebook, 10 ảnh này được dùng làm validation trong quá trình train và chọn `best.pt`; vì vậy chúng không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Self-QC cấu trúc | Cần phân biệt lỗi format với lỗi chất lượng annotation. | Chạy `python tools/check_yolo_labels.py`; đọc riêng `outputs/annotation_evaluation.json`. | Checker format đạt 50/50; gold QC sau rework vẫn chưa đạt vì F1=0.5896 < 0.80. |
| Gold QC và rework | Sau rework, FP giảm nhưng FN tăng; F1 chỉ tăng nhẹ và vẫn dưới ngưỡng. | So sánh hai lần evaluation và dùng danh sách `per_image` để ưu tiên mở ảnh có ít matches, ví dụ `drive_042` (1/4 gold match), `drive_018` (1/2), `drive_017` (2/4). | Đã giảm FP 30; cần tiếp tục rà từng ảnh/box để xác định nguyên nhân, vì JSON không nêu chi tiết lỗi từng box. |
| Train/eval YOLO | Thiếu log, metrics và hình ảnh output trong repo để xác minh kết quả model. | Đã kiểm tra notebook, `outputs/` và `report/individual/assets/`. | Không có `model_eval_metrics.json`, ZIP output, `results.csv` hoặc ảnh prediction trong các file hiện có; hardware và model metrics chưa xác minh được. |

## 7. Tự đánh giá theo rubric

- **Định dạng YOLO:** checker đạt 50/50 file, đúng cấu trúc theo script.
- **Độ đầy đủ, box và phân lớp:** gold QC hiện F1=0.5896 (dưới 0.80); mean IoU=0.8632 (trên 0.70). Kết quả tổng thể chưa đạt; cần tiếp tục kiểm unmatched boxes và xác nhận nguyên nhân trên ảnh.
- **Mini guideline:** có định nghĩa bốn lớp, bốn quy tắc chung và ba case theo ảnh. Case `drive_034.jpg` vẫn cần giữ nhất quán giữa mô tả mơ hồ và class `truck` trong nhãn.
- **Gold-set QC:** đã có JSON sau rework nhưng `passed=false`; tiêu chí F1 chưa đạt.
- **Pipeline YOLO:** notebook và link Colab có, nhưng metrics/artifacts không nằm trong repo, nên runtime, kết quả model và phần nhận xét trên ảnh eval chưa kiểm chứng được.

Đây là trạng thái theo bằng chứng trong workspace, không quy đổi thành điểm số.

## 8. Công cụ AI và tài liệu tham khảo

- **ChatGPT:** hỗ trợ đọc tài liệu Markdown, đối chiếu JSON gold QC, xem một số ảnh train và soạn/chỉnh lời văn cho guideline/báo cáo. Không sửa nhãn train hoặc nhãn gold. Học viên cần tự kiểm tra box, class và các phân tích trước khi nộp.
- **Công cụ gán nhãn và runtime:** tên công cụ gán nhãn, GPU/CPU và phiên bản Ultralytics/Torch không được lưu trong workspace; không tự ghi CVAT hay thông tin runtime khi chưa xác minh.
- **Tài liệu tham khảo:** `GUIDE.md`, `RULES.md`, `RUBRIC.md`, `CHECKPOINTS.md`, `SUBMISSION.md`, `notebooks/day2_annotation_to_yolo.ipynb` và `outputs/annotation_evaluation.json`. Không dùng nguồn ngoài repo trong phần phân tích này.
