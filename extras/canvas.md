# AI Product Canvas: AI Customer Service Agent (VinFast)

## 1. Canvas Trọng Tâm

| Tiêu chí | Trả lời |
| :--- | :--- |
| **Value** | **User:** Khách hàng (chủ xe, khách hàng tiềm năng) và Nhân viên tổng đài CSKH (Agent).<br>**Pain:** Khách hàng phải chờ máy lâu vào giờ cao điểm; Agent bị quá tải bởi các câu hỏi lặp đi lặp lại (chính sách thuê pin, bảng giá, thủ tục bảo hành, vị trí trạm sạc).<br>**AI giải quyết:** Trả lời ngôn ngữ tự nhiên, linh hoạt và tức thì 24/7. Có khả năng truy xuất dữ liệu cá nhân hóa (ví dụ: tiến độ giao xe của riêng khách hàng) mà chatbot kịch bản (rule-based) cũ không làm được. |
| **Trust** | **Ảnh hưởng khi AI sai:** Khách hàng nhận sai thông tin về chính sách (giá cả, bảo hành), dẫn đến bức xúc, khiếu nại hoặc thậm chí là rủi ro khủng hoảng truyền thông.<br>**Cách user nhận biết:** Khách hàng thấy câu trả lời mâu thuẫn với website, hợp đồng hoặc AI lặp vòng vo không giải quyết được vấn đề.<br>**Cách user sửa:** Khách hàng bấm nút "Gặp nhân viên CSKH" (Human Handoff). Toàn bộ ngữ cảnh chat được chuyển qua người thật xử lý tiếp mà khách không cần nhắc lại từ đầu. |
| **Feasibility**| **Cost:** ~$0.01 - $0.03 / lượt hội thoại (dựa trên token LLM API).<br>**Latency:** < 2s / câu trả lời.<br>**Risk chính:** Hallucination (AI tự bịa ra chính sách bán hàng hoặc ưu đãi không có thật). Cần cơ chế RAG (Retrieval-Augmented Generation) kiểm soát nguồn dữ liệu cực kỳ chặt chẽ. |

## 2. Automation hay augmentation?

☑ **Automation** — AI làm thay, user không can thiệp (Đối với khách hàng cuối)

☑ **Augmentation** — AI gợi ý, user quyết định cuối cùng (Đối với nhân viên CSKH)

**Justify:**
Sản phẩm này hoạt động theo mô hình Hybrid:
* **Tier 1 (Automation):** Trực tiếp đối thoại với khách hàng để lọc và giải quyết các vấn đề cơ bản.
* **Tier 2 (Augmentation):** Khi vấn đề phức tạp, AI chuyển đoạn chat cho Agent người thật, đồng thời tóm tắt nội dung và *gợi ý sẵn câu trả lời* để Agent duyệt, chỉnh sửa và gửi đi (Cost of reject = 0, Agent chỉ mất vài giây để xóa/sửa gợi ý).

## 3. Learning signal

| # | Câu hỏi | Trả lời |
| :--- | :--- | :--- |
| 1 | User correction đi vào đâu? | Khi khách hàng yêu cầu chuyển sang người thật (Human Handoff), cách người thật trả lời và giải quyết vấn đề đó sẽ được lưu lại. Log này được dùng làm DPO (Direct Preference Optimization) để AI học cách xử lý các case khó tương tự trong tương lai. |
| 2 | Product thu signal gì để biết tốt lên hay tệ đi? | **Implicit:** Deflection Rate (Tỷ lệ ticket AI tự giải quyết thành công không cần người thật); Thời gian xử lý trung bình (AHT - Average Handling Time) của Agent giảm xuống.<br>**Explicit:** Nút Thumbs up/Thumbs down hoặc đánh giá CSAT (1-5 sao) sau khi kết thúc phiên chat. |
| 3 | Data thuộc loại nào? | ☑ **Domain-specific** (Chính sách, tài liệu kỹ thuật, cẩm nang xe VinFast)<br>☑ **User-specific** (Lịch sử bảo dưỡng, thông tin hợp đồng của khách)<br>☐ Real-time<br>☑ **Human-judgment** (Log sửa chữa câu trả lời từ Agent người thật)<br><br>**Có marginal value không?** Rất cao. Data bộ câu hỏi thực tế của người dùng và cách phản hồi chuẩn mực của nhân viên CSKH VinFast là "tài sản" độc quyền để fine-tune AI ngày càng giống con người và am hiểu văn hóa doanh nghiệp. |