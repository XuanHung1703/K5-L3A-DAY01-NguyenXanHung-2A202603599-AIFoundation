# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Bản nháp tổng hợp từ output của notebook đã tải. Các mục CHƯA HOÀN THÀNH cần được bổ sung trước khi nộp; chưa có kết quả LLM chạy thành công.

- **MSSV:**
- **Họ và tên:**
- **SEED** (in ở ô đầu notebook): 6576

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — có output, còn ô chưa chạy | Có bản nháp |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

Số liệu dưới đây chép từ bảng kết quả đã lưu trong notebook, không phải kết quả chạy lại.

| Phương án | F1 test | F1 SUB 40 | p95 ms/tin | USD/1k tin |
| --- | ---: | ---: | ---: | ---: |
| BoW + LogReg | 0.861 | 0.857 | 0.157 | 0.0 |
| BoW + MLP | 0.904 | 0.889 | 0.142 | 0.0 |
| Encoder + kNN | 0.908 | 0.976 | 42.835 | 0.0 |
| Encoder + LogReg | 0.960 | 1.000 | 30.595 | 0.0 |
| Encoder + MLP head | 0.965 | 1.000 | 31.851 | 0.0 |

LLM chưa có kết quả hợp lệ: các lần gọi API lưu trong notebook báo RateLimitError 429 do hết tín dụng. Không coi kết quả kiểm tra bằng API giả lập là kết quả phân loại thật. Cột chi phí 0.0 của các mô hình cục bộ không bao gồm chi phí phần cứng hoặc vận hành.

## 3. Dự đoán và thực tế

CHƯA HOÀN THÀNH: tự chọn và giải thích 3 dự đoán lệch nhiều nhất. Các bằng chứng có sẵn để đối chiếu:

- `ml_f1`: dự đoán F1 khoảng 0.8; output F1 test là 0.861.
- `emb_tricky`: dự đoán Encoder + LogReg đúng nhiều hơn BoW; output cho thấy cả hai đều đúng 5/6 tin.
- `tradeoff`: dự đoán chưa có phương án đạt p95 < 50 ms; output cho thấy cả 5 phương án đã đo đều đạt ngưỡng này. Dự đoán về hai phương án nằm trên đường Pareto khớp output.

## 4. Phân tích 50 case khó

| Phương án | F1 SUB thường | F1 50 case khó | Accuracy 50 case khó |
| --- | ---: | ---: | ---: |
| BoW + LogReg | 0.857 | 0.378 | 0.54 |
| BoW + MLP | 0.889 | 0.410 | 0.54 |
| Encoder + kNN | 0.976 | 0.746 | 0.70 |
| Encoder + LogReg | 1.000 | 0.667 | 0.64 |
| Encoder + MLP head | 1.000 | 0.667 | 0.64 |

Theo số liệu làm tròn đã lưu, hai phương án BoW cùng giảm khoảng 0.479 F1, lớn nhất trong các phương án đã đo. Output ghi nhận cả 5 mô hình đều sai các case #12, #13, #14, #16 và #19 thuộc nhóm spam giọng tự nhiên.

CHƯA HOÀN THÀNH: chọn 2 case, tự giải thích cơ chế gây lỗi. Chưa thể so sánh LLM hoặc prompt phòng thủ do chưa có kết quả API thành công.

## 5. Thử thách cá nhân

CHƯA HOÀN THÀNH: ô chọn đề và ô code thử thách trong bản notebook đã tải chưa chạy. Chạy ô chọn đề trên Colab, tự thực hiện theo yêu cầu không dùng AI của lab, rồi ghi cách làm, kết quả và giải thích vào đây.

## 6. Phương án cho sản phẩm thật

Notebook đã có câu trả lời chọn Encoder + LogReg theo F1 SUB và p95: F1 SUB = 1.000, p95 = 30.595 ms/tin. BoW + MLP và Encoder + LogReg nằm trên đường Pareto trong output đã lưu.

CHƯA HOÀN THÀNH: xác nhận lựa chọn và tự bổ sung đánh đổi khi triển khai 10 triệu SMS/ngày. Phân biệt độ trễ từng tin với năng lực xử lý: throughput cả lô đo được của Encoder + LogReg là 63.025 tin/s, thấp hơn mức trung bình khoảng 116 tin/s của yêu cầu. Số đo Colab này chưa chứng minh một tiến trình đáp ứng đủ tải; cần kiểm chứng batching hoặc nhiều worker trên môi trường triển khai. Không có số đo chi phí LLM thực tế trong bản notebook này.

## 7. AI log và điều đã học

| Phạm vi | Yêu cầu với AI | Hỗ trợ đã thực hiện | Tự kiểm chứng |
| --- | --- | --- | --- |
| Chuẩn bị bài nộp | Hướng dẫn tải bài từ Colab và sắp xếp thư mục trên ổ D | Đọc hướng dẫn nộp; sao chép notebook; đổi tên báo cáo mẫu; tổng hợp các output có sẵn và liệt kê mục còn thiếu | CHƯA HOÀN THÀNH: đối chiếu với Colab và xác nhận nội dung trước khi nộp |

CHƯA HOÀN THÀNH: bổ sung các lần dùng AI trước đó, nếu có, và tự viết 2–3 câu về điều đã học. Bảng trên chỉ ghi hỗ trợ trong chat này, không đại diện toàn bộ quá trình làm bài.
