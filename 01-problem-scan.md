# 01 — Problem Scan (Phase 1 & 2)

> Lab 02: AI Product Scoping — Vin Smart Future
> Vai trò: AI Product Engineer tại Vin Smart Future

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

Dùng **4 Lenses** quét hoạt động vận hành các công ty thành viên Vingroup. Ghi tối thiểu 5 bài toán thực tế.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công sự cố hết pin / sạc thực địa: tra GPS, tìm trụ trống, soạn tin chỉ dẫn (mất ~15 phút/lượt). |
| 2 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn điểm đón gợi ý sai / xa thực tế, dẫn đến khách hủy và tài xế mất cuốc vào giờ cao điểm. |
| 3 | **VinFast** | Lặp lại | So khớp hóa đơn sạc điện từ mạng trạm đối tác với dữ liệu tài chính nội bộ hằng tuần (hàng nghìn giao dịch). |
| 4 | **Vinhomes** | AI-upgrade | Phân loại & điều hướng khiếu nại cư dân trên App Vinhomes Resident còn chậm, phản hồi rập khuôn (SLA ~12 giờ). |
| 5 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20–30 phút/bệnh nhân để viết tóm tắt hồ sơ xuất viện từ EMR + xét nghiệm + ghi chú lâm sàng. |
| 6 | **Vinpearl** | Pain từ người khác | Manager phải quét thủ công review Booking/Agoda/Google để phát hiện khiếu nại khẩn (phòng bẩn, thái độ nhân viên). |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 từ SCAN: **#1 (Xanh SM sự cố pin)**, **#4 (Vinhomes CSKH)**, **#5 (Vinmec Discharge Summary)**.

## Quick Problem Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo hết pin / sự cố sạc giữa đường │
│ cần điều phối tìm trạm trống hoặc cứu hộ pin di động.       │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Điều phối viên (quá tải); Tài xế (chờ cứu hộ)  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Nhận cuộc gọi sự cố                                    │
│   → 2. Tra cứu GPS xe trên bản đồ nội bộ                    │
│   → 3. Tra cứu trạm sạc VinFast còn trụ trống               │
│   → 4. Soạn tin chỉ dẫn gửi App tài xế                      │
│   → 5. Gọi xe cứu hộ pin nếu pin cực thấp                   │
│                                                             │
│ Bước nào tốn nhất? Bước 3–4 (⏱ ~10 phút/lượt)               │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–4              │
│ (Auto-pull vị trí + trạm trống → draft tin / đề xuất cứu hộ)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian xử lý từ 15 phút ──> dưới 3 phút.            │
│ Tỉ lệ hướng dẫn đúng loại trụ / đúng địa điểm ≥ 98%.        │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

## Quick Problem Card #2 — Vinhomes: Phân loại & route khiếu nại cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Khiếu nại trên App Vinhomes Resident bị phân loại │
│ thủ công / phản hồi chậm, cư dân chờ lâu.                   │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? CSKH Ban Quản lý; Cư dân chờ phản hồi          │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi ticket trên App                             │
│   → 2. CSKH đọc & tự gắn loại (nước/điện/ồn/an ninh)        │
│   → 3. Forward thủ công sang đúng tổ kỹ thuật               │
│   → 4. Soạn phản hồi cập nhật trạng thái cho cư dân         │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ ~8–15 phút/ticket + hàng đợi)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–4              │
│ (Classify intent → suggest assignee → draft reply)          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ 85% ticket được phân loại đúng dưới 10 giây;                │
│ giảm median first-response từ 12 giờ ──> dưới 2 giờ.        │
│                                                             │
│ Quick Architecture: [x] LLM Feature (+ Rule router ưu tiên) │
└─────────────────────────────────────────────────────────────┘
```

## Quick Problem Card #3 — Vinmec: Tóm tắt hồ sơ xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Bác sĩ mất quá nhiều thời gian viết Discharge     │
│ Summary dễ hiểu cho bệnh nhân từ EMR + lab + ghi chú.       │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ lâm sàng; Bệnh nhân chờ giấy tờ xuất viện│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Mở EMR, đọc diễn biến + thuốc + xét nghiệm             │
│   → 2. Tóm tắt chẩn đoán & hướng dẫn theo dõi               │
│   → 3. Viết lại bằng ngôn ngữ dễ hiểu cho bệnh nhân         │
│   → 4. In / ký / bàn giao cho điều dưỡng                    │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ 20–30 phút/bệnh nhân)        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3              │
│ (Draft summary từ EMR; bác sĩ HITL duyệt trước ký)          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian soạn từ 25 phút ──> dưới 8 phút (kể cả duyệt)│
│ ≥ 95% bản nháp được bác sĩ chấp nhận sau ≤ 1 lần chỉnh.     │
│                                                             │
│ Quick Architecture: [x] LLM Feature (bắt buộc HITL)         │
└─────────────────────────────────────────────────────────────┘
```

---

## Ghi chú sau Stress-Test thẻ bài toán (CFO / Ops lens)

- **Card #2 (Vinhomes):** Rủi ro pháp lý/phí quản lý nếu route sai → cần Rule-based priority + LLM classify, chưa nên Agent tự trị.
- **Card #3 (Vinmec):** Domain y tế nhạy cảm, cần HITL bắt buộc; chi phí validate lâm sàng cao hơn ROI ngắn hạn so với Card #1.
- **Card #1 (Xanh SM):** Bottleneck rõ, metric đo được, ranh giới an toàn (DRAFT_ONLY + pin < 5%) có thể kiểm thử bằng prompt prototype → phù hợp Deep-Dive nhất.