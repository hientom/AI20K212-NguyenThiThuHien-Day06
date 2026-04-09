# Individual reflection — [Tên của bạn] ([Mã sinh viên])

## 1. Role
Backend Developer. Phụ trách xây dựng module Search và Local Caching (Phase 3) để truy xuất dữ liệu từ bên ngoài, làm đầu vào chuẩn xác cho AI xử lý ở các phase sau.

## 2. Đóng góp cụ thể
- Xây dựng hoàn chỉnh file `search.py` đáp ứng tiêu chí: không chứa logic AI, thuần HTTP request và thao tác đọc/ghi JSON.
- Thiết kế luồng kiểm tra cache nội bộ (`check_cache`) trước khi gọi API ngoài, giúp tiết kiệm tài nguyên và tăng tốc độ phản hồi.
- Tích hợp DuckDuckGo Instant Answer API (`search_ddg`) và viết logic parse dữ liệu linh hoạt từ nhiều nguồn (Abstract, Related Topics, Direct Results) để lấy tối đa 3 snippets.
- Thiết lập chế độ `OFFLINE_MODE` với mock data (VF 8, VF 3) để đảm bảo team vẫn có thể demo và test các phase khác khi không có internet.

## 3. SPEC mạnh/yếu
- **Mạnh nhất:** Xử lý lỗi (Error Handling) và Edge Cases rất chặt chẽ. Hệ thống không bị sập (crash) khi DDG timeout (>5s), API trả lỗi 4xx/5xx, mất mạng, hay file `cache.json` bị hỏng. Tất cả đều có fallback return rỗng hoặc bỏ qua khéo léo.
- **Yếu nhất:** Logic match cache hiện tại là Exact Match (chỉ lowercase và bỏ dấu câu cuối). Nếu user nhập "Giá VF 8" và "VF 8 giá bao nhiêu", hệ thống sẽ tính là 2 query khác nhau và dẫn đến Cache Miss, gây lãng phí API call.

## 4. Đóng góp khác
- Viết sẵn bộ test script chạy cục bộ ở cuối file (không cần API key) để test hàm `_normalize`, luồng ghi/đọc cache, giúp team dễ dàng verify logic trước khi ráp nối.
- Thiết kế sẵn các hàm API nội bộ (`add_to_cache`, `get_all_cache`, `delete_cache_entry`) cực kỳ gọn gàng để Phase 5 (Admin) có thể gọi và quản lý trực tiếp mà không cần can thiệp vào logic bên trong.

## 5. Điều học được
Trước đây mình hay gộp chung việc gọi API tìm kiếm và đưa cho AI xử lý vào chung một function. Qua task này, mình nhận ra việc tách bạch module Search (chỉ trả về snippets/urls thuần) và module AI là cực kỳ quan trọng. Nó giúp việc debug dễ dàng hơn hẳn (nếu lỗi tìm kiếm thì chỉ check file `search.py`, không đổ lỗi cho AI hallucination) và giúp hệ thống dễ scale/bảo trì hơn.

## 6. Nếu làm lại
Sẽ cải thiện hàm `_normalize(query)` hoặc đưa thêm cơ chế Semantic Caching (hoặc ít nhất là fuzzy matching nhẹ nhàng) thay vì string exact match. Chẳng hạn, loại bỏ các từ stopwords (như "là", "thì", "bao nhiêu") để tăng tỷ lệ Cache Hit. Ngoài ra, nếu có thời gian sẽ dùng `aiohttp` thay vì `requests` đồng bộ để hệ thống mượt hơn khi có nhiều user request cùng lúc.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Mình đã dùng AI để viết nhanh các hàm đọc/ghi file an toàn (với `utf-8` và `ensure_ascii=False`) và tạo bộ mock data chuẩn form JSON rất tiết kiệm thời gian. AI cũng gợi ý việc set `timeout=5` cho `requests` – một chi tiết nhỏ nhưng cứu thua cho toàn bộ app nếu DDG API bị treo.
- **Sai/mislead:** Ban đầu khi nhờ AI thiết kế caching, nó khuyên dùng luôn Redis hoặc SQLite. Nghe thì "xịn" nhưng lại làm architecture của một project nhỏ/hackathon trở nên quá cồng kềnh và khó setup cho các thành viên khác. Mình đã phải phớt lờ gợi ý đó và ép nó viết lại bằng `json.load/dump` file tĩnh để giữ đúng tiêu