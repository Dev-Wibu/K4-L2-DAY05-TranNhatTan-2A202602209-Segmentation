# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602209
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, AI Tools (Segment Anything / SAM)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, vị trí người phụ nữ mặc áo dài trắng đi bộ qua đường ở chính giữa trung tâm ảnh.
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Tôi vẽ mask bám sát đường viền tà áo dài và chân theo phần thực tế nhìn thấy; dừng biên ở mặt đường tiếp xúc với chân, không tự suy đoán phần gót chân bị bóng mờ che; phần tà áo sau bay nhẹ vẫn giữ trọn trong mask của người.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Khi dùng SAM cho các phương tiện và người lái xe máy, SAM có xu hướng gộp chung người lái và chiếc xe máy thành một khối. Tôi đã dùng công cụ Brush/Polygon để tách riêng người lái (`person`) và thân xe máy (`motorcycle`) thành 2 instance độc lập theo đúng quy tắc.
- Nếu không dùng gợi ý: không áp dụng.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`, ảnh `000000181542.jpg` (khu vực vỉa hè phía sau trước cửa hàng Chanel và cửa sổ xe buýt).
- Lỗi thuộc loại: thiếu-thừa vật (False Positive - vẽ thừa đối tượng mờ ở xa).
- Bằng chứng tôi nhìn thấy: Ở lần export đầu tiên, bài nộp có tới 86 annotations (riêng ảnh 1 có tới 40 vật), trong khi ground truth chỉ có 20 vật cho ảnh này. Tôi nhận thấy mình đã vẽ cả những người đi bộ rất nhỏ li ti ở tít xa phía sau và hành khách ngồi sau kính xe buýt mờ.
- Quy tắc và hành động sửa: Áp dụng quy tắc chỉ gán nhãn cho các cá thể nhìn thấy rõ ràng và có thể phân định độc lập; hành khách ngồi trong xe buýt không tách thành instance riêng. Tôi đã mở lại job trên CVAT, xóa bớt các mask person li ti này, đưa tổng số vật của toàn bộ task về đúng 71 vật (khớp hoàn toàn với ground truth).
- Sau sửa đã Save và export lại chưa? Đã Save và export lại thành file `medium_instance.zip` mới trong thư mục `submissions/`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Sau khi sửa và chạy lại scorer, số lượng object khớp tuyệt đối (submitted 71 vs GT 71, count error = 0), Precision tăng từ 0.59 lên 0.70, số lượng FP giảm từ 35 xuống 21, chất lượng viền mean matched IoU đạt mức cao 0.770. Scorecard ba tier đạt 40.5 / 82.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp1_holes` (ảnh `000000144300.jpg`): Kính chắn gió và nan hoa mô tô | Khoét bỏ vùng kính chắn gió và nan hoa bánh xe vì nhìn xuyên qua thấy nền, hay giữ nguyên trong mask? | Quy tắc checkpoint: Kính xe, nan hoa, khe hở động cơ là một phần của vật thể, không khoét lỗ tùy tiện. | Quyết định giữ nguyên vẹn toàn bộ kính chắn gió và nan hoa bánh xe trong mask `motorcycle`, không khoét lỗ. |
| 2. `cp2_slice` (ảnh `000000017627.jpg`): Hai xe ô tô đỗ sát nhau ở bãi đỗ | SAM gộp 2 xe cạnh nhau (sedan đen và wagon trắng) thành 1 mask lớn hay tách rời? | Quy tắc instance: Hai vật cùng class nhưng là hai thực thể vật lý riêng biệt phải là 2 instance. | Quyết định dùng công cụ Slice (phím tắt Alt + J) cắt đôi đường biên tiếp giáp thành 2 mask `car` riêng. |
| 3. `cp4_curb` (ảnh `7d83710e-4697c3b2.jpg`): Ranh giới bó vỉa giữa đường và vỉa hè | Vùng mép đường sát vỉa hè cùng màu nhựa/bê tông tối: gán toàn bộ là `road` hay tách `sidewalk`? | Quy tắc semantic: Phân định ranh giới chức năng dựa vào gờ bó vỉa (curb) nhô cao hơn mặt đường xe chạy. | Quyết định phóng to 100%, lần theo mép gờ bó vỉa để gán phần lòng đường là `road` và phần gờ nâng cao là `sidewalk`. |
