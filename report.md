# Báo Cáo Lab Day21-Track2 — CI/CD cho AI Systems       
                                                                  
**Học viên:** Trương Đăng Nghĩa                                                    
**Khóa:** AIInAction - VinUni - Day 21

## Bước 1 — Bộ siêu tham số đã chọn
n_estimators: 200
max_depth: 10
min_samples_split: 5

**Kết quả run được chọn:**
- Accuracy = 0.644 
- f1 = 0.642

---

## Lý do chọn

So sánh các run đã thực nghiệm:
| Run | n_estimators | max_depth | min_samples_split | accuracy | f1_score |
|---|---|---|---|---|---|
| 1 | 100 | 5 | 2 | 0.564 | 0.553 |
| 2 | 50 | 3 | 2 | 0.558 | 0.518 |
| 3 | 200 | 10 | 5 | 0.644 | 0.642 |
| 4 | 300 | 20 | 2 | 0.678 | 0.677 |
| 5 | 500 | null | 2 | 0.676 | 0.675 |
| 6 | 500 | 15 | 2 | 0.674 | 0.672 |


**Phân tích ngắn:**

- Run 2 (50 cây, độ sâu 3) cho kết quả thấp nhất (f1=0.518), thể hiện rõ tình trạng **underfitting**: cây quá nông không đủ khả năng học các ranh giới giữa 3 lớp chất lượng trên 12 đặc trưng hóa học vốn có tương tác phi tuyến phức tạp.
- So với Run 1, Run 3 cải thiện accuracy ~8 điểm phần trăm (0.564 → 0.644) nhờ kết hợp **tăng độ sâu (5 → 10)** và **tăng số cây (100 → 200)**. Trong đó, `max_depth` đóng vai trò quyết định: cây sâu hơn cho phép split theo nhiều đặc trưng nối tiếp, nắm bắt được tương tác giữa `alcohol`, `volatile_acidity`, `sulphates` — vốn ảnh hưởng mạnh đến chất lượng rượu.
- `min_samples_split=5` ở Run 3 đóng vai trò **regularization nhẹ**, tránh overfit khi tree mở rộng đến độ sâu 10. Do đó Run 3 được chọn làm bộ siêu tham số đưa vào pipeline CI/CD ở Bước 2.
