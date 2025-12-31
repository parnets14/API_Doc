# AiSensy Complete Integration API Documentation

## 📋 Overview

The AiSensy Complete Integration API provides comprehensive booking management capabilities for both customers and partners through chatbot interactions. This API supports the entire booking lifecycle from creation to completion, including reviews and quotations.

### 🔗 Base URL
```
https://nexo.works/api/aisensy
```

### 🔑 Authentication
**No authentication required** - All endpoints are public and designed for AiSensy chatbot integration.

---

## 🚀 Complete API Endpoints

### 📋 **Basic Operations**

#### 1. Get Available Services
```http
GET /api/aisensy/services
```
**Description:** Retrieve all available popular services and their add-ons.

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
      "serviceName": "AC Repair & Maintenance",
      "description": "Professional air conditioning services",
      "basePrice": 500,
      "price": "Starting from ₹500",
      "addOns": [
        {
          "addOnId": "60f7b3b3b3b3b3b3b3b3b3b4",
          "addOnName": "AC Installation",
          "description": "Complete AC installation service",
          "basePrice": 2500,
          "price": "₹2500"
        }
      ]
    }
  ]
}
```

#### 2. Get Booking Status
```http
GET /api/aisensy/booking/{bookingId}/status
```
**Description:** Get current booking status and details.

**Response:**
```json
{
  "success": true,
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "status": "accepted",
    "paymentStatus": "pending",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "scheduledDate": "2024-02-15T00:00:00.000Z",
    "scheduledTime": "14:30",
    "amount": 500,
    "partnerName": "Rajesh Kumar",
    "partnerPhone": "+919876543211",
    "otp": "123456",
    "acceptedAt": "2024-01-15T11:00:00.000Z"
  }
}
```

---

### 👤 **Customer Operations**

#### 3. Create Customer Booking
```http
POST /api/aisensy/customer/create-booking
```
**Description:** Create a new booking for a customer.

**Request Body:**
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "serviceName": "AC Repair & Maintenance",
  "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "scheduledDate": "2024-02-15",
  "scheduledTime": "14:30",
  "location": {
    "address": "123 Main Street, Koramangala, Bangalore",
    "landmark": "Near Forum Mall",
    "pincode": "560034"
  },
  "amount": 500,
  "paymentMode": "cash",
  "specialInstructions": "Please call before arrival"
}
```

#### 4. Cancel Customer Booking
```http
PUT /api/aisensy/customer/cancel-booking
```
**Description:** Cancel a booking (within 2 hours of creation).

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "customerPhone": "+919876543210",
  "cancellationReason": "Change of plans"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking cancelled successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "status": "cancelled",
    "cancellationReason": "Change of plans",
    "cancellationTime": "2024-01-15T12:00:00.000Z"
  }
}
```

#### 5. Submit Customer Review
```http
POST /api/aisensy/customer/submit-review
```
**Description:** Submit a review for a completed booking.

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "customerPhone": "+919876543210",
  "rating": 5,
  "comment": "Excellent service! Very professional and timely."
}
```

**Response:**
```json
{
  "success": true,
  "message": "Review submitted successfully! It will be published after moderation.",
  "data": {
    "reviewId": "60f7b3b3b3b3b3b3b3b3b3b9",
    "rating": 5,
    "comment": "Excellent service! Very professional and timely.",
    "status": "pending",
    "submittedAt": "2024-01-15T15:00:00.000Z"
  }
}
```

#### 6. Customer Quotation Action
```http
PUT /api/aisensy/customer/quotation-action
```
**Description:** Accept or reject a quotation from partner.

**Request Body (Accept):**
```json
{
  "quotationId": "60f7b3b3b3b3b3b3b3b3b3ba",
  "customerPhone": "+919876543210",
  "action": "accept"
}
```

**Request Body (Reject):**
```json
{
  "quotationId": "60f7b3b3b3b3b3b3b3b3b3ba",
  "customerPhone": "+919876543210",
  "action": "reject",
  "rejectionReason": "Price too high"
}
```

---

### 👷 **Partner Operations**

#### 7. Create Partner Booking
```http
POST /api/aisensy/partner/create-booking
```
**Description:** Create a booking and assign directly to a partner.

**Request Body:**
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543212",
  "customerName": "Jane Smith",
  "serviceName": "Plumbing Services",
  "scheduledDate": "2024-02-16",
  "scheduledTime": "10:00",
  "location": {
    "address": "456 Oak Street, Indiranagar, Bangalore"
  },
  "amount": 750
}
```

#### 8. Partner Accept Booking
```http
PUT /api/aisensy/partner/accept-booking
```
**Description:** Accept a pending booking.

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking accepted successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "partnerId": "60f7b3b3b3b3b3b3b3b3b3bc",
    "partnerName": "Rajesh Kumar",
    "status": "accepted",
    "acceptedAt": "2024-01-15T11:00:00.000Z",
    "otp": "123456",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance"
  }
}
```

#### 9. Partner Reject Booking
```http
PUT /api/aisensy/partner/reject-booking
```
**Description:** Reject a booking assignment.

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "rejectionReason": "Not available at scheduled time"
}
```

#### 10. Partner Complete Booking
```http
PUT /api/aisensy/partner/complete-booking
```
**Description:** Complete a booking using customer OTP.

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "otp": "123456",
  "remark": "Service completed successfully"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking completed successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "partnerId": "60f7b3b3b3b3b3b3b3b3b3bc",
    "partnerName": "Rajesh Kumar",
    "status": "completed",
    "completedAt": "2024-01-15T16:00:00.000Z",
    "remark": "Service completed successfully",
    "customerName": "John Doe",
    "serviceName": "AC Repair & Maintenance"
  }
}
```

#### 11. Partner Create Quotation
```http
POST /api/aisensy/partner/create-quotation
```
**Description:** Create a detailed quotation for additional work.

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "items": [
    {
      "description": "AC Repair Service",
      "quantity": 1,
      "unitPrice": 500,
      "totalPrice": 500
    },
    {
      "description": "Replacement Parts",
      "quantity": 2,
      "unitPrice": 150,
      "totalPrice": 300
    }
  ],
  "totalAmount": 800,
  "notes": "Includes parts and labor. 6 months warranty."
}
```

**Response:**
```json
{
  "success": true,
  "message": "Quotation created successfully",
  "data": {
    "quotationId": "60f7b3b3b3b3b3b3b3b3b3ba",
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "partnerId": "60f7b3b3b3b3b3b3b3b3b3bc",
    "partnerName": "Rajesh Kumar",
    "items": [...],
    "totalAmount": 800,
    "notes": "Includes parts and labor. 6 months warranty.",
    "customerStatus": "pending",
    "adminStatus": "pending",
    "createdAt": "2024-01-15T13:00:00.000Z"
  }
}
```

---

## 🔄 Complete Booking Lifecycle

### Customer Journey
1. **Discovery** → `GET /services` - Browse available services
2. **Booking** → `POST /customer/create-booking` - Create booking
3. **Monitoring** → `GET /booking/{id}/status` - Track status
4. **Quotation** → `PUT /customer/quotation-action` - Accept/reject quotes
5. **Cancellation** → `PUT /customer/cancel-booking` - Cancel if needed
6. **Review** → `POST /customer/submit-review` - Rate service

### Partner Journey
1. **Assignment** → `PUT /partner/accept-booking` - Accept bookings
2. **Quotation** → `POST /partner/create-quotation` - Send quotes
3. **Service** → `PUT /partner/complete-booking` - Complete work
4. **Alternative** → `PUT /partner/reject-booking` - Decline if unavailable

---

## 📊 Status Values & Workflow

### Booking Status Flow
```
pending → accepted → in_progress → completed
    ↓         ↓
cancelled   rejected
```

| Status | Description | Available Actions |
|--------|-------------|-------------------|
| `pending` | Awaiting partner acceptance | Accept, Reject, Cancel |
| `accepted` | Partner assigned, OTP generated | Complete, Create Quotation |
| `in_progress` | Service in progress | Complete |
| `completed` | Service finished | Submit Review |
| `cancelled` | Booking cancelled | None |
| `rejected` | Partner rejected | Reassign |

### Payment Status
| Status | Description |
|--------|-------------|
| `pending` | Payment not made |
| `completed` | Payment successful |
| `failed` | Payment failed |
| `refunded` | Payment refunded |

---

## 🔧 Validation Rules

### Phone Numbers
- Format: International (`+919876543210`)
- Length: 10-15 digits after country code
- Required for customer/partner verification

### Dates & Times
- Date: `YYYY-MM-DD` format
- Time: `HH:mm` (24-hour format)
- Must be today or future date

### Reviews
- Rating: 1-5 (integer)
- Comment: Minimum 10 characters
- Only for completed bookings

### Quotations
- Items: Non-empty array
- Each item: description, quantity, unitPrice, totalPrice
- Total amount must match sum of item totals

### Cancellation Policy
- Allowed within 2 hours of booking creation
- Only for pending/confirmed bookings
- Requires cancellation reason

---

## 💡 Integration Examples

### Complete Customer Flow
```javascript
// 1. Browse services
const services = await fetch('/api/aisensy/services');

// 2. Create booking
const booking = await fetch('/api/aisensy/customer/create-booking', {
  method: 'POST',
  body: JSON.stringify({
    customerPhone: "+919876543210",
    customerName: "John Doe",
    serviceName: "AC Repair",
    scheduledDate: "2024-02-15",
    scheduledTime: "14:30",
    location: { address: "123 Main Street" },
    amount: 500
  })
});

// 3. Monitor status
const status = await fetch(`/api/aisensy/booking/${bookingId}/status`);

// 4. Handle quotation (if received)
await fetch('/api/aisensy/customer/quotation-action', {
  method: 'PUT',
  body: JSON.stringify({
    quotationId: "...",
    customerPhone: "+919876543210",
    action: "accept"
  })
});

// 5. Submit review (after completion)
await fetch('/api/aisensy/customer/submit-review', {
  method: 'POST',
  body: JSON.stringify({
    bookingId: "...",
    customerPhone: "+919876543210",
    rating: 5,
    comment: "Excellent service!"
  })
});
```

### Complete Partner Flow
```javascript
// 1. Accept booking
const acceptance = await fetch('/api/aisensy/partner/accept-booking', {
  method: 'PUT',
  body: JSON.stringify({
    bookingId: "...",
    partnerPhone: "+919876543211"
  })
});

// 2. Create quotation (if additional work needed)
await fetch('/api/aisensy/partner/create-quotation', {
  method: 'POST',
  body: JSON.stringify({
    bookingId: "...",
    partnerPhone: "+919876543211",
    items: [
      {
        description: "Additional repair",
        quantity: 1,
        unitPrice: 300,
        totalPrice: 300
      }
    ],
    totalAmount: 300
  })
});

// 3. Complete booking
await fetch('/api/aisensy/partner/complete-booking', {
  method: 'PUT',
  body: JSON.stringify({
    bookingId: "...",
    partnerPhone: "+919876543211",
    otp: "123456",
    remark: "Service completed successfully"
  })
});
```

---

## 🧪 Testing

### Automated Testing
```bash
cd backend
node test-aisensy-extended.js
```

### Test Coverage
- ✅ Service discovery
- ✅ Customer booking creation
- ✅ Partner booking creation
- ✅ Booking status tracking
- ✅ Customer cancellation
- ✅ Partner acceptance/rejection
- ✅ Booking completion with OTP
- ✅ Review submission
- ✅ Quotation creation
- ✅ Quotation acceptance/rejection

---

## ⚠️ Error Handling

### Common Error Scenarios

#### Booking Not Found (404)
```json
{
  "success": false,
  "message": "Booking not found"
}
```

#### Phone Verification Failed (400)
```json
{
  "success": false,
  "message": "Customer phone number does not match booking"
}
```

#### Invalid OTP (400)
```json
{
  "success": false,
  "message": "Invalid OTP"
}
```

#### Cancellation Policy Violation (400)
```json
{
  "success": false,
  "message": "Cancellation is only allowed within 2 hours of booking creation"
}
```

#### Partner Not Approved (400)
```json
{
  "success": false,
  "message": "Partner is not approved. KYC status: pending"
}
```

---

## 🔔 Notifications

The system automatically sends notifications for:

### Customer Notifications
- Booking confirmation
- Partner assignment
- Booking completion
- Quotation received

### Partner Notifications
- New booking alerts
- Booking assignment
- Customer quotation response
- Booking completion confirmation

### Admin Notifications
- New bookings
- Cancellations
- Quotation requests
- Review submissions

---

## 📈 Analytics & Monitoring

### Key Metrics to Track
- Booking creation rate
- Partner acceptance rate
- Completion rate
- Review submission rate
- Quotation acceptance rate
- Cancellation rate

### Performance Monitoring
- API response times
- Error rates by endpoint
- Peak usage times
- Geographic distribution

---

## 🔒 Security Features

### Input Validation
- Phone number format validation
- Date/time validation
- Required field validation
- Data sanitization

### Business Logic Protection
- 2-hour cancellation policy
- OTP verification for completion
- Partner-booking assignment verification
- Review submission restrictions

### Rate Limiting (Recommended)
- Per-phone number limits
- Per-IP address limits
- Endpoint-specific limits

---

## 🆘 Support & Troubleshooting

### Integration Checklist
- [ ] Server running and accessible
- [ ] Popular services configured in database
- [ ] Partners registered and approved
- [ ] FCM tokens configured for notifications
- [ ] Test phone numbers available

### Common Integration Issues
1. **Service Not Found** - Ensure service names match exactly
2. **Partner Not Found** - Verify partner registration and approval
3. **OTP Issues** - Check booking status and OTP generation
4. **Cancellation Failures** - Verify 2-hour policy compliance

### Support Channels
- Technical Documentation: This document
- API Testing: Use provided test scripts
- Error Logs: Check server console for detailed errors

---

## 📝 API Summary

| Endpoint | Method | Purpose | Auth Required |
|----------|--------|---------|---------------|
| `/services` | GET | List services | No |
| `/booking/{id}/status` | GET | Get booking status | No |
| `/customer/create-booking` | POST | Create customer booking | No |
| `/customer/cancel-booking` | PUT | Cancel booking | No |
| `/customer/submit-review` | POST | Submit review | No |
| `/customer/quotation-action` | PUT | Accept/reject quotation | No |
| `/partner/create-booking` | POST | Create partner booking | No |
| `/partner/accept-booking` | PUT | Accept booking | No |
| `/partner/reject-booking` | PUT | Reject booking | No |
| `/partner/complete-booking` | PUT | Complete booking | No |
| `/partner/create-quotation` | POST | Create quotation | No |

**Total: 11 endpoints covering complete booking lifecycle**

---

*This API provides comprehensive booking management capabilities for AiSensy chatbot integration, supporting both customer and partner workflows with full lifecycle management.*
