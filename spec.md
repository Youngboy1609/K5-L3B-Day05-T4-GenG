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

Quy ước lớp rủi ro: **A — thiếu căn cứ**, **B — nhầm dữ liệu/người dùng**, **C —
lỗi hệ thống**, **D — bối cảnh công bằng/quyền lợi**. “Nhãn hại” là hậu quả cần tránh.

| # / lớp | Kịch bản và dữ liệu còn thiếu | Tool T4 liên quan | Hành vi an toàn | Nhãn hại |
|---|---|---|---|---|
| 1 / A | Ảnh mờ, không thấy mã hàng; thiếu `anh_bang_chung` | `yeu_cau_bang_chung` | Hỏi ảnh cận cảnh, không kết luận | Mất quyền khiếu nại |
| 2 / A | Chỉ có câu “hàng sai”, thiếu mô tả đặt hàng | `chuyen_truong_nhom` | Hiển thị thiếu dữ liệu, chuyển người | Chậm hoàn tiền |
| 3 / B | Người mua tải nhầm ảnh của đơn khác | `yeu_cau_bang_chung` | Yêu cầu mã đơn/ảnh mới, giữ ảnh cũ không làm căn cứ | Từ chối nhầm |
| 4 / B | Người mua bấm “hoàn tiền” khi chưa đọc lý do | `soan_hoan_tien` | Mở bản nháp, bắt xác nhận trước khi gửi | Gửi sai yêu cầu |
| 5 / C | Lịch sử chỉnh sửa không tải được; thiếu `audit_log` | `chuyen_truong_nhom` | Báo lỗi nguồn, chuyển cùng snapshot hiện có | Kết luận thiếu căn cứ |
| 6 / C | Bộ phân loại nhầm “khác màu” thành “khác mẫu” | `yeu_cau_bang_chung` | Nêu giới hạn, xin ảnh/đối chiếu thủ công | Đổ lỗi oan shop |
| 7 / D **(đắt nhất)** | Bằng chứng cho thấy người bán đúng nhưng AI chỉ thấy lời báo | `tu_choi_khieu_nai` (không tự chạy) | Không từ chối; giải thích thiếu căn cứ và chuyển người | Mất quyền người mua hoặc oan shop |
| 8 / D **(đắt nhất)** | Mô tả bị sửa sau mua; thiếu snapshot tại thời điểm đặt | `soan_hoan_tien`, `chuyen_truong_nhom` | Lấy mô tả lúc đặt làm nguồn kiểm tra, xin audit và duyệt người | Hoàn sai hoặc đổ lỗi shop |
| 9 / D | Người mua đã mở trả hàng nhưng shop chưa nhận | `chuyen_truong_nhom` | Giữ trạng thái, không tạo yêu cầu trùng | Trễ xử lý, trùng hoàn tiền |

Hai ca đắt nhất là **người bán đúng** và **mô tả bị sửa sau mua**: một quyết định
sai có thể tước quyền người mua hoặc gây phạt/hoàn tiền oan cho shop; hậu quả tiền,
uy tín và khả năng khiếu nại đều khó đảo ngược. “Hàng sai, xử lý đi” không được chọn
vào top hai vì đây là ca thường gặp nhưng câu nói thiếu bằng chứng; tác hại trước mắt
thấp hơn nếu hệ thống chỉ xin thêm dữ liệu và chưa thực hiện giao dịch.

# §3. Xử lý hai ca đắt nhất

## 3.1 Người bán đúng

- **AI nói nguyên văn:** “Mình chưa đủ căn cứ để từ chối khiếu nại hay hoàn tiền.
  Mình sẽ giữ nguyên hồ sơ, xin ảnh mã sản phẩm và chuyển nhân viên kiểm tra.”
- **Người dùng thấy:** trạng thái `chưa đủ căn cứ`; lời báo của người mua, ảnh mock,
  lịch sử giao hàng và checklist còn thiếu; nút **Xin thêm bằng chứng** và **Chuyển
  nhân viên**, không có nút tự từ chối/tự hoàn.
- **Bước tiếp theo/người nhận:** người mua bổ sung ảnh; nếu vẫn mâu thuẫn, nhân viên
  nhận snapshot và được báo trong hộp thư nội bộ mock.
- **Đã chọn:** hỏi bằng chứng rồi chuyển người, vì có thể đảo lại. **Bị loại:** tự
  `tu_choi_khieu_nai` (mất quyền người mua nếu phân loại nhầm) và tự `soan_hoan_tien`
  (gây thiệt shop).
- **Loại lỗi:** lỗi bối cảnh (căn cứ đúng nằm ngoài lượt chat), có thể kèm lỗi hệ thống
  nếu ảnh không khớp.
- **HAX/PAIR:** chú thích 1 (G10, Explainability), 3 (G10), 4 (G8/G9, Graceful
  Failure); người dùng luôn có đường sửa/chuyển.
- **Không thể giải bằng câu chữ:** nếu snapshot hoặc mã hàng không tồn tại, lời giải
  thích hay hơn không tạo ra bằng chứng; phải có người và nguồn dữ liệu bổ sung.

## 3.2 Mô tả đã bị sửa sau khi mua

- **AI nói nguyên văn:** “Mô tả hiện tại khác mô tả tại thời điểm bạn đặt hàng. Mình
  chưa thể kết luận bản sửa có liên quan đến đơn này; mình đã đánh dấu điểm khác và
  đề nghị nhân viên kiểm tra lịch sử.”
- **Người dùng thấy:** hai phiên bản mock có timestamp, trạng thái `chưa đủ căn cứ`,
  mục thiếu `audit_log`, nút xin ảnh và **Soạn yêu cầu hoàn tiền** (chỉ bản nháp).
- **Bước tiếp theo/người nhận:** người mua kiểm tra bản tóm tắt; nhân viên nhận hai
  phiên bản, timestamp, ảnh và lý do. Chỉ sau khi duyệt, người mua mới gửi yêu cầu.
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

| Tool | Mức | Quyền duyệt | Cost-of-error rationale |
|---|---|---|---|
| `yeu_cau_bang_chung` | Tự làm | Không cần | Chỉ gửi yêu cầu, chưa phân xử tiền |
| `chuyen_truong_nhom` | Tự làm | Không cần | Có thể đảo lại, chỉ làm chậm xử lý nếu dư thừa |
| `soan_hoan_tien` | Cần người duyệt | Người mua trước khi gửi | Gửi sai gây mất tiền và hàng |
| `tu_choi_khieu_nai` | Cần người duyệt | Nhân viên/trưởng nhóm | Từ chối sai làm mất quyền lợi người mua |
| `phat_shop` | Cấm | Bộ phận chuyên trách ngoài luồng | Ảnh hưởng shop, không đủ căn cứ từ khiếu nại thường |

Tool khó nhất là `tu_choi_khieu_nai`: nó phải phân biệt “người bán đúng” với
“thiếu bằng chứng”, trong khi dữ liệu thời gian/ảnh có thể sai. Vì vậy AI không được
gọi tool này trong flow; chỉ người có hồ sơ đầy đủ mới duyệt. Test tối thiểu: (a) ca
người bán đúng phải giữ `chưa đủ căn cứ`, không lộ nút từ chối; (b) ca mô tả bị sửa
phải hiện hai timestamp, xin audit và chỉ mở bản nháp hoàn tiền; (c) ca ảnh mờ thông
thường phải gửi `yeu_cau_bang_chung`, không chuyển tiền. Mỗi test kiểm tra cả thông
báo, recipient và không có side effect ngoài mock.

**Lời hứa thay đổi:** “AI chuẩn bị và giải thích bước tiếp theo; AI không tự phán xử
khi căn cứ chưa đủ.”

# §5. Prototype và ánh xạ tiêu chí

Prototype offline: [prototype/index.html](prototype/index.html), dùng vanilla
HTML/CSS/JavaScript, không dependency, API key hay dữ liệu thật. Các chú thích đánh
số trên màn hình map như sau:

1. `Chưa đủ căn cứ` → **G10**, PAIR Explainability (nói rõ giới hạn).
2. `Mô tả tại thời điểm đặt hàng` → **G11**, PAIR Explainability/Trust (nguồn sự thật).
3. Checklist bằng chứng thiếu → **G10**, giúp hỏi/thu hẹp thay vì đoán.
4. Chuyển nhân viên và SLA → **G8/G9**, PAIR Graceful Failure (sửa và thoát an toàn).
5. Bản nháp hoàn tiền + xác nhận → **G8/G9**, PAIR Explainability (kiểm soát trước tác động).

# §6. Tiêu chí Pass/Fail

1. **Pass** nếu ca mô tả bị sửa hiển thị cả hai timestamp và `chưa đủ căn cứ`, **Fail**
   nếu chỉ dùng mô tả hiện tại hoặc tự kết luận.
2. **Pass** nếu bấm “Xin thêm bằng chứng” và “Chuyển nhân viên” tạo thông báo mock,
   giữ nguyên tóm tắt; **Fail** nếu mất bằng chứng hoặc không có đường chuyển.
3. **Pass** nếu “Soạn yêu cầu hoàn tiền” chỉ mở bản nháp và cần xác nhận rõ ràng;
   **Fail** nếu click đầu tiên gửi tiền hoặc ca người bán đúng có nút tự từ chối.

# §7. Kịch bản demo và giới hạn

1. **Nghĩa** giới thiệu §1–§2, chọn ca mô tả bị sửa; chỉ ra hai bản mô tả, dữ liệu
   mock và trạng thái chưa đủ căn cứ.
2. **Quang** trình diễn §3–§4: xin bằng chứng, mở bản nháp hoàn tiền nhưng chưa gửi,
   rồi chuyển nhân viên; giải thích vì sao `tu_choi_khieu_nai` cần duyệt.
3. **Quân** trình diễn §5–§6 trên [prototype/index.html](prototype/index.html):
   đổi sang ca người bán đúng, chỉ ra không có tự từ chối/tự hoàn, đọc ba Pass/Fail.

Demo không gọi Shopee thật, không phạt shop, không gửi tiền và không đại diện cho
quyết định pháp lý. Nếu nguồn audit không có, sản phẩm chỉ nêu thiếu dữ liệu và chuyển
người; không biến sự không chắc chắn thành một phán quyết.
