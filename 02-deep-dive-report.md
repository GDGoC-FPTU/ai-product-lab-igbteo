Họ và tên: Lý Thành Đạt
Thành viên nhóm: Ngô Hữu Nghĩa, Nguyễn Hữu Nhật Minh, Vũ Minh Quang, Bùi Văn Khởi, Phan Trọng Tiến
# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận yêu cầu │     │ Xác minh vị  │     │ Tra cứu trạm │     │ Soạn hướng   │
│ hỗ trợ từ    │ ──→ │ trí GPS và   │ ──→ │ sạc VinFast  │ ──→ │ dẫn gửi tài  │
│ tài xế       │     │ mức pin xe   │     │ khả dụng     │     │ xế           │
│              │     │              │     │              │     │              │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Cuộc gọi │     │ In: Biển số, │     │ In: Vị trí   │     │ In: Thông tin│
│ hoặc App     │     │ GPS, % pin   │     │ GPS          │     │ trạm sạc     │
│ Out: Log sự  │     │ Out: Thông   │     │ Out: Trạm    │     │ Out: Tin nhắn│
│ cố           │     │ tin xe       │     │ phù hợp      │     │ hướng dẫn    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Điều phối    │
                                                               │ xe cứu hộ    │
                                                               │ (nếu cần)    │
                                                               │              │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 1 phút     │
                                                               │ In: Kết quả  │
                                                               │ đánh giá     │
                                                               │ Out: Lệnh    │
                                                               │ cứu hộ       │
                                                               └──────────────┘
🔴 = Bottlenecks
⏱ Tổng thời gian xử lý thủ công: 15 phút/lượt.
## 3.2. Problem Statement (6-field) — Vin Smart Future Standard
| Field | Nội dung |
|---|---|
| **1. Actor / Operator** |Nhân viên điều phối của Xanh SM chịu trách nhiệm tiếp nhận các yêu cầu hỗ trợ từ tài xế, đánh giá tình trạng xe và lựa chọn phương án xử lý phù hợp trong từng trường hợp. |
| **2. Current Workflow** | Khi nhận được thông báo xe gặp sự cố về pin, điều phối viên kiểm tra thông tin xe và vị trí hiện tại, tìm kiếm trạm sạc còn hoạt động trên hệ thống quản lý, đánh giá khoảng cách có thể di chuyển, chuẩn bị nội dung hướng dẫn rồi gửi cho tài xế. Nếu xe không còn khả năng tiếp tục vận hành, điều phối viên sẽ liên hệ đội cứu hộ để hỗ trợ. Toàn bộ quy trình được thực hiện thủ công và mất khoảng 15 phút cho mỗi yêu cầu. |
| **3. Bottleneck** |Khâu xác định trạm sạc phù hợp và xây dựng phương án hướng dẫn là phần mất nhiều thời gian nhất vì phải tổng hợp dữ liệu từ nhiều nguồn khác nhau. Ngoài việc kiểm tra vị trí, điều phối viên còn phải cân nhắc mức pin, khả năng di chuyển và tình trạng của trạm sạc trước khi phản hồi cho tài xế. |
| **4. Business Impact** | Quy trình thủ công làm tăng thời gian phản hồi khi số lượng yêu cầu hỗ trợ tăng cao. Điều này khiến tài xế phải chờ lâu, ảnh hưởng đến khả năng nhận cuốc xe tiếp theo, đồng thời làm giảm năng suất làm việc của đội điều phối và chất lượng dịch vụ của Xanh SM. |
| **5. Success Metric** | Rút ngắn thời gian xử lý từ 15 phút xuống còn dưới 3 phút. AI tạo phương án xử lý trong dưới 10 giây. Ít nhất 95% đề xuất của AI được điều phối viên sử dụng sau khi kiểm tra. |
| **6. Operational Boundary** |AI chỉ đóng vai trò hỗ trợ phân tích thông tin, đề xuất phương án và tạo bản nháp hướng dẫn. AI không được phép tự gửi thông báo cho tài xế, không tự điều động xe cứu hộ, không tự thay đổi dữ liệu chuyến xe và không đưa ra quyết định thay con người. Mọi kết quả do AI tạo ra đều phải được điều phối viên xem xét và phê duyệt trước khi áp dụng. |