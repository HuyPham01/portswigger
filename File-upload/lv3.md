# Lab: Web shell upload via path 
Một trong những cách các website đối phó với lỗ hổng upload file đó chính là chỉ cho phép server thực thi các tệp được cho phép. Nếu các extension của file không nằm trong cấu hình của server, server có thể trả về các thông báo lỗi hoặc nội dung với `Content-Type: text/plain`

Điều này vô hình chung có thể dẫn đến attacker lợi dụng để soi source code từ server hoặc các tệp riêng tư của server.

Trong bài lab này, tác giả yêu cầu ta thực hiện upload một RCE code lên server và lợi dụng path traversal để có thể leak được secret của chú Carlos
 ## Tip
 Các máy chủ web thường sử dụng trường FileName trong các yêu cầu dữ liệu `multipart/form-data` để xác định tên và vị trí nơi tệp sẽ được lưu.

# Bắt đầu
Tương tự các phần bên trên, trước tiên, ta thử upload đoạn payload của tệp 1.php lên hệ thống:
<img src="./img/image5.png">

Ta có thể thấy, server cho phép ta tải tệp lên dù cho nó là .php (có thể thực thi). Nhưng ở lab này, các file này không thể thực thi trong folder của user (một cách ngăn chặn khai thác lỗ hổng upload file) và server trả về nội dung của file ta upload dưới dạng `Content-Type: text/plain`.

Từ đó, ta có thể sử dụng kỹ thuật `directory traversal` để kiểm tra xem các folder khác (folder con hoặc folder cha) có cho phép thực thi hay không (thường thì server sẽ block user thôi hehe). Ở đây, mình sẽ sửa đổi request POST ban nãy mình upload file `1.php` lên ở chỗ Content-Disposition từ `filename="1.php"` thành `filename="../1.php"`:
<img src="./img/image6.png">

Có vẻ như không thể upload ra một folder khác khi sử dụng cách `../`. Ta thử một cách khác đó chính là sử dụng URL encoding đổi `../` thành `..%2f`:  
Đẫ thành công  
<img src="./img/image7.png">

Như vậy, ta đã upload thành công file `1.php` lên một thư mục khác với folder của user. Dùng Burp Suite để request đến `/files/avatars/..%2f1.php` để xem bí mật của chú Carlos thôi:

