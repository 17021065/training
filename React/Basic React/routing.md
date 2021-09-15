# Định tuyến
Các trang web thông thường khi chuyển từ trang này sang trang khác, thường sử dụng thẻ `<a>` để làm điều đó. Cách này khiến trình duyệt phải tải mới toàn bộ trang, dẫn đến thời gian chờ cao, làm giảm chất lượng trải nghiệm người dùng. Các ứng dụng phát triển bằng React thì không như vậy, ứng dụng React là Single Page Application(SPA), tức là chỉ có một trang duy nhất với nhiều view khác nhau. Để có thể hiển thị các view khác nhau trên cùng một trang, ta sẽ cần sử dụng đến các thư viện routing để điều hướng. Thư viện phổ biến trong số ấy là `React Router`.

React router gồm 3 package là:
- `react-router`
- `react-router-dom`
- `react-router-native`
Trong đó, `react-router` là phần base của thư viện, `react-router-dom` dùng để phát triển web, `react-router-native` dùng để phát triển ứng dụng di động.

React router có 3 thành phần chính:
- `BrowserRouter`: dùng để bọc tất cả route component.
- `Route`: dùng để ẩn/hiện component (page) mà nó chứa.
- `Link`: sinh ra các đường dẫn để di chuyển tới page mà ta mong muốn.

## BrowserRouter
Import `BrowserRouter` từ thư viện và dùng nó để bọc cả chương trình của chúng ta.
```
import React from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter as Router } from 'react-router-dom'

ReactDOM.render(
  <Router>
      <div>
        <!-- -->
      </div>
  </Router>,
  document.getElementById('app')
)
```
`BrowserRouter` chỉ có thể chứa 1 component con, vì vậy ta có thể bọc app vào trong trong 1 thẻ `<div>` hoặc react fragment `<>`.

## Route
`Route` dùng để xác định component nào sẽ được render với `path` nào trên `url`. `BrowserRouter` sẽ dựa theo `path` để render component, nếu không khớp với `path` nào, nó sẽ trả ra `null`.
```
import React from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter as Router, Route } from 'react-router-dom'

const Home = () => (
  <div>
    <h1>Home Component</h1>
  </div>
)

const About = () => (
  <div>
    <h1>About Component</h1>
  </div>
)

ReactDOM.render(
  <Router>
      <div>
            <Route path='/' component={Home} />
            <Route path='/about' component={About} />
      </div>
  </Router>,
  document.getElementById('app')
)
```
Có 3 cách dùng `Route`:
1. `<Route component>`: Render ra component chỉ khi `location` khớp với thuộc tính `path`.
```
<Route path='/' component={Home} />
```

2. `<Route render>`: Giống như cách trên, nhưng cách này có thể render ra inline component và có thể truyền thêm các `props` khác cho component.
```
const FadingRoute = ({ component: Component, ...rest }) => (
  <Route {...rest} render={routeProps => (
    <FadeIn>
      <Component {...routeProps}/>
    </FadeIn>
  )}/>
)

<FadingRoute path="/cool" component={Something}/>
```

3. `<Route children>`: Cách này sẽ luôn luôn render ra component, nhưng sẽ có một biến `match` để kiểm tra `url` có khớp với path không. Ứng dụng điển hình cho các này là thêm `class="active"` cho menu bar item.
```
const ListItemLink = ({ to, ...rest }) => (
  <Route
    path={to}
    children={({ match }) => (
      <li className={match ? "active" : ""}>
        <Link to={to} {...rest} />
      </li>
    )}
  />
);

<ul>
  <ListItemLink to="/somewhere" />
  <ListItemLink to="/somewhere-else" />
</ul>;
```

## Link
Component này dùng để di chuyển đến view khác, tác dụng tương tự như thẻ `<a>` nhưng không làm mới lại trang.
```
import React from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter as Router, Route, Link } from 'react-router-dom'

ReactDOM.render(
  <Router>
      <div>
           <nav>
                <Link to='/'>Home</Link>
                <Link to='/about'>About</Link>
            </nav>
            <Route path='/' component={Home} />
            <Route path='/about' component={About} />
      </div>
  </Router>,
  document.getElementById('app')
)
```

Có 2 cách định nghĩa `Link`:
- `<Link to='path-name'>`: di chuyển đến Route mà không cần thêm bất kỳ `state`, hay search params nào cả.
- `<Link to={{pathname: 'abc', state: { title: 'this is title' }}}>`: di chuyển đến Route và có thêm state đi kèm.

Mỗi lần click vào `Link` thì trình duyệt sẽ thêm 1 bản ghi lưu lại state, params vào trong history stack, nếu back lại thì sẽ xóa bản ghi đó khỏi history.

## Routing động
Routing động là ta sẽ định hướng trang với các tham số trong url, ứng dụng sẽ render trang phù hợp với tham số đó. Chẳng hạn như ta có kịch bản như sau: route `/posts` sẽ hiển thị danh sách tiêu đề các post, route `posts/:id` sẽ display chi tiết post đó.
```
const  PostDetail = ({match}) => {
  const postId = match.params.id;
  // fetch post detail with id and render ...
}(

const PostList = ({match}) => (
  // fetch post list ...
  <div>
    <h3>Post List View</h3>
    <ul>
      {IDList.map(item => 
        <li>
          <Link to={`${match.url}/${item.id}`}>item.title</Link>
        </li>
      )}
    </ul>  
  </div>
);
```
Component `Route` của view này sẽ như sau:
```
<Route path="/posts" exact component={PostList}/>
<Route path="/posts/:id" component={PostDetail}/>
```

Ngoài ra React còn 2 thành phần nâng cao là: 
- `<Redirect>`: Điều hướng đến một location mới, location này sẽ được ghi đè lên location hiện tại trong history stack.
```
<Route exact path="/">
  {loggedIn ? <Redirect to="/dashboard" /> : <PublicHomePage />}
</Route>
```
- `<Switch>`: Hoạt đông giống `BrowserRouter` nhưng chỉ render 1 component duy nhất khớp với `path`
```
import { Route } from "react-router";

let routes = (
  <div>
    <Route path="/about">
      <About />
    </Route>
    <Route path="/:user">
      <User />
    </Route>
    <Route>
      <NoMatch />
    </Route>
  </div>
);
```