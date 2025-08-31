# Airbnb Clone Backend - Requirement Specifications

This document outlines the **functional and technical requirements** for three key backend features of the Airbnb Clone project.

---

## 1. User Authentication

### Description
Users should be able to **register**, **log in**, and **log out** securely. The system must ensure proper credential validation, password hashing, and session/token management.

### API Endpoints
- **POST /api/auth/register**
  - Input: `{ "name": "string", "email": "string", "password": "string" }`
  - Output: `{ "message": "User registered successfully", "userId": "uuid" }`
- **POST /api/auth/login**
  - Input: `{ "email": "string", "password": "string" }`
  - Output: `{ "token": "jwt_token", "userId": "uuid" }`
- **POST /api/auth/logout**
  - Input: `{ "token": "jwt_token" }`
  - Output: `{ "message": "Logged out successfully" }`

### Validation Rules
- Email must be unique and in a valid format.
- Password must be at least 8 characters long, containing upper/lowercase letters and numbers.
- Tokens expire after 24 hours.

### Performance Criteria
- Authentication requests should respond in under **300ms**.
- Passwords must be hashed using **bcrypt** or a similar algorithm.

---

## 2. Property Management

### Description
Hosts can **add, update, view, and delete properties**. Properties must include essential details for display to potential guests.

### API Endpoints
- **POST /api/properties**
  - Input: `{ "title": "string", "description": "string", "location": "string", "price": "number", "hostId": "uuid" }`
  - Output: `{ "message": "Property created successfully", "propertyId": "uuid" }`
- **GET /api/properties/:id**
  - Input: `propertyId`
  - Output: `{ "propertyId": "uuid", "title": "string", "description": "string", "location": "string", "price": "number", "hostId": "uuid" }`
- **PUT /api/properties/:id**
  - Input: Updated property fields
  - Output: `{ "message": "Property updated successfully" }`
- **DELETE /api/properties/:id**
  - Input: `propertyId`
  - Output: `{ "message": "Property deleted successfully" }`

### Validation Rules
- Price must be a positive number.
- Title and description cannot be empty.
- Only the host who created the property can update or delete it.

### Performance Criteria
- Must support listing **10,000+ properties** with pagination.
- Property retrieval should respond in under **500ms**.

---

## 3. Booking System

### Description
Guests can book available properties by specifying dates. The system must ensure that double-booking is prevented.

### API Endpoints
- **POST /api/bookings**
  - Input: `{ "propertyId": "uuid", "guestId": "uuid", "startDate": "date", "endDate": "date" }`
  - Output: `{ "message": "Booking created successfully", "bookingId": "uuid" }`
- **GET /api/bookings/:id**
  - Input: `bookingId`
  - Output: `{ "bookingId": "uuid", "propertyId": "uuid", "guestId": "uuid", "startDate": "date", "endDate": "date", "status": "confirmed|cancelled" }`
- **DELETE /api/bookings/:id**
  - Input: `bookingId`
  - Output: `{ "message": "Booking cancelled successfully" }`

### Validation Rules
- Dates must follow ISO format (`YYYY-MM-DD`).
- Start date must be before end date.
- A property cannot be double-booked within the same date range.

### Performance Criteria
- Booking confirmation must be processed in under **400ms**.
- Must scale to handle **1,000+ concurrent booking requests**.

---

## Summary
- **User Authentication** ensures secure access.
- **Property Management** allows hosts to manage listings.
- **Booking System** enables guests to reserve properties safely.

This requirements document will serve as a **blueprint** for backend implementation.
