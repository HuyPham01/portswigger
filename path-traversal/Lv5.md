# Common obstacles to exploiting path traversal vulnerabilities
Ứng dụng có thể yêu cầu filename do người dùng cung cấp bắt đầu bằng thư mục gốc dự kiến, chẳng hạn như `/var/www/images`. Trong trường hợp này, có thể bao gồm thư mục gốc cần thiết theo sau là các chuỗi duyệt phù hợp. Ví dụ: `filename=/var/www/images/../../../etc/passwd`.
# Lab: File path traversal, validation of start of path
Bài thực hành này chứa lỗ hổng duyệt đường dẫn trong việc hiển thị hình ảnh sản phẩm.

Ứng dụng truyền toàn bộ đường dẫn tệp qua tham số yêu cầu và xác thực rằng đường dẫn được cung cấp bắt đầu bằng thư mục mong muốn.

Để giải quyết bài thực hành, hãy truy xuất nội dung của tệp `/etc/passwd`.
# Kết quả
Vì thấy nó là full path nên chỉ cần `filename=/var/www/images/../../../etc/passwd` --> 200.