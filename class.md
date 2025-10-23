# Class Design - Rill (Website Bán Đĩa Than Online)
*Tài liệu thiết kế class để vẽ Class Diagram (Phiên bản đơn giản hóa)*

## 1. Tổng quan Class

Hệ thống Rill bao gồm 9 class chính được chia thành các nhóm:
- **User Management**: User, Address
- **Product Management**: Product, Artist, Review
- **Order Management**: Order, OrderItem
- **Cart Management**: Cart, CartItem

---

## 2. Chi tiết từng Class

### 2.1. User (Người dùng)
**Mục đích**: Quản lý thông tin người dùng hệ thống

**Attributes**:
- `id: int`
- `email: string` {unique}
- `password: string`
- `fullName: string`
- `phone: string`
- `role: UserRole`
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+register(email, password, fullName, phone): User`
- `+login(email, password): boolean`
- `+updateProfile(fullName, phone): void`

**Relationships**:
- 1 User có nhiều Address (1:n)
- 1 User có 1 Cart (1:1)
- 1 User có nhiều Order (1:n)

---

### 2.2. Address (Địa chỉ)
**Mục đích**: Quản lý địa chỉ giao hàng của khách hàng

**Attributes**:
- `id: int`
- `userId: int` {FK}
- `fullName: string`
- `phone: string`
- `address: string`
- `city: string`
- `district: string`
- `ward: string`
- `isDefault: boolean`

**Methods**:
- `+create(user: User, addressInfo: map): Address`
- `+setAsDefault(): void`
- `+update(addressInfo: map): void`

**Relationships**:
- Nhiều Address thuộc về 1 User (n:1)

---

### 2.3. Artist (Nghệ sĩ)
**Mục đích**: Quản lý thông tin nghệ sĩ

**Attributes**:
- `id: int`
- `name: string`
- `bio: text`
- `image: string`

**Methods**:
- `+create(name, bio, image): Artist`
- `+update(name, bio, image): void`
- `+getProducts(): Product[]`

**Relationships**:
- 1 Artist có nhiều Product (1:n)

---

### 2.4. Product (Sản phẩm)
**Mục đích**: Quản lý thông tin sản phẩm đĩa than

**Attributes**:
- `id: int`
- `artistId: int` {FK}
- `title: string`
- `genre: string`
- `label: string`
- `releaseYear: int`
- `price: decimal`
- `salePrice: decimal` {nullable}
- `stockQuantity: int`
- `description: text`

**Methods**:
- `+create(productInfo: map): Product`
- `+update(productInfo: map): void`
- `+decreaseStock(quantity: int): boolean`
- `+getCurrentPrice(): decimal`
- `+isInStock(quantity: int): boolean`
- `+findTopSellingProducts(limit: int): Product[]`

**Relationships**:
- Nhiều Product thuộc về 1 Artist (n:1)
- 1 Product có nhiều CartItem (1:n)
- 1 Product có nhiều OrderItem (1:n)

---

### 2.5. Cart (Giỏ hàng)
**Mục đích**: Quản lý giỏ hàng của khách hàng

**Attributes**:
- `id: int`
- `userId: int` {FK}

**Methods**:
- `+create(user: User): Cart`
- `+addItem(product: Product, quantity: int): void`
- `+removeItem(product: Product): void`
- `+updateItemQuantity(product: Product, quantity: int): void`
- `+getTotalAmount(): decimal`
- `+clear(): void`

**Relationships**:
- 1 Cart thuộc về 1 User (1:1)
- 1 Cart có nhiều CartItem (1:n)

---

### 2.6. CartItem (Item trong giỏ hàng)
**Mục đích**: Quản lý từng sản phẩm trong giỏ hàng

**Attributes**:
- `id: int`
- `cartId: int` {FK}
- `productId: int` {FK}
- `quantity: int`

**Methods**:
- `+create(cart: Cart, product: Product, quantity: int): CartItem`
- `+getSubTotal(): decimal`

**Relationships**:
- Nhiều CartItem thuộc về 1 Cart (n:1)
- Nhiều CartItem thuộc về 1 Product (n:1)

---

### 2.7. Order (Đơn hàng)
**Mục đích**: Quản lý đơn hàng của khách hàng

**Attributes**:
- `id: int`
- `userId: int` {FK}
- `orderNumber: string` {unique}
- `status: OrderStatus`
- `shippingAddress: json`
- `subtotal: decimal`
- `shippingFee: decimal`
- `totalAmount: decimal`
- `notes: text` {nullable}
- `paymentMethod: PaymentMethod`
- `paymentStatus: PaymentStatus`
- `paidAt: datetime` {nullable}
- `createdAt: datetime`

**Methods**:
- `+create(customer: User, cart: Cart, shippingAddress: Address): Order`
- `+confirmByAdmin(admin: User): void`
- `+cancel(): void`
- `+ship(): void`
- `+deliver(): void`
- `+calculateTotalRevenue(): decimal`

**Relationships**:
- Nhiều Order thuộc về 1 User (n:1)
- 1 Order có nhiều OrderItem (1:n)

---

### 2.8. OrderItem (Item trong đơn hàng)
**Mục đích**: Quản lý từng sản phẩm trong đơn hàng

**Attributes**:
- `id: int`
- `orderId: int` {FK}
- `productId: int` {FK}
- `productName: string`
- `productPrice: decimal`
- `quantity: int`

**Methods**:
- `+create(order: Order, product: Product, quantity: int): OrderItem`
- `+calculateSubtotal(): decimal`

**Relationships**:
- Nhiều OrderItem thuộc về 1 Order (n:1)
- Nhiều OrderItem thuộc về 1 Product (n:1)
- 1 OrderItem có 1 Review (1:1) {optional}

---

### 2.9. Review (Đánh giá)
**Mục đích**: Quản lý đánh giá sản phẩm đã mua

**Attributes**:
- `id: int`
- `orderItemId: int` {FK, unique}
- `rating: int` {1-5}
- `comment: text` {nullable}

**Methods**:
- `+create(orderItem: OrderItem, rating: int, comment: string): Review`

**Relationships**:
- 1 Review thuộc về 1 OrderItem (1:1)

---

## 3. Mối quan hệ giữa các Class

### 3.1. Quan hệ chính
```
User (1) ←→ (1) Cart ←→ (n) CartItem ←→ (1) Product
User (1) ←→ (n) Address
User (1) ←→ (n) Order ←→ (n) OrderItem
OrderItem (1) ←→ (1) Review
Artist (1) ←→ (n) Product
```

### 3.2. Multiplicities
- **User - Cart**: 1:1
- **User - Address**: 1:n
- **User - Order**: 1:n
- **Cart - CartItem**: 1:n
- **Order - OrderItem**: 1:n
- **OrderItem - Review**: 1:1
- **Artist - Product**: 1:n

---

## 4. Enums và Constants

- **UserRole**: ADMIN, CUSTOMER
- **OrderStatus**: PENDING, CONFIRMED, SHIPPING, DELIVERED, CANCELLED
- **PaymentStatus**: PENDING, COMPLETED, CANCELLED
- **PaymentMethod**: COD

---

## 5. Ghi chú cho Class Diagram

### 5.1. Key Constraints
- `User.email` phải unique.
- `Order.orderNumber` phải unique.
- `Review.orderItemId` phải unique.
- `Product.stockQuantity` không được âm.

### 5.2. Derived Attributes
- `Order.totalAmount` = subtotal + shippingFee
- `CartItem.subtotal` = quantity * product.getCurrentPrice()
- `OrderItem.subtotal` = quantity * productPrice

---

**Mục đích**: Document này được thiết kế để hỗ trợ vẽ Class Diagram chính xác và đầy đủ cho hệ thống Rill.
