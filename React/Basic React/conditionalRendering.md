# Render có điều kiện

Trong chương trình có rất nhiều component riêng biệt làm những công việc khác nhau, ta có thể chỉ hiển thị một vài trong số đó, phụ thuộc vào trạng thái của ứng dụng. Render có điều kiện hoạt động giống với câu lệnh điều kiện trong JavaScript, sử dụng `if` hoặc toán tử điều kiện để tạo các element phù hợp cho trạng thái hiện tại và để cho React cập nhật UI theo chúng. 

Có một số phương pháp để thực hiện điều này.

## Biến Element
Ta có thể dùng biến để lưu element, ví dụ như component `Greeting` dưới đây.
```
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}

function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  let greeting;

  if (isLoggedIn) greeting = <UserGreeting />;
  else greeting = <GuestGreeting />;

  return greeting;
}
```

## Toán tử &&
Ta có thể nhúng biểu thức vào JSX bằng cách bọc trong dấu ngoặc nhọn, bao gồm cả toán tử `&&`.

Ví dụ:
```
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  
  return (isLoggedIn && <UserGreeting />) || <GuestGreeting />;
}
```

## Toán tử điều kiện
Một cách nữa để render có điều kiện đó là sử dụng toán tử điều kiện `condition ? true : false`. 

Ví dụ:
```
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  
  return isLoggedIn ? <UserGreeting /> : <GuestGreeting />;
}
```

