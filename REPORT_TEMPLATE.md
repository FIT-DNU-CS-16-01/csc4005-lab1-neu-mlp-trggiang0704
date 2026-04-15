# CSC4005 – Lab 1 Report

## 1. Mục tiêu

Mục tiêu của bài lab là xây dựng và đánh giá một mô hình MLP cho bài toán phân loại bề mặt thép sử dụng bộ dữ liệu **NEU Surface Defect (NEU-CLS)**.  

Dataset gồm 6 lớp:
- Crazing
- Inclusion
- Patches
- Pitted_Surface
- Rolled-in_Scale
- Scratches

Bài lab tập trung vào:
- Huấn luyện mô hình MLP
- Theo dõi learning curves
- So sánh nhiều cấu hình (optimizer, regularization)
- Chọn mô hình tốt nhất dựa trên validation set

---

## 2. Cấu hình thí nghiệm

Ba cấu hình được sử dụng:

### 🔹 Run A – Baseline (AdamW)
- Optimizer: AdamW
- Learning rate: 0.001
- Weight decay: 0.0001
- Dropout: 0.3

### 🔹 Run B – SGD
- Optimizer: SGD
- Learning rate: 0.01
- Weight decay: 0.0
- Dropout: 0.3

### 🔹 Run C – Strong Regularization
- Optimizer: AdamW
- Learning rate: 0.0005
- Weight decay: 0.001
- Dropout: 0.5

---

## 3. Kết quả

### 📊 Tổng hợp kết quả

| Run | Optimizer | Best Val Acc | Test Acc | Val Loss |
|-----|----------|-------------|----------|---------|
| Baseline | AdamW | ~0.4185 | ~0.3815 | ~1.49 |
| Run B | SGD | **~0.4630** | **~0.4556** | **~1.44** |
| Run C | AdamW + Strong Reg | ~0.3296 | ~0.3444 | ~1.63 |

---

### 📈 Learning Curves

📌 *(Chèn hình: curves của từng run hoặc so sánh 3 run)*

- Baseline: học ổn định, tăng đều
- Run B: tăng mạnh sau epoch 10, hội tụ tốt
- Run C: tăng chậm, hiệu năng thấp

---

### 🔥 Confusion Matrix

📌 *(Chèn hình: confusion matrix của 3 run)*

- Run B phân loại tốt nhất
- Run C bị lệch dự đoán (collapse)
- Baseline ở mức trung bình

---

## 4. Phân tích

### 🔹 Cấu hình tốt nhất

👉 **Run B (SGD)** là cấu hình tốt nhất

Vì:
- Val accuracy cao nhất (~46.3%)
- Val loss thấp nhất (~1.44)
- Learning curves ổn định
- Confusion matrix tốt nhất

---

### 🔹 Overfitting / Underfitting

- Không có overfitting rõ ràng ở cả 3 run
- Run C bị **underfitting mạnh**
- Baseline bị **underfitting nhẹ**
- Run B cải thiện tốt nhất

---

### 🔹 So sánh AdamW vs SGD

| Tiêu chí | AdamW | SGD |
|--------|------|-----|
| Tốc độ học ban đầu | Nhanh | Chậm |
| Ổn định | Cao | Trung bình |
| Hiệu năng cuối | Trung bình | **Tốt nhất** |

👉 Trong thí nghiệm:
- AdamW học nhanh nhưng dừng sớm
- SGD học chậm hơn nhưng **hội tụ tốt hơn**

---

## 5. Kết luận

Mô hình tốt nhất là:

👉 **Run B – SGD**

### Lý do:
- Validation accuracy cao nhất
- Validation loss thấp nhất
- Generalization tốt nhất
- Confusion matrix tốt nhất

---

### 📊 Đánh giá trên test set

- Test Accuracy: ~45.56%
- Test Loss: ~1.39

---

### 📋 Nhận xét

- Model hoạt động tốt với các class:
  - Patches
  - Crazing
  - Scratches

- Model kém với:
  - Inclusion (F1-score = 0)

👉 Nguyên nhân:
- Class khó phân biệt
- Có thể do imbalance hoặc đặc trưng chồng lấn

---

### 🔚 Kết luận chung

- SGD outperform AdamW trong bài toán này
- Regularization quá mạnh gây underfitting
- MLP đạt ~45% accuracy → còn hạn chế

👉 Hướng cải thiện:
- Dùng CNN thay MLP
- Data balancing
- Tuning sâu hơn

---

## (Optional) Self-check Questions

(Phần này bạn có thể giữ hoặc bỏ tùy yêu cầu nộp)

- Validation dùng để chọn model
- Test dùng để đánh giá cuối
- Cross-Entropy phù hợp multi-class
- SGD hội tụ tốt hơn trong bài này
- Overfitting: train tốt, val kém
- W&B giúp so sánh run dễ dàng