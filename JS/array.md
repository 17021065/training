# Mảng

## Định nghĩa
Mảng là đơn vị có thể chứa nhiều giá trị khác bên trong nó. Những giá trị nằm bên trong mảng được gọi là phần tử.

## Cách tạo
Có 2 cách khai báo mảng cơ bản:\
`[value1, value2, value3,...]`\
hoặc\
`Array.of(value1, value2, value3,...)`\
trong đó `value1, value2, value3...` là các phần tử của mảng

## Kiểm tra dữ liệu
Có 4 cách để kiểm tra 1 object có phải là một mảng không:
### Cách 1
Sử dụng phương thức `isArray()` của object Array, kết quả trả về sẽ là **true** hoặc **false**.\
`arrayName.isArray()`
### Cách 2
Sử dụng `Object.prototype.toString.call()`. Phương thức này trả về thông tin lớp chi tiết hơn so với `typeof`. Nó sẽ có dạng `[object [[class]]]`, trong đó `[[class]]` là tên internal class của object đó.\
`Object.prototype.toString.call(arrayName) === [object Array]`
### Cách 3
Sử dụng `instanceof`. Toán tử này kiểm tra xem đối tượng có phải một biểu hiện của phân lớp cụ thể nào đó không, kết quả trả về sẽ là **true** hoặc **false**.\
`arrayName instanceof Array`
### Cách 4
Sử dụng `constructor`. Phương thức này trả về chính hàm khởi tạo của đối tượng, so sánh với hàm khởi tạo của phân lớp tương ứng để kiểm tra dữ liệu.\
`arrayName.constructor === Array`\
hoặc\
`arrayName.constructor.name === "Array"`

## Truy xuất mảng
Sử dụng cấu trúc `arrayName[index]` trong đó `arrayName` là tên mảng, `index` là số danh mục của phần tử cần truy xuất.

## Phương thức mảng
- **Concat:** Ghép 2 hoặc nhiều mảng lại với nhau bằng cách trả về mảng mới với các phần tử là các phần tử của mảng tham số, không làm thay đổi mảng ban đầu.\
`array1.concat(array2, array3,…)`
- **CopyWithin:** Sao chép một nhóm các phần tử vào vị trí mới mà không làm thay đổi độ dài mảng. Phương thức làm thay đổi mảng ban đầu.\
`arrayName.copyWithin(newIndex, fromIndex, toIndex)`\
trong đó `fromIndex` là vị trí đầu, `toIndex` là vị trí cuối, `newIndex` là vị trí sẽ sao chép đến.
- **Entries:** Cung cấp một bộ lặp duyệt mảng dưới dạng cặp **key**-**value**.\
```    
    var iterator = arrayName.entries();
    for (let e of iterator) {
        console.log(e);
    }
```
- **Every:** Kiểm tra từng phần tử trong mảng với hàm thử được cung cấp. Trả lại giá trị **true** hoặc **false**.\
`arrayName.every(callbackFunction)`
- **Fill:** Thay thế một nhóm các phần tử bằng một giá trị tĩnh cụ thể. Phương thức làm thay đổi mảng ban đầu.
`arrayName.fill(value, fromIndex, toIndex)`
trong đó `fromIndex` là vị trí đầu, `toIndex` là vị trí cuối, `value` là giá trị sẽ thay thế.
- **Filter:** Trả về một mảng mới với các phần tử là những phần tử của mảng ban đầu đã thông qua hàm thử được cung cấp. Phương thức không làm thay đổi mảng ban đầu.
`arrayName.filter(callbackFunction)`
- **Find:** Trả về **giá trị** của phần tử đầu tiên trong mảng thỏa mãn hàm thử được cung cấp, trả về **undefined** nếu không có phần tử nào thỏa mãn.
`arrayName.find(callbackFunction)`
- **FindIndex:** Trả về **index** của phần tử đầu tiên trong mảng thỏa mãn hảm thử được cung cấp, trả về **-1** nếu không có phần tử nào thỏa mãn.
`arrayName.findIndex(callbackFunction)`
- **Flat:** Tạo một mảng mới với các phần tử của mảng con được ghép vào theo quy tắc đệ quy đến một độ sâu nhất định. Không làm thay đổi mảng ban đầu.
`arrayName.flat(depth)`
trong đó `depth` là độ sâu cần làm phẳng.
- **FlatMap:** Tạo ra một mảng với bằng cách áp dụng một hàm cho trước với từng phần tử rồi làm phẳng nó với độ sâu 1. Phương thức là sự kết hợp của 2 phương thức map() và flat(1). Không làm thay đổi mảng ban đầu.
`arrayName.flatMap(callbackFunction)`
- **ForEach:** Áp dụng hàm cho trước với từng phần tử trong mảng, làm thay đổi mảng ban đầu.
`arrayName.forEach(callbackFunction)`
- **From:** Tạo một mảng mới từ một mảng hoặc object có tính mảng. Không làm thay đổi mảng ban đầu.
`Array.from(originalArray, callbackFunction)`
trong đó `originalArray` là tên mảng hoặc object có tính mảng.
- **Include:** Kiểm tra xem có một giá trị cụ thể nào đó trong mảng không.
`arrayName.include(value)`
- **IndexOf:** Trả ra **index** của phần tử có giá trị bằng giá trị cho trước, trả ra **-1** nếu không có phần tử nào thỏa mãn. Phương thức duyệt mảng theo thứ tự từ trái sang phải.
`arrayName.indexOf(value, fromIndex)`
trong đó `fromIndex` là vị trí bắt đầu duyệt.
- **IsArray:** Kiểm tra xem một object nào đó có thuộc kiểu dữ liệu Array không. Giá trị trả về sẽ là **true** hoặc **false**.
`Array.isArray(object)`
- **Join:** Trả về một chuỗi gồm các phần tử ngăn cách nhau bởi một chuỗi kí tự nào đó, mặc định sẽ là dấu phẩy.
`arrayName.join(separatorString)`
trong đó `separatorString` là chuỗi kí tự ngăn cách.
- **Keys:** Cung cấp bộ lặp duyệt danh mục các phần tử trong mảng.
    var iterator = arrayName.keys();
    for (let e of iterator) {
        console.log(e);
    }
- **LastIndexOf:** Trả về **index** của phần tử có giá trị bằng giá trị cho trước, trả về **-1** nếu không có phần tử nào thỏa mãn. Phương thức duyệt mảng theo thứ tự từ phải sang trái.
`arrayName.lastIndexOf(value, fromIndex)`
trong đó `fromIndex` là vị trí bắt đầu duyệt.
- **Map:** Tạo một mảng mới với các phần tử là phần tử của mảng ban đầu đã áp dụng một hàm cho trước.
`arrayName.map(callbackFunction)`
- **Of:** Khởi tạo mảng với độ dài cụ thể hoặc với một bộ giá trị cụ thể.
`Array.of(length)` 
hoặc 
`Array.of(value1, value2, …)`
- **Pop:** Lấy ra phần tử cuối cùng của mảng. Phương thức làm thay đổi mảng ban đầu.
`arrayName.pop()`
- **Push:** Thêm một phần tử vào cuối mảng. Phương thức làm thay đổi mảng ban đầu.
`arrayName.push()`
- **Reduce:** Thực thi một hàm cho trước với từng phần tử trong mảng theo thứ tự từ trái sang phải và trả về một giá trị đơn từ việc tính toán với các phần tử trước đó. Không làm thay đổi mảng ban đầu. Giá trị khởi tạo mặc định là 0.
`arrayName.reduce(callbackFunction, initialValue)`
trong đó `initialValue` là giá trị khởi tạo.
- **ReduceRight:** Tương tự như `reduce()` nhưng duyệt từ phải sang trái.
`arrayName.reduceRight(callbackFunction, initialValue)`
- **Reverse:** Đảo ngược vị trí các phần tử trong mảng. Phương thức làm thay đổi mảng ban đầu.
`arrayName.reverse()`
- **Shift:** Lấy ra phần tử đầu tiên trong mảng. Phương thức làm thay đổi mảng ban đầu.
`arrayName.shift()`
- **Slice:** Trả về một phần của mảng, không làm thay đổi mảng ban đầu.
`arrayName.slice(fromIndex, toIndex)`
trong đó `fromIndex` là vị trí đầu, `toIndex` là vị trí cuối.
- **Some:** Kiểm tra nếu có ít nhất một phần tử thỏa mãn điều kiện cho trước. Giá trị trả về là **true** hoặc **false**.
`arrayName.some(callbackFunction)`
- **Sort:** Sắp xếp mảng theo một thứ tự nào đó. Thứ tự mặc định là tăng dần trong bảng giá trị UTF-16.
`arrayName.sort(callbackFunction)`
- **Splice:** Thay thế một nhóm các phần tử bắt đầu từ bằng một nhóm các phần tử khác.
`arrayName.sort(index, length, item1, item2,…)`
trong đó `index` là vị trí cần thay thế, `length` là độ dài mảng con cần thay thế, `item` là phần tử mới.
- **ToString:** Trả về chuỗi ký tự gồm các phần tử trong mảng ngăn cách nhau bởi dấu phẩy.
`arrayName.toString()`
- **Unshift:** Thêm phần tử vào đầu mảng. Phương thức làm thay đổi mảng ban đầu.
`arrayName.unShift()`
- **Value:** Cung cấp bộ lặp duyệt giá trị các phần tử trong mảng.
    var iterator = arrayName.value();
    for (let e of iterator) {
        console.log(e);
    }

## So sánh các phương thức mảng
- **Kiểm tra mảng:** `some()`, `find()`, `every()` sẽ tiết kiệm thời gian hơn `forEach()`, `map()`, `filter()` trong việc kiểm tra phần tử trong mảng có thỏa mãn yếu tố nào đó hay không do những hàm này sẽ dừng ngay khi tìm thấy phần tử không thỏa mãn.
- **Tìm vị trí phần tử:** Trong trường hợp mảng đã được sắp xếp, `lastIndexOf()` sẽ có hiệu quả tìm kiếm cao hơn với những giá trị nằm ở nửa cuối mảng.
- **Tính toán mảng:** `reduce()` sẽ dễ dàng hơn khi dùng để tính toán với các phần tử trong mảng so với `forEach()`, `every()`, `map()` vì có sẵn 1 biến truyền đi xuyên suốt quá trình duyệt mảng.
- **In phần tử mảng:** `join()` đa dụng hơn `toString()` và `toLocaleString()` vì có thể tùy chỉnh kí tự phân cách.

## Sao chép mảng
**1.Array.slice**
`copy = arrayName.slice()`
**2.Array.map**
`copy = arrayName.map(item => item)`
**3.Array.from**
`copy =  Array.from(arrayName)`
**4.Spread Operator**
`copy = […arrayName]`
**5.Array.of**
`copy = Array.of(…arrayName)`
**6.New Array**
`copy = new Array(…arrayName)`
**7.Destructuring**
`[…arrayName] = copy`
**8.Array.concat**
`copy = arrayName.concat()`
**9.Array.push**
`copy.push(…arrayName)`
**10.Array.unshift**
`copy.unshift(…arrayName)`
**11.Array.forEach**
`arrayName.forEach(value => copy.push(value)) `
**12.For**
`for(let = 0; i<arrayName.length, i++) copy.push (arrayName[i])`
**13.Array.reduce**
    copy = arrayName.reduce((result, current) =>{
    result.push(current); 
    return result;
    }, [])
**14.Apply**
`Array.prototype.push.apply(copy, arrayName)`

**So sánh hiệu năng:** Spread Operator > `for` > `slice` > `concat` > `forEach` > `reduce` > `map`;

