# Components và Props
Về mặt khái niệm, **Component** giống như hàm trong JavaScript, nó nhận vào input bất kỳ -**Props** và trả về một React element mô tả những thứ ta muốn đưa lên màn hình.\ 
Components cho phép ta chia UI thành nhiều mảnh độc lập, có khả năng tái sử dụng. Có 2 loại component là **Function Components** và **Class Components**.
```
// Function Components
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Class Components
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
Một component có thể tham chiếu đến một component khác ở output, điều này cho phép ta trừu tượng hóa các component.
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```
Nó cũng cho phép ta có thể tách các component phức tạp thành các component đơn giản hơn như ví dụ dưới đây.
```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
Đầu tiên ta sẽ tách component `Avatar` ra,
```
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
Bây giờ đoạn code ban đầu sẽ trở thành,
```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
Tiếp đến, ta sẽ tách component `UserInfo`,
```
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```
Component `Comment` cuối cùng sẽ ngắn gọn như thế này.
```
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
Trong React có môt quy tắc bắt buộc đó là các component phải hoạt động như các hàm thuần túy, tức là không được thay đổi **props** được truyền vào nó, bằng không, nó sẽ không được gọi là một component.
```
// Pure
function sum(a, b) {
  return a + b;
}

// Impure
function withdraw(account, amount) {
  account.total -= amount;
}
```