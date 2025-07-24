# Lab: File path traversal, simple case
Bài thực hành này chứa lỗ hổng bảo mật đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Để giải quyết bài thực hành, hãy truy xuất nội dung của tệp `/etc/passwd.`

# Kết quả
Playload 1
```
../../../../etc/passwd
```
Playload 2
```
/etc/passwd
```
URL: `https://0a48005a04e7c3848140bb7500f30011.web-security-academy.net/`.  
Sử dụng burp suite để thấy các gói tin. Lúc này thấy 1 số URL load ảnh `GET /image?filename=31.jpg`
<img src="./img/Screenshot 2025-07-24 at 21.03.45.png">
Sử dụng `repeater` để thay đổi file.  
Playload 1
```
GET /image?filename=/etc/passwd
```
<img src='./img/Screenshot 2025-07-24 at 21.09.35.png'>
Respone trả về `400` và báo 'No such file'.

Playload hiện tại là 1 đường dẫn tuyệt đối. Như ta biết ảnh thường được lưu tại `/var/www/images/` với playload trên có thể hiểu là `/var/www/images/etc/passwd` và nó không tồn tại thật.

Playload 2
```
GET /image?filename=../../../etc/passwd
```
Với playload này thì đã exploit thành công. 

Vì sao: Trong linux với mỗi một `../` ta đã ra ngoài 1 folder vậy ở đây đang có 3 `../../../` thì đã thoát ra khỏi `/var/www/images/` nên đọc được file ở `/etc/passwd`.
<img src="./img/Screenshot 2025-07-24 at 21.13.23.png">
# Tip
Có thể `../` nhiều hơn 3 lần vì nhiều đến đâu rồi cũng tới `/root`