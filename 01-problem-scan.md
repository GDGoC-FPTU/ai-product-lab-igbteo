# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)
### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
| 1 | Xanh SM| Tốn thời gian | Nhân viên CSKH phải đọc và phân loại hàng nghìn phản ánh của tài xế mỗi ngày (mất khoảng 4–6 phút/ticket), gây chậm xử lý và chuyển sai bộ phận. |
| 2 | VinFast | Lặp lại| Nhân viên CSKH liên tục trả lời các câu hỏi lặp lại về pin, trạm sạc, bảo hành, cập nhật phần mềm và hướng dẫn sử dụng xe điện. |
| 3 | Vinhomes | Pain từ người khác | Cư dân gửi phản ánh về điện, nước, thang máy, vệ sinh qua nhiều kênh. Việc phân loại và chuyển cho bộ phận xử lý còn thủ công nên thời gian phản hồi chậm. |
| 4 | Vinmec | AI có thể tốt hơn | Chatbot hỗ trợ đặt lịch khám chỉ trả lời theo kịch bản cố định, chưa hiểu được các câu hỏi tự nhiên hoặc nhiều ý cùng lúc. |
| 5 | Vinpearl | Tốn thời gian | Nhân viên tư vấn phải trả lời lặp lại các câu hỏi về đặt phòng, giá vé, hoàn hủy và ưu đãi, làm tăng thời gian chờ của khách hàng. |
# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: AI hỗ trợ trả lời các câu hỏi về pin, sạc và      │
│ bảo hành xe điện VinFast bằng cách soạn bản nháp phản hồi.  │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Nhân viên Chăm sóc khách hàng (CSKH),          │
│ Khách hàng phải chờ phản hồi.                               │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách hàng gửi câu hỏi qua App/Website/Tổng đài        │
│   → 2. Nhân viên CSKH đọc và xác định nội dung              │
│   → 3. Tra cứu tài liệu kỹ thuật, chính sách bảo hành       │
│   → 4. Soạn câu trả lời phù hợp                             │
│   → 5. Gửi phản hồi cho khách hàng                          │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 6–8 phút/lượt)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (Hiểu câu hỏi -> Tra cứu FAQ -> Soạn bản nháp trả lời)      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian phản hồi từ 7 phút ──> dưới 1 phút;          │
│ ≥90% câu trả lời được nhân viên sử dụng sau khi chỉnh sửa   │
│ rất ít.                                                     │
│                                                             │
│ Quick Architecture: [x] LLM Feature (AI soạn bản nháp trả   │
│ lời, nhân viên kiểm duyệt trước khi gửi khách hàng).        │
└─────────────────────────────────────────────────────────────┘
