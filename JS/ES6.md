# ES6

## I.Let & Const
`let` và `const` đều sử dụng để khai báo một biến **block-scoped**, những biến này chỉ có giá trị trong block bao chứa nó và không thể tái khai báo.
```
let num1 = 1;
const num2 = 2;
```
Nếu kiểu giá trị của biến là kiểu **nguyên thủy**(`string`, `number`, `boolean`, `null`, `undefined`) thì: 
- Biến `let` có thể thay đổi giá trị.
- Biến `const` không thể thay đổi giá trị. 

Nếu kiểu giá trị của biến là kiểu **tham chiếu**(`object`, `array`, `function`) thì:
- Biến `let` có thể thay đổi giá trị biến và giá trị thuộc tính của biến.
- Biến `const` chỉ có thể thay đổi giá trị thuộc tính của biến, không thể thay đổi giá trị thuộc tính biến

## II.Template literals (Template strings)
Template literals là chuỗi ký tự được khai báo trong dấu (``) thay vì dấu ("").
```
let text = `Some text`;
```
Có thể sử dụng cả ("") và ('') trong Template literals
```
let text = `He's often called "Johnny"`;
```
Template literals cho phép multiline string
```
let text =
`The quick
brown fox
jumps over
the lazy dog`;
```
Template literals khác `string` thông thường ở chỗ nó cho phép ta nhúng vào những biểu thức gọi là **substitutions** được đặt trong **interpolation** `${...}`, bao gồm:

**Variable Substitutions**
```
let firstName = "John";
let lastName = "Doe";

let text = `Welcome ${firstName}, ${lastName}!`;
```
**Expression Substitution**
```
let price = 10;
let VAT = 0.25;

let total = `Total: ${(price * (1 + VAT)).toFixed(2)}`;
```

## III.Arrow Function
Arrow function là biểu thức thay thế cho function truyền thống, làm cú pháp định nghĩa hàm trở nên ngắn gọn hơn.

**Một tham số, biểu thức đơn giản:** Không cần dấu ngoặc đơn, không cần return.
```
param => expression
```
**Nhiều tham số, biểu thức đơn giản:** Cần có ngoặc đơn, không cần return.
```
(param1, param2,...) => expression
```
**Một tham số, nhiều dòng lệnh:** Không cần dấu ngoặc đơn, cần có dấu ngoặc nhọn ở body.
```
param => {
  let a = 1;
  return a + param;
}
```
**Nhiều tham số, nhiều dòng lệnh:** Cần có ngoặc đơn, cần có dấu ngoặc nhọn ở body.
```
(param1, paramN) => {
   let a = 1;
   return a + param1 + paramN;
}
```
Điểm khác biệt của arrow function so với function truyền thống:
- Không hình thành ngữ cảnh riêng, từ khóa `this` của arrow function phụ thuộc vào phạm vi nó được định nghĩa.
- Không có arguments object, vì thế không thể nhận trực tiếp các biến tham chiếu.
- Không thể sử dụng như constructor của object.

## IV.Enhanced Object Literals
Enhanced object literals là ký hiệu dùng để định nghĩa một đối tượng, thay thế cho ký hiệu cũ trong phiên bản ES5.

**Khai báo**\
ES5:
```
var
    a = 1, b = 2;
    obj = {
        a: a,
        b: b,
        sum:  function(a, b) { return a + b; },
        mult: function(a, b) { return a * b; }
    };

// obj.a = 1, obj.b = 2
// obj.sum = 3, obj.mult = 2
```
ES6:
```
const
    a = 1, b = 2;
    obj = {
        a,
        b,
        sum:  (a, b) => a + b,
        mult: (a, b) => a * b
    };
// obj.a = 1, obj.b = 2
// obj.sum = 3, obj.mult = 2
```

**Keys thuộc tính động**\
ES5 không thể dùng một biến cho tên key, mặc dù có thể được thêm sau khi đối tượng được tạo.
```
var
  key1 = 'one',
  obj = {
    two: 2,
    three: 3
  };

obj[key1] = 1;

// obj.one = 1, obj.two = 2, obj.three = 3
```
Tên của key có thể được gán động bằng cách đặt biểu thức trong dấu ngoặc vuông.
```
const
  key1 = 'one',
  obj = {
    [key1]: 1,
    two: 2,
    three: 3
  };

// obj.one = 1, obj.two = 2, obj.three = 3
```

**Hỗ trợ Destructuring**
```
const myObject = {
  one:   'a',
  two:   'b',
  three: 'c'
};

const { one, two, three } = myObject;
// one = 'a', two = 'b', three = 'c'
```

## V.Tham số mặc định(Default Parameter)
Tham số mặc định cho phép tham số có thể tự khởi tạo nếu không có giá trị nào hoặc `undefined` được truyền vào.

**Cú pháp**
```
function fnName(param1 = defaultValue1, ..., paramN = defaultValueN) { ... }
```
Ví dụ:
```
function multiply(a, b = 1) {
  return a * b
}

multiply(5, 2)          // 10
multiply(5)             // 5
multiply(5, undefined)  // 5
```
ta cũng có thể sử dụng phương pháp cũ để thay thế
```
function multiply(a, b) {
  b = (typeof b !== 'undefined') ?  b : 1
  return a * b
}

multiply(5, 2)  // 10
multiply(5)     // 5
```
Tuy nhiên 2 phương pháp có sự khác biệt về **scope**. Khi tham số mặc định được khai báo, một scope mới sẽ được tạo để xác định các tham số, scope này là scope cha của scope body hàm. Điều này có nghĩa là các hàm và biến trong phần body hàm không thể truy cập vào các hàm và biến trong tham số mặc định.

Ví dụ: 
```
// Call function in parameter
function f(a = go()) { // Throws a `ReferenceError` when `f` is invoked.
  function go() { return ':P' }
}

// Call function using variable in parameter
function f(a, b = () => console.log(a)) {
  var a = 1
  b() // Prints `undefined`, because default parameter values exist in their own scope
}
```

## VI.Spread Operator
Cú pháp Spread cho phép một danh sách các thực thể có thể mở rộng ở nơi mà nó được mong đợi.
```
// Function call
myFunction(...iterableObj); // pass all elements of iterableObj as arguments to function myFunction

// Array
[...iterableObj, '4', 'five', 6]; // combine two arrays by inserting all elements from iterableObj

// Object
let objClone = { ...obj }; // pass all key:value pairs from an object 
```
Spread cung cấp một số giải pháp ngắn gọn và nhanh hơn các phương thức thông thường.

**Nối mảng**
```
let parts = ['shoulders', 'knees'];
let lyrics = ['head', ...parts, 'and', 'toes'];
//  ["head", "shoulders", "knees", "and", "toes"]
```

**Sao chép mảng**
```
let arr = [1, 2, 3];
let arr2 = [...arr]; // like arr.slice()

arr2.push(4);
//  arr2 becomes [1, 2, 3, 4]
//  arr remains unaffected
```

**Thay thế apply()**
```
// Before
function myFunction(x, y, z) { }
let args = [0, 1, 2];
myFunction.apply(null, args);

// After
function myFunction(x, y, z) { }
let args = [0, 1, 2];
myFunction(...args);
```

**Rest Operator**
Rest có cú pháp giống với Spread nhưng có tác dụng khác nhau, Spread cho phép danh sách thực thể mở rộng còn Rest cho phép tóm gọn danh sách thực thể lại. Rest thường được sử dụng để gói tham số của hàm.
```
function f(a, b, ...theArgs) {
  // ...
}
```
Đặc điểm:
- Chỉ có một Rest Parameter trong tham số.
- Rest Parameter phải là tham số cuối của hàm.
- Khác với `argument`, Rest Parameter là một instance của Array nên có thể áp dụng các phương thức của Array như `sort`, `map`, `forEach`,...

## VII.Lớp(Class)
Lớp là khuôn mẫu để hình thành đối tượng, chúng gói gọn dữ liệu với những hàm sử dụng dữ liệu đó.

### 1.Định nghĩa lớp
Có 2 cách để định nghĩa lớp đó là: 
- **Class Declarations** 
```
class Rectangle {}
```
- **Class Expressions**
```
let Rectangle = class {};
```
Định nghĩa hàm không có **hoisting** nên ta cần khai báo trước rồi mới sử dụng.
```
const p = new Rectangle(); // ReferenceError

class Rectangle {}
```

### 2.Thành phần
Cấu trúc lớp gồm 3 thành phần chính là **property**, **constructor** và **method**.
- **Property:** Là đơn vị mang dữ liệu của lớp. Property có 2 loại là **public** và **private**, public property có thể được truy cập trực tiếp từ bên ngoài lớp còn private property chỉ có thể truy cập trực tiếp bởi các thành phần trong lớp.
```
class Rectangle {
  height; // public
  #width; // private
}
```
- **Constructor:** Là phương thức khởi tạo của lớp, nó được gọi tự động khi một instance của lớp được tạo. Phương thức khởi tạo luôn có tên là `constructor`, nếu sử dụng tên khác, lớp sẽ nhận định nó như là phương thức bình thường và sử dụng phương thức khởi tạo mặc định. 
```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```
- **Method:** Là phương thức dùng để thao tác với lớp.
```
class Rectangle {
  height;
  width;
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10);

console.log(square.calcArea()); // 100
```
Property và Method có 2 loại là static và non-static, static chỉ có thể gọi trực tiếp, không qua instance của lớp, non-static thì ngược lại.
```
class Point {
  static displayName = "Point";
}

const p1 = new Point();
p1.displayName; // undefined

console.log(Point.displayName);      // "Point"
```

### 3.Subclass
Từ khóa `extends` được dùng khi định nghĩa lớp để tạo ra một lớp là lớp con của lớp khác.
```
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```
Từ khóa `super` dùng để truy cập phương thức của class cha.
```
class Cat {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Lion extends Cat {
  speak() {
    super.speak();
    console.log(`${this.name} roars.`);
  }
}

let l = new Lion('Fuzzy');
l.speak();
// Fuzzy makes a noise.
// Fuzzy roars.
```
Lớp trong ECMAScript chỉ có thể có 1 lớp cha, để có thể kế thừa từ nhiều lớp khác nhau, ta phải tạo ra nhiều lớp kế thừa lồng nhau. Để làm được điều đó ta định nghĩa 1 hàm có lớp cha là đầu vào - **Mix-in**
```
let calculatorMixin = Base => class extends Base {
  calc() { }
};

let randomizerMixin = Base => class extends Base {
  randomize() { }
};

class Foo { }
class Bar extends calculatorMixin(randomizerMixin(Foo)) { }
```

## VIII.Module

### 1.Định nghĩa
Các chương trình lớn hiện nay đều được chia thành các module nhỏ, các module này có tính đóng gói cao với những chức năng riêng biệt, điều đó cho phép chúng có thể bị xáo trộn, xóa đi hay thêm vào mà không ảnh hưởng đến hệ thống.

### 2.Lợi ích
- **Dễ bảo trì:** Module có tính đóng gói cao, chúng được thiết kế để ít phụ thuộc với bên ngoài càng nhiều càng tốt để có thể phát triển một cách độc lập. Việc sửa đổi module sẽ dễ dàng hơn khi chúng không có nhiều ảnh hưởng đến các module khác.
- **Phân chia namespace:** Việc vô tình sử dụng thực thể global nào đó trong chương trình lớn là không thể tránh khỏi, các module cho phép chúng ta phòng tránh việc đó bằng cách tạo ra các không gian riêng cho các thực thể đó.
- **Tái sử dụng:** Việc ít phụ thuộc vào bên ngoài khiến các module có thể dễ dàng sử dụng lại ở chương trình khác mà không phải sửa đổi gì.

### 3.Xuất/Nhập module
Ta dùng câu lệnh `export` để xuất các item cần thiết, `export` phải được đặt ở item cấp cao nhất trong module.
```
export const name = 'square';

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, length, length);

  return {
    length: length,
    x: x,
    y: y,
    color: color
  };
}
```
Có thể dùng 1 lệnh `export` để xuất tất cả các item bằng cách đặt chúng vào trong dấu ngoặc nhọn.
```
export { name, draw };
```
Để nhập module ta dùng câu lệnh `import`, bao gồm danh sách item và đường dẫn đến module cần nhập. Sau khi import các item có thể sử dụng bình thường.
```
import { name, draw } from 'square.js';
```
Ta có thẻ nhập module ở HTML bằng thể `<script>` với `type="module"`
```
<script type="module" src="main.js"></script>
```

**Tránh trùng tên**\
Để tránh trùng lặp tên giữa các item từ các module khác nhau, sử dụng từ khóa `as` để đổi tên chúng.
```
// inside module.js
export {
  function1 as newFunctionName,
  function2 as anotherNewFunctionName
};

// inside main.js
import { newFunctionName, anotherNewFunctionName } from './modules/module.js';
```
hoặc
```
// inside module.js
export { function1, function2 };

// inside main.js
import { function1 as newFunctionName,
         function2 as anotherNewFunctionName } from './modules/module.js';
```
Một phương pháp tốt hơn là import chúng dưới dạng object
```
import * as Module from './modules/module.js';

Module.function1()
Module.function2()
etc.
```

**Ghép module**\
Ta có thể ghép các module con thành một module lớn hơn để rút gọn câu lệnh import. Ở module lớn ta export tất cả item từ các module nhỏ, biến module lớn thành trạm trung chuyển item.
```
export { Square } from './shapes/square.js';
export { Triangle } from './shapes/triangle.js';
export { Circle } from './shapes/circle.js';
```
sau đó import các item đó từ module lớn về nơi cần thiết
```
import { Square, Circle, Triangle } from './modules/shapes.js';
```

**Tải module động**\
Module còn có thể tải tại thời điểm nó cần dùng, thay vì từ đầu.
```
import('./modules/myModule.js')
  .then((module) => {
    // Do something with the module.
  });
```

## IX.Promise
Promise là một đại diện cho sự thành công hoặc thất bại của một thao tác bất đồng bộ và giá trị trả về của nó.

Promise có 3 trạng thái:
- **pending:** Trạng thái khởi tạo, chưa thành công cũng không thất bại.
- **fulfilled:** Thao tác đã thành công.
- **rejected:** Thao tác thất bại.
Khi promise đạt trạng thái `fulfilled` hoặc `rejected` các handler gắn phương thức với `then()` của nó sẽ được gọi.

### Chain promise
Vì hàm `then()` và `catch()` đều trả về promise, nên các promise có thể nối với nhau.
```
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo');
  }, 300);
});

myPromise
  .then(handleResolvedA, handleRejectedA)
  .then(handleResolvedB, handleRejectedB)
  .then(handleResolvedC, handleRejectedC);
```
Xử lí rejected ở mỗi `then()` có thể gây ra một vài vấn đề với promise chain nhưng đôi khi không còn cách nào khác, lỗi phải được xử lí ngay khi nó xảy ra. Với trường hợp không cần thiết, ta có thể để đẩy việc xử lí lỗi có lệnh `catch()` ở cuối.
```
myPromise
.then(handleResolvedA)
.then(handleResolvedB)
.then(handleResolvedC)
.catch(handleRejectedAny);
```
Khi trong chuỗi, promise xuất hiện trạng thái mới goi là `settled` - tức trạng thái khi promise đã hoàn thành(`resolved`) nhưng bị giữ lại cho đến khi promise tiếp theo hoàn thành. Việc giải phóng promise xảy ra khi promise kế tiếp nó đạt trạng thái `settled`.

### Phương thức Promise
- **all:** Xử lí một chuỗi promise và trả về một mảng các kết quả. Kết quả trả về cũng là một promise, nó sẽ `rejected` nếu bất kỳ promise nào trong chuỗi `rejected` hoặc một non-promise xảy ra lỗi.
```
Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
```
- **allSettled:** Xử lí một chuỗi promise và trả về một mảng các các object chưa thông tin mô tả đầu ra của promise. Kết quả trả về cũng là một promise, nó sẽ `fulfilled` bất kể trạng thái của promise trong chuỗi là `fulfilled` hay `rejected`.
```
Promise.allSettled(promises).
  then((results) => results.forEach((result) => console.log(result.status)));
```
- **any:** Xử lí một chuỗi promise và trả về kết quả của promise đầu tiên `fulfilled`, nếu không có promise nào `fulfilled` nó sẽ trả về một `AggregateError`-một subclass của `Error` mà gộp nhiều lỗi lại với nhau.
```
const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));
```
- **race:** Xử lí một chuỗi promise và trả về kết quả của promise đầu tiên `resolved`.
```
Promise.race([promise1, promise2]).then((value) => console.log(value));
```

