# 03 — AI Log & Reflection (Phase 6, Cá nhân)

> Lab 02: AI Product Scoping — Vin Smart Future
> Bài toán liên quan: **Xanh SM — Xử lý sự cố sạc pin thực địa**

---

## Cách tôi dùng AI làm thought-partner trong buổi lab

Tôi dùng AI để brainstorm nhanh danh sách bài toán ở Phase 1 (Scan) khi
chưa có đủ ý tưởng cụ thể, đặc biệt để đối chiếu các pain point tôi tự
quan sát được với gợi ý theo từng mảng kinh doanh của Vingroup (Xanh SM,
VinFast, Vinhomes, Vinmec). Tôi cũng dùng AI để phản biện các Quick
Problem Card theo hướng "đóng vai CFO/Trưởng phòng Vận hành khắt khe" nhằm
kiểm tra xem bài toán có thực sự cần AI, hay rule-based code đơn giản đã
đủ giải quyết — nhờ vậy nhóm loại được 2 bài toán (Vinhomes CSKH, Vinmec
Discharge Summary) khỏi phạm vi Deep-Dive vì rủi ro pháp lý/lâm sàng cao
hơn giá trị chứng minh được trong 1 buổi lab.

Ở Phase 4, tôi dùng AI hỗ trợ viết code Python gọi Gemini 2.5 SDK và viết
`SYSTEM_PROMPT` cho bài toán Xanh SM (điều xe sạc pin di động khi pin dưới
5%, bắt buộc thẻ `[DRAFT_ONLY]`), sau đó tự chạy stress-test với 2 câu
input "tấn công" để kiểm tra ranh giới có bị phá vỡ không.

## AI giúp gì

- Tăng tốc brainstorm ban đầu và giúp cấu trúc Problem Card theo đúng
  format 6-field nhất quán, không bỏ sót Metric hay Operational Boundary.
- Hỗ trợ viết code mẫu (`evaluate_prompt`, cấu hình SDK `google-genai`)
  nhanh hơn nhiều so với tự tra docs từ đầu.
- **Quan trọng nhất:** hỗ trợ debug từng bước khi script liên tục báo lỗi
  API — tôi gặp lần lượt 3 lớp lỗi khác nhau và AI giúp tôi tách bạch từng
  lớp thay vì đoán mò:
  1. `API key not valid` — do biến môi trường `$env:GEMINI_API_KEY` đang
     giữ đúng chuỗi placeholder mẫu (`AIzaSyYourGeminiApiKeyHere`) từ lúc
     copy hướng dẫn, chưa thay bằng key thật.
  2. `PERMISSION_DENIED (403)` — do project Google Cloud gắn với key bị
     từ chối quyền truy cập Generative Language API.
  3. Script tự thoát với `exit code 1` khi chạy qua autograder — do
     `load_dotenv()` không tìm thấy `.env` vì autograder chạy script từ
     một thư mục làm việc (cwd) khác với vị trí thật của file `.env`.

## AI sai ở đâu / cần tôi tự kiểm tra lại

- AI không thể tự phát hiện tôi đã gõ nhầm tên model thành
  `"gemini-3.5-flash"` (model không tồn tại) thay vì `"gemini-2.5-flash"`
  — đây là lỗi tôi tự gây ra khi gõ tay, và chỉ phát hiện được khi tự đọc
  lại từng dòng code thay vì chỉ chạy và nhìn lỗi chung chung.
- Ở phần Metric của Quick Card ban đầu, con số AI gợi ý khá mơ hồ (kiểu
  "giảm đáng kể thời gian"), tôi phải tự sửa lại thành số cụ thể (từ 15
  phút xuống dưới 3 phút, đạt 98% đúng địa điểm) để đúng tiêu chí chấm G2.
- Khi debug lỗi 403 PERMISSION_DENIED, gợi ý ban đầu tập trung vào việc
  key sai, nhưng tôi phải tự kiểm tra thêm khả năng project bị giới hạn
  quyền do dùng email tổ chức (trường học) thay vì Gmail cá nhân — đây là
  nguyên nhân thực tế sau khi loại trừ dần từng khả năng.

## Bài học rút ra

AI là công cụ tăng tốc brainstorm và viết code rất tốt, nhưng phần quan
trọng nhất — đặt đúng ranh giới an toàn (Operational Boundary), xác định
Metric có số cụ thể, và **tự kiểm tra kỹ từng chi tiết cấu hình** (tên
model, đường dẫn `.env`, quyền truy cập API) — vẫn cần sự cẩn thận và tư
duy phản biện của con người. Quá trình debug nhiều lớp lỗi liên tiếp cũng
cho tôi thấy rõ giá trị của việc kiểm tra từng giả thuyết một cách có hệ
thống (loại trừ dần: key sai → quyền project sai → cấu hình đường dẫn sai
→ lỗi gõ tay) thay vì thử ngẫu nhiên nhiều thứ cùng lúc.