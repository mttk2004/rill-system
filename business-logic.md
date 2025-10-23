# Business Logic - Rill (Website Bán Đĩa Than Online)
*Tài liệu phân tích thiết kế hệ thống*

## 1. Tổng quan hệ thống

### 1.1. Mục đích
Rill là hệ thống thương mại điện tử chuyên bán đĩa than (vinyl records) trực tuyến, phục vụ cộng đồng người yêu nhạc và sưu tầm đĩa than tại Việt Nam.

### 1.2. Phạm vi hệ thống (Đơn giản hóa cho phân tích)
- **Thanh toán**: Chỉ hỗ trợ COD (Cash on Delivery), thông tin thanh toán được quản lý trong đơn hàng.
- **Vai trò người dùng**: Admin và Customer (2 vai trò).
- **Quản lý tồn kho**: Kiểm tra stock real-time tại checkout.
- **Nghệ sĩ**: Mỗi sản phẩm chỉ do một nghệ sĩ chính thực hiện.

---

## 2. Actors (Các tác nhân)

### 2.1. Customer (Khách hàng)
- Người dùng đã đăng ký và đăng nhập hệ thống
- **Quyền hạn**:
  - Xem danh sách và chi tiết sản phẩm
  - Thêm/xóa/sửa sản phẩm vào giỏ hàng
  - Đặt hàng và thanh toán COD
  - Hủy đơn hàng của chính mình
  - Quản lý địa chỉ giao hàng
  - Xem lịch sử đơn hàng
  - Cập nhật thông tin cá nhân
  - Đánh giá sản phẩm đã mua

### 2.2. Admin (Quản trị viên)
- Người quản lý toàn bộ hệ thống
- **Quyền hạn**:
  - Quản lý sản phẩm (thêm, sửa, xóa)
  - Quản lý nghệ sĩ (thêm, sửa, xóa)
  - Quản lý đơn hàng (xác nhận, hủy, cập nhật trạng thái)
  - Xem dashboard và thống kê
  - Quản lý thông tin khách hàng

---

## 3. Use Cases chính

### 3.1. Quản lý tài khoản
#### UC-01: Đăng ký tài khoản
- **Actor**: Người dùng mới
- **Luồng chính**:
  1. Truy cập trang đăng ký
  2. Nhập thông tin: email, password, họ tên, số điện thoại
  3. Hệ thống validate dữ liệu
  4. Hệ thống tạo tài khoản với role="customer"
  5. Chuyển hướng đến trang đăng nhập

#### UC-02: Đăng nhập
- **Actor**: Customer, Admin
- **Luồng chính**:
  1. Nhập email và password
  2. Hệ thống xác thực thông tin
  3. Tạo phiên đăng nhập
  4. Chuyển hướng đến trang chủ (Customer) hoặc Dashboard (Admin)

#### UC-03: Quản lý địa chỉ giao hàng
- **Actor**: Customer
- **Luồng chính**:
  1. Customer truy cập trang quản lý địa chỉ
  2. Xem danh sách địa chỉ hiện có
  3. Thêm/sửa/xóa địa chỉ
  4. Đặt một địa chỉ làm mặc định
- **Quy tắc nghiệp vụ**:
  - Chỉ 1 địa chỉ được đặt làm mặc định

---

### 3.2. Quản lý sản phẩm
#### UC-04: Xem danh sách sản phẩm
- **Actor**: Customer, Admin
- **Luồng chính**:
  1. Truy cập trang danh sách sản phẩm
  2. Hệ thống hiển thị sản phẩm với phân trang
  3. Lọc theo: thể loại, hãng đĩa, nghệ sĩ, giá
  4. Tìm kiếm theo tên album hoặc nghệ sĩ
  5. Sắp xếp theo: mới nhất, giá

#### UC-05: Xem chi tiết sản phẩm
- **Actor**: Customer, Admin
- **Thông tin hiển thị**:
  - Tên album
  - Nghệ sĩ chính
  - Hãng đĩa (label)
  - Thể loại (genre)
  - Năm phát hành
  - Giá bán, giá khuyến mãi (nếu có)
  - Số lượng tồn kho
  - Mô tả chi tiết
  - Hình ảnh sản phẩm

#### UC-06: Quản lý sản phẩm (Admin)
- **Actor**: Admin
- **Luồng chính**:
  1. Thêm/sửa/xóa sản phẩm
  2. Cập nhật thông tin sản phẩm (tên, giá, mô tả)
  3. Gán một nghệ sĩ cho sản phẩm
  4. Cập nhật số lượng tồn kho
  5. Thiết lập giá và giá khuyến mãi

---

### 3.3. Giỏ hàng và Thanh toán
#### UC-07: Quản lý giỏ hàng
- **Actor**: Customer
- **Luồng chính**:
  1. Thêm sản phẩm vào giỏ hàng
  2. Cập nhật số lượng sản phẩm
  3. Xóa sản phẩm khỏi giỏ hàng
  4. Xem tổng giá trị giỏ hàng
- **Quy tắc nghiệp vụ**:
  - Một khách hàng chỉ có một giỏ hàng
  - Số lượng sản phẩm trong giỏ hàng không được vượt quá tồn kho.

#### UC-08: Đặt hàng
- **Actor**: Customer
- **Luồng chính**:
  1. Customer nhấn "Thanh toán"
  2. Chọn địa chỉ giao hàng
  3. Kiểm tra tồn kho real-time (nếu có sản phẩm hết hàng, báo lỗi)
  4. Tạo đơn hàng với trạng thái "Chờ xác nhận"
  5. Lưu địa chỉ giao hàng và thông tin thanh toán (COD) vào đơn hàng
  6. Xóa sản phẩm khỏi giỏ hàng
- **Quy tắc nghiệp vụ**:
  - Tồn kho chưa bị trừ tại thời điểm đặt hàng

---

### 3.4. Quản lý đơn hàng
#### UC-09: Xử lý đơn hàng (Admin)
- **Actor**: Admin
- **Luồng xác nhận đơn hàng**:
  1. Admin xem danh sách đơn hàng chờ xác nhận.
  2. Chọn đơn hàng để xem chi tiết.
  3. Kiểm tra thông tin khách hàng và tồn kho sản phẩm.
  4. Xác nhận đơn hàng:
     - Trạng thái: "Chờ xác nhận" → "Đã xác nhận"
     - Trừ số lượng tồn kho
- **Luồng hủy đơn hàng (Admin)**:
  1. Admin chọn "Hủy đơn hàng".
  2. Nhập lý do hủy.
  3. Trạng thái: "Chờ xác nhận" → "Đã hủy".
- **Luồng ngoại lệ (Exception Flow)**:
  1. Khi Admin xác nhận, hệ thống kiểm tra lại tồn kho lần cuối.
  2. Nếu có sản phẩm hết hàng, hệ thống báo lỗi và không cho phép xác nhận.
  3. Admin phải hủy đơn hàng hoặc liên hệ khách hàng để xử lý.

#### UC-10: Cập nhật trạng thái giao hàng
- **Actor**: Admin
- **Luồng trạng thái đơn hàng**:
  ```
  Chờ xác nhận → Đã xác nhận → Đang giao → Đã giao
       ↓
    Đã hủy (từ "Chờ xác nhận", bởi Admin hoặc Customer)
  ```
- **Quy tắc**:
  - "Đã xác nhận" → "Đang giao": Khi giao cho shipper
  - "Đang giao" → "Đã giao": Khi khách nhận hàng. Trạng thái thanh toán của đơn hàng tự động cập nhật thành "Hoàn tất".

#### UC-11: Hủy đơn hàng (Customer)
- **Actor**: Customer
- **Luồng chính**:
  1. Customer truy cập trang "Lịch sử đơn hàng".
  2. Chọn đơn hàng đang ở trạng thái "Chờ xác nhận".
  3. Nhấn nút "Hủy đơn hàng".
  4. Hệ thống xác nhận và chuyển trạng thái đơn hàng thành "Đã hủy".
- **Quy tắc nghiệp vụ**:
  - Khách hàng chỉ có thể hủy đơn hàng khi trạng thái là "Chờ xác nhận".

---

### 3.5. Đánh giá sản phẩm
#### UC-12: Đánh giá sản phẩm
- **Actor**: Customer
- **Luồng chính**:
  1. Customer truy cập trang "Lịch sử đơn hàng".
  2. Chọn đơn hàng đã ở trạng thái "Đã giao".
  3. Chọn một sản phẩm trong đơn hàng chưa được đánh giá.
  4. Nhập điểm đánh giá (1-5 sao) và bình luận.
  5. Gửi đánh giá.
- **Quy tắc nghiệp vụ**:
  - Chỉ được đánh giá các sản phẩm trong đơn hàng đã giao thành công.
  - Mỗi sản phẩm trong một đơn hàng (mỗi order item) chỉ được đánh giá một lần.

---

## 4. Quy tắc nghiệp vụ tổng hợp

### 4.1. Quy tắc sản phẩm
- Mỗi sản phẩm phải có một nghệ sĩ chính.
- Tồn kho không được âm.
- Giá khuyến mãi phải nhỏ hơn giá gốc.

### 4.2. Quy tắc đơn hàng
- Đơn hàng chỉ có thể bị hủy từ trạng thái "Chờ xác nhận" (bởi Admin hoặc Customer).
- Tồn kho chỉ bị trừ khi đơn hàng được Admin xác nhận.
- Địa chỉ giao hàng được lưu vào đơn hàng tại thời điểm đặt hàng.
- Phương thức thanh toán mặc định là COD và trạng thái thanh toán được quản lý bên trong đơn hàng.

### 4.3. Quy tắc giỏ hàng
- Kiểm tra tồn kho tại thời điểm thanh toán.
- Một khách hàng chỉ có một giỏ hàng.
- Số lượng cập nhật trong giỏ hàng không được vượt tồn kho.

### 4.4. Quy tắc người dùng
- Vai trò chỉ có 2 loại: "admin" hoặc "customer".
- Email phải duy nhất.
- Chỉ 1 địa chỉ có thể là mặc định.

### 4.5. Quy tắc đánh giá
- Khách hàng chỉ được đánh giá sản phẩm họ đã mua và nhận hàng thành công.
- Mỗi mục sản phẩm trong đơn hàng chỉ được đánh giá một lần duy nhất.

---

## 5. Luồng nghiệp vụ chính

### 5.1. Luồng mua hàng của khách hàng
```
1. Đăng nhập hệ thống
2. Duyệt danh sách sản phẩm → Lọc/Tìm kiếm
3. Xem chi tiết sản phẩm
4. Thêm vào giỏ hàng → Cập nhật số lượng
5. Vào giỏ hàng → Thanh toán
6. Chọn địa chỉ giao hàng → Xác nhận thông tin
7. Đặt hàng (COD)
8. Chờ admin xác nhận (Có thể tự hủy trong giai đoạn này)
9. Nhận hàng
10. Đánh giá sản phẩm đã mua
```

### 5.2. Luồng xử lý đơn hàng của admin
```
1. Đăng nhập dashboard admin
2. Xem danh sách đơn hàng chờ xác nhận
3. Chọn đơn hàng để xem chi tiết
4. Kiểm tra thông tin & tồn kho
5. Quyết định:
   - XÁC NHẬN → Trừ tồn kho, cập nhật trạng thái (Báo lỗi nếu hết hàng)
   - HỦY → Không thay đổi tồn kho, cập nhật trạng thái
6. Cập nhật "Đang giao" khi giao cho shipper
7. Cập nhật "Đã giao" khi khách nhận hàng, thanh toán hoàn tất
```

---

## 6. Quy tắc validate dữ liệu

### 6.1. Người dùng
- Email: bắt buộc, định dạng email, duy nhất
- Password: bắt buộc
- Số điện thoại: bắt buộc
- Họ tên: bắt buộc

### 6.2. Sản phẩm
- Tên: bắt buộc
- Giá: bắt buộc, số dương
- Tồn kho: bắt buộc, số không âm
- Thể loại: bắt buộc
- Hãng đĩa: bắt buộc
- Nghệ sĩ: bắt buộc

### 6.3. Đơn hàng
- Địa chỉ giao hàng: bắt buộc
- Tổng đơn hàng: trường tính toán
- Trạng thái: ENUM ('Chờ xác nhận', 'Đã xác nhận', 'Đang giao', 'Đã giao', 'Đã hủy')

### 6.4. Đánh giá
- Rating: bắt buộc, số nguyên từ 1 đến 5
- Comment: tùy chọn

---

## 7. Tóm tắt hệ thống

Rill được thiết kế đơn giản với các thành phần cốt lõi:
- ✅ Quản lý sản phẩm đĩa than
- ✅ Giỏ hàng và đặt hàng
- ✅ Quản lý đơn hàng (bao gồm thanh toán COD)
- ✅ Đánh giá sản phẩm
- ✅ Dashboard admin

**Mục tiêu**: Hệ thống đơn giản, dễ hiểu để phục vụ việc phân tích và thiết kế UML.
