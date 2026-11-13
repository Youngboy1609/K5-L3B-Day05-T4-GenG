**Lớp:** L3B · **Bàn / zone:** Zone A · **Track:** T4 · **Nhóm:** GenG

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Hồ Trọng Nghĩa | 2A202603729 | Nhóm trưởng; §1–§2: lát cắt, phạm vi và kịch bản rủi ro |
| Phạm Đăng Quang | 2A202603760 | §3–§4: UX khi AI sai và mức tự chủ |
| Nguyễn Minh Quân | 2A202603747 | §5–§7: prototype HTML và demo |

# §1. Lát cắt, người dùng và phạm vi

Trong “Trò chuyện với Shopee”, người mua báo **hàng không đúng mô tả**. AI đối chiếu
mô tả tại lúc đặt, mô tả hiện tại, nội dung khiếu nại và bằng chứng; sau đó (1) hướng
dẫn tạo yêu cầu trả/hoàn tiền, (2) xin thêm bằng chứng, hoặc (3) chuyển nhân viên.
Mục tiêu là bảo vệ quyền lợi người mua mà không tự động gây thiệt cho người bán đúng.
Toàn bộ đơn hàng, ảnh và thời gian trong bài là dữ liệu mock.

Ngoài phạm vi: phán quyết cuối cùng về lỗi của shop; tự phạt/khóa shop; tự gửi tiền
hoặc tự hủy đơn. AI chỉ chuẩn bị, giải thích và giữ nguyên hồ sơ cho người có thẩm quyền.

# §2. Rủi ro và kịch bản

Quy ước bám theo bốn lớp của đề: **① nguồn sự thật**, **② mơ hồ/thiếu thông tin**,
**③ ngoài phạm vi/thẩm quyền**, **④ đặc thù domain**. Nhãn hại dùng ba mức của đề:
**nhẹ**, **sửa được**, **không chấp nhận được**.

| # / lớp | Tin nhắn và cách AI có thể sai (bằng chứng T4) | Tool / hành vi an toàn | Mức hại |
|---|---|---|---|
| 1 / ② | “Ảnh hơi tối, tem size không rõ, giờ mình cần gửi thêm gì?” AI có thể coi ảnh mờ là đủ căn cứ; `anh_bang_chung` chưa xác nhận mã hàng. | `yeu_cau_bang_chung`; xin ảnh cận tem, chưa kết luận. | Sửa được |
| 2 / ② | “Hàng sai rồi, xử lý đi.” Thiếu mã đơn và điểm sai; AI có thể gọi `soan_hoan_tien` khi chưa biết đơn nào. | Không gọi tool tiền; hỏi mã đơn và chỗ khác mô tả. | Sửa được |
| 3 / ① **(đắt nhất)** | “Lúc mua ghi có logo, giờ shop sửa mô tả thành không có.” Nếu chỉ đọc mô tả hiện tại thay vì snapshot lúc đặt, AI có thể gọi sai `tu_choi_khieu_nai`; snapshot lúc đặt là dữ liệu chuẩn, còn `audit_log` không có trong fixture/tool T4 và phải được nhân viên kiểm tra thêm. | Dùng mô tả lúc đặt; nếu thiếu snapshot/audit thì xin dữ liệu và gọi `chuyen_truong_nhom`, không kết luận. | Không chấp nhận được |
| 4 / ② | “Đây là ảnh áo của mình” nhưng ảnh có mã đơn khác. AI có thể gắn nhầm ảnh vào hồ sơ rồi gọi `tu_choi_khieu_nai` hoặc `soan_hoan_tien`. | `yeu_cau_bang_chung`; xác nhận mã đơn, không dùng ảnh lệch mã làm căn cứ. | Sửa được |
| 5 / ① | “Shop cũng nhận gửi nhầm rồi.” AI chỉ có lời kể, chưa có phản hồi của shop hay ảnh; có thể gọi `soan_hoan_tien` chỉ dựa trên lời kể một phía. | Xin ảnh/mã hàng bằng `yeu_cau_bang_chung`, chờ phản hồi hai bên và chưa thực hiện `soan_hoan_tien`. | Không chấp nhận được |
| 6 / ③ | “Khóa shop này ngay, nó lừa đảo.” `phat_shop` có thể trừ điểm/khóa shop; khiếu nại thường không trao thẩm quyền này cho AI. | Không gọi `phat_shop`; giải thích giới hạn và chuyển bộ phận chuyên trách. | Không chấp nhận được |
| 7 / ③ | “Cho tôi số điện thoại chủ shop để tự nói chuyện.” Không tool T4 nào cho phép chia sẻ liên hệ; AI có thể làm lộ dữ liệu giữa hai bên. | Từ chối tiết lộ, giữ trao đổi trong nền tảng và đưa đường chuyển nhân viên. | Không chấp nhận được |
| 8 / ④ **(đắt nhất)** | “Shop nói tem và mã hàng khớp mô tả; tôi chưa gửi được ảnh rõ nhưng muốn xử lý ngay.” AI có thể xem lời shop là đủ và gọi `tu_choi_khieu_nai`, làm người mua mất quyền; hoặc bỏ qua lời shop, gọi `soan_hoan_tien` khiến shop mất tiền/hàng. | Xin bằng chứng bằng `yeu_cau_bang_chung`, giữ chưa đủ căn cứ và gọi `chuyen_truong_nhom`; không tự chấp nhận hay từ chối. | Không chấp nhận được — một phía có thể mất quyền hoặc tiền khó đảo |
| 9 / ④ | “Tôi đã mở yêu cầu trả hàng, shop chưa nhận; tạo thêm yêu cầu hoàn tiền giúp tôi.” AI có thể gọi `soan_hoan_tien` lần nữa khi yêu cầu cũ còn mở. | `chuyen_truong_nhom`; giữ trạng thái hiện tại, không tạo yêu cầu trùng. | Sửa được |

Hai ca đắt nhất là **#8 (người bán đúng)** và **#3 (mô tả bị sửa sau mua)**: ở #8,
tự từ chối có thể tước quyền người mua, còn tự hoàn có thể làm shop mất tiền và hàng;
ở #3, dùng mô tả hiện tại thay cho snapshot có thể dẫn tới cùng hai hậu quả khó đảo.
Ca **#2 (“Hàng sai rồi, xử lý đi”)** suýt được chọn vì rất mơ hồ, nhưng nếu AI hỏi mã
đơn và chưa gọi tool thay đổi trạng thái thì hậu quả còn sửa được, thấp hơn việc chốt
sai một trong hai quyền lợi ở #3/#8.

# §3. Xử lý hai ca đắt nhất

## 3.1 Người bán đúng

- **Tin nhắn:** “Shop nói tem và mã hàng khớp mô tả; tôi chưa gửi được ảnh rõ nhưng muốn xử lý ngay.”
- **AI nói nguyên văn:** “Mình chưa đủ căn cứ để từ chối khiếu nại hay hoàn tiền.
  Mình sẽ giữ nguyên hồ sơ, xin ảnh mã sản phẩm và chuyển nhân viên kiểm tra.”
- **Người dùng thấy:** trạng thái `chưa đủ căn cứ`; lời báo của người mua, ảnh mock,
  lịch sử giao hàng và checklist còn thiếu; nút **Xin thêm bằng chứng** và **Chuyển
  nhân viên**, không có nút tự từ chối/tự hoàn.
- **Bước tiếp theo/người nhận:** người mua bổ sung ảnh mã hàng; người bán được mời
  phản hồi trong hồ sơ. Nếu vẫn mâu thuẫn, nhân viên nhận snapshot và được báo trong
  hộp thư nội bộ mock, với SLA phản hồi mô phỏng 24 giờ.
- **Đã chọn:** hỏi bằng chứng rồi chuyển người; các bước này chưa tác động tiền/quyền,
  còn hồ sơ có thể được bổ sung hoặc chuyển lại. **Bị loại:** tự
  `tu_choi_khieu_nai` (mất quyền người mua nếu phân loại nhầm) và tự `soan_hoan_tien`
  (gây thiệt shop).
- **Loại lỗi:** lỗi bối cảnh (căn cứ đúng nằm ngoài lượt chat), có thể kèm lỗi hệ thống
  nếu ảnh không khớp.
- **HAX/PAIR:** chú thích 1 (G10, Explainability), 3 (G10), 4 (G8/G9, Graceful
  Failure); người dùng luôn có đường sửa/chuyển.
- **Không thể giải bằng câu chữ:** nếu snapshot hoặc mã hàng không tồn tại, lời giải
  thích hay hơn không tạo ra bằng chứng; phải có người và nguồn dữ liệu bổ sung.

## 3.2 Mô tả đã bị sửa sau khi mua

- **Tin nhắn:** “Lúc mua ghi áo có logo, giờ shop sửa mô tả thành không có logo.”
- **AI nói nguyên văn:** “Mô tả hiện tại khác mô tả tại thời điểm bạn đặt hàng. Mình
  chưa thể kết luận bản sửa có liên quan đến đơn này; mình đã đánh dấu điểm khác và
  đề nghị nhân viên kiểm tra lịch sử.”
- **Người dùng thấy:** hai phiên bản mock có timestamp, trạng thái `chưa đủ căn cứ`,
  mục thiếu `audit_log`, nút xin ảnh và **Soạn yêu cầu hoàn tiền** (chỉ bản nháp).
- **Bước tiếp theo/người nhận:** người mua kiểm tra bản tóm tắt và xác nhận gửi yêu cầu
  cho nhân viên; người bán có cơ hội phản hồi. Nhân viên nhận hai phiên bản, timestamp,
  ảnh và lý do, với SLA mô phỏng 24 giờ. Quá hạn thì nhắc/escalate, không tự động hoàn
  hoặc từ chối.
- **Đã chọn:** giữ mô tả lúc đặt làm nguồn sự thật, hỏi audit và chuyển người. **Bị
  loại:** lấy mô tả hiện tại để từ chối (có thể che giấu thay đổi) hoặc tự hoàn tiền
  (có thể trả sai).
- **Loại lỗi:** lỗi bối cảnh (thời điểm pháp lý khác thời điểm đọc); lỗi hệ thống nếu
  audit không tải được.
- **HAX/PAIR:** chú thích 2 (G11, PAIR Explainability), 4 (G8/G9, Graceful Failure),
  5 (PAIR Explainability và quyền xác nhận).
- **Không thể giải bằng câu chữ:** thiếu bản ghi trước khi sửa là thiếu dữ liệu không
  thể suy ra; cần audit/nhân viên, không được bịa độ tin cậy.

# §4. Mức tự chủ và chi phí sai

| Tool | Mức | Ai duyệt / xem gì / quá hạn | Ai chịu gì, đảo được không, cái giá của việc chờ |
|---|---|---|---|
| `yeu_cau_bang_chung` | Tự làm | Không cần duyệt; người mua hoặc người bán thấy chính xác loại ảnh/mã hàng cần bổ sung. | Tin đã gửi không thu hồi được, nhưng chưa đổi tiền hay kết luận. Xin nhầm làm người nhận tốn công; chỉ xin mục cụ thể, cho phép bỏ qua/chuyển người. Chờ duyệt mọi yêu cầu làm chậm việc thu thập bằng chứng. |
| `chuyen_truong_nhom` | Tự làm | Không cần duyệt; trưởng nhóm nhận snapshot, lời hai bên và danh sách dữ liệu còn thiếu. | Có thể chuyển lại hồ sơ, nhưng chuyển thừa tăng hàng đợi. Không chuyển khi thiếu audit hoặc có mâu thuẫn sẽ làm hai bên chờ mà không có người giải quyết. SLA phản hồi mock 24 giờ; quá hạn nhắc/escalate, không tự phán quyết. |
| `soan_hoan_tien` | Cần người duyệt | Nhân viên xem mô tả lúc đặt, bằng chứng, phản hồi hai bên, số tiền và lý do; người mua xác nhận nội dung yêu cầu. Quá SLA thì nhắc/escalate, không tự hoàn. | Hoàn sai khó đảo, shop mất tiền/hàng; chờ duyệt làm người mua chờ tiền hoặc có nguy cơ lỡ hạn. Giữ hồ sơ mở và báo thời hạn, không tự chuyển tiền để cân bằng hai phía. |
| `tu_choi_khieu_nai` | Cần người duyệt | Nhân viên/trưởng nhóm xem cùng bộ hồ sơ hai phía và nguồn mô tả lúc đặt; quá SLA thì chuyển cấp xử lý, không tự đóng. | Từ chối sai làm người mua mất quyền khiếu nại; chờ người làm chậm xử lý nhưng còn giữ quyền. Không để AI gọi tool khi chỉ có lời một phía hoặc thiếu snapshot. |
| `phat_shop` | Cấm trong flow này | Không có nút duyệt trong chat; bộ phận chuyên trách điều tra theo quy trình riêng. | Trừ điểm/khóa nhầm ảnh hưởng cả shop và khó đảo; chờ điều tra có thể chậm nhưng rẻ hơn trừng phạt dựa trên một khiếu nại chưa xác minh. |

**Tách bước hoàn tiền:** tool `soan_hoan_tien` trong đề có tác động chuyển tiền khó đảo. Nhóm tách thành `tao_nhap_yeu_cau_hoan_tien` (AI chuẩn bị nội dung, không side effect) và `gui_yeu_cau_hoan_tien` (người mua xác nhận gửi hồ sơ cho nhân viên); chỉ nhân viên mới được duyệt rồi gọi tool chuyển tiền. Prototype chỉ mô phỏng bản nháp/yêu cầu, không mô phỏng chuyển tiền thật.

Tool khó xếp nhất là `tu_choi_khieu_nai`: tự làm có thể tước quyền người mua; luôn
chờ duyệt làm chậm ca rõ ràng; cấm AI giữ được quyền khiếu nại nhưng tăng tải nhân
viên. Nhóm chọn cần người duyệt trong quy trình phân xử, nhưng cấm AI tự gọi tool trong
lát cắt chat này. Hồ sơ hiện có quyết định nhóm đã chốt, không lưu phiếu xếp mức riêng
của từng thành viên nên không gán lựa chọn cá nhân hồi tố.

**Thử mức tự chủ trên ba kịch bản:**

| Kịch bản | Cái sai có bị chặn? | Có việc nào AI vẫn làm khi lẽ ra phải hỏi? | Người dùng bình thường có chờ vô ích? |
|---|---|---|---|
| #8 người bán đúng (đắt nhất) | Có: AI không gọi từ chối/hoàn; xin chứng cứ và chuyển trưởng nhóm. | Không; chỉ gửi yêu cầu bằng chứng và chuyển hồ sơ. | Không phải chờ duyệt để bổ sung ảnh; chỉ chờ phân xử nếu còn mâu thuẫn. |
| #3 mô tả bị sửa (đắt nhất) | Có: dùng snapshot lúc đặt; thiếu audit thì chuyển người, không hoàn/từ chối. | Không; AI chỉ soạn nháp/yêu cầu. | Không chờ ở bước tạo hồ sơ; nhân viên có SLA mock 24 giờ và escalation nếu quá hạn. |
| #1 ảnh mờ (bình thường) | Có: hỏi ảnh rõ hơn, không gọi tool tiền. | Không; xin đúng bằng chứng thiếu là việc mức thấp. | Không; không cần chờ trưởng nhóm duyệt yêu cầu ảnh. |

**Lời hứa thay đổi:** “AI chuẩn bị và giải thích bước tiếp theo; AI không tự phán xử
khi căn cứ chưa đủ.”

# §5. Prototype và ánh xạ tiêu chí

Prototype offline: [prototype/index.html](prototype/index.html), dùng vanilla
HTML/CSS/JavaScript, không dependency, API key hay dữ liệu thật. Hai ảnh đánh số:
[ca mô tả bị sửa](prototype/evidence-1-description-changed.png) và
[ca người bán đúng](prototype/evidence-2-seller-correct.png). Các chú thích trên ảnh map như sau:

1. `Chưa đủ căn cứ` → **G10**, PAIR Explainability (nói rõ giới hạn).
2. `Mô tả tại thời điểm đặt hàng` → **G11**, PAIR Explainability/Trust (nguồn sự thật).
3. Checklist bằng chứng thiếu → **G10**, giúp hỏi/thu hẹp thay vì đoán.
4. Chuyển nhân viên và SLA → **G8/G9**, PAIR Graceful Failure (sửa và thoát an toàn).
5. Bản nháp hoàn tiền + xác nhận → **G8/G9**, PAIR Explainability (kiểm soát trước tác động).

Nhóm chọn case desk kết hợp nguồn sự thật, bằng chứng và nút điều khiển trên một
màn hình, kèm modal riêng cho bước duyệt nháp. Phương án wizard nhiều màn hình bị
loại vì người demo phải đổi trang để đối chiếu timestamp; confidence gauge bị loại vì
prototype không có mô hình đã hiệu chuẩn, nên trạng thái “chưa đủ căn cứ” và checklist
nguồn/bằng chứng dễ kiểm chứng hơn.

# §6. Tiêu chí Pass/Fail

1. **Pass** nếu ca mô tả bị sửa hiển thị cả hai timestamp và `chưa đủ căn cứ`, **Fail**
   nếu chỉ dùng mô tả hiện tại hoặc tự kết luận.
2. **Pass** nếu bấm “Xin thêm bằng chứng” và “Chuyển nhân viên” tạo thông báo mock,
   giữ nguyên tóm tắt; **Fail** nếu mất bằng chứng hoặc không có đường chuyển.
3. **Pass** nếu “Soạn yêu cầu hoàn tiền” chỉ mở bản nháp và cần xác nhận rõ ràng;
   **Fail** nếu click đầu tiên gửi tiền hoặc ca người bán đúng có nút tự từ chối.

**Chấm thử trên hai ca đắt nhất:**

| Tiêu chí | #3 Mô tả bị sửa | #8 Người bán đúng | Giới hạn còn lại |
|---|---|---|---|
| 1. Snapshot và trạng thái | Pass — prototype hiện mô tả lúc đặt, mô tả hiện tại, timestamp và “chưa đủ căn cứ”. | Pass một phần — không kết luận và vẫn có đường chuyển người; fixture chưa có phản hồi shop đầy đủ. | Snapshot/audit là dữ liệu mock, chưa kết nối nguồn thật. |
| 2. Xin bằng chứng/chuyển người | Pass trong mô phỏng — hai hành động hiện thông báo và giữ hồ sơ. | Pass trong mô phỏng — có đường xin thêm ảnh/chuyển nhân viên, không phán quyết tự động. | Không có hàng đợi nhân viên thật; SLA 24 giờ là mock. |
| 3. Nháp và xác nhận | Pass trong mô phỏng — mở bản nháp, cần xác nhận mới gửi yêu cầu chờ duyệt. | Pass — nút nháp hoàn tiền bị ẩn, không có nút tự từ chối. | Không mô phỏng quyền duyệt cuối hoặc giao dịch tiền thật. |

# §7. Kịch bản demo và giới hạn

1. **Nghĩa** giới thiệu §1–§2, chọn ca mô tả bị sửa; chỉ ra hai bản mô tả, dữ liệu
   mock và trạng thái chưa đủ căn cứ.
2. **Quang** trình diễn §3–§4: xin bằng chứng, mở bản nháp hoàn tiền nhưng chưa gửi,
   rồi chuyển nhân viên; giải thích vì sao `tu_choi_khieu_nai` cần duyệt.
3. **Quân** trình diễn §5–§6 trên [prototype/index.html](prototype/index.html):
   đổi sang ca người bán đúng, chỉ ra không có tự từ chối/tự hoàn, đọc ba Pass/Fail.

**Ca lạ để diễn tập:** “Cho tôi số điện thoại chủ shop, tôi qua nói chuyện cho nhanh.”
Câu trả lời dự kiến: “Mình không thể chia sẻ thông tin liên hệ giữa hai bên. Mình có
thể giữ trao đổi trong hồ sơ khiếu nại hoặc chuyển nhân viên kiểm tra.” Không có tool
T4 cho phép chia sẻ liên hệ; nếu người xem đưa ca khác, nhóm nêu giới hạn, không bịa
tool/quyền và chuyển người khi ngoài thẩm quyền.

Demo không gọi Shopee thật, không phạt shop, không gửi tiền và không đại diện cho
quyết định pháp lý. Nếu nguồn audit không có, sản phẩm chỉ nêu thiếu dữ liệu và chuyển
người; không biến sự không chắc chắn thành một phán quyết.
