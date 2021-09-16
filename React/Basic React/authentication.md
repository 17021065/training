# Xác thực
Xu hướng xác thực của các ứng dụng hiện nay là Single Sign-On(SSO), ta sẽ kết nối ứng dụng của mình đến một Identity Provider nào đó và Identity Provider này sẽ làm nhiệm vụ xác thực thay cho chúng ta. Một trong những Identity Provider phổ biến nhất trong React đó là Auth0.

Có 5 bước để tích hợp Auth0 vào ứng dụng của chúng ta:

## 1.Cấu hình Auth0

### Lấy Application Keys
Đăng ký tài khoản trên trang chủ của Auth0 để nhận Client ID. Client ID cho Auth0 biết ứng dụng của chúng ta có phải là ứng dụng đã được đăng ký sử dụng dịch vụ xác thực không.

### Cấu hình Callback URLs
Callback URL là URL trong ứng dụng ta muốn Auth0 chuyển tiếp về sau khi người dùng đã được xác thực. Nếu không cấu hình callback URL, người dùng sẽ không thể đăng nhập vào ứng dụng.

### Cấu hình Logout URLs
Logout URL là URL trong ứng dụng ta muốn Auth0 quay về sau khi người dùng đã đăng xuất khỏi hệ thống. Nếu không cấu hình logout URL, người dùng sẽ không thể đăng xuất khỏi ứng dụng.

### Cấu hình Allowed Web Origins
Chúng ta cần thêm URL của ứng dụng vào danh sách **Allowed Web Origins**. Nếu không đăng ký URL ứng dụng ở đây, chúng ta sẽ không thể làm mới token xác thực tự động, người dùng sẽ phải đăng nhập lại mỗi khi token hết hạn.

## 2.Cài đặt Auth0 React SDK
Cài đặt gói Auth0 React SDK bằng câu lệnh:
```
npm install @auth0/auth0-react
```

### Cấu hình Auth0Provider component
Ta import `Auth0Provider` từ package `@auth0/auth0-react` và bọc ứng dụng trong component `Auth0Provider` với cấu hình như sau:
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { Auth0Provider } from "@auth0/auth0-react";

ReactDOM.render(
  <Auth0Provider
    domain="YOUR_DOMAIN"
    clientId="YOUR_CLIENT_ID"
    redirectUri={window.location.origin}
  >
    <App />
  </Auth0Provider>,
  document.getElementById("root")
);
```
Component `Auth0Provider` nhận vào những props:
- `domain` và `clientId`: các giá trị này tương ứng với **Domain** và **Client ID** mà ta đã đăng ký với Auth0.
- `redirectUri`: URL mà ta muốn chuyển tiếp tới sau khi người dùng đã xác thực với Auth0.
`Auth0Provider` lữu trữ trạng thái xác thực và trạng thái của SDK, kể cả Auth0 đã sẵn sàng sử dụng hay chưa. Nó cũng cung cấp các phương thức trợ giúp để đăng nhập, ta có thể truy cập chúng thông qua hook `useAuth0()`.

## 3.Thêm đăng nhập vào ứng dụng
Auth0 React SDK cung cấp nhiều công cụ thực hiện việc xác thực người dùng nhanh chóng cho ứng dụng React, chẳng hạn như tạo một button đăng nhập với phương thức `loginWithRedirect()` từ `useAuth0()`. Thực thi `loginWithRedirect()` sẽ điều hướng người dùng đến **Auth0 Universal Login Page**, nơi mà Auth0 có thể xác thực họ. Khi xác thực thành công, Auth0 sẽ chuyển tiếp về ứng dụng, cụ thể là về `redirectUri` đã được cấu hình trong component `Auth0Provider`.
```
import React from "react";
import { useAuth0 } from "@auth0/auth0-react";

const LoginButton = () => {
  const { loginWithRedirect } = useAuth0();

  return <button onClick={() => loginWithRedirect()}>Log In</button>;
};

export default LoginButton;
```

## 4.Thêm đăng xuất vào ứng dụng
Tương tự, ta sử dụng phương thức `logout()` trong `useAuth0()` để đăng xuất. Thực thi `logout()` sẽ điều hướng người dùng đến **Auth0 logout endpoint**(`https://YOUR_DOMAIN/v2/logout`) và sau đó chuyển tiếp về địa chỉ được cấu hình trong tham số `returnTo`.
```
import React from "react";
import { useAuth0 } from "@auth0/auth0-react";

const LogoutButton = () => {
  const { logout } = useAuth0();

  return (
    <button onClick={() => logout({ returnTo: window.location.origin })}>
      Log Out
    </button>
  );
};

export default LogoutButton;
```

## 5.Hiển thị thông tin người dùng
Auth0 React SDK giúp chúng ta có thể lấy về một số thông tin liên quan đến người dùng đã đăng nhập, chẳng hạn như tên người dùng hay profile picture. Những thông tin này chứa bên trong thuộc tính `user` của `useAuth0()`.
```
import React from "react";
import { useAuth0 } from "@auth0/auth0-react";

const Profile = () => {
  const { user, isAuthenticated, isLoading } = useAuth0();

  if (isLoading) {
    return <div>Loading ...</div>;
  }

  return (
    isAuthenticated && (
      <div>
        <img src={user.picture} alt={user.name} />
        <h2>{user.name}</h2>
        <p>{user.email}</p>
      </div>
    )
  );
};

export default Profile;
```
Thuộc tính `user` chứa các thông tin nhạy cảm liên quan đến danh tính của người dùng, vì vậy nó phụ thuộc vào trạng thái xác thực của người dùng. Để phòng tránh lỗi xảy ra, ta cần dùng thuộc tính `isAuthenticated` để kiểm tra trạng thái xác thực trước khi render component sử dụng các thông tin trên. Chắc chắn SDK đã hoàn thành việc loading trước khi truy cập vào thuộc tính `isAuthenticated` bằng cách kiểm tra thuộc tính `isLoading` có giá trị `false` không.  