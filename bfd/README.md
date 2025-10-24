# Sơ đồ Phân rã Chức năng (Business Function Diagram - BFD)

Thư mục này chứa Sơ đồ Phân rã Chức năng (BFD) cho hệ thống website bán đĩa than Rill.

## Mục đích

Sơ đồ BFD được sử dụng để trực quan hóa các chức năng nghiệp vụ chính của hệ thống theo cấu trúc phân cấp. Nó giúp cung cấp một cái nhìn tổng quan, từ cấp cao nhất xuống các chức năng chi tiết hơn, trả lời cho câu hỏi: *"Hệ thống có thể làm được những gì?"*.

## Sơ đồ: `bfd.mermaid`

Sơ đồ này phân rã hệ thống Rill thành các khối chức năng chính sau:
* **Account Management**: Các chức năng liên quan đến tài khoản người dùng như đăng ký, đăng nhập.
* **Product & Catalog Management**: Các chức năng duyệt, tìm kiếm và quản lý sản phẩm, nghệ sĩ.
* **Sales & Checkout**: Các chức năng liên quan đến giỏ hàng và quá trình đặt hàng của khách.
* **Order Fulfillment & Post-Sale**: Các chức năng xử lý đơn hàng sau khi được đặt và các hoạt động sau bán hàng như đánh giá.
* **Administration**: Các chức năng quản trị dành riêng cho Admin.

Sơ đồ này là cơ sở để xác định các ca sử dụng (use case) và phân tích hệ thống chi tiết hơn.
