# 🌍 Chiến Binh Xanh — Trò Chơi Bảo Vệ Môi Trường

Trò chơi trắc nghiệm về bảo vệ môi trường, chạy trực tiếp trên trình duyệt (không cần cài đặt).

**▶️ Chơi ngay:** https://hoanghai0810.github.io/bao-ve-moi-truong-game/

## Cách chơi

- Bấm **Bắt đầu chơi**, chọn số câu mỗi lượt (10 / 15 / tất cả).
- Mỗi câu có **20 giây** để trả lời. Đúng: **+100 điểm**, cộng thêm **+5 điểm cho mỗi giây còn lại**, đúng từ 3 câu liên tiếp được thêm **+20 điểm/câu**.
- Trả lời đúng, cây xanh lớn lên 🌱 → 🌿 → 🌲 → 🌳. Cuối lượt có xếp hạng: Hạt Mầm Xanh, Mầm Xanh Đang Lớn, Chiến Binh Xanh, Đại Sứ Hành Tinh Xanh.
- Sau mỗi câu đều có **giải thích** để học thêm kiến thức môi trường.

## Đổi câu hỏi (không cần biết code)

1. Ở màn hình chính, bấm **⚙️ Đổi câu hỏi**.
2. Sửa nội dung câu hỏi, các đáp án, chọn ô tròn ở đáp án đúng, viết giải thích.
3. Có thể **Thêm câu hỏi mới** hoặc **Xóa** câu không muốn dùng.
4. Bấm **💾 Lưu bộ câu hỏi** — bộ câu hỏi được lưu trên trình duyệt, lần sau mở lại vẫn còn.
5. **Xuất file JSON** để sao lưu / gửi cho người khác, **Nhập file JSON** để dùng lại ở máy khác.
6. Muốn quay về bộ gốc: bấm **↩️ Khôi phục câu hỏi gốc** (20 câu mặc định).

## Đổi câu hỏi trong code (cho người quản trị trang)

Mở file `index.html`, tìm `DEFAULT_QUESTIONS` ở đầu phần `<script>`. Mỗi câu có dạng:

```js
{ q: "Câu hỏi?",
  options: ["Đáp án 1", "Đáp án 2", "Đáp án 3", "Đáp án 4"],
  answer: 0,              // vị trí đáp án đúng, đếm từ 0
  explain: "Giải thích." }
```

Sửa xong, commit lên GitHub — GitHub Pages sẽ tự cập nhật trang sau khoảng 1 phút.

## Kỹ thuật

- Một file `index.html` duy nhất (HTML + CSS + JavaScript thuần), không cần build.
- Bộ câu hỏi tùy chỉnh được lưu bằng `localStorage` của trình duyệt.
- Chạy tốt trên điện thoại và máy tính.
