# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người loại nhỏ, sedan, hatchback, SUV, taxi, xe bán tải (pickup cabin nhỏ). | Nhỏ hơn xe buýt/xe tải; xe đang đỗ vẫn tính là car; không xếp minivan vào đây nếu chưa rõ lớp. |
| `truck` | Xe tải chở hàng nặng, xe đầu kéo container | Thường có cabin và phần thùng chở hàng, không gán cho xe bán tải thông thường. |
| `bus` | Xe buýt, minibus chở nhiều hành khách. | Thân xe dài, khoang hành khách lớn. |
| `van` | Xe van/minivan chở hàng hoặc chở người. | Dáng thân hộp, cao hơn car, nhỏ gọn hơn bus. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật ôm sát toàn bộ phần thân xe nhìn thấy trong ảnh; không cắt mất phần nhìn thấy và không lấy thêm nhiều nền như mặt đường, bầu trời hay xe bên cạnh. Mỗi xe có một box riêng, kể cả xe cùng lớp hoặc nằm sát/chồng lên nhau; không gộp nhiều xe thành một box.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần xe còn nhìn thấy đủ để nhận diện chắc chắn là xe thuộc một trong bốn lớp. Box chỉ bao phần nhìn thấy được, không ước lượng phần nằm sau vật che. Nếu chỉ còn một mảnh quá nhỏ hoặc không thể phân biệt chắc lớp, không tự đoán; ghi tên ảnh và hỏi giảng viên.
- Vật thể bị cắt bởi rìa ảnh: nếu phần xe nằm trong khung vẫn nhận diện được, gán box quanh phần nhìn thấy và để cạnh box chạm đúng mép ảnh nơi xe bị cắt. Không suy đoán kích thước hay vẽ phần xe nằm ngoài ảnh. Nếu phần còn lại quá ít để xác định lớp, ghi lại case thay vì đoán.
- Vật thể rất nhỏ/mơ hồ: chỉ gán khi nhìn thấy đủ dấu hiệu để xác định đó là xe và phân biệt được `car`, `truck`, `bus` hoặc `van`; nếu không đủ chắc chắn thì không gán lớp theo suy đoán, hãy ghi mã ảnh và hỏi giảng viên.


## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_025.jpg` | Một xe con màu trắng ở góc dưới bên phải bị cắt bởi cạnh dưới ảnh. | Gán `car`; vẽ box quanh phần xe nhìn thấy và để cạnh dưới box chạm mép ảnh. | Phần xe còn lại nhận diện được; chỉ đánh dấu phần trong khung, không ước lượng phần nằm ngoài ảnh. |
| `drive_042.jpg` | Xe buýt hai tầng và xe chở hàng chất cao xuất hiện gần nhau, một phần bị hàng hóa che. | Vẽ box riêng; gán `bus` cho xe buýt và `truck` cho xe chở hàng nếu phần thân/cabin còn nhìn thấy đủ để nhận diện. | Hai xe là hai đối tượng thuộc schema; không gộp box. Phân lớp theo loại xe, không theo hàng hóa che khuất. |
| `drive_034.jpg` | Ở giữa ảnh có xe chuyên dụng màu trắng với cabin và sàn kéo; hình dáng có thể gây phân vân giữa `car` và `truck`. | Kiểm tra lại lớp với giảng viên nếu chưa phân biệt chắc; không chốt chỉ dựa vào mục đích sử dụng của xe. | Schema yêu cầu phân lớp nhất quán theo loại xe. Nhãn hiện tại của ảnh đang ghi `truck`, nên cần xác nhận quyết định này khi self-QC. |
