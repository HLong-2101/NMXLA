
## Nhập Môn Xử Lý Ảnh Số - Lab 5
### Biến đổi hình học ảnh

- Sinh viên thực hiện: Nguyễn Hoàng Long  
MSSV: 2174802010937
- Môn học: Nhập môn Xử lý ảnh số
- Giảng viên: Đỗ Hữu Quân

---

## Giới thiệu  
Bài lab này nhằm mục đích biến đổi hình học ảnh như:  
- Viết được chương trình phân vùng ảnh theo histogram
- Viết được chương trình phân vùng ảnh theo Region
- Viết được chương trình thay đổi ảnh

## Công nghệ sử dụng

- Python: Ngôn ngữ chính
- Pillow (PIL): Đọc, chuyển đổi, và lưu ảnh
- NumPy: Xử lý ảnh dưới dạng mảng số học
- ImageIO: Đọc file ảnh với định dạng hiện đại
- Matplotlib: Hiển thị ảnh trực quan

## Chi tiết các phép biến đổi & công thức

### 1. Phân vùng theo histogram 


- Một ngưỡng xác định dựa theo histogram của ảnh. Mỗi pixel trong ảnh được so sánh với ngưỡng, nếu giá trị pixel nhỏ hơn ngưỡng, thì pixel trong phân vùng được gán giá trị 0. Ngược lại, gán giá trị 1

#### Phương pháp Otsu
Code chính:
```python
thres = threshold_otsu(a)
b = a > thres
```
- threshold_otsu(a): Tự động tìm ngưỡng tối ưu để phân tách ảnh thành 2 vùng:

    - vùng tối (background)

    - vùng sáng (foreground)
- Tạo ảnh nhị phân (binary image) theo ngưỡng vừa tìm được:

    - Pixel nào lớn hơn ngưỡng (thres) → True (255/1)

    - Pixel nào nhỏ hơn hoặc bằng → False (0)

- Kết quả b là một mảng True/False hoặc 1/0 — ảnh đen trắng đơn giản.



### 2. Adaptive Thresholding

Mục đích:
- Cải tiến phân vùng chính xác hơn Otsu. Chia ảnh thành nhiều ảnh nhỏ và tính threshold cho từng ảnh nhỏ

Code chính:  
```python
b = threshold_local(a,39,offset=10)
b = Image.fromarray(b)
```
- a: ảnh đầu vào dạng grayscale (2D, không phải RGB).

- 39: là kích thước khối (block size) = 39x39 pixel.
→ Mỗi pixel sẽ được so sánh với ngưỡng tính từ vùng xung quanh nó (thay vì toàn ảnh như Otsu).

- offset=10: là giá trị bị trừ đi khỏi ngưỡng cục bộ → giúp điều chỉnh ngưỡng nghiêm ngặt hơn.

### 3. Phân vùng theo Region

Mục đích:
- Một Region là một nhóm các pixel có cùng thuộc tính



### 4. Biến đổi dối tượng trong ảnh bằng binary_dilation
Mục đích:
- Làm cho vùng trắng trong ảnh mở rộng ra

Code chính:  
```python
b = nd.binary_dilation(data,iterations=50)
```
Nguyên lý:  
- binary_dilation: phép giãn nở, mở rộng các vùng trắng ra ngoài theo hướng lân cận
- iterations=50: áp dụng giãn nở 50 lần liên tiếp

### 5. Sử dụng binary_opening
Mục đích của opening:

- Xóa nhiễu nhỏ.

- Loại bỏ các vùng trắng bé, lẻ.

- Làm "mịn" biên của vật thể trắng.


Code chính:  
```python
b = nd.binary_opening(data, structure=a, iterations=25)
```
Nguyên lý:  
- binary_opening: Giãn nỡ ảnh sau khi co rút nhưng không phục hồi các chi tiết đã mất
- iterations=25: áp dụng giãn nở 50 lần liên tiếp