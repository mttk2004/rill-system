# Bảng Câu Hỏi Thu Thập Yêu Cầu - Website Bán Đĩa Than Online

*Tài liệu này dùng để thu thập và làm rõ các yêu cầu từ khách hàng cho dự án xây dựng trang web bán đĩa than "Rill".*

---

## Phần A: Tổng Quan về Dự Án và Mục Tiêu Kinh Doanh

*Mục đích của phần này là để hiểu rõ tầm nhìn, mục tiêu và đối tượng mà website hướng tới.*

1.  **Mục tiêu chính:** Mục tiêu quan trọng nhất của website này là gì? (Ví dụ: bán sản phẩm, xây dựng cộng đồng, cung cấp thông tin cho người sưu tầm,...)
2.  **Đối tượng khách hàng:** Chân dung khách hàng mục tiêu của bạn là ai? (Ví dụ: người sưu tầm lâu năm, người mới chơi đĩa than, người mua quà tặng,...)
3.  **Lợi thế cạnh tranh:** Điều gì sẽ làm cho website của bạn nổi bật so với các đối thủ khác trên thị trường? (Ví dụ: chuyên về một thể loại nhạc hiếm, giá cả cạnh tranh, có các bài đánh giá chuyên sâu,...)
4.  **Tên thương hiệu:** Tên thương hiệu chính thức cho trang web là gì?
5.  **Website tham khảo:** Có trang web bán đĩa than hoặc trang thương mại điện tử nào bạn đặc biệt yêu thích về thiết kế hoặc tính năng không?

---

## Phần B: Yêu Cầu về Chức Năng

*Phần này tập trung vào các tính năng cụ thể mà người dùng và quản trị viên có thể thực hiện trên website.*

### B1. Chức năng dành cho Người dùng (Khách & Khách hàng đã đăng ký)

#### Sản phẩm & Tìm kiếm
6.  **Thông tin sản phẩm:** Khách hàng cần xem những thông tin chi tiết nào về một đĩa than? (Ví dụ: tên album, nghệ sĩ, danh sách bài hát, năm phát hành, hãng đĩa, tình trạng (mới/đã qua sử dụng), giá,...)
7.  **Tìm kiếm & Lọc:** Người dùng có cần các bộ lọc để tìm kiếm sản phẩm không? Nếu có, họ nên được lọc theo những tiêu chí nào? (Ví dụ: theo nghệ sĩ, thể loại, mức giá, năm phát hành,...)
8.  **Sắp xếp:** Kết quả tìm kiếm và danh sách sản phẩm nên được sắp xếp theo thứ tự nào? (Ví dụ: mới nhất, bán chạy nhất, giá từ thấp đến cao,...)

#### Tài khoản Người dùng
9.  **Mua hàng không cần tài khoản:** Khách truy cập có thể mua hàng mà không cần đăng ký tài khoản không (Guest Checkout)?
10. **Quản lý tài khoản:** Sau khi đăng ký, khách hàng có thể quản lý những thông tin gì? (Ví dụ: thông tin cá nhân, sổ địa chỉ giao hàng, xem lại lịch sử đơn hàng,...)

#### Giỏ hàng & Thanh toán
11. **Giỏ hàng:** Quy trình thêm, sửa đổi số lượng, hoặc xóa sản phẩm khỏi giỏ hàng nên hoạt động như thế nào?
12. **Phương thức thanh toán:** Website nên hỗ trợ những hình thức thanh toán nào? (Ví dụ: thanh toán khi nhận hàng (COD), chuyển khoản ngân hàng, ví điện tử (Momo, ZaloPay), thẻ tín dụng/ghi nợ,...)
13. **Vận chuyển:** Phí vận chuyển sẽ được tính toán ra sao? (Ví dụ: phí cố định cho mọi đơn hàng, miễn phí cho đơn hàng có giá trị cao, tính phí dựa trên địa chỉ giao hàng,...)

#### Các tính năng khác
14. **Đánh giá sản phẩm:** Khách hàng có được phép để lại đánh giá (rating) và bình luận cho những sản phẩm họ đã mua không?
15. **Sản phẩm yêu thích:** Có cần chức năng "Wishlist" (danh sách yêu thích) để khách hàng lưu lại các sản phẩm họ quan tâm không?

### B2. Chức năng dành cho Quản trị viên (Admin)

#### Quản lý Hệ thống
16. **Quản lý Sản phẩm:** Giao diện quản lý sản phẩm cần cho phép Admin thực hiện những thao tác nào? (Ví dụ: thêm/sửa/xóa sản phẩm, cập nhật giá, quản lý số lượng tồn kho,...)
17. **Quản lý Nghệ sĩ/Thể loại:** Admin có cần một mục riêng để quản lý danh sách các nghệ sĩ và thể loại nhạc không?
18. **Quản lý Đơn hàng:** Quy trình xử lý một đơn hàng từ khi được tạo đến khi hoàn thành sẽ bao gồm những bước nào? (Ví dụ: Chờ xác nhận → Đã xác nhận → Đang giao hàng → Đã giao → Hủy)
19. **Xem thông tin Khách hàng:** Admin có cần xem danh sách khách hàng và lịch sử mua hàng của họ không? Admin có được quyền chỉnh sửa thông tin của khách hàng không?
20. **Báo cáo & Thống kê:** Admin cần xem những loại báo cáo nào để theo dõi tình hình kinh doanh? (Ví dụ: tổng doanh thu theo ngày/tháng, sản phẩm bán chạy nhất, lượng đơn hàng,...)
21. **Quản lý Mã giảm giá:** Hệ thống có cần chức năng tạo và quản lý các mã giảm giá (vouchers/coupons) không?

---

## Phần C: Yêu Cầu Phi Chức Năng

*Phần này làm rõ các yếu tố không liên quan trực tiếp đến tính năng nhưng rất quan trọng đối với trải nghiệm và sự ổn định của website.*

22. **Thiết kế & Giao diện:** Bạn có định hướng về phong cách thiết kế cho website không? (Ví dụ: cổ điển (vintage), tối giản (minimalist), hiện đại,...)
23. **Tốc độ tải trang:** Bạn có yêu cầu cụ thể nào về tốc độ tải trang không? (Ví dụ: trang chủ phải tải xong trong vòng 3 giây)
24. **Tính tương thích:** Website có cần hiển thị tốt trên các thiết bị khác nhau như máy tính bảng và điện thoại di động không (Responsive Design)?
25. **Bảo mật:** Có yêu cầu đặc biệt nào về việc bảo mật thông tin cá nhân của khách hàng và dữ liệu giao dịch không?

---

## Phần D: Yêu Cầu về Kỹ Thuật và Vận Hành

*Phần này giúp xác định các vấn đề liên quan đến kỹ thuật, ngân sách và kế hoạch dài hạn.*

26. **Tên miền và Hosting:** Bạn đã sở hữu tên miền (domain) và dịch vụ lưu trữ (hosting) cho website chưa?
27. **Ngân sách:** Ngân sách dự kiến cho việc phát triển website này là bao nhiêu?
28. **Bảo trì & Cập nhật:** Sau khi website đi vào hoạt động, ai sẽ là người chịu trách nhiệm cập nhật sản phẩm, bài viết và bảo trì kỹ thuật cho website?
