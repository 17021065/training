# Single Sign-On (SSO)

## I.Định nghĩa
SSO là một phương pháp xác thực cho phép người dùng xác thực trên nhiều ứng dụng và website một cách an toàn chỉ với một lần ủy quyền.

## II.Cơ chế
Để người dùng có thể đăng nhập nhiều domain khác nhau trong một lần, ta cần phải có một cơ chế để các domain chia sẻ thông tin session. Các giao thức SSO chia sẻ session theo nhiều các khác nhau những về cơ bản là sẽ có một **cental domain** để xác thực, sau đó central domain sẽ chia sẻ thông tin session với các domain khác theo nhiều cách, ví dụ: JWT. Mỗi khi người dùng đăng nhập đến domain yêu cầu phải xác thực, người dùng sẽ được chuyển tiếp đến cental domain, nếu central domain đã được xác thực, họ sẽ ngay lập tức được chuyển về domain ban đầu với token để thực hiện các request tiếp theo.

### Luồng đăng nhập
1. Người dùng truy cập đến ứng dụng hoặc website (Service Provider) họ muốn.
2. Service Provider gửi token chứa thông tin về người dùng cho hệ thống SSO (Identity Provider) để yêu cầu xác thực người dùng.
3. Identity Provider kiểm tra xem người dùng đã được xác thực chưa, nếu có sẽ cho phép ngường dùng truy cập Service Provider và nhảy qua bước 5.
4. Nếu người dùng chưa đăng nhập, họ sẽ nhận được một yêu cầu cung cấp giấy ủy quyền từ Identity Provider. Đó có thể là một biểu mẫu đơn giản gồm username và password hoặc là một biểu mẫu xác thực khác, chẳng hạn: One-Time Password(OTP).
5. Khi Identity Provider thông qua giấy ủy quyền được cung cấp, nó sẽ gửi token xác nhận xác thực thành công về cho Service Provider.
6. Token này sẽ sẽ được chuyển đến Service Provider thông qua trình duyệt của người dùng.
7. Token được nhận vởi Service Provider được thông qua dựa trên mối quan hệ tin tưởng giữa Service Provider và Identity Provider từ lúc cài đặt.
8. Người dùng được cấp phép truy cập Service Provider.

## III.Ưu điểm và nhược điểm

### Ưu điểm
- Người dùng chỉ cần ghi nhớ 1 mật khẩu có độ phức tạp cao.
- Người dùng có thể nhận được quyền truy cập nhanh hơn.
- Quản trị viên có thể nhanh chóng hủy bỏ đăng nhập trên toàn hệ thống khi người dùng rời khỏi.

## Nhược điểm
- Với một số ứng dụng cần độ bảo mật cao, cần phải chọn hệ thống SSO có thể cung cấp yếu tố xác thực cao hơn để ngăn cản người dùng truy cập vào những ứng dụng này mà không thông qua mạng lưới bảo mật.