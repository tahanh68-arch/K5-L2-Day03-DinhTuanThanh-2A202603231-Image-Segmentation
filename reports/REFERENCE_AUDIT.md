# Kiểm tra tiers_gt.zip

Mục đích: kiểm tra gói reference và cách đọc dữ liệu có làm sai điểm do khác ảnh, nhãn hoặc kích thước hay không.

SHA-256 của ZIP: `bd25ce33fcbff1312ee635d1af23449a921abaf537df9ca6e99085cb46489f49`.

| Task | Ảnh trong ZIP | Bộ lớp | Tự so reference với chính nó |
| --- | --- | --- | ---: |
| easy_semantic | Trùng từng byte | Khớp tên lớp | 20.0 |
| medium_instance | Trùng từng byte | Khớp tên lớp | 32.0 |
| hard_panoptic | Trùng từng byte | Khớp tên lớp | 30.0 |

Tự so reference với chính nó: **82.0/82**. Đây là phép kiểm bộ đọc và công thức, **không phải điểm bài nộp**.

Lỗi kỹ thuật phát hiện: **0**.
Medium trong gói thiếu bảng màu so với classes.json của repo; danh sách lớp vẫn khớp. Scorer instance đọc tên category và segmentation, không dùng bảng màu, nên khác biệt này không làm giảm điểm.
Hard dùng các tên COCO gốc: tree-merged → vegetation, pavement-merged → sidewalk, building-other-merged → building, sky-other-merged → sky. Đối chiếu theo category ID của repo; không so theo thứ tự category.
Ảnh PNG panoptic khớp kích thước ảnh, segment ID và diện tích khai báo trong JSON. Ảnh semantic đúng kích thước; reference đã giải nén trùng từng byte với gói ZIP.

## Kết luận

Chưa thấy sai lệch kỹ thuật trong gói reference khiến điểm bị mất. Tại lần kiểm reference, điểm bài là 41,2/82. Sau khi người học sửa Hard và export lại ngày 13/11, điểm mới là 41,8/82 (xem SCORECARD). Thay đổi đến từ ZIP bài làm mới, không phải cộng điểm do nghi ngờ reference.
Kiểm tra này không chứng minh từng đường biên của đáp án luôn đúng hoặc gói do coach chính thức phát. Không có checksum/chữ ký công bố để xác thực nguồn. Nếu thấy vùng đáp án có vẻ sai, cần đối chiếu vùng cụ thể và đề nghị coach xem lại.
Không sửa ground truth, ZIP bài nộp, trọng số hay công thức chấm trong lần kiểm này. Các reference vẫn bị Git bỏ qua.
