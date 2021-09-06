# Bất đồng bộ

## I.Định nghĩa
Bất đồng bộ là các câu lệnh trong chương trình có thể thực hiện cùng lúc, không phải đợi câu lệnh trước.

**Ưu điểm:** Tối ưu thời gian chạy của các câu lệnh. Giúp thực hiện những tác vụ tốn nhiều thời gian mà không ảnh hưởng đến luồng chính của chương trình.\
**Nhược điểm:** Kết quả trả về của các câu lệnh không có thứ tự dẫn đến khó kiểm soát.

## II.Xử lí bất đồng bộ
### 1.Callback
Truyền một hàm dưới dạng tham số vào hàm khác để sử dụng.

**Ưu điểm:** Đơn giản, dễ hiểu.\
**Nhược điểm:** Khi cần thực hiện nhiều câu lệnh bất đồng bộ, sẽ xuất hiện rất nhiều hàm callback lồng nhau hay còn gọi là **callback hell**, khiến code rất khó đọc.

Ví dụ:
```
const printResult = (res) => console.log(res);
const throwError = (err) => console.log(err);

const fetchData = (printResult, throwError) => {
    const url = 'API';
    let request = new XMLHttpRequest();

    request.open('GET', url);
    request.onload = function() {
        if (request.status == '200') {
            printResult(request.response);
        } else {
            throwError(request.statusText); 
        }
    };

    request.onerror = function() {
        throwError('Error fetching data.');
    };

    request.send();
}
```

### 2.Promise:
Tạo ra một object đại diện cho giá trị sẽ được trả về trong tương lai. Promise nhận vào 2 tham số là 2 hàm xử lí cho 2 trường hợp đoạn code trong Promise chạy thành công và thất bại. Promise cũng cung cấp 2 phương thức là then() và catch() để thực hiện công việc tương tự.

**Ưu điểm:** Tránh được callback hell.\
**Nhược điểm:** Không có cơ chế mặc định để truyền biến giữa các Promise.

Ví dụ:
```
const printResult = (res) => console.log(res);
const throwError = (err) => console.log(err);

const fetchData = new Promise (resolve, reject) => {
    const url = 'API';
    let request = new XMLHttpRequest();

    request.open('GET', url);
    request.onload = function() {
        if (request.status == '200') {
            resolve(request.response);
        } else {
            reject(Error(request.statusText)); 
        }
    };

    request.onerror = function() {
        reject(Error('Error fetching data.'));
    };

    request.send();
}

fetchData
.then(res => printResult(res))
.catch(err => throwError(err));
```

### 3.Async/Await:
Hàm async sẽ trả về Promise sau khi thực hiện câu lệnh đồng bộ thay vì định nghĩa ngay từ đầu. Từ khóa async được đặt trước hàm, await được đặt trước các thao tác cần đồng bộ. 

**Ưu điểm:** Cú pháp ngắn gọn, dễ đọc, dễ quản lý. Có cơ chế truyền biến giữa các Promise.\
**Nhược điểm:** Code sẽ trở nên rối hơn trong trường hợp chạy nhiều Promise cùng lúc. Phải sử dụng try/catch để xử lý lỗi trong quá trình chạy hàm async.

Ví dụ:
```
async printData() {
    try{
        const url = 'API';
        const result = await fetch(url);
        const processed = result.json();
        console.log(result);
    }catch (err){
        console.log(err);
    }
}
```