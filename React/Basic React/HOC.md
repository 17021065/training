# High-Order Components

## Định nghĩa
High-Order Components(HOC) là một hàm nhận vào một component và trả ra component mới.

Ví dụ:
```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
Một component chuyển đổi props thành UI, còn HOC chuyển đổi một component thành component khác.

## Sử dụng HOC cho Cross-Cutting Concerns
Component là đơn vị chính trong việc tái sử dụng code trong React, tuy nhiên có một số pattern không phù hợp với component truyền thống.\
Ví dụ component `CommentList` theo dõi một nguồn dữ liệu ngoại vi để render một danh sách bình luận.
```
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" is some global data source
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // Subscribe to changes
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // Clean up listener
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // Update component state whenever the data source changes
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```
Tiếp theo là component `BlogPost` cũng theo dõi nguồn dữ liệu đó cho một bài viết của blog.
```
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```  
2 component này không có tính identical - chúng gọi các phương thức khác nhau của `DataSource`, render ra các output khác nhau nhưng chúng lại được cài đặt giống nhau.
- Khi mount, thêm một change listener cho `DataSource`.
- Trong listener, gọi setState mỗi khi dữ liệu thay đổi.
- Khi unmount, loại bỏ change listener.
Trong một chương trình lớn, những pattern như trên xuất hiện rất nhiều lần, ta cần phải có một thực thể trừu tượng cho phép ta có thể định nghĩa  những logic đó ở một chỗ duy nhất và chia sẻ chúng đến các component khác. Đó chính là lợi thế của HOC.

Ta có thể viết một hàm tạo ra các component giống như `CommentList` và `BlogPost`, cùng theo theo dõi đến `DataSource`. Hàm này sẽ nhận vào component muốn theo dõi `DataSource` làm tham số. 
```
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```
Tham số đầu tiên là component cần bọc, tham số thứ hai là dữ liệu mong muốn.

Khi `CommentListWithSubscription` và `BlogPostWithSubscription` được render, `CommentList` và `BlogPost` sẽ được truyền vào dữ liệu mà chúng cần từ `DataSource`.
```
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
HOC không thay đổi component đầu vào, cũng không dùng tính kế thừa để sao chép hành vi. HOC kết hợp với component gốc bằng cách bọc component gốc trong một container component. HOC là một hàm thuần túy, không có bất kì side effect nào.

Component được bọc sẽ nhận được đầy đủ dữ liệu mà nó mong muốn, HOC không quan tâm dữ liệu đó được sử dụng làm gì và component được bọc cũng không quan tâm dữ liệu nó nhận được từ đâu.

Bởi vì `withSubscription` làm một hàm thông thường, ta có thêm hoặc bớt bao nhiêu tham số tùy ý. Ví dụ như ta muốn tên thuộc tính `data` có thể điều chỉnh được, thêm cấu hình cho `shouldComponentUpdate` hay cấu hình cho `DataSource`. Tất cả đều có thể vì HOC có toàn quyền quyết định cách nó được định nghĩa.

Giống như component, sự kết nối giữa `withSubscription` và component được bọc là props-based, vì thế ta có thể dễ dàng thay thế HOC này bằng HOC khác, chừng nào chúng còn cung cấp cùng một data cho component được bọc.

Có một số quy ước đối với HOC:
- **Chỉ truyền vào những props không liên quan:** HOC sẽ phải tách riêng những props mà nó sử dụng với những props mà nó muốn truyền vào component được bọc.
- **Tối ưu hóa khả năng kết hợp:** Tạo ra các hàm có thể kết hợp các HOC để tăng tình dễ đọc.
- **Đổi tên hiển thị**: Để thuận tiện cho việc truy vết debug, ta nên đổi tên hiển thị của HOC sao cho có liên quan tới component được bọc.