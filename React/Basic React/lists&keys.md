# Lists & Keys

## Lists

### Render multiple component
Ta có thể xây dựng một bộ sưu tập element bằng cách sử dụng vòng lặp trong dấu ngoặc nhọn.

Ví dụ:
```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);

ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

### Basic List Component
Để tạo ra một list component cơ bản ta sẽ chuyển đổi ví dụ trên thành component với input là mảng số tự nhiên từ 1 đến 5 và đầu ra là list component.
```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) => <li>{number}</li>);

  return (<ul>{listItems}</ul>);
}

const numbers = [1, 2, 3, 4, 5];

ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
Đối với list component, ta phải có các key riêng biệt với mỗi đơn vị trong danh sách.


## Keys
Keys giúp ta có thể xác định được component nào trong danh sách bị thay đổi, từ đó đưa ra hành động phù hợp. Keys nên được đặt trong component để đảm bảo tính ổn định.
```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}> {number} </li>
  );

  return (<ul>{listItems}</ul>);
}

const numbers = [1, 2, 3, 4, 5];

ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

### Tách component có key
Keys chỉ có ý nghĩa khi đi cùng với mảng component, vậy nên khi tách component có key, ta phải đặt key ở list component thay vì các element bên trong nó. Như vậy React có thể tối ưu hóa thay đổi trong DOM bằng cách chỉ thay đổi các item có key tương ứng.
```
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()} value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```
Các key cho item trong danh sách cần phải là độc nhất giữa các anh em của nó, tuy nhiên không cần thiết phải độc nhất trong toàn cục.