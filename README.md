[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23574085&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** tienvodich456@gmail.com
**Name:** Bùi Đức Tiến

---

## Mô tả

Bài lab Day 10 xây dựng một ETL Pipeline tự động gồm 4 bước: Extract (đọc dữ liệu JSON), Validate (loại bỏ records không hợp lệ), Transform (chuẩn hóa category, tính giá giảm 10%, thêm timestamp), và Load (lưu ra CSV). Ngoài ra, bài lab còn thực hiện thí nghiệm so sánh ảnh hưởng của dữ liệu sạch vs dữ liệu rác đối với một AI Agent đơn giản (RAG simulation).

---

## Cách chạy (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Chạy Agent Simulation (Stress Test)
```bash
# Chạy agent với cả 2 bộ dữ liệu để so sánh kết quả
python agent_simulation.py
```

Kết quả mong đợi:
- **Clean data** (`processed_data.csv`): Agent trả lời chính xác với sản phẩm phù hợp.
- **Garbage data** (`garbage_data.csv`): Agent trả lời sai lệch do dữ liệu không được kiểm soát chất lượng.

---

## Cấu trúc thư mục

```
├── solution.py              # ETL Pipeline script
├── agent_simulation.py      # Agent stress test script
├── raw_data.json            # Dữ liệu đầu vào
├── garbage_data.csv         # Dữ liệu rác dùng để stress test
├── processed_data.csv       # Output của pipeline
├── experiment_report.md     # Báo cáo thí nghiệm
└── README.md                # File này
```

---

## Kết quả

### ETL Pipeline (`solution.py`)

| Bước | Kết quả |
|------|---------|
| Extract | Đọc thành công từ `raw_data.json` |
| Validate | **Valid: 3**, Errors (bị loại): **2** |
| Transform | Thêm `discounted_price` (giảm 10%), chuẩn hóa `category`, thêm `processed_at` |
| Load | **3 records** đã lưu vào `processed_data.csv` |

### Agent Simulation (`agent_simulation.py`)

| Scenario | Agent Response | Nhận xét |
|----------|----------------|----------|
| Clean Data (`processed_data.csv`) | "the best choice is **Laptop** at **$1200**" | Chính xác, phù hợp với dữ liệu thật |
| Garbage Data (`garbage_data.csv`) | "the best choice is **Nuclear Reactor** at **$999999**" | Sai lệch nghiêm trọng do outlier trong dữ liệu rác |

> **Kết luận:** Dữ liệu chất lượng kém khiến AI Agent đưa ra quyết định sai. Quality Data > Quality Prompt.
