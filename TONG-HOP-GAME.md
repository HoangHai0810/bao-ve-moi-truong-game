# FILE TỔNG HỢP TOÀN BỘ — GAME “CHIẾN BINH XANH: BẢO VỆ MÔI TRƯỜNG”

> **Mục đích của file này:** Đây là tài liệu tổng hợp *toàn bộ* dự án — ý tưởng, luật chơi, toàn bộ câu hỏi, định dạng dữ liệu, kiến trúc kỹ thuật, cách triển khai và **toàn bộ mã nguồn** — gói trong MỘT file duy nhất.
> Người nhận (hoặc một AI như ChatGPT / Claude / Gemini) chỉ cần đọc file này là có thể:
> 1. Hiểu trọn vẹn game hiện tại,
> 2. Dựng lại game giống hệt bản gốc,
> 3. Hoặc phát triển tiếp (thêm tính năng, thêm câu hỏi, đổi giao diện) mà không cần hỏi lại người bàn giao.
>
> **Cách dùng với AI:** tải file này lên cuộc trò chuyện với AI, rồi ra lệnh bằng các mẫu prompt ở **Mục 9**.

---

## MỤC LỤC

1. Thông tin dự án
2. Ý tưởng & mục tiêu
3. Luật chơi chi tiết
4. Cách tính điểm & xếp hạng
5. Các màn hình & luồng chơi
6. Toàn bộ bộ câu hỏi gốc (20 câu — bản đọc dễ)
7. Định dạng dữ liệu câu hỏi (JSON — bản cho máy/AI)
8. Kiến trúc kỹ thuật & tính năng “đổi câu hỏi”
9. Hướng dẫn triển khai (deploy) lên GitHub Pages
10. Lộ trình phát triển tiếp (gợi ý cho người nhận)
11. Mẫu prompt ra lệnh cho AI
12. Phụ lục: Toàn bộ mã nguồn `index.html` bản gốc

---

## 1. THÔNG TIN DỰ ÁN

| Hạng mục | Nội dung |
|---|---|
| Tên game | **Chiến Binh Xanh** (Chiến Binh Xanh — Trò Chơi Bảo Vệ Môi Trường) |
| Chủ đề | **Bảo vệ môi trường** |
| Thể loại | Trò chơi trắc nghiệm (quiz) giáo dục, chơi trên trình duyệt web |
| Đối tượng người chơi | Học sinh, thiếu nhi và mọi người yêu môi trường (tiếng Việt) |
| Link chơi trực tiếp | https://hoanghai0810.github.io/bao-ve-moi-truong-game/ |
| Kho mã nguồn (GitHub) | https://github.com/HoangHai0810/bao-ve-moi-truong-game |
| Công nghệ | HTML + CSS + JavaScript thuần, **1 file duy nhất** `index.html`, không cần build, không cần server |
| Lưu trữ dữ liệu | `localStorage` của trình duyệt (bộ câu hỏi tùy chỉnh) — không có backend, không có tài khoản |
| Ngày tạo bản gốc | 20/11/2026 |
| Trạng thái | Đã deploy, đã chơi thử trọn vẹn 1 ván trên link live, không lỗi |

## 2. Ý TƯỞNG & MỤC TIÊU

- Người chơi hóa thân thành “Chiến Binh Xanh”: trả lời đúng các câu hỏi về môi trường để **gieo mầm và nuôi một cây xanh lớn lên**, tượng trưng cho việc cứu hành tinh.
- Học mà chơi: sau mỗi câu trả lời đều có **đoạn giải thích ngắn** giúp người chơi ghi nhớ kiến thức (Ngày Môi trường Thế giới, phân loại rác, tác hại túi ni lông, xử lý pin cũ, tiết kiệm điện nước, năng lượng tái tạo…).
- Điểm nhấn của dự án: **bộ câu hỏi đổi được** — giáo viên / người quản trị có thể sửa, thêm, xóa câu hỏi ngay trong game mà không cần biết lập trình, hoặc nhờ AI tạo bộ câu hỏi mới theo đúng định dạng ở Mục 7 rồi nhập vào game.

## 3. LUẬT CHƠI CHI TIẾT

1. Ở màn hình chính, người chơi chọn số câu cho mỗi lượt: **10 câu**, **15 câu** hoặc **Tất cả câu hỏi** trong ngân hàng câu hỏi.
2. Bấm **“Bắt đầu chơi”**: hệ thống **xáo trộn ngẫu nhiên** thứ tự câu hỏi; trong mỗi câu, **thứ tự 4 đáp án cũng được xáo trộn** (vị trí đáp án đúng thay đổi mỗi lần chơi).
3. Mỗi câu hỏi có **20 giây** để trả lời (có thanh đếm ngược; dưới 5 giây thanh chuyển đỏ). Hết giờ = tính là sai câu đó.
4. Sau khi chọn đáp án:
   - Đúng: đáp án tô xanh, hiện giải thích, cộng điểm, cây xanh lớn thêm.
   - Sai: đáp án đã chọn tô đỏ, đáp án đúng được tô xanh, hiện giải thích, chuỗi đúng liên tiếp bị reset về 0.
5. Hình ảnh cây phát triển theo tỉ lệ số câu đúng / tổng số câu của lượt chơi:
   - 0 câu đúng: 🌰 (hạt giống)
   - \> 0%: 🌱 (mầm)
   - ≥ 35%: 🌿
   - ≥ 60%: 🌲
   - ≥ 85%: 🌳 (cây sum suê)
6. Hết lượt chơi → màn hình kết quả: tổng điểm, số câu đúng / tổng số câu, chuỗi đúng dài nhất, danh hiệu (Mục 4), nút **Chơi lại** (xáo trộn lại từ đầu), **Về màn hình chính**, **Đổi câu hỏi**.
7. Trong lúc chơi có nút **Dừng chơi** để thoát về màn hình chính.
8. Có âm thanh báo đúng / sai / thắng bằng WebAudio (tự sinh tiếng, không dùng file âm thanh ngoài).

## 4. CÁCH TÍNH ĐIỂM & XẾP HẠNG

**Công thức điểm mỗi câu đúng:**

```
Điểm câu = 100 + (số giây còn lại × 5) + (20 nếu đang đúng ≥ 3 câu liên tiếp)
```

- Trả lời sai hoặc hết giờ: **0 điểm** câu đó.
- Ví dụ: trả lời đúng khi còn 15 giây → 100 + 75 = **175 điểm**.

**Danh hiệu cuối ván (theo tỉ lệ câu đúng):**

| Tỉ lệ đúng | Danh hiệu |
|---|---|
| ≥ 90% | 🌍 Đại Sứ Hành Tinh Xanh |
| ≥ 70% | 🌳 Chiến Binh Xanh |
| ≥ 45% | 🌿 Mầm Xanh Đang Lớn |
| < 45% | 🌱 Hạt Mầm Xanh |

## 5. CÁC MÀN HÌNH & LUỒNG CHƠI

```
[Màn hình chính] ──Bắt đầu chơi──▶ [Màn hình chơi] ──hết câu──▶ [Màn hình kết quả]
       │                                  │                              │
       └──Đổi câu hỏi──▶ [Màn hình sửa câu hỏi] ◀────Đổi câu hỏi────────┘
```

1. **Màn hình chính:** logo 🌍, tên game, khung hướng dẫn cách chơi, chọn số câu, chip hiển thị tổng số câu hỏi hiện có trong ngân hàng, nút Bắt đầu chơi, nút Đổi câu hỏi, nút Cách tính điểm.
2. **Màn hình chơi:** HUD gồm số câu (ví dụ 3/10), điểm, chuỗi đúng liên tiếp 🔥, thời gian còn lại; thanh đếm ngược; cây xanh + dòng trạng thái; câu hỏi; 4 nút đáp án A/B/C/D; khung giải thích; nút Câu tiếp theo; nút Dừng chơi.
3. **Màn hình kết quả:** emoji + danh hiệu, điểm lớn, chi tiết (đúng x/y câu, chuỗi dài nhất), thông điệp động viên, nút Chơi lại / Về màn hình chính / Đổi câu hỏi.
4. **Màn hình sửa câu hỏi (“Đổi câu hỏi”):** danh sách toàn bộ câu hỏi dạng form sửa trực tiếp (nội dung câu hỏi, 4 đáp án, nút tròn chọn đáp án đúng, ô giải thích, nút xóa từng câu), nút Thêm câu hỏi mới, Lưu bộ câu hỏi, Khôi phục câu hỏi gốc, Xuất file JSON, Nhập file JSON.

---

## 6. TOÀN BỘ BỘ CÂU HỎI GỐC (20 CÂU — BẢN ĐỌC DỄ)

Đáp án đúng được đánh dấu ✅. Phần giải thích là nội dung hiện cho người chơi sau mỗi câu.

**Câu 1. Ngày Môi trường Thế giới được tổ chức vào ngày nào hằng năm?**

- A. Ngày 5 tháng 6 ✅
- B. Ngày 1 tháng 1
- C. Ngày 30 tháng 4
- D. Ngày 22 tháng 12

*Giải thích: Ngày 5/6 là Ngày Môi trường Thế giới do Liên Hợp Quốc phát động, nhắc mọi người cùng hành động vì môi trường.*

**Câu 2. Một chiếc túi ni lông dùng vài phút nhưng mất khoảng bao lâu để phân hủy hoàn toàn trong tự nhiên?**

- A. Khoảng 1 tuần
- B. Khoảng 1 tháng
- C. Khoảng 1 năm
- D. Từ 500 đến 1.000 năm ✅

*Giải thích: Túi ni lông cực kỳ khó phân hủy, có thể tồn tại 500–1.000 năm. Hãy mang túi vải khi đi chợ nhé!*

**Câu 3. Ba chữ R trong bảo vệ môi trường (3R) có nghĩa là gì?**

- A. Read – Run – Rest (Đọc – Chạy – Nghỉ)
- B. Reduce – Reuse – Recycle (Giảm dùng – Tái sử dụng – Tái chế) ✅
- C. Red – Round – River (Đỏ – Tròn – Sông)
- D. Rain – Road – Rock (Mưa – Đường – Đá)

*Giải thích: 3R = Reduce (giảm dùng), Reuse (tái sử dụng), Recycle (tái chế) — nguyên tắc vàng để giảm rác thải.*

**Câu 4. Cây xanh giúp làm sạch không khí bằng cách nào?**

- A. Hấp thụ khí CO₂ và nhả ra khí ôxy ✅
- B. Làm cho trời mưa nhiều hơn
- C. Che hết ánh nắng mặt trời
- D. Đuổi hết các loài chim

*Giải thích: Khi quang hợp, cây hấp thụ CO₂ — khí gây biến đổi khí hậu — và nhả ra ôxy cho chúng ta hít thở.*

**Câu 5. Pin cũ, pin hỏng nên được xử lý như thế nào?**

- A. Vứt chung vào thùng rác sinh hoạt
- B. Chôn xuống vườn
- C. Mang đến điểm thu gom rác thải nguy hại / hộp thu hồi pin ✅
- D. Ném xuống sông cho trôi đi

*Giải thích: Pin chứa chì, thủy ngân, cadmi rất độc. Vứt bừa bãi sẽ ngấm vào đất và nước. Hãy mang pin cũ đến điểm thu gom riêng.*

**Câu 6. Hành động nào sau đây giúp tiết kiệm điện nhất?**

- A. Bật điều hòa 16°C cả ngày
- B. Tắt điện, rút phích cắm khi không sử dụng ✅
- C. Mở tủ lạnh liên tục cho mát phòng
- D. Bật tất cả đèn trong nhà

*Giải thích: Tắt và rút phích cắm khi không dùng giúp giảm điện lãng phí — nhiều thiết bị vẫn 'ăn điện ngầm' khi cắm chờ.*

**Câu 7. Rác hữu cơ gồm những loại nào?**

- A. Vỏ trái cây, rau thừa, lá cây ✅
- B. Chai nhựa, lon nhôm
- C. Pin, bóng đèn hỏng
- D. Túi ni lông, xốp

*Giải thích: Rác hữu cơ là rác từ thực phẩm và cây cối, phân hủy nhanh và có thể ủ thành phân bón cho cây.*

**Câu 8. Vì sao không nên đốt rác nhựa và xốp ngoài trời?**

- A. Vì lửa không cháy được nhựa
- B. Vì khói đốt nhựa sinh ra khí độc gây bệnh ✅
- C. Vì nhựa rất đắt tiền
- D. Vì đốt rác làm trời tối đi

*Giải thích: Đốt nhựa, xốp sinh ra khí độc như dioxin, gây hại cho phổi và sức khỏe. Rác nhựa nên được thu gom, tái chế.*

**Câu 9. Năng lượng nào sau đây là năng lượng tái tạo, sạch và không bao giờ cạn?**

- A. Than đá
- B. Dầu mỏ
- C. Năng lượng mặt trời ✅
- D. Khí đốt tự nhiên

*Giải thích: Năng lượng mặt trời, gió, nước là năng lượng tái tạo — sạch và vô tận, khác với than đá, dầu mỏ gây ô nhiễm.*

**Câu 10. Khi đánh răng, khóa vòi nước lại giúp tiết kiệm được khoảng bao nhiêu nước mỗi phút?**

- A. Khoảng 0,1 lít
- B. Khoảng 1 lít
- C. Khoảng 6 lít ✅
- D. Không tiết kiệm được gì

*Giải thích: Để vòi chảy liên tục khi đánh răng có thể lãng phí khoảng 6 lít nước mỗi phút. Khóa vòi là thói quen rất đáng quý!*

**Câu 11. Rùa biển thường nhầm túi ni lông trôi trên biển với thứ gì và ăn phải?**

- A. Con sứa — thức ăn của rùa biển ✅
- B. Con cá nhỏ
- C. Rong biển
- D. San hô

*Giải thích: Túi ni lông trong nước trông giống con sứa. Rùa ăn phải sẽ bị tắc ruột và chết. Đừng xả rác ra biển nhé!*

**Câu 12. Loại bóng đèn nào tiết kiệm điện nhất trong các loại sau?**

- A. Bóng đèn sợi đốt
- B. Bóng đèn huỳnh quang ống dài
- C. Bóng đèn LED ✅
- D. Bóng đèn halogen

*Giải thích: Đèn LED tiết kiệm tới 75–80% điện so với đèn sợi đốt và dùng được lâu hơn nhiều lần.*

**Câu 13. Rừng ngập mặn ở ven biển có vai trò quan trọng nào?**

- A. Chắn sóng, chống xói lở và là nhà của nhiều loài tôm cá ✅
- B. Làm nước biển mặn hơn
- C. Để lấy gỗ xây nhà là chính
- D. Không có vai trò gì

*Giải thích: Rừng ngập mặn như 'bức tường xanh' chắn sóng bão, giữ đất và là nơi sinh sống của nhiều loài thủy sản.*

**Câu 14. Khí nào là nguyên nhân chính gây hiệu ứng nhà kính và biến đổi khí hậu?**

- A. Khí ôxy (O₂)
- B. Khí CO₂ (cacbonic) ✅
- C. Khí nitơ (N₂)
- D. Hơi nước sạch

*Giải thích: CO₂ từ đốt xăng, than, dầu giữ nhiệt trong khí quyển làm Trái Đất nóng lên — đó là biến đổi khí hậu.*

**Câu 15. Việc làm nào giúp giảm rác thải nhựa dùng một lần hiệu quả?**

- A. Dùng bình nước cá nhân và túi vải thay đồ nhựa dùng một lần ✅
- B. Mua thêm nhiều túi ni lông dự trữ
- C. Dùng ống hút nhựa cho mọi ly nước
- D. Vứt ly nhựa ngay sau một lần uống

*Giải thích: Mang bình nước cá nhân, túi vải, hộp cơm riêng giúp cắt giảm rất nhiều nhựa dùng một lần mỗi ngày.*

**Câu 16. Giấy đã qua sử dụng nên làm gì để bảo vệ rừng?**

- A. Đốt bỏ ngay
- B. Thu gom để tái chế giấy ✅
- C. Vứt xuống cống
- D. Chôn xuống đất

*Giải thích: Tái chế 1 tấn giấy giúp cứu khoảng 17 cây xanh và tiết kiệm nhiều nước, điện so với làm giấy mới.*

**Câu 17. Khi in tài liệu, cách nào giúp tiết kiệm giấy?**

- A. Chỉ in một mặt
- B. In hai mặt và chỉ in khi thật cần thiết ✅
- C. In thật nhiều bản dự phòng
- D. In màu cho mọi trang

*Giải thích: In hai mặt giúp giảm một nửa lượng giấy. Trước khi in, hãy hỏi: 'Mình có thật sự cần bản giấy này không?'*

**Câu 18. Đi lại bằng phương tiện nào thân thiện với môi trường nhất cho quãng đường ngắn?**

- A. Đi xe máy một mình
- B. Đi ô tô riêng
- C. Đi xe đạp hoặc đi bộ ✅
- D. Đi taxi

*Giải thích: Xe đạp và đi bộ không xả khói bụi, lại tốt cho sức khỏe. Quãng ngắn, hãy ưu tiên 'đôi chân xanh'!*

**Câu 19. Nước thải sinh hoạt chưa qua xử lý đổ thẳng ra sông sẽ gây hậu quả gì?**

- A. Làm sông sạch hơn
- B. Ô nhiễm nguồn nước, cá tôm chết, người dân thiếu nước sạch ✅
- C. Làm cá lớn nhanh hơn
- D. Không ảnh hưởng gì

*Giải thích: Nước thải bẩn làm sông hồ ô nhiễm, sinh vật chết và con người mất nguồn nước sạch để sinh hoạt.*

**Câu 20. Một hành động đơn giản mà học sinh có thể làm mỗi ngày để bảo vệ môi trường là gì?**

- A. Xả rác đúng nơi quy định và phân loại rác tại nguồn ✅
- B. Mang nhiều túi ni lông về nhà
- C. Để nước chảy tràn khi rửa tay
- D. Bẻ cành cây trong sân trường

*Giải thích: Bỏ rác đúng chỗ, phân loại rác, tắt điện nước khi không dùng — việc nhỏ mỗi ngày tạo thay đổi lớn!*


---

## 7. ĐỊNH DẠNG DỮ LIỆU CÂU HỎI (JSON — BẢN CHO MÁY / AI)

Toàn bộ ngân hàng câu hỏi là **một mảng JSON**. Mỗi phần tử là 1 câu hỏi với đúng 4 trường:

| Trường | Kiểu | Ý nghĩa |
|---|---|---|
| `q` | chuỗi | Nội dung câu hỏi (tiếng Việt, có dấu đầy đủ) |
| `options` | mảng 4 chuỗi | 4 đáp án |
| `answer` | số nguyên 0–3 | Vị trí đáp án đúng trong `options`, **đếm từ 0** (0 = đáp án đầu tiên) |
| `explain` | chuỗi | Giải thích ngắn (1–2 câu) hiện sau khi trả lời |

**Quy tắc khi tạo câu hỏi mới (cho người hoặc AI):**

- Mỗi câu đúng 4 đáp án, chỉ 1 đáp án đúng; các đáp án sai phải “có vẻ hợp lý” nhưng sai rõ ràng khi biết kiến thức.
- Kiến thức phải chính xác, phù hợp học sinh Việt Nam; ưu tiên chủ đề: phân loại rác, tái chế, túi ni lông, pin thải, tiết kiệm điện/nước, trồng cây, năng lượng tái tạo, bảo vệ biển/rừng, biến đổi khí hậu.
- `explain` bắt buộc có, viết thân thiện, khuyến khích hành động.
- File JSON xuất từ game (nút “Xuất file JSON”) và file nhập vào game dùng đúng định dạng này.

**Toàn bộ 20 câu hỏi gốc ở định dạng JSON (copy khối này cho AI hoặc nhập thẳng vào game):**

```json

[
  {
    "q": "Ngày Môi trường Thế giới được tổ chức vào ngày nào hằng năm?",
    "options": [
      "Ngày 5 tháng 6",
      "Ngày 1 tháng 1",
      "Ngày 30 tháng 4",
      "Ngày 22 tháng 12"
    ],
    "answer": 0,
    "explain": "Ngày 5/6 là Ngày Môi trường Thế giới do Liên Hợp Quốc phát động, nhắc mọi người cùng hành động vì môi trường."
  },
  {
    "q": "Một chiếc túi ni lông dùng vài phút nhưng mất khoảng bao lâu để phân hủy hoàn toàn trong tự nhiên?",
    "options": [
      "Khoảng 1 tuần",
      "Khoảng 1 tháng",
      "Khoảng 1 năm",
      "Từ 500 đến 1.000 năm"
    ],
    "answer": 3,
    "explain": "Túi ni lông cực kỳ khó phân hủy, có thể tồn tại 500–1.000 năm. Hãy mang túi vải khi đi chợ nhé!"
  },
  {
    "q": "Ba chữ R trong bảo vệ môi trường (3R) có nghĩa là gì?",
    "options": [
      "Read – Run – Rest (Đọc – Chạy – Nghỉ)",
      "Reduce – Reuse – Recycle (Giảm dùng – Tái sử dụng – Tái chế)",
      "Red – Round – River (Đỏ – Tròn – Sông)",
      "Rain – Road – Rock (Mưa – Đường – Đá)"
    ],
    "answer": 1,
    "explain": "3R = Reduce (giảm dùng), Reuse (tái sử dụng), Recycle (tái chế) — nguyên tắc vàng để giảm rác thải."
  },
  {
    "q": "Cây xanh giúp làm sạch không khí bằng cách nào?",
    "options": [
      "Hấp thụ khí CO₂ và nhả ra khí ôxy",
      "Làm cho trời mưa nhiều hơn",
      "Che hết ánh nắng mặt trời",
      "Đuổi hết các loài chim"
    ],
    "answer": 0,
    "explain": "Khi quang hợp, cây hấp thụ CO₂ — khí gây biến đổi khí hậu — và nhả ra ôxy cho chúng ta hít thở."
  },
  {
    "q": "Pin cũ, pin hỏng nên được xử lý như thế nào?",
    "options": [
      "Vứt chung vào thùng rác sinh hoạt",
      "Chôn xuống vườn",
      "Mang đến điểm thu gom rác thải nguy hại / hộp thu hồi pin",
      "Ném xuống sông cho trôi đi"
    ],
    "answer": 2,
    "explain": "Pin chứa chì, thủy ngân, cadmi rất độc. Vứt bừa bãi sẽ ngấm vào đất và nước. Hãy mang pin cũ đến điểm thu gom riêng."
  },
  {
    "q": "Hành động nào sau đây giúp tiết kiệm điện nhất?",
    "options": [
      "Bật điều hòa 16°C cả ngày",
      "Tắt điện, rút phích cắm khi không sử dụng",
      "Mở tủ lạnh liên tục cho mát phòng",
      "Bật tất cả đèn trong nhà"
    ],
    "answer": 1,
    "explain": "Tắt và rút phích cắm khi không dùng giúp giảm điện lãng phí — nhiều thiết bị vẫn 'ăn điện ngầm' khi cắm chờ."
  },
  {
    "q": "Rác hữu cơ gồm những loại nào?",
    "options": [
      "Vỏ trái cây, rau thừa, lá cây",
      "Chai nhựa, lon nhôm",
      "Pin, bóng đèn hỏng",
      "Túi ni lông, xốp"
    ],
    "answer": 0,
    "explain": "Rác hữu cơ là rác từ thực phẩm và cây cối, phân hủy nhanh và có thể ủ thành phân bón cho cây."
  },
  {
    "q": "Vì sao không nên đốt rác nhựa và xốp ngoài trời?",
    "options": [
      "Vì lửa không cháy được nhựa",
      "Vì khói đốt nhựa sinh ra khí độc gây bệnh",
      "Vì nhựa rất đắt tiền",
      "Vì đốt rác làm trời tối đi"
    ],
    "answer": 1,
    "explain": "Đốt nhựa, xốp sinh ra khí độc như dioxin, gây hại cho phổi và sức khỏe. Rác nhựa nên được thu gom, tái chế."
  },
  {
    "q": "Năng lượng nào sau đây là năng lượng tái tạo, sạch và không bao giờ cạn?",
    "options": [
      "Than đá",
      "Dầu mỏ",
      "Năng lượng mặt trời",
      "Khí đốt tự nhiên"
    ],
    "answer": 2,
    "explain": "Năng lượng mặt trời, gió, nước là năng lượng tái tạo — sạch và vô tận, khác với than đá, dầu mỏ gây ô nhiễm."
  },
  {
    "q": "Khi đánh răng, khóa vòi nước lại giúp tiết kiệm được khoảng bao nhiêu nước mỗi phút?",
    "options": [
      "Khoảng 0,1 lít",
      "Khoảng 1 lít",
      "Khoảng 6 lít",
      "Không tiết kiệm được gì"
    ],
    "answer": 2,
    "explain": "Để vòi chảy liên tục khi đánh răng có thể lãng phí khoảng 6 lít nước mỗi phút. Khóa vòi là thói quen rất đáng quý!"
  },
  {
    "q": "Rùa biển thường nhầm túi ni lông trôi trên biển với thứ gì và ăn phải?",
    "options": [
      "Con sứa — thức ăn của rùa biển",
      "Con cá nhỏ",
      "Rong biển",
      "San hô"
    ],
    "answer": 0,
    "explain": "Túi ni lông trong nước trông giống con sứa. Rùa ăn phải sẽ bị tắc ruột và chết. Đừng xả rác ra biển nhé!"
  },
  {
    "q": "Loại bóng đèn nào tiết kiệm điện nhất trong các loại sau?",
    "options": [
      "Bóng đèn sợi đốt",
      "Bóng đèn huỳnh quang ống dài",
      "Bóng đèn LED",
      "Bóng đèn halogen"
    ],
    "answer": 2,
    "explain": "Đèn LED tiết kiệm tới 75–80% điện so với đèn sợi đốt và dùng được lâu hơn nhiều lần."
  },
  {
    "q": "Rừng ngập mặn ở ven biển có vai trò quan trọng nào?",
    "options": [
      "Chắn sóng, chống xói lở và là nhà của nhiều loài tôm cá",
      "Làm nước biển mặn hơn",
      "Để lấy gỗ xây nhà là chính",
      "Không có vai trò gì"
    ],
    "answer": 0,
    "explain": "Rừng ngập mặn như 'bức tường xanh' chắn sóng bão, giữ đất và là nơi sinh sống của nhiều loài thủy sản."
  },
  {
    "q": "Khí nào là nguyên nhân chính gây hiệu ứng nhà kính và biến đổi khí hậu?",
    "options": [
      "Khí ôxy (O₂)",
      "Khí CO₂ (cacbonic)",
      "Khí nitơ (N₂)",
      "Hơi nước sạch"
    ],
    "answer": 1,
    "explain": "CO₂ từ đốt xăng, than, dầu giữ nhiệt trong khí quyển làm Trái Đất nóng lên — đó là biến đổi khí hậu."
  },
  {
    "q": "Việc làm nào giúp giảm rác thải nhựa dùng một lần hiệu quả?",
    "options": [
      "Dùng bình nước cá nhân và túi vải thay đồ nhựa dùng một lần",
      "Mua thêm nhiều túi ni lông dự trữ",
      "Dùng ống hút nhựa cho mọi ly nước",
      "Vứt ly nhựa ngay sau một lần uống"
    ],
    "answer": 0,
    "explain": "Mang bình nước cá nhân, túi vải, hộp cơm riêng giúp cắt giảm rất nhiều nhựa dùng một lần mỗi ngày."
  },
  {
    "q": "Giấy đã qua sử dụng nên làm gì để bảo vệ rừng?",
    "options": [
      "Đốt bỏ ngay",
      "Thu gom để tái chế giấy",
      "Vứt xuống cống",
      "Chôn xuống đất"
    ],
    "answer": 1,
    "explain": "Tái chế 1 tấn giấy giúp cứu khoảng 17 cây xanh và tiết kiệm nhiều nước, điện so với làm giấy mới."
  },
  {
    "q": "Khi in tài liệu, cách nào giúp tiết kiệm giấy?",
    "options": [
      "Chỉ in một mặt",
      "In hai mặt và chỉ in khi thật cần thiết",
      "In thật nhiều bản dự phòng",
      "In màu cho mọi trang"
    ],
    "answer": 1,
    "explain": "In hai mặt giúp giảm một nửa lượng giấy. Trước khi in, hãy hỏi: 'Mình có thật sự cần bản giấy này không?'"
  },
  {
    "q": "Đi lại bằng phương tiện nào thân thiện với môi trường nhất cho quãng đường ngắn?",
    "options": [
      "Đi xe máy một mình",
      "Đi ô tô riêng",
      "Đi xe đạp hoặc đi bộ",
      "Đi taxi"
    ],
    "answer": 2,
    "explain": "Xe đạp và đi bộ không xả khói bụi, lại tốt cho sức khỏe. Quãng ngắn, hãy ưu tiên 'đôi chân xanh'!"
  },
  {
    "q": "Nước thải sinh hoạt chưa qua xử lý đổ thẳng ra sông sẽ gây hậu quả gì?",
    "options": [
      "Làm sông sạch hơn",
      "Ô nhiễm nguồn nước, cá tôm chết, người dân thiếu nước sạch",
      "Làm cá lớn nhanh hơn",
      "Không ảnh hưởng gì"
    ],
    "answer": 1,
    "explain": "Nước thải bẩn làm sông hồ ô nhiễm, sinh vật chết và con người mất nguồn nước sạch để sinh hoạt."
  },
  {
    "q": "Một hành động đơn giản mà học sinh có thể làm mỗi ngày để bảo vệ môi trường là gì?",
    "options": [
      "Xả rác đúng nơi quy định và phân loại rác tại nguồn",
      "Mang nhiều túi ni lông về nhà",
      "Để nước chảy tràn khi rửa tay",
      "Bẻ cành cây trong sân trường"
    ],
    "answer": 0,
    "explain": "Bỏ rác đúng chỗ, phân loại rác, tắt điện nước khi không dùng — việc nhỏ mỗi ngày tạo thay đổi lớn!"
  }
]


```

---

## 8. KIẾN TRÚC KỸ THUẬT & TÍNH NĂNG “ĐỔI CÂU HỎI”

**Cấu trúc file duy nhất `index.html`:**

- Phần `<style>`: toàn bộ giao diện (phông chữ Be Vietnam Pro qua Google Fonts, màu chủ đạo xanh lá `#1d9e57` / xanh đậm `#0f6b38`, thẻ trắng bo góc 20px, tương thích điện thoại).
- Phần thân HTML: 4 khối màn hình (`#screenStart`, `#screenGame`, `#screenResult`, `#screenEditor`), hiển thị/ẩn bằng class `hidden`.
- Phần `<script>`:
  - Hằng số `DEFAULT_QUESTIONS`: ngân hàng 20 câu gốc (đúng nội dung Mục 6–7).
  - `questionBank`: ngân hàng đang dùng = bản tùy chỉnh trong `localStorage` (khóa `chien-binh-xanh-questions-v1`) nếu có, ngược lại dùng bản gốc.
  - Hàm `sanitize()`: kiểm tra/làm sạch dữ liệu câu hỏi khi tải hoặc nhập file (bỏ câu lỗi, giới hạn tối đa 6 đáp án/câu, ép `answer` về khoảng hợp lệ).
  - Luồng chơi: `startGame()` → `nextQuestion()` (xáo trộn câu + đáp án, chạy đồng hồ 20 giây bằng `setInterval` 200ms) → `answer()` / `timeOut()` → `finishGame()` (tính danh hiệu).
  - Điểm: đúng = 100 + giây còn lại × 5 + 20 nếu chuỗi đúng ≥ 3.
  - Cây xanh: hàm `treeStage()` đổi emoji theo tỉ lệ câu đúng (🌰 → 🌱 → 🌿 → 🌲 → 🌳).
  - Âm thanh: hàm `beep()` dùng WebAudio API (đúng: 660Hz→880Hz; sai: 180Hz răng cưa; thắng: hợp âm 523/659/784/1046Hz).
  - Trình sửa câu hỏi: `renderEditor()` dựng form cho từng câu; `collectEditor()` đọc lại form; Lưu → `localStorage`; Xuất JSON qua `Blob` tải xuống; Nhập JSON qua `FileReader`.

**Tính năng “đổi câu hỏi” hoạt động như sau:**

1. Người dùng sửa trong giao diện game → lưu vào `localStorage` của trình duyệt đó (mở lại vẫn còn, chỉ mất khi xóa dữ liệu trình duyệt).
2. Muốn đổi **mặc định cho mọi người chơi**: sửa `DEFAULT_QUESTIONS` trong mã nguồn (Phụ lục, Mục 12) rồi deploy lại — hoặc xuất file JSON từ một máy đã sửa và nhập vào các máy khác.

## 9. HƯỚNG DẪN TRIỂN KHAI (DEPLOY) LÊN GITHUB PAGES

Game là trang tĩnh 1 file nên deploy rất đơn giản:

1. Tạo repository **public** trên GitHub (ví dụ tên `bao-ve-moi-truong-game`).
2. Tải file `index.html` (mã nguồn ở Mục 12) và `README.md` lên thư mục gốc của repo, nhánh `main`.
3. Vào **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main`, thư mục `/ (root)` → Save.
4. Chờ khoảng 1 phút, trang chạy tại địa chỉ: `https://<tên-tài-khoản>.github.io/<tên-repo>/`
5. Mỗi lần push/sửa file trên GitHub, trang tự cập nhật sau ~1 phút.

Bản gốc đang chạy tại: https://hoanghai0810.github.io/bao-ve-moi-truong-game/ (repo: https://github.com/HoangHai0810/bao-ve-moi-truong-game).

## 10. LỘ TRÌNH PHÁT TRIỂN TIẾP (GỢI Ý CHO NGƯỜI NHẬN)

Các ý tưởng mở rộng, xếp theo độ dễ tăng dần — người nhận/AI có thể chọn làm từng mục, giữ nguyên định dạng dữ liệu ở Mục 7:

1. **Thêm câu hỏi theo chủ đề/cấp độ:** chia ngân hàng thành nhóm (Rác thải, Nước, Không khí, Năng lượng, Sinh vật biển…) và mức Dễ/Trung bình/Khó; cho chọn chủ đề trước khi chơi. *(Cần thêm trường `topic` và `level` vào JSON.)*
2. **Bảng xếp hạng:** lưu điểm cao vào `localStorage`, hiển thị top 10 lượt chơi kèm tên người chơi.
3. **Chế độ 2 người chơi / thi theo đội:** luân phiên trả lời, cộng điểm riêng.
4. **Hình ảnh minh họa cho câu hỏi:** thêm trường `image` (đường link ảnh) vào JSON và hiển thị trong màn hình chơi.
5. **Hiệu ứng ăn mừng:** pháo giấy khi đạt danh hiệu cao; cây xanh có hoạt hình lớn dần thay vì đổi emoji.
6. **Âm thanh nền & giọng đọc câu hỏi:** dùng Web Speech API (`speechSynthesis`) đọc câu hỏi cho học sinh nhỏ tuổi.
7. **Chế độ thử thách thời gian toàn ván:** tổng 5 phút cho 20 câu, tính điểm theo thời gian còn lại của cả ván.
8. **Thống kê cho giáo viên:** xuất báo cáo CSV các câu hay sai nhất sau mỗi lượt chơi.
9. **Đa ngôn ngữ:** thêm bản tiếng Anh, chuyển đổi bằng nút EN/VI.
10. **Bản đóng gói lớp học:** mã QR in trên tờ rơi trỏ tới link game; bộ câu hỏi riêng cho từng tuần học, nhập bằng file JSON.

**Nguyên tắc khi phát triển tiếp (giữ đúng tinh thần bản gốc):**

- Giữ game chạy được bằng **1 file HTML tĩnh** nếu có thể; nếu tách file/phình to công nghệ, phải ghi rõ cách deploy mới trong tài liệu.
- Không phá vỡ định dạng JSON ở Mục 7 — bộ câu hỏi cũ phải vẫn nhập vào được bản mới.
- Tiếng Việt có dấu đầy đủ; nội dung kiến thức môi trường phải chính xác, tích cực, phù hợp học sinh.
- Mọi thay đổi về luật chơi/điểm số phải cập nhật lại Mục 3–4 của tài liệu này.

## 11. MẪU PROMPT RA LỆNH CHO AI

Người nhận có thể copy các mẫu sau (kèm theo file này) để AI làm việc:

**Dựng lại game từ đầu:**
> “Đọc file tổng hợp đính kèm. Hãy tạo lại trò chơi đúng như Mục 1–8 mô tả, dùng toàn bộ câu hỏi ở Mục 7 và tham khảo mã nguồn ở Mục 12. Xuất ra một file `index.html` hoàn chỉnh chạy được ngay.”

**Tạo thêm câu hỏi:**
> “Đọc Mục 7 trong file đính kèm. Hãy tạo thêm 10 câu hỏi trắc nghiệm về chủ đề [VD: bảo vệ biển] cho học sinh tiểu học, đúng định dạng JSON trong mục đó, kèm giải thích. Chỉ trả về khối JSON.”

**Thêm tính năng:**
> “Đọc file đính kèm. Hãy thêm tính năng [VD: bảng xếp hạng lưu trong localStorage] vào game, giữ nguyên luật chơi ở Mục 3–4 và định dạng dữ liệu ở Mục 7. Trả về file `index.html` hoàn chỉnh sau khi sửa và giải thích ngắn gọn phần đã thay đổi.”

**Đổi giao diện:**
> “Đọc file đính kèm. Hãy làm lại giao diện game theo phong cách [VD: biển và sinh vật biển], giữ nguyên toàn bộ logic và bộ câu hỏi. Trả về file `index.html` hoàn chỉnh.”

---

## 12. PHỤ LỤC: TOÀN BỘ MÃ NGUỒN `index.html` BẢN GỐC

Toàn bộ mã nguồn đang chạy trên link live (commit `673c92f8`). Copy khối dưới đây lưu thành file tên `index.html` là dựng lại được game y hệt bản gốc:

```html

<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chiến Binh Xanh — Trò Chơi Bảo Vệ Môi Trường</title>
<meta name="description" content="Trò chơi trắc nghiệm bảo vệ môi trường: trả lời đúng để trồng cây, cứu hành tinh. Bộ câu hỏi có thể đổi, sửa, thêm mới ngay trong game.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Be+Vietnam+Pro:wght@400;600;800;900&display=swap" rel="stylesheet">
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --green:#1d9e57; --green-dark:#0f6b38; --green-light:#e7f7ee;
    --sky:#bfe8ff; --ink:#17301f; --muted:#5b7263; --card:#ffffff;
    --wrong:#e5484d; --gold:#f5a623; --radius:20px;
    --shadow:0 10px 30px rgba(15,80,45,.18);
  }
  body{
    font-family:"Be Vietnam Pro",system-ui,-apple-system,"Segoe UI",sans-serif;
    color:var(--ink); min-height:100vh;
    background:linear-gradient(180deg,#aee3ff 0%,#d8f5e3 55%,#eefbeF 100%);
    background-color:#d8f5e3;
  }
  .wrap{max-width:760px;margin:0 auto;padding:18px 14px 48px}
  header.top{text-align:center;padding:14px 0 6px}
  header.top .logo{font-size:52px;line-height:1}
  header.top h1{font-size:clamp(26px,6vw,40px);font-weight:900;color:var(--green-dark);letter-spacing:.2px}
  header.top p{color:var(--muted);font-weight:600;margin-top:6px}
  .card{background:var(--card);border-radius:var(--radius);box-shadow:var(--shadow);padding:22px;margin-top:16px}
  .hidden{display:none!important}
  /* ---------- Start screen ---------- */
  .scene{font-size:64px;text-align:center;letter-spacing:6px;padding:8px 0 2px}
  .howto{background:var(--green-light);border-radius:14px;padding:14px 16px;line-height:1.75;font-size:15px}
  .howto b{color:var(--green-dark)}
  .row{display:flex;gap:10px;flex-wrap:wrap;align-items:center}
  .row.center{justify-content:center}
  label.pick{font-weight:700;color:var(--muted);font-size:14px}
  select{font:inherit;font-weight:700;padding:10px 12px;border-radius:12px;border:2px solid #cfe8d8;background:#fff;color:var(--ink)}
  button{font:inherit;cursor:pointer;border:none;border-radius:14px;font-weight:800;transition:transform .08s,box-shadow .15s,background .15s}
  button:active{transform:scale(.97)}
  .btn-main{background:var(--green);color:#fff;font-size:19px;padding:15px 30px;box-shadow:0 8px 20px rgba(29,158,87,.35);width:100%}
  .btn-main:hover{background:var(--green-dark)}
  .btn-ghost{background:#fff;color:var(--green-dark);border:2px solid var(--green);padding:11px 18px;font-size:15px}
  .btn-ghost:hover{background:var(--green-light)}
  .btn-small{padding:8px 14px;font-size:14px}
  .btn-danger{background:#fff;color:var(--wrong);border:2px solid var(--wrong)}
  .stat-chips{display:flex;gap:8px;justify-content:center;flex-wrap:wrap;margin-top:12px}
  .chip{background:#fff;border:2px solid #d7efe0;color:var(--green-dark);border-radius:999px;padding:6px 14px;font-weight:800;font-size:14px}
  /* ---------- Game screen ---------- */
  .hud{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap}
  .hud .box{background:var(--green-light);border-radius:12px;padding:8px 14px;font-weight:900;color:var(--green-dark);min-width:96px;text-align:center}
  .hud .box small{display:block;font-size:11px;font-weight:700;color:var(--muted);letter-spacing:.6px}
  .timer-wrap{height:12px;background:#e3efe7;border-radius:999px;overflow:hidden;margin:14px 0 4px}
  #timerBar{height:100%;background:var(--green);border-radius:999px;transition:width .2s linear}
  #timerBar.low{background:var(--wrong)}
  .tree-zone{text-align:center;padding:6px 0 0}
  #tree{font-size:58px;line-height:1.15;transition:transform .3s}
  #tree.grow{transform:scale(1.18)}
  .progress-text{font-weight:700;color:var(--muted);font-size:14px}
  #question{font-size:clamp(19px,4.6vw,24px);font-weight:800;line-height:1.45;margin:12px 0 16px}
  .answers{display:grid;gap:10px}
  .ans{
    text-align:left;background:#f4faf6;border:2px solid #d7efe0;color:var(--ink);
    padding:14px 16px;font-size:16px;font-weight:600;border-radius:14px;width:100%;
    display:flex;gap:10px;align-items:flex-start;line-height:1.5;
  }
  .ans .key{flex:0 0 auto;width:28px;height:28px;border-radius:50%;background:var(--green);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:900;font-size:14px;margin-top:1px}
  .ans:hover:not(:disabled){border-color:var(--green);background:var(--green-light)}
  .ans:disabled{cursor:default;opacity:.92}
  .ans.correct{background:#dff7e7;border-color:var(--green)}
  .ans.correct .key{background:var(--green-dark)}
  .ans.wrong{background:#fde8e8;border-color:var(--wrong)}
  .ans.wrong .key{background:var(--wrong)}
  .explain{margin-top:14px;border-radius:14px;padding:13px 15px;line-height:1.65;font-size:15px;display:none}
  .explain.ok{display:block;background:#dff7e7;border:1.5px solid var(--green)}
  .explain.bad{display:block;background:#fdecec;border:1.5px solid var(--wrong)}
  #nextBtn{margin-top:14px;width:100%}
  /* ---------- Result ---------- */
  .result{text-align:center}
  .result .big{font-size:72px}
  .result h2{font-size:28px;font-weight:900;color:var(--green-dark);margin:6px 0}
  .score-big{font-size:44px;font-weight:900;color:var(--green)}
  .rank{display:inline-block;background:#fff6df;border:2px solid var(--gold);color:#8a5b00;font-weight:900;border-radius:999px;padding:8px 18px;margin:10px 0}
  /* ---------- Editor ---------- */
  .editor-head{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap}
  .qedit{border:2px solid #e0f0e6;border-radius:16px;padding:14px;margin-top:12px;background:#fbfefc}
  .qedit textarea,.qedit input[type=text]{width:100%;font:inherit;padding:10px 12px;border:1.5px solid #cfe8d8;border-radius:10px;margin-top:6px;background:#fff}
  .qedit label{font-size:13px;font-weight:800;color:var(--muted);display:block;margin-top:10px}
  .qedit .opts{display:grid;gap:4px}
  .qedit .opt-row{display:flex;gap:8px;align-items:center}
  .qedit .opt-row input[type=text]{flex:1}
  .qedit .opt-row input[type=radio]{width:20px;height:20px;accent-color:var(--green)}
  .notice{background:#fff8e1;border:1.5px solid var(--gold);border-radius:12px;padding:10px 14px;font-size:14px;line-height:1.6;margin-top:12px}
  footer{text-align:center;color:var(--muted);font-size:13px;margin-top:22px;line-height:1.7}
  @media (max-width:520px){ .hud .box{min-width:82px;padding:7px 8px} }
</style>
</head>
<body>
<div class="wrap">
  <header class="top">
    <div class="logo">🌍</div>
    <h1>CHIẾN BINH XANH</h1>
    <p>Trò chơi trắc nghiệm Bảo vệ Môi trường — trả lời đúng, trồng cây, cứu hành tinh!</p>
  </header>

  <!-- ======= MÀN HÌNH BẮT ĐẦU ======= -->
  <section id="screenStart" class="card">
    <div class="scene">🌱 🌳 🦋 🌻 🐢</div>
    <div class="howto">
      🎮 <b>Cách chơi:</b> Mỗi câu trả lời đúng, bạn gieo thêm một mầm xanh cho Trái Đất và ghi điểm.<br>
      ⏱️ Mỗi câu có <b>20 giây</b>. Trả lời càng nhanh, điểm thưởng càng cao.<br>
      🌳 Trả lời đúng liên tiếp để cây lớn nhanh — trả lời sai, cây sẽ héo mất một lá!<br>
      ⚙️ Bộ câu hỏi <b>đổi được</b>: bấm nút “Đổi câu hỏi” ở dưới để sửa, thêm, xóa câu hỏi theo ý bạn.
    </div>
    <div class="row center" style="margin-top:16px">
      <label class="pick" for="selCount">Số câu mỗi lượt:</label>
      <select id="selCount">
        <option value="10">10 câu</option>
        <option value="15">15 câu</option>
        <option value="all">Tất cả câu hỏi</option>
      </select>
      <span class="chip" id="bankInfo">📚 Đang tải bộ câu hỏi…</span>
    </div>
    <div style="margin-top:16px">
      <button class="btn-main" id="btnStart">▶️ BẮT ĐẦU CHƠI</button>
    </div>
    <div class="row center" style="margin-top:12px">
      <button class="btn-ghost" id="btnOpenEditor">⚙️ Đổi câu hỏi</button>
      <button class="btn-ghost" id="btnHowScore">🏆 Cách tính điểm</button>
    </div>
    <div id="scoreHelp" class="notice hidden">Đúng 1 câu: <b>+100 điểm</b>. Thưởng tốc độ: mỗi giây còn lại <b>+5 điểm</b>. Đúng 3 câu liên tiếp trở lên: cộng thêm <b>+20 điểm</b> mỗi câu. Hết giờ hoặc trả lời sai: không được điểm câu đó.</div>
  </section>

  <!-- ======= MÀN HÌNH CHƠI ======= -->
  <section id="screenGame" class="card hidden">
    <div class="hud">
      <div class="box"><small>CÂU HỎI</small><span id="hudQ">1/10</span></div>
      <div class="box"><small>ĐIỂM</small><span id="hudScore">0</span></div>
      <div class="box"><small>ĐÚNG LIÊN TIẾP</small><span id="hudStreak">🔥 0</span></div>
      <div class="box"><small>THỜI GIAN</small><span id="hudTime">20s</span></div>
    </div>
    <div class="timer-wrap"><div id="timerBar" style="width:100%"></div></div>
    <div class="tree-zone">
      <div id="tree">🌱</div>
      <div class="progress-text" id="treeText">Mầm xanh vừa nhú — hãy trả lời đúng để cây lớn lên!</div>
    </div>
    <div id="question">Câu hỏi…</div>
    <div class="answers" id="answers"></div>
    <div class="explain" id="explain"></div>
    <button class="btn-main hidden" id="nextBtn">Câu tiếp theo ➡️</button>
    <div class="row center" style="margin-top:14px">
      <button class="btn-ghost btn-small" id="btnQuit">⏹️ Dừng chơi</button>
    </div>
  </section>

  <!-- ======= MÀN HÌNH KẾT QUẢ ======= -->
  <section id="screenResult" class="card hidden">
    <div class="result">
      <div class="big" id="resultEmoji">🌳</div>
      <h2 id="resultTitle">Hoàn thành!</h2>
      <div class="score-big"><span id="finalScore">0</span> điểm</div>
      <div><span class="rank" id="finalRank">🌱 Hạt Mầm Xanh</span></div>
      <p id="finalDetail" style="line-height:1.7;color:var(--muted);font-weight:600"></p>
      <p id="finalMsg" style="line-height:1.7;margin-top:8px"></p>
      <div style="margin-top:16px" class="row center">
        <button class="btn-main" id="btnReplay" style="width:auto">🔄 Chơi lại (xáo trộn câu hỏi)</button>
      </div>
      <div class="row center" style="margin-top:10px">
        <button class="btn-ghost" id="btnHome">🏠 Về màn hình chính</button>
        <button class="btn-ghost" id="btnEditor2">⚙️ Đổi câu hỏi</button>
      </div>
    </div>
  </section>

  <!-- ======= MÀN HÌNH ĐỔI CÂU HỎI ======= -->
  <section id="screenEditor" class="card hidden">
    <div class="editor-head">
      <h2 style="color:var(--green-dark)">⚙️ Đổi câu hỏi</h2>
      <button class="btn-ghost btn-small" id="btnCloseEditor">✖️ Đóng</button>
    </div>
    <div class="notice">
      Sửa trực tiếp nội dung bên dưới rồi bấm <b>“Lưu bộ câu hỏi”</b>. Bộ câu hỏi của bạn được lưu ngay trên trình duyệt này (lần sau mở lại vẫn còn).
      Muốn dùng trên máy khác: bấm <b>Xuất file JSON</b>, rồi ở máy kia bấm <b>Nhập file JSON</b>.
      Ô tròn ● bên cạnh đáp án là đáp án đúng.
    </div>
    <div id="editorList"></div>
    <div class="row" style="margin-top:14px">
      <button class="btn-ghost" id="btnAddQ">➕ Thêm câu hỏi mới</button>
      <button class="btn-ghost" id="btnResetQ">↩️ Khôi phục câu hỏi gốc</button>
    </div>
    <div class="row" style="margin-top:10px">
      <button class="btn-main" id="btnSaveQ" style="width:auto;flex:1">💾 Lưu bộ câu hỏi</button>
    </div>
    <div class="row" style="margin-top:10px">
      <button class="btn-ghost btn-small" id="btnExportQ">⬇️ Xuất file JSON</button>
      <button class="btn-ghost btn-small" id="btnImportQ">⬆️ Nhập file JSON</button>
      <input type="file" id="fileImport" accept=".json,application/json" class="hidden">
    </div>
    <p id="editorMsg" style="margin-top:10px;font-weight:700;color:var(--green-dark)"></p>
  </section>

  <footer>
    🌍 Mỗi hành động nhỏ — phân loại rác, tiết kiệm điện nước, trồng một cây xanh — đều giúp Trái Đất khỏe hơn.<br>
    Game “Chiến Binh Xanh” • Chơi trực tiếp trên trình duyệt, không cần cài đặt.
  </footer>
</div>

<script>
"use strict";
/* =====================================================================
   BỘ CÂU HỎI GỐC — muốn đổi nhanh trên GitHub thì sửa ngay tại đây.
   Mỗi câu: q = câu hỏi, options = 4 đáp án, answer = vị trí đáp án đúng
   (đếm từ 0), explain = giải thích sau khi trả lời.
   Trong game cũng có nút "Đổi câu hỏi" để sửa mà không cần đụng vào code.
===================================================================== */
const DEFAULT_QUESTIONS = [
  { q:"Ngày Môi trường Thế giới được tổ chức vào ngày nào hằng năm?",
    options:["Ngày 5 tháng 6","Ngày 1 tháng 1","Ngày 30 tháng 4","Ngày 22 tháng 12"], answer:0,
    explain:"Ngày 5/6 là Ngày Môi trường Thế giới do Liên Hợp Quốc phát động, nhắc mọi người cùng hành động vì môi trường." },
  { q:"Một chiếc túi ni lông dùng vài phút nhưng mất khoảng bao lâu để phân hủy hoàn toàn trong tự nhiên?",
    options:["Khoảng 1 tuần","Khoảng 1 tháng","Khoảng 1 năm","Từ 500 đến 1.000 năm"], answer:3,
    explain:"Túi ni lông cực kỳ khó phân hủy, có thể tồn tại 500–1.000 năm. Hãy mang túi vải khi đi chợ nhé!" },
  { q:"Ba chữ R trong bảo vệ môi trường (3R) có nghĩa là gì?",
    options:["Read – Run – Rest (Đọc – Chạy – Nghỉ)","Reduce – Reuse – Recycle (Giảm dùng – Tái sử dụng – Tái chế)","Red – Round – River (Đỏ – Tròn – Sông)","Rain – Road – Rock (Mưa – Đường – Đá)"], answer:1,
    explain:"3R = Reduce (giảm dùng), Reuse (tái sử dụng), Recycle (tái chế) — nguyên tắc vàng để giảm rác thải." },
  { q:"Cây xanh giúp làm sạch không khí bằng cách nào?",
    options:["Hấp thụ khí CO₂ và nhả ra khí ôxy","Làm cho trời mưa nhiều hơn","Che hết ánh nắng mặt trời","Đuổi hết các loài chim"], answer:0,
    explain:"Khi quang hợp, cây hấp thụ CO₂ — khí gây biến đổi khí hậu — và nhả ra ôxy cho chúng ta hít thở." },
  { q:"Pin cũ, pin hỏng nên được xử lý như thế nào?",
    options:["Vứt chung vào thùng rác sinh hoạt","Chôn xuống vườn","Mang đến điểm thu gom rác thải nguy hại / hộp thu hồi pin","Ném xuống sông cho trôi đi"], answer:2,
    explain:"Pin chứa chì, thủy ngân, cadmi rất độc. Vứt bừa bãi sẽ ngấm vào đất và nước. Hãy mang pin cũ đến điểm thu gom riêng." },
  { q:"Hành động nào sau đây giúp tiết kiệm điện nhất?",
    options:["Bật điều hòa 16°C cả ngày","Tắt điện, rút phích cắm khi không sử dụng","Mở tủ lạnh liên tục cho mát phòng","Bật tất cả đèn trong nhà"], answer:1,
    explain:"Tắt và rút phích cắm khi không dùng giúp giảm điện lãng phí — nhiều thiết bị vẫn 'ăn điện ngầm' khi cắm chờ." },
  { q:"Rác hữu cơ gồm những loại nào?",
    options:["Vỏ trái cây, rau thừa, lá cây","Chai nhựa, lon nhôm","Pin, bóng đèn hỏng","Túi ni lông, xốp"], answer:0,
    explain:"Rác hữu cơ là rác từ thực phẩm và cây cối, phân hủy nhanh và có thể ủ thành phân bón cho cây." },
  { q:"Vì sao không nên đốt rác nhựa và xốp ngoài trời?",
    options:["Vì lửa không cháy được nhựa","Vì khói đốt nhựa sinh ra khí độc gây bệnh","Vì nhựa rất đắt tiền","Vì đốt rác làm trời tối đi"], answer:1,
    explain:"Đốt nhựa, xốp sinh ra khí độc như dioxin, gây hại cho phổi và sức khỏe. Rác nhựa nên được thu gom, tái chế." },
  { q:"Năng lượng nào sau đây là năng lượng tái tạo, sạch và không bao giờ cạn?",
    options:["Than đá","Dầu mỏ","Năng lượng mặt trời","Khí đốt tự nhiên"], answer:2,
    explain:"Năng lượng mặt trời, gió, nước là năng lượng tái tạo — sạch và vô tận, khác với than đá, dầu mỏ gây ô nhiễm." },
  { q:"Khi đánh răng, khóa vòi nước lại giúp tiết kiệm được khoảng bao nhiêu nước mỗi phút?",
    options:["Khoảng 0,1 lít","Khoảng 1 lít","Khoảng 6 lít","Không tiết kiệm được gì"], answer:2,
    explain:"Để vòi chảy liên tục khi đánh răng có thể lãng phí khoảng 6 lít nước mỗi phút. Khóa vòi là thói quen rất đáng quý!" },
  { q:"Rùa biển thường nhầm túi ni lông trôi trên biển với thứ gì và ăn phải?",
    options:["Con sứa — thức ăn của rùa biển","Con cá nhỏ","Rong biển","San hô"], answer:0,
    explain:"Túi ni lông trong nước trông giống con sứa. Rùa ăn phải sẽ bị tắc ruột và chết. Đừng xả rác ra biển nhé!" },
  { q:"Loại bóng đèn nào tiết kiệm điện nhất trong các loại sau?",
    options:["Bóng đèn sợi đốt","Bóng đèn huỳnh quang ống dài","Bóng đèn LED","Bóng đèn halogen"], answer:2,
    explain:"Đèn LED tiết kiệm tới 75–80% điện so với đèn sợi đốt và dùng được lâu hơn nhiều lần." },
  { q:"Rừng ngập mặn ở ven biển có vai trò quan trọng nào?",
    options:["Chắn sóng, chống xói lở và là nhà của nhiều loài tôm cá","Làm nước biển mặn hơn","Để lấy gỗ xây nhà là chính","Không có vai trò gì"], answer:0,
    explain:"Rừng ngập mặn như 'bức tường xanh' chắn sóng bão, giữ đất và là nơi sinh sống của nhiều loài thủy sản." },
  { q:"Khí nào là nguyên nhân chính gây hiệu ứng nhà kính và biến đổi khí hậu?",
    options:["Khí ôxy (O₂)","Khí CO₂ (cacbonic)","Khí nitơ (N₂)","Hơi nước sạch"], answer:1,
    explain:"CO₂ từ đốt xăng, than, dầu giữ nhiệt trong khí quyển làm Trái Đất nóng lên — đó là biến đổi khí hậu." },
  { q:"Việc làm nào giúp giảm rác thải nhựa dùng một lần hiệu quả?",
    options:["Dùng bình nước cá nhân và túi vải thay đồ nhựa dùng một lần","Mua thêm nhiều túi ni lông dự trữ","Dùng ống hút nhựa cho mọi ly nước","Vứt ly nhựa ngay sau một lần uống"], answer:0,
    explain:"Mang bình nước cá nhân, túi vải, hộp cơm riêng giúp cắt giảm rất nhiều nhựa dùng một lần mỗi ngày." },
  { q:"Giấy đã qua sử dụng nên làm gì để bảo vệ rừng?",
    options:["Đốt bỏ ngay","Thu gom để tái chế giấy","Vứt xuống cống","Chôn xuống đất"], answer:1,
    explain:"Tái chế 1 tấn giấy giúp cứu khoảng 17 cây xanh và tiết kiệm nhiều nước, điện so với làm giấy mới." },
  { q:"Khi in tài liệu, cách nào giúp tiết kiệm giấy?",
    options:["Chỉ in một mặt","In hai mặt và chỉ in khi thật cần thiết","In thật nhiều bản dự phòng","In màu cho mọi trang"], answer:1,
    explain:"In hai mặt giúp giảm một nửa lượng giấy. Trước khi in, hãy hỏi: 'Mình có thật sự cần bản giấy này không?'" },
  { q:"Đi lại bằng phương tiện nào thân thiện với môi trường nhất cho quãng đường ngắn?",
    options:["Đi xe máy một mình","Đi ô tô riêng","Đi xe đạp hoặc đi bộ","Đi taxi"], answer:2,
    explain:"Xe đạp và đi bộ không xả khói bụi, lại tốt cho sức khỏe. Quãng ngắn, hãy ưu tiên 'đôi chân xanh'!" },
  { q:"Nước thải sinh hoạt chưa qua xử lý đổ thẳng ra sông sẽ gây hậu quả gì?",
    options:["Làm sông sạch hơn","Ô nhiễm nguồn nước, cá tôm chết, người dân thiếu nước sạch","Làm cá lớn nhanh hơn","Không ảnh hưởng gì"], answer:1,
    explain:"Nước thải bẩn làm sông hồ ô nhiễm, sinh vật chết và con người mất nguồn nước sạch để sinh hoạt." },
  { q:"Một hành động đơn giản mà học sinh có thể làm mỗi ngày để bảo vệ môi trường là gì?",
    options:["Xả rác đúng nơi quy định và phân loại rác tại nguồn","Mang nhiều túi ni lông về nhà","Để nước chảy tràn khi rửa tay","Bẻ cành cây trong sân trường"], answer:0,
    explain:"Bỏ rác đúng chỗ, phân loại rác, tắt điện nước khi không dùng — việc nhỏ mỗi ngày tạo thay đổi lớn!" }
];

/* ================== LƯU / TẢI BỘ CÂU HỎI ================== */
const STORAGE_KEY = "chien-binh-xanh-questions-v1";
let questionBank = loadBank();
function loadBank(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(raw){
      const arr = JSON.parse(raw);
      if(Array.isArray(arr) && arr.length) return sanitize(arr);
    }
  }catch(e){}
  return JSON.parse(JSON.stringify(DEFAULT_QUESTIONS));
}
function sanitize(arr){
  return arr.filter(q => q && typeof q.q === "string" && Array.isArray(q.options) && q.options.length >= 2)
            .map(q => ({ q:q.q, options:q.options.slice(0,6).map(String),
                         answer: Math.min(Math.max(0, q.answer|0), q.options.length-1),
                         explain: typeof q.explain === "string" ? q.explain : "" }));
}
function saveBank(){
  try{ localStorage.setItem(STORAGE_KEY, JSON.stringify(questionBank)); }catch(e){}
}

/* ================== ÂM THANH NHẸ (WebAudio, không cần file) ================== */
let audioCtx = null;
function beep(freq, dur, type){
  try{
    audioCtx = audioCtx || new (window.AudioContext || window.webkitAudioContext)();
    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
    o.type = type || "sine"; o.frequency.value = freq;
    g.gain.setValueAtTime(0.12, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
    o.connect(g); g.connect(audioCtx.destination); o.start(); o.stop(audioCtx.currentTime + dur);
  }catch(e){}
}
const sndOk   = () => { beep(660, .12); setTimeout(()=>beep(880,.16), 110); };
const sndBad  = () => beep(180, .28, "sawtooth");
const sndWin  = () => { [523,659,784,1046].forEach((f,i)=>setTimeout(()=>beep(f,.18), i*130)); };

/* ================== TRẠNG THÁI GAME ================== */
const $ = id => document.getElementById(id);
const screens = { start:$("screenStart"), game:$("screenGame"), result:$("screenResult"), editor:$("screenEditor") };
function show(name){
  Object.values(screens).forEach(s => s.classList.add("hidden"));
  screens[name].classList.remove("hidden");
  window.scrollTo({top:0, behavior:"smooth"});
}
let game = null, timerId = null, timeLeft = 0;
const TIME_PER_Q = 20;

function shuffle(a){ a = a.slice(); for(let i=a.length-1;i>0;i--){ const j = Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } return a; }

function updateBankInfo(){ $("bankInfo").textContent = "📚 Bộ câu hỏi hiện có: " + questionBank.length + " câu"; }

function startGame(){
  const sel = $("selCount").value;
  let pool = shuffle(questionBank);
  if(sel !== "all") pool = pool.slice(0, Math.min(parseInt(sel,10), pool.length));
  game = { list: pool, idx: 0, score: 0, correct: 0, streak: 0, bestStreak: 0, answered:false };
  show("game");
  nextQuestion();
}

function treeStage(){
  if(!game || !game.list.length) return ["🌱","Mầm xanh vừa nhú — hãy trả lời đúng để cây lớn lên!"];
  const r = game.correct / game.list.length;
  if(r >= .85) return ["🌳","Cây xanh sum suê! Hành tinh đang mỉm cười với bạn!"];
  if(r >= .6)  return ["🌲","Cây đã cao lớn! Cố thêm chút nữa nào!"];
  if(r >= .35) return ["🌿","Cây đang vươn lá! Tốt lắm, tiếp tục nhé!"];
  if(r > 0)    return ["🌱","Mầm đã nảy! Trả lời đúng tiếp để cây lớn nhanh!"];
  return ["🌰","Hạt giống đang chờ — trả lời đúng để hạt nảy mầm!"];
}

function nextQuestion(){
  clearInterval(timerId);
  if(!game || game.idx >= game.list.length){ return finishGame(); }
  game.answered = false;
  const item = game.list[game.idx];
  $("hudQ").textContent = (game.idx+1) + "/" + game.list.length;
  $("hudScore").textContent = game.score;
  $("hudStreak").textContent = "🔥 " + game.streak;
  const [emoji, txt] = treeStage();
  $("tree").textContent = emoji; $("treeText").textContent = txt;
  $("question").textContent = "Câu " + (game.idx+1) + ": " + item.q;
  $("explain").className = "explain"; $("explain").textContent = "";
  $("nextBtn").classList.add("hidden");

  // Xáo trộn thứ tự đáp án, nhớ vị trí đáp án đúng sau khi xáo
  const order = shuffle(item.options.map((_,i)=>i));
  const box = $("answers"); box.innerHTML = "";
  const keys = ["A","B","C","D","E","F"];
  order.forEach((optIdx, pos) => {
    const b = document.createElement("button");
    b.className = "ans";
    b.innerHTML = '<span class="key"></span><span></span>';
    b.querySelector(".key").textContent = keys[pos];
    b.querySelector("span:last-child").textContent = item.options[optIdx];
    b.onclick = () => answer(optIdx === item.answer, b);
    box.appendChild(b);
  });

  // Đếm ngược
  timeLeft = TIME_PER_Q;
  renderTimer();
  timerId = setInterval(() => {
    timeLeft -= 0.2;
    if(timeLeft <= 0){ timeLeft = 0; renderTimer(); clearInterval(timerId); timeOut(); }
    else renderTimer();
  }, 200);
}
function renderTimer(){
  $("hudTime").textContent = Math.ceil(timeLeft) + "s";
  const bar = $("timerBar");
  bar.style.width = (timeLeft / TIME_PER_Q * 100) + "%";
  bar.classList.toggle("low", timeLeft <= 5);
}
function lockAnswers(correctBtn){
  document.querySelectorAll(".ans").forEach(b => {
    b.disabled = true;
    if(b === correctBtn) b.classList.add("correct");
  });
  // Tô xanh đáp án đúng dù người chơi chọn sai
  if(!correctBtn){
    const item = game.list[game.idx];
    document.querySelectorAll(".ans").forEach(b => {
      if(b.querySelector("span:last-child").textContent === item.options[item.answer]) b.classList.add("correct");
    });
  }
}
function showExplain(ok, msg){
  const ex = $("explain");
  ex.className = "explain " + (ok ? "ok" : "bad");
  ex.textContent = msg;
  const nb = $("nextBtn");
  nb.textContent = (game.idx + 1 >= game.list.length) ? "Xem kết quả 🏁" : "Câu tiếp theo ➡️";
  nb.classList.remove("hidden");
}
function answer(isCorrect, btn){
  if(game.answered) return;
  game.answered = true;
  clearInterval(timerId);
  const item = game.list[game.idx];
  if(isCorrect){
    game.correct++; game.streak++; game.bestStreak = Math.max(game.bestStreak, game.streak);
    let pts = 100 + Math.ceil(timeLeft) * 5 + (game.streak >= 3 ? 20 : 0);
    game.score += pts;
    sndOk();
    lockAnswers(btn);
    showExplain(true, "✅ Chính xác! +" + pts + " điểm. " + (item.explain || ""));
    $("tree").classList.add("grow"); setTimeout(()=>$("tree").classList.remove("grow"), 350);
  } else {
    game.streak = 0; sndBad();
    btn.classList.add("wrong");
    lockAnswers(null);
    showExplain(false, "❌ Chưa đúng rồi. Đáp án đúng là: “" + item.options[item.answer] + "”. " + (item.explain || ""));
  }
  $("hudScore").textContent = game.score;
  $("hudStreak").textContent = "🔥 " + game.streak;
  const [emoji, txt] = treeStage();
  $("tree").textContent = emoji; $("treeText").textContent = txt;
}
function timeOut(){
  if(game.answered) return;
  game.answered = true; game.streak = 0; sndBad();
  const item = game.list[game.idx];
  lockAnswers(null);
  showExplain(false, "⏰ Hết giờ! Đáp án đúng là: “" + item.options[item.answer] + "”. " + (item.explain || ""));
  $("hudStreak").textContent = "🔥 0";
}
function finishGame(){
  clearInterval(timerId);
  const total = game.list.length, c = game.correct, r = total ? c/total : 0;
  let emoji, rank, msg;
  if(r >= .9){ emoji="🏆"; rank="🌍 Đại Sứ Hành Tinh Xanh"; msg="Xuất sắc! Bạn là tấm gương bảo vệ môi trường. Hãy lan tỏa thói quen xanh cho mọi người quanh bạn nhé!"; }
  else if(r >= .7){ emoji="🌳"; rank="🌳 Chiến Binh Xanh"; msg="Rất tốt! Cây của bạn đã sum suê. Chỉ cần để ý thêm vài thói quen nhỏ nữa là hoàn hảo."; }
  else if(r >= .45){ emoji="🌿"; rank="🌿 Mầm Xanh Đang Lớn"; msg="Khởi đầu tốt! Ôn lại phần giải thích ở các câu sai rồi chơi lại, cây sẽ cao hơn nữa."; }
  else { emoji="🌱"; rank="🌱 Hạt Mầm Xanh"; msg="Đừng nản! Mỗi chuyên gia môi trường đều bắt đầu từ một hạt mầm. Chơi lại một lượt nữa nhé!"; }
  $("resultEmoji").textContent = emoji;
  $("resultTitle").textContent = "Hoàn thành hành trình!";
  $("finalScore").textContent = game.score;
  $("finalRank").textContent = rank;
  $("finalDetail").textContent = "Bạn trả lời đúng " + c + "/" + total + " câu • Chuỗi đúng dài nhất: " + game.bestStreak + " câu";
  $("finalMsg").textContent = msg;
  sndWin();
  show("result");
}

/* ================== TRÌNH SỬA CÂU HỎI ================== */
function renderEditor(){
  const list = $("editorList"); list.innerHTML = "";
  questionBank.forEach((q, i) => {
    const div = document.createElement("div");
    div.className = "qedit";
    div.innerHTML =
      '<div class="row" style="justify-content:space-between"><b>Câu ' + (i+1) + '</b>' +
      '<button type="button" class="btn-danger btn-small" data-del="' + i + '">🗑️ Xóa câu này</button></div>' +
      '<label>Nội dung câu hỏi</label><textarea rows="2" data-f="q" data-i="' + i + '"></textarea>' +
      '<label>Các đáp án (chọn ô tròn = đáp án đúng)</label><div class="opts"></div>' +
      '<label>Giải thích (hiện sau khi trả lời)</label><input type="text" data-f="explain" data-i="' + i + '">';
    div.querySelector('[data-f="q"]').value = q.q;
    div.querySelector('[data-f="explain"]').value = q.explain || "";
    const opts = div.querySelector(".opts");
    q.options.forEach((op, j) => {
      const row = document.createElement("div");
      row.className = "opt-row";
      row.innerHTML = '<input type="radio" name="correct-' + i + '" ' + (q.answer === j ? "checked" : "") + ' data-f="answer" data-i="' + i + '" data-j="' + j + '">' +
                      '<input type="text" data-f="opt" data-i="' + i + '" data-j="' + j + '">';
      row.querySelector('[data-f="opt"]').value = op;
      opts.appendChild(row);
    });
    div.querySelector("[data-del]").onclick = () => {
      if(questionBank.length <= 1){ $("editorMsg").textContent = "⚠️ Phải giữ lại ít nhất 1 câu hỏi."; return; }
      collectEditor();
      questionBank.splice(i, 1);
      renderEditor();
      $("editorMsg").textContent = "Đã xóa câu " + (i+1) + ". Nhớ bấm Lưu nhé!";
    };
    list.appendChild(div);
  });
}
function collectEditor(){
  document.querySelectorAll("#editorList [data-f]").forEach(el => {
    const i = +el.dataset.i, f = el.dataset.f;
    if(!questionBank[i]) return;
    if(f === "q") questionBank[i].q = el.value;
    else if(f === "explain") questionBank[i].explain = el.value;
    else if(f === "opt") questionBank[i].options[+el.dataset.j] = el.value;
    else if(f === "answer" && el.checked) questionBank[i].answer = +el.dataset.j;
  });
}
function openEditor(){ renderEditor(); $("editorMsg").textContent = ""; show("editor"); }

/* ================== SỰ KIỆN ================== */
$("btnStart").onclick = startGame;
$("btnReplay").onclick = startGame;
$("nextBtn").onclick = () => { game.idx++; nextQuestion(); };
$("btnQuit").onclick = () => { clearInterval(timerId); show("start"); };
$("btnHome").onclick = () => show("start");
$("btnHowScore").onclick = () => $("scoreHelp").classList.toggle("hidden");
$("btnOpenEditor").onclick = openEditor;
$("btnEditor2").onclick = openEditor;
$("btnCloseEditor").onclick = () => { updateBankInfo(); show("start"); };
$("btnAddQ").onclick = () => {
  collectEditor();
  questionBank.push({ q:"Câu hỏi mới về môi trường?", options:["Đáp án A","Đáp án B","Đáp án C","Đáp án D"], answer:0, explain:"" });
  renderEditor();
  window.scrollTo({ top: document.body.scrollHeight, behavior:"smooth" });
};
$("btnSaveQ").onclick = () => {
  collectEditor();
  const cleaned = sanitize(questionBank);
  if(!cleaned.length){ $("editorMsg").textContent = "⚠️ Chưa có câu hỏi hợp lệ để lưu."; return; }
  questionBank = cleaned; saveBank(); updateBankInfo();
  $("editorMsg").textContent = "✅ Đã lưu " + questionBank.length + " câu hỏi. Bấm Đóng rồi Bắt đầu chơi để dùng bộ mới!";
};
$("btnResetQ").onclick = () => {
  if(!confirm("Khôi phục về 20 câu hỏi gốc? Bộ câu hỏi bạn đã sửa sẽ bị thay thế.")) return;
  questionBank = JSON.parse(JSON.stringify(DEFAULT_QUESTIONS));
  saveBank(); renderEditor(); updateBankInfo();
  $("editorMsg").textContent = "↩️ Đã khôi phục bộ câu hỏi gốc.";
};
$("btnExportQ").onclick = () => {
  collectEditor();
  const blob = new Blob([JSON.stringify(questionBank, null, 2)], { type:"application/json" });
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob); a.download = "bo-cau-hoi-bao-ve-moi-truong.json"; a.click();
  URL.revokeObjectURL(a.href);
};
$("btnImportQ").onclick = () => $("fileImport").click();
$("fileImport").onchange = e => {
  const f = e.target.files[0]; if(!f) return;
  const rd = new FileReader();
  rd.onload = () => {
    try{
      const arr = sanitize(JSON.parse(rd.result));
      if(!arr.length) throw new Error("empty");
      questionBank = arr; saveBank(); renderEditor(); updateBankInfo();
      $("editorMsg").textContent = "✅ Đã nhập " + arr.length + " câu hỏi từ file.";
    }catch(err){ $("editorMsg").textContent = "⚠️ File không hợp lệ. File cần là JSON danh sách câu hỏi đã xuất từ game này."; }
  };
  rd.readAsText(f);
  e.target.value = "";
};

updateBankInfo();
show("start");
</script>
</body>
</html>
```


---

*Hết file tổng hợp. File này chứa toàn bộ thông tin của dự án “Chiến Binh Xanh — Bảo Vệ Môi Trường” tính đến ngày 20/11/2026: ý tưởng, luật chơi, 20 câu hỏi gốc (bản đọc + bản JSON), kiến trúc kỹ thuật, hướng dẫn deploy, gợi ý phát triển tiếp, mẫu prompt cho AI và toàn bộ mã nguồn bản gốc.*
