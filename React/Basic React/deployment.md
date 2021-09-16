# Triển khai

Trước tiên ta chạy lệnh `npm run build`, lệnh này tạo một thư mục `build`. Bên trong thư mục `build/static` sẽ là các tệp JavaScript, CSS. Mỗi tên tệp trong `build/static` sẽ chứa một hash độc nhất của nội dung tệp. Sau khi `build` ta sẽ đẩy thư mục đó lên một HTML server để phục vụ người dùng.

## Server tĩnh
Với những môi trường sử dụng Nodejs như React, cách đơn giản nhất là cài đặt package `serve` và để nó xử lí phần còn lại.
```
npm install -g serve
serve -s build
```
Câu lệnh thứ 2 sẽ serve trang web của chúng ta ở cổng 5000,  để cấu hình cổng khác ta dùng flag `-l` hoặc `--listen`.
```
serve -s build -l 4000
```
Chạy lệnh sau để xem tất cả tùy chọn của lệnh serve.
```
serve -h
```

## Server động
Với server động ta có thể sử dụng các server có sẵn để triển khai. Có rất nhiều http-server lớn có thể hỗ trợ chúng ta như Firebase hay Netlify.

### Firebase
Đầu tiên ta tạo dự án trên console của Firebase, sau đó cài package của Firebase
```
npm i -g firebase
```
Sau đó khởi tạo Firebase project trong React project của chúng ta
```
firebase init
```
Sau đó thực hiện các cấu hình cho Firebase project, cung cấp đường dẫn đến thư mục `build` đã có cho Firebase. Cuối cùng là thực hiện triển khai bằng câu lệnh:
```
firebase deploy 
```
Firebase sẽ triển khai ứng dụng của chúng ta trên remote server và sinh ra một URL để chạy ứng dụng trên đó.

### Netlify
Đầu tiên ta kết nối Git repository của ứng dụng vào cloud service của Netlify. Sau đó cấu hình deploy setting, đặt build command là `npm run build` và thư mục sẽ xuất bản là `/build`. Sau đó nhấn chọn Deploy site để triển khai lên remote server. Sau khi triển khai thành công, chúng ta sẽ nhận được một URL để truy cập vào ứng dụng.