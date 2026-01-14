# AiSensy Booking API Documentation

## Enhanced Create Customer Booking API

### Endpoint
```
POST /api/aisensy/customer/create-booking
```

### Authentication
- **Header**: `Authorization: Bearer aisensy_nexo_2025_secure_token_v1`
- **Alternative**: `x-api-token: aisensy_nexo_2025_secure_token_v1`
- **Query Parameter**: `?token=aisensy_nexo_2025_secure_token_v1`

### Description
Creates a booking for a customer with support for:
- ✅ Base service pricing
- ✅ Add-ons (from service addons)
- ✅ Sub-services (specific items within addons)
- ✅ Visiting charges
- ✅ Coupon/Offer discounts
- ✅ Automatic tax calculation (CGST/SGST)
- ✅ PayU payment integration
- ✅ WhatsApp payment link delivery

### Request Body Parameters

#### Required Fields
| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `customerPhone` | string | 10-digit Indian mobile number | `"9876543210"` |
| `customerName` | string | Customer's full name | `"John Doe"` |
| `serviceName` | string | Name of the service to book | `"AC Service"` |
| `scheduledDate` | string | Booking date (multiple formats supported) | `"25/02/2026"` |
| `scheduledTime` | string | Booking time in HH:mm format | `"14:30"` |

#### Optional Fields
| Parameter | Type | Description | Example | Default |
|-----------|------|-------------|---------|---------|
| `customerEmail` | string | Customer's email address | `"john@example.com"` | Auto-generated |
| `serviceId` | string | Service ID (if known) | `"60f7b3b3..."` | - |
| `addOnNames` | string | Comma-separated addon names | `"Deep Cleaning,Gas Refill"` | - |
| `subServiceIds` | string/array | Subservice IDs (comma-separated or array) | `"691da2ba...,691da2bb..."` | - |
| `couponCode` | string | Coupon/offer code for discount | `"SAVE20"` | - |
| `locationAddress` | string | Full service address | `"123 Main St, City"` | "To be confirmed" |
| `locationLandmark` | string | Nearby landmark | `"Near City Mall"` | - |
| `locationPincode` | string | Area pincode | `"560001"` | - |
| `amount` | string | Manual amount override | `"2500"` | Auto-calculated |
| `paymentMode` | string | Payment method | `"online"`, `"cash"`, `"upi"` | `"online"` |
| `specialInstructions` | string | Additional instructions | `"Call before arriving"` | - |
| `lat` | string | Latitude coordinate | `"12.9716"` | - |
| `lng` | string | Longitude coordinate | `"77.5946"` | - |
| `includeVisitingCharge` | boolean | Include visiting charge | `true`, `false` | `true` |

### Date Format Support
The API supports multiple date formats:
- `DD/MM/YYYY` - `25/02/2026`
- `MM/DD/YYYY` - `02/25/2026`
- `YYYY-MM-DD` - `2026-02-25`
- `DD-MM-YYYY` - `25-02-2026`
- `MM-DD-YYYY` - `02-25-2026`

### Price Calculation Logic

```
Base Service Amount
+ Add-ons Amount (from selected addons)
+ Sub-services Amount (from selected subservices)
+ Visiting Charge (if includeVisitingCharge = true)
= Subtotal Before Discount

Subtotal Before Discount
- Discount Amount (from coupon if provided)
= Subtotal After Discount

Subtotal After Discount
+ Tax Amount (CGST + SGST)
= Final Amount
```

### Example Request

```bash
curl -X POST "http://localhost:9088/api/aisensy/customer/create-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "customerPhone": "9876543210",
    "customerName": "John Doe",
    "customerEmail": "john@example.com",
    "serviceName": "AC Service",
    "addOnNames": "Foam-jet service (2 ACs)",
    "subServiceIds": "691da2ba7cb1810b530c42f7,691da2ba7cb1810b530c42f8",
    "couponCode": "SAVE20",
    "scheduledDate": "25/02/2026",
    "scheduledTime": "14:30",
    "locationAddress": "123 Main Street, Bangalore",
    "locationLandmark": "Near City Mall",
    "locationPincode": "560001",
    "paymentMode": "online",
    "specialInstructions": "Please call before arriving",
    "lat": "12.9716",
    "lng": "77.5946",
    "includeVisitingCharge": true
  }'
```

### Success Response (201 Created)

```json
{
  "success": true,
  "message": "Booking created successfully! Use the payment link to complete payment. Payment link sent via WhatsApp.",
  "data": {
    "bookingId": "60f7b3b3b3b3b3b3b3b3b3b3",
    "customerId": "60f7b3b3b3b3b3b3b3b3b3b4",
    "customerName": "John Doe",
    "customerPhone": "9876543210",
    "serviceName": "AC Service",
    "selectedAddOns": [
      {
        "addOnId": "60f7b3b3b3b3b3b3b3b3b3b5",
        "name": "Foam-jet service (2 ACs)",
        "description": "Deep cleaning service for 2 AC units",
        "basePrice": 1000,
        "price": "₹1000",
        "type": "addon"
      }
    ],
    "selectedSubServices": [
      {
        "subServiceId": "691da2ba7cb1810b530c42f7",
        "addonId": "60f7b3b3b3b3b3b3b3b3b3b5",
        "addonName": "Foam-jet service (2 ACs)",
        "name": "Wire Plug",
        "description": "Servicing ac filter",
        "price": "300",
        "basePrice": 300,
        "icon": "FaPlug"
      },
      {
        "subServiceId": "691da2ba7cb1810b530c42f8",
        "addonId": "60f7b3b3b3b3b3b3b3b3b3b5",
        "addonName": "Foam-jet service (2 ACs)",
        "name": "Ac service",
        "description": "complete ac servicing",
        "price": "500",
        "basePrice": 500,
        "icon": "FaCheckCircle"
      }
    ],
    "priceBreakdown": {
      "baseServiceAmount": 2000,
      "addOnsAmount": 1000,
      "subServicesAmount": 800,
      "visitingCharge": 200,
      "subtotalBeforeDiscount": 4000,
      "discountPercentage": 20,
      "discountAmount": 800,
      "subtotalAfterDiscount": 3200,
      "taxAmount": 576,
      "cgst": 288,
      "sgst": 288,
      "finalAmount": 3776
    },
    "appliedCoupon": {
      "code": "SAVE20",
      "discount": 20,
      "title": "20% Off on All Services"
    },
    "status": "pending",
    "paymentStatus": "pending",
    "paymentMode": "online",
    "scheduledDate": "25/02/2026",
    "scheduledTime": "14:30",
    "location": {
      "address": "123 Main Street, Bangalore",
      "landmark": "Near City Mall",
      "pincode": "560001"
    },
    "paymentLink": "http://localhost:9088/api/aisensy/payu/payment-page/AISENSY_60f7b3b3...",
    "whatsappSent": true,
    "createdAt": "2026-01-14T10:30:00.000Z"
  }
}
```

### Error Responses

#### 400 Bad Request - Missing Required Fields
```json
{
  "success": false,
  "message": "Missing required fields: customerPhone, customerName, serviceName, scheduledDate, scheduledTime are required",
  "received": {
    "customerPhone": true,
    "customerName": false,
    "serviceName": true,
    "scheduledDate": true,
    "scheduledTime": true,
    "locationAddress": false
  }
}
```

#### 400 Bad Request - Invalid Phone Number
```json
{
  "success": false,
  "message": "Invalid phone number format. Please provide a valid 10-digit Indian mobile number."
}
```

#### 400 Bad Request - Invalid Coupon Code
```json
{
  "success": false,
  "message": "Invalid or expired coupon code: INVALID20"
}
```

#### 400 Bad Request - Coupon Already Used
```json
{
  "success": false,
  "message": "You have already used this coupon code: SAVE20"
}
```

#### 400 Bad Request - Invalid Subservice
```json
{
  "success": false,
  "message": "Subservice with ID '691da2ba7cb1810b530c42f9' not found in service 'AC Service'"
}
```

#### 400 Bad Request - Service Not Found
```json
{
  "success": false,
  "message": "Service 'Invalid Service' not found. Please provide a valid serviceName or serviceId",
  "suggestion": "Use GET /api/aisensy/services to get available services"
}
```

#### 400 Bad Request - Invalid Date Format
```json
{
  "success": false,
  "message": "Invalid date format. Supported formats: DD/MM/YYYY, MM/DD/YYYY, YYYY-MM-DD, DD-MM-YYYY, MM-DD-YYYY",
  "examples": [
    "25/02/2026 (DD/MM/YYYY)",
    "02/25/2026 (MM/DD/YYYY)",
    "2026-02-25 (YYYY-MM-DD)",
    "25-02-2026 (DD-MM-YYYY)",
    "02-25-2026 (MM-DD-YYYY)"
  ]
}
```

#### 500 Internal Server Error
```json
{
  "success": false,
  "message": "Error creating booking",
  "error": "Internal server error"
}
```

---

## Get Addon Subservices API

### Endpoint
```
GET /api/aisensy/services/:slug/addons/:addonname/subservices
```

### Authentication
Same as Create Booking API

### Description
Retrieves all subservices for a specific addon within a service. Useful for displaying available subservices before booking.

### Path Parameters
| Parameter | Type | Description | Example |
|-----------|------|-------------|---------|
| `slug` | string | Service slug | `"ac-service"` |
| `addonname` | string | Addon name (URL encoded) | `"Foam-jet service (2 ACs)"` |

### Example Request
```bash
curl -X GET "http://localhost:9088/api/aisensy/services/ac-service/addons/Foam-jet%20service%20(2%20ACs)/subservices" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

### Success Response (200 OK)
```json
{
  "success": true,
  "message": "Addon subservices fetched successfully",
  "data": [
    {
      "_id": "691da2ba7cb1810b530c42f7",
      "name": "Wire Plug",
      "shortDescription": "Servicing ac filter",
      "price": "300",
      "icon": "FaPlug"
    },
    {
      "_id": "691da2ba7cb1810b530c42f8",
      "name": "Ac service",
      "shortDescription": "complete ac servicing",
      "price": "500",
      "icon": "FaCheckCircle"
    }
  ]
}
```

### Error Response (404 Not Found)
```json
{
  "success": false,
  "message": "Addon 'Invalid Addon' not found in service 'AC Service'",
  "availableAddons": [
    "Foam-jet service (2 ACs)",
    "Foam-jet service (3 ACs)"
  ]
}
```

---

## Get Available Services API

### Endpoint
```
GET /api/aisensy/services
```

### Authentication
Same as Create Booking API

### Description
Retrieves all available active services with their addons and subservices.

### Example Request
```bash
curl -X GET "http://localhost:9088/api/aisensy/services" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

### Success Response (200 OK)
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "60f7b3b3b3b3b3b3b3b3b3b3",
      "name": "AC Service",
      "slug": "ac-service",
      "description": "Professional AC repair and maintenance",
      "basePrice": 2000,
      "visitingCharge": 200,
      "cgst": 9,
      "sgst": 9,
      "addOns": [
        {
          "_id": "60f7b3b3b3b3b3b3b3b3b3b5",
          "name": "Foam-jet service (2 ACs)",
          "description": "Deep cleaning for 2 AC units",
          "basePrice": 1000,
          "price": "₹1000",
          "subServices": [
            {
              "_id": "691da2ba7cb1810b530c42f7",
              "name": "Wire Plug",
              "shortDescription": "Servicing ac filter",
              "price": "300",
              "icon": "FaPlug"
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Integration Flow

### 1. Get Available Services
```
GET /api/aisensy/services
```
- Retrieve all services with their addons and subservices
- Display to user for selection

### 2. Get Addon Subservices (Optional)
```
GET /api/aisensy/services/{slug}/addons/{addonname}/subservices
```
- Get specific subservices for an addon
- Allow user to select individual subservices

### 3. Create Booking
```
POST /api/aisensy/customer/create-booking
```
- Include selected service, addons, subservices, and coupon
- Receive booking confirmation with price breakdown
- Get payment link for online payments

### 4. Payment (if online)
- User clicks payment link
- Redirected to PayU payment gateway
- Payment success/failure callback handled automatically

---

## Testing Examples

### Test 1: Basic Booking (No Addons/Subservices)
```json
{
  "customerPhone": "9876543210",
  "customerName": "Test User",
  "serviceName": "AC Service",
  "scheduledDate": "25/02/2026",
  "scheduledTime": "14:30",
  "locationAddress": "Test Address",
  "paymentMode": "cash"
}
```
**Expected**: Base service price + visiting charge + tax

### Test 2: Booking with Addons
```json
{
  "customerPhone": "9876543210",
  "customerName": "Test User",
  "serviceName": "AC Service",
  "addOnNames": "Foam-jet service (2 ACs)",
  "scheduledDate": "25/02/2026",
  "scheduledTime": "14:30",
  "locationAddress": "Test Address",
  "paymentMode": "cash"
}
```
**Expected**: Base + addon price + visiting charge + tax

### Test 3: Booking with Subservices
```json
{
  "customerPhone": "9876543210",
  "customerName": "Test User",
  "serviceName": "AC Service",
  "subServiceIds": "691da2ba7cb1810b530c42f7,691da2ba7cb1810b530c42f8",
  "scheduledDate": "25/02/2026",
  "scheduledTime": "14:30",
  "locationAddress": "Test Address",
  "paymentMode": "cash"
}
```
**Expected**: Base + subservices price + visiting charge + tax

### Test 4: Booking with Coupon
```json
{
  "customerPhone": "9876543210",
  "customerName": "Test User",
  "serviceName": "AC Service",
  "couponCode": "SAVE20",
  "scheduledDate": "25/02/2026",
  "scheduledTime": "14:30",
  "locationAddress": "Test Address",
  "paymentMode": "cash"
}
```
**Expected**: (Base + visiting charge - 20% discount) + tax

### Test 5: Complete Booking (All Features)
```json
{
  "customerPhone": "9876543210",
  "customerName": "Test User",
  "serviceName": "AC Service",
  "addOnNames": "Foam-jet service (2 ACs)",
  "subServiceIds": "691da2ba7cb1810b530c42f7",
  "couponCode": "SAVE20",
  "scheduledDate": "25/02/2026",
  "scheduledTime": "14:30",
  "locationAddress": "Test Address",
  "paymentMode": "online",
  "includeVisitingCharge": true
}
```
**Expected**: (Base + addon + subservice + visiting - discount) + tax + payment link

---

## Notes

1. **Phone Number Format**: Always use 10-digit Indian mobile numbers without country code
2. **Coupon Usage**: Each coupon can only be used once per user
3. **Visiting Charge**: Can be excluded by setting `includeVisitingCharge: false`
4. **Tax Calculation**: Automatically applied based on service configuration
5. **Payment Link**: Sent via WhatsApp for online payments
6. **Auto-Registration**: Users are automatically created if they don't exist
7. **Address Resolution**: If lat/lng provided without address, Google Maps API resolves it

---

## Support

For API issues or questions, contact the development team or refer to the Swagger documentation at:
```
http://localhost:9088/api-docs
```
