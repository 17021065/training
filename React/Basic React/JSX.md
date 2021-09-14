# JSX
JSX là một cú pháp mở rộng của JavaScript, JSX được khuyến khích sử dụng với React để mô tả UI một cách rõ ràng hơn.
Ví dụ:
```
const element = <h1>Hello, world!</h1>;
```

## Vì sao sử dụng JSX
Trên thực tế, render logic gắn liền với các logic liên quan như các event được xử lí như thế nào, trang thái thay đổi theo thời gian ra sao và dữ liệu được chuẩn bị để hiển thị như thế nào. Thay vì phân chia các thẻ và logic thành các tệp khác nhau, React chia UI thành các đơn vị gọi là **component** chứa cả 2 thứ đó. JSX không bắt buộc trong React tuy nhiên phần lớn lập trình viên đều thấy nó hữu ích như một công cụ hỗ trợ trực quan và cho phép React đưa ra những cảnh báo lỗi hữu dụng hơn.  

## Nhúng biểu thức trong JSX
Các biểu thức có thể được đưa vào trong JSX bằng cách đặt chúng vào trong dấu ngoặc nhọn.
```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Toan',
  lastName: 'Dang Tran'
};

const element = (
<h1> 
    Hello, {formatName(user)}! 
</h1>
);
``` 
Việc xuống dòng ở ví dụ trên nhằm mục đích tăng tính dễ đọc của code. Khi làm như vậy ta nên bao chúng trong dấu ngoặc đơn để tránh bẫy tự *động chèn chấm phẩy* của JavaScript 

## Biểu thức JSX
JSX được tính là biểu thức sau khi biên dịch JavaScript, điều đó có nghĩa là JSX có thể sử dụng trong câu lệnh `if` và vòng lặp `for`, gán cho một biến, nhận làm tham số và trả về từ một hàm.
```
function showAllUsername(userList) {
  if (userList) {
    return userList.map((name, index) => <h1>{++index + ". Fullname: " + formatName(name)}</h1>);
  }else return <h1>There are no user</h1>;
}
```

## Xác định thuộc tính với JSX
Ta có thể dùng dấu ngoặc nhọn để gắn biểu thức JavaScript như một thuộc tính thẻ.
```
const element = <img src={user.avatarUrl}></img>;
```
Dấu ngoặc nhọn không nên sử dụng cùng dấu ngoặc kép khi dùng để xác định thuộc tính thẻ.
```
const element = <input name="inputOf{element}" />; // Wrong

const element = <input name="inputOf"+{element} />; // Wrong

const element = <input name={"inputOf"+element} />; // Correct
```

## Xác định children với JSX
Nếu thẻ không có children, kết thúc thẻ với `/>`
```
const element = <img src={user.avatarUrl} />;
```
Đối với thẻ có children
```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## JSX ngăn chặn tấn công xâm nhập
Mọi giá trị gắn với JSX sẽ được chuyển đổi thành `string` trước khi render, từ đó ngăn cản sự xâm nhập của bất cứ thứ gì không được viết rõ ràng trong ứng dụng.

## JSX Objects
Babel biên dịch JSX thành các lệnh gọi `React.createElement()`, bên dưới là 2 cách viết đúng:
```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
hoặc
```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
``` 
Lệnh này sẽ tạo ra object như sau
```
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
object này được gọi là **React element**, object này chứa thông tin mô tả những thứ ta muốn hiển thị và React sẽ sử dụng nó để xây dựng và cập nhật DOM.