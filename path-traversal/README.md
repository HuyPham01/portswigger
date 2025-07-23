## LAB
[Path traversal lab](https://portswigger.net/web-security/learning-paths/path-traversal/what-is-path-traversal/file-path-traversal/what-is-path-traversal)

Path traversal là gì?
Path traversal còn được gọi là directory traversal. Các lỗ hổng này cho phép kẻ tấn công đọc các tệp tùy ý trên máy chủ đang chạy ứng dụng. Các tệp này có thể bao gồm:

Code và dữ liệu ứng dụng.
Thông tin đăng nhập cho các hệ thống back-end.
Các tệp hệ điều hành nhạy cảm.
Trong một số trường hợp, kẻ tấn công có thể ghi vào các tệp tùy ý trên máy chủ, cho phép chúng sửa đổi dữ liệu hoặc hành vi của ứng dụng, và cuối cùng là chiếm toàn quyền kiểm soát máy chủ.

## Đọc các tệp tùy ý thông qua duyệt đường dẫn
Hãy tưởng tượng một ứng dụng mua sắm hiển thị hình ảnh các mặt hàng đang bán. Ứng dụng này có thể tải hình ảnh bằng mã HTML sau:
```
<img src="/loadImage?filename=218.png">
```
URL `loadImage` lấy tham số tên tệp và trả về nội dung của tệp được chỉ định. Các tệp hình ảnh được lưu trữ trên đĩa tại vị trí `/var/www/images/`. Để trả về một hình ảnh, ứng dụng sẽ thêm tên tệp được yêu cầu vào thư mục gốc này và sử dụng API hệ thống tệp để đọc nội dung của tệp. Nói cách khác, ứng dụng đọc từ đường dẫn tệp sau:
```
/var/www/images/218.png
```
Ứng dụng này không triển khai bất kỳ biện pháp phòng thủ nào chống lại các cuộc tấn công vượt qua đường dẫn. Do đó, kẻ tấn công có thể yêu cầu URL sau để truy xuất tệp `/etc/passwd` từ hệ thống tệp của máy chủ:
```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```
Điều này khiến ứng dụng đọc từ đường dẫn tệp sau:
```
/var/www/images/../../../etc/passwd
```
Chuỗi `../` hợp lệ trong một đường dẫn tệp và có nghĩa là tăng một cấp trong cấu trúc thư mục. Ba chuỗi `../../../` liên tiếp được chuyển từ `/var/www/images/` lên thư mục gốc của hệ thống tệp, do đó tệp thực sự được đọc là:
```
/etc/passwd
```
Trên các hệ điều hành Unix, đây là một tệp tiêu chuẩn chứa thông tin chi tiết về người dùng đã đăng ký trên máy chủ, nhưng kẻ tấn công có thể truy xuất các tệp tùy ý khác bằng cùng kỹ thuật.

Trên Windows, cả `../` và `..\` đều là các chuỗi duyệt thư mục hợp lệ. Sau đây là một ví dụ về một cuộc tấn công tương đương vào máy chủ chạy Windows:
```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```