# Thiết kế bài nộp LAB5 T4 — GenG

## Mục tiêu

Tạo bài nộp cho LAB5 track T4, gồm `spec.md` và prototype HTML chạy offline. Thiết kế tập trung vào tính trung lập khi xử lý khiếu nại “hàng không đúng mô tả”, đặc biệt khi mô tả sản phẩm đã bị sửa sau thời điểm đặt hàng.

## Cấu trúc deliverable

```text
spec.md
prototype/
├── index.html
└── README.md
```

`spec.md` bám đúng sáu phần nội dung của template và bổ sung phần demo. `prototype/index.html` là một file HTML/CSS/JavaScript không cần dependency. `prototype/README.md` hướng dẫn mở file và đánh số các khu vực cần chỉ trong demo.

## Nội dung thiết kế

### Spec

- Ghi lớp L3B, Zone A, Track T4, nhóm GenG và ba thành viên đã cung cấp.
- Phân công: Nghĩa phụ trách phạm vi/kịch bản, Quang phụ trách UX và mức tự chủ, Quân phụ trách prototype/demo.
- §2 có tối thiểu tám kịch bản, phủ đủ bốn lớp rủi ro và trỏ về tool/dữ liệu cụ thể của T4.
- Hai kịch bản đắt nhất là người bán đúng và mô tả đã bị sửa.
- §3 mô tả câu AI nói nguyên văn, căn cứ hiển thị, đường sửa, chuyển nhân viên và loại lỗi.
- §4 xếp mức cho toàn bộ tool: xin bằng chứng và chuyển trưởng nhóm tự làm; hoàn tiền cần duyệt; từ chối cần duyệt; phạt shop bị cấm.
- §5 liên kết prototype và ánh xạ các điểm HAX/PAIR vào số chú thích trên giao diện.
- §6 có ba tiêu chí Pass/Fail và §7 có kịch bản demo ca lạ.

### Prototype

Prototype mô phỏng ca mô tả đã bị sửa:

1. Người mua gửi khiếu nại và ảnh bằng chứng.
2. AI đối chiếu mô tả lúc đặt hàng với mô tả hiện tại, hiển thị trạng thái “chưa đủ căn cứ”.
3. AI tự gửi yêu cầu bổ sung bằng chứng.
4. Người mua có thể chuyển hồ sơ cho nhân viên; hồ sơ giữ nguyên tóm tắt và bằng chứng.
5. Nút hoàn tiền không thực hiện ngay; mở màn hình xác nhận với lý do, căn cứ và cảnh báo.
6. Prototype có nút chuyển sang ca người bán đúng để chứng minh AI không tự từ chối hoặc hoàn tiền.

Các chú thích chính:

- G10: hỏi/thu hẹp khi thiếu căn cứ.
- G11 và PAIR Explainability + Trust: hiển thị nguồn sự thật là mô tả tại thời điểm đặt.
- G8/G9: sửa, bỏ qua hoặc chuyển người dễ dàng.
- PAIR Errors + Graceful Failure: luôn có đường chuyển nhân viên.

## Ràng buộc và xác nhận

- Chỉ dùng dữ liệu dựng, không dùng đơn hàng hoặc thông tin cá nhân thật.
- Không gọi sản phẩm thật và không cần API key.
- Prototype phải bấm qua được ca khó từ đầu đến cuối.
- Mọi thao tác có ảnh hưởng tiền hoặc quyền lợi đều có bước xác nhận hoặc chuyển người.
- Không tạo nội dung ngoài phạm vi T4.

## Tiêu chí hoàn thành

- Repository có đúng `spec.md` và thư mục `prototype/` theo cấu trúc yêu cầu.
- Spec có đủ bằng chứng và lập luận cho các mục R1–R7.
- Prototype chạy bằng cách mở trực tiếp `prototype/index.html`.
- Các nút chính hoạt động và thể hiện ít nhất hai mức tự chủ.
