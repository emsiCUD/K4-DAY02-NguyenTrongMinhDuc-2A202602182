# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyễn Trọng Minh Đức<br>
**MSSV:** 2A202602182<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** `SOLO`

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp                 | Gán khi nhìn thấy                                         | Không gán vào lớp này                               |
| --: | -------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
|   0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con  | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
|   1 | `truck` (xe tải)  | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối       |
|   2 | `bus` (xe buýt)   | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế       | xe van nhỏ; xe tải; ô tô con                         |
|   3 | `van` (xe van)     | thân hộp nhỏ, kín, dùng chở người hoặc hàng        | thân xe buýt; khoang hàng tách biệt như xe tải    |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính                             | Giá trị                                                         | Ý nghĩa                                   |
| ---------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------- |
| `visibility` (mức nhìn thấy)        | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy               |
| `boundary` (quan hệ mép ảnh)        | `inside` (trong ảnh), `truncated` (bị cắt)                 | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại)         | đánh dấu quyết định cần quay lại    |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

> **Ghi chú về thời điểm — đọc trước.** Phiếu yêu cầu hoàn thành phần này trước khi xem bài của người khác hoặc bộ nhãn tham chiếu. Tôi đã không làm đúng thứ tự đó: ba mô tả dưới đây được viết **sau** khi đã chạy bước đối chiếu. Ba quyết định gán nhãn thì có từ trước và kiểm lại được trong gói xuất mang mã băm ghi ở báo cáo, nhưng phần diễn giải bằng chữ là viết sau. Tôi ghi rõ ở đây thay vì để trống hoặc để người đọc tự hiểu nhầm là viết trước.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: `drive_038`, hộp thứ 8 trong tệp nhãn — `xyxy ≈ (493, 109, 582, 209)`, kích thước 89×100 điểm ảnh.
- Dấu hiệu nhìn thấy: thân đỏ liền một khối, mui vươn cao hơn nóc ca-bin, một dãy ba ô cửa sổ hành khách ở hông, ca-bin và khoang sau không có khe tách, một trục sau đơn, chiều dài thân chỉ nhỉnh hơn chiếc xe con đứng cạnh. Phần đầu xe bị một xe tải chở hàng phía trước che một phần.
- Quy tắc áp dụng: mục 2. Dãy cửa sổ hành khách là dấu hiệu chung của cả `bus` lẫn `van` nên không dùng để phân biệt được. Dấu hiệu thật sự phân biệt là chiều dài thân và kiểu thân: `bus` cần "thân xe khách dài" và loại trừ "xe van nhỏ"; `van` là "thân hộp nhỏ, kín, dùng chở người hoặc hàng". Đây là thân hộp đặt trên khung gầm xe tải nhẹ, không phải thân xe khách.
- Quyết định: `van`, với `visibility = occluded`, `boundary = inside`, `review_state = confident`.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Giữ hộp nhưng chuyển `review_state` sang `needs_review` và ghi lý do vào nhật ký quyết định, rồi quay lại sau khi đã xử lý các hộp rõ ràng. Nếu đến cuối vẫn không đủ căn cứ thì hỏi Lab Coach chứ không chọn bừa một trong hai lớp cho đủ số.

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: `drive_022`, hộp thứ 5 — `xyxy ≈ (573, 258, 629, 329)`, kích thước 56×70 điểm ảnh.
- Dấu hiệu nhìn thấy: ca-bin trắng thấp nằm riêng phía trước, phía sau là một thùng kín hình khối chữ nhật cao hơn hẳn nóc ca-bin, giữa ca-bin và thùng có khe hở nhìn thấy được. Gầm cao, bánh sau lộ ra bên dưới thùng.
- Quy tắc áp dụng: mục 2. Ranh giới giữa `truck` và `van` nằm ở chỗ khoang hàng có tách rời ca-bin hay không: `truck` gán khi thấy "thùng, ben, sàn hàng… rõ ràng" và loại trừ "xe van kín một khối"; `van` loại trừ "khoang hàng tách biệt như xe tải". Khe hở giữa ca-bin và thùng là dấu hiệu quyết định.
- Quyết định: `truck`, với `visibility = clear`, `boundary = inside`, `review_state = confident`.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Phóng 100% tìm đúng một dấu hiệu: khe nối giữa ca-bin và khoang hàng. Thấy khe thì là `truck`, thấy thân liền một khối thì là `van`. Nếu góc chụp che mất chỗ đó thì đánh `needs_review` và ghi rõ dấu hiệu nào đang thiếu, thay vì suy đoán theo kích thước xe.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: `drive_008`, hộp thứ 28 — `xyxy ≈ (204, 0, 217, 10)`, kích thước 13×10 điểm ảnh, nằm sát mép trên của ảnh.
- Dấu hiệu nhìn thấy khi phóng 100%: một mảng sẫm nhoè trên mặt đường ở xa. Không đọc được đường viền thân xe, không phân biệt được kính, bánh hay khoang hàng. Phần trên của vật bị mép ảnh cắt.
- Giá trị `visibility`: `unclear` — bằng chứng nhìn thấy không đủ để mô tả hình dáng, chứ không phải bị vật khác che.
- Giá trị `boundary`: `truncated` — cạnh trên của hộp nằm đúng tại `y = 0`.
- Trạng thái `review_state`: `needs_review`.
- Lý do: đây là chỗ tôi tự thấy chưa nhất quán với chính quy tắc của mình. Mục 1 nói vật quá nhỏ hoặc mờ đến mức không phân lớp có căn cứ thì không đoán; nhưng tôi đã gán `car` kèm `needs_review` thay vì bỏ trống và ghi vào nhật ký. Hai cách xử lý cho ra hai kết quả khác nhau khi đối chiếu: gán kèm cờ thì tạo một hộp có thể lệch lớp, bỏ trống thì tạo một vật thiếu. Tôi giữ nguyên trạng thái hiện tại, ghi rõ ở đây, và chuyển câu hỏi sang mục 7 của báo cáo để hỏi Lab Coach ngưỡng đúng là ở đâu. Năm hộp `needs_review` còn lại đều cùng dạng: nhỏ dưới 25×25 điểm ảnh hoặc bị mép ảnh cắt.

## 6. Xác nhận tự kiểm tra

- [X] Đã rà đủ bốn ảnh.
- [X] Đã kiểm vật thể thiếu và trùng. — kiểm trùng đạt: không cặp hộp nào chồng nhau quá IoU 0.5. Kiểm thiếu chưa đạt: bước đối chiếu tìm ra một chiếc `van` bị bỏ sót ở giữa khung `drive_008`.
- [X] Đã kiểm lớp và hình học từng hộp. — 13 hộp không thuộc lớp `car` đã được xem lại từng cái ở độ phóng đầy đủ.
- [X] Mỗi hộp có đủ ba thuộc tính. — 99/99 hộp có đủ ba giá trị hợp lệ. Lưu ý: đủ không có nghĩa là đúng — 8 hộp có giá trị `boundary` sai, ghi ở mục 3 của báo cáo.
- [X] Đã xử lý mọi hộp `needs_review`. — còn 6 hộp.
- [X] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu. — viết sau, xem ghi chú đầu mục 5.
- [X] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi. — không áp dụng, làm cá nhân.
- [X] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu. — hai gói xuất đã được kiểm và ghi mã băm trước khi mở gói tham chiếu.
- [X] Số vật thể thực tế: **99** — 40–60 là mục tiêu khối lượng, không phải điểm cắt.
