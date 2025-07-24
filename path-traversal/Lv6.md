# Common obstacles to exploiting path traversal vulnerabilities - null byte
Ứng dụng có thể yêu cầu tên tệp do người dùng cung cấp phải kết thúc bằng phần mở rộng tệp mong muốn, chẳng hạn như `.png`. Trong trường hợp này, có thể sử dụng `null byte` để kết thúc đường dẫn tệp trước phần mở rộng bắt buộc. Ví dụ: `filename=../../../etc/passwd%00.png`.
# Lab: File path traversal, validation of file extension with null byte bypass
Bài thực hành này chứa lỗ hổng duyệt đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Ứng dụng xác thực tên tệp được cung cấp kết thúc bằng phần mở rộng tệp mong muốn.

Để giải bài thực hành, hãy truy xuất nội dung của tệp /etc/passwd.

# kết quả
Chả nhẽ chúng ta phải nhập vào `/etc/passwd.png` (cũng chã thành công :v) 

Như đã đề cập, chúng ta có thể thêm vào 1 byte rỗng trước đuôi file để bypass qua điều kiện này.

`/etc/passwd%00.jpg` --> 400

`../../..//etc/passwd%00.jpg` --> 200