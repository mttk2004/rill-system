# Sơ đồ Use Case

Thư mục này chứa các sơ đồ Use Case cho hệ thống website bán đĩa than Rill, mô tả sự tương tác giữa người dùng (actors) và hệ thống để thực hiện một mục tiêu cụ thể.

## Cấu trúc thư mục

* `uc-0.puml`: Sơ đồ Use Case tổng quan, cung cấp cái nhìn bao quát về các nhóm chức năng chính và các actor có thể tương tác với chúng.
* `detailed/`: Thư mục chứa các sơ đồ Use Case chi tiết, phân rã từ sơ đồ tổng quan.

## Cách tiếp cận

Thay vì vẽ một sơ đồ riêng lẻ cho mỗi use case trong tài liệu `business-logic.md`, chúng tôi đã nhóm các use case có liên quan vào các sơ đồ lớn hơn theo luồng nghiệp vụ hoặc vai trò của actor. Cách tiếp cận này giúp:
* **Thể hiện rõ ngữ cảnh và luồng kịch bản:** Dễ dàng thấy được mối quan hệ và trình tự giữa các hành động.
* **Tăng tính dễ đọc:** Giảm sự lặp lại và giúp người xem tập trung vào một khía cạnh của hệ thống.

Mỗi sơ đồ trong thư mục `detailed/` sẽ có một file mô tả chi tiết đi kèm, giải thích rõ về tóm tắt, dòng sự kiện, tiền điều kiện và hậu điều kiện.

---

## Mô tả cho UC-1: Account & Profile Management

### Tóm tắt
Use case này mô tả các hoạt động liên quan đến việc quản lý tài khoản của người dùng. Nó bao gồm việc khách vãng lai (Guest) đăng ký tài khoản mới, người dùng (Customer, Admin) đăng nhập/đăng xuất hệ thống, và khách hàng (Customer) tự quản lý thông tin cá nhân cũng như sổ địa chỉ giao hàng.

### Dòng sự kiện chính (Happy Path)
1.  Khách vãng lai truy cập trang đăng ký.
2.  Hệ thống hiển thị form và yêu cầu nhập thông tin (email, mật khẩu, họ tên, SĐT).
3.  Khách vãng lai điền thông tin hợp lệ và gửi đi.
4.  Hệ thống tạo tài khoản vai trò "customer" và chuyển hướng đến trang đăng nhập.
5.  Người dùng nhập email và mật khẩu chính xác để đăng nhập.
6.  Hệ thống xác thực thành công và tạo một phiên làm việc (session).
7.  Khách hàng truy cập trang quản lý địa chỉ, thêm một địa chỉ giao hàng mới và đặt nó làm mặc định.
8.  Khách hàng nhấn nút đăng xuất, hệ thống hủy phiên làm việc và chuyển về trang chủ.

### Dòng sự kiện phụ (Alternative Flows)
* **Đăng nhập thất bại:** Người dùng nhập sai email hoặc mật khẩu, hệ thống hiển thị thông báo lỗi.
* **Đăng ký thất bại:** Khách vãng lai đăng ký bằng email đã tồn tại trong hệ thống, hệ thống báo lỗi.
* **Truy cập trái phép:** Người dùng chưa đăng nhập cố gắng truy cập vào một trang yêu cầu xác thực (ví dụ: trang quản lý tài khoản), hệ thống sẽ chuyển hướng họ đến trang đăng nhập.

### Tiền điều kiện (Pre-conditions)
* Người dùng có thể truy cập vào hệ thống Rill thông qua trình duyệt web.

### Hậu điều kiện (Post-conditions)
* **Đăng ký thành công:** Một tài khoản `User` mới với vai trò `CUSTOMER` được tạo trong cơ sở dữ liệu.
* **Đăng nhập thành công:** Một phiên làm việc (session) được tạo cho người dùng.
* **Cập nhật thành công:** Thông tin cá nhân hoặc danh sách địa chỉ của khách hàng được cập nhật.
* **Đăng xuất thành công:** Phiên làm việc của người dùng bị hủy.

---

## Mô tả cho UC-2: Customer Shopping Flow

### Tóm tắt
Use case này mô tả toàn bộ luồng mua sắm của một khách hàng đã đăng nhập. Quá trình bắt đầu từ việc duyệt và tìm kiếm sản phẩm, thêm vào giỏ hàng, tiến hành đặt hàng, và cuối cùng là để lại đánh giá cho sản phẩm đã mua.

### Dòng sự kiện chính (Happy Path)
1.  Khách hàng đăng nhập thành công vào hệ thống.
2.  Khách hàng duyệt danh sách sản phẩm, sử dụng bộ lọc/tìm kiếm để tìm đĩa than mong muốn.
3.  Khách hàng thêm một hoặc nhiều sản phẩm vào giỏ hàng.
4.  Khách hàng đi đến trang giỏ hàng, kiểm tra lại sản phẩm và số lượng, sau đó tiến hành thanh toán.
5.  Khách hàng chọn một địa chỉ giao hàng từ sổ địa chỉ đã lưu.
6.  Hệ thống kiểm tra tồn kho và xác nhận tất cả sản phẩm đều còn hàng.
7.  Khách hàng xác nhận đặt hàng (với phương thức COD).
8.  Hệ thống tạo một đơn hàng mới với trạng thái "Pending Confirmation" và xóa các sản phẩm trong giỏ hàng.
9.  Sau khi đơn hàng được giao thành công (trạng thái "Delivered"), khách hàng truy cập lịch sử đơn hàng và viết đánh giá (rating và comment) cho một sản phẩm đã mua.

### Dòng sự kiện phụ (Alternative Flows)
* **Hết hàng khi thanh toán:** Tại bước 6, nếu một sản phẩm trong giỏ hàng hết hàng, hệ thống sẽ hiển thị thông báo lỗi và không cho phép đặt hàng.
* **Khách hàng hủy đơn hàng:** Sau bước 8, khi đơn hàng đang ở trạng thái "Pending Confirmation", khách hàng có thể vào xem chi tiết đơn hàng và nhấn nút hủy. Hệ thống sẽ cập nhật trạng thái đơn hàng thành "Cancelled".
* **Xóa sản phẩm khỏi giỏ:** Trước khi thanh toán, khách hàng có thể xóa một sản phẩm ra khỏi giỏ hàng.
* **Đánh giá không hợp lệ:** Khách hàng cố gắng đánh giá một sản phẩm từ một đơn hàng chưa được giao thành công. Hệ thống sẽ không cho phép.

### Tiền điều kiện (Pre-conditions)
* Người dùng phải đăng nhập vào hệ thống với vai trò `CUSTOMER`.

### Hậu điều kiện (Post-conditions)
* **Đặt hàng thành công:** Một đối tượng `Order` được tạo với trạng thái `PENDING`, giỏ hàng (`Cart`) của khách hàng được làm trống.
* **Hủy đơn thành công:** Trạng thái của `Order` được cập nhật thành `CANCELLED`.
* **Đánh giá thành công:** Một đối tượng `Review` được tạo và liên kết với sản phẩm trong đơn hàng đó.

---

## Mô tả cho UC-3: Admin Functions

### Tóm tắt
Use case này mô tả các hoạt động quản trị hệ thống được thực hiện bởi Admin. Các hoạt động này bao gồm quản lý danh mục (sản phẩm, nghệ sĩ), xử lý và thực thi đơn hàng (xác nhận, cập nhật trạng thái), và quản lý thông tin khách hàng.

### Dòng sự kiện chính (Happy Path)
1.  Admin đăng nhập vào trang quản trị.
2.  Admin truy cập mục quản lý sản phẩm, thêm một sản phẩm mới với đầy đủ thông tin (tên, giá, số lượng tồn kho, nghệ sĩ...).
3.  Admin truy cập danh sách đơn hàng và thấy một đơn hàng mới ở trạng thái "Pending Confirmation".
4.  Admin xem chi tiết đơn hàng và nhấn "Xác nhận".
5.  Hệ thống cập nhật trạng thái đơn hàng thành "Confirmed" và tự động trừ số lượng tồn kho của các sản phẩm tương ứng.
6.  Sau khi giao hàng cho đơn vị vận chuyển, Admin cập nhật trạng thái đơn hàng thành "Shipping".
7.  Khi nhận được xác nhận giao hàng thành công, Admin cập nhật trạng thái thành "Delivered".
8.  Admin truy cập mục quản lý khách hàng để xem lịch sử mua hàng của một khách hàng cụ thể.

### Dòng sự kiện phụ (Alternative Flows)
* **Xóa nghệ sĩ không thành công:** Admin cố gắng xóa một nghệ sĩ đang được liên kết với ít nhất một sản phẩm. Hệ thống sẽ hiển thị thông báo lỗi và không cho phép xóa.
* **Admin hủy đơn hàng:** Tại bước 4, thay vì xác nhận, Admin có thể hủy đơn hàng (ví dụ: do không liên lạc được với khách). Hệ thống cập nhật trạng thái đơn hàng thành "Cancelled".
* **Cập nhật sản phẩm:** Admin chỉnh sửa thông tin (ví dụ: giá, mô tả) của một sản phẩm đã tồn tại.

### Tiền điều kiện (Pre-conditions)
* Người dùng phải đăng nhập vào hệ thống với vai trò `ADMIN`.
* Tất cả các hoạt động đều yêu cầu xác thực quyền Admin.

### Hậu điều kiện (Post-conditions)
* **Quản lý danh mục thành công:** Dữ liệu sản phẩm hoặc nghệ sĩ được tạo mới, cập nhật hoặc xóa khỏi hệ thống.
* **Xử lý đơn hàng thành công:** Trạng thái của `Order` được cập nhật theo đúng quy trình (`Confirmed` -> `Shipping` -> `Delivered`).
* **Xác nhận đơn hàng:** Số lượng tồn kho (`stockQuantity`) của `Product` bị giảm đi.
