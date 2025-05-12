# 🎭 Use Case Diagram – Airbnb Clone Backend

This document provides the **UML Use Case Diagram** for the backend of the Airbnb Clone project. It illustrates the system functionalities and the interactions between system users (actors) and the backend features.

The diagram follows standard UML conventions and is based on the features documented in [Task 0: Features and Functionalities](../features-and-functionalities/README.md).

---

## 👥 Actors

### 1. **Guest**
A user who browses listings and makes bookings.

### 2. **Host**
A registered user who lists properties and manages bookings.
- Inherits from Guest.

### 3. **Admin**
A superuser who manages all users, listings, and payments.
- Inherits from Host.

---

## 🎯 Main Use Cases

### For **Guest**:
- Register Account
- Login
- Search Listings
- Filter Listings
- View Listing Details
- Book Property
- Cancel Property (cancel a booking)
- Make Payment
- Leave Review
- Receive Notifications

### For **Host**:
- Create Property
- Edit Property
- Delete Property
- View Bookings
- Respond to Reviews

### For **Admin**:
- Manage Users
- View All Listings
- View All Bookings
- View All Payments
- Remove Listings

---

## 🔁 Include Relationships

The following use cases include shared or prerequisite functionality:

| Base Use Case       | Included Use Case         |
|---------------------|---------------------------|
| Register Account     | Validate Input            |
| Login                | Validate Credentials      |
| Book Property        | Validate Availability     |
| Book Property        | Make Payment              |
| Cancel Property      | Check Cancellation Policy |
| Leave Review         | Verify Booking Completed  |

All `<<include>>` arrows point **from the base use case ➡️ to the included logic**, as per UML best practices.

---

## 🔐 Authentication Note

A UML note is added to the diagram to clarify:

> “All actions except 'Register' and 'Login' require user to be authenticated.”

This is represented visually using actors (Guest, Host, Admin), and avoids misusing `<<include>>` from secured use cases to `Login`.

---

## 🖼️ Diagram Preview

> See the image file: [use-case-diagram.png](./use-case-diagram.png).