# Lifing State Up
Thông thường, khi một dữ liệu thay đổi, nó sẽ ảnh hưởng đến nhiều component cùng một lúc, vì vậy `state` nên được chia sẻ ở component cha của chúng.

Ví dụ ta có bài toán tính nhiệt độ, nó sẽ tính xem nước có sôi ở nhiệt độ cho trước hay không. Chúng ta sẽ bắt đầu với một component `BoilingVerdict`. Nhiệt độ celsius được truyền vào component này như là một `props` và nó sẽ hiển thị thông báo nhiệt độ đó đủ làm sôi nước hay không:
```
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```
Tiếp theo, ta sẽ tạo ra một component khác là `Calculator`. Nó sẽ có một `<input>` để nhập dữ liệu, và giữ giá trị đó trong `this.state.temperature`.
```
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```
Nếu dừng lại ở đây thì sẽ chẳng có gì để nói, nhưng nếu ta muốn có thêm một input cho nhiệt độ fahrenheit và 2 input phải đồng bộ với nhau, khi ta thay đổi một input, cái còn lại cũng được cập nhật theo, thì vấn đề bắt đầu nảy sinh.\
Ta tách phần input thành component `TemperatureInput` để sử dụng chung cho cả 2 nhiệt độ như sau:
```
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```
Lúc này component `Calculator` sẽ như thế này:
```
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```
Component `TemperatureInput` được truyền vào một trong 2 giá trị `c` hoặc `f` tương ứng với 2 độ đo. Với đoạn code này, mỗi `TemperatureInput` sẽ điều khiển `state` của riêng nó, component theo độ C không thể biết `state` của component theo độ F và ngược lại. Dể chúng có thể đồng bộ về trạng thái, ta phải kéo `state` lên component `Calculator` sau đó `Calculator` sẽ truyền lại trạng thái vào `props` của `TemperatureInput`.
Bây giờ ta sẽ tạo component `Calculator` chưa 2 component `TemperatureInput` với `props` là `c` và `f`.
```
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```
Thuộc tính `value` của input bây giờ sẽ là `this.props.temperature`, dữ liệu thay đổi sẽ được báo lên component cha bằng callback `onTemperatureChange()` được truyền xuống qua `props`.\
Component `Calculator` bây giờ sẽ như sau:
```
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```
Dữ liệu thay đổi mới nhất sẽ được lưu vào `state` dưới dạng object với 2 thuộc tính `scale` và `temperature`. Dữ liệu này sẽ được chuyển đổi thành nhiệt độ còn lại bằng hàm `tryConvert()` được viết từ trước rồi truyền xuống `TemperatureInput`. Cuối cùng, nhiệt độ celcius sẽ được truyền vào `BoilingVerdict` để hiển thị kết quả.

Nếu state cần được sử dụng bởi nhiều component khác nhau, ta nên chuyển chúng nên component cha gần nhất. Thay vì cố gắng đồng bộ hóa bằng ràng buộc 2 chiều, ta nên đồng bộ hóa bằng luồng dữ liệu top-down. Phương pháp này có thể tốn nhiều dòng code hơn ràng buộc 2 chiều, nhưng nó giúp ta cô lập lỗi tốt hơn vì `state` tồn tại ở ít component hơn. Đồng thời, nó cũng giúp truy vết đến component chịu trách nhiệm khi UI xảy ra lỗi.
