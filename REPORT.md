# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Trọng Minh Đức<br>
**MSSV:** 2A202602182<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** `SOLO`

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: `f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33`
- Bốn mã ảnh: `drive_008`, `drive_022`, `drive_033`, `drive_038`
- Số vật thể thực tế: **99** (drive_022: 5 — drive_033: 29 — drive_038: 37 — drive_008: 28). Mục tiêu khối lượng của slide là 40–60; kết quả kiểm ghi `within_slide_workload_target: false`, tức nằm ngoài khoảng mục tiêu chứ không phải điểm cắt đạt/không đạt. Ô 3a của sổ thực hành in cảnh báo "CẦN KIỂM TRA: số hộp ngoài khoảng 40–60. Ghi số thật và rà phạm vi; không vẽ ẩu để đủ số" — tôi ghi số thật và phần rà phạm vi nằm ở mục 6.
- Mã SHA-256 của gói YOLO của bạn (`lab_2_images_YOLO.zip`): `b5121801d3c54c7cfca716a426ae898b0c95954f49b6d836ec99586d064ae681`
- Mã SHA-256 của gói CVAT gốc của bạn (`lab_2_images_CVAT.zip`): `b656cd7e18026bed75236b886b3ccfd98d78e089ac9f569c4895c3ccc5eb91f0`
- Nguồn đối chiếu: bộ tham chiếu do người hướng dẫn thực hành cấp (`release_id: day2-reference-4img-v1`, commit nguồn `710d2c1`)
- Mã SHA-256 của gói đối chiếu (`day2-teaching-reference.zip`): `c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b`
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: `day2-reference-4img-v1` — thời điểm: [CẦN #3]

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:

Chuỗi bằng chứng khép kín ở ba mức. Thứ nhất, ảnh gốc không bị đụng vào: bốn ảnh trong gói YOLO của tôi, trong gói CVAT của tôi và trong gói tham chiếu đều băm ra cùng một giá trị, và bốn giá trị đó trùng đúng với bảng `IMAGE_ATTRIBUTION.md` (ví dụ `drive_008` = `fde088a7…d67a05d1`). Thứ hai, hai gói xuất của tôi là hai cách viết của cùng một trạng thái gán nhãn: 99 hộp ghép được đôi một với IoU thấp nhất 0.99992 trên ngưỡng 0.995, và từng chỉ số lớp trong tệp YOLO khớp với tên lớp trong `annotations.xml` — không có lần sửa nào chen vào giữa hai lần xuất. Thứ ba, hai mã băm đó đã được ghi vào kết quả kiểm trước khi tôi mở gói tham chiếu, nên mọi thay đổi sau khi đối chiếu sẽ làm lệch mã băm và bị phát hiện ngay.

Hình thức cá nhân không phải tự khai: ô 5a của sổ thực hành chỉ đặt `comparison_source = "teaching_reference"` khi người chạy chọn phương án 1 (cá nhân), còn phương án 2 (theo cặp) sẽ ghi `peer`. Bản tóm tắt đối chiếu đã nộp ghi `teaching_reference`.

Giới hạn của lập luận này: mã băm chứng minh nội dung không đổi, không chứng minh thứ tự thời gian. Mốc thời gian nhận bộ tham chiếu ở trên mới là phần đóng lại lỗ hổng đó. [CẦN #3]

## 2. Quyết định phân lớp

Bảng dưới là 13 hộp không thuộc lớp `car` trong số 46 hộp ghép được — đúng những chỗ bản tóm tắt đối chiếu báo là bất đồng. Cột dấu hiệu nhìn thấy được mô tả từ ảnh cắt ở độ phóng đầy đủ; bạn nên mở `overlay/` kiểm lại từng dòng trước khi nộp.

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| `drive_022` / hộp 1 | `bus` | xe khách khớp nối hai khoang, sơn vàng–trắng–xanh, hàng cửa sổ chạy suốt thân | mục 2 `bus`: thân xe khách dài, nhiều cửa sổ |
| `drive_033` / hộp 1 | `bus` | thân xe khách đỏ–trắng chiếm gần hết khung, dãy cửa sổ liên tục | mục 2 `bus` |
| `drive_033` / hộp 8 | `bus` | mảng thân xe khách xanh–trắng, cửa sổ cao và dài | mục 2 `bus` |
| `drive_008` / hộp 22 | `bus` | xe khách khớp nối vàng–xanh, hai khoang nối bằng khớp xếp | mục 2 `bus` |
| `drive_022` / hộp 5 | `truck` | ca-bin tách rời, thùng kín dạng khối chữ nhật đặt trên khung gầm, gầm cao | mục 2 `truck`: thùng/ben/sàn hàng rõ ràng |
| `drive_033` / hộp 2 | `truck` | ca-bin trắng, sàn chở hàng phía sau có thành chắn | mục 2 `truck` |
| `drive_038` / hộp 1 | `truck` | xe công vụ cứu hộ, có cần cẩu và thiết bị lắp sau ca-bin | mục 2 `truck`: thiết bị công vụ rõ ràng |
| `drive_038` / hộp 9 | `truck` | xe tải xanh, sàn hàng phủ bạt, ca-bin tách khỏi khoang hàng | mục 2 `truck` |
| `drive_008` / hộp 23 | `truck` | xe ben đỏ chở đất, thùng ben rõ, nhiều cầu | mục 2 `truck` |
| `drive_038` / hộp 8 | `van` | thân hộp nhỏ màu đỏ, kín một khối, không có khoang hàng tách rời | mục 2 `van`: thân hộp nhỏ, kín |
| `drive_008` / hộp 24 | `van` | xe chở khách nhỏ thân hộp trắng, một khối liền từ ca-bin ra sau | mục 2 `van` |
| `drive_008` / hộp 25 | `van` | xe thân hộp trắng tương tự, kính bên liền dãy | mục 2 `van` |
| `drive_022` / hộp 4 | `van` | thân hộp trắng cao, kín, mui vươn cao hơn ca-bin — **hộp duy nhất còn bất đồng thật sự sau khi sửa lỗi thứ tự lớp ở mục 6; bộ tham chiếu ghi `car`** | mục 2 `van` |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

`drive_022` hộp 4: lớp `van`, `visibility = occluded`, `boundary = inside`, `review_state = confident`. Thân xe bị cột biển báo che một phần theo chiều dọc nên mức nhìn thấy là `occluded`; nhưng phần còn thấy đã đủ để chốt lớp nên trạng thái xem lại vẫn là `confident`. Hai trục thông tin chạy độc lập: `occluded` không hạ `van` xuống `car`, và ngược lại, `drive_008` hộp 28 có `visibility = unclear` nhưng vẫn được gán `car` kèm `needs_review` — mức nhìn thấy nói điều kiện quan sát, lớp nói vật đó là gì, trạng thái xem lại nói tôi có định quay lại hay không. Trên toàn bài: 59 `clear` / 27 `occluded` / 13 `unclear`, 85 `inside` / 14 `truncated`, 93 `confident` / 6 `needs_review`. Định dạng YOLO không lưu được cả ba trục này — đó là lý do phải xuất thêm `CVAT for images 1.1`.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| `drive_008`: không có hộp nào phủ chiếc xe thân hộp bạc ở giữa khung, quanh `xyxy ≈ (284, 249, 348, 351)` | phạm vi — vật thể thiếu | thấy ngay trên `comparison_overlay.png`: chiếc xe đó chỉ có một khung xanh của bộ tham chiếu và không có khung đỏ nào của tôi; hộp gần nhất của tôi chỉ phủ 9.8% diện tích và đã ghép với vật khác | thêm một hộp `van`; quy tắc phát sinh: rà theo lưới trước khi xuất, vì vật bị bỏ sót nằm ngay giữa khung chứ không ở rìa |
| `drive_033` hộp 1 (`bus`) ghi `boundary = inside`, cùng 6 hộp khác ghi `inside` dù chạm mép, và `drive_008` hộp 26 ghi `truncated` dù cách mép 36 px — tổng cộng 8 hộp | thuộc tính | rà lượt hai sau bước đối chiếu: `boundary` chỉ phụ thuộc vào việc hộp có chạm mép ảnh hay không, nên lọc toàn bộ 99 hộp rồi so toạ độ với kích thước ảnh là ra ngay; hộp `drive_033` số 1 bắt đầu đúng tại `x = 0` và kết thúc đúng tại `y = 640` | 7 hộp đổi sang `truncated`, hộp `drive_008` số 26 đổi sang `inside`; quy tắc phát sinh: `boundary` phải được kiểm bằng toạ độ chứ không bằng mắt, và nên chạy phép kiểm này như bước bắt buộc trước mỗi lần xuất |

Trạng thái tại thời điểm nộp: cả hai dòng trên đều được phát hiện ở lượt rà lại sau bước đối chiếu và **chưa được sửa** trong gói xuất mang mã băm ghi ở mục 1. Tôi ghi rõ thay vì sửa tệp nhãn bằng tay, đúng yêu cầu của bước 6 trong sổ thực hành. Ba việc còn tồn và giá trị đích của từng việc:

1. Thêm một hộp `van` ở `drive_008` quanh `xyxy ≈ (284, 249, 348, 351)`, thuộc tính `clear/inside/confident`. Số vật thể sẽ thành 100.
2. Đổi `boundary` sang `truncated` cho `drive_033` hộp 1, `drive_038` hộp 4, 6, 7, 17, và `drive_008` hộp 13, 27; đổi sang `inside` cho `drive_008` hộp 26. Số hộp `truncated` sẽ từ 14 lên 21, số vật thể không đổi.
3. Quyết định giữ hoặc bỏ từng hộp trong 6 hộp `needs_review` theo mục 1 phiếu quy tắc, đưa con số đó về 0. Việc này có thể làm đổi số vật thể nếu bỏ các hộp quá nhỏ.

Khi ba việc xong thì phải xuất lại cả hai định dạng từ cùng một công việc, chạy lại sổ thực hành, rồi cập nhật mã băm và số vật thể ở mục 1, bảng ở mục 3 và toàn bộ số đo ở mục 6. Mã băm hiện tại được giữ nguyên trong bản này để mốc "trước khi sửa" vẫn kiểm lại được.

- Số hộp `needs_review` trước và sau khi kiểm: tôi không ghi lại con số trước khi rà, nên chỉ có số cuối: **6 hộp** vẫn đang mang `needs_review` trong gói xuất cuối. Đây cũng là một quy tắc phát sinh — ghi số `needs_review` ngay sau lượt gán đầu tiên thì mới đo được lượt tự kiểm có tác dụng gì. Sáu hộp còn lại: `drive_008` hộp 17 (`car`, occluded/inside, 33×39 px), hộp 26 (`car`, clear/truncated, 113×82 px), hộp 28 (`car`, unclear/truncated, 13×10 px); `drive_033` hộp 29 (`car`, unclear/inside, 7×9 px); `drive_038` hộp 34 (`car`, occluded/inside, 22×19 px), hộp 37 (`car`, unclear/inside, 14×22 px). Đối chiếu với ô tự kiểm "đã xử lý mọi hộp `needs_review`" trong phiếu quy tắc: ô đó chưa đạt.
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: năm trong sáu hộp trên nhỏ hơn 25×25 điểm ảnh hoặc bị mép ảnh cắt, tức đúng tình huống mục 1 phiếu quy tắc gọi là "quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ". Tôi đã gán `car` kèm `needs_review` thay vì bỏ trống, và cho tới lúc nộp thì chưa hỏi được ai — nên câu hỏi này được chuyển thẳng sang mục 7 cho Lab Coach: ở kích thước đó thì nên gán kèm `needs_review`, hay nên không gán và ghi lý do vào nhật ký quyết định.
**Kết quả lượt tự kiểm trên cả bốn ảnh.**

- *Vật thể trùng:* đạt. Không cặp hộp nào trong 99 hộp chồng nhau quá IoU 0.5, nên không có phương tiện nào bị gán hai lần và không có hộp nào gộp hai xe.
- *Vật thể thiếu:* chưa đạt. Một chiếc `van` giữa khung `drive_008` bị bỏ sót, ghi ở dòng 1 bảng trên. Ba hộp còn lại mà bộ tham chiếu có và tôi không ghép được thì không phải vật tôi bỏ sót: hai hộp là hộp `car` vẽ nhầm trên thân xe khách phía tham chiếu, một hộp là khác biệt về cách tách hai xe đỗ sát nhau (mục 6).
- *Hình học:* hộp của tôi hơi rộng hơn bộ tham chiếu một cách có hệ thống. Trên 46 cặp ghép được, diện tích hộp của tôi gấp trung vị 1.053 lần hộp tham chiếu, 36/46 hộp lớn hơn, và trung vị 7.7% diện tích hộp của tôi nằm ngoài hộp tham chiếu. Độ lệch nhỏ nhưng một chiều, nên nhiều khả năng là thói quen vẽ chừa viền chứ không phải sai số ngẫu nhiên. Năm hộp lệch nhiều nhất: `drive_038` hộp 16 (1.36 lần), `drive_022` hộp 4 (1.33), `drive_008` hộp 23 (1.23), `drive_033` hộp 4 (1.23), `drive_008` hộp 20 (1.23). Quy tắc phát sinh: bám sát đường viền phần nhìn thấy, không chừa viền an toàn; kiểm lại năm hộp này ở độ phóng 100%.
- *Thuộc tính:* đủ nhưng chưa đúng hết. Cả 99 hộp có đủ ba thuộc tính với giá trị nằm trong tập cho phép, nhưng 8 hộp sai giá trị `boundary` như ghi ở dòng 2 bảng trên.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: `2 0.383570 0.724641 0.435609 0.348156` (dòng 1, `labels/train/drive_022.txt`)
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp `2` = `bus`; trên ảnh 640×640 quy ra `xyxy = (106.09, 352.36, 384.88, 575.18)`. Con số này trùng khít với `annotations.xml`, nơi cùng vật thể được ghi `xtl="106.09" ytl="352.36" xbr="384.88" ybr="575.18"` — một minh chứng nhỏ cho thấy hai gói xuất mô tả cùng một trạng thái.
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

Định dạng chỉ ràng buộc hình thức: năm trường, chỉ số lớp là số nguyên trong dải đã khai, bốn số còn lại chuẩn hóa về [0, 1]. Không trường nào biết vật thể thật sự là gì hay có đáng được gán hay không. Bài này có đủ ba loại phản ví dụ, tất cả đều nằm trong tệp hợp lệ tuyệt đối. Sai lớp: bộ tham chiếu ghi chỉ số `3` cho chính chiếc xe khách khớp nối này, mà theo `data.yaml` của chính nó thì `3` là `van` — dòng hợp lệ, lớp vô lý. Sai phạm vi: tôi bỏ sót hẳn một chiếc `van` giữa khung trong `drive_008`, tệp vẫn hợp lệ vì thiếu một dòng không phải lỗi cú pháp. Sai hình học: bộ tham chiếu có hai hộp nằm gọn trên thân xe khách trong `drive_008` và gán chỉ số `0`, tức `car` — hộp hợp lệ, vẽ trên một mảng thân xe buýt. Không trình kiểm định dạng nào bắt được ba trường hợp này; chỉ mở ảnh ra nhìn mới bắt được.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: `drive_022`, `drive_033`, `drive_038`
- Mã ảnh thẩm định: `drive_008`
- Mô tả một dự đoán trong `detect_result.jpg`:

Không có dự đoán nào. `detect_result.jpg` chính là `drive_008.jpg` được lưu lại, không vẽ thêm gì: so từng điểm ảnh với ảnh gốc cho chênh lệch trung bình 0.15 và tối đa 10 trên thang 255, tức chỉ là sai khác do nén JPEG lại. Dò theo chương trình cũng không thấy cạnh hộp nào: đoạn điểm ảnh bão hòa màu nằm ngang dài nhất chỉ 24 điểm, không hàng nào dài quá 40 — trong khi trên `comparison_overlay.png`, nét vẽ hộp cho ra hàng nghìn điểm bão hòa liên tục. Vậy mô tả trung thực là: trên ảnh thẩm định, mô hình không đưa ra dự đoán nào vượt ngưỡng tin cậy mặc định.

- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?

Kết quả rỗng phù hợp với cấu hình lần chạy hơn là với một kết luận về nhãn. Đọc ô 4 của sổ thực hành: `yolo11n.pt` huấn luyện `epochs=8, batch=4, imgsz=640, freeze=10, patience=3, seed=42` trên CPU, hết 35.13 giây, rồi dự đoán bằng `best.pt` ở `conf=0.25`. Ba tham số quyết định ở đây. `freeze=10` giữ nguyên phần lõi trích đặc trưng, nên chỉ phần đầu dự đoán được học; phần đầu đó lại phải học lại từ đầu vì bài dùng bốn lớp riêng chứ không phải tám mươi lớp của trọng số gốc. 8 epoch trên 3 ảnh là quá ít để phần đầu đó đẩy độ tin cậy lên trên 0.25. `patience=3` còn có thể dừng sớm trước cả khi hết 8 epoch. Nói cách khác, ảnh trắng hộp ở đây là kết quả có thể đoán trước từ cấu hình, không phải tín hiệu về chất lượng nhãn.

Một khả năng tôi đã loại trừ: dữ liệu huấn luyện không lấy từ `data.yaml` trong gói xuất của tôi. Ô 4 gọi `build_training_dataset`, hàm này chép ảnh và nhãn sang thư mục mới theo cột `split` của manifest rồi tự viết `data.yaml` riêng có đủ `train` và `val`. Vậy việc `data.yaml` trong gói xuất của tôi thiếu khóa `val` và `train.txt` trỏ tới `data/images/train/…` không ảnh hưởng tới lần chạy này.

- Minh chứng nào có thể bác bỏ nhận định của bạn?

Sửa `conf=0.25` thành `conf=0.01` trong ô 4 rồi chạy lại phần dự đoán. Nếu có hộp hiện ra ở độ tin cậy 0.05–0.2 thì nhận định "chỉ là dưới ngưỡng vì huấn luyện quá ít" được củng cố. Nếu vẫn rỗng, nhận định bị bác và phải tìm ở chỗ khác. Hai phép kiểm tách được hai nguyên nhân: đọc dòng bộ khung in ra lúc bắt đầu về số ảnh đã nạp (nạp 0 ảnh thì lỗi là dữ liệu, nạp đủ 3 ảnh thì không phải), và mở biểu đồ mất mát trong thư mục `runs` mà `plots=True` đã sinh ra (mất mát không giảm thì vấn đề là ở vòng huấn luyện, giảm bình thường thì vấn đề chỉ là quy mô).

- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?

Ba ảnh huấn luyện và một ảnh thẩm định đều rút từ cùng một kho bốn ảnh, cùng kiểu ngã tư đô thị, cùng góc camera trên cao, cùng điều kiện ban ngày; không có tập kiểm thử tách riêng, không có ảnh đêm, mưa hay góc khác. Một ảnh thẩm định nghĩa là mỗi vật thể bắt hụt làm đổi chỉ số vài phần trăm, và không có cách nào tách phương sai do dữ liệu khỏi phương sai do lần chạy. Hồ sơ lần chạy cũng tự khai điều này: `purpose` ghi chỉ dùng để phản hồi và tìm lỗi dữ liệu, `not_production_benchmark: true`. Quan trọng hơn, nhãn đem đi huấn luyện chính là nhãn đang được đem đi đối chiếu — mọi lỗi có hệ thống trong nhãn sẽ được mô hình học lại rồi trả ra như thể là xác nhận.

Một hệ quả cần nói thẳng: không con số nào của lần chạy này — mAP, số hộp bắt được, hay việc ảnh dự đoán trống — được dùng để chấm người gán nhãn. Chúng đo một vòng huấn luyện 35 giây với phần lõi bị đóng băng trên ba ảnh, nên chúng nói về cấu hình lần chạy chứ không nói về chất lượng nhãn. Chiều dùng đúng của chúng là chiều ngược lại: một dự đoán lạ có thể chỉ chỗ cần mở ảnh kiểm lại quy tắc hoặc dữ liệu, và chỉ dừng ở mức gợi ý chỗ cần nhìn.

## 6. Đối chiếu nhãn

- Số hộp ghép được: **46**
- IoU trung bình và trung vị: **0.86727** và **0.879456** (thấp nhất 0.6614, cao nhất 0.9864; ngưỡng sàn khi ghép là 0.01 và theo công bố của gói đối chiếu, đây không phải ngưỡng đạt chính thức)
- Mức đồng thuận lớp: **0.717391** như công cụ xuất ra (33/46) — **0.978261** (45/46) sau khi sửa lỗi thứ tự lớp mô tả bên dưới
- Số hộp phía bạn không ghép được: **53** trên tổng 99
- Số hộp phía đối chiếu không ghép được: **4** trên tổng 50

| Ảnh | Hộp của tôi | Hộp đối chiếu | Ghép được | IoU trung bình | Tôi thừa | Đối chiếu thừa |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| `drive_022` | 5 | 5 | 5 | 0.8587 | 0 | 0 |
| `drive_033` | 29 | 12 | 12 | 0.8554 | 17 | 0 |
| `drive_038` | 37 | 17 | 16 | 0.8773 | 21 | 1 |
| `drive_008` | 28 | 16 | 13 | 0.8692 | 15 | 3 |

### Ảnh phủ nói được gì và không nói được gì

`comparison_overlay.png` vẽ 99 khung đỏ của tôi và 50 khung xanh của bộ tham chiếu trên bốn ảnh. Tôi đã kiểm lại nó chứ không chỉ nhìn: dựng lại vị trí khung từ hai bộ nhãn rồi đối chiếu với điểm ảnh màu trong ảnh phủ, chu vi của cả 50 khung xanh được giải thích trọn vẹn, phần điểm màu không giải thích được chỉ là các chi tiết đỏ có sẵn trong cảnh như đèn tín hiệu và thân chiếc van đỏ ở `drive_038`. Nghĩa là ảnh phủ khớp đúng hai bộ nhãn, không thừa không thiếu khung nào.

Hai điều đọc được ngay từ ảnh phủ:

- **Khoảng cách phạm vi hiện rõ.** Ở `drive_033` và `drive_038`, các chùm khung đỏ nhỏ nằm xa trên mặt đường gần như không có khung xanh nào đi kèm. Bộ tham chiếu dừng sớm hơn tôi rất nhiều ở nhóm xe nhỏ ở xa.
- **Chỗ tôi bỏ sót cũng hiện rõ.** Ở `drive_008`, chiếc xe thân hộp bạc giữa lòng đường chỉ có một khung xanh trơ trọi, không có khung đỏ nào — đây chính là lỗi phạm vi đã ghi ở mục 3.

Một điều ảnh phủ **không** nói được, và đây là điểm quan trọng nhất của bước đối chiếu: nó chỉ vẽ hình học, không viết tên lớp. Toàn bộ lỗi chiếm 12 trong 13 bất đồng ở dưới là lỗi tên lớp trên những cặp hộp trùng khít nhau — trên ảnh phủ chúng hiện ra như các cặp đỏ–xanh chồng nhau đẹp nhất, tức trông giống bằng chứng là hai bên đang đồng ý. Muốn thấy lỗi đó phải mở tệp nhãn ra đọc. Vì vậy tôi có bổ sung bốn ảnh phủ có ghi tên lớp trong `overlay/`.

### Một điểm khác biệt cụ thể, và điều nó dẫn tới

`drive_033`, hộp 1 của tôi ghép với hộp 1 của bộ tham chiếu ở IoU **0.9864** — cặp trùng khít nhất cả bài — nhưng tôi ghi `bus` còn bản tóm tắt báo bộ tham chiếu ghi `van`. Hình học gần như đồng nhất nên chắc chắn hai bên đang nói về cùng một vật; khác biệt nằm hoàn toàn ở tên lớp. Cắt vùng đó ra xem thì đó là thân xe khách đỏ–trắng với dãy cửa sổ chạy suốt.

Khác biệt này không đứng một mình. Bảng chéo 46 hộp ghép được có dạng rất đều:

| Lớp của tôi | Bản tóm tắt báo bộ tham chiếu ghi | Số hộp |
| --- | --- | ---: |
| `car` | `car` | 33 |
| `truck` | `bus` | 5 |
| `bus` | `van` | 4 |
| `van` | `truck` | 3 |
| `van` | `car` | 1 |

Toàn bộ 33 hộp `car` trùng nhau và toàn bộ 13 hộp còn lại đều lệch. Bất đồng do phán đoán không có hình dạng như vậy — nó sẽ rải rác và sẽ đụng cả vào `car`. Hình dạng này là của một hoán vị chỉ số lớp.

**Kiểm bằng bằng chứng, không chỉ bằng suy luận.** Tôi đã mở cả ba gói:

1. *Tệp khai báo nói gì.* `data.yaml` của gói tham chiếu khai `0 car, 1 truck, 2 bus, 3 van`, giống hệt của tôi, và `release-manifest.json` cũng lặp lại đúng thứ tự đó. Nếu chỉ đọc tới đây thì giả thuyết hoán vị bị bác.
2. *Chỉ số thật trong tệp nhãn nói gì.* Đếm theo từng ảnh, số hộp mang chỉ số `2` của bộ tham chiếu là 1–1–2–1, trùng tuyệt đối với số hộp `truck` của tôi là 1–1–2–1. Theo thứ tự đã khai thì `2` phải là `bus`, mà số `bus` của tôi là 1–2–5–2, không khớp ở đâu cả.
3. *Ảnh nói gì.* Đây là phần quyết định. Chiếc xe khách khớp nối trong `drive_022` mang chỉ số `3` trong tệp tham chiếu — theo khai báo là `van`. Chiếc xe thùng kín gầm cao mang chỉ số `2` — theo khai báo là `bus`. Chiếc xe thân hộp bạc bị tôi bỏ sót trong `drive_008` mang chỉ số `1` — theo khai báo là `truck`. Cả ba đều mâu thuẫn với ảnh, và cả ba đều đúng nếu đọc chỉ số theo thứ tự `[car, van, truck, bus]`.

Đọc lại theo thứ tự đó thì 12 trong 13 bất đồng biến mất, đồng thuận thành **45/46 = 0.978**. Tôi đã cắt cả 13 vật thể ra xem ở độ phóng đầy đủ: cả 13 nhãn của tôi đều khớp với thứ mình nhìn thấy. Kết luận là gói tham chiếu **tự mâu thuẫn với chính nó** — tệp nhãn được viết theo một thứ tự lớp, còn `data.yaml` và bản kê phát hành công bố một thứ tự khác — chứ không phải hai bên bất đồng về cách phân lớp.

**Vì sao lỗi này lọt qua mọi cổng kiểm.** Đọc mã sổ thực hành thì thấy rõ. Hàm `audit_yolo_export` chỉ đối chiếu danh sách `names` đọc từ `data.yaml` với `EXPECTED_CLASS_NAMES` và dừng chương trình nếu khác — gói tham chiếu khai đúng `[car, truck, bus, van]` nên qua cổng này trong nháy mắt. Sau đó `compare_annotation_records` tính đồng thuận bằng `left_record["class_id"] == right_record["class_id"]`, tức so hai số nguyên với nhau. Không có bước nào trong cả sổ thực hành đối chiếu chỉ số lớp với nội dung điểm ảnh. Hệ quả là một gói có tệp khai báo đúng nhưng chỉ số viết sai sẽ đi trót lọt từ đầu đến cuối và chỉ hiện ra dưới dạng một con số đồng thuận thấp — đúng thứ dễ bị đọc nhầm thành "người gán nhãn này phân lớp kém".

Cần nói rõ một điều để tránh đọc nhầm: tôi **không** đổi mã lớp trong gói xuất của mình, và cũng không sửa tệp nhãn của bất kỳ bên nào. Thứ tự lớp trong gói của tôi vẫn đúng `0 car, 1 truck, 2 bus, 3 van` như phiếu quy tắc quy định, kiểm lại được bằng mã băm ghi ở mục 1. Con số 0.978 là kết quả của việc đọc lại chỉ số phía gói tham chiếu khi tính toán, không phải kết quả của việc sửa nhãn cho khớp.

Một giả thuyết khác cần để ngỏ: gói tham chiếu có thể đã được cố ý gài lỗi để xem người học có kiểm tới nơi hay không. Bản kê phát hành tự mô tả là tài liệu phản hồi riêng sau khi bài của người học đã khóa. Hai khả năng dẫn tới cùng một hành động nên không cần phân xử ngay, nhưng nên hỏi.

### Bất đồng thật sự còn lại và lỗi phía bộ tham chiếu

- **`drive_022` hộp 4** là điểm bất đồng duy nhất sống sót: tôi ghi `van`, bộ tham chiếu ghi chỉ số `0` — là `car` theo cả hai cách đọc. Ảnh cho thấy một xe thân hộp trắng cao, kín, mui vươn cao hơn ca-bin. Theo mục 2 phiếu quy tắc, đó là `van`; tôi giữ nhãn của mình và ghi nhận đây là chỗ đáng hỏi.
- **Hai hộp của bộ tham chiếu vẽ trên thân xe buýt.** Trong `drive_008`, hộp tham chiếu số 8 tại `(535, 220)–(591, 290)` và số 10 tại `(480, 181)–(534, 241)` đều mang chỉ số `0` = `car` theo cả hai cách đọc, nhưng nằm gọn trên mặt sau và mảng cửa sổ của chiếc xe khách khớp nối. Đây là lỗi hình học kèm lỗi lớp ở phía tham chiếu, không phải chỗ tôi bỏ sót.
- **Một hộp tham chiếu là vật tôi thật sự bỏ sót.** Hộp số 12 trong `drive_008`, `(284, 249)–(348, 351)`, chỉ số `1` = `van` theo cách đọc đã sửa: một chiếc xe thân hộp bạc giữa lòng đường, không bị che, 64×102 điểm ảnh, và trên ảnh phủ chỉ có khung xanh trơ trọi. Đây là lỗi của tôi, và là lỗi đáng ngại nhất trong bài vì vật thể nằm giữa khung chứ không ở rìa.
- **Hộp thứ tư** là hộp số 10 trong `drive_038`, chỉ số `0` = `car`, chồng 0.237 IoU với một hộp `car` của tôi đã ghép sang vật khác — nhiều khả năng là khác biệt về cách tách hai xe đỗ sát nhau.

### 53 hộp phía tôi không ghép được

Đây là khoảng cách lớn nhất giữa hai bên, và IoU trung bình 0.867 hoàn toàn không nói gì về nó. Phần lớn là chênh lệch ngưỡng phạm vi: cạnh trung vị của hộp ghép được là 44–63 điểm ảnh, còn của hộp không ghép được chỉ 19–30 điểm ảnh — tức tôi gán nhiều xe nhỏ ở xa mà bộ tham chiếu bỏ qua, đúng như các chùm khung đỏ lẻ trên ảnh phủ. Riêng một nhóm thì không giải thích được bằng kích thước: cả 5 hộp `bus` của tôi trong `drive_038` đều không ghép được, trong đó có một xe khách vàng 97×99 điểm ảnh nhìn rõ hoàn toàn. Bộ tham chiếu không có hộp nào ở đó (chỉ số `3` xuất hiện 0 lần trong `drive_038`). Vậy phạm vi của bộ tham chiếu hẹp hơn phạm vi của tôi, chứ không phải tôi gán sai.

### Quy tắc hoặc hành động sửa phát sinh

1. Báo cáo cả hai con số đồng thuận, 0.717 và 0.978, kèm giải thích — không thay số lặng lẽ.
2. Thêm vào phiếu quy tắc: trước khi đối chiếu, in thứ tự lớp của cả hai gói **và** đối chiếu vài vật thể mẫu với ảnh, vì trong bài này chính tệp khai báo là thứ sai.
3. Ảnh phủ dùng để đối chiếu phải ghi tên lớp cạnh mỗi khung, nếu không thì loại lỗi nặng nhất của bài này vô hình trên đó.
4. Thêm hộp `van` bị bỏ sót trong `drive_008` và rà lại cả bốn ảnh theo lưới, vì một vật giữa khung lọt qua được thì có thể còn vật khác.
5. Xử lý 6 hộp `needs_review` còn tồn.
6. Thống nhất ngưỡng phạm vi cho vật nhỏ ở xa trước khi so số lượng hộp với bất kỳ ai.

### Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?

Bài này cho phản ví dụ theo cả hai chiều. Chiều thứ nhất: 0.717 trông như bất đồng nặng về phán đoán, nhưng hầu hết là một lỗi ánh xạ chỉ số — con số đồng thuận không tự phân biệt được "hai người nghĩ khác nhau" với "một bên viết sai thứ tự lớp". Chiều thứ hai: nếu tôi sửa nhãn theo bộ tham chiếu cho khớp, tôi sẽ đạt đồng thuận gần tuyệt đối bằng cách gọi xe khách là `van` và xe tải là `bus` — đồng thuận tăng, nhãn sai đi. Thêm nữa, IoU trung bình 0.867 chỉ tính trên 46 hộp ghép được, bỏ ra ngoài 53 hộp phía tôi và 4 hộp phía tham chiếu; phần bất đồng lớn nhất, tức chuyện vật nào đáng được gán, không nằm trong con số đó. Cuối cùng, bản thân bộ tham chiếu tự công bố là tài liệu dạy học dùng để phản hồi sau bài làm độc lập, không phải phán quyết chất lượng, và nó chứa ít nhất hai hộp vẽ trên thân xe buýt gán là `car` — trùng với nó không đồng nghĩa với đúng. Vì vậy tôi không dùng IoU trung bình hay mức đồng thuận để kết luận bên nào đúng hơn; hai con số đó chỉ dùng để chỉ ra chỗ cần mở ảnh xem lại, và mọi kết luận trong mục này đều dựa trên việc xem ảnh chứ không dựa trên thứ hạng.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ. — `GUIDELINE_MINI_SHEET.md`, ba tình huống A/B/C đã viết, kèm ghi chú rõ là viết sau bước đối chiếu chứ không phải trước.
- [x] Có kết quả kiểm hai gói xuất. — `my_export_audit.json`, `my_native_export_audit.json`.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán. — `training_run.json`, `detect_result.jpg`.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu. — `comparison_summary.json`, `comparison_iou.csv`, `comparison_overlay.png`, và bốn ảnh phủ có ghi tên lớp trong `overlay/`. Lưu ý kỹ thuật nhỏ: dòng tiêu đề trong `comparison_overlay.png` bị mất dấu tiếng Việt (hiện ra ô vuông) do phông chữ khi vẽ; nên vẽ lại bằng phông có dấu trước khi đưa vào kho.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình. — ba tệp `.zip` và tệp trọng số nằm ngoài kho. Một điểm tôi chủ động nêu ra để hỏi: bốn ảnh phủ bổ sung trong `overlay/` có ghi tên lớp của từng hộp phía tham chiếu, đủ để dựng lại phần lớn bộ nhãn đó. Ảnh phủ chính thức thì chỉ vẽ hình học nên không có vấn đề này. Tôi giữ lại duy nhất `overlay_drive_022.png` vì lập luận về thứ tự lớp cần nó, và chờ Lab Coach xác nhận trước khi đưa ba ảnh còn lại lên.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập. — cần bạn xác nhận trên kho thật.

Ô 7 của sổ thực hành tự kiểm đủ chín tệp đầu ra rồi đóng gói `REPORT.md`, `GUIDELINE_MINI_SHEET.md` và thư mục `day2_lab_outputs/` thành kho tên `KX-DAY02-<Ho-Ten-Khong-Dau>-<MSSV>`. Lưu ý một giới hạn của ô đó: nó chỉ chặn khi còn đúng chuỗi giữ chỗ mặc định của hai tệp mẫu, nên các chỗ `[CẦN #n]` trong bản báo cáo này sẽ **không** bị nó bắt. Phải tự rà bằng mắt trước khi chạy.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

Minh chứng mạnh nhất là việc lần ra lỗi thứ tự lớp trong gói tham chiếu. Nó không đến từ một con số mà từ ba nguồn độc lập cùng chỉ về một hướng: hình dạng bảng chéo (mọi hộp `car` trùng, mọi hộp khác lệch), số đếm theo từng ảnh (chỉ số `2` của tham chiếu khớp tuyệt đối với `truck` của tôi ở cả bốn ảnh), và ảnh gốc (chiếc xe khách khớp nối mang chỉ số mà tệp khai báo của chính gói đó gọi là `van`). Đáng chú ý là ảnh phủ chính thức không giúp được gì ở đây, vì nó chỉ vẽ hình học. Đứng sau nó là kiểm tra chéo hai định dạng của tôi: 99/99 hộp ghép, IoU thấp nhất 0.99992, ba thuộc tính đủ trên cả 99 hộp.

Câu hỏi còn lại: gói `day2-reference-4img-v1` có được phát ra với thứ tự lớp cố ý sai để làm bài tập kiểm chứng, hay đó là lỗi phát hành cần báo lại? Và trong báo cáo em nên lấy con số nào làm chính — 0.717 như công cụ xuất ra, hay 0.978 sau khi đọc lại chỉ số? Câu hỏi thứ hai: bộ tham chiếu gán 50 hộp còn em gán 99; ngưỡng "quá nhỏ hoặc mờ đến mức không phân lớp có căn cứ" nên hiểu chặt tới đâu, và năm chiếc xe khách ở rìa `drive_038` mà bộ tham chiếu bỏ qua thì nên gán hay không?

---
