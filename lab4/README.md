
## Nhập Môn Xử Lý Ảnh Số - Lab 4
### Biến đổi hình học ảnh

- Sinh viên thực hiện: Nguyễn Hoàng Long  
MSSV: 2174802010937
- Môn học: Nhập môn Xử lý ảnh số
- Giảng viên: Đỗ Hữu Quân

---

## Giới thiệu  
Bài lab này nhằm mục đích biến đổi hình học ảnh như:  
- Xoay chiều ảnh
- Tịnh tiến ảnh
- Thay đổi kích thước ảnh
- Chọn đối tượng ảnh

## Công nghệ sử dụng

- Python: Ngôn ngữ chính
- Pillow (PIL): Đọc, chuyển đổi, và lưu ảnh
- NumPy: Xử lý ảnh dưới dạng mảng số học
- ImageIO: Đọc file ảnh với định dạng hiện đại
- Matplotlib: Hiển thị ảnh trực quan

## Chi tiết các phép biến đổi & công thức

### 1. Chọn đối tượng ảnh

Mục đích:  
- Chọn một vùng ảnh từ một ảnh gốc.

Code chính:
```python
data = iio.imread('fruit.jpg')
bmg = data[800:1200,570:980]
```

Nguyên lý:  
- Lấy ảnh từ tọa độ [y1:y2,x1:x2]  
- Tương úng lấy ảnh từ độ cao 800 đến 1200 và 570 đến 980 theo chiều ngang

### 2. Tịnh tiến ảnh

Mục đích:
- Tịnh tiến ảnh là phép biến đổi dịch chuyển toàn bộ nội dung ảnh sang vị trí mới theo hướng ngang (trục x), dọc (trục y), hoặc cả hai.  

Code chính:  
```python
bdata = nd.shift(data, (100,25))
```
Nguyên lý:  
- Dịch chuyển ảnh data theo chiều dọc 100 pixel và 25 pixel theo chiều ngang

### 3. Thay đổi kích thước ảnh

Mục đích:
- Là tăng hoặc giảm không gian của ảnh

Code chính:  
```python
data2 = nd.zoom(data,(2,3,1))
```
Nguyên lý:  
- Cách phóng to ảnh:  nd.zoom(ảnh,(y,x,1))
    - Phóng to theo chiều cao x2
    - Phóng to theo chiều ngang x3
    - 1: giữ nguyên số kênh màu

### 4. Xoay ảnh

Mục đích:
- Xoay một ảnh với một góc độ tương ứng

Code chính:  
```python
d1 = nd.rotate(data,20)
```
Nguyên lý:  
- Dùng hàm rotate(image,degree) để xoay một ảnh với:
    - image: Ảnh cần xoay
    - degree: góc cần xoay(độ)

### 5. Dilation và Erosion

Mục đích:
- Để loại bỏ những pixel nhiễu 
- Dilation thay thế pixel tọa độ (i,j) bằng giá trị lớn nhất của những pixel liền kề
- Erosion thay thế pixel tọa độ (i,j) bằng giá trị nhỏ nhất của những pixel liền kề

Code chính:  
```python
d2 = nd.binary_dilation(data,iterations=3)
```
Nguyên lý:  
- binary_dilation: sẽ làm mở rộng (dilate) các vùng trắng (True/1)
- iterations=3: là phép mở rộng sẽ lặp lại 3 lần liên tiếp, mở rộng rộng hơn mỗi lần.