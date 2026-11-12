# Báo cáo thực hành Day 3

- Họ tên: Đinh Tuấn Thành.
- Mã học viên: 2A202603231.
- Thời gian làm bài: 11-12/11/2026.
- Công cụ vẽ và xuất nhãn: CVAT local tại `localhost:8080`, với Polygon và mask.
- Hỗ trợ: ChatGPT hướng dẫn thao tác, chuẩn bị notebook gợi ý và kiểm tra file. Các notebook được cung cấp là `Hard_2_anh_Panoptic_CVAT.ipynb` (Mask2Former Swin Large COCO Panoptic).

## 1. Bài đã nộp

Tôi đã đưa đủ 9 ZIP vào thư mục `submissions/`. Các file đọc được và khớp ảnh, tên lớp theo task. Đây là trạng thái file trên máy; tôi chưa xác nhận đã nộp link lên LMS.

| Task | File ZIP | Số ảnh trong export | Điểm tối đa |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3/3 | 20 |
| medium_instance | `medium_instance.zip` | 3/3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2/2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1/1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1/1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1/1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1/1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1/1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1/1 | 3 |
| Tổng | | 14 ảnh | 100 |

Sau khi có `tiers_gt.zip`, bài được chạy bằng script chấm của repo. Kết quả ba phần là **41,8/82**: Easy 17,6/20; Medium 13,8/32; Hard 10,4/30. Đây là điểm tự đánh giá, không phải điểm chính thức trên 100. Chưa có reference của 6 checkpoint nên tôi không tự ghi điểm các phần đó.

## 2. Một quyết định trước khi dùng gợi ý

Ở ảnh **`000000181542.jpg`**, các vật đầu tiên tôi tự vẽ là **hai người ngồi trên xe máy và chiếc xe máy có phần vỏ trắng**. Hai người ngồi xe nằm ở hai bên người phụ nữ đang đi bộ giữa ảnh; chiếc xe máy trắng nằm phía bên phải. Tôi đã chỉ lại các vật này trong ảnh chụp CVAT.

Tôi chọn lớp `person` cho từng người và `motorcycle` cho xe máy, giữ mỗi vật là một object riêng. Dù người đang ngồi trên xe, tôi không gộp người và xe vào cùng một Polygon. Biên người đi theo phần cơ thể và quần áo nhìn thấy; biên xe đi theo phần xe còn lộ ra. Chỗ bị người phụ nữ đứng giữa che thì dừng ở mép phần nhìn thấy, không nối xuyên qua người đó.

Đây là quyết định tách người và xe ở phần tự vẽ ban đầu. Khi kiểm tra lại gợi ý, tôi cần giữ cách tách từng instance này và chỉnh phần biên bị ăn sang vật khác. Lỗi chồng lấn taxi–xe buýt ở mục 3 vẫn chưa được sửa trong export hiện tại.

## 3. Một lỗi tôi tìm thấy và sửa

Lần đầu, file `cp1_holes.zip` không có nhãn: COCO JSON có 0 annotation. Tôi đã thêm lại file export. Kiểm tra lần sau cho thấy file có **6 Polygon**, đúng ảnh và tên lớp. Như vậy lỗi export rỗng đã được khắc phục; chất lượng vùng vẽ cp1 chưa được chấm vì chưa có reference.

Ngoài ra, kiểm tra Medium phát hiện một lỗi **chưa sửa** ở ảnh `000000181542.jpg`: xe taxi bên trái chồng vùng nhãn lên xe buýt phía sau. Hai mask giao nhau 2.840 pixel, trong vùng tọa độ `(1,129)` đến `(115,195)`. Taxi nằm trước xe buýt nên cần sửa lại biên theo phần nhìn thấy, giữ hai vật riêng. Tôi chưa thay export Medium sau khi phát hiện lỗi này.

Easy còn yếu ở lớp vỉa hè, IoU khoảng 0,476. Medium còn mask thừa hoặc chưa khớp vật.

Ngày 13/11, tôi sửa Hard và thêm lại file export. Bản mới có 51 annotation (29 Polygon, 22 RLE). Điểm Hard tăng từ 9,8 lên 10,4/30; PQ tăng từ khoảng 0,346 lên 0,355. Người, xe đạp, đèn giao thông và vỉa hè vẫn chưa ghép đúng theo ngưỡng scorer nên còn cần xem lại; tôi chưa ghi các lỗi này là đã xử lý hết.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi |
| --- | --- | --- | --- |
| Medium, `000000181542.jpg`: taxi ở góc trái, phía trước xe buýt; vùng `(1,129)`–`(115,195)` | Giữ cả hai đường bao chồng nhau / cắt mask xe buýt theo phần taxi che | Ảnh gốc cho thấy taxi ở trước. Kiểm tra ZIP phát hiện hai mask giao nhau 2.840 pixel | Cần giữ hai instance riêng và bỏ phần xe buýt bị taxi che; đây là lỗi còn phải sửa trong CVAT |
| cp1, `000000144300.jpg`: kính chắn gió và phần bên trong xe máy | Giữ trong mask / tẩy thành lỗ | Tôi đã hỏi lại vì chưa hiểu quy tắc giữ kính và chi tiết bên trong | Theo quy tắc cp1, không tự khoét kính/khe thành lỗ. File mới đã có 6 Polygon; cần kiểm vùng vẽ trong CVAT |
| cp5, `000000336232.jpg`: dãy ô tô đỗ sát nhau bên trái; người và xe máy đi ngang ở tiền cảnh che một phần dãy xe | Tô gộp dãy xe / tách từng xe và chỉ tô phần còn nhìn thấy | Các xe đỗ sát nhưng vẫn là những vật khác nhau; xe máy và người lái ở phía trước cũng có lớp riêng | Mỗi xe là một instance. Không tô xuyên người hoặc xe máy. Nếu một xe có nhiều mảng nhìn thấy rời thì giữ chúng trong cùng một mask |

Tôi còn từng phân vân ở chỗ sảnh tòa nhà có người đứng phía trước, nhưng chưa lưu tên ảnh nên không dùng trường hợp đó làm một trong ba ví dụ cụ thể trên. Quy tắc cần giữ là không tô `building` đè lên phần người nhìn thấy.
