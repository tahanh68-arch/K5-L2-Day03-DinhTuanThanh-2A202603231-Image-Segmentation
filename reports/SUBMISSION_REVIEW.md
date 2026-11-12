# Kiểm tra bài nộp Day03

**Đủ 9 ZIP và đều qua kiểm tra cấu trúc.** Cp1 đã xuất lại, có 6 Polygon. Vị trí các vật Medium tự vẽ ban đầu đã được xác nhận bằng ảnh chụp CVAT; lỗi chồng lấn taxi–xe buýt chưa được sửa trong ZIP hiện tại.

Người học xác nhận các vật Medium tự vẽ ban đầu là hai người ngồi trên xe máy và chiếc xe máy trắng. Ảnh chụp CVAT khớp ảnh `000000181542.jpg`, với người phụ nữ đứng giữa và các xe máy ở tiền cảnh. Báo cáo đã cập nhật vị trí, lớp `person`/`motorcycle` và cách tách các vật. Ca sảnh tòa nhà có người vẫn cần tên ảnh cụ thể.

Kiểm tra trực tiếp `medium_instance.zip` phát hiện tại `000000181542.jpg`: annotation 19 (`car`) và 26 (`bus`) chồng 2.840 pixel ở phía trái ảnh, trong khung `(1,129)..(115,195)`. Ảnh gốc cho thấy taxi đứng trước xe buýt. Cần sửa trong CVAT, Save và export lại; hiện chưa sửa. Chi tiết: [annotation_overlap_review.json](annotation_overlap_review.json).

## Kết quả tự chấm

**Ba tier: 41,8/82**, không phải điểm chính thức /100. Không có reference 6 checkpoint.

| Task | Điểm |
| --- | ---: |
| easy_semantic | 17.6/20 |
| medium_instance | 13.8/32 |
| hard_panoptic | 10.4/30 |

## File đã kiểm tra

| ZIP | Annotation (nếu COCO) | SHA-256 |
| --- | ---: | --- |
| `easy_semantic.zip` | semantic PNG | `8c1c16a451b1b19121a94821571291a503a96464c2bb5d6748e7144084bd9efe` |
| `medium_instance.zip` | 84 | `82fc4c7e9aca83817934ef5be9ff69822a6e201c14e0fa3e2f02ab5990270abe` |
| `hard_panoptic.zip` | 51 | `84474d81e4c02059d3eb5409baa9826b1bb1c21641da63260fcafeaea950ebd9` |
| `cp1_holes.zip` | 6 | `a4734a39cf7864ea7660ff6b2cf505e102d29dcb547cffa942ac98bc9b530ec7` |
| `cp2_slice.zip` | 15 | `80a383e518540ff923dd08d8dc78bb9e83e2dd9c06585091f0362c7070e2ff74` |
| `cp5_occlusion.zip` | 35 | `5ca5246f0ca80461e543a78824847ac62e9adadb956d93fcd8ba5f2c08f7c4af` |
| `cp3_thin.zip` | semantic PNG | `cee04c56f62f280e01f8432f7a175c4197aa24626f1d3a0515bd54fec66c04de` |
| `cp4_curb.zip` | semantic PNG | `83ffc290910c426bcb1a06e1cd54add6d46150418c2bfcf454a590f80e411d94` |
| `cp6_coverage.zip` | semantic PNG | `e4054168925761815c10460b5dd28b2585fb8fd37e1aea1e4b54f7d63d2ef8a3` |

Các ZIP là file người học đã thêm; không chỉnh nội dung annotation bên trong ZIP.

## Việc còn cần làm

1. Báo cáo đã bổ sung tên ảnh/vị trí vật Medium tự vẽ ban đầu theo ảnh chụp. Phần công cụ liệt kê chính xác notebook/model đã được cung cấp, không nhận đó là nhật ký chạy đã xác thực. Ba ca trong bảng đều có tên ảnh/vị trí: taxi–xe buýt ở Medium, kính xe máy ở cp1 và dãy xe đỗ bên trái bị xe máy che ở cp5. Ca sảnh tòa nhà chưa xác định ảnh được giữ như ghi chú trao đổi, không gán tên ảnh tùy ý.

## File được tạo trong đợt kiểm này

- `report/individual/2A202603231_DinhTuanThanh.md`: báo cáo dựa trên file và trao đổi, đánh dấu thông tin chưa được xác nhận.
- `reports/submission_qc.json`: kết quả kiểm cấu trúc 9 ZIP.
- `reports/tiers/easy_semantic.json`, `medium_instance.json`, `hard_panoptic.json`: metric chi tiết.
- `reports/tiers/SCORECARD.md`, `reports/tiers/scorecard.json`: tổng hợp 41,8/82.
- `reports/SUBMISSION_REVIEW.md`: tài liệu kiểm tra này.
- `reports/annotation_overlap_review.json`: số pixel và tọa độ chồng lấn giữa các annotation Medium/Hard; không chứa mask ground truth.
- `reports/REFERENCE_AUDIT.md`, `reports/reference_audit.json`: kiểm tra gói reference và phép tự so reference 82/82; đây không phải điểm bài nộp.

Reference được giải nén cục bộ vào `data/tiers/*/groundtruth/` (đã xác nhận bị Git bỏ qua). Không thêm đáp án hay `tiers_gt.zip` vào commit. Không sửa mã chấm, class hoặc workflow.

## Cập nhật Hard ngày 13/11/2026

Đã nhận ZIP mới: 51 annotation (29 Polygon, 22 RLE). Hard tăng 9,8 → 10,4/30; tổng ba tier tăng 41,2 → 41,8/82. Kết quả từ script gốc, không sửa công thức chấm.
Các lớp sidewalk, person, bicycle, traffic light vẫn có PQ=0 theo scorer. Cần kiểm lại vùng/instance cụ thể trong CVAT.
