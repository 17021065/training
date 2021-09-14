# Biểu mẫu

Form element hoạt động khác một chút với các DOM element khác trong React, bởi vì form thường có một số trạng thái nội bộ bên trong nó, chẳng hạn như trạng thái của `<input>`, `<textarea>`, hay `<select>`.
```
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```
Nếu chỉ cần submit form thì sẽ chẳng có vấn đề gì, tuy nhiên để có thể kiểm soát sự thay đổi bên trong form, ta sẽ phải sử dụng đến một kỹ thuật đó là **controlled components**.

Controlled components là component mà giá trị hiện tại của nó có thể được quản lí và điều khiển bởi một component cha.\
Trong HTML, các element `<input>`, `<textarea>`, hay `<select>` sẽ tự quản lý trạng thái của mình. Còn với React, các trạng thái được lưu trữ trong `state` object của component. Ta có thể kết hợp 2 thứ này bằng cách dùng React state như single source of truth (SSOT).
```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
Thuộc tính `value` của `state` được gán vào form, giá trị hiển thị sẽ luôn là `this.state.value`. Sau mỗi lần tương tác với bàn phím, `handleChange` sẽ cập nhật lại `state`. Với controlled component, ta có thể truyền giá trị hiện tại của input đến các element khác hoặc reset nó thông qua một handler.

Với nhiều input element, ta thêm thuộc tính `name` cho từng element và điều khiển chúng thông qua `event.target.name`.
```
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
Ngoài ra, kể từ khi `setState()` tự động gộp các phần của `state` thành `state` hiện tại, ta chỉ cần gọi hàm `setState()` đối với những phần của `state` bị thay đổi.

Việc sử dụng controlled component đôi khi sẽ gây khó khăn trong việc chuyển đổi codebase cũ sang React hoặc tích hợp React vào ứng dụng khác vì phải viết lại các hàm xử lí data và kết nối tất cả input state vào React state. Trong những trường hợp đó, ta có thể xử dụng một biện pháp thay thế đó là uncontrolled component - component được điều khiển bởi DOM thay vì React state.
