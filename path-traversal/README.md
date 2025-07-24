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
URL `loadImage` lấy tham số tên tệp và trả về nội dung của tệp được chỉ định. Các tệp hình ảnh được lưu trữ trên đĩa tại vị trí `/var/www/images/`. Để trả về một hình ảnh, ứng dụng sẽ thêm tên tệp được yêu cầu vào thư mục gốc này và sử dụng API filesystem  để đọc nội dung của tệp. Nói cách khác, ứng dụng đọc từ đường dẫn tệp sau:
```
/var/www/images/218.png
```
Ứng dụng này không triển khai bất kỳ biện pháp phòng thủ nào chống lại các cuộc tấn công vượt qua đường dẫn. Do đó, kẻ tấn công có thể yêu cầu URL sau để truy xuất tệp `/etc/passwd` từ filesystem  của máy chủ:
```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```
Điều này khiến ứng dụng đọc từ đường dẫn tệp sau:
```
/var/www/images/../../../etc/passwd
```
Chuỗi `../` hợp lệ trong một đường dẫn tệp và có nghĩa là tăng một cấp trong cấu trúc thư mục. Ba chuỗi `../../../` liên tiếp được chuyển từ `/var/www/images/` lên thư mục gốc của filesystem , do đó tệp thực sự được đọc là:
```
/etc/passwd
```
Trên các hệ điều hành Unix, đây là một tệp tiêu chuẩn chứa thông tin chi tiết về người dùng đã đăng ký trên máy chủ, nhưng kẻ tấn công có thể truy xuất các tệp tùy ý khác bằng cùng kỹ thuật.

Trên Windows, cả `../` và `..\` đều là các chuỗi duyệt thư mục hợp lệ. Sau đây là một ví dụ về một cuộc tấn công tương đương vào máy chủ chạy Windows:
```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```
# Setup burp suite
* Trong Filter settings của Burp Suite, chọn Show all để thấy toàn bộ ảnh, css ...  
<img src="./img/Screenshot 2025-07-24 at 21.00.49.png">
<img src="./img/Screenshot 2025-07-23 at 22.40.11.png">

# Cách ngăn chặn
Cách hiệu quả nhất để ngăn chặn lỗ hổng duyệt đường dẫn là tránh hoàn toàn việc truyền dữ liệu đầu vào do người dùng cung cấp cho các API filesystem . Nhiều hàm ứng dụng thực hiện điều này có thể được viết lại để cung cấp cùng một hành vi theo cách an toàn hơn.

Nếu bạn không thể tránh việc truyền dữ liệu đầu vào do người dùng cung cấp cho các API filesystem , chúng tôi khuyên bạn nên sử dụng hai lớp phòng thủ để ngăn chặn các cuộc tấn công:

Xác thực dữ liệu đầu vào của người dùng trước khi xử lý. Lý tưởng nhất là so sánh dữ liệu đầu vào của người dùng với danh sách trắng các giá trị được phép. Nếu không thể, hãy xác minh rằng dữ liệu đầu vào chỉ chứa nội dung được phép, chẳng hạn như chỉ các ký tự chữ và số.

Sau khi xác thực dữ liệu đầu vào được cung cấp, hãy thêm dữ liệu đầu vào vào thư mục gốc và sử dụng API filesystem  nền tảng để chuẩn hóa đường dẫn. Xác minh rằng đường dẫn chuẩn hóa bắt đầu bằng thư mục gốc dự kiến.

## Giai đoạn đang phát triển
Lọc ra các kí tự đầu vào nhằm ngăn chặn người dùng sử dụng các dấu phân cách truy cập tới API filesystem .

Xác thực đầu vào của người dùng trước khi xử lý bằng whitelist các giá trị được phép, còn nếu không thể tránh khỏi việc phải sử dụng path truy cập thì phải xác thực được nội dung cho phép truy cập (ví dụ như các kí tự phải hoàn toàn là chữ và số)

Sau khi xác thực đầu vào, ứng dụng sẽ thêm đầu vào vào base directory và sử dụng API hệ thống để chuẩn hóa đường dẫn. Nó sẽ xác minh rằng đường dẫn được chuẩn hóa bắt đầu với base directory.
## Giai đoạn release
Bằng cách sử dụng WAF (Web Application Firewall) chúng ta sẽ có thêm 1 lớp bảo mật nữa từ giai đoạn đang phát triển.

Cấu hình lọc các chuỗi đầu vào từ người dùng (tránh tình trạng chấm chấm xuỵt chấm chấm v.v.v.v) 