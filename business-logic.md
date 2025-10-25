# Business Logic - Rill (Online Vinyl Record Store)
*System Analysis and Design Document*

## 1. System Overview

### 1.1. Purpose
Rill is an e-commerce system specializing in selling vinyl records online, serving the community of music lovers and vinyl collectors in Vietnam.

### 1.2. System Scope (Simplified for Analysis)
- **Payment**: Only supports COD (Cash on Delivery), payment information is managed within the order.
- **User Roles**: Admin and Customer (2 roles).
- **Inventory Management**: A single, definitive stock check occurs at the moment of placing an order.
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
  - View customer information
  - Update personal information

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

#### UC-02b: Logout
- **Actor**: Customer, Admin
- **Main Flow**:
  1. The user clicks the "Logout" button.
  2. The system destroys the current session.
  3. Redirect to the homepage.

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

#### UC-06b: Manage Artists (Admin)
- **Actor**: Admin
- **Main Flow**:
  1. The admin accesses the "Artist Management" page.
  2. The system displays a list of all artists.
  3. The admin can add a new artist by providing a name, bio, and image.
  4. The admin can edit the information of an existing artist.
  5. The admin can delete an artist.
- **Business Rule**:
  - An artist cannot be deleted if they are associated with any existing products.

---

### 3.3. Cart and Checkout
#### UC-07: Manage Shopping Cart
- **Actor**: Customer
- **Main Flow**:
  1. Add a product to the cart.
  2. Update the quantity of a product.
  3. Remove a product from the cart.
  4. View the total value of the cart.
- **Business Rule**:
  - A customer has only one shopping cart.

#### UC-08: Place Order
- **Actor**: Customer
- **Main Flow**:
  1. The customer proceeds to checkout.
  2. Select a shipping address.
  3. The system performs a definitive real-time stock check. If a product is out of stock, it shows an error and stops the process.
  4. Create an order with the status "Pending Confirmation".
  5. Save the shipping address and payment information (COD) to the order.
  6. Clear the products from the shopping cart.
- **Business Rule**:
  - Stock is not decremented at the time of placing the order.

---

### 3.4. Order Management
#### UC-09: Process Order (Admin)
- **Actor**: Admin
- **Main Flow**:
  1. The admin views the list of orders pending confirmation.
  2. Select an order to view its details.
  3. Confirm the order:
     - Status: "Pending Confirmation" → "Confirmed"
     - Decrement stock quantity.

#### UC-10: Update Shipping Status
- **Actor**: Admin
- **Main Flow**:
  1. The admin updates the order status from "Confirmed" to "Shipping".
  2. The admin later updates the status from "Shipping" to "Delivered".
- **Order Status Flow**:
  ```
  Pending Confirmation → Confirmed → Shipping → Delivered
           ↓
        Cancelled
  ```
- **Rules**:
  - "Shipping" is set when the package is handed over to the shipper.
  - "Delivered" is set when the customer receives the package. The order's payment status automatically updates to "Completed".

#### UC-11: Cancel Order
- **Actor**: Customer, Admin
- **Main Flow**:
  1. The user (Customer or Admin) accesses an order with "Pending Confirmation" status.
  2. The user initiates the cancellation.
  3. The system changes the order status to "Cancelled".
- **Business Rule**:
  - An order can only be cancelled if its status is "Pending Confirmation".

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

### 3.6. Administration
#### UC-13: View Customer Infomation
- **Actor**: Admin
- **Main Flow**:
  1. The admin accesses the "Customer Information" page.
  2. The system displays a list of all customers.
  3. The admin can search for a customer by name or email.
  4. The admin can select a customer to view their detailed information, including their address list and complete order history.
- **Business Rule**:
  - The admin can only view customer information and cannot edit it directly.

---

## 4. Overall Business Rules

### 4.1. Product Rules
- Each product must have one main artist.
- Stock quantity cannot be negative.
- Sale price must be less than the original price.

### 4.2. Order Rules
- An order can only be cancelled from the "Pending Confirmation" status.
- Stock is only decremented when an order is confirmed by an Admin.
- The shipping address is saved to the order at the time of purchase.
- The default payment method is COD, and payment status is managed within the order.

### 4.3. Cart Rules
- Stock is checked definitively at the time of checkout.
- A customer can only have one shopping cart.

### 4.4. User Rules
- Roles are limited to "admin" or "customer".
- Email must be unique.
- Only one address can be set as the default per user.

### 4.5. Review Rules
- A customer can only review a product they have successfully purchased and received.
- Each order item can be reviewed only once.

---

## 5. Main Business Flows

### 5.1. Customer Purchase Flow
```
1. Log in to the system.
2. Browse the product list → Filter/Search.
3. View product details.
4. Add products to the cart.
5. Proceed to checkout.
6. Select a shipping address → Confirm information.
7. Place the order (COD).
8. Wait for admin confirmation (Can self-cancel during this stage).
9. Receive the package.
10. Review the purchased products.
```

### 5.2. Admin Order Processing Flow
```
1. Log in to the admin dashboard.
2. View the list of orders pending confirmation.
3. Select an order to view details.
4. Decide:
   - CONFIRM → Decrement stock, update status.
   - CANCEL → Update status.
5. Update to "Shipping".
6. Update to "Delivered".
```

---

## 6. Data Validation Rules

### 6.1. User
- Email: required, valid email format, unique.
- Password: required.
- Phone Number: required.
- Full Name: required.

### 6.2. Product
- Name: required.
- Price: required, positive number.
- Stock: required, non-negative number.
- Genre: required.
- Label: required.
- Artist: required.

### 6.3. Order
- Shipping Address: required.
- Total Amount: calculated field.
- Status: ENUM ('Pending Confirmation', 'Confirmed', 'Shipping', 'Delivered', 'Cancelled').

### 6.4. Review
- Rating: required, integer from 1 to 5.
- Comment: optional.

---

## 7. System Summary

Rill is designed with the following core components:
- ✅ Vinyl Product Management
- ✅ Shopping Cart and Ordering
- ✅ Order Management (including COD payment)
- ✅ Product Reviews
- ✅ Admin Dashboard

**Objective**: A simple and understandable system to serve as a basis for UML analysis and design.
