# 🚗 VinFast Auto-Agent: Báo cáo Bài nộp Day 06

  
**Dự án:** Trợ lý AI tư vấn xe VinFast (VinFast Auto-Agent)

---

## 📌 Giới thiệu (Overview)
Thư mục này chứa toàn bộ các tài liệu báo cáo, đánh giá cá nhân và nhật ký kiểm thử của cá nhân mình cho dự án **VinFast Auto-Agent** trong Lab **Day 06**. 

Trong dự án này, mình đảm nhận vai trò chính là thiết kế **AI Engine**, xây dựng module **Search & Local Caching (Phase 3)**, viết System Prompt và thực hiện khâu kiểm thử tính an toàn/độ chính xác của mô hình.

---

## 📂 Cấu trúc thư mục (Directory Structure)

Dưới đây là giải thích chi tiết về các tệp tài liệu có trong thư mục bài nộp:

* **`reflection.md`**
  Bản đánh giá cá nhân (Individual Reflection) trình bày chi tiết về vai trò, những đóng góp cụ thể vào mã nguồn (thiết kế luồng Caching, xử lý lỗi API), phân tích điểm mạnh/yếu của đặc tả kỹ thuật (SPEC) và những bài học rút ra được sau khi làm việc với AI trong dự án thực tế.

* **`extras/`** (Thư mục tài liệu bổ sung/Bonus)
  * **`prompt-tests.md`***: Bộ ca kiểm thử (Test Cases Logs) cực kỳ chi tiết. Ghi lại quá trình mình "tra tấn" (stress-test) System Prompt của AI Engine với 11 kịch bản khác nhau (Happy path, Off-topic, Prompt Injection, Low-confidence, v.v.) để đảm bảo AI phản hồi an toàn và chính xác.
  * **`canvas.md`**: Bản vẽ thiết kế luồng kịch bản hội thoại và cấu trúc kiến trúc của dự án.

* **`.gitignore`**
  File cấu hình Git để loại bỏ các tệp tin không cần thiết hoặc chứa dữ liệu nhạy cảm (như `secrets.toml` chứa API Key, thư mục `__pycache__`, file log hệ thống) khi đẩy code lên kho lưu trữ.

---

## 🚀 Điểm nhấn kỹ thuật cá nhân (Key Highlights)
1. **Kiến trúc Caching thông minh:** Xây dựng luồng `check_cache` trước khi gọi DuckDuckGo API để tối ưu tài nguyên và tăng tốc độ phản hồi.
2. **Xử lý lỗi (Error Handling):** Hệ thống được thiết kế Fallback mượt mà, không bị sập (crash) khi API bên thứ 3 bị timeout.
3. **Guardrail & AI Safety:** Thiết kế System Prompt chặt chẽ giúp AI từ chối các câu hỏi so sánh đối thủ, câu hỏi ngoài lề hoặc nỗ lực Jailbreak từ người dùng.

