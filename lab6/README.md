
## Nhập Môn Xử Lý Ảnh Số - Lab 6
### Biến đổi hình học ảnh

- Sinh viên thực hiện: Nguyễn Hoàng Long  
MSSV: 2174802010937
- Môn học: Nhập môn Xử lý ảnh số
- Giảng viên: Đỗ Hữu Quân

---

## Giới thiệu  
Bài lab này nhằm mục đích biến đổi hình học ảnh như:  
- Viết được chương trình gán nhãn cho phân vùng ảnh
- Viết được chương trình phân vùng ảnh theo Region
- Viết được chương trình thay đổi ảnh

## Công nghệ sử dụng

- Python: Ngôn ngữ chính
- Pillow (PIL): Đọc, chuyển đổi, và lưu ảnh
- NumPy: Xử lý ảnh dưới dạng mảng số học
- ImageIO: Đọc file ảnh với định dạng hiện đại
- Matplotlib: Hiển thị ảnh trực quan

## Chi tiết các phép biến đổi & công thức

### Gán nhãn ảnh
- Gán nhãn dùng để phân biệt các đối tượng khác nhau trong ảnh. Trong 1 ảnh đã được gán nhãn tất cả các pixel của một đối tượng có giá trị như nhau



### Dò tìm cạnh theo chiều dọc
Code chính:
```python
bmg = abs(data - nd.shift(data, (0,1), order=0))
```
- nd.shift(data, (0,1), order=0)	Dùng scipy.ndimage.shift để dịch ảnh sang phải 1 pixel. Tham số (0,1) nghĩa là dịch 0 pixel theo chiều cao (y), và 1 pixel theo chiều ngang (x). order=0 là nội suy bậc 0 (giữ nguyên giá trị pixel).
- data - shifted_data	Lấy hiệu của ảnh gốc và ảnh đã dịch → cho ra sự thay đổi pixel giữa ảnh và bản dịch.
- abs(...)	Lấy giá trị tuyệt đối để tránh giá trị âm → ra ảnh biểu diễn độ khác biệt tại từng pixel.

Ý nghĩa:
- Nếu pixel bên phải giống pixel hiện tại → kết quả ≈ 0 (màu tối)

- Nếu pixel bên phải khác nhiều → kết quả lớn (màu sáng)

### Dò tìm cạnh với Sobel Filter

Thay vì dùng lệnh shift để dò tìm cạnh, Sobel sử dụng kernel để thực hiện tìm cạnh

Code chính:  
```python
a = nd.sobel(data, axis=0)
b = nd.sobel(data, axis=1)
bmg = abs(a) + abs(b)
```
- nd.sobel(data, axis=0)	Tính gradient theo chiều dọc (trục y) → phát hiện biên ngang. Lưu vào a.
- nd.sobel(data, axis=1)	Tính gradient theo chiều ngang (trục x) → phát hiện biên dọc. Lưu vào b.
- abs(a) + abs(b)	Kết hợp độ lớn biên ở cả 2 hướng → tạo ra bản đồ biên tổng hợp bmg.

Nguyên lý:
- Dùng 2 kernel tích chập Gx(Theo trục x) Gy(Theo trục y)
```text
Sobel theo x (axis=1):               Sobel theo y (axis=0):
[-1 0 1]                             [-1 -2 -1]
[-2 0 2]                             [ 0  0  0]
[-1 0 1]                             [ 1  2  1]
```

Mục tiêu: tìm chỗ nào độ sáng thay đổi đột ngột, tức là có biên (cạnh vật thể).
### Xác định góc đối tượng
Code chính:
```python
def Harris(indata, alpha=0.2):
    x = nd.sobel(indata,0)
    y = nd.sobel(indata,1)
    x1 = x ** 2
    y1 = y ** 2
    xy = abs(x * y)
    x1 = nd.gaussian_filter(x1,3)
    y1 = nd.gaussian_filter(y1,3)
    xy = nd.gaussian_filter(xy,3)

    detC = x1 * y1 - 2 * xy
    trC = x1 + y1
    R = detC - alpha * trC ** 2
    return R
```
Nguyên lý:
- Công thức tính Harris Response:
```text
R=det(C)−α⋅(trace(C))^2

```
- x = nd.sobel(indata, 0)  # Gradient theo chiều dọc (trục y)
- y = nd.sobel(indata, 1)  # Gradient theo chiều ngang (trục x)


### Dò tìm đường tròn trong ảnh

```python
image_gray = rgb2gray(data)
coordinate = corner_harris(image_gray, k = 0.001)
```

- image_gray = rgb2gray(data): 
    - Chuyển ảnh RGB (data) thành ảnh xám (grayscale).

- coordinate = corner_harris(image_gray, k=0.001):
    - Tìm ảnh phản hồi góc 
