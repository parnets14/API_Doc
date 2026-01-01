# AiSensy Complete API Documentation

## 📋 Overview

The AiSensy API provides comprehensive booking and payment management for chatbot integration. All endpoints require static token authentication and are designed specifically for AiSensy's string-only data format.

### 🔗 Base URL
```
https://nexo.works/api/aisensy
```

### 🔐 Authentication
All AiSensy API endpoints require a static token for authentication. The token can be provided in multiple ways:

#### Option 1: Authorization Header (Recommended)
```bash
curl -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
     -X GET "https://nexo.works/api/aisensy/services"
```

#### Option 2: Custom Header
```bash
curl -H "x-api-token: aisensy_nexo_2025_secure_token_v1" \
     -X GET "https://nexo.works/api/aisensy/services"
```

#### Option 3: Query Parameter
```bash
curl -X GET "https://nexo.works/api/aisensy/services?token=aisensy_nexo_2025_secure_token_v1"
```

#### Option 4: Request Body (for POST/PUT requests)
```json
{
  "token": "aisensy_nexo_2025_secure_token_v1",
  "customerPhone": "+919876543210",
  "customerName": "John Doe"
}
```

### 🚨 Authentication Errors
**Missing Token (401)**:
```json
{
  "success": false,
  "message": "Authentication required",
  "error": "Missing API token",
  "hint": "Provide token in Authorization header, x-api-token header, or as query/body parameter"
}
```

**Invalid Token (401)**:
```json
{
  "success": false,
  "message": "Invalid API token",
  "error": "Authentication failed"
}
```

### 🚨 Critical: AiSensy String-Only Format
AiSensy can **ONLY** send string data. All numeric values, IDs, and complex data must be sent as strings:
- ✅ `"750"` (amount as string)
- ✅ `"5"` (rating as string) 
- ✅ `"locationAddress"` (flattened location)
- ✅ `"Deep Cleaning,Gas Refill"` (comma-separated sub add-on names)
- ❌ `750` (numeric values not supported)
- ❌ `{"address": "123 Street"}` (objects not supported)

### 📋 Service Structure
Each service has **add-ons**, and each add-on contains **sub-services**:
```
Service (AC Repair)
├── Add-on 1 (Maintenance)
│   ├── Sub-service: Deep Cleaning
│   ├── Sub-service: Filter Replacement
│   └── Sub-service: Gas Refill
└── Add-on 2 (Installation)
    ├── Sub-service: Wall Mount
    └── Sub-service: Window Mount
```

**Note**: `addOnNames` refers to the **sub-service names** within add-ons, sent as comma-separated string.

---

## � SeIrvice & Add-On Structure

### Understanding Services and Add-Ons
Each service in the system has a hierarchical structure:

```
📋 Service (e.g., "AC Repair & Maintenance")
├── 🔧 Add-On Category (e.g., "AC Maintenance")
│   ├── 🛠️ Sub-Service: "Deep Cleaning" (₹150)
│   ├── 🛠️ Sub-Service: "Filter Replacement" (₹100)
│   └── 🛠️ Sub-Service: "Gas Refill" (₹300)
├── 🔧 Add-On Category (e.g., "AC Installation")
│   ├── 🛠️ Sub-Service: "Wall Mount Installation" (₹500)
│   └── 🛠️ Sub-Service: "Window Mount Installation" (₹400)
└── 🔧 Add-On Category (e.g., "AC Repair")
    ├── 🛠️ Sub-Service: "Compressor Repair" (₹800)
    └── 🛠️ Sub-Service: "Thermostat Replacement" (₹200)
```

### How `addOnNames` Works
- **`addOnNames`** refers to either **add-on names** or **sub-service names** within add-ons
- Send as comma-separated string: `"Deep Cleaning,AC Maintenance,Wall Mount Installation"`
- The system searches in this priority:
  1. **Sub-services first**: Looks for sub-services within all add-on categories
  2. **Add-ons second**: If not found as sub-service, looks for add-on category itself

### Pricing Logic
- **Sub-service selected**: Adds individual sub-service price (e.g., Deep Cleaning = ₹150)
- **Add-on selected**: Adds entire add-on price (e.g., AC Maintenance = ₹500)
- **Mixed selection**: Can combine both sub-services and full add-ons

### Example Scenarios

#### Scenario 1: Select specific sub-services
```json
{
  "addOnNames": "Deep Cleaning,Gas Refill"
}
```
**Result**: Base Service + Deep Cleaning (₹150) + Gas Refill (₹300) = Total

#### Scenario 2: Select entire add-on
```json
{
  "addOnNames": "AC Maintenance"
}
```
**Result**: Base Service + AC Maintenance (₹500) = Total

#### Scenario 3: Mixed selection
```json
{
  "addOnNames": "Deep Cleaning,AC Installation"
}
```
**Result**: Base Service + Deep Cleaning (₹150) + AC Installation (₹800) = Total

### Example Service Response Structure
```json
{
  "serviceName": "AC Repair & Maintenance",
  "addOns": [
    {
      "addOnName": "AC Maintenance",
      "subServices": [
        {"name": "Deep Cleaning", "price": "₹150"},
        {"name": "Filter Replacement", "price": "₹100"},
        {"name": "Gas Refill", "price": "₹300"}
      ]
    },
    {
      "addOnName": "AC Installation", 
      "subServices": [
        {"name": "Wall Mount Installation", "price": "₹500"},
        {"name": "Window Mount Installation", "price": "₹400"}
      ]
    }
  ]
}
```

### Booking with Add-Ons and Sub-Services
When you send `"addOnNames": "Deep Cleaning,AC Maintenance"`, the system:
1. Searches for "Deep Cleaning" → Finds it as sub-service in "AC Maintenance" category
2. Searches for "AC Maintenance" → Finds it as add-on category
3. Adds Deep Cleaning sub-service (₹150) to booking
4. Adds AC Maintenance add-on (₹500) to booking  
5. Calculates total: Base Service + Deep Cleaning (₹150) + AC Maintenance (₹500)

**Smart Logic**: The system prioritizes sub-services over add-ons, so if you want the entire add-on, use the exact add-on name.

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
          "addOnName": "AC Maintenance",
          "description": "Complete AC maintenance services",
          "basePrice": 200,
          "price": "₹200",
          "subServices": [
            {
              "name": "Deep Cleaning",
              "shortDescription": "Complete AC deep cleaning",
              "price": "₹150"
            },
            {
              "name": "Filter Replacement",
              "shortDescription": "Replace AC filters",
              "price": "₹100"
            },
            {
              "name": "Gas Refill",
              "shortDescription": "AC gas refill service",
              "price": "₹300"
            }
          ]
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
  "addOnNames": "Deep Cleaning,AC Installation",
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
        "basePrice": 150,
        "type": "subservice",
        "parentAddOn": "AC Maintenance"
      },
      {
        "name": "AC Installation", 
        "basePrice": 800,
        "type": "addon"
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

## 🚀 **Minimal Request APIs**

#### 15. Simple Booking (Minimal Data)
```http
POST /api/aisensy/simple-booking
```
**Description:** Create booking with minimal required data. Auto-fills missing details with smart defaults.

**Required Field:** Only `phone`
**Optional Fields:** `name`, `service`, `amount`

**Request Body:**
```json
{
  "phone": "+919876543210",
  "name": "John Doe",
  "service": "AC Repair",
  "amount": "500"
}
```

**Minimal Request (Phone Only):**
```json
{
  "phone": "+919876543210"
}
```

**Auto-Generated Defaults:**
- **Name**: "Customer" (if not provided)
- **Service**: First available service (if not provided)
- **Amount**: Service base price (if not provided)
- **Date**: Tomorrow
- **Time**: 10:00 AM
- **Address**: "Address to be confirmed"
- **Email**: Generated from phone number

**Response:**
```json
{
  "success": true,
  "message": "Simple booking created successfully! Customer will be contacted to confirm details.",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "scheduledDate": "2024-02-16",
    "scheduledTime": "10:00",
    "amount": 500,
    "status": "pending",
    "note": "This is a simplified booking. Customer will be contacted to confirm address and other details."
  }
}
```

#### 16. Ultra-Minimal Booking (Phone Only)
```http
POST /api/aisensy/minimal-booking
```
**Description:** Create booking with ONLY phone number. Everything else is auto-generated.

**Required Field:** Only `phone`

**Request Body:**
```json
{
  "phone": "+919876543210"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Booking created! Our team will contact you shortly to confirm details.",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "scheduledDate": "2024-02-16",
    "scheduledTime": "10:00",
    "amount": 500,
    "status": "pending",
    "message": "Customer service will call to confirm address and service details"
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
- **Add-ons**: `"Deep Cleaning,Gas Refill"` (comma-separated sub-service names)

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

## 🤖 AiSensy Integration Examples

### API Complexity Comparison

#### 🔴 Full Booking API (7 required fields)
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe", 
  "serviceName": "AC Repair",
  "scheduledDate": "2024-02-15",
  "scheduledTime": "14:30",
  "locationAddress": "123 Street",
  "amount": "750"
}
```

#### 🟡 Simple Booking API (1 required field)
```json
{
  "phone": "+919876543210",
  "name": "John Doe",
  "service": "AC Repair", 
  "amount": "750"
}
```

#### 🟢 Minimal Booking API (1 required field)
```json
{
  "phone": "+919876543210"
}
```

### Chatbot Integration Scenarios

#### Scenario 1: Customer with Complete Information
```javascript
// Customer: "Book AC repair for tomorrow at 2 PM, my name is John, amount 750"
const fullBooking = await fetch('https://nexo.works/api/aisensy/customer/create-booking', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    customerPhone: "+919876543210",
    customerName: "John Doe",
    serviceName: "AC Repair",
    scheduledDate: "2024-02-15",
    scheduledTime: "14:30",
    locationAddress: "123 Main Street",
    amount: "750"
  })
});
```

#### Scenario 2: Customer with Basic Information
```javascript
// Customer: "Book AC repair, my name is John"
const simpleBooking = await fetch('https://nexo.works/api/aisensy/simple-booking', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    phone: "+919876543210",
    name: "John Doe",
    service: "AC Repair"
    // amount, date, time, address auto-filled
  })
});
```

#### Scenario 3: Customer with Phone Only
```javascript
// Customer: "I need help with my AC"
const minimalBooking = await fetch('https://nexo.works/api/aisensy/minimal-booking', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    phone: "+919876543210"
    // Everything else auto-filled
  })
});
```

### Benefits of Minimal APIs

#### For AiSensy Chatbots
- **Reduced Complexity**: Less data extraction required
- **Higher Success Rate**: Fewer required fields = fewer failures
- **Better UX**: Customers don't need to provide all details upfront
- **Graceful Handling**: Works even with incomplete information

#### For Business Operations
- **Lead Capture**: Never lose a potential customer due to missing info
- **Follow-up Process**: Customer service confirms details later
- **Conversion Rate**: More bookings created successfully
- **Customer Satisfaction**: Quick and easy booking process

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

---

## 📊 Booking Retrieval & Action APIs

### 📱 Get Customer Bookings by Phone
**Endpoint**: `GET /customer/bookings/{phone}`
**Description**: Retrieve all bookings for a customer by phone number

#### Request
```
GET /api/aisensy/customer/bookings/+919876543210
```

#### Response
```json
{
  "success": true,
  "data": {
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "totalBookings": 3,
    "bookings": [
      {
        "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
        "serviceName": "AC Repair & Maintenance",
        "status": "completed",
        "paymentStatus": "completed",
        "scheduledDate": "2024-01-15T00:00:00.000Z",
        "scheduledTime": "14:30",
        "amount": 750,
        "location": {
          "address": "123 Main Street",
          "landmark": "Near City Mall",
          "pincode": "560001"
        },
        "selectedAddOns": [
          {
            "name": "Deep Cleaning",
            "basePrice": 150
          }
        ],
        "partnerInfo": {
          "name": "Partner Name",
          "phone": "+919876543211"
        },
        "otp": null,
        "createdAt": "2024-01-14T10:30:00.000Z",
        "updatedAt": "2024-01-15T16:00:00.000Z",
        "createdVia": "aisensy"
      }
    ]
  }
}
```

---

### 📋 Get Customer Actions
**Endpoint**: `GET /customer/actions`
**Description**: Get all available customer API actions with details

#### Request
```
GET /api/aisensy/customer/actions
```

#### Response
```json
{
  "success": true,
  "message": "Available customer actions",
  "data": {
    "totalActions": 10,
    "actions": [
      {
        "action": "create-booking",
        "method": "POST",
        "endpoint": "/api/aisensy/customer/create-booking",
        "description": "Create a new booking with full details",
        "requiredFields": ["customerPhone", "customerName", "serviceName", "scheduledDate", "scheduledTime", "locationAddress", "amount"],
        "optionalFields": ["customerEmail", "serviceId", "addOnNames", "locationLandmark", "locationPincode", "paymentMode", "specialInstructions", "lat", "lng"]
      },
      {
        "action": "simple-booking",
        "method": "POST",
        "endpoint": "/api/aisensy/simple-booking",
        "description": "Create booking with minimal data (auto-fills missing details)",
        "requiredFields": ["phone"],
        "optionalFields": ["name", "service", "amount"]
      },
      {
        "action": "minimal-booking",
        "method": "POST",
        "endpoint": "/api/aisensy/minimal-booking",
        "description": "Create booking with phone only (everything auto-filled)",
        "requiredFields": ["phone"],
        "optionalFields": []
      },
      {
        "action": "cancel-booking",
        "method": "PUT",
        "endpoint": "/api/aisensy/customer/cancel-booking",
        "description": "Cancel a booking (within 2 hours of creation)",
        "requiredFields": ["bookingId", "customerPhone", "cancellationReason"],
        "optionalFields": []
      },
      {
        "action": "submit-review",
        "method": "POST",
        "endpoint": "/api/aisensy/customer/submit-review",
        "description": "Submit review for completed booking",
        "requiredFields": ["bookingId", "customerPhone", "rating", "comment"],
        "optionalFields": []
      },
      {
        "action": "quotation-action",
        "method": "PUT",
        "endpoint": "/api/aisensy/customer/quotation-action",
        "description": "Accept or reject partner quotation",
        "requiredFields": ["quotationId", "customerPhone", "action"],
        "optionalFields": ["rejectionReason"]
      },
      {
        "action": "initiate-payment",
        "method": "POST",
        "endpoint": "/api/aisensy/payu/initiate-payment",
        "description": "Initiate PayU payment for booking",
        "requiredFields": ["bookingId", "customerPhone", "customerName", "amount"],
        "optionalFields": ["customerEmail", "productInfo"]
      },
      {
        "action": "get-bookings",
        "method": "GET",
        "endpoint": "/api/aisensy/customer/bookings/{phone}",
        "description": "Get all bookings by phone number",
        "requiredFields": ["phone"],
        "optionalFields": []
      },
      {
        "action": "get-booking-status",
        "method": "GET",
        "endpoint": "/api/aisensy/booking/{bookingId}/status",
        "description": "Get specific booking status",
        "requiredFields": ["bookingId"],
        "optionalFields": []
      },
      {
        "action": "check-payment-status",
        "method": "GET",
        "endpoint": "/api/aisensy/payu/payment-status/{txnid}",
        "description": "Check payment status by transaction ID",
        "requiredFields": ["txnid"],
        "optionalFields": []
      }
    ]
  }
}
```

---

### 🔧 Get Partner Actions
**Endpoint**: `GET /partner/actions`
**Description**: Get all available partner API actions with details

#### Request
```
GET /api/aisensy/partner/actions
```

#### Response
```json
{
  "success": true,
  "message": "Available partner actions",
  "data": {
    "totalActions": 6,
    "actions": [
      {
        "action": "create-partner-booking",
        "method": "POST",
        "endpoint": "/api/aisensy/partner/create-booking",
        "description": "Create booking and assign directly to partner",
        "requiredFields": ["partnerPhone", "customerPhone", "customerName", "serviceName", "scheduledDate", "scheduledTime", "locationAddress", "amount"],
        "optionalFields": ["customerEmail", "serviceId", "addOnNames", "locationLandmark", "locationPincode", "paymentMode", "specialInstructions", "lat", "lng"]
      },
      {
        "action": "accept-booking",
        "method": "PUT",
        "endpoint": "/api/aisensy/partner/accept-booking",
        "description": "Accept a pending booking",
        "requiredFields": ["bookingId", "partnerPhone"],
        "optionalFields": []
      },
      {
        "action": "reject-booking",
        "method": "PUT",
        "endpoint": "/api/aisensy/partner/reject-booking",
        "description": "Reject a booking assignment",
        "requiredFields": ["bookingId", "partnerPhone", "rejectionReason"],
        "optionalFields": []
      },
      {
        "action": "complete-booking",
        "method": "PUT",
        "endpoint": "/api/aisensy/partner/complete-booking",
        "description": "Complete booking using customer OTP",
        "requiredFields": ["bookingId", "partnerPhone", "otp"],
        "optionalFields": ["remark"]
      },
      {
        "action": "create-quotation",
        "method": "POST",
        "endpoint": "/api/aisensy/partner/create-quotation",
        "description": "Create quotation for additional work",
        "requiredFields": ["bookingId", "partnerPhone", "items", "totalAmount"],
        "optionalFields": ["notes"]
      },
      {
        "action": "get-partner-bookings",
        "method": "GET",
        "endpoint": "/api/aisensy/partner/bookings/{partnerPhone}",
        "description": "Get all bookings assigned to partner",
        "requiredFields": ["partnerPhone"],
        "optionalFields": []
      }
    ]
  }
}
```

---

### 🔧 Get Partner Bookings
**Endpoint**: `GET /partner/bookings/{partnerPhone}`
**Description**: Retrieve all bookings assigned to a partner

#### Request
```
GET /api/aisensy/partner/bookings/+919876543210
```

#### Response
```json
{
  "success": true,
  "data": {
    "partnerPhone": "+919876543210",
    "partnerName": "Partner Name",
    "totalBookings": 5,
    "bookings": [
      {
        "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
        "serviceName": "Plumbing Service",
        "status": "accepted",
        "paymentStatus": "pending",
        "scheduledDate": "2024-01-16T00:00:00.000Z",
        "scheduledTime": "10:00",
        "amount": 1200,
        "location": {
          "address": "456 Oak Street",
          "landmark": "Near Park",
          "pincode": "560002"
        },
        "selectedAddOns": [
          {
            "name": "Pipe Replacement",
            "basePrice": 300
          }
        ],
        "customerInfo": {
          "name": "Customer Name",
          "phone": "+919876543211",
          "email": "customer@example.com"
        },
        "otp": "123456",
        "acceptedAt": "2024-01-15T12:00:00.000Z",
        "completedAt": null,
        "createdAt": "2024-01-15T10:30:00.000Z",
        "updatedAt": "2024-01-15T12:00:00.000Z",
        "createdVia": "aisensy"
      }
    ]
  }
}
```

---

### 📊 Get All Actions
**Endpoint**: `GET /actions`
**Description**: Get all available API actions (customer, partner, and general)

#### Request
```
GET /api/aisensy/actions
```

#### Response
```json
{
  "success": true,
  "message": "All available AiSensy API actions",
  "data": {
    "totalActions": 19,
    "customerActions": 8,
    "partnerActions": 6,
    "generalActions": 3,
    "actions": {
      "customer": [
        {
          "action": "create-booking",
          "method": "POST",
          "endpoint": "/api/aisensy/customer/create-booking",
          "description": "Create a new booking with full details",
          "category": "booking"
        },
        {
          "action": "simple-booking",
          "method": "POST",
          "endpoint": "/api/aisensy/simple-booking",
          "description": "Create booking with minimal data",
          "category": "booking"
        },
        {
          "action": "minimal-booking",
          "method": "POST",
          "endpoint": "/api/aisensy/minimal-booking",
          "description": "Create booking with phone only",
          "category": "booking"
        },
        {
          "action": "cancel-booking",
          "method": "PUT",
          "endpoint": "/api/aisensy/customer/cancel-booking",
          "description": "Cancel a booking",
          "category": "booking"
        },
        {
          "action": "submit-review",
          "method": "POST",
          "endpoint": "/api/aisensy/customer/submit-review",
          "description": "Submit review for completed booking",
          "category": "review"
        },
        {
          "action": "quotation-action",
          "method": "PUT",
          "endpoint": "/api/aisensy/customer/quotation-action",
          "description": "Accept or reject partner quotation",
          "category": "quotation"
        },
        {
          "action": "initiate-payment",
          "method": "POST",
          "endpoint": "/api/aisensy/payu/initiate-payment",
          "description": "Initiate PayU payment",
          "category": "payment"
        },
        {
          "action": "get-bookings",
          "method": "GET",
          "endpoint": "/api/aisensy/customer/bookings/{phone}",
          "description": "Get all customer bookings",
          "category": "information"
        }
      ],
      "partner": [
        {
          "action": "create-partner-booking",
          "method": "POST",
          "endpoint": "/api/aisensy/partner/create-booking",
          "description": "Create and assign booking to partner",
          "category": "booking"
        },
        {
          "action": "accept-booking",
          "method": "PUT",
          "endpoint": "/api/aisensy/partner/accept-booking",
          "description": "Accept a pending booking",
          "category": "booking"
        },
        {
          "action": "reject-booking",
          "method": "PUT",
          "endpoint": "/api/aisensy/partner/reject-booking",
          "description": "Reject a booking assignment",
          "category": "booking"
        },
        {
          "action": "complete-booking",
          "method": "PUT",
          "endpoint": "/api/aisensy/partner/complete-booking",
          "description": "Complete booking with OTP",
          "category": "booking"
        },
        {
          "action": "create-quotation",
          "method": "POST",
          "endpoint": "/api/aisensy/partner/create-quotation",
          "description": "Create quotation for additional work",
          "category": "quotation"
        },
        {
          "action": "get-partner-bookings",
          "method": "GET",
          "endpoint": "/api/aisensy/partner/bookings/{partnerPhone}",
          "description": "Get all partner bookings",
          "category": "information"
        }
      ],
      "general": [
        {
          "action": "get-services",
          "method": "GET",
          "endpoint": "/api/aisensy/services",
          "description": "Get all available services",
          "category": "information"
        },
        {
          "action": "get-booking-status",
          "method": "GET",
          "endpoint": "/api/aisensy/booking/{bookingId}/status",
          "description": "Get booking status by ID",
          "category": "information"
        },
        {
          "action": "check-payment-status",
          "method": "GET",
          "endpoint": "/api/aisensy/payu/payment-status/{txnid}",
          "description": "Check payment status",
          "category": "payment"
        }
      ]
    }
  }
}
```

---

## 🧪 Testing the New Endpoints

### Test Customer Bookings Retrieval
```bash
# Get all bookings for a customer
curl -X GET "https://nexo.works/api/aisensy/customer/bookings/+919876543210"

# Get customer actions list
curl -X GET "https://nexo.works/api/aisensy/customer/actions"
```

### Test Partner Bookings Retrieval
```bash
# Get all bookings for a partner
curl -X GET "https://nexo.works/api/aisensy/partner/bookings/+919876543210"

# Get partner actions list
curl -X GET "https://nexo.works/api/aisensy/partner/actions"
```

### Test All Actions
```bash
# Get all available actions
curl -X GET "https://nexo.works/api/aisensy/actions"
```

---

## 📈 Integration Workflow Examples

### Customer Workflow: Check Bookings & Take Action
```javascript
// 1. Get customer's bookings
const bookingsResponse = await fetch('/api/aisensy/customer/bookings/+919876543210');
const bookings = await bookingsResponse.json();

// 2. Get available customer actions
const actionsResponse = await fetch('/api/aisensy/customer/actions');
const actions = await actionsResponse.json();

// 3. Take action based on booking status
if (bookings.data.bookings[0].status === 'completed') {
  // Submit review
  await fetch('/api/aisensy/customer/submit-review', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      bookingId: bookings.data.bookings[0].bookingId,
      customerPhone: '+919876543210',
      rating: '5',
      comment: 'Excellent service!'
    })
  });
}
```

### Partner Workflow: Check Assignments & Complete Work
```javascript
// 1. Get partner's assigned bookings
const partnerBookings = await fetch('/api/aisensy/partner/bookings/+919876543210');
const assignments = await partnerBookings.json();

// 2. Complete a booking with OTP
const completedBooking = await fetch('/api/aisensy/partner/complete-booking', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    bookingId: assignments.data.bookings[0].bookingId,
    partnerPhone: '+919876543210',
    otp: assignments.data.bookings[0].otp,
    remark: 'Work completed successfully'
  })
});
```

---

## 🎯 Summary of New Endpoints

### Customer Endpoints (5 new)
1. **GET** `/customer/bookings/{phone}` - Get all customer bookings
2. **GET** `/customer/actions` - Get available customer actions
3. **GET** `/booking/{bookingId}/status` - Get booking status (already existed)
4. **GET** `/payu/payment-status/{txnid}` - Check payment status (already existed)
5. **GET** `/actions` - Get all actions (general endpoint)

### Partner Endpoints (2 new)
1. **GET** `/partner/bookings/{partnerPhone}` - Get all partner bookings
2. **GET** `/partner/actions` - Get available partner actions

### General Endpoints (1 new)
1. **GET** `/actions` - Get all available actions (customer + partner + general)

### Total API Count: **19 Endpoints**
- **Customer Actions**: 8 endpoints
- **Partner Actions**: 6 endpoints  
- **General/Information**: 3 endpoints
- **Payment Integration**: 4 endpoints (included in counts above)

All endpoints are now fully implemented, documented, and ready for AiSensy integration! 🚀
