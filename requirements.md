# Backend Feature Requirements – Airbnb Clone

This document specifies the technical and functional requirements for core backend features in the Airbnb Clone application, based on the approved database specification.

---

## 🔐 1. User Authentication

### 📌 Description
Enables users to register, log in, and manage access to the platform securely.

### 🔗 Endpoints

| Method | Endpoint           | Description            |
|--------|--------------------|------------------------|
| POST   | /api/auth/register | Register a new account |
| POST   | /api/auth/login    | Login with credentials |

### 🧾 Input / Output

#### Registration Input (JSON)
```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "jane@example.com",
  "password": "StrongPass123",
  "country_code": "+237",
  "phone_number": "1234567890",
  "role": "guest"
}
```

#### Login Input (JSON)
```json
{
  "email": "jane@example.com",
  "password": "StrongPass123"
}
```

#### Successful Login Output
```json
{
  "access_token": "access_token",
  "refresh_token": "refresh_token",
  "user": {
    "user_id": "uuid",
    "role": "guest",
    "first_name": "Jane",
    "last_name": "Doe"
  }
}
```

### ✅ Validation Rules
- Email must be valid and unique
- Password: minimum 8 characters
- `role` must be one of `guest`, `host`, or `admin`
- Required fields: first_name, last_name, email, password, role

### 🔐 Security
- Use JWT for authentication
- Passwords hashed with bcrypt
- Rate limiting and brute-force protection on login
- Refresh tokens must be securely stored on the client

---

## 🏠 2. Property Management

### 📌 Description
Allows hosts to manage property listings including create, update, and delete operations.

### 🔗 Endpoints

| Method | Endpoint              | Description                    |
|--------|-----------------------|--------------------------------|
| POST   | /api/properties       | Create a new property          |
| PUT    | /api/properties/{id}  | Update an existing property    |
| DELETE | /api/properties/{id}  | Delete a property              |
| GET    | /api/properties       | Get list of properties         |
| GET    | /api/properties/{id}  | Get single property by ID      |

### 🧾 Input (POST/PUT)
```json
{
  "name": "Sunny Apartment",
  "description": "2-bedroom, WiFi included",
  "location": "Downtown",
  "pricepernight": 85.50
}
```

### ✅ Validation Rules
- Fields `name`, `description`, `location`, `pricepernight` must not be null
- `pricepernight` must be a positive decimal
- Only authenticated users with role `host` can create or update properties

### 📊 Performance
- Indexed by `host_id` and `location`
- Search and pagination supported on GET

---

## 📅 3. Booking System

### 📌 Description
Guests can book available properties and manage their reservations.

### 🔗 Endpoints

| Method | Endpoint              | Description                     |
|--------|-----------------------|---------------------------------|
| POST   | /api/bookings         | Create a new booking            |
| GET    | /api/bookings         | List bookings for current user  |
| GET    | /api/bookings/{id}    | View details of a booking       |
| DELETE | /api/bookings/{id}    | Cancel a booking                |

### 🧾 Input (POST)
```json
{
  "property_id": "uuid",
  "start_date": "2025-08-01",
  "end_date": "2025-08-04"
}
```

### ✅ Validation Rules
- `start_date` must be before `end_date`
- Dates must not overlap existing bookings for the same property
- User must not book their own property
- `status` is set to `pending` by default

### 🔐 Integrity
- Foreign keys: property_id (Property), user_id (User)
- Enum constraint: `status` must be one of `pending`, `confirmed`, `canceled`
- Indexed by `property_id`, `user_id`

---

## 📌 Notes
- All routes (except `/register` and `/login`) require JWT authentication
- Roles: `guest`, `host`, and `admin` determine access rights
- Constraints and relationships follow database specification strictly
