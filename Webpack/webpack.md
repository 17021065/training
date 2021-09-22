# Webpack

## I.Định nghĩa
Webpack là một công cụ đóng gói module. Webpack giúp tối ưu và tối giản chương trình trước khi triển khai bằng cách xây dựng **dependency graph** với root là **entry point**, sau đó gói gọn các module đó vào một **bundle**.

**Dependency graph:** là một biểu đồ mối quan hệ **import** và **export** các module.\
**Entry point:** điểm đầu tiên mà webpack duyệt để xây dựng dependency graph.\
**Bundle:** tệp chứa tất cả những code dùng trong chương trình(đã được tối ưu và tối giản).\

## II.Hệ thống tệp và thư mục
Chương trình áp dụng webpack sẽ có cấu trúc như sau:
```
webpack-demo
  |- /build
  |- /dist
  |- /node_modules
  |- /src
  |- package.json
  |- webpack.config.js
```
### 1.src (Sources)
Thư mục `src` sẽ chứa các module sử dụng trong chương trình của chúng ta, có thể là các tệp `.js`, `.css`, `.scss`, `.png`...

Module là các mảnh nhỏ của chương trình, chúng có tính thuần túy, độc lập với các module khác. Chúng liên kết với nhau bằng câu lệnh `import` và `export`.

### 2.node_modules
Thư mục `node_modules` chứa các thư viện và module được cài đặt thông qua Node.js hay còn gọi là dependency.

### 2.dist (Distribution)
Thư mục `dist` chứa các **chunk** và **asset** của chương trình.

**Chunk:** là các module đã tối ưu và tối giản.
**Asset:** là các tệp tài nguyên như hình ảnh, font, dữ liệu... đã qua xử lý. 

Các tệp trong src và node_modules sẽ được đưa vào một quá trình xử lý. Ở đây, chúng sẽ được kiểm tra cú pháp, rà soát lỗi, cắt bỏ những phần không sử dụng và biên dịch thành các code mà các trình duyệt cũ có thể hiểu được.

### 4.build 
Thư mục `build` chứa các bundle - thứ được dùng để triển khai ứng dụng. Code trong bundle là code từ các asset được tổng hợp lại.

### 5.package.json
Tệp `package.json` chứa các cấu hình của ứng dụng được biểu diễn dưới dạng JSON, bao gồm những trường như:
- `name`: tên ứng dụng.
- `version`: phiên bản.
- `devDependencies`: danh sách thư viện và công cụ được cài đặt trong ứng dụng.
Và nhiều trường cấu hình khác.

### 6.webpack.config.js
Tệp `webpack.config.js` chứa các cấu hình cho công tác dóng gói của webpack, ta có rất nhiều trường để cấu hình, ví dụ như:
- `entry`: chỉ tới module mà webpack xây dựng dependency graph dựa trên nó. Webpack sẽ bắt đầu duyệt từ entry point và ghi lại các thư viện và module được `import` trong từng node.

- `output`: chỉ tới thư mục đầu ra của quá trình xử lí code. Thuộc tính bao gồm:
    - `filename`: cấu hình tên các tệp asset. 
    -  `path`: đường dẫn tới thư mục `dist`.
    - `publicPath`: đường dẫn url mặc định của page.

- `modules`: chứa các cấu hình cho module. Chủ yếu là các `rules` chỉ định loader cho từng kiểu tệp asset.

- `plugins`: chứa các plugins dùng trong webpack. Có nhiều module hữu dụng như `HtmlWebpackPlugin`, `HotModuleReplacementPlugin`, `ESLintPlugin`...

- `optimization`: chứa các cấu hình cho việc tối ưu hóa chunk.

- `mode`: cấu hình môi trường đóng gói. Có 2 mode trong webpack là `development` và `production`. Development cho phép ta sử dụng source map, local server, liveloading và hot module replacement còn Production hỗ trợ tối giản, tối ưu asset và chunk.

## III.Cơ chế hoạt động
Các tệp trong src sẽ được đưa vào hệ thống xử lí của webpack, chuyển hóa thành các asset và chunk trong dist. Quá trình này gồm:

- **Phát hiện lỗi:** các lỗi không phải lỗi trong thời gian chạy như sai đường dẫn, sử dụng biến chưa khai báo... sẽ được hệ thống phát hiện và cảnh bảo cho lập trình viên. Quá trình xử lí sẽ bị hủy nếu xảy ra lỗi.

- **Kiểm tra cú pháp:** kiểm tra code có tuân thủ đúng theo một quy tắc cài đặt trước hay không. Quá trình này là optional, được thêm vào bằng cách cài đặt plugin Eslint.
```
npm install --save-dev eslint
```
Cài đặt plugin vào trong `webpack.config.js`.
```
const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [new ESLintPlugin(options)],
  // ...
};
```
Eslint nhận vào một `options` chứa các thông số cấu hình cho quá trình kiểm tra, nó được định nghĩa trong tệp đuôi `.eslintrc`. Tệp này có thể viết dưới nhiều cấu trúc khác nhau như `js`, `yaml`, `yml`, `json`. 

- **Cắt bỏ phần không sử dụng:** webpack sẽ không biên dịch các tệp không sử dụng và xử lý trùng lặp import module. Quá trình cũng này là optional, được cài đặt thông qua thuộc tính của `output` và `optimization`.\
Thêm thuộc tính `output.clean` để **loại bỏ các tệp không sử dụng**.
```
module.exports = {
  // ...
  output: {
    // ...
    clean: true,
  },
  // ...
};
```
Sử dụng thuộc tính `optimization.splitChunks` hoặc `optimization.runtimeChunk` để **xử lí trùng lặp import module**. `splitChunks` tách các module được import nhiều vào một chunk riêng, `runtimeChunk` tạo ra tệp runtime xử lí công việc import và export riêng thay vì thực hiện điều đó trong entry.
```
module.exports = {
  // ...
  optimization: {
    runtimeChunk: true,
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
  // ...
};
```
- **Biên dịch:** các tệp sẽ được truyền qua các loader - công cụ giúp biên dịch code thành dạng mọi browser đều hiểu. Các loader được cấu hình tại `module.rules` với 2 thuộc tính là `test`(tệp nào) và `use`(loader nào).
```
module.exports = {
  // ...
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  // ...
};
```