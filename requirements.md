# 📋 Airbnb Clone – Backend Feature Specifications

This document provides detailed technical and functional specifications for three core backend features of the Airbnb Clone:

- ✅ User Authentication  
- 🏡 Property Management  
- 📅 Booking System

---

## ✅ 1. User Authentication

### 🔧 Functional Requirements
- Allow users to register as guests or hosts
- Authenticate users using JWT tokens
- Implement secure login/logout functionality
- Restrict access to protected routes using role-based access control

### 🔌 API Endpoints

| Method | Endpoint                | Description                     |
|--------|-------------------------|---------------------------------|
| POST   | `/api/v1/auth/register` | Register a new user             |
| POST   | `/api/v1/auth/login`    | Authenticate and return a JWT   |
| POST   | `/api/v1/auth/logout`   | Invalidate current user session |

### 📥 Input Example (Register)
```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "jane@example.com",
  "password": "StrongPass123!",
  "role": "guest"
}

```

### 📤 Output Example (Login)
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "user_id": "uuid",
    "email": "jane@example.com",
    "role": "guest"
  }
}
```

### ✅ Validation Rules
- Email must be unique and valid
- Password must be at least 8 characters with a mix of symbols and numbers
- Role must be one of: `guest`, `host`, or `admin`

### 🚀 Performance Criteria
- Login and registration responses in < 300ms
- Support 100+ concurrent requests during peak usage

## 🏡 2. Property Management
### 🔧 Functional Requirements
- Hosts can create, edit, delete, and view their property listings
- Each listing must include name, description, location, and price
- Properties must be associated with a host (user_id)

### 🔌 API Endpoints

| Method |	Endpoint                 |        Description              |
|--------|-------------------------- |---------------------------------|
| POST	 | `/api/v1/properties`      |	Create a new property listing  |
| PUT	 | `/api/v1/properties/:id`  |	Update an existing property    |
| DELETE |	`/api/v1/properties/:id` |	Delete a property              |
| GET	 | `/api/v1/properties`      |	List all properties            |
| GET	 | `/api/v1/properties/:id`  |	Retrieve specific property info|

### 📥 Input Example (Create Property)
```json
{
  "name": "Oceanview Cottage",
  "description": "A cozy place by the beach.",
  "location": "Diani, Kenya",
  "pricepernight": 150.00
}
```

### 📤 Output Example
```json
{
  "property_id": "uuid",
  "host_id": "uuid",
  "created_at": "2025-06-27T08:30:00Z",
  "name": "Oceanview Cottage"
}
```

### ✅ Validation Rules
- Name, description, location, and price are required
- Price must be a positive decimal
- Only users with the host role can access these endpoints

### 🚀 Performance Criteria
- Property list must be paginated (default: 10 per page)
- Support filters: location, price range, amenities
- Average response time < 500ms

## 📅 3. Booking System
### 🔧 Functional Requirements
- Guests can book available properties for valid dates
- Prevent double bookings via date conflict validation
- Track booking status: `pending`, `confirmed`, `cancelled`

### 🔌 API Endpoints

| Method |	         Endpoint	           |            Description                  |
|--------|---------------------------------|-----------------------------------------|
| POST	 | `/api/v1/bookings`              |	Create a new booking                 |
| GET	 | `/api/v1/bookings/:id`          |	Retrieve booking details             |      
| PUT	 | `/api/v1/bookings/:id`          | 	Update or cancel a booking           |      
| GET	 | `/api/v1/bookings/user/:userId` |	Get all bookings for a specific user |

### 📥 Input Example (Create Booking)
```json
{
  "property_id": "uuid",
  "user_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
```

### 📤 Output Example
```json
{
  "booking_id": "uuid",
  "status": "pending",
  "total_price": 600.00,
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
```

### ✅ Validation Rules
- `start_date` must be earlier than `end_date`
- Property must be available during the selected period
- Only users with role `guest` can create bookings

### 🚀 Performance Criteria
- Must prevent race conditions in double bookings
- Response time for availability check < 300ms
- Support concurrent booking requests with atomic validation

## 📌 Notes
- All endpoints must return standardized JSON and HTTP status codes:
`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `404 Not Found`
- Use token-based authentication (JWT) for secure access
- Enforce role-based access (RBAC) at the route level
- Input validation must happen both client-side and server-side