**Lớp:** L3B · **Bàn / zone:** Zone A · **Track:** T4 · **Nhóm:** GenG

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Hồ Trọng Nghĩa | 2A202603729 | Nhóm trưởng; §1–§2: lát cắt, phạm vi và kịch bản rủi ro |
| Phạm Đăng Quang | 2A202603760 | §3–§4: UX khi AI sai và mức tự chủ |
| Nguyễn Minh Quân | 2A202603747 | §5–§7: prototype HTML và demo |

# §1. Lát cắt, người dùng và phạm vi

Trong “Trò chuyện với Shopee”, người mua báo **hàng không đúng mô tả**. Luồng hiện có
chủ yếu theo menu, thường yêu cầu chọn đơn, lý do và gửi bằng chứng trước khi người
bán phản hồi; sàn xem xét nếu hai bên chưa thống nhất. AI bổ sung khả năng đọc khiếu
nại cùng mô tả tại lúc đặt, xin đúng bằng chứng còn thiếu, chuẩn bị yêu cầu để người
mua xem lại hoặc chuyển người — không thay quyết định phân xử. Mục tiêu là giúp người
mua biết bước tiếp theo mà không tự động gây thiệt cho người bán đúng. Đơn hàng, ảnh
và mốc thời gian dưới đây đều là dữ liệu mock.

Ngoài phạm vi: AI không phán quyết lỗi cuối cùng vì fixture có thể thiếu audit hoặc
phản hồi hai bên; không tự phạt/khóa shop vì đây là thẩm quyền điều tra riêng; không
tự gửi tiền hoặc hủy đơn vì tác động khó đảo. AI chỉ chuẩn bị, giải thích và giữ hồ sơ
cho người có thẩm quyền.

# §2. Rủi ro và kịch bản

Quy ước bám theo bốn lớp của đề: **① nguồn sự thật**, **② mơ hồ/thiếu thông tin**,
**③ ngoài phạm vi/thẩm quyền**, **④ đặc thù domain**. Nhãn hại dùng ba mức của đề:
**nhẹ**, **sửa được**, **không chấp nhận được**.

| # / lớp | Tin nhắn và cách AI có thể sai (bằng chứng T4) | Tool / hành vi an toàn | Mức hại |
|---|---|---|---|
| 1 / ② | “Ảnh hơi tối, tem size không rõ, giờ mình cần gửi thêm gì?” AI có thể coi ảnh do người mua gửi là đã xác nhận mã hàng; tool `yeu_cau_bang_chung` chỉ gửi mẫu xin, không tự xác minh ảnh. | Dùng `yeu_cau_bang_chung` xin ảnh cận tem/mã hàng, nêu rõ còn thiếu gì và chưa kết luận. | Sửa được |
| 2 / ② | “Hàng sai rồi, xử lý đi.” Thiếu mã đơn và điểm sai; AI có thể gọi `soan_hoan_tien` khi chưa biết đơn nào. | Không gọi tool tiền; hỏi mã đơn và chỗ khác mô tả. | Sửa được |
| 3 / ① **(đắt nhất)** | “Lúc mua ghi có logo, giờ shop sửa mô tả thành không có.” Nếu chỉ đọc mô tả hiện tại thay vì snapshot lúc đặt, AI có thể gọi sai `tu_choi_khieu_nai`; snapshot lúc đặt là dữ liệu chuẩn, còn `audit_log` không có trong fixture/tool T4 và phải được nhân viên kiểm tra thêm. | Dùng mô tả lúc đặt; nếu thiếu snapshot/audit thì xin dữ liệu và gọi `chuyen_truong_nhom`, không kết luận. | Không chấp nhận được |
| 4 / ② | “Đây là ảnh áo của mình” nhưng ảnh có mã đơn khác. AI có thể gắn nhầm ảnh vào hồ sơ rồi gọi `tu_choi_khieu_nai` hoặc `soan_hoan_tien`. | `yeu_cau_bang_chung`; xác nhận mã đơn, không dùng ảnh lệch mã làm căn cứ. | Sửa được |
| 5 / ① | “Shop cũng nhận gửi nhầm rồi.” AI chỉ có lời kể, chưa có phản hồi của shop hay ảnh; có thể gọi `soan_hoan_tien` chỉ dựa trên lời kể một phía. | Xin ảnh/mã hàng bằng `yeu_cau_bang_chung`, chờ phản hồi hai bên và chưa thực hiện `soan_hoan_tien`. | Không chấp nhận được |
| 6 / ③ | “Khóa shop này ngay, nó lừa đảo.” `phat_shop` có thể trừ điểm/khóa shop; khiếu nại thường không trao thẩm quyền này cho AI. | Không gọi `phat_shop`; giải thích giới hạn và chuyển bộ phận chuyên trách. | Không chấp nhận được |
| 7 / ③ | “Cho tôi số điện thoại chủ shop để tự nói chuyện.” Không tool T4 nào cho phép chia sẻ liên hệ; AI có thể làm lộ dữ liệu giữa hai bên. | Từ chối tiết lộ, giữ trao đổi trong nền tảng và đưa đường chuyển nhân viên. | Không chấp nhận được |
| 8 / ④ **(đắt nhất)** | “Shop nói tem và mã hàng khớp mô tả; tôi chưa gửi được ảnh rõ nhưng muốn xử lý ngay.” AI có thể xem lời shop là đủ và gọi `tu_choi_khieu_nai`, làm người mua mất quyền; hoặc bỏ qua lời shop, gọi `soan_hoan_tien` khiến shop mất tiền/hàng. | Xin bằng chứng bằng `yeu_cau_bang_chung`, giữ chưa đủ căn cứ và gọi `chuyen_truong_nhom`; không tự chấp nhận hay từ chối. | Không chấp nhận được — một phía có thể mất quyền hoặc tiền khó đảo |
| 9 / ④ | “Tôi đã mở yêu cầu trả hàng, shop chưa nhận; tạo thêm yêu cầu hoàn tiền giúp tôi.” AI có thể gọi `soan_hoan_tien` lần nữa khi yêu cầu cũ còn mở. | `chuyen_truong_nhom`; giữ trạng thái hiện tại, không tạo yêu cầu trùng. | Sửa được |
| 10 / ④ | “Shop không hoàn trong hôm nay tôi sẽ đăng bóc phốt; hoàn ngay cho tôi.” AI có thể coi lời đe dọa là bằng chứng và gọi `soan_hoan_tien`, hoặc bỏ qua quyền phản hồi của shop. | Không dùng lời đe dọa làm căn cứ; giữ trung lập, đối chiếu snapshot/bằng chứng, nêu hạn phản hồi mock của hai bên và chuyển nhân viên nếu còn tranh chấp. | Không chấp nhận được |

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
  Mình sẽ giữ nguyên hồ sơ, xin ảnh mã sản phẩm và chuyển trưởng nhóm kiểm tra.”
- **Người dùng thấy:** trạng thái `chưa đủ căn cứ`; lời báo của người mua, ảnh mock,
  lịch sử giao hàng, checklist còn thiếu, hạn gửi yêu cầu và thời gian còn lại tại
  mốc chat mock; nút **Xin thêm bằng chứng** và **Chuyển trưởng nhóm**, không có nút tự
  từ chối/tự hoàn.
- **Bước tiếp theo/người nhận:** người mua bổ sung ảnh mã hàng; người bán được mời
  phản hồi trước 18/05/2026 · 16:40 (mốc mock, ba ngày sau tin khiếu nại). Nếu shop
  chưa phản hồi hoặc hai bên còn mâu thuẫn, hồ sơ vẫn mở và chuyển trưởng nhóm; SLA
  nhân viên là 24 giờ từ lúc nhận snapshot, tách biệt với hạn của người mua và shop.
- **Đã chọn:** hỏi bằng chứng rồi chuyển người; các bước này chưa tác động tiền/quyền,
  còn hồ sơ có thể được bổ sung hoặc chuyển lại. **Bị loại:** tự
  `tu_choi_khieu_nai` (mất quyền người mua nếu phân loại nhầm) và tự `soan_hoan_tien`
  (gây thiệt shop).
- **Loại lỗi và cách sửa:** lỗi bối cảnh là chưa có ảnh rõ hoặc phản hồi shop nằm
  ngoài lượt chat; hỏi đúng dữ liệu còn thiếu, giữ nguyên quyền khiếu nại. Lỗi hệ thống
  là ảnh/audit không tải được; báo rõ nguồn nào chưa đọc được, giữ hồ sơ mở và chuyển
  nhân viên thay vì nói như thể người dùng chưa gửi gì.
- **Lỗi thao tác người dùng (#4 gửi ảnh thuộc đơn khác):** nói trung tính rằng ảnh
  chưa khớp mã đơn nên chưa dùng làm căn cứ; hướng dẫn mở đúng đơn và tải lại ảnh,
  đồng thời giữ ảnh cũ trong nhật ký để nhân viên có thể kiểm tra nếu cần. Không xem
  lỗi tải ảnh (lỗi hệ thống) là người mua gửi thiếu, và không suy ra họ gian lận.
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
  cho nhân viên; người bán có cơ hội phản hồi trước 18/05/2026 · 16:40 (mock). Hạn gửi
  yêu cầu mock là 21/05/2026 · 09:00; tại mốc chat 15/05/2026 · 16:40 còn 5 ngày
  16 giờ 20 phút. Nhân viên nhận hai phiên bản, timestamp, ảnh và lý do; SLA 24 giờ
  chỉ bắt đầu sau khi nhân viên nhận snapshot. Trễ hạn phía shop thì chuyển nhân viên,
  không tự động hoàn hoặc từ chối.
- **Đã chọn:** giữ mô tả lúc đặt làm nguồn sự thật, hỏi audit và chuyển người. **Bị
  loại:** lấy mô tả hiện tại để từ chối (có thể che giấu thay đổi) hoặc tự hoàn tiền
  (có thể trả sai).
- **Loại lỗi và cách sửa:** lỗi bối cảnh là đang so hai mốc mô tả khác nhau; dùng
  snapshot tại lúc đặt và yêu cầu người có thẩm quyền kiểm tra audit. Lỗi hệ thống là
  không tải được audit; nói rõ chưa đọc được lịch sử, không buộc tội shop, giữ hồ sơ
  mở và chuyển nhân viên. Hai lỗi không được gộp thành kết luận “shop đã sửa”.
- **HAX/PAIR:** chú thích 2 (G11, PAIR Explainability), 4 (G8/G9, Graceful Failure),
  5 (PAIR Explainability và quyền xác nhận).
- **Không thể giải bằng câu chữ:** thiếu bản ghi trước khi sửa là thiếu dữ liệu không
  thể suy ra; cần audit/nhân viên, không được bịa độ tin cậy.

# §4. Mức tự chủ và chi phí sai

| Tool | Mức | Ai duyệt / xem gì / quá hạn | Ai chịu gì, đảo được không, cái giá của việc chờ |
|---|---|---|---|
| `yeu_cau_bang_chung` | Tự làm | Không cần duyệt; người mua hoặc người bán thấy chính xác loại ảnh/mã hàng cần bổ sung. UI giữ yêu cầu 5 giây trước khi gửi và cho hủy trong khoảng đó. | Hủy được trước khi gửi; sau khi tin tới người nhận thì không thể thu hồi, cần gửi tin đính chính nếu cần. Xin nhầm làm người nhận tốn công; chỉ xin mục cụ thể, cho phép bỏ qua/chuyển người. Chờ duyệt mọi yêu cầu làm chậm việc thu thập bằng chứng. |
| `chuyen_truong_nhom` | Tự làm | Không cần duyệt; trưởng nhóm nhận snapshot, lời hai bên, hạn còn lại và danh sách dữ liệu thiếu. | Có thể thu hồi khi hồ sơ còn đang chờ nhân viên nhận; sau khi đã nhận thì không thể thu hồi bằng nút này. Chuyển thừa tăng hàng đợi. Không chuyển khi thiếu audit hoặc có mâu thuẫn sẽ làm hai bên chờ mà không có người giải quyết. SLA phản hồi mock 24 giờ tính từ lúc nhận snapshot; không thay hạn gửi yêu cầu của người mua hay hạn shop phản hồi. Prototype không mô phỏng sự kiện nhân viên nhận. |
| `soan_hoan_tien` | Cần người duyệt | Nhân viên xem mô tả lúc đặt, bằng chứng, phản hồi hai bên, số tiền và lý do; người mua xác nhận nội dung yêu cầu. Quá SLA thì nhắc/escalate, không tự hoàn. | Hoàn sai khó đảo, shop mất tiền/hàng; chờ duyệt làm người mua chờ tiền hoặc có nguy cơ lỡ hạn. Giữ hồ sơ mở và báo thời hạn, không tự chuyển tiền để cân bằng hai phía. |
| `tu_choi_khieu_nai` | Cần người duyệt | Nhân viên/trưởng nhóm xem cùng bộ hồ sơ hai phía và nguồn mô tả lúc đặt rồi mới gọi tool; AI không tự kích hoạt tool trong chat. Quá SLA thì chuyển cấp xử lý, không tự đóng. | Từ chối sai làm người mua mất quyền khiếu nại; chờ người làm chậm xử lý nhưng còn giữ quyền. Không để AI gọi tool khi chỉ có lời một phía hoặc thiếu snapshot. |
| `phat_shop` | Cấm trong flow này | Không có nút duyệt trong chat; bộ phận chuyên trách điều tra theo quy trình riêng. | Trừ điểm/khóa nhầm ảnh hưởng cả shop và khó đảo; chờ điều tra có thể chậm nhưng rẻ hơn trừng phạt dựa trên một khiếu nại chưa xác minh. |

**Tách bước trong thiết kế, không thêm tool backend:** tool gốc `soan_hoan_tien` trong đề có tác động chuyển tiền khó đảo nên được xếp cần người duyệt. Prototype mô phỏng hai bước giao diện — AI chuẩn bị bản nháp không side effect, người mua đọc/xác nhận gửi yêu cầu để nhân viên xem; chỉ sau khi nhân viên duyệt mới được thực hiện tác động tiền. Các nhãn `tao_nhap_yeu_cau_hoan_tien` và `gui_yeu_cau_hoan_tien` dưới đây là tên bước UX, không phải tool mới. Prototype không mô phỏng giao dịch tiền thật.

Tool khó xếp nhất là `tu_choi_khieu_nai`: tự làm ở #3 có thể lấy mất quyền người mua
nếu AI tin mô tả hiện tại thay snapshot; ở #8, chọn nhầm hoàn tiền thay vì từ chối có
thể làm shop mất tiền và hàng. Luôn chờ duyệt làm chậm ca rõ ràng như #1/#2; cấm AI
giữ được quyền khiếu nại nhưng tăng tải nhân viên. Nhóm chọn cần người duyệt trong
quy trình phân xử, nhưng cấm AI tự gọi tool trong lát cắt chat này. Nhóm xác nhận
không ghi lại/không nhớ lựa chọn xếp mức ban đầu của từng thành viên; spec chỉ ghi
quyết định chung, không gán lựa chọn cá nhân hồi tố.

Ở đây “cần người duyệt” là mức của tool trong quy trình phân xử: người có thẩm quyền
mới gọi `tu_choi_khieu_nai` sau khi xem hồ sơ; trong chat, AI không có quyền tự gọi.
Vì vậy nhóm vừa giữ đúng mức cần duyệt ở bảng tool, vừa không để nút từ chối xuất hiện
trên prototype.

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
HTML/CSS/JavaScript, không dependency, API key hay dữ liệu thật. Ba ảnh bằng chứng:
[ca mô tả bị sửa](prototype/evidence-1-description-changed.png),
[ca người bán đúng](prototype/evidence-2-seller-correct.png) và
[review yêu cầu hoàn tiền](prototype/evidence-3-refund-review.png). Ảnh 1 thể hiện
cửa sổ hủy yêu cầu bằng chứng; ảnh 2 thể hiện chuyển trưởng nhóm mock đang chờ;
ảnh 3 mở modal, với checkbox xác nhận chưa tích. Chú thích 1–4 ở case desk; chú thích
5 nằm cạnh nội dung bản nháp và xác nhận trong modal:

1. `Chưa đủ căn cứ` → **G10**, PAIR Explainability (nói rõ giới hạn).
2. `Mô tả tại thời điểm đặt hàng` → **G11**, PAIR Explainability/Trust (nguồn sự thật).
3. Checklist bằng chứng thiếu → **G10**, giúp hỏi/thu hẹp thay vì đoán.
4. Chuyển trưởng nhóm và SLA → **G8/G9**, PAIR Graceful Failure (sửa và thoát an toàn).
5. Bản nháp hoàn tiền + xác nhận → **G8/G9**, PAIR Explainability (kiểm soát trước tác động).

Nhóm chọn case desk kết hợp nguồn sự thật, bằng chứng và nút điều khiển trên một
màn hình, kèm modal riêng cho bước duyệt nháp. Phương án wizard nhiều màn hình bị
loại vì người demo phải đổi trang để đối chiếu timestamp; confidence gauge bị loại vì
prototype không có mô hình đã hiệu chuẩn, nên trạng thái “chưa đủ căn cứ” và checklist
nguồn/bằng chứng dễ kiểm chứng hơn.

# §6. Tiêu chí Pass/Fail

1. **Pass** nếu ca mô tả bị sửa hiển thị snapshot và mô tả hiện tại, `chưa đủ căn cứ`,
   hạn gửi yêu cầu mock cùng thời gian còn lại; **Fail** nếu chỉ dùng mô tả hiện tại,
   tự kết luận hoặc không cho biết hạn còn bao lâu.
2. **Pass** nếu yêu cầu bằng chứng chờ 5 giây và hủy được trước khi gửi, sau khi gửi
   nói rõ không thể thu hồi; chuyển nhân viên có thể thu hồi khi còn chờ trong mock và
   giữ nguyên tóm tắt. **Fail** nếu UI cho rằng tin đã gửi còn thu hồi được hoặc làm mất hồ sơ.
3. **Pass** nếu “Soạn yêu cầu hoàn tiền” chỉ mở bản nháp và cần xác nhận rõ ràng;
   **Fail** nếu click đầu tiên gửi tiền hoặc ca người bán đúng có nút tự từ chối.

**Chấm thử trên hai ca đắt nhất:**

| Tiêu chí | #3 Mô tả bị sửa | #8 Người bán đúng | Giới hạn còn lại |
|---|---|---|---|
| 1. Snapshot, trạng thái và hạn | Pass — prototype có snapshot, mô tả hiện tại, timestamp, hạn gửi yêu cầu mock và thời gian còn lại tại mốc khiếu nại mock. | Pass một phần — không kết luận, có đường chuyển người và thời hạn phản hồi shop mock; fixture chưa có phản hồi shop đầy đủ. | Chính sách thời hạn là mock, chưa kết nối nguồn thật; không được hiểu là chính sách Shopee. |
| 2. Xin bằng chứng/chuyển người | Pass trong mô phỏng — có 5 giây để hủy trước khi gửi; trạng thái đã gửi ghi rõ không thể thu hồi; chuyển hồ sơ tới trưởng nhóm có nút thu hồi khi còn chờ. | Pass trong mô phỏng — cùng cửa sổ hủy và thu hồi, có đường chuyển trưởng nhóm, không phán quyết tự động. | Không có hàng đợi hay sự kiện nhân viên nhận thật; SLA 24 giờ và trạng thái chuyển đều là mock. |
| 3. Nháp và xác nhận | Pass trong mô phỏng — mở bản nháp, cần xác nhận mới gửi yêu cầu chờ duyệt. | Pass — nút nháp hoàn tiền bị ẩn, không có nút tự từ chối. | Không mô phỏng quyền duyệt cuối hoặc giao dịch tiền thật. |

# §7. Kịch bản demo và giới hạn

1. **Nghĩa** giới thiệu §1–§2, chọn ca mô tả bị sửa; chỉ ra hai bản mô tả, dữ liệu
   mock và trạng thái chưa đủ căn cứ.
2. **Quang** trình diễn §3–§4: xin bằng chứng rồi hủy trong 5 giây; gửi lại và chờ
   đến khi UI nêu rõ tin đã tới người nhận, không thể thu hồi. Mở bản nháp hoàn tiền
   nhưng chưa gửi, chuyển hồ sơ mock rồi thu hồi khi còn chờ; giải thích prototype
   không có sự kiện nhân viên nhận và vì sao `tu_choi_khieu_nai` cần duyệt.
3. **Quân** trình diễn §5–§6 trên [prototype/index.html](prototype/index.html):
   đổi sang ca người bán đúng, chỉ ra không có tự từ chối/tự hoàn, hạn trả lời của
   shop và đường chuyển trưởng nhóm; đọc ba Pass/Fail.

**Chuẩn bị chất vấn chéo (kế hoạch, chưa phải kết quả demo):** mỗi thành viên tập
trả lời cho #8, #3 và #1 theo cùng một nhịp: ai chịu thiệt gì nếu sai → có đảo được
không → chờ người gây mất gì → vì sao mức tool đã chọn phù hợp → chỉ đúng chỗ trên
màn hình. Người nói phần prototype cũng cần giải thích cost-of-error trong §4; người
nói phần rủi ro cần tự bấm được đường xử lý và chú thích HAX/PAIR.

**Ca lạ dự kiến để diễn tập:** “Cho tôi số điện thoại chủ shop, tôi qua nói chuyện cho nhanh.”
Câu trả lời dự kiến: “Mình không thể chia sẻ thông tin liên hệ giữa hai bên. Mình có
thể giữ trao đổi trong hồ sơ khiếu nại hoặc chuyển nhân viên kiểm tra.” Không có tool
T4 cho phép chia sẻ liên hệ; nếu người xem đưa ca khác, nhóm nêu giới hạn, không bịa
tool/quyền và chuyển người khi ngoài thẩm quyền.

Repo hiện mới ghi kịch bản demo dự kiến; sau buổi trình bày nhóm cần ghi câu hỏi lạ
thực tế, màn hình/hành vi prototype đã cho thấy, và quyết định nào (nếu có) nhóm đổi.
Không ghi như đã diễn ra trước khi demo thật.

Demo không gọi Shopee thật, không phạt shop, không gửi tiền và không đại diện cho
quyết định pháp lý. Nếu nguồn audit không có, sản phẩm chỉ nêu thiếu dữ liệu và chuyển
người; không biến sự không chắc chắn thành một phán quyết.
