# NEXO API DOCUMENTATION
**Complete API Reference Guide**

---

**Document Information:**
- **Project:** Nexo Service Platform
- **Version:** 1.0
- **Date:** December 30, 2024
- **Base URL:** `http://localhost:9088/api`

---

## TABLE OF CONTENTS

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Customer/User APIs](#customer-user-apis)
4. [Partner APIs](#partner-apis)
5. [Vendor APIs](#vendor-apis)
6. [Admin APIs](#admin-apis)
7. [Public APIs](#public-apis)
8. [Response Formats](#response-formats)
9. [Status Codes](#status-codes)
10. [Security Notes](#security-notes)

---

## OVERVIEW

The Nexo API provides comprehensive endpoints for managing a multi-sided service platform connecting customers, service partners, vendors, and administrators. The API supports various functionalities including user management, service bookings, quotations, payments, and administrative operations.

### Key Features:
- **Multi-user Authentication** (Customers, Partners, Vendors, Admins)
- **Service Booking System** with payment integration
- **Quotation Management** with approval workflows
- **Real-time Notifications** via FCM
- **Comprehensive Dashboard** analytics
- **File Upload Support** for documents and images
- **Payment Gateway Integration** (PayU)

---

## AUTHENTICATION

Most endpoints require JWT authentication. Include the token in the Authorization header:

```
Authorization: Bearer <jwt_token>
```

### Token Types:
- **User Token:** For customer operations
- **Partner Token:** For service provider operations
- **Vendor Token:** For spare parts vendor operations
- **Admin Token:** For administrative operations

---

## CUSTOMER/USER APIs

### 🔐 Authentication Endpoints

#### 1. Send OTP for Registration/Login
**Endpoint:** `POST /user/auth/send-otp`

**Request Body:**
```json
{
  "phone": "9876543210"
}
```

**Response:**
```json
{
  "success": true,
  "message": "OTP sent successfully",
  "data": {
    "otpSent": true,
    "expiresIn": 300
  }
}
```

#### 2. Verify OTP
**Endpoint:** `POST /user/auth/verify-otp`

**Request Body:**
```json
{
  "phone": "9876543210",
  "otp": "123456"
}
```

**Response:**
```json
{
  "success": true,
  "message": "OTP verified successfully",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "user123",
      "name": "John Doe",
      "phone": "9876543210",
      "email": "john@example.com"
    }
  }
}
```

#### 3. Register New User
**Endpoint:** `POST /user/register`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "9876543210",
  "password": "securePassword123"
}
```

#### 4. Login with Password
**Endpoint:** `POST /user/login/password`

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

### 👤 Profile Management

#### 5. Get User Profile
**Endpoint:** `GET /user/profile`
**Authentication:** Required

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user123",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "9876543210",
    "profilePicture": "https://example.com/profile.jpg",
    "addresses": [
      {
        "id": "addr1",
        "street": "123 Main St",
        "city": "Mumbai",
        "state": "Maharashtra",
        "zip": "400001",
        "type": "home"
      }
    ]
  }
}
```

#### 6. Update Profile
**Endpoint:** `PUT /user/profile`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

**Form Data:**
- `name`: John Doe Updated
- `email`: john.updated@example.com
- `profilePicture`: [File]

### 🏠 Address Management

#### 7. Get All Addresses
**Endpoint:** `GET /user/addresses`
**Authentication:** Required

#### 8. Add New Address
**Endpoint:** `POST /user/addresses`
**Authentication:** Required

**Request Body:**
```json
{
  "street": "123 Main Street",
  "city": "Mumbai",
  "state": "Maharashtra",
  "zip": "400001",
  "type": "home",
  "landmark": "Near Metro Station"
}
```

#### 9. Update Address
**Endpoint:** `PUT /user/address/{addressId}`
**Authentication:** Required

#### 10. Delete Address
**Endpoint:** `DELETE /user/address/{addressId}`
**Authentication:** Required

### 🛍️ Service Discovery

#### 11. Get Service Categories
**Endpoint:** `GET /user/services/categories`

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "cat1",
      "name": "Plumbing",
      "icon": "https://example.com/plumbing-icon.png",
      "description": "All plumbing services"
    }
  ]
}
```

#### 12. Get Services by Category
**Endpoint:** `GET /user/services/categories/{categoryId}/services`

#### 13. Get Service Details
**Endpoint:** `GET /user/services/{serviceId}`

#### 14. Search Services
**Endpoint:** `GET /user/services/search?query=plumbing`

#### 15. Get Popular Services
**Endpoint:** `GET /user/services/popular`

### 📅 Service Booking

#### 16. Create Service Booking (with Payment)
**Endpoint:** `POST /user/service-booking/create`
**Authentication:** Required

**Request Body:**
```json
{
  "serviceId": "service123",
  "subServiceId": "subservice456",
  "scheduledDate": "2024-01-15",
  "scheduledTime": "10:00 AM",
  "address": {
    "street": "123 Main Street",
    "city": "Mumbai",
    "state": "Maharashtra",
    "zip": "400001",
    "landmark": "Near Metro Station"
  },
  "amount": 500,
  "paymentMethod": "online",
  "notes": "Please call before arriving"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "booking123",
    "paymentUrl": "https://secure.payu.in/...",
    "txnid": "txn456789",
    "amount": 500,
    "status": "pending_payment"
  }
}
```

#### 17. Create Service Booking (Wallet Payment)
**Endpoint:** `POST /user/service-booking/create-wallet-payment`
**Authentication:** Required

#### 18. Create Basic Booking
**Endpoint:** `POST /user/bookings`
**Authentication:** Required

#### 19. Get User Bookings
**Endpoint:** `GET /user/bookings`
**Authentication:** Required

**Query Parameters:**
- `page`: Page number (optional)
- `limit`: Items per page (optional)
- `status`: Filter by status (optional)

#### 20. Get Service Bookings
**Endpoint:** `GET /user/service-bookings`
**Authentication:** Required

#### 21. Get Booking Details
**Endpoint:** `GET /user/bookings/{bookingId}`
**Authentication:** Required

#### 22. Cancel Booking
**Endpoint:** `PUT /user/bookings/{bookingId}/cancel`
**Authentication:** Required

**Request Body:**
```json
{
  "reason": "Change of plans"
}
```

#### 23. Check Payment Status
**Endpoint:** `GET /user/service-booking/payment-status/{txnid}`
**Authentication:** Required

### 🚗 Travel Bookings

#### 24. Create Travel Booking
**Endpoint:** `POST /user/booktravel`
**Authentication:** Required

**Request Body:**
```json
{
  "from": "Mumbai",
  "to": "Pune",
  "date": "2024-01-15",
  "time": "09:00 AM",
  "passengers": 2,
  "vehicleType": "sedan",
  "notes": "Airport pickup required"
}
```

#### 25. Create Driver Booking
**Endpoint:** `POST /user/bookdriver`
**Authentication:** Required

#### 26. Get Travel Bookings
**Endpoint:** `GET /user/travelbookings`
**Authentication:** Required

#### 27. Cancel Travel Booking
**Endpoint:** `PUT /user/travelbookings/{bookingId}/cancel`
**Authentication:** Required

### 💰 Quotation Management

#### 28. Get Quotations for Booking
**Endpoint:** `GET /user/bookings/{bookingId}/quotations`
**Authentication:** Required

#### 29. Get Quotation Details
**Endpoint:** `GET /user/quotations/{quotationId}`
**Authentication:** Required

#### 30. Accept Quotation
**Endpoint:** `POST /user/quotations/{quotationId}/accept`
**Authentication:** Required

#### 31. Reject Quotation
**Endpoint:** `POST /user/quotations/{quotationId}/reject`
**Authentication:** Required

**Request Body:**
```json
{
  "rejectionReason": "Price too high"
}
```

#### 32. Submit Material Quotation Request
**Endpoint:** `POST /user/quotations/material-request`

**Request Body:**
```json
{
  "name": "John Doe",
  "phone": "9876543210",
  "email": "john@example.com",
  "customerName": "Jane Smith",
  "customerPhone": "9876543211",
  "customerAddress": "123 Main St, Mumbai",
  "technicianName": "Tech Mike",
  "technicianPhone": "9876543212",
  "serviceType": "Plumbing",
  "urgency": "normal",
  "requirements": "Need pipes and fittings",
  "selectedItems": {},
  "totalAmount": 5000
}
```

### ⭐ Reviews

#### 33. Submit Booking Review
**Endpoint:** `POST /user/bookings/{bookingId}/review`
**Authentication:** Required

**Request Body:**
```json
{
  "rating": 5,
  "comment": "Excellent service! Very professional and timely.",
  "type": "booking"
}
```

### 📊 Dashboard

#### 34. Get Enhanced Dashboard
**Endpoint:** `GET /user/dashboard`
**Authentication:** Required

#### 35. Get Quick Stats
**Endpoint:** `GET /user/dashboard/quick-stats`
**Authentication:** Required

#### 36. Get Smart Alerts
**Endpoint:** `GET /user/dashboard/alerts`
**Authentication:** Required

### 🔧 AMC Subscriptions

#### 37. Get AMC Plans
**Endpoint:** `GET /amc-plans`

#### 38. Initiate AMC Subscription Payment
**Endpoint:** `POST /user-subscriptions/initiate-payment`
**Authentication:** Required

**Request Body:**
```json
{
  "planId": "plan123",
  "duration": 12,
  "paymentMethod": "online",
  "address": {
    "street": "123 Main Street",
    "city": "Mumbai",
    "state": "Maharashtra",
    "zip": "400001"
  }
}
```

#### 39. Get User AMC Subscriptions
**Endpoint:** `GET /user-subscriptions/my-subscriptions`
**Authentication:** Required

#### 40. Cancel AMC Subscription
**Endpoint:** `PUT /user-subscriptions/{subscriptionId}/cancel`
**Authentication:** Required

---

## PARTNER APIs

### 🔐 Authentication

#### 41. Send Login OTP
**Endpoint:** `POST /partner/auth/send-otp`

#### 42. Verify Login OTP
**Endpoint:** `POST /partner/auth/verify-otp`

### 👤 Profile Management

#### 43. Get Partner Profile
**Endpoint:** `GET /partner/profile`
**Authentication:** Required

#### 44. Complete Profile
**Endpoint:** `POST /partner/profile/complete`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

### 📋 KYC Management

#### 45. Complete KYC
**Endpoint:** `POST /partner/kyc/complete`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

**Form Data:**
- `panCard`: File
- `aadhaar`: File
- `aadhaarback`: File
- `chequeImage`: File
- `accountNumber`: String
- `ifscCode`: String
- `accountHolderName`: String
- `bankName`: String

### 🛠️ Service Management

#### 46. Get Available Services
**Endpoint:** `GET /partner/services/available`
**Authentication:** Required

#### 47. Select Category and Service
**Endpoint:** `POST /partner/select-category-and-service`
**Authentication:** Required

### 📅 Booking Management

#### 48. Get All Partner Bookings
**Endpoint:** `GET /partner/bookings`
**Authentication:** Required

#### 49. Accept Booking
**Endpoint:** `PUT /partner/bookings/{bookingId}/accept`
**Authentication:** Required

#### 50. Complete Booking
**Endpoint:** `POST /partner/bookings/{bookingId}/complete`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

**Form Data:**
- `photos`: Files (max 10)
- `videos`: Files (max 5)
- `remark`: String
- `completionNotes`: String

#### 51. Pause Booking
**Endpoint:** `POST /partner/bookings/{bookingId}/pause`
**Authentication:** Required

**Request Body:**
```json
{
  "pauseReason": "Material shortage",
  "nextScheduledDate": "2024-01-15",
  "nextScheduledTime": "10:00 AM",
  "estimatedDuration": "2 hours"
}
```

#### 52. Resume Booking
**Endpoint:** `POST /partner/bookings/{bookingId}/resume`
**Authentication:** Required

### 💰 Quotation Management

#### 53. Create Quotation
**Endpoint:** `POST /partner/bookings/{bookingId}/quotation`
**Authentication:** Required

**Request Body:**
```json
{
  "items": [
    {
      "name": "Pipe Fitting",
      "description": "High quality pipe fitting",
      "quantity": 2,
      "unitPrice": 150,
      "total": 300,
      "materialId": "materialId123",
      "category": "Plumbing"
    }
  ],
  "subtotal": 300,
  "tax": 54,
  "discount": 0,
  "totalAmount": 354,
  "description": "Additional work required",
  "validTill": "2024-02-01T00:00:00.000Z",
  "notes": "Quality materials used"
}
```

#### 54. Get Partner Quotations
**Endpoint:** `GET /partner/quotations`
**Authentication:** Required

### 👥 Team Member Management

#### 55. Get Team Members
**Endpoint:** `GET /partner/team-members`
**Authentication:** Required

#### 56. Add Team Member
**Endpoint:** `POST /partner/team-members`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

#### 57. Assign Booking to Team Member
**Endpoint:** `POST /partner/team-members/assign-booking`
**Authentication:** Required

### 💼 Job Items Management

#### 58. Get Job Items
**Endpoint:** `GET /partner/jobs/{jobId}/items`
**Authentication:** Required

#### 59. Add Job Item
**Endpoint:** `POST /partner/jobs/items`
**Authentication:** Required

### 💳 Wallet Management

#### 60. Get Wallet Balance
**Endpoint:** `GET /partner/getWalletbypartner`
**Authentication:** Required

#### 61. Top Up Wallet
**Endpoint:** `POST /partner/wallet/topup`
**Authentication:** Required

### 📱 Notifications

#### 62. Get Notifications
**Endpoint:** `GET /partner/notifications`
**Authentication:** Required

#### 63. Update FCM Token
**Endpoint:** `PUT /partner/updateTokenFmc`
**Authentication:** Required

---

## VENDOR APIs

### 🔐 Authentication

#### 64. Send Login OTP
**Endpoint:** `POST /vendor/auth/send-otp`

#### 65. Verify OTP
**Endpoint:** `POST /vendor/auth/verify-otp`

#### 66. Login with Password
**Endpoint:** `POST /vendor/auth/login`

### 🔧 Spare Parts Management

#### 67. Get Spare Parts
**Endpoint:** `GET /vendor/spare-parts`
**Authentication:** Required

#### 68. Add Spare Part
**Endpoint:** `POST /vendor/spare-parts`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

**Form Data:**
- `name`: String
- `description`: String
- `category`: String
- `price`: Number
- `stock`: Number
- `image`: File

#### 69. Update Spare Part
**Endpoint:** `PUT /vendor/spare-parts/{id}`
**Authentication:** Required

### 📊 Transaction Management

#### 70. Get Transactions
**Endpoint:** `GET /vendor/transactions`
**Authentication:** Required

#### 71. Create Transaction
**Endpoint:** `POST /vendor/transactions`
**Authentication:** Required

---

## ADMIN APIs

### 🔐 Authentication

#### 72. Admin Login
**Endpoint:** `POST /admin/login`

**Request Body:**
```json
{
  "email": "admin@example.com",
  "password": "admin123"
}
```

#### 73. Create Sub-Admin
**Endpoint:** `POST /admin/create`
**Authentication:** Required

### 📊 Dashboard & Analytics

#### 74. Get Dashboard Analytics
**Endpoint:** `GET /admin/dashboard`
**Authentication:** Required

#### 75. Get Dashboard Counts
**Endpoint:** `GET /admin/dashboard/counts`
**Authentication:** Required

### 👥 User Management

#### 76. Get All Users
**Endpoint:** `GET /admin/users`
**Authentication:** Required

#### 77. Get All Customers
**Endpoint:** `GET /admin/customers`
**Authentication:** Required

### 🤝 Partner Management

#### 78. Get All Partners
**Endpoint:** `GET /admin/partners`
**Authentication:** Required

#### 79. Get Partner Details
**Endpoint:** `GET /admin/partners/{partnerId}`
**Authentication:** Required

#### 80. Update Partner Status
**Endpoint:** `PUT /admin/partners/{partnerId}/status`
**Authentication:** Required

**Request Body:**
```json
{
  "status": "active",
  "reason": "KYC verified"
}
```

#### 81. Verify Partner KYC
**Endpoint:** `PUT /admin/partners/{partnerId}/kyc`
**Authentication:** Required

### 🏪 Vendor Management

#### 82. Get All Vendors
**Endpoint:** `GET /admin/vendors`
**Authentication:** Required

#### 83. Create Vendor
**Endpoint:** `POST /admin/vendors`
**Authentication:** Required

#### 84. Update Vendor Status
**Endpoint:** `PUT /admin/vendors/{vendorId}/status`
**Authentication:** Required

### 🛠️ Service Management

#### 85. Create Service Category
**Endpoint:** `POST /admin/service-category`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

#### 86. Get All Categories
**Endpoint:** `GET /admin/service-categories`
**Authentication:** Required

#### 87. Create Service
**Endpoint:** `POST /admin/service`
**Authentication:** Required

#### 88. Create Sub-Service
**Endpoint:** `POST /admin/sub-service`
**Authentication:** Required

### 🎨 Banner Management

#### 89. Upload Banner
**Endpoint:** `POST /admin/banners`
**Authentication:** Required
**Content-Type:** `multipart/form-data`

#### 90. Get All Banners
**Endpoint:** `GET /admin/banners`
**Authentication:** Required

### 💰 Quotation Management

#### 91. Get All Quotations
**Endpoint:** `GET /admin/quotations`
**Authentication:** Required

#### 92. Approve Quotation
**Endpoint:** `PUT /admin/quotations/{quotationId}/approve`
**Authentication:** Required

#### 93. Reject Quotation
**Endpoint:** `PUT /admin/quotations/{quotationId}/reject`
**Authentication:** Required

### 📦 Inventory Management

#### 94. Get All Inventory Items
**Endpoint:** `GET /admin/inventory/items`
**Authentication:** Required

#### 95. Create Inventory Item
**Endpoint:** `POST /admin/inventory/items`
**Authentication:** Required

#### 96. Update Inventory Stock
**Endpoint:** `PUT /admin/inventory/items/{id}/stock`
**Authentication:** Required

### 📈 Reports & Analytics

#### 97. Create Booking (Admin)
**Endpoint:** `POST /admin/bookings/create`
**Authentication:** Required

#### 98. Get Revenue Analytics
**Endpoint:** `GET /admin/reports/revenue`
**Authentication:** Required

#### 99. Get Partner Performance
**Endpoint:** `GET /admin/reports/partners/performance`
**Authentication:** Required

#### 100. Get Transaction Report
**Endpoint:** `GET /admin/reports/transactions`
**Authentication:** Required

---

## PUBLIC APIs

### 🌐 No Authentication Required

#### 101. Get Material Categories
**Endpoint:** `GET /public/material-categories`

#### 102. Get Service Categories
**Endpoint:** `GET /public/service-categories`

#### 103. Get Popular Services
**Endpoint:** `GET /public/popular-services`

#### 104. Get Featured Reviews
**Endpoint:** `GET /public/featured-reviews`

---

## PAYMENT CALLBACKS

### 💳 Payment Gateway Integration

#### 105. Service Booking Payment Success
**Endpoint:** `POST /user/service-booking/payment/success`
**Description:** PayU success callback

#### 106. Service Booking Payment Failure
**Endpoint:** `POST /user/service-booking/payment/failure`
**Description:** PayU failure callback

#### 107. AMC Subscription Payment Success
**Endpoint:** `POST /user-subscriptions/payment-success`
**Description:** PayU success callback for AMC subscriptions

---

## RESPONSE FORMATS

### Success Response
```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": {
    // Response data
  }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error message",
  "error": "Detailed error information",
  "errors": ["Validation error 1", "Validation error 2"]
}
```

### Paginated Response
```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "pages": 5
  }
}
```

---

## STATUS CODES

| Code | Description |
|------|-------------|
| 200  | Success |
| 201  | Created |
| 400  | Bad Request |
| 401  | Unauthorized |
| 403  | Forbidden |
| 404  | Not Found |
| 422  | Validation Error |
| 500  | Internal Server Error |

---

## SECURITY NOTES

1. **JWT Authentication:** All sensitive endpoints require valid JWT tokens
2. **File Upload Validation:** File types and sizes are strictly validated
3. **Rate Limiting:** API calls are rate-limited to prevent abuse
4. **Input Validation:** All inputs are validated and sanitized
5. **Password Security:** Passwords are hashed using bcrypt
6. **OTP Security:** OTPs have expiry times and attempt limits
7. **HTTPS Only:** All production endpoints must use HTTPS
8. **CORS Configuration:** Proper CORS headers are configured

---

## SUPPORT INFORMATION

**Development Team Contact:**
- **Email:** dev-team@nexo.com
- **Documentation:** Internal Wiki
- **Last Updated:** December 30, 2024
- **API Version:** 1.0

---

**END OF DOCUMENT**