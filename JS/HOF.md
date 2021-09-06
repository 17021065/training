# Hàm bậc cao

## I.Định nghĩa
Hàm bậc cao là hàm thỏa mãn một trong hai điều kiện sau:
- Nhận vào một hàm làm đối số.
- Trả về kết quả là một hàm.

## II.Khái niệm liên quan
### Currying: 
Là kỹ thuật cho phép chuyển đổi một hàm có nhiều tham số thành những hàm liên tiếp có 1 tham số.

Ví dụ:
```
const example = (value1, value2, value3) => {
	…
	return result;
}
```
Chuyển đổi thành:
```
const example = value1 => value2 => value3 => {
	…
	return result;
}
```

Currying được sử dụng nhằm mục đích làm sạch code và thể hiện rõ quy trình nghiệp vụ, đồng thời hỗ trợ việc phát hiện và sửa lỗi. 

Ví dụ ta có một quy trình xuất hàng như sau:\
Kiểm tra nếu còn hàng\
    =>  Kiểm tra mã nhà kho\
        => Xuất hàng

Currying code cho quy trình này sẽ có dạng:
```
function checkStock(stockID){ 
    //some check code 
    if(err){throw err;} 
    
    return (warehouseID) => { 
        //some check code 
        if(err){throw err;}

        return(stockDeduct)=> { 
            //some check code         
            if(err){throw err;}
            return stockID + ' from ' + warehouseID + ' is reduced by ' + stockDeduct;      
        }
    }
}
```
Lệnh gọi hàm sẽ có dạng:
```
let orderItem298 = checkStock('FN9382')('SOUTH')(3);
```
Lúc này chúng ta có thể nhìn thấy rõ ràng quy trình nghiệp vụ trên lệnh gọi hàm, thứ tự đi vào của các tham số cũng được thể hiện trên đó. Ngoài ra, khi chúng ta không truyền đủ tham số vào hàm, nó sẽ trả lại một hàm cho chúng ta biết tham số nào còn thiếu. Điều đó sẽ giúp chúng ta kiểm tra xem mình đã có đủ tham số khi thực hiện quy trình hay chưa. Nếu không có Currying chúng ta sẽ không thể biết được đâu là tham số bị lỗi giữa rừng tham số của hàm.

### Closure: 
Là kĩ thuật xây dựng hàm mà hàm cha sẽ trả về 1 hàm con, trong đó hàm con có thể truy cập và thay đổi các biến của hàm cha.

Ví dụ:
```
const numFunc = () => {
	let num = 0;
	const plus = () => ++num;
	return plus;
}

var increment = numFunc();
```
Mục đích của Closure là để bảo vệ các biến và hàm nhạy cảm. Trong lập trình hướng chức năng, các biến trong hàm thường được giải phóng sau khi hàm được thực thi. Để cache lại những biến này buộc chúng chỉ được truy cập thông qua một hàm khác, từ đó tăng mức độ bảo mật của hệ thống. Như ví dụ trên, `increment()` tham chiếu đến hàm `plus()`, biến `num` của hàm `numFunc()` sẽ không bị giải phóng sau khi `increment()` được gọi. Ta có thể tiếp tục gọi hàm `increment()` để điều khiển biến `num`.

Một ví dụ khác:
```
a = (function () {
    var privateVariable;
    var privateFunction = function () {
        //do something sensitively
    }

    return {
        publicFunction : function () {
            //do something protect
            privateFunction();
        }
    }
})();
```
Trong quá trình phát triển, việc tách biệt các hàm theo chức năng là không thể tránh khỏi, các hacker có thể lợi dụng điều này để bỏ qua hàm bảo mật của hệ thống và truy cập các hàm private. Việc bao đóng nó lại trong các hàm public sẽ ngăn cản việc truy cập trực tiếp vào hàm private.

## III.Các phương thức HOF của Array
**Every** 
```
const isBelowThreshold = (currentValue) => currentValue < 40;
const array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
// expected output: true
```
**Filter**
```
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];
const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```
**Find**
```
const array1 = [5, 12, 8, 130, 44];
const found = array1.find(element => element > 10);

console.log(found);
// expected output: 12
```
**FindIndex**
```
const array1 = [5, 12, 8, 130, 44];
const isLargeNumber = (element) => element > 13;

console.log(array1.findIndex(isLargeNumber));
// expected output: 3
```
**FlatMap**
```
let arr1 = [1, 2, 3, 4];

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]
```
**ForEach**
```
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"
```
**Map**
```
const array1 = [1, 4, 9, 16];
// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```
**Reduce**
```
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```
**ReduceRight**
```
const array1 = [[0, 1], [2, 3], [4, 5]].reduceRight(
  (accumulator, currentValue) => accumulator.concat(currentValue)
);

console.log(array1);
// expected output: Array [4, 5, 2, 3, 0, 1]
```
**Some**
```
const array = [1, 2, 3, 4, 5];

// checks whether an element is even
const even = (element) => element % 2 === 0;

console.log(array.some(even));
// expected output: true
```