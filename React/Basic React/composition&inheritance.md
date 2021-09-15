# Kết hợp và kế thừa
React có một một mô hình kết hợp mạnh mẽ, cho phép các component nhỏ kết hợp với nhau thành một component mới lớn hơn. Tính kết hợp được khuyến khích sử dụng hơn là kế thừa để tái sử dụng các component.

Có 2 trường hợp để áp dụng tính kết hợp này.

## Vật chứa(Containment)
Một số component không biết trước component con của nó là gì, điển hình là các component như `Sidebar` hay `Dialog` - những component đóng vai trò như chiếc hộp chung. Với những trường hợp này tham số `children` được khuyến khích sử dụng để truyền element con đến đầu ra của chúng.
```
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```
Nó giúp cho các component khác truyền những element con một cách linh động hơn bằng cách lồng JSX với nhau:
```
function WelcomeDialog() {vào
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```
Mọi thứ trong thẻ `<FancyBorder>` đều được truyền vào `FancyBorder` component như một `children` và được chuyển đến đầu ra qua  `{props.children}`.

## Cụ thể hóa(Specialization)
Đôi lúc một component có thể là trường hợp đặc biệt của component khác. Ví dụ như ở đây, `WelcomeDialog` là một trường hợp đặc biệt của `Dialog`.
```
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```
Component "cụ thể" `WelcomeDialog` render một component "chung" `Dialog` với cấu hình `props` "cụ thể".

Các component được thiết kế để tách biệt và và thực hiện các chức năng khác nhau, vậy nên tính kế thừa không được khuyến khích trong phát triển ứng dụng React. Thay vào đó ta nên sử dụng tính kết hợp với tính linh động và an toàn cao.