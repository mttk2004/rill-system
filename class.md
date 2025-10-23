# Class Design - Rill (Website Bán Đĩa Than Online)
*Tài liệu thiết kế class để vẽ Class Diagram*

## 1. Tổng quan Class

Hệ thống Rill bao gồm 11 class chính được chia thành các nhóm:
- **User Management**: User, Address
- **Product Management**: Product, Artist, ProductArtist, Review
- **Order Management**: Order, OrderItem, Payment
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
- `+isAdmin(): boolean`
- `+isCustomer(): boolean`

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
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(user: User, addressInfo: map): Address`
- `+setAsDefault(): void`
- `+update(addressInfo: map): void`
- `+delete(): void`
- `+getFullAddress(): string`

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
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(name, bio, image): Artist`
- `+update(name, bio, image): void`
- `+delete(): void`
- `+getProducts(): Product[]`

**Relationships**:
- Nhiều Artist có nhiều Product (m:n qua ProductArtist)

---

### 2.4. Product (Sản phẩm)
**Mục đích**: Quản lý thông tin sản phẩm đĩa than

**Attributes**:
- `id: int`
- `title: string`
- `genre: string`
- `label: string`
- `releaseYear: int`
- `price: decimal`
- `salePrice: decimal` {nullable}
- `stockQuantity: int`
- `description: text`
- `image: string`
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(productInfo: map): Product`
- `+update(productInfo: map): void`
- `+updateStock(quantity: int): void`
- `+decreaseStock(quantity: int): boolean`
- `+increaseStock(quantity: int): void`
- `+getCurrentPrice(): decimal`
- `+isInStock(quantity: int): boolean`
- `+delete(): void`
- `+findTopSellingProducts(limit: int): Product[]`

**Relationships**:
- Nhiều Product có nhiều Artist (m:n qua ProductArtist)
- 1 Product có nhiều CartItem (1:n)
- 1 Product có nhiều OrderItem (1:n)

---

### 2.5. ProductArtist (Liên kết Sản phẩm - Nghệ sĩ)
**Mục đích**: Quản lý mối quan hệ nhiều-nhiều giữa Product và Artist

**Attributes**:
- `id: int`
- `productId: int` {FK}
- `artistId: int` {FK}
- `role: ArtistRole`
- `createdAt: datetime`

**Methods**:
- `+create(product: Product, artist: Artist, role: ArtistRole): ProductArtist`
- `+delete(): void`

**Relationships**:
- Nhiều ProductArtist thuộc về 1 Product (n:1)
- Nhiều ProductArtist thuộc về 1 Artist (n:1)

---

### 2.6. Cart (Giỏ hàng)
**Mục đích**: Quản lý giỏ hàng của khách hàng

**Attributes**:
- `id: int`
- `userId: int` {FK}
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(user: User): Cart`
- `+addItem(product: Product, quantity: int): CartItem`
- `+removeItem(product: Product): void`
- `+updateItemQuantity(product: Product, quantity: int): void`
- `+getTotalAmount(): decimal`
- `+getItemCount(): int`
- `+clear(): void`
- `+validateStock(): boolean`

**Relationships**:
- 1 Cart thuộc về 1 User (1:1)
- 1 Cart có nhiều CartItem (1:n)

---

### 2.7. CartItem (Item trong giỏ hàng)
**Mục đích**: Quản lý từng sản phẩm trong giỏ hàng

**Attributes**:
- `id: int`
- `cartId: int` {FK}
- `productId: int` {FK}
- `quantity: int`
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(cart: Cart, product: Product, quantity: int): CartItem`
- `+updateQuantity(newQuantity: int): void`
- `+getSubTotal(): decimal`
- `+delete(): void`

**Relationships**:
- Nhiều CartItem thuộc về 1 Cart (n:1)
- Nhiều CartItem thuộc về 1 Product (n:1)

---

### 2.8. Order (Đơn hàng)
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
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(customer: User, cart: Cart, shippingAddress: Address): Order`
- `+updateStatus(newStatus: OrderStatus): void`
- `+confirmByAdmin(admin: User): boolean`
- `+cancelByAdmin(admin: User, reason: string): void`
- `+cancelByCustomer(customer: User): void`
- `+ship(): void`
- `+deliver(): void`
- `+calculateTotal(): decimal`
- `+canBeCancelled(): boolean`
- `+calculateTotalRevenue(): decimal`

**Relationships**:
- Nhiều Order thuộc về 1 User (n:1)
- 1 Order có nhiều OrderItem (1:n)
- 1 Order có 1 Payment (1:1)

---

### 2.9. OrderItem (Item trong đơn hàng)
**Mục đích**: Quản lý từng sản phẩm trong đơn hàng

**Attributes**:
- `id: int`
- `orderId: int` {FK}
- `productId: int` {FK}
- `productName: string`
- `productPrice: decimal`
- `quantity: int`
- `subtotal: decimal`
- `createdAt: datetime`

**Methods**:
- `+create(order: Order, product: Product, quantity: int): OrderItem`
- `+calculateSubtotal(): decimal`

**Relationships**:
- Nhiều OrderItem thuộc về 1 Order (n:1)
- Nhiều OrderItem thuộc về 1 Product (n:1)
- 1 OrderItem có 1 Review (1:1) {optional}

---

### 2.10. Payment (Thanh toán)
**Mục đích**: Quản lý thông tin thanh toán

**Attributes**:
- `id: int`
- `orderId: int` {FK}
- `method: PaymentMethod`
- `status: PaymentStatus`
- `amount: decimal`
- `paidAt: datetime` {nullable}
- `createdAt: datetime`
- `updatedAt: datetime`

**Methods**:
- `+create(order: Order, amount: decimal): Payment`
- `+complete(): void`
- `+cancel(): void`
- `+isPending(): boolean`
- `+isCompleted(): boolean`

**Relationships**:
- 1 Payment thuộc về 1 Order (1:1)

---

### 2.11. Review (Đánh giá)
**Mục đích**: Quản lý đánh giá sản phẩm đã mua

**Attributes**:
- `id: int`
- `orderItemId: int` {FK, unique}
- `rating: int` {1-5}
- `comment: text` {nullable}
- `createdAt: datetime`

**Methods**:
- `+create(orderItem: OrderItem, rating: int, comment: string): Review`
- `+update(rating: int, comment: string): void`
- `+delete(): void`

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
OrderItem (n) ←→ (1) Product
Order (1) ←→ (1) Payment
Product (n) ←→ (n) Artist (qua ProductArtist)
```

### 3.2. Multiplicities
- **User - Cart**: 1:1
- **User - Address**: 1:n
- **User - Order**: 1:n
- **Cart - CartItem**: 1:n
- **Order - OrderItem**: 1:n
- **OrderItem - Review**: 1:1 (Một mục đơn hàng chỉ có một đánh giá)
- **Product - Artist**: n:m
- **Order - Payment**: 1:1

---

## 4. Enums và Constants

### 4.1. UserRole
- ADMIN, CUSTOMER

### 4.2. OrderStatus
- PENDING, CONFIRMED, SHIPPING, DELIVERED, CANCELLED

### 4.3. PaymentStatus
- PENDING, COMPLETED, CANCELLED

### 4.4. PaymentMethod
- COD

### 4.5. ArtistRole
- MAIN, FEATURED, COMPOSER, PRODUCER

---

## 5. Ghi chú cho Class Diagram

### 5.1. Quy tắc thiết kế
- **Composition**: Order-OrderItem, Cart-CartItem
- **Aggregation**: User-Address, User-Order

### 5.2. Key Constraints
- `User.email` phải unique.
- `Order.orderNumber` phải unique.
- `Review.orderItemId` phải unique.
- Chỉ 1 `Address` có thể là default cho mỗi User.
- `Product.stockQuantity` không được âm.
- `Product.salePrice` phải nhỏ hơn `price`.

### 5.3. Derived Attributes
- `Order.totalAmount` = subtotal + shippingFee
- `CartItem.subtotal` = quantity * product.getCurrentPrice()
- `OrderItem.subtotal` = quantity * productPrice

---

**Mục đích**: Document này được thiết kế để hỗ trợ vẽ Class Diagram chính xác và đầy đủ cho hệ thống Rill.
