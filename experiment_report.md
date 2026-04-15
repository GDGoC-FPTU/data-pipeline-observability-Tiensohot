# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600003
**Name:** Bùi Đức Tiến
**Date:** 15/04/2026

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "the best choice is Laptop at $1200." | 9 | Kết quả hợp lý, đúng sản phẩm điện tử giá cao nhất |
| Garbage Data (`garbage_data.csv`) | "the best choice is Nuclear Reactor at $999999." | 1 | Hoàn toàn sai lệch do outlier cực đoan trong dữ liệu rác |

---

## 2. Phân tích & nhận xét

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Khi sử dụng `garbage_data.csv`, agent đã đưa ra gợi ý hoàn toàn phi thực tế là "Nuclear Reactor" với giá $999,999. Điều này xảy ra do nhiều vấn đề chất lượng dữ liệu cùng tồn tại:

- **Outliers (giá trị ngoại lệ cực đoan):** Dữ liệu rác chứa các bản ghi với giá trị `price` bất thường như $999,999. Vì logic của agent đơn giản là tìm sản phẩm có giá cao nhất trong category "electronics", nó sẽ chọn ngay outlier này mà không hề kiểm tra tính hợp lý.

- **Duplicate IDs / dữ liệu trùng lặp:** Các bản ghi trùng lặp làm sai lệch thống kê, khiến một số sản phẩm được "đếm" nhiều lần, tăng trọng số không đáng có.

- **Sai kiểu dữ liệu (wrong data types):** Nếu cột `price` chứa chuỗi hoặc giá trị không hợp lệ, các phép tính so sánh có thể cho ra kết quả ngẫu nhiên hoặc không nhất quán.

- **Null values:** Các bản ghi thiếu thông tin quan trọng (tên sản phẩm, category) vẫn có thể lọt qua nếu không có bước Validate, dẫn đến agent trả về dữ liệu vô nghĩa.

Tóm lại, agent không có cơ chế kiểm tra tính hợp lý của đầu ra — nó chỉ làm đúng theo logic được lập trình. Khi đầu vào là rác, đầu ra tất yếu cũng là rác (**Garbage In, Garbage Out**).

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Đồng ý.

Dù prompt và logic của agent có hoàn hảo đến đâu, nếu dữ liệu nguồn không được làm sạch và kiểm soát chất lượng, kết quả vẫn sẽ sai lệch nghiêm trọng. ETL Pipeline với bước Validate đóng vai trò quan trọng như một "lớp bảo vệ" trước khi dữ liệu được đưa vào bất kỳ hệ thống AI nào. Một AI agent thông minh cũng không thể bù đắp cho dữ liệu kém chất lượng — nền tảng dữ liệu tốt mới là yếu tố quyết định độ tin cậy của hệ thống.
