
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

🔁 Hàm `inverse_image(img)`
---
```python
def inverse_image(img):
    return 255 - img
```
Chức năng:
Thực hiện phép biến đổi âm bản cho ảnh bằng cách đảo ngược giá trị pixel với phép tính 255 - pixel


🌗 Gamma Correction
---
```python
def gamma_correction(img, gamma=2.2):
    norm = img / 255.0
    corrected = np.power(norm, gamma)
    return np.uint8(corrected * 255)
    
```
Điều chỉnh độ sáng của ảnh theo quy luật phi tuyến.

Nguyên lý:
Áp dụng công thức:
I_out = (I_in / 255)^gamma * 255

Nếu gamma > 1: làm cho ảnh trở nên tối hơn (giảm sáng)

Nếu gamma < 1: làm cho ảnh sáng hơn (tăng sáng)

🔎 Log Transformation
---
```python
def log_transform(img):
    img_float = img.astype(np.float32)
    log_image = np.log1p(img_float)
    log_image = cv2.normalize(log_image, None, 0, 255, cv2.NORM_MINMAX)
    return np.uint8(log_image)
```
Tăng cường chi tiết các vùng tối của ảnh bằng cách sử dụng phép biến đổi logarit.

Tính log(1 + pixel) với mỗi giá trị pixel, giúp nén dải giá trị pixel lại và nhấn mạnh các chi tiết vùng có giá trị thấp.
Sau đó, chuẩn hóa lại kết quả về khoảng 0–255 bằng cv2.normalize().

📈 Histogram Equalization
---
```python
def histogram_equalization(img):
    if len(img.shape) == 2:
        return cv2.equalizeHist(img)
    else:
        ycrcb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)
        ycrcb[:, :, 0] = cv2.equalizeHist(ycrcb[:, :, 0])
        return cv2.cvtColor(ycrcb, cv2.COLOR_YCrCb2BGR)
```
Cân bằng histogram của ảnh để nâng cao độ tương phản.

Với ảnh grayscale: Áp dụng trực tiếp cv2.equalizeHist().

Với ảnh màu: Chuyển hệ màu sang YCrCb, chỉ cân bằng kênh Y (độ sáng) để không làm sai lệch màu sắc, rồi chuyển lại về BGR.

🌓 Contrast Stretching
---
```python
def contrast_stretching(img):
    in_min = np.min(img)
    in_max = np.max(img)
    stretched = (img - in_min) * (255 / (in_max - in_min))
    return np.uint8(stretched)
```
Kéo giãn dải giá trị pixel từ khoảng [min, max] của ảnh ban đầu thành [0, 255].

Áp dụng công thức:
I_out = (I_in - min) * (255 / (max - min))
Giúp làm tăng độ tương phản của ảnh.

⚡ Fast Fourier Transform (FFT)
---
```python
def fast_fourier(img):
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    f = fft2(gray)
    fshift = fftshift(f)
    magnitude = 20 * np.log(np.abs(fshift) + 1)
    return np.uint8(cv2.normalize(magnitude, None, 0, 255, cv2.NORM_MINMAX))
```
Biến đổi ảnh từ miền không gian sang miền tần số bằng FFT và tính phổ biên (magnitude spectrum).

Chuyển ảnh màu sang grayscale.

Áp dụng hàm fft2 để tính biến đổi Fourier.

Sử dụng fftshift để đưa tần số thấp về trung tâm.

Tính giá trị biên của phổ bằng logarit và chuẩn hóa về khoảng [0, 255].

🧽 Butterworth Filter
---
```python
def butterworth_filter(img, d0, n=2, highpass=False):
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    rows, cols = gray.shape
    u = np.arange(rows)
    v = np.arange(cols)
    u[u > rows // 2] -= rows
    v[v > cols // 2] -= cols
    V, U = np.meshgrid(v, u)
    D = np.sqrt(U**2 + V**2)
    if highpass:
        H = 1 / (1 + (d0 / (D + 1e-5))**(2 * n))
    else:
        H = 1 / (1 + (D / d0)**(2 * n))
    F = fftshift(fft2(gray))
    G = H * F
    g = np.abs(ifft2(ifftshift(G)))
    return np.uint8(cv2.normalize(g, None, 0, 255, cv2.NORM_MINMAX))
```

Áp dụng mặt nạ lọc Butterworth cho ảnh trong miền tần số.

Chuyển ảnh sang grayscale.

Tính khoảng cách D(u,v) của mỗi điểm tần số so với trung tâm.

Tùy thuộc vào tham số highpass:

Lowpass: H(u,v) = 1 / (1 + (D/d0)^(2n))

Highpass: H(u,v) = 1 / (1 + (d0/D)^(2n))

Nhân mặt nạ H với phổ Fourier và áp dụng biến đổi ngược (iFFT) để thu được ảnh đã xử lý.

```python
def butterworth_lowpass(img):
    return butterworth_filter(img, d0=50, n=2, highpass=False)

def butterworth_highpass(img):
    return butterworth_filter(img, d0=50, n=2, highpass=True)

```

Lowpass Filter: Loại bỏ tần số cao, làm mờ ảnh.

Highpass Filter: Loại bỏ tần số thấp, nhấn mạnh cạnh.

🔽 Min Filter & 🔼 Max Filter
---
```python
def min_filter(img):
    return cv2.erode(img, np.ones((3, 3), np.uint8))

def max_filter(img):
    return cv2.dilate(img, np.ones((3, 3), np.uint8))

```
Min Filter (Erosion): Áp dụng phép lọc nhỏ bằng cách lấy giá trị nhỏ nhất của các pixel lân cận, giúp làm mờ hoặc loại bỏ các nhiễu nhỏ.

Max Filter (Dilation): Áp dụng phép lọc lớn bằng cách lấy giá trị lớn nhất, giúp nhấn mạnh các vùng có giá trị cao.

🎨 Đảo thứ tự RGB
---
```python
def random_rgb_order(img):
    channels = list(cv2.split(img))
    random.shuffle(channels)
    return cv2.merge(channels)

```
Hàm cv2.split(img) tách ảnh thành 3 kênh màu (thường là B, G, R).

Chuyển tuple các kênh thành list để có thể thay đổi thứ tự bằng hàm random.shuffle.

Gộp lại các kênh sau khi sắp xếp ngẫu nhiên bằng cv2.merge.

## ✅ Cách chạy thử

1. Mở Jupyter Notebook hoặc VS Code.
2. Chạy từng notebook `.ipynb`.
3. Xem kết quả ảnh được hiển thị và lưu vào các thư mục tương ứng.

---

## 💬 Ghi chú thêm

- Các thư mục output sẽ chứa ảnh kết quả để bạn nộp bài hoặc demo.
- Ảnh gốc nên có trong thư mục `exercise/`, định dạng `.jpg`, `.png`...

---


