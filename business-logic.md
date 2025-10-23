# Business Logic - Rill (Online Vinyl Record Store)
*System Analysis and Design Document*

## 1. System Overview

### 1.1. Purpose
Rill is an e-commerce system specializing in selling vinyl records online, serving the community of music lovers and vinyl collectors in Vietnam.

### 1.2. System Scope (Simplified for Analysis)
- **Payment**: Only supports COD (Cash on Delivery), payment information is managed within the order.
- **User Roles**: Admin and Customer (2 roles).
- **Inventory Management**: Real-time stock check at checkout.
- **Artists**: Each product is associated with one main artist.

---

## 2. Actors

### 2.1. Guest
- An unauthenticated user of the system.
- **Permissions**:
  - View list and details of products
  - Search for products
  - Register for a new account

### 2.2. Customer
- A registered and logged-in user.
- **Permissions (inherits from Guest)**:
  - Add/remove/edit products in the shopping cart
  - Place a COD order
  - Cancel their own orders
  - Manage shipping addresses
  - View order history
  - Update personal information
  - Review purchased products

### 2.3. Admin
- The administrator of the entire system.
- **Permissions**:
  - Manage products (add, edit, delete)
  - Manage artists (add, edit, delete)
  - Manage orders (confirm, cancel, update status)
  - View dashboard and statistics
  - Manage customer information

---

## 3. Main Use Cases

### 3.1. Account Management
#### UC-01: Register Account
- **Actor**: Guest
- **Main Flow**:
  1. Access the registration page.
  2. Enter information: email, password, full name, phone number.
  3. The system validates the data.
  4. The system creates an account with the role "customer".
  5. Redirect to the login page.

#### UC-02: Login
- **Actor**: Customer, Admin
- **Main Flow**:
  1. Enter email and password.
  2. The system authenticates the credentials.
  3. A login session is created.
  4. Redirect to the homepage (Customer) or Dashboard (Admin).

#### UC-03: Manage Shipping Addresses
- **Actor**: Customer
- **Main Flow**:
  1. The customer accesses the address management page.
  2. View the list of existing addresses.
  3. Add/edit/delete an address.
  4. Set one address as the default.
- **Business Rule**:
  - Only one address can be set as the default.

---

### 3.2. Product Management
#### UC-04: View Product List
- **Actor**: Guest, Customer, Admin
- **Main Flow**:
  1. Access the product list page.
  2. The system displays products with pagination.
  3. Filter by: genre, label, artist, price.
  4. Search by album name or artist.
  5. Sort by: newest, price.

#### UC-05: View Product Details
- **Actor**: Guest, Customer, Admin
- **Displayed Information**:
  - Album name
  - Main artist
  - Record label
  - Genre
  - Release year
  - Price, sale price (if any)
  - Stock quantity
  - Detailed description
  - Product images

#### UC-06: Manage Products (Admin)
- **Actor**: Admin
- **Main Flow**:
  1. Add/edit/delete a product.
  2. Update product information (name, price, description).
  3. Assign an artist to the product.
  4. Update stock quantity.
  5. Set price and sale price.

---

### 3.3. Cart and Checkout
#### UC-07: Manage Shopping Cart
- **Actor**: Customer
- **Main Flow**:
  1. Add a product to the cart.
  2. Update the quantity of a product.
  3. Remove a product from the cart.
  4. View the total value of the cart.
- **Business Rules**:
  - A customer has only one shopping cart.
  - The quantity of a product in the cart cannot exceed the available stock.

#### UC-08: Place Order
- **Actor**: Customer
- **Main Flow**:
  1. The customer proceeds to checkout.
  2. Select a shipping address.
  3. The system performs a real-time stock check (shows an error if a product is out of stock).
  4. Create an order with the status "Pending Confirmation".
  5. Save the shipping address and payment information (COD) to the order.
  6. Clear the products from the shopping cart.
- **Business Rule**:
  - Stock is not decremented at the time of placing the order.

---

### 3.4. Order Management
#### UC-09: Process Order (Admin)
- **Actor**: Admin
- **Confirmation Flow**:
  1. The admin views the list of orders pending confirmation.
  2. Select an order to view its details.
  3. Check customer information and product stock.
  4. Confirm the order:
     - Status: "Pending Confirmation" → "Confirmed"
     - Decrement stock quantity.
- **Cancellation Flow (Admin)**:
  1. The admin selects "Cancel Order".
  2. Enter a reason for cancellation.
  3. Status: "Pending Confirmation" → "Cancelled".
- **Exception Flow**:
  1. When the admin confirms, the system performs a final stock check.
  2. If a product is out of stock, the system shows an error and prevents confirmation.
  3. The admin must either cancel the order or contact the customer.

#### UC-10: Update Shipping Status
- **Actor**: Admin
- **Order Status Flow**:
  ```
  Pending Confirmation → Confirmed → Shipping → Delivered
           ↓
        Cancelled (from "Pending Confirmation", by Admin or Customer)
  ```
- **Rules**:
  - "Confirmed" → "Shipping": When the package is handed over to the shipper.
  - "Shipping" → "Delivered": When the customer receives the package. The order's payment status is automatically updated to "Completed".

#### UC-11: Cancel Order (Customer)
- **Actor**: Customer
- **Main Flow**:
  1. The customer accesses the "Order History" page.
  2. Select an order with the status "Pending Confirmation".
  3. Click the "Cancel Order" button.
  4. The system confirms and changes the order status to "Cancelled".
- **Business Rule**:
  - A customer can only cancel an order if its status is "Pending Confirmation".

---

### 3.5. Product Review
#### UC-12: Review Product
- **Actor**: Customer
- **Main Flow**:
  1. The customer accesses the "Order History" page.
  2. Select an order with the status "Delivered".
  3. Choose a product within the order that has not yet been reviewed.
  4. Enter a rating (1-5 stars) and a comment.
  5. Submit the review.
- **Business Rules**:
  - Only products from successfully delivered orders can be reviewed.
  - Each item in an order (each order item) can only be reviewed once.

---

## 4. Business Rules Summary

... (The rest of the file remains the same, as it's already mostly in English-friendly terms or technical descriptions)
