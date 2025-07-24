Common obstacles to exploiting path traversal vulnerabilities

Many applications that place user input into file paths implement defenses against path traversal attacks. These can often be bypassed.

If an application strips or blocks directory traversal sequences from the user-supplied filename, it might be possible to bypass the defense using a variety of techniques.

You might be able to use an absolute path from the filesystem root, such as `filename=/etc/passwd`, to directly reference a file without using any traversal sequences.

# Lab: File path traversal, traversal sequences blocked with absolute path bypass
Bài thực hành này chứa một lỗ hổng duyệt đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Ứng dụng chặn các chuỗi duyệt nhưng coi tên tệp được cung cấp là tương đối với thư mục làm việc mặc định.

Để giải quyết bài thực hành, hãy truy xuất nội dung của tệp /etc/passwd.
# Kết quả
Bài lab đường dẫn tuyệt đối thì chỉ cần nhập `/etc/passwd` đã thành công.
<img src='./img/Screenshot 2025-07-24 at 21.31.09.png'>