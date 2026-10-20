# Găng tay nhận dạng ký hiệu với Edge AI

Lộ trình khởi tạo ngày 15/10/2026. Người dùng đã có linh kiện nhưng chưa chạy ổn một tay. Đã thêm và kiểm tra phần mềm khởi đầu; chưa có log cảm biến thật hoặc kết quả nhận dạng thực nghiệm.

## Bắt đầu với code và train AI

Đã thêm bộ mã khởi đầu một tay; chưa có dữ liệu thật hay trọng số mô hình của người dùng. Xem [hướng dẫn từng bước](docs/CODE_TRAIN_QUICKSTART.md).

- Chạy `firmware/flex_diagnostic/flex_diagnostic.ino` trước để sửa lỗi flex không đổi khi gập.
- Khi tín hiệu đã đổi nhưng có xung nhảy, thử riêng [flex_filter_demo](firmware/flex_filter_demo/README.md): so raw/trung bình/median+EMA trên một kênh. Chưa thay preprocessing của pipeline AI.
- Sau đó `firmware/glove_capture/glove_capture.ino` thu 11 kênh ở tốc độ khởi điểm 50 Hz.
- Python trong `glove_ai/` thu từng trial, kiểm tra log, train MLP nhỏ và xuất C++ float32 để suy luận trên ESP32-S3.
- Bản đầu chưa triển khai hai tay, không dây, phát lời tự động hoặc nhận dạng đầy đủ IS. Các nhãn thử kỹ thuật không được mô tả thành ký hiệu IS đã xác thực.

Cấu hình do người dùng xác nhận mới nhất: **flex GPIO1–5; MPU6050 SDA=GPIO9/SCL=GPIO8; nguồn 3.3V và điện trở cố định 10 kΩ; Arduino IDE**. Thứ tự ngón thực tế, chiều lắp, mạch chia áp và hoạt động cảm biến vẫn cần kiểm tra.

Ngày 15/10/2026: 10 test Python/C++ đã qua, gồm đọc CSV thật với dữ liệu giả kiểm thử, chia tập, export binary/multiclass và so xác suất native C++ với Python. Hai sketch đã compile trên Arduino-ESP32 3.3.12. Đây là kiểm tra phần mềm, chưa phải đánh giá hiệu năng ký hiệu/người dùng. Môi trường Python đã thử nằm trong `.venv`; phiên bản lưu ở `requirements-lock.txt`.

## Nguồn lực và định hướng đã xác nhận

- Thời hạn mong muốn: 7 tháng; mốc đích đề xuất 15/05/2027 nếu bắt đầu từ 15/10/2026.
- Thời gian tối đa: 3 giờ/ngày; chưa có cam kết tổng giờ mỗi tuần, không mặc định làm đủ 21 giờ/tuần.
- Tự đánh giá kinh nghiệm: C++/ESP32 70%, điện tử 60%, Python/ML 0%. Đây là mức tự đánh giá, không phải kết quả kiểm tra năng lực.
- Hướng người dùng quốc tế; mong muốn dùng “ngôn ngữ ký hiệu quốc tế”; đầu ra giọng nói tiếng Anh.
- Đã có linh kiện; chưa xác nhận số lượng thực tế, mã bo, ngân sách còn lại hoặc số thành viên.
- Chưa xác nhận người cộng tác thành thạo ký hiệu và quyền tiếp cận người dùng để thu dữ liệu.

International Sign (IS) là thực hành giao tiếp giữa người dùng các ngôn ngữ ký hiệu khác nhau, có biến thể; không phải bộ ký hiệu phổ quát cố định. ASL là ngôn ngữ riêng, không đồng nghĩa với IS hoặc tiếng Anh. Đầu ra tiếng Anh không quyết định ngôn ngữ ký hiệu đầu vào. Tạm ghi nhận mong muốn của người dùng là IS, chưa tự chuyển sang ASL; cần chốt phạm vi có người thành thạo kiểm tra trước khi thu bộ dữ liệu chính thức. Nguồn: [WASLI về IS](https://wasli.org/sign-language-interpreters/about-the-international-sign/) và [NIDCD về ASL](https://www.nidcd.nih.gov/health/american-sign-language).

Chi tiết phân bổ công việc và lộ trình học ML: [ROADMAP_7_MONTHS.md](docs/ROADMAP_7_MONTHS.md).

## Mục tiêu của người thực hiện

1. Làm một dự án cá nhân có ích cho cộng đồng, được kiểm tra cùng người dùng mục tiêu.
2. Xây dựng báo cáo khoa học có câu hỏi, dữ liệu, đối chứng và kết quả có thể tái lập.
3. Tạo hồ sơ kỹ thuật thể hiện năng lực điện tử, embedded, AI và thiết kế thí nghiệm.

Các mục tiêu này đã được người dùng nêu. Lộ trình và câu hỏi nghiên cứu bên dưới là đề xuất ban đầu, cần điều chỉnh theo nguồn lực và kết quả pilot; chưa khẳng định tính mới hay khả năng được nhận bài.

## Phần cứng dự tính

| Thành phần | Mỗi tay | Tổng |
|---|---:|---:|
| Flex sensor, hiểu là một cảm biến mỗi ngón | 5 | 10 |
| MPU6050 trên mu bàn tay | 1 | 2 |
| ESP32-S3 N16R8 | 1 | 2 |

Người dùng đã có linh kiện; chưa xác nhận model bo phát triển, loại/chiều dài/điện trở flex, breakout MPU6050, nguồn và số lượng thực tế. N16R8 là thông tin dự tính của người dùng, chưa kiểm tra trên bo thực tế. Chưa chốt pinout hoặc mua thêm thiết bị.

Nếu đọc đủ 3 trục gia tốc và 3 trục gyro, dữ liệu thô có 11 kênh mỗi tay, 22 kênh hai tay. Không tính nhiệt độ, timestamp hoặc đặc trưng suy ra vào 22 kênh cảm biến.

## Phạm vi ban đầu

- Nhận dạng 5–10 ký hiệu/từ trong một bộ được chọn cùng người thành thạo; số lượng này là mục tiêu nguyên mẫu, không phải ngưỡng đủ cho công bố.
- Chọn ngôn ngữ ký hiệu/biến thể địa phương và một tình huống sử dụng cụ thể trước khi thu dữ liệu chính thức.
- Nhận dạng chạy trên ESP32-S3 ở mốc triển khai Edge AI; điện thoại hoặc máy tính hiển thị chữ và phát lời.
- Tách rõ thử nghiệm thuật toán trên máy tính với kết quả chạy thật trên MCU.
- Giai đoạn đầu có thể dùng thao tác xác nhận phát lời; ghi rõ cơ chế này trong đánh giá.

Găng tay không đo nét mặt, vị trí tay so với thân người hoặc mọi bậc tự do của ngón. Một flex mỗi ngón không tách được đầy đủ từng khớp, độ xòe hoặc tiếp xúc. MPU6050 không cung cấp vị trí không gian tuyệt đối đáng tin chỉ bằng tích phân. Vì vậy, chưa mô tả nguyên mẫu là hệ thống dịch hội thoại đầy đủ.

## Kiến trúc đề xuất

```text
Tay A: 5 flex + MPU6050 -> ESP32-S3 A -- dữ liệu có timestamp --> ESP32-S3 B
Tay B: 5 flex + MPU6050 -------------------------------------> ESP32-S3 B
                                       ghép dữ liệu, xử lý, suy luận
                                                       |
                                              nhãn / trạng thái
                                                       v
                                         Điện thoại hoặc máy tính
                                           hiển thị + phát lời
```

Trước khi kiểm chứng liên kết không dây, dùng USB để ghi dữ liệu và chẩn đoán. Giao thức giữa hai tay và liên kết đến điện thoại sẽ được chọn sau khi xác nhận bo, kỹ năng và đo mất gói/độ trễ; chưa mặc định ESP-NOW và BLE đồng thời đáp ứng yêu cầu.

## Các mốc nghiệm thu

| Mốc | Công việc | Bằng chứng để chuyển mốc |
|---|---|---|
| M0 — Nhu cầu và khả năng đo | Chọn tình huống, người dùng, bộ ký hiệu ứng viên; xác nhận BOM | Bản mô tả nhu cầu, phản hồi ban đầu và danh mục linh kiện thực tế |
| M1 — Một tay | Đọc 5 flex + 6 kênh MPU; timestamp, sequence number, hiệu chuẩn | Log thô, đồ thị, ghi nhận nhiễu/mất mẫu; chạy lặp lại sau tháo/đeo |
| M2 — Hai tay | Ghép 22 kênh theo thời gian và phát hiện dữ liệu thiếu | Log hai tay, kiểm tra độ lệch thời gian và mất gói; không ghép chỉ bằng số thứ tự hàng |
| M3 — Mô hình đối chứng | Thu pilot, xem ký hiệu có phân biệt được không; thử mô hình đơn giản | Cách chia tập rõ, macro-F1, ma trận nhầm lẫn, phân tích lỗi |
| M4 — Edge AI và phát lời | Đưa một mô hình nhỏ lên ESP32-S3, nối với giao diện phát lời | So sánh cùng đầu vào trên PC/MCU; thời gian suy luận, RAM/flash, demo hệ thống |
| M5 — Nghiên cứu chính | Thu dữ liệu theo giao thức đã khóa; kiểm tra cơ chế giảm phát lời sai | Kết quả trên người/phiên mới và các phép đối chứng trong RESEARCH_PLAN.md |
| M6 — Bàn giao và portfolio | Hoàn thiện nguyên mẫu, tài liệu, báo cáo, video và gói tái lập | Người khác làm theo được hướng dẫn; số liệu có log và định nghĩa phép đo |

Lộ trình 7 tháng là mục tiêu thực hiện theo thông tin mới của người dùng; sẽ điều chỉnh phạm vi theo số giờ thực tế, ngân sách và khả năng tiếp cận người tham gia. Có thể viết báo cáo phương pháp từ đầu, nhưng kết luận chỉ viết theo kết quả đã đo. Thời gian phản biện/chấp nhận bài không nằm trong cam kết hoàn thành 7 tháng.

## Việc đầu tiên

- [x] Xác nhận giai đoạn: đã có linh kiện, chưa chạy ổn một tay.
- [x] Ghi kết nối người dùng xác nhận: flex1–5, SDA9/SCL8, 3.3V và 10kΩ.
- [ ] Xác nhận tên bo phát triển chính xác, loại flex, chiều lắp và log cảm biến sau khi nạp sketch chẩn đoán.
- [ ] Chọn ngôn ngữ ký hiệu/biến thể và một nhu cầu giao tiếp cụ thể với người dùng mục tiêu.
- [x] Xác nhận mong muốn 7 tháng, tối đa 3 giờ/ngày và mức kỹ năng tự đánh giá.
- [ ] Bổ sung ngân sách còn lại, số thành viên và giờ làm thực tế mỗi tuần.
- [ ] Nếu đã có phần cứng: lập phép thử một flex và một MPU qua USB; sau đó mở rộng lên một tay.

## Cách làm việc với mentor

Mỗi phiên tập trung vào một đầu ra kiểm chứng được. Người thực hiện cung cấp trạng thái, dữ liệu hoặc lỗi; mentor giải thích nguyên lý, hỗ trợ thiết kế/code, kiểm tra bằng chứng và chốt bước tiếp theo. Ghi quyết định cùng lý do; nêu rõ phần nào do thành viên thực hiện và hiểu cách vận hành của phần mình chịu trách nhiệm.

Mỗi lần báo tiến độ có thể dùng mẫu:

```text
Mốc đang làm:
Đã hoàn thành:
Bằng chứng (log/ảnh/mã/đồ thị):
Lỗi hoặc điều chưa hiểu:
Linh kiện và thời gian còn lại:
```

## Giá trị cộng đồng và hồ sơ cá nhân

Người dùng mục tiêu tham gia chọn nhu cầu, từ vựng và cách phản hồi, có quyền cho biết găng tay không phù hợp với tình huống nào. Đánh giá cả sự thoải mái, thao tác đeo, quyền kiểm soát phát lời, mức hữu ích và những lỗi gây khó chịu. Người Điếc tham gia như cộng tác viên thiết kế, không chỉ cung cấp mẫu dữ liệu.

Hồ sơ dự án nên có BOM, sơ đồ, firmware, công cụ thu dữ liệu, quy trình huấn luyện, cấu hình tái lập, kết quả chạy trên thiết bị, phân tích lỗi và video có điều kiện thử rõ. Mã nguồn/dữ liệu chỉ công khai khi đã xác định quyền sử dụng, giấy phép và đồng ý tương ứng; chưa tạo repository công khai.

CV dùng số liệu đã đo như số người/phiên thử, macro-F1 trên người mới, RAM và độ trễ; không điền trước con số hoặc ghi công bố khi chưa được chấp nhận. Sự nổi bật phụ thuộc chất lượng thực hiện và bằng chứng, không thể bảo đảm thứ hạng.

## Tài liệu

- [Kế hoạch nghiên cứu](docs/RESEARCH_PLAN.md).
- [Lộ trình 7 tháng và học Python/ML](docs/ROADMAP_7_MONTHS.md).
- [ESP-DL chính thức](https://github.com/espressif/esp-dl): kiểm tra toán tử hỗ trợ trước khi chọn kiến trúc; không mặc định mô hình nào cũng chuyển đổi được.
- [ADC của ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/adc/index.html).
- [Nghiên cứu đồng thiết kế công nghệ ngôn ngữ ký hiệu](https://arxiv.org/abs/2408.13171): cơ sở cho việc cộng tác có thực chất với cộng đồng.

## Nhật ký quyết định

| Ngày | Quyết định/trạng thái | Cơ sở |
|---|---|---|
| 15/10/2026 | Bắt đầu lộ trình mentor cho ba mục tiêu: cộng đồng, nghiên cứu và CV | Yêu cầu trực tiếp của người dùng |
| 15/10/2026 | Đề xuất làm ổn tín hiệu một tay trước hai tay, mô hình và không dây | Giảm số nguồn lỗi khi bring-up |
| 15/10/2026 | Hướng nghiên cứu ứng viên: giảm phát lời ngoài ý muốn | Đã trao đổi; cần pilot và rà soát tính mới trước khi chốt |
| 15/10/2026 | Người dùng đã có linh kiện, chưa ổn một tay; mục tiêu 7 tháng, tối đa 3 giờ/ngày; Python/ML bắt đầu từ 0 | Câu trả lời trực tiếp của người dùng |
| 15/10/2026 | Đầu ra tiếng Anh, đầu vào mong muốn IS; cần xác định bộ ký hiệu có kiểm tra, chưa tự đổi thành ASL | Câu trả lời của người dùng và phân biệt ngôn ngữ từ WASLI/NIDCD |
| 15/10/2026 | Chốt khởi đầu Arduino IDE, flex GPIO1–5, SDA9/SCL8; viết lại từ 0 theo yêu cầu | Câu trả lời trực tiếp của người dùng; chưa có log cảm biến thật |
| 15/10/2026 | Dùng MLP đặc trưng thống kê và xuất C++ float32 làm baseline đầu | Giảm phụ thuộc để hoàn thành một tay trước; chưa chọn kiến trúc nghiên cứu cuối |
| 15/10/2026 | Đã qua 10 test Python/native C++ và build hai sketch Arduino | Dữ liệu kiểm thử giả và compile, chưa có phép đo bo thật |
| 15/10/2026 | Người dùng báo nhiễu dạng nhảy đột ngột; thêm demo so sánh trung bình 8, median3 và EMA ở 50 Hz | Chưa có biên độ/log để định lượng hoặc xác nhận nguyên nhân; code thu/train hiện tại giữ nguyên |
