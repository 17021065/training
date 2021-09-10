# Ủy quyền(Authorization)

## JSON Web Token(JWT)

### Định nghĩa
JWT là một phương tiện đại diện cho các yêu cầu chuyển giao giữa Client-Server, các thông tin trong chuỗi JWT được định dạng bằng JSON. JWT bao gồm 3 phần **header**, **payload** và **signature** ngăn cách nhau bằng dấu chấm.
```
const token = header + '.' + payload + '.' + signature
```

**header:** chứa kiểu token và thuật toán mã.
```
{
    "typ": "JWT",
    "alg": "HS256"
}
```
- `typ` (type) chỉ ra rằng đối tượng là một **JWT**
- `alg` (algorithm) xác định thuật toán mã hóa cho chuỗi là ***HS256***

**payload:** chứa dữ liệu.
```
{
  "user_name": "admin",
  "user_id": "1513717410",
  "authorities": "ADMIN_USER",
  "jti": "474cb37f-2c9c-44e4-8f5c-1ea5e4cc4d18"
}
```

**signature:** chưa một đoạn mã hóa từ header, payload và một khóa bí mật(secret), dùng để kiểm tra tính toàn vẹn dữ liệu.
```
data = base64urlEncode( header ) + "." + base64urlEncode( payload )
signature = Hash( data, secret );
```

### 2.Lợi ích
- **Nhẹ:** JWT có có định dạng JSON nên nhẹ hơn rất nhiều so với các token khác có định dạng XML như **Simple Web Tokens (SWT)** và **Security Assertion Markup Language Tokens (SAML)**.
- **Bảo mật:** Cơ chế bảo mật bằng cặp khóa public/private của JWT hiệu quả và đơn giản hơn chữ kí số XML.
- **Dễ xử lí** Định dạng JSON dễ dàng thao tác với các hàm liên quan đến object hơn văn bản XML.

## II.Oauth2

### 1.Định nghĩa
Oauth2 là một giao thức chuẩn công nghiệp cho công tác ủy quyền. Oauth2 được thiết kế tập trung vào việc đơn giản hóa công việc ủy quyền của người dùng và tạo ra một dòng ủy quyền giữa các ứng dụng trên các nền tảng khác nhau.

### 2.Cơ chế
- Ứng dụng (website hoặc mobile app) yêu cầu ủy quyền để truy cập vào Resource Server (Gmail,Facebook, Twitter hay Github…) thông qua User.
- Nếu User ủy quyền cho yêu cầu trên, Ứng dụng sẽ nhận được ủy quyền từ phía User (dưới dạng một token).
- Ứng dụng gửi thông tin định danh (ID) của mình kèm theo ủy quyền của User tới Authorization Server.
- Nếu thông tin định danh được xác thực và ủy quyền hợp lệ, Authorization Server sẽ trả về cho Ứng dụng `access_token`. Đến đây quá trình ủy quyền hoàn tất.
- Để truy cập vào tài nguyên từ Resource Server và lấy thông tin, Ứng dụng sẽ phải đưa ra `access_token` để xác thực.
Nếu `access_token` hợp lệ, Resource Server sẽ trả về dữ liệu của tài nguyên đã được yêu cầu cho Ứng dụng.

## III.Token

### 1.Định nghĩa
Token là một mảnh dữ liệu chứa đủ thông tin để phục vụ cho công tác **xác thực** và **ủy quyền**.
- **Xác thực:** Quá trình xác nhận danh tính của người dùng hoặc thiết bị.
- **Ủy quyền:** Quá trình kiểm tra những nguồn tài nguyên nào, những hành động nào người dùng hoặc thiết bị có thể truy cập, thực hiện.

### 2.Access Token
Access Token là token chưa thông tin ủy quyền cho phép ứng dụng khách được thực hiện một tác vụ hoặc truy cập một nguồn tài nguyên nào đó.\ 

Bất kì ai có access token đều có thể sử dụng nó, nghĩa là các hacker có thể cố gắng xâm nhập hệ thống và đánh cắp access token nhằm truy cập trái phép vào các nguồn tài nguyên được bảo hộ. Chính vì lý do này, việc giảm thiểu tối đa sự xâm phạm access token là vô cùng quan trọng đối với các nhà cung cấp dịch vụ. Một phương pháp được đưa ra đó là đặt tuổi thọ cho access token.
Đã có một vài cách để ứng dụng khách có thể tái cung cấp access token mới cho người dùng. Ví dụ, một khi token hết hạn, ứng dụng khách có thể yêu cầu người dùng đăng nhập lại để lấy token mới. Tuy nhiên cách này có thể gây ra bất tiện cho người dùng, thay vào đó họ sử dụng **refresh token**.

### 3.Refresh Token
Refresh Token là token cho phép ứng dụng khách có thể lấy access token mới mà không cần yêu cầu người dùng đăng nhập lại.\

Ứng dụng khách sẽ gửi refresh token này đến Authorization Server khi cần lấy access token mới. Refresh token không thể ngăn access token rơi vào tay hacker tuy nhiên người dùng và Resource Server có thể chủ động tiêu hủy refresh token, tránh để hacker tiếp tục lợi dụng nó.

