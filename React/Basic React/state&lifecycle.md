# State và Lifecycle

## I.State
Giao diện người dùng của ứng dụng luôn có tính động và thay đổi theo thời gian, khái niệm **State** được tạo ra để cho phép component có thể thay đổi theo hành động của người dùng, phản hồi từ mạng, hay bất cứ thứ gì khác mà không vi phạm tính thuần túy của component.\
Ví dụ ta có một component Car với `state` object như sau:
```
class Car extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      brand: "Ford",
      model: "Mustang",
      color: "red",
      year: 1964
    };
  }
  render() {
    return (
      <div>
        <h1>My Car</h1>
      </div>
    );
  }
}
```
State object có thể chứa nhiều thuộc tính tùy theo nhu cầu. Tiếp theo ta sẽ sử dụng `state` trong hiển thị component.
```
class Car extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      brand: "Ford",
      model: "Mustang",
      color: "red",
      year: 1964
    };
  }
  render() {
    return (
      <div>
        <h1>My {this.state.brand}</h1>
        <p>
          It is a {this.state.color}
          {this.state.model}
          from {this.state.year}.
        </p>
      </div>
    );
  }
}
```
Để thay đổi `state`, ta sử dụng phương thức `setState()` của `React.Component`.
```
class Car extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      brand: "Ford",
      model: "Mustang",
      color: "red",
      year: 1964
    };
  }
  changeColor = () => {
    this.setState({color: "blue"});
  }
  render() {
    return (
      <div>
        <h1>My {this.state.brand}</h1>
        <p>
          It is a {this.state.color}
          {this.state.model}
          from {this.state.year}.
        </p>
        <button
          type="button"
          onClick={this.changeColor}
        >Change color</button>
      </div>
    );
  }
}
```
Như vậy thuộc tính `color` của `state` object sẽ thay đổi từ `red` sang `blue` khi ta click button, từ đó component Car hiển thị trên màn hình sẽ được cập nhật lại dựa theo sự thay đổi của `state` object.

## II.Lifecycle
Mỗi component đều có vòng đời của của riêng nó, ta có thể điều khiển vòng đời thông qua 3 giai đoạn **Mounting**, **Updating** và **Unmounting**.

### Mounting
Mounting nghĩa là đưa component đó vào trong DOM.\
React có 4 phương thức được gọi lần lượt trong giai đoạn này:
1. `constructor()`: Phương thức này khởi tạo `state` và các giá trị khác của component. Nó được gọi cùng với tham số `props` và ta sẽ luôn bắt đầu với câu lệnh `super(props)` trước mọi câu lệnh khác để component có thể kế thừa tất cả các phườn thức từ component cha.
2. `getDerivedStateFromProps()`: Phương thức này sẽ cài đặt `state` dựa theo tham số `props`. Nó nhận vào `state` và `props` làm tham số và trả về một `state` object mới.
3. `render()`: Phương thức này là bắt buộc trong component, nó làm nhiệm vụ xuất HTML vào DOM.
4. `componentDidMount()` : Phương thức này sẽ được gọi sau khi component đã được đưa vào DOM.

Ví dụ: 
```
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }

  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }

  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }

  render() {
    return (
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
    );
  }
}
```

### Updating
Component sẽ cập nhật mỗi khi `state` hoặc `props` thay đổi.\
React có 5 phương thức được goi lần lượt trong giai đoạn này:
1. `getDerivedStateFromProps()`: Phương thức này cập nhật `state` theo `props`. Nếu phương thức này sẽ ghi đè nên những thuộc tính `state` được thay đổi nếu giá trị thuộc tính đó có trong `props`.
Ví dụ: 
```
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }
  changeColor = () => {
    this.setState({favoritecolor: "blue"});
  }
  render() {
    return (
      <div>
      <h1>My Favorite Color is {this.state.favoritecolor}</h1>
      <button type="button" onClick={this.changeColor}>Change color</button>
      </div>
    );
  }
}

ReactDOM.render(<Header favcol="yellow"/>, document.getElementById('root'));
```
Mặc dù `color` được chuyển từ `red` sang `blue` do click button, `favoritecolor` vẫn được render là `yellow`.
2. `shouldComponentUpdate()`: Phương thức này trả lại giá trị Boolean xác định component có phải render lại hay không. Giá trị mặc định là **true**.
3. `render()`: Phương thức này render lại component.
4. `getSnapshotBeforeUpdate()`: Phương thức dùng để kiểm tra `state` trước thay đổi. Một khi phương thức này được định nghĩa, `componentDidUpdate()` cũng phải được định nghĩa theo vì giá trị trả về của `getSnapshotBeforeUpdate()` sẽ lập tức được chuyển qua `componentDidUpdate()`.
5. `componentDidUpdate()`: Phương thức này sẽ được gọi sau khi component đã được update.

Ví dụ:
```
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }

  shouldComponentUpdate() {
    return true;
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
  }

  componentDidUpdate() {
    document.getElementById("div2").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }

  changeColor = () => {
    this.setState({favoritecolor: "blue"});
  }

  render() {
    return (
      <div>
        <h1>My Favorite Color is {this.state.favoritecolor}</h1>
        <div id="div1"></div>
        <div id="div2"></div>
      </div>
    );
  }
}

ReactDOM.render(<Header />, document.getElementById('root'));
```

### Unmounting
Unmounting là khi component được xóa khỏi DOM.\
Chỉ có một phương thức được gọi trong giai đoạn này đó là `componentWillUnmount()`, phương thức này thực thi sau khi component đã được xóa.
```
class Container extends React.Component {
  constructor(props) {
    super(props);
    this.state = {show: true};
  }
  delHeader = () => {
    this.setState({show: false});
  }
  render() {
    let myheader;
    if (this.state.show) {
      myheader = <Child />;
    };
    return (
      <div>
      {myheader}
      <button type="button" onClick={this.delHeader}>Delete Header</button>
      </div>
    );
  }
}

class Child extends React.Component {
  componentWillUnmount() {
    alert("The component named Header is about to be unmounted.");
  }
  render() {
    return (
      <h1>Hello World!</h1>
    );
  }
}

ReactDOM.render(<Container />, document.getElementById('root'));
```
Một thông điệp `alert` sẽ được hiển thị khi component Child bị xóa.