# AiSensy Complete API Documentation

## 📋 Overview

The AiSensy API provides comprehensive booking and payment management for chatbot integration. All endpoints are public (no authentication required) and designed specifically for AiSensy's string-only data format.

### 🔗 Base URL
```
https://nexo.works/api/aisensy
```

### 🚨 Critical: AiSensy String-Only Format
AiSensy can **ONLY** send string data. All numeric values, IDs, and complex data must be sent as strings:
- ✅ `"750"` (amount as string)
- ✅ `"5"` (rating as string) 
- ✅ `"locationAddress"` (flattened location)
- ✅ `"Deep Cleaning,Gas Refill"` (comma-separated add-ons)
- ❌ `750` (numeric values not supported)
- ❌ `{"address": "123 Street"}` (objects not supported)

---

## 📚 API Endpoints

### 🔍 **Service Discovery**

#### 1. Get Available Services
```http
GET /api/aisensy/services
```

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
          "addOnName": "Deep Cleaning",
          "description": "Complete AC deep cleaning",
          "basePrice": 200,
          "price": "₹200"
        }
      ]
    }
  ]
}
```

---

### 👤 **Customer Operations**

#### 2. Create Customer Booking
```http
POST /api/aisensy/customer/create-booking
```

**Request Body (All Strings):**
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "serviceName": "AC Repair & Maintenance",
  "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "addOnNames": "Deep Cleaning,Gas Refill",
  "scheduledDate": "2024-02-15",
  "scheduledTime": "14:30",
  "locationAddress": "123 Main Street, Bangalore",
  "locationLandmark": "Near City Mall",
  "locationPincode": "560001",
  "amount": "750",
  "paymentMode": "cash",
  "specialInstructions": "Please call before arriving",
  "lat": "12.9716",
  "lng": "77.5946"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3b4",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "selectedAddOns": [
      {
        "name": "Deep Cleaning",
        "basePrice": 200
      }
    ],
    "status": "pending",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "amount": 750,
    "location": {
      "address": "123 Main Street, Bangalore",
      "landmark": "Near City Mall",
      "pincode": "560001"
    },
    "createdAt": "2024-01-15T10:00:00.000Z"
  }
}
```

#### 3. Cancel Customer Booking
```http
PUT /api/aisensy/customer/cancel-booking
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "customerPhone": "+919876543210",
  "cancellationReason": "Change of plans"
}
```

#### 4. Submit Customer Review
```http
POST /api/aisensy/customer/submit-review
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "customerPhone": "+919876543210",
  "rating": "5",
  "comment": "Excellent service! Very professional and timely."
}
```

#### 5. Customer Quotation Action
```http
PUT /api/aisensy/customer/quotation-action
```

**Request Body:**
```json
{
  "quotationId": "60f7b3b3b3b3b3b3b3b3b3ba",
  "customerPhone": "+919876543210",
  "action": "accept"
}
```

---

### 👷 **Partner Operations**

#### 6. Create Partner Booking
```http
POST /api/aisensy/partner/create-booking
```

**Request Body (All Strings):**
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543212",
  "customerName": "Jane Smith",
  "serviceName": "Plumbing Services",
  "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "addOnNames": "Pipe Replacement,Emergency Service",
  "scheduledDate": "2024-02-16",
  "scheduledTime": "10:00",
  "locationAddress": "456 Oak Street, Bangalore",
  "locationLandmark": "Near Metro Station",
  "locationPincode": "560038",
  "amount": "1200",
  "paymentMode": "cash",
  "specialInstructions": "Urgent repair needed"
}
```

#### 7. Partner Accept Booking
```http
PUT /api/aisensy/partner/accept-booking
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211"
}
```

#### 8. Partner Reject Booking
```http
PUT /api/aisensy/partner/reject-booking
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "rejectionReason": "Not available at scheduled time"
}
```

#### 9. Partner Complete Booking
```http
PUT /api/aisensy/partner/complete-booking
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "otp": "123456",
  "remark": "Service completed successfully"
}
```

#### 10. Partner Create Quotation
```http
POST /api/aisensy/partner/create-quotation
```

**Request Body (String Numbers):**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
  "partnerPhone": "+919876543211",
  "items": [
    {
      "description": "AC Repair Service",
      "quantity": "1",
      "unitPrice": "500",
      "totalPrice": "500"
    },
    {
      "description": "Replacement Parts",
      "quantity": "2",
      "unitPrice": "150",
      "totalPrice": "300"
    }
  ],
  "totalAmount": "800",
  "notes": "Includes parts and labor. 6 months warranty."
}
```

---

### 📊 **Status & Information**

#### 11. Get Booking Status
```http
GET /api/aisensy/booking/{bookingId}/status
```

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
    "amount": 750,
    "partnerName": "Rajesh Kumar",
    "partnerPhone": "+919876543211",
    "otp": "123456",
    "acceptedAt": "2024-01-15T11:00:00.000Z"
  }
}
```

---

### 💳 **Payment Operations**

#### 12. Initiate PayU Payment
```http
POST /api/aisensy/payu/initiate-payment
```

**Request Body:**
```json
{
  "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "amount": "750",
  "productInfo": "Service Booking Payment"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Payment initiated successfully",
  "data": {
    "txnid": "AISENSY_60f7b3b3b3b3b3b3b3b3b3b3_1640995200000",
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "amount": 750,
    "paymentUrl": "https://secure.payu.in/_payment?key=...",
    "payuData": {
      "action": "https://secure.payu.in/_payment",
      "params": {
        "key": "81Jp42",
        "txnid": "AISENSY_60f7b3b3b3b3b3b3b3b3b3b3_1640995200000",
        "amount": "750",
        "productinfo": "Service Booking Payment - AC Repair",
        "firstname": "John Doe",
        "email": "john@example.com",
        "phone": "+919876543210",
        "hash": "generated_hash_value"
      }
    }
  }
}
```

#### 13. Check Payment Status
```http
GET /api/aisensy/payu/payment-status/{txnid}
GET /api/aisensy/payu/booking-payment-status/{bookingId}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "txnid": "AISENSY_60f7b3b3b3b3b3b3b3b3b3b3_1640995200000",
    "paymentStatus": "completed",
    "paymentMode": "online",
    "amount": 750,
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "paymentDetails": {
      "gateway": "payu",
      "payuPaymentId": "403993715527806312",
      "completedAt": "2024-01-15T14:30:00.000Z",
      "status": "success"
    }
  }
}
```

---

## 🔄 Complete Integration Flows

### Customer Booking with Payment Flow
```javascript
// 1. Get available services
const services = await fetch('https://nexo.works/api/aisensy/services');

// 2. Create booking
const booking = await fetch('https://nexo.works/api/aisensy/customer/create-booking', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    customerPhone: "+919876543210",
    customerName: "John Doe",
    serviceName: "AC Repair",
    addOnNames: "Deep Cleaning,Gas Refill",
    scheduledDate: "2024-02-15",
    scheduledTime: "14:30",
    locationAddress: "123 Main Street, Bangalore",
    locationLandmark: "Near City Mall",
    locationPincode: "560001",
    amount: "750"
  })
});

const bookingData = await booking.json();
const bookingId = bookingData.data.bookingId;

// 3. Initiate payment
const payment = await fetch('https://nexo.works/api/aisensy/payu/initiate-payment', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: bookingId,
    customerPhone: "+919876543210",
    customerName: "John Doe",
    amount: "750"
  })
});

const paymentData = await payment.json();
const paymentUrl = paymentData.data.paymentUrl;
const txnid = paymentData.data.txnid;

// 4. Send payment URL to customer via AiSensy
// Customer clicks and completes payment

// 5. Check payment status
const status = await fetch(`https://nexo.works/api/aisensy/payu/payment-status/${txnid}`);
const paymentStatus = await status.json();

// 6. Check booking status
const bookingStatus = await fetch(`https://nexo.works/api/aisensy/booking/${bookingId}/status`);

// 7. Submit review after completion
const review = await fetch('https://nexo.works/api/aisensy/customer/submit-review', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: bookingId,
    customerPhone: "+919876543210",
    rating: "5",
    comment: "Excellent service!"
  })
});
```

### Partner Workflow
```javascript
// 1. Accept booking
const acceptance = await fetch('https://nexo.works/api/aisensy/partner/accept-booking', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: "60f7b3b3b3b3b3b3b3b3b3b8",
    partnerPhone: "+919876543211"
  })
});

// 2. Create quotation if needed
const quotation = await fetch('https://nexo.works/api/aisensy/partner/create-quotation', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: "60f7b3b3b3b3b3b3b3b3b3b8",
    partnerPhone: "+919876543211",
    items: [
      {
        description: "Additional repair",
        quantity: "1",
        unitPrice: "300",
        totalPrice: "300"
      }
    ],
    totalAmount: "300"
  })
});

// 3. Complete booking
const completion = await fetch('https://nexo.works/api/aisensy/partner/complete-booking', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: "60f7b3b3b3b3b3b3b3b3b3b8",
    partnerPhone: "+919876543211",
    otp: "123456",
    remark: "Service completed successfully"
  })
});
```

---

## 🚨 Error Handling

### Common Error Responses
```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error information (development only)"
}
```

### HTTP Status Codes
- `200` - Success
- `201` - Created successfully
- `400` - Bad request (validation error)
- `404` - Resource not found
- `500` - Internal server error

### Common Validation Errors
- **Invalid phone number**: Phone must match format `+919876543210`
- **Invalid amount**: Amount must be positive number as string
- **Invalid rating**: Rating must be "1" to "5" as string
- **Missing required fields**: All required fields must be provided
- **Booking not found**: Invalid booking ID
- **Customer mismatch**: Phone number doesn't match booking

---

## 📋 Field Reference

### Required Fields by Endpoint

#### Customer Booking
- `customerPhone` (string)
- `customerName` (string)
- `serviceName` (string)
- `scheduledDate` (string, YYYY-MM-DD)
- `scheduledTime` (string, HH:mm)
- `locationAddress` (string)
- `amount` (string)

#### Payment Initiation
- `bookingId` (string)
- `customerPhone` (string)
- `customerName` (string)
- `amount` (string)

#### Review Submission
- `bookingId` (string)
- `customerPhone` (string)
- `rating` (string, "1" to "5")
- `comment` (string, min 10 chars)

### Data Formats
- **Phone**: `+919876543210` (with country code)
- **Date**: `2024-02-15` (YYYY-MM-DD)
- **Time**: `14:30` (HH:mm, 24-hour format)
- **Amount**: `"750"` (string, will be converted to number)
- **Rating**: `"5"` (string, 1-5 scale)
- **Add-ons**: `"Deep Cleaning,Gas Refill"` (comma-separated)

---

## 🧪 Testing

### Test Endpoints
```bash
# Test service discovery
curl https://nexo.works/api/aisensy/services

# Test booking creation
curl -X POST https://nexo.works/api/aisensy/customer/create-booking \
  -H "Content-Type: application/json" \
  -d '{
    "customerPhone": "+919876543210",
    "customerName": "Test User",
    "serviceName": "AC Repair",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "locationAddress": "Test Address",
    "amount": "500"
  }'

# Test payment initiation
curl -X POST https://nexo.works/api/aisensy/payu/initiate-payment \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "BOOKING_ID",
    "customerPhone": "+919876543210",
    "customerName": "Test User",
    "amount": "500"
  }'
```

### Test Data
Use these test values for development:
- **Phone**: `+919876543210`
- **Service**: `"AC Repair & Maintenance"`
- **Amount**: `"500"`
- **Date**: `"2024-02-15"`
- **Time**: `"14:30"`

---

## 🎯 Quick Start Guide

### 1. Basic Booking Flow
```javascript
// Create booking
const booking = await createBooking({
  customerPhone: "+919876543210",
  customerName: "John Doe",
  serviceName: "AC Repair",
  scheduledDate: "2024-02-15",
  scheduledTime: "14:30",
  locationAddress: "123 Main Street",
  amount: "750"
});

// Get booking ID
const bookingId = booking.data.bookingId;

// Check status
const status = await checkBookingStatus(bookingId);
```

### 2. Payment Integration
```javascript
// Initiate payment
const payment = await initiatePayment({
  bookingId: bookingId,
  customerPhone: "+919876543210",
  customerName: "John Doe",
  amount: "750"
});

// Send payment URL to customer
const paymentUrl = payment.data.paymentUrl;
sendToCustomer(paymentUrl);

// Check payment status
const paymentStatus = await checkPaymentStatus(payment.data.txnid);
```

### 3. AiSensy Chatbot Integration
```javascript
// In your AiSensy webhook handler
async function handleBookingRequest(customerData) {
  // Create booking
  const booking = await fetch('https://nexo.works/api/aisensy/customer/create-booking', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      customerPhone: customerData.phone,
      customerName: customerData.name,
      serviceName: customerData.service,
      scheduledDate: customerData.date,
      scheduledTime: customerData.time,
      locationAddress: customerData.address,
      amount: customerData.amount
    })
  });
  
  const bookingData = await booking.json();
  
  if (bookingData.success) {
    return `✅ Booking created! ID: ${bookingData.data.bookingId}`;
  } else {
    return `❌ Error: ${bookingData.message}`;
  }
}
```

---

## 📞 Support

### API Status
- **Environment**: Production
- **Base URL**: `https://nexo.works/api/aisensy`
- **Rate Limiting**: None currently applied
- **Authentication**: Not required

### Common Issues
1. **String Format**: Ensure all numeric values are sent as strings
2. **Phone Format**: Use international format with country code
3. **Date Format**: Use YYYY-MM-DD format
4. **Add-ons**: Use comma-separated string format

### Contact
- **API Issues**: Check endpoint responses for detailed error messages
- **Integration Help**: Refer to code examples above
- **Payment Issues**: Check PayU payment status endpoints

---

**Last Updated**: January 2025  
**Version**: 2.0  
**Total Endpoints**: 15  
**String Format Compatible**: ✅ All endpoints
