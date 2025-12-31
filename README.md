# AiSensy Integration API Documentation

This document describes the AiSensy integration APIs that allow creating bookings without authentication. These APIs are designed to be called by AiSensy chatbots to create service bookings on behalf of customers and partners.

## Overview

The AiSensy integration provides 4 main endpoints:

1. **Customer Booking API** - Create bookings for customers
2. **Partner Booking API** - Create bookings and assign them directly to partners
3. **Booking Status API** - Get booking status and details
4. **Services API** - Get available services and sub-services

All APIs are **public** and do **not require authentication**.

## Base URL

```
https://nexo.works/
```

## API Endpoints

### 1. Get Available Services

**Endpoint:** `GET /api/aisensy/services`

**Description:** Retrieve all available services and their sub-services.

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
      "serviceName": "AC Repair",
      "description": "Air conditioning repair services",
      "subServices": [
        {
          "subServiceId": "60f7b3b3b3b3b3b3b3b3b3b4",
          "subServiceName": "AC Installation",
          "description": "Professional AC installation",
          "price": 500
        }
      ]
    }
  ]
}
```

### 2. Create Customer Booking

**Endpoint:** `POST /api/aisensy/customer/create-booking`

**Description:** Create a booking for a customer. If the customer doesn't exist, a new user account will be created automatically.

**Request Body:**
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "serviceName": "AC Repair",
  "subServiceId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "scheduledDate": "2024-01-15",
  "scheduledTime": "14:30",
  "location": {
    "address": "123 Main Street, Bangalore",
    "landmark": "Near City Mall",
    "pincode": "560001"
  },
  "amount": 500,
  "paymentMode": "cash",
  "specialInstructions": "Please call before arriving",
  "lat": "12.9716",
  "lng": "77.5946"
}
```

**Required Fields:**
- `customerPhone` - Customer phone number
- `customerName` - Customer name
- `serviceName` - Service name (will be matched with existing services)
- `scheduledDate` - Booking date (YYYY-MM-DD format)
- `scheduledTime` - Booking time (HH:mm format)
- `location.address` - Full address
- `amount` - Service amount

**Optional Fields:**
- `customerEmail` - Customer email
- `subServiceId` - Specific sub-service ID (if known)
- `location.landmark` - Nearby landmark
- `location.pincode` - Area pincode
- `paymentMode` - Payment mode (default: "cash")
- `specialInstructions` - Special instructions
- `lat`, `lng` - GPS coordinates

**Response:**
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3b4",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair",
    "status": "pending",
    "paymentStatus": "pending",
    "scheduledDate": "2024-01-15",
    "scheduledTime": "14:30",
    "amount": 500,
    "location": {
      "address": "123 Main Street, Bangalore",
      "landmark": "Near City Mall",
      "pincode": "560001"
    },
    "createdAt": "2024-01-01T10:00:00.000Z"
  }
}
```

### 3. Create Partner Booking

**Endpoint:** `POST /api/aisensy/partner/create-booking`

**Description:** Create a booking and assign it directly to a specific partner. The partner must be registered and approved.

**Request Body:**
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543212",
  "customerName": "Jane Doe",
  "customerEmail": "jane@example.com",
  "serviceName": "Plumbing Service",
  "subServiceId": "60f7b3b3b3b3b3b3b3b3b3b5",
  "scheduledDate": "2024-01-16",
  "scheduledTime": "10:00",
  "location": {
    "address": "456 Oak Street, Bangalore",
    "landmark": "Near Park",
    "pincode": "560002"
  },
  "amount": 750,
  "paymentMode": "cash",
  "specialInstructions": "Urgent repair needed",
  "lat": "12.9716",
  "lng": "77.5946"
}
```

**Required Fields:**
- `partnerPhone` - Partner phone number (must be registered)
- `customerPhone` - Customer phone number
- `customerName` - Customer name
- `serviceName` - Service name
- `scheduledDate` - Booking date (YYYY-MM-DD format)
- `scheduledTime` - Booking time (HH:mm format)
- `location.address` - Full address
- `amount` - Service amount

**Response:**
```json
{
  "success": true,
  "message": "Booking created and assigned to partner successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b6",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3b7",
    "customerName": "Jane Doe",
    "customerPhone": "+919876543212",
    "partnerId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "partnerName": "Partner Name",
    "partnerPhone": "+919876543211",
    "serviceName": "Plumbing Service",
    "status": "accepted",
    "paymentStatus": "pending",
    "scheduledDate": "2024-01-16",
    "scheduledTime": "10:00",
    "amount": 750,
    "location": {
      "address": "456 Oak Street, Bangalore",
      "landmark": "Near Park",
      "pincode": "560002"
    },
    "otp": "123456",
    "createdAt": "2024-01-01T10:00:00.000Z",
    "acceptedAt": "2024-01-01T10:00:00.000Z"
  }
}
```

### 4. Get Booking Status

**Endpoint:** `GET /api/aisensy/booking/{bookingId}/status`

**Description:** Get the current status and details of a booking.

**Parameters:**
- `bookingId` - The booking ID returned from create booking APIs

**Response:**
```json
{
  "success": true,
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "status": "accepted",
    "paymentStatus": "pending",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair",
    "scheduledDate": "2024-01-15T00:00:00.000Z",
    "scheduledTime": "14:30",
    "amount": 500,
    "location": {
      "address": "123 Main Street, Bangalore",
      "landmark": "Near City Mall",
      "pincode": "560001"
    },
    "partnerName": "Partner Name",
    "partnerPhone": "+919876543211",
    "otp": "123456",
    "acceptedAt": "2024-01-01T10:00:00.000Z",
    "createdAt": "2024-01-01T10:00:00.000Z",
    "updatedAt": "2024-01-01T10:05:00.000Z"
  }
}
```

## Booking Status Values

- `pending` - Booking created, waiting for partner acceptance
- `accepted` - Partner has accepted the booking (OTP generated)
- `confirmed` - Booking confirmed by admin
- `in_progress` - Service is being performed
- `completed` - Service completed successfully
- `cancelled` - Booking cancelled
- `rejected` - Booking rejected by partner

## Payment Status Values

- `pending` - Payment not yet made
- `completed` - Payment completed
- `failed` - Payment failed
- `refunded` - Payment refunded

## Error Responses

All APIs return consistent error responses:

```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error message (in development mode)"
}
```

Common HTTP status codes:
- `400` - Bad Request (validation errors)
- `404` - Not Found (booking/service not found)
- `500` - Internal Server Error

## Validation Rules

### Phone Numbers
- Must be in international format (e.g., +919876543210)
- Must contain 10-15 digits after country code

### Dates and Times
- Date format: YYYY-MM-DD
- Time format: HH:mm (24-hour format)
- Date must be today or in the future

### Service Names
- Must match existing service names (case-insensitive)
- Use the services API to get available service names

### Partner Requirements
- Partner must be registered in the system
- Partner KYC status must be "approved"
- Partner must provide services matching the requested service

## Integration Examples

### AiSensy Chatbot Flow for Customer Booking

1. **Get Available Services**
   ```
   GET /api/aisensy/services
   ```

2. **Collect Customer Information**
   - Phone number
   - Name
   - Service selection
   - Date and time preference
   - Address

3. **Create Booking**
   ```
   POST /api/aisensy/customer/create-booking
   ```

4. **Provide Booking Confirmation**
   - Share booking ID
   - Inform about partner assignment process

5. **Check Status Updates**
   ```
   GET /api/aisensy/booking/{bookingId}/status
   ```

### AiSensy Chatbot Flow for Partner Booking

1. **Verify Partner**
   - Collect partner phone number
   - Validate partner exists and is approved

2. **Collect Customer and Service Information**
   - Customer details
   - Service requirements
   - Scheduling information

3. **Create Partner Booking**
   ```
   POST /api/aisensy/partner/create-booking
   ```

4. **Provide Confirmation**
   - Share booking ID and OTP
   - Confirm assignment to partner

## Testing

A test script is provided at `backend/test-aisensy-api.js` to test all endpoints:

```bash
cd backend
node test-aisensy-api.js
```

## Security Considerations

1. **Rate Limiting**: Consider implementing rate limiting for these public APIs
2. **Input Validation**: All inputs are validated server-side
3. **Phone Number Verification**: Consider adding OTP verification for phone numbers
4. **Monitoring**: Monitor API usage for abuse detection

## Notifications

The system automatically sends notifications when bookings are created:

1. **Customer Bookings**: Notifications sent to all eligible partners
2. **Partner Bookings**: Notifications sent to assigned partner and customer
3. **FCM Push Notifications**: Sent if FCM tokens are available
4. **In-App Notifications**: Stored in the database for app users

## Database Changes

The integration adds a `createdVia` field to bookings to track AiSensy-created bookings:

```javascript
{
  // ... other booking fields
  createdVia: "aisensy"
}
```

Users created via AiSensy are automatically verified and have:

```javascript
{
  // ... other user fields
  isVerified: true,
  createdVia: "aisensy"
}
```

## Support

For integration support or questions, please contact the development team.
