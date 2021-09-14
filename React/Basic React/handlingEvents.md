# Xử lí sự kiện

Xử lí sự kiện trong React khá tương đồng với JavaScript với vài khác biệt nhỏ về cú pháp:
- Tên sự kiện trong React là camelCase.
- JSX truyền vào hàm làm event handler, thay vì string.
```
// HTML
<button onclick="activateLasers()">
  Activate Lasers
</button>

// React
<button onClick={activateLasers}>
  Activate Lasers
</button>
```
Một khác biệt nữa là không thể trả về **false** để chặn hành động mặc định trong React, thay vào đó ta sử dụng `preventDefault`.
```
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```
Khi sử dụng React, ta không cần gọi `addEventListener` để thêm listener vào DOM, thay vào đó ta cung cấp listener khi component được render.
```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```
Trong JavaScript, phương thức lớp không được bound nên ta phải bind `this` vào handler trước khi chuyển nó vào component.\
Để truyền thêm tham số vào handler, ta sử dụng 1 trong 2 cách sau:
```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
Trong cả 2 trường hợp, `e` đại diện cho React event sẽ được truyền vào sau id, Đối với arrow function, ta phải truyền nó rõ ràng, còn với `bind`, các tham số liên quan sẽ được truyền vào tự động sau id.