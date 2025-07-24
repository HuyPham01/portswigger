# Common obstacles to exploiting path traversal vulnerabilities - Continued
# Những trở ngại phổ biến khi khai thác lỗ hổng traversal
Bạn có thể sử dụng các chuỗi duyệt lồng nhau, chẳng hạn như `....//` hoặc `....\/`. 
Các chuỗi này sẽ trở lại thành chuỗi duyệt đơn giản khi chuỗi bên trong bị loại bỏ.

# Lab: File path traversal, traversal sequences stripped non-recursively
Bài thực hành này chứa một lỗ hổng duyệt đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Ứng dụng sẽ xóa các chuỗi duyệt đường dẫn khỏi tên tệp do người dùng cung cấp trước khi sử dụng.

Để giải bài thực hành, hãy truy xuất nội dung của tệp `/etc/passwd`.
## Kết quả
Khi nhập `../../../etc/passwd` --> lỗi 400 (có thể đã bị input validation `../` có thể bị xóa hay gì đó ...)

Nếu có input validation `../` thì nhập `....//` nếu đúng như dự đón thì từ `....//` --> `../`. 

<img src="./img/Screenshot 2025-07-24 at 21.37.47.png">