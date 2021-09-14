# Rendering Element
Element là cấu trúc khối nhỏ nhất của ứng dụng React. Element mô tả thứ ta muốn hiển thị trên màn hình.
```
const element = <h1>Hello, world</h1>;
```
Không giống DOM element, React element là một object thuần túy và đơn giản để khởi tạo. React DOM sẽ lo việc cập nhật DOM sao cho khớp với các React element đã tạo.\
Các ứng dụng React thường chỉ có một thẻ `<div>` trong tệp HTML.
```
<div id="root"></div>
```
Thẻ này được gọi là root DOM vì mọi thứ trong nó sẽ được quản lý bởi React DOM. Các ứng dụng React thường chỉ có một root duy nhất. Nếu ta cần tích hợp React vào ứng dụng có sẵn, ta có thể tạo nhiều root độc lập theo nhu cầu.\ 
Ứng dụng React sẽ được xây dựng bằng cách render các element vào trong root DOM bằng lệnh `ReactDOM.render()`
```
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
Các React element là bất biến, vì thế để có thể thay đổi UI, ta phải tạo ra element mới và đưa chúng vào trong DOM. Ví dụ như app đồng hồ dưới đây.
```
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
`ReactDOM.render()` sẽ được gọi mỗi giây bởi hàm callback của `setInterval()`, từ đó có thể làm mới thời gian mỗi giây.
