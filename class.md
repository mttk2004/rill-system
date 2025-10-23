# Class Design - Rill (Website Bán Đĩa Than Online)
*Tài liệu thiết kế class để vẽ Class Diagram*

## 1. Tổng quan Class

Hệ thống Rill bao gồm 11 class chính được chia thành các nhóm:
- **User Management**: User, Address, Review
- **Product Management**: Product, Artist, ProductArtist
- **Order Management**: Order, OrderItem, Payment
- **Cart Management**: Cart, CartItem

---

## 2. Chi tiết từng Class

### 2.1. User (Người dùng)
**Mục đích**: Quản lý thông tin người dùng hệ thống

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `email: string` - Email đăng nhập (unique)
- `password: string` - Mật khẩu đã mã hóa
- `fullName: string` - Họ và tên
- `phone: string` - Số điện thoại
- `role: enum` - Vai trò ('admin', 'customer')
- `createdAt: datetime` - Thời gian tạo tài khoản
- `updatedAt: datetime` - Thời gian cập nhật cuối

**Methods**:
- `+register(email, password, fullName, phone): User` - Đăng ký tài khoản mới
- `+login(email, password): boolean` - Đăng nhập
- `+updateProfile(fullName, phone): void` - Cập nhật thông tin cá nhân
- `+isAdmin(): boolean` - Kiểm tra có phải admin
- `+isCustomer(): boolean` - Kiểm tra có phải customer

**Relationships**:
- 1 User có nhiều Address (1:n)
- 1 User có 1 Cart (1:1)
- 1 User có nhiều Order (1:n)
- 1 User có nhiều Review (1:n)

---

### 2.2. Address (Địa chỉ)
**Mục đích**: Quản lý địa chỉ giao hàng của khách hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `userId: int` - Mã người dùng (FK)
- `fullName: string` - Tên người nhận
- `phone: string` - Số điện thoại người nhận
- `address: string` - Địa chỉ chi tiết
- `city: string` - Thành phố
- `district: string` - Quận/huyện
- `ward: string` - Phường/xã
- `isDefault: boolean` - Địa chỉ mặc định
- `createdAt: datetime` - Thời gian tạo
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(userId, addressInfo): Address` - Tạo địa chỉ mới
- `+setAsDefault(): void` - Đặt làm địa chỉ mặc định
- `+update(addressInfo): void` - Cập nhật thông tin địa chỉ
- `+delete(): void` - Xóa địa chỉ
- `+getFullAddress(): string` - Lấy địa chỉ đầy đủ

**Relationships**:
- Nhiều Address thuộc về 1 User (n:1)

---

### 2.3. Artist (Nghệ sĩ)
**Mục đích**: Quản lý thông tin nghệ sĩ

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `name: string` - Tên nghệ sĩ
- `bio: text` - Tiểu sử nghệ sĩ
- `image: string` - URL hình ảnh
- `createdAt: datetime` - Thời gian tạo
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(name, bio, image): Artist` - Tạo nghệ sĩ mới
- `+update(name, bio, image): void` - Cập nhật thông tin
- `+delete(): void` - Xóa nghệ sĩ
- `+getProducts(): Product[]` - Lấy danh sách sản phẩm

**Relationships**:
- Nhiều Artist có nhiều Product (m:n qua ProductArtist)

---

### 2.4. Product (Sản phẩm)
**Mục đích**: Quản lý thông tin sản phẩm đĩa than

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `title: string` - Tên album
- `genre: string` - Thể loại nhạc
- `label: string` - Hãng đĩa
- `releaseYear: int` - Năm phát hành
- `price: decimal` - Giá bán
- `salePrice: decimal` - Giá khuyến mãi (nullable)
- `stockQuantity: int` - Số lượng tồn kho
- `description: text` - Mô tả chi tiết
- `image: string` - URL hình ảnh
- `createdAt: datetime` - Thời gian tạo
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(productInfo): Product` - Tạo sản phẩm mới
- `+update(productInfo): void` - Cập nhật thông tin
- `+updateStock(quantity): void` - Cập nhật tồn kho
- `+decreaseStock(quantity): boolean` - Giảm tồn kho
- `+increaseStock(quantity): void` - Tăng tồn kho
- `+getCurrentPrice(): decimal` - Lấy giá hiện tại (ưu tiên sale price)
- `+isInStock(quantity): boolean` - Kiểm tra còn hàng
- `+delete(): void` - Xóa sản phẩm
- `+findTopSellingProducts(limit: int): Product[]` - Tìm sản phẩm bán chạy

**Relationships**:
- Nhiều Product có nhiều Artist (m:n qua ProductArtist)
- 1 Product có nhiều CartItem (1:n)
- 1 Product có nhiều OrderItem (1:n)
- 1 Product có nhiều Review (1:n)

---

### 2.5. ProductArtist (Liên kết Sản phẩm - Nghệ sĩ)
**Mục đích**: Quản lý mối quan hệ nhiều-nhiều giữa Product và Artist

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `productId: int` - Mã sản phẩm (FK)
- `artistId: int` - Mã nghệ sĩ (FK)
- `role: string` - Vai trò ('main', 'featured', 'composer', 'producer')
- `createdAt: datetime` - Thời gian tạo

**Methods**:
- `+create(productId, artistId, role): ProductArtist` - Tạo liên kết mới
- `+delete(): void` - Xóa liên kết

**Relationships**:
- Nhiều ProductArtist thuộc về 1 Product (n:1)
- Nhiều ProductArtist thuộc về 1 Artist (n:1)

---

### 2.6. Cart (Giỏ hàng)
**Mục đích**: Quản lý giỏ hàng của khách hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `userId: int` - Mã người dùng (FK)
- `createdAt: datetime` - Thời gian tạo
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(userId): Cart` - Tạo giỏ hàng mới
- `+addItem(productId, quantity): CartItem` - Thêm sản phẩm
- `+removeItem(productId): void` - Xóa sản phẩm
- `+updateItemQuantity(productId, quantity): void` - Cập nhật số lượng
- `+getTotalAmount(): decimal` - Tính tổng giá trị
- `+getItemCount(): int` - Đếm số sản phẩm
- `+clear(): void` - Xóa toàn bộ giỏ hàng
- `+validateStock(): boolean` - Kiểm tra tồn kho

**Relationships**:
- 1 Cart thuộc về 1 User (1:1)
- 1 Cart có nhiều CartItem (1:n)

---

### 2.7. CartItem (Item trong giỏ hàng)
**Mục đích**: Quản lý từng sản phẩm trong giỏ hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `cartId: int` - Mã giỏ hàng (FK)
- `productId: int` - Mã sản phẩm (FK)
- `quantity: int` - Số lượng
- `createdAt: datetime` - Thời gian thêm vào giỏ
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(cartId, productId, quantity): CartItem` - Tạo item mới
- `+updateQuantity(quantity): void` - Cập nhật số lượng
- `+getSubTotal(): decimal` - Tính thành tiền
- `+delete(): void` - Xóa khỏi giỏ hàng

**Relationships**:
- Nhiều CartItem thuộc về 1 Cart (n:1)
- Nhiều CartItem thuộc về 1 Product (n:1)

---

### 2.8. Order (Đơn hàng)
**Mục đích**: Quản lý đơn hàng của khách hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `userId: int` - Mã khách hàng (FK)
- `orderNumber: string` - Số đơn hàng (unique)
- `status: enum` - Trạng thái ('Chờ xác nhận', 'Đã xác nhận', 'Đang giao', 'Đã giao', 'Đã hủy')
- `shippingAddress: json` - Địa chỉ giao hàng (snapshot)
- `subtotal: decimal` - Tổng tiền hàng
- `shippingFee: decimal` - Phí vận chuyển
- `totalAmount: decimal` - Tổng thanh toán
- `notes: text` - Ghi chú đơn hàng
- `createdAt: datetime` - Thời gian đặt hàng
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(userId, cartItems, addressId): Order` - Tạo đơn hàng từ giỏ
- `+updateStatus(newStatus): void` - Cập nhật trạng thái
- `+confirm(): void` - Xác nhận đơn hàng (trừ stock)
- `+cancel(reason): void` - Hủy đơn hàng
- `+ship(): void` - Chuyển trạng thái đang giao
- `+deliver(): void` - Hoàn thành giao hàng
- `+calculateTotal(): decimal` - Tính tổng tiền
- `+canBeCancelled(): boolean` - Kiểm tra có thể hủy
- `+calculateTotalRevenue(): decimal` - Tính tổng doanh thu

**Relationships**:
- Nhiều Order thuộc về 1 User (n:1)
- 1 Order có nhiều OrderItem (1:n)
- 1 Order có 1 Payment (1:1)

---

### 2.9. OrderItem (Item trong đơn hàng)
**Mục đích**: Quản lý từng sản phẩm trong đơn hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `orderId: int` - Mã đơn hàng (FK)
- `productId: int` - Mã sản phẩm (FK)
- `productName: string` - Tên sản phẩm (snapshot)
- `productPrice: decimal` - Giá sản phẩm (snapshot)
- `quantity: int` - Số lượng
- `subtotal: decimal` - Thành tiền
- `createdAt: datetime` - Thời gian tạo

**Methods**:
- `+create(orderId, productInfo, quantity): OrderItem` - Tạo item từ product
- `+calculateSubtotal(): decimal` - Tính thành tiền

**Relationships**:
- Nhiều OrderItem thuộc về 1 Order (n:1)
- Nhiều OrderItem thuộc về 1 Product (n:1)

---

### 2.10. Payment (Thanh toán)
**Mục đích**: Quản lý thông tin thanh toán

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `orderId: int` - Mã đơn hàng (FK)
- `method: string` - Phương thức thanh toán ('cod')
- `status: enum` - Trạng thái ('Chờ', 'Hoàn tất', 'Hủy')
- `amount: decimal` - Số tiền thanh toán
- `paidAt: datetime` - Thời gian thanh toán (nullable)
- `createdAt: datetime` - Thời gian tạo
- `updatedAt: datetime` - Thời gian cập nhật

**Methods**:
- `+create(orderId, amount): Payment` - Tạo thanh toán COD
- `+complete(): void` - Hoàn tất thanh toán
- `+cancel(): void` - Hủy thanh toán
- `+isPending(): boolean` - Kiểm tra đang chờ
- `+isCompleted(): boolean` - Kiểm tra đã hoàn tất

**Relationships**:
- 1 Payment thuộc về 1 Order (1:1)

---

### 2.11. Review (Đánh giá)
**Mục đích**: Quản lý đánh giá sản phẩm của khách hàng

**Attributes**:
- `id: int` - Mã định danh duy nhất
- `userId: int` - Mã người dùng (FK)
- `productId: int` - Mã sản phẩm (FK)
- `rating: int` - Điểm đánh giá (1-5)
- `comment: text` - Nội dung bình luận
- `createdAt: datetime` - Thời gian tạo

**Methods**:
- `+create(userId, productId, rating, comment): Review` - Tạo đánh giá mới
- `+update(rating, comment): void` - Cập nhật đánh giá
- `+delete(): void` - Xóa đánh giá

**Relationships**:
- Nhiều Review thuộc về 1 User (n:1)
- Nhiều Review thuộc về 1 Product (n:1)

---

## 3. Mối quan hệ giữa các Class

### 3.1. Quan hệ chính
```
User (1) ←→ (1) Cart ←→ (n) CartItem ←→ (1) Product
User (1) ←→ (n) Address
User (1) ←→ (n) Review ←→ (1) Product
User (1) ←→ (n) Order ←→ (n) OrderItem ←→ (1) Product
Order (1) ←→ (1) Payment
Product (n) ←→ (n) Artist (qua ProductArtist)
```

### 3.2. Multiplicities
- **User - Cart**: 1:1 (mỗi user có 1 giỏ hàng)
- **User - Address**: 1:n (user có nhiều địa chỉ)
- **User - Order**: 1:n (user có nhiều đơn hàng)
- **User - Review**: 1:n (user có nhiều đánh giá)
- **Cart - CartItem**: 1:n (giỏ hàng có nhiều item)
- **Order - OrderItem**: 1:n (đơn hàng có nhiều item)
- **Product - Artist**: n:m (sản phẩm có nhiều nghệ sĩ, nghệ sĩ có nhiều sản phẩm)
- **Product - Review**: 1:n (sản phẩm có nhiều đánh giá)
- **Order - Payment**: 1:1 (đơn hàng có 1 thanh toán)

---

## 4. Enums và Constants

### 4.1. UserRole
```
enum UserRole {
    ADMIN = "admin"
    CUSTOMER = "customer"
}
```

### 4.2. OrderStatus
```
enum OrderStatus {
    PENDING = "Chờ xác nhận"
    CONFIRMED = "Đã xác nhận"
    SHIPPING = "Đang giao"
    DELIVERED = "Đã giao"
    CANCELLED = "Đã hủy"
}
```

### 4.3. PaymentStatus
```
enum PaymentStatus {
    PENDING = "Chờ"
    COMPLETED = "Hoàn tất"
    CANCELLED = "Hủy"
}
```

### 4.4. PaymentMethod
```
enum PaymentMethod {
    COD = "cod"
}
```

### 4.5. ArtistRole
```
enum ArtistRole {
    MAIN = "main"
    FEATURED = "featured"
    COMPOSER = "composer"
    PRODUCER = "producer"
}
```

---

## 5. Ghi chú cho Class Diagram

### 5.1. Quy tắc thiết kế
- **Abstract Classes**: Không có (hệ thống đơn giản)
- **Interfaces**: Không cần thiết cho scope hiện tại
- **Inheritance**: Không sử dụng kế thừa để đơn giản hóa
- **Composition vs Aggregation**:
  - Composition: Order-OrderItem, Cart-CartItem
  - Aggregation: User-Address, User-Order, User-Review

### 5.2. Key Constraints
- Email trong User phải unique
- Order number phải unique
- Chỉ 1 Address có thể là default per User
- Stock quantity không được âm
- Sale price phải nhỏ hơn regular price

### 5.3. Derived Attributes
- `Order.totalAmount` = subtotal + shippingFee
- `CartItem.subtotal` = quantity * product.getCurrentPrice()
- `OrderItem.subtotal` = quantity * productPrice

---

**Mục đích**: Document này được thiết kế để hỗ trợ vẽ Class Diagram chính xác và đầy đủ cho hệ thống Rill.
