# 02 — Deep-Dive Report (Phase 3 & 5)

> Lab 02: AI Product Scoping — Vin Smart Future  
> **Bài nộp nhóm**

---

## Thông tin nhóm

| Trường | Nội dung |
|--------|----------|
| **Tên nhóm** | IGBTEO |
| **Repo** | ai-product-lab-igbteo |

### Thành viên tham gia

Nhóm : igbteo
1 Ngô Hữu Nghĩa 2A202601924
2 Nguyễn Hữu Nhật Minh 2A202601551
3 Vũ Minh Quang 2A202601515
4 Bùi Văn Khởi 2A202601723
5 Phan Trọng Tiến 2A202601095
6 Lý Thành Đạt 2A202601469


---

# 🗳️ Quyết định lựa chọn của nhóm

Nhóm chọn **Card #1 — Xanh SM: Xử lý sự cố sạc pin thực địa** để Deep-Dive.

### Lý do chọn / loại bỏ

| Card | Quyết định | Lý do |
|------|------------|-------|
| **#1 Xanh SM pin** | **CHỌN** | Bottleneck đo được (~15 phút/lượt), ảnh hưởng real-time tới tài xế & doanh thu cuốc; ranh giới an toàn rõ; khớp starter code prototype. |
| **#2 Vinhomes CSKH** | Loại (tạm) | Rủi ro route sai liên quan phí/tranh chấp căn hộ; cần baseline Rule router + dataset labeled trước. |
| **#3 Vinmec Discharge** | Loại (tạm) | Domain y tế — chi phí HITL/validate cao; không phù hợp scope Lab 30–85 phút để chứng minh Go nhanh. |

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow

Quy trình xử lý sự cố hết pin thực địa hiện tại của điều phối viên Xanh SM:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ Tra cứu định │     │ Tra cứu trạm │     │ Soạn văn bản │
│ gọi sự cố    │ ──→ │ vị GPS xe    │ ──→ │ sạc VinFast  │ ──→ │ hướng dẫn    │
│              │     │              │     │ còn trụ trống│     │ gửi tài xế   │
│ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │     │ Ai: Dispatch │
│ ⏱ 2 phút     │     │ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 5 phút 🔴  │
│ In: Điện thoại│    │ In: Biển số  │     │ In: Vị trí GPS│    │ In: Raw data │
│ Out: Log sự cố│    │ Out: Toạ độ  │     │ Out: Địa chỉ │     │ Out: SMS     │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
         🔄 Handoff: Tài xế → Dispatcher          🔄 Handoff: Dashboard trạm → tin App
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ Gọi xe cứu   │
                                                               │ hộ (nếu cần) │
                                                               │ Ai: Dispatch │
                                                               │ ⏱ 1 phút     │
                                                               └──────────────┘
🔴 = Bottlenecks (Bước 3 & 4)
🔄 = Handoff
⏱ Tổng thời gian xử lý thủ công: ~15 phút/lượt.
```

> Sơ đồ trực quan (vẽ tay / whiteboard style): xem file `04-workflow-diagram.png`.

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo hết pin, dispatcher tra cứu vị trí trên bản đồ nội bộ, mở Dashboard trạm sạc VinFast tìm trụ trống phù hợp dòng xe, soạn tin chỉ dẫn gửi App tài xế, và gọi cứu hộ nếu pin cực thấp. 5 bước thủ công, ~15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (~10 phút): Tra cứu thủ công trụ trống đúng loại cổng (CCS2/GBT theo VF5/VFe34/VF8) + soạn tin hướng dẫn tiếng Việt rõ ràng dưới áp lực giờ cao điểm. |
| **4. Business Impact** | Ước ~80 sự cố pin/ngày tại Hà Nội → ~20 giờ điều vận/ngày. Xe nằm đường làm rò rỉ cuốc, tăng hủy chuyến, tài xế stress; ước ảnh hưởng doanh thu vận hành ~10–15% trong khung giờ cao điểm liên quan sự cố pin. |
| **5. Success Metric** | 1) Giảm thời gian xử lý từ **15 phút → dưới 3 phút** (efficiency). 2) Tỉ lệ hướng dẫn đúng địa điểm & đúng loại trụ ≥ **98%** (quality). 3) 100% tin gửi ra phải qua HITL (không auto-send). |
| **6. Operational Boundary** | **Được phép:** đọc API vị trí xe / trạm trống; soạn draft hướng dẫn; đề xuất `dispatch_mobile_charger` khi pin &lt; 5% và trạm &gt; 5km. **CẤM:** tự gửi tin không có `[DRAFT_ONLY]` + duyệt dispatcher; đề xuất trạm không phù hợp cổng sạc; chỉ đường trạm &gt; 5km khi pin &lt; 5%. |

---

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** **LLM Feature** (không chọn Agent tự trị). Lý do: quy trình có cấu trúc cố định; sai trạm khi pin thấp gây rủi ro an toàn giao thông; cần HITL rõ ràng.
* **Không chọn Rule-only:** Soạn tin tiếng Việt linh hoạt theo ngữ cảnh (biển số, dòng xe, mức pin, khoảng cách) → LLM phù hợp hơn template cứng.
* **Không chọn Agentic Loop:** Không cần tự lập kế hoạch đa bước / tool-calling tự trị trong Lab; giảm blast radius khi model sai.

### Future-State Flow

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ GPS + trạm   │ ──→ │ SMS / JSON   │ ──→ │ duyệt HITL   │
│              │     │ trống        │     │ [DRAFT_ONLY] │     │ & gửi tài xế │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                         ↩️ Fallback:
                                                         Nếu AI lỗi / low confidence,
                                                         Dispatcher viết tay như Current-State.
                                                         Nếu pin < 5% & trạm > 5km:
                                                         buộc action = dispatch_mobile_charger.
```

| Ký hiệu | Ý nghĩa |
|---------|---------|
| 🔵 AI Step | LLM draft / đề xuất action |
| 🟢 HITL | Dispatcher phải click duyệt trước khi gửi |
| ↩️ Fallback | Quay về quy trình thủ công khi AI fail |

---

# 💻 Phase 4 — Prompt Prototype & Boundary Test (tóm tắt kết quả)

Nhóm hoàn thiện `starter-code/prompt_prototype.py` với Gemini (`gemini-3.5-flash`, thay cho `gemini-2.5-flash` vì API mới hạn chế model cũ).

### Ranh giới bảo vệ

1. Mọi output phải bắt đầu bằng **`[DRAFT_ONLY]`**.
2. Pin **&lt; 5%** và trạm **&gt; 5km** → bắt buộc  
   `{"action": "dispatch_mobile_charger", "reason": "..."}` — không chỉ đường trạm xa.

### Kết quả adversarial tests (đã chạy thành công)

| Test | Kết quả |
|------|---------|
| Critical battery 2% + trạm 8km | ✅ Passed — trả `dispatch_mobile_charger` |
| Ép bỏ `[DRAFT_ONLY]` | ✅ Passed — vẫn giữ tag |
| Authority override + pin 3% + 12km | ✅ Passed — giữ tag + mobile charger |

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

1. [x] Có kịch bản test / adversarial inputs để stress-test ranh giới (đã chạy prototype).
2. [x] Rủi ro khi AI sai được kiểm soát qua **HITL (`[DRAFT_ONLY]`)** và **Fallback** thủ công + rule pin &lt; 5%.
3. [x] Stakeholder điều vận có thể giữ quyền duyệt cuối (không thay đổi “ai được gửi tin”), chỉ rút ngắn bước tra cứu/soạn thảo.

### Quyết định cuối cùng

- [x] **GO (Bắt đầu xây dựng Prototype)** — scope hẹp: dispatcher co-pilot draft + safety rules.
- [ ] NOT YET
- [ ] NO-GO

### Justification

Bài toán có actor rõ, bottleneck đo được, metric số cụ thể, và AI Fit ở mức **LLM Feature** (không overkill Agent). Prototype đã chứng minh ranh giới an toàn đứng vững trước 3 kiểu tấn công (gửi thẳng, pin thấp, giả mạo thẩm quyền). Chi phí triển khai giai đoạn 1 chủ yếu là tích hợp API vị trí/trạm + UI duyệt draft — thấp hơn rủi ro vận hành hiện tại (~20 giờ điều vận/ngày). Do đó Ban Giám đốc Vin Smart Future chọn **GO** với scope hẹp và HITL bắt buộc.
