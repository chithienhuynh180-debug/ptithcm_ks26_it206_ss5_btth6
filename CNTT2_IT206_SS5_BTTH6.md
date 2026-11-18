# Lập kế hoạch sự kiện "AI Innovation Day" bằng CoT, Prompt Chaining và Context Management

## 1. Sơ đồ chuỗi prompt

```
[THÔNG TIN SỰ KIỆN] (cố định, dán vào mọi prompt)
   |
P1 Phân tích khả thi (CoT)
   | kết quả P1
P2 Lịch trình
   | kết quả P1 + P2
P3 Dự toán ngân sách (CoT)
   | kết quả P2 + P3
P4 Phân công 10 nhân sự
   | kết quả P2 + P3 + P4
P5 Kế hoạch truyền thông
   | kết quả P1 đến P5
P6 Kiểm tra và hiệu chỉnh (CoT) -> sai thì sửa lại phần tương ứng
   | kế hoạch đã sửa
P7 Tóm tắt ngữ cảnh -> dán sang chat mới để làm tiếp
```

Cách dùng 3 kỹ thuật:

- **Chain-of-Thought:** P1, P3, P6 bắt AI suy nghĩ từng bước, ghi rõ phép tính và lý do.
- **Prompt Chaining:** P1 đến P7 chạy nối tiếp, kết quả bước trước dán vào bước sau.
- **Context Management:** P7 tóm tắt dự án. Chat dài hoặc sang chat mới thì dán bản tóm tắt vào để AI làm tiếp, không quên thông tin.

**[THÔNG TIN SỰ KIỆN]** (dán nguyên vào mọi prompt, không sửa):

```
Sự kiện: AI Innovation Day của câu lạc bộ công nghệ, dành cho sinh viên trong trường.
Quy mô: 150 sinh viên. Thời gian: 1 ngày. Ngân sách tối đa: 15.000.000 đồng.
Nhân sự tổ chức: 10 người.
Mục tiêu: giúp sinh viên hiểu và trải nghiệm ứng dụng AI trong học tập và công việc.
Ràng buộc: không vượt 15.000.000 đồng, không đổi 150 sinh viên, không đổi 10 nhân sự.
```

## 2. Bộ prompt

**P1: Phân tích tính khả thi (CoT)**

```
[THÔNG TIN SỰ KIỆN]
Hãy suy nghĩ từng bước một và tính toán chi tiết trước khi kết luận:
Bước 1: ngân sách trung bình mỗi sinh viên.
Bước 2: số sinh viên trên mỗi người tổ chức.
Bước 3: yêu cầu về địa điểm và thiết bị.
Bước 4: thời gian một ngày có đủ nội dung không.
Bước 5: các rủi ro chính.
Cuối cùng kết luận khả thi hay không, kèm điều kiện. Không tự đổi thông tin ở trên. Không lời chào, không lời kết.
```

**P2: Lịch trình**

```
[THÔNG TIN SỰ KIỆN]
Kết quả phân tích khả thi: [KẾT QUẢ P1]
Lập lịch trình 1 ngày theo khung giờ, gồm: check-in, khai mạc, talk, demo, workshop thực hành, giải lao, minigame, trao giải, bế mạc. Mỗi mục ghi nội dung và ban phụ trách. Phải xử lý các rủi ro đã nêu ở P1.
```

**P3: Dự toán ngân sách (CoT)**

```
[THÔNG TIN SỰ KIỆN]
Phân tích khả thi: [KẾT QUẢ P1]. Lịch trình: [KẾT QUẢ P2].
Hãy suy nghĩ từng bước, lập dự toán cho các hạng mục cần thiết theo lịch trình. Mỗi hạng mục ghi phép tính (số lượng x đơn giá). Cộng tổng và so sánh với 15.000.000. Nếu vượt thì chỉ rõ vượt bao nhiêu. Trả bảng: hạng mục, phép tính, thành tiền.
```

**P4: Phân công 10 nhân sự**

```
[THÔNG TIN SỰ KIỆN]
Lịch trình: [KẾT QUẢ P2]. Dự toán: [KẾT QUẢ P3].
Phân công đúng 10 người. Mỗi người ghi vai trò và nhiệm vụ, bám theo ban phụ trách trong lịch trình và các hạng mục chi trong dự toán. Trả bảng: số thứ tự, vai trò, nhiệm vụ chính.
```

**P5: Kế hoạch truyền thông trước sự kiện**

```
[THÔNG TIN SỰ KIỆN]
Lịch trình: [KẾT QUẢ P2]. Dự toán: [KẾT QUẢ P3]. Phân công: [KẾT QUẢ P4].
Lập kế hoạch truyền thông trước sự kiện theo mốc ngày (D là ngày tổ chức): kênh, nội dung, người phụ trách (theo P4), chi phí (không vượt khoản truyền thông trong P3). Phải có mốc đóng đăng ký đủ 150 sinh viên.
```

**P6: Kiểm tra và hiệu chỉnh (CoT)**

```
[THÔNG TIN SỰ KIỆN]
Toàn bộ kế hoạch: [KẾT QUẢ P1, P2, P3, P4, P5]
Hãy suy nghĩ từng bước và kiểm tra:
1. Tổng ngân sách có vượt 15.000.000 không (cộng lại từng khoản).
2. Quy mô còn 150 sinh viên, nhân sự còn đúng 10 người không.
3. Lịch trình có khớp với phân công không (ban nào phụ trách mà đủ người không).
4. Dự toán có khớp với lịch trình không (có khoản chi nào thiếu hoặc thừa).
5. Truyền thông có khớp với lịch và ngân sách không.
Mỗi mục ghi ĐẠT hoặc KHÔNG ĐẠT kèm lý do. Mục không đạt thì sửa luôn và ghi lại phần đã sửa. Không được tự đổi 150 sinh viên, 10 nhân sự, 15 triệu.
```

**P7: Tóm tắt ngữ cảnh (Context Management)**

```
Dựa trên kế hoạch đã chỉnh sửa ở trên, hãy viết bản tóm tắt dự án để tôi dán sang cuộc trò chuyện mới. Gồm các mục: thông tin cố định, đã chốt (khả thi, lịch trình, ngân sách, phân công, truyền thông), vấn đề đã sửa, việc còn lại. Ngắn gọn, chỉ giữ số liệu và quyết định quan trọng, không lời chào.
```

**Prompt mở chat mới (dùng sau P7)**

```
Dưới đây là bản tóm tắt dự án AI Innovation Day. Hãy ghi nhớ làm cơ sở, không thay đổi các thông tin cố định (150 sinh viên, 10 nhân sự, tối đa 15.000.000 đồng, 1 ngày). Nếu thiếu thông tin thì hỏi lại, không tự bịa.
[DÁN BẢN TÓM TẮT]
Tiếp theo hãy giúp tôi: [VIỆC CẦN LÀM TIẾP]
```

## 3. Kết quả chạy thử

### 3.1. Bản phân tích tính khả thi (P1)

- Bước 1: ngân sách mỗi sinh viên = 15.000.000 / 150 = 100.000 đồng. Đủ cho sự kiện 1 ngày nếu không tốn tiền thuê địa điểm.
- Bước 2: 150 / 10 = 15 sinh viên mỗi người tổ chức. Chấp nhận được, nhưng hai điểm đông nhất là check-in và workshop.
- Bước 3: cần 1 hội trường từ 160 chỗ trở lên (150 sinh viên + 10 người tổ chức), có máy chiếu, âm thanh, wifi. Giả định mượn hội trường của trường miễn phí. Nếu phải thuê thì sẽ vượt ngân sách.
- Bước 4: chương trình 8 tiếng 30 phút, đủ 2 talk, 1 demo, 1 workshop, 1 minigame.
- Bước 5: rủi ro: wifi quá tải khi 150 người thực hành cùng lúc, diễn giả hủy sát ngày, đăng ký nhưng không đến, vượt ngân sách.
- Kết luận: **khả thi có điều kiện**: mượn được hội trường miễn phí và có wifi ổn định hoặc phương án dự phòng.

### 3.2. Lịch trình sự kiện (P2)

| Khung giờ | Nội dung | Ban phụ trách |
|---|---|---|
| 7:00 - 7:30 | Ban tổ chức đến, bày trí, kiểm tra thiết bị | Kỹ thuật, Hậu cần |
| 7:30 - 8:15 | Check-in, phát thẻ tên | Đón tiếp, Hậu cần |
| 8:15 - 8:30 | Khai mạc | Trưởng ban, MC (Nội dung) |
| 8:30 - 9:30 | Talk 1: AI trong học tập | Nội dung, Kỹ thuật |
| 9:30 - 9:45 | Giải lao 1 | Hậu cần |
| 9:45 - 10:45 | Talk 2: AI trong công việc | Nội dung, Kỹ thuật |
| 10:45 - 11:30 | Demo công cụ AI trực tiếp | Nội dung, Kỹ thuật |
| 11:30 - 13:00 | Nghỉ trưa (sinh viên tự túc) | Thư ký kiểm lại hội trường |
| 13:00 - 14:30 | Workshop thực hành, 15 nhóm, mỗi nhóm 10 sinh viên | Nội dung, cả ban tổ chức hỗ trợ |
| 14:30 - 14:45 | Giải lao 2 | Hậu cần |
| 14:45 - 15:45 | Minigame "AI Challenge" | Nội dung, Truyền thông |
| 15:45 - 16:15 | Trao giải, khảo sát | Trưởng ban, Thư ký |
| 16:15 - 16:30 | Bế mạc, dọn dẹp | Tất cả |

### 3.3. Bảng dự toán ngân sách (P3 và sau hiệu chỉnh P6)

| Hạng mục | Phép tính | Bản nháp P3 | Sau hiệu chỉnh |
|---|---|---|---|
| Diễn giả (hỗ trợ đi lại, quà cảm ơn) | 2 người x 1.000.000 | 2.000.000 | 2.000.000 |
| Giải lao 2 lần (nước, bánh) | 160 người x 2 lần x giá | 5.600.000 (35.000/người) | 4.800.000 (30.000/người) |
| In ấn (backdrop, standee, thẻ tên, poster) | trọn gói | 2.500.000 | 2.200.000 |
| Quà và giải thưởng | trọn gói | 2.000.000 | 2.000.000 |
| Âm thanh, micro thuê thêm | trọn gói | 1.000.000 | 1.000.000 |
| Truyền thông (quảng cáo nhỏ) | trọn gói | 500.000 | 500.000 |
| Áo đồng phục ban tổ chức | 10 người x 100.000 | 1.000.000 | 1.000.000 |
| Dự phòng | | 1.500.000 | 1.200.000 |
| **Tổng** | | **16.100.000 (vượt 1.100.000)** | **14.700.000 (dư 300.000)** |

Cách cộng sau hiệu chỉnh: 2.000.000 + 4.800.000 + 2.200.000 + 2.000.000 + 1.000.000 + 500.000 + 1.000.000 + 1.200.000 = 14.700.000, nhỏ hơn 15.000.000.

### 3.4. Bảng phân công 10 nhân sự (P4)

| STT | Vai trò | Nhiệm vụ chính |
|---|---|---|
| 1 | Trưởng ban tổ chức | Điều phối chung, duyệt chi, làm việc với nhà trường |
| 2 | Thư ký | Giữ lịch trình, theo dõi ngân sách, tổng hợp báo cáo |
| 3 | Ban nội dung 1 | Mời diễn giả, làm MC, chuẩn bị minigame |
| 4 | Ban nội dung 2 | Chuẩn bị workshop, demo, hướng dẫn các nhóm |
| 5 | Ban kỹ thuật | Âm thanh, máy chiếu, wifi, phương án dự phòng |
| 6 | Ban hậu cần 1 | Giải lao, bố trí hội trường, hỗ trợ check-in |
| 7 | Ban hậu cần 2 | In ấn, thẻ tên, quà, hỗ trợ check-in |
| 8 | Ban truyền thông 1 | Poster, fanpage, quản lý form đăng ký |
| 9 | Ban truyền thông 2 | Chụp ảnh, quay video, đăng bài trong ngày |
| 10 | Ban đón tiếp | Check-in, phát thẻ tên, hướng dẫn chỗ ngồi |

Tổng: 1 + 1 + 2 + 1 + 2 + 2 + 1 = 10 người.

### 3.5. Kế hoạch truyền thông trước sự kiện (P5)

D là ngày tổ chức. Kênh: fanpage câu lạc bộ, nhóm Facebook và Zalo của khoa, lớp, poster tại trường.

| Mốc | Nội dung | Phụ trách |
|---|---|---|
| D-14 | Đăng poster, teaser, mở form đăng ký 150 chỗ | Truyền thông 1 |
| D-12 | Bài giới thiệu 2 diễn giả | Truyền thông 1, Nội dung 1 |
| D-10 | Bài giới thiệu workshop và minigame | Truyền thông 1, Nội dung 2 |
| D-7 | Đăng nhắc trong các nhóm khoa, lớp, dán poster | Truyền thông 2, Hậu cần 2 |
| D-5 | Minigame nhỏ trên fanpage để tăng tương tác | Truyền thông 1 |
| D-3 | Đóng đăng ký khi đủ 150, mở danh sách chờ | Truyền thông 1, Thư ký |
| D-2 | Chốt danh sách, in thẻ tên, gửi tin xác nhận (mang laptop hoặc điện thoại) | Hậu cần 2, Truyền thông 1 |
| D-1 | Nhắc giờ, địa điểm, sơ đồ hội trường | Truyền thông 2 |

Chi phí truyền thông: 500.000 đồng (quảng cáo nhỏ), poster tính trong khoản in ấn.

### 3.6. Báo cáo kiểm tra và hiệu chỉnh (P6)

| Tiêu chí | Kết quả | Ghi chú |
|---|---|---|
| Tổng ngân sách không vượt 15 triệu | KHÔNG ĐẠT (bản nháp) | Bản nháp 16.100.000, vượt 1.100.000. Đã sửa còn 14.700.000 |
| Quy mô 150 sinh viên, 10 nhân sự | ĐẠT | Không thay đổi |
| Lịch trình khớp phân công | KHÔNG ĐẠT (bản nháp) | Check-in 7:30 đến 8:15 chỉ có 1 người đón tiếp cho 150 sinh viên. Đã sửa |
| Dự toán khớp lịch trình | ĐẠT | Có đủ giải lao 2 lần, thiết bị, diễn giả, quà cho minigame |
| Truyền thông khớp lịch và ngân sách | KHÔNG ĐẠT (bản nháp) | Chưa có mốc đóng đăng ký trước khi in thẻ tên. Đã sửa |

Các vấn đề và cách sửa:

1. **Vượt ngân sách 1.100.000:** giảm giải lao từ 35.000 xuống 30.000 mỗi người (giảm 800.000), giảm in ấn 300.000, giảm dự phòng 300.000. Tổng giảm 1.400.000, còn 14.700.000.
2. **Check-in thiếu người:** 2 người ban hậu cần hỗ trợ check-in từ 7:30 đến 8:15, sau đó quay lại việc của mình. Không thêm người, vẫn đúng 10 nhân sự.
3. **Truyền thông chưa khớp:** đóng đăng ký ở D-3, in thẻ tên ở D-2, để thẻ tên in đúng 150 người.
4. **Rủi ro wifi:** ban kỹ thuật chuẩn bị bản demo offline, sinh viên dự phòng dùng 4G cá nhân.

Kết luận sau hiệu chỉnh: kế hoạch thống nhất, ngân sách 14.700.000 đồng, 150 sinh viên, 10 nhân sự.

### 3.7. Bản tóm tắt ngữ cảnh dự án (P7)

```
TÓM TẮT DỰ ÁN AI INNOVATION DAY

THÔNG TIN CỐ ĐỊNH
- Câu lạc bộ công nghệ, sinh viên trong trường, 150 sinh viên, 1 ngày
- Ngân sách tối đa 15.000.000 đồng, 10 nhân sự tổ chức
- Mục tiêu: giúp sinh viên hiểu và trải nghiệm ứng dụng AI trong học tập và công việc

ĐÃ CHỐT
- Khả thi có điều kiện: mượn hội trường miễn phí (từ 160 chỗ), có wifi hoặc phương án dự phòng
- Lịch trình: 7:00 đến 16:30. Gồm check-in, khai mạc, 2 talk, demo, nghỉ trưa tự túc, workshop 15 nhóm (mỗi nhóm 10 sinh viên), minigame, trao giải, bế mạc
- Ngân sách: 14.700.000 đồng (diễn giả 2.000.000, giải lao 4.800.000, in ấn 2.200.000, quà 2.000.000, âm thanh 1.000.000, truyền thông 500.000, áo 1.000.000, dự phòng 1.200.000), dư 300.000
- Nhân sự: trưởng ban 1, thư ký 1, nội dung 2, kỹ thuật 1, hậu cần 2, truyền thông 2, đón tiếp 1
- Truyền thông: từ D-14 đến D-1, đóng đăng ký D-3, in thẻ tên D-2

VẤN ĐỀ ĐÃ SỬA
- Ngân sách nháp vượt 1.100.000 nên đã giảm giải lao, in ấn, dự phòng
- Check-in thiếu người nên 2 người hậu cần hỗ trợ
- Thêm mốc đóng đăng ký trước khi in thẻ tên

VIỆC CÒN LẠI
- Xác nhận được mượn hội trường và wifi
- Chốt 2 diễn giả
- Chuẩn bị nội dung workshop và minigame
```

## 4. Đối chiếu yêu cầu

| Yêu cầu | Đáp ứng |
|---|---|
| Dùng cả 3 kỹ thuật | CoT (P1, P3, P6), Prompt Chaining (P1 đến P7), Context Management (P7 và prompt mở chat mới) |
| Không vượt 15 triệu | 14.700.000 đồng |
| Giữ 150 sinh viên, 10 nhân sự | Không đổi |
| Các hạng mục liên kết | Lịch, dự toán, phân công, truyền thông dùng kết quả của nhau |
| Có bước kiểm tra và hiệu chỉnh | P6, đã phát hiện và sửa 3 vấn đề chính |

Ghi chú: các kết quả ở trên là chạy mẫu. Khi nộp nên chạy thật các prompt rồi dán kết quả của AI vào. Giả định hội trường mượn miễn phí cần được xác nhận.
