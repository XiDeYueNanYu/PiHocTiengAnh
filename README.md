# Pi Học Tiếng Anh

Ứng dụng học tiếng Anh cho trẻ em, chạy tĩnh (thuần HTML/CSS/JS), phù hợp deploy trên GitHub Pages.

## Cấu trúc thư mục

```
/index.html          → toàn bộ app (giao diện + logic 5 dạng luyện tập)
/data/lessons.json   → dữ liệu bài học (từ vựng, câu, tên file media) — cập nhật tự động qua GitHub Action / Google Sheet
/media/               → TẤT CẢ file audio + ảnh/gif, để phẳng chung 1 thư mục (đúng tên đã khai trong lessons.json)
```

## Schema của `data/lessons.json`

```json
{
  "lessons": [
    {
      "id": "lesson1",
      "title": "Tên bài hiện trong ô chọn bài học",
      "teacherPools": {
        "type1": ["teacher_01.png", "..."],   // ảnh giáo viên dùng cho Dạng 1
        "type4": ["teacher_04.png", "..."],   // ảnh giáo viên dùng cho Dạng 4
        "type5": ["teacher_06.png", "..."]    // ảnh giáo viên dùng cho Dạng 5
      },
      "words": [
        {
          "en": "ant",
          "vi": "con kiến",
          "ipa": "/ænt/",
          "gif": "ant.gif",
          "audioWordVi": "con-kien.mp3",
          "audioWordEn": "ant.mp3",
          "sentenceEn": "This is an ant.",
          "sentenceVi": "Đây là con kiến.",
          "audioSentenceVi": "vn_ant.mp3",
          "audioSentenceEn": "el_ant.mp3",
          "questionEn": "What is this?",
          "questionVi": "Đây là gì?",
          "audioQuestion": "question.mp3",
          "answerEn": "This is an ant.",
          "audioAnswer": "el_ant.mp3"
        }
      ]
    }
  ]
}
```

Mỗi `word` là 1 đơn vị dùng chung cho cả 5 dạng luyện tập (từ vựng, câu, hỏi-đáp) — không cần tách mảng riêng, vì các dạng chỉ khác nhau ở cách trình bày/audio nào được phát.

Mỗi bài học cần **tối thiểu 3 từ** để Dạng 2 và Dạng 3 (chọn ảnh) hoạt động, vì cần 2 ảnh nhiễu lấy trong cùng bài.

## File media cần chuẩn bị (theo bộ dữ liệu mẫu)

Đặt tất cả trực tiếp trong thư mục `/media/`:

**Ảnh giáo viên:** `teacher_01.png` → `teacher_07.png` (bạn có thể thêm bao nhiêu tuỳ ý, chỉ cần khai trong JSON)

**Ảnh gif từ vựng:** `ant.gif`, `ax.gif`, `apple.gif`, `alligator.gif`

**Audio từ vựng tiếng Việt:** `con-kien.mp3`, `cai-riu.mp3`, `qua-tao.mp3`, `con-ca-sau.mp3`

**Audio từ vựng tiếng Anh:** `ant.mp3`, `ax.mp3`, `apple.mp3`, `alligator.mp3`

**Audio câu (Dạng 4 & 5):**
`vn_ant.mp3`, `el_ant.mp3`, `vn_ax.mp3`, `el_ax.mp3`, `vn_apple.mp3`, `el_apple.mp3`, `vn_alligator.mp3`, `el_alligator.mp3`, `question.mp3`

**Hiệu ứng đúng/sai (Dạng 2 & 3):** `Dung.mp3`, `Sai.mp3`

## Ghi chú kỹ thuật

- **Mở khoá âm thanh:** trình duyệt di động (đặc biệt Safari/iOS) chặn tự động phát audio nếu chưa có thao tác chạm. App có màn hình "Bắt Đầu" ở đầu để xử lý việc này — sau khi bấm 1 lần, mọi audio trong phiên học sẽ tự phát bình thường.
- **Vòng lặp:** ở từ/câu cuối cùng, bấm "Tiếp Theo" sẽ quay lại từ/câu đầu tiên (và ngược lại với "Trước").
- **Đổi bài học / đổi dạng luyện tập:** luôn quay về từ đầu tiên. Khi đổi bài học, app tự chuyển về Dạng 1.
- **Chạy thử cục bộ:** vì app dùng `fetch()` để tải `data/lessons.json`, bạn cần chạy qua 1 local server (không mở trực tiếp bằng `file://`), ví dụ: `python3 -m http.server` rồi mở `http://localhost:8000`. Khi deploy lên GitHub Pages thì không cần lo việc này.
- **Cập nhật dữ liệu tự động:** vì toàn bộ nội dung nằm trong 1 file `data/lessons.json`, GitHub Action lấy dữ liệu từ Google Sheet chỉ cần build lại đúng file này theo schema ở trên.
