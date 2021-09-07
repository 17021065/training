# Lập trình hàm

## I.Định nghĩa
Lập trình hàm là một dạng mô hình lập trình mà chương trình sẽ được xây dựng dựa trên việc biên soạn và ứng dụng hàm.

Lập trình hàm có 2 đặc điểm chính:
- **Tính thuần túy:** Tất cả các hàm đều là hàm thuần túy, không tác động đến bất cứ thứ gì bên ngoài nó, từ đó giảm thiểu lỗi có thể xảy ra.
- **Tính bất biến:** Tất cả các biến và hàm được khai báo đều là bất biến, không được thay đổi. Sự thay đổi của dữ liệu trở nên rõ ràng hơn, ta có thể truy cập giá trị trong quá khứ của chúng bất cứ lúc nào.

### Ưu điểm
- **Tăng tính tái sử dụng:** Từ việc chia nhỏ luồng xử lí thành nhiều hàm với chức năng nhất định, ta có thể tái sử dụng nhưng hàm đó ở những luồng khác trong chương trình.
- **Thuận tiện cho phát hiện và xử lí lỗi:** Việc phân mảnh luồng xử lí cũng giúp các nhà phát triển xác định lỗi và khắc phục chúng dễ dàng hơn do các hàm trong chương trình đều được phát triển một cách độc lập.
- **Bảo mật dữ liệu:** Chương trình áp dụng lập trình hàm được vận hành bằng cách phối hợp các hàm với nhau, không lưu trữ biến, không lưu trữ trạng thái, các biến trong hàm được giải phóng khỏi bộ nhớ sau khi hàm thực thi, từ đó tránh việc các biến quan trọng bị truy cập và thay đổi trái phép.
- **Thể hiện rõ thuật toán:** Điểm khác biệt giữa lập trình hàm và lập trình thủ tục là thay vì thực hiện tuần tự các bước với biến để lưu trạng thái, thì lập trình hàm chú trọng vào việc thực thi luồng thông qua việc kết hợp các hàm bậc cao(HOF). Các hàm sẽ được truyền như tham số hoặc kết quả trả về trong hàm bậc cao, từ đó thuật toán trong lập trình hàm được diễn tả một cách ngắn gọn và rõ ràng.

### Nhược điểm:
- **Khó tiếp cận:** Để xây dựng được các hàm thuần túy tốt, lập trinh viên cần đầu tư rất nhiều thời gian và công sức để học tập.
- **Tiêu tốn phần cứng:** Cấu trúc hàm bậc cao hoạt động giống như thuật toán đệ quy, gây ra gánh nặng rất lớn cho phần cứng từ đó làm giảm hiệu năng của chương trình. 

## II.Khái niệm liên quan
### 1.First-Class Function & High-Order Function
**First-Class Function**(FCF) là hàm được ngôn ngữ lập trình coi như một biến. Chúng có thể dùng làm tham số, trả về từ một hàm khác, thay đổi hoặc được gán cho một biến. Không có bất kì giới hạn nào cho việc sử dụng FCF, nó có thể xuất hiện ở bất cứ nói nào trong chương trình, vì thế nó đóng vai trò quan trọng trong lập trình hàm.\
**High-Order Function**(HOF) là hàm có thể nhận vào một hàm làm tham số cũng như trả về kết quả là một hàm. HOF cho phép phân mảnh chương trình hoặc currying, giúp chương trình trở nên ngắn gọn, dễ hiểu.

### 2.Đệ quy(Recursion)
HOF sẽ được stack lại cho đến khi các hàm con của nó được thực thi, chính vì vậy đệ quy được sử dụng nhiều trong lập trình hàm. Đệ quy là thuật toán rất có hại cho bộ nhớ nên khi lập trình chúng ta cần sử dụng đệ quy đuôi, tức là lệnh đệ quy sẽ là lệnh cuối cùng của hàm.

### 3.Side effect
Là tất cả các tác động không chủ đích trong chương trình, nó có thể ở bất kì đâu từ thao tác trạng thái, tương tác với I/O đến database, log system, APIs,... Bản thân side effect không đáng lo ngại nhưng nếu chúng bị ẩn đi hoặc không thể nhìn thấy 1 cách rõ ràng, chúng sẽ trở nên vô cùng nguy hiểm.

### 4.Hàm thuần túy(Purely Function)
Hàm thuần túy là hàm không có side effect. Điều này có nghĩa là hàm thuần túy sẽ có những tính chất hữu hiệu như:
- Khi kết quả của hàm không được sử dụng, nó sẽ bị giải phóng khỏi bộ nhớ mà không ảnh hưởng tới các hàm khác.
- Khi một hàm thuần túy được gọi với một tham số thuần túy, kết quả trả về sẽ là hằng số. Điều này có thể ứng dụng trong tối ưu hóa caching. Ví dụ: Gọi lại hàm với cùng một tham số và nhận lại cùng một kết quả.
- Khi 2 hàm thuần túy không có dữ liệu phụ thuộc, thức tự của chúng có thể được đảo lại hoặc có thể chạy song song mà không ảnh hưởng đến nhau.
- Khi ngôn ngữ không có bất kì side effect nào, trình biên dịch có thể tự do sắp xếp và kết hợp các biểu thức, từ đó có thể tối ưu hóa chương trình tốt hơn.

### 4.Shared State
Là việc một biến có thể được truy cập ở nhiều phạm vi riêng biệt. Đây là điều phải tránh trong lập trình hàm vì nó làm mất tính thuần túy của hàm. Nếu tồn tại shared state thì khi chương trình tranh đổi một biến nào đó, side effect sẽ được sinh ra, điều nay vi phạm quy tắc lập trình hàm và sẽ dẫn tới nhiều hậu quả khó lường.

### 5.Kết hợp hàm(Function Composition)
Kết hợp hàm là việc kết hợp 2 hoặc nhiều hàm thành một hàm mới. Kết quả của một hàm sẽ lập tức được truyền qua một hàm khác và tiếp tục đến hàm cuối cùng và đưa ra một kết quả cuối cùng. Số lượng hàm có thể kết nối vào chuỗi là không giới hạn.

## III.FP và OOP
| Yếu tố               | Functional Programming                     | Object Oriented Programming              | 
| :------------------- | :----------------------------------------- | :--------------------------------------- | 
| Định nghĩa           | Nhấn mạnh việc ước lượng của hàm           | Dựa trên khái niệm về đối tượng          |
| Dữ liệu              | Bất biến                                   | Khả biến                                 |
| Mô hình              | Lập trình khai báo(Declaretive)            | Lập trình mệnh lệnh(Imperative)          |
| Lập trình song song  | Hỗ trợ                                     | Không hỗ trợ                             | 
| Thực thi             | Theo bất kì thứ tự nào                     | Theo thứ tự nhất định                    |
| Lệnh lặp             | Sử dụng đệ quy                             | Sử dụng vòng lặp                         |
| Đơn vị thao tác      | Biến và Hàm                                | Đối tượng và Phương thức                 |
| Sử dụng              | Khi có ít đối tượng nhưng nhiều thao tác   | Khi có nhiều đối tượng như ít thao tác   |

## IV.Quy tắc và xu hướng
### No for/while
Bỏ hoàn toàn việc sử dụng `for/while`, thay vào đó sử dụng các iterator HOF để tăng tính logic cũng như tính sử dụng lại của các lệnh lặp.

Ví dụ ta có trang HTML như sau:
```
<style>
.box {
  width: 100px;
  height: 100px;
  float: left;
  margin: 10px;
  background-color: red;
}
.hide {
  display: none;
}
</style>
<div class="box hide"></div>
<div class="box hide"></div>
<div class="box hide"></div>
<div class="box hide"></div>
```
thay vì
```
const els = document.querySelectorAll('.box');
for (let i = 0; i < els.length; i++) {
  let el = els[i];
  el.classList.remove('hide');
}
```
thì
```
const map = fn => arr => arr.map(fn);
const getElements = selector => Array.from(document.querySelectorAll(selector));
const removeClass = className => el => el.classList.remove(className);

const removeClassHide = removeClass('hide');
const getListBoxElement = getElements('.box');
map(removeClassHide)(getListBoxElement);
```

### No if/else
Bỏ hoàn toàn việc sử dụng `if/esle`,
```
let title = 'Mr.';
if (person.gender === 'female') {
  if (!person.gotMarried) {
    title = 'Ms.';
  } else {
    title = 'Mrs.';
  }
}
```
thay vào đó là **Tenary**
```
const title = person.gender === 'female' ? (!person.gotMarried ? 'Ms.' : 'Mrs.') : 'Mr.';
```
hoặc **Logical operators**
```
const title = (person.gender === 'female' || 'Mr') && ((!person.gotMarried || 'Ms.') && 'Mrs.');
```

### No new/this
Bỏ hoàn toàn việc sử dụng `new/this`, thay vì
```
function Dog(name) {
  this.name = name;
  this.say = function() {
    console.log('woof-woof, my name is ' + this.name);
  }
}

var rocky = new Dog('Rocky');
rocky.say();

var molly = new Dog('Molly');
molly.say();
```
thì
```
const sayName = (state) => {
  return Object.assign(
    state,
    {
      say: () => {
        console.log(`woof-woof, my name is ${state.name}`);
      },
    }
  );
};

const createDog = (name) => {
  let state = {
    name,
  };
  return Object.assign(state, sayName(state));
};

const rocky = createDog('Rocky');
rocky.say();
const molly = createDog('Molly');
molly.say();
```