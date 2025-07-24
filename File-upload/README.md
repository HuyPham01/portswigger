# Lỗ hổng file upload là gì?
Lỗ hổng file upload xảy ra khi máy chủ web cho phép người dùng file upload hệ thống tệp của mình mà không xác thực đầy đủ các thông tin như tên, loại, nội dung hoặc kích thước. Việc không thực thi đúng các hạn chế này có thể đồng nghĩa với việc ngay cả một chức năng tải hình ảnh cơ bản cũng có thể được sử dụng để tải lên các tệp tùy ý và tiềm ẩn nguy hiểm. Điều này thậm chí có thể bao gồm các tệp tập lệnh phía máy chủ cho phép thực thi mã từ xa.

Trong một số trường hợp, hành động file upload tự nó đã đủ để gây ra thiệt hại. Các cuộc tấn công khác có thể bao gồm một yêu cầu HTTP tiếp theo cho tệp, thường là để kích hoạt việc thực thi tệp bởi máy chủ.
# Tác động của lỗ hổng file upload là gì?
Tác động của lỗ hổng file upload thường phụ thuộc vào hai yếu tố chính:

Trang web không xác thực đúng khía cạnh nào của tệp, có thể là kích thước, loại, nội dung, v.v.

Những hạn chế nào được áp dụng cho tệp sau khi đã tải lên thành công?

Trong trường hợp xấu nhất, loại tệp không được xác thực đúng cách và cấu hình máy chủ cho phép một số loại tệp nhất định (chẳng hạn như `.php` và `.jsp`) được thực thi dưới dạng mã. Trong trường hợp này, kẻ tấn công có thể tải lên một tệp mã phía máy chủ hoạt động như một web shell, trên thực tế cấp cho chúng toàn quyền kiểm soát máy chủ.