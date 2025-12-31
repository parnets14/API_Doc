# AiSensy Integration API Documentation

## 📋 Overview

The AiSensy Integration API enables seamless booking creation through chatbot interactions without requiring authentication. This API is specifically designed for AiSensy chatbots to create and manage service bookings on behalf of customers and partners.

### 🔗 Base URL
```
https://nexo.works/api/aisensy
```

### 🔑 Authentication
**No authentication required** - All endpoints are public and can be called directly by AiSensy chatbots.

---

## 🚀 API Endpoints

### 1. Get Available Services
**Get all available popular services and their add-ons**

```http
GET /api/aisensy/services
```

#### Response
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
      "serviceName": "AC Repair & Maintenance",
      "description": "Professional air conditioning repair and maintenance services",
      "basePrice": 500,
      "price": "Starting from ₹500",
      "addOns": [
        {
          "addOnId": "60f7b3b3b3b3b3b3b3b3b3b4",
          "addOnName": "AC Installation",
          "description": "Complete AC installation with warranty",
          "basePrice": 2500,
          "price": "₹2500"
        },
        {
          "addOnId": "60f7b3b3b3b3b3b3b3b3b3b5",
          "addOnName": "AC Deep Cleaning",
          "description": "Thorough cleaning of AC unit",
          "basePrice": 800,
          "price": "₹800"
        }
      ]
    },
    {
      "serviceId": "60f7b3b3b3b3b3b3b3b3b3b6",
      "serviceName": "Plumbing Services",
      "description": "Complete plumbing solutions for homes and offices",
      "basePrice": 300,
      "price": "Starting from ₹300",
      "addOns": [
        {
          "addOnId": "60f7b3b3b3b3b3b3b3b3b3b7",
          "addOnName": "Pipe Installation",
          "description": "New pipe installation and replacement",
          "basePrice": 1200,
          "price": "₹1200"
        }
      ]
    }
  ]
}
```

---

### 2. Create Customer Booking
**Create a booking for a customer via AiSensy**

```http
POST /api/aisensy/customer/create-booking
```

#### Request Body
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
  "specialInstructions": "Please call 30 minutes before arrival",
  "lat": "12.9352",
  "lng": "77.6245"
}
```

#### Required Fields
| Field | Type | Description |
|-------|------|-------------|
| `customerPhone` | string | Customer phone number (international format) |
| `customerName` | string | Customer full name |
| `serviceName` | string | Name of the service (must match existing service) |
| `scheduledDate` | string | Booking date (YYYY-MM-DD format) |
| `scheduledTime` | string | Booking time (HH:mm format, 24-hour) |
| `location.address` | string | Complete address |
| `amount` | number | Service amount in rupees |

#### Optional Fields
| Field | Type | Description |
|-------|------|-------------|
| `customerEmail` | string | Customer email address |
| `serviceId` | string | Specific service ID (if known) |
| `location.landmark` | string | Nearby landmark |
| `location.pincode` | string | Area pincode |
| `paymentMode` | string | Payment method (default: "cash") |
| `specialInstructions` | string | Special instructions for service provider |
| `lat` | string | Latitude coordinates |
| `lng` | string | Longitude coordinates |

#### Response
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b8",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3b9",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "status": "pending",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "amount": 500,
    "location": {
      "address": "123 Main Street, Koramangala, Bangalore",
      "landmark": "Near Forum Mall",
      "pincode": "560034"
    },
    "createdAt": "2024-01-15T10:30:00.000Z"
  }
}
```

---

### 3. Create Partner Booking
**Create a booking and assign it directly to a partner**

```http
POST /api/aisensy/partner/create-booking
```

#### Request Body
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543212",
  "customerName": "Jane Smith",
  "customerEmail": "jane@example.com",
  "serviceName": "Plumbing Services",
  "serviceId": "60f7b3b3b3b3b3b3b3b3b3b6",
  "scheduledDate": "2024-02-16",
  "scheduledTime": "10:00",
  "location": {
    "address": "456 Oak Street, Indiranagar, Bangalore",
    "landmark": "Near Metro Station",
    "pincode": "560038"
  },
  "amount": 750,
  "paymentMode": "online",
  "specialInstructions": "Emergency repair - please prioritize",
  "lat": "12.9719",
  "lng": "77.6412"
}
```

#### Additional Required Fields
| Field | Type | Description |
|-------|------|-------------|
| `partnerPhone` | string | Partner phone number (must be registered and approved) |

#### Response
```json
{
  "success": true,
  "message": "Booking created and assigned to partner successfully",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3ba",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3bb",
    "customerName": "Jane Smith",
    "customerPhone": "+919876543212",
    "partnerId": "60f7b3b3b3b3b3b3b3b3b3bc",
    "partnerName": "Rajesh Kumar",
    "partnerPhone": "+919876543211",
    "serviceName": "Plumbing Services",
    "status": "accepted",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-16",
    "scheduledTime": "10:00",
    "amount": 750,
    "location": {
      "address": "456 Oak Street, Indiranagar, Bangalore",
      "landmark": "Near Metro Station",
      "pincode": "560038"
    },
    "otp": "123456",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "acceptedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

---

### 4. Get Booking Status
**Retrieve current booking status and details**

```http
GET /api/aisensy/booking/{bookingId}/status
```

#### Path Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `bookingId` | string | Unique booking identifier |

#### Response
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
    "location": {
      "address": "123 Main Street, Koramangala, Bangalore",
      "landmark": "Near Forum Mall",
      "pincode": "560034"
    },
    "partnerName": "Rajesh Kumar",
    "partnerPhone": "+919876543211",
    "otp": "123456",
    "acceptedAt": "2024-01-15T11:00:00.000Z",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T11:00:00.000Z"
  }
}
```

---

## 📊 Status Values

### Booking Status
| Status | Description |
|--------|-------------|
| `pending` | Booking created, waiting for partner acceptance |
| `accepted` | Partner has accepted the booking |
| `confirmed` | Booking confirmed by admin |
| `in_progress` | Service is currently being performed |
| `completed` | Service completed successfully |
| `cancelled` | Booking cancelled |
| `rejected` | Booking rejected by partner |

### Payment Status
| Status | Description |
|--------|-------------|
| `pending` | Payment not yet made |
| `completed` | Payment completed successfully |
| `failed` | Payment failed |
| `refunded` | Payment refunded |

---

## ⚠️ Error Responses

All endpoints return consistent error responses:

```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error message (development mode only)"
}
```

### Common HTTP Status Codes
| Code | Description |
|------|-------------|
| `400` | Bad Request - Validation errors |
| `404` | Not Found - Resource not found |
| `500` | Internal Server Error |

### Common Error Scenarios

#### 400 Bad Request Examples
```json
{
  "success": false,
  "message": "Missing required fields: customerPhone, customerName, serviceName, scheduledDate, scheduledTime, location.address, amount are required",
  "received": {
    "customerPhone": true,
    "customerName": false,
    "serviceName": true,
    "scheduledDate": true,
    "scheduledTime": true,
    "locationAddress": false,
    "amount": true
  }
}
```

```json
{
  "success": false,
  "message": "Service 'Invalid Service' not found. Please provide a valid serviceName or serviceId",
  "suggestion": "Use GET /api/aisensy/services to get available services"
}
```

```json
{
  "success": false,
  "message": "Partner with phone number +919876543211 not found. Partner must be registered first."
}
```

---

## 🔧 Validation Rules

### Phone Numbers
- Must be in international format (e.g., `+919876543210`)
- Must contain 10-15 digits after country code
- No spaces or special characters except `+`

### Dates and Times
- **Date format**: `YYYY-MM-DD`
- **Time format**: `HH:mm` (24-hour format)
- Date must be today or in the future
- Time must be valid (00:00 to 23:59)

### Service Names
- Must match existing popular service names (case-insensitive)
- Use the services API to get available service names

### Partner Requirements
- Partner must be registered in the system
- Partner KYC status must be "approved"
- Partner must be active and available

---

## 💡 Integration Examples

### Example 1: Customer Booking Flow

```javascript
// Step 1: Get available services
const servicesResponse = await fetch('https://your-domain.com/api/aisensy/services');
const services = await servicesResponse.json();

// Step 2: Create customer booking
const bookingData = {
  customerPhone: "+919876543210",
  customerName: "John Doe",
  serviceName: "AC Repair & Maintenance",
  scheduledDate: "2024-02-15",
  scheduledTime: "14:30",
  location: {
    address: "123 Main Street, Koramangala, Bangalore"
  },
  amount: 500
};

const bookingResponse = await fetch('https://your-domain.com/api/aisensy/customer/create-booking', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(bookingData)
});

const booking = await bookingResponse.json();
console.log('Booking ID:', booking.data.bookingId);

// Step 3: Check booking status
const statusResponse = await fetch(`https://your-domain.com/api/aisensy/booking/${booking.data.bookingId}/status`);
const status = await statusResponse.json();
console.log('Booking Status:', status.data.status);
```

### Example 2: Partner Booking Flow

```javascript
// Create partner booking (direct assignment)
const partnerBookingData = {
  partnerPhone: "+919876543211",
  customerPhone: "+919876543212",
  customerName: "Jane Smith",
  serviceName: "Plumbing Services",
  scheduledDate: "2024-02-16",
  scheduledTime: "10:00",
  location: {
    address: "456 Oak Street, Indiranagar, Bangalore"
  },
  amount: 750
};

const partnerBookingResponse = await fetch('https://your-domain.com/api/aisensy/partner/create-booking', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(partnerBookingData)
});

const partnerBooking = await partnerBookingResponse.json();
console.log('Booking ID:', partnerBooking.data.bookingId);
console.log('OTP:', partnerBooking.data.otp);
```

---

## 🧪 Testing

### Automated Testing
```bash
cd backend
node test-aisensy-api.js
```

### Manual Testing with Postman
1. Import the collection: `backend/AiSensy_API_Collection.postman_collection.json`
2. Set the `baseUrl` variable to your server URL
3. Run the requests in order

### Prerequisites for Testing
- Server running and accessible
- Database with popular services configured
- For partner booking tests: approved partner with test phone number

---

## 🔔 Notifications

The system automatically sends notifications when bookings are created:

### Customer Bookings
- **Partner Notifications**: Sent to all approved partners
- **FCM Push Notifications**: If partner has FCM token
- **In-App Notifications**: Stored in database

### Partner Bookings
- **Customer Notifications**: Booking confirmation with partner details
- **Partner Notifications**: Booking assignment confirmation
- **OTP Generation**: Automatic OTP for service completion

---

## 🔒 Security Considerations

### Input Validation
- All inputs are validated server-side
- Phone number format validation
- Date/time format validation
- Required field validation
- Service name sanitization

### Rate Limiting (Recommended)
- Implement rate limiting for production use
- Monitor API usage for abuse detection
- Consider IP-based rate limiting

### Data Privacy
- Minimal data collection
- Automatic email generation for users without email
- Phone number as primary identifier
- GDPR compliance considerations

---

## 📈 Monitoring and Analytics

### Recommended Metrics
- API response times
- Success/failure rates
- Booking creation rates
- Partner assignment rates
- Notification delivery rates

### Logging
- Comprehensive request/response logging
- Error tracking with context
- Performance monitoring
- User behavior analytics

---

## 🆘 Support and Troubleshooting

### Common Issues

#### Service Not Found
**Problem**: `Service 'XYZ' not found`
**Solution**: 
1. Check available services using GET `/api/aisensy/services`
2. Ensure service name matches exactly (case-insensitive)
3. Verify service is active in admin panel

#### Partner Not Found
**Problem**: `Partner with phone number not found`
**Solution**:
1. Verify partner is registered in the system
2. Check partner KYC approval status
3. Ensure phone number format is correct

#### Invalid Date/Time
**Problem**: `Invalid time format` or `Scheduled date must be in the future`
**Solution**:
1. Use YYYY-MM-DD format for dates
2. Use HH:mm format for times (24-hour)
3. Ensure date is today or in the future

### Contact Support
For integration support or technical issues:
- Email: support@your-domain.com
- Documentation: https://your-domain.com/api-docs
- Status Page: https://status.your-domain.com

---

## 📝 Changelog

### Version 2.0.0 (Current)
- **BREAKING**: Updated to use PopularService instead of Service/SubService
- Added support for service add-ons
- Improved error messages and validation
- Enhanced notification system
- Better partner assignment logic

### Version 1.0.0
- Initial release with basic booking functionality
- Customer and partner booking APIs
- Status checking endpoint
- Basic notification system

---

*Last updated: January 2024*
