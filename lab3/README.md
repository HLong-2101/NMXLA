
# 🎓 Nhập môn Xử lý ảnh

## LAB 2: Ảnh kỹ thuật số & màu

- Hiểu và thực hành các kỹ thuật biến đổi ảnh đơn giản.
- Làm quen với xử lý ảnh bằng OpenCV, NumPy và Matplotlib.
- Kết hợp nhiều kỹ thuật như: đổi màu, làm mờ, tăng độ nét, biến đổi tần số.

---

## 🛠️ Cần chuẩn bị gì?

### 1. Cài Python và thư viện cần thiết

Chạy lệnh sau trong Terminal hoặc cmd:

```
pip install opencv-python numpy matplotlib scipy
```

### 2. Cấu trúc thư mục đề xuất

```
.
├── exercise/                        # Ảnh gốc (đầu vào)
├── output/                         $ Ảnh sau biến đổi cường độ
├── output_random_transform/        # Ảnh sau biến đổi màu + cường độ
├── output_fft/                     # Ảnh sau biến đổi tần số
├── output_fft_combined/           # Ảnh xử lý nâng cao (color + filter)
├── 2.1.ipynb 
├── 2.2.ipynb
├── 2.3.ipynb
├── 2.4.ipynb
├── README.md
```

---

## 📚 Các bài và chức năng chính

### 📊 Bài 2.1 - Biến đổi cường độ ảnh
File: `2.1.ipynb`
- Áp dụng 1 trong 5 phép biến đổi ảnh ngẫu nhiên:
  - `I`: Đảo ảnh (âm bản)
  - `G`: Làm tối/sáng bằng Gamma Correction
  - `L`: Biến đổi log (làm sáng vùng tối)
  - `H`: Cân bằng histogram (tăng tương phản)
  - `C`: Giãn tương phản
  
---

### 📊 Bài 2.2 – Biến đổi miền tần số
File: `2.2.ipynb`

- Chọn phím `F`, `L`, `H` để thực hiện:
  - `F`: Fast Fourier Transform (hiện phổ tần số)
  - `L`: Lọc Butterworth Lowpass (làm mờ)
  - `H`: Lọc Butterworth Highpass (làm nét)

👉 Kết quả lưu vào `output_fft/`

---
### 🧪 2.3 – Thay đổi màu RGB và biến đổi cường độ ảnh
File: `2.3.ipynb`

- Đảo thứ tự kênh màu RGB ngẫu nhiên
- Áp dụng 1 trong 5 phép biến đổi ảnh ngẫu nhiên:
  - `I`: Đảo ảnh (âm bản)
  - `G`: Làm tối/sáng bằng Gamma Correction
  - `L`: Biến đổi log (làm sáng vùng tối)
  - `H`: Cân bằng histogram (tăng tương phản)
  - `C`: Giãn tương phản

👉 Kết quả hiển thị và lưu vào `output_random_transform/`

---
### 🧠 Bài 2.4 – Thay đổi màu RGB và biến đổi miền tần số
File: `2.4.ipynb`

- Đảo thứ tự màu RGB
- Ngẫu nhiên chọn 1 trong 3 biến đổi:
  - `F`: Fast Fourier
  - `L`: Butterworth Lowpass + Min Filter (lọc + làm mờ)
  - `H`: Butterworth Highpass + Max Filter (lọc + làm nét)

👉 Kết quả lưu trong `output_fft_combined/`

---

## 🧠 Giải thích các kỹ thuật đơn giản

| Kỹ thuật | Mô tả đơn giản |
|---------|----------------|
| Inverse | Lật ngược màu ảnh: sáng → tối, tối → sáng |
| Gamma   | Làm tối hoặc sáng ảnh theo cấp số mũ |
| Log     | Làm nổi bật chi tiết ở vùng tối |
| Histogram Equalization | Cân bằng ánh sáng toàn ảnh |
| Contrast Stretching | Tăng độ tương phản bằng cách kéo dãn mức sáng |
| FFT     | Chuyển ảnh sang dạng tần số để xử lý lọc |
| Butterworth | Lọc ảnh trong miền tần số, loại bỏ nhiễu hoặc làm nét |
| Min Filter | Làm mờ chi tiết nhỏ (lọc điểm nhỏ) |
| Max Filter | Làm rõ chi tiết cạnh (nhấn mạnh vùng sáng) |

---

## ✅ Cách chạy thử

1. Mở Jupyter Notebook hoặc VS Code.
2. Chạy từng notebook `.ipynb`.
3. Xem kết quả ảnh được hiển thị và lưu vào các thư mục tương ứng.

---

## 💬 Ghi chú thêm

- Các thư mục output sẽ chứa ảnh kết quả để bạn nộp bài hoặc demo.
- Ảnh gốc nên có trong thư mục `exercise/`, định dạng `.jpg`, `.png`...

---


