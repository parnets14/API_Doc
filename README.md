# AiSensy API - Exact Data Guide

## 🔐 Authentication Token
**Static Token**: `aisensy_nexo_2025_secure_token_v1`

---

## 📋 1. CREATE CUSTOMER BOOKING

### **Endpoint**: `POST /api/aisensy/customer/create-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "serviceName": "AC Repair",
  "addOnNames": "Deep Cleaning,Gas Refill",
  "scheduledDate": "2024-02-15",
  "scheduledTime": "14:30",
  "locationAddress": "123 Main Street, Bangalore",
  "locationLandmark": "Near City Mall",
  "locationPincode": "560001",
  "amount": "750",
  "paymentMode": "cash",
  "specialInstructions": "Please call before arriving"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking created successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerId": "65f7b3b3b3b3b3b3b3b3b3b4",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair",
    "selectedAddOns": [
      {
        "name": "Deep Cleaning",
        "basePrice": 150
      },
      {
        "name": "Gas Refill",
        "basePrice": 300
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
    "createdAt": "2024-02-14T10:30:00.000Z"
  }
}
```

---

## 📋 2. CREATE PARTNER BOOKING

### **Endpoint**: `POST /api/aisensy/partner/create-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543210",
  "customerName": "Jane Doe",
  "customerEmail": "jane@example.com",
  "serviceName": "Plumbing Service",
  "addOnNames": "Pipe Replacement,Emergency Service",
  "scheduledDate": "2024-02-16",
  "scheduledTime": "10:00",
  "locationAddress": "456 Oak Street, Mumbai",
  "locationLandmark": "Near Park",
  "locationPincode": "400001",
  "amount": "1200",
  "paymentMode": "online",
  "specialInstructions": "Urgent repair needed"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking created and assigned to partner successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b5",
    "customerId": "65f7b3b3b3b3b3b3b3b3b3b6",
    "customerName": "Jane Doe",
    "customerPhone": "+919876543210",
    "partnerId": "65f7b3b3b3b3b3b3b3b3b3b7",
    "partnerName": "Partner Name",
    "partnerPhone": "+919876543211",
    "serviceName": "Plumbing Service",
    "selectedAddOns": [
      {
        "name": "Pipe Replacement",
        "basePrice": 300
      }
    ],
    "status": "accepted",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-16",
    "scheduledTime": "10:00",
    "amount": 1200,
    "location": {
      "address": "456 Oak Street, Mumbai",
      "landmark": "Near Park",
      "pincode": "400001"
    },
    "otp": "123456",
    "createdAt": "2024-02-14T11:00:00.000Z",
    "acceptedAt": "2024-02-14T11:00:00.000Z"
  }
}
```

---

## 📋 3. SIMPLE BOOKING (Minimal Data)

### **Endpoint**: `POST /api/aisensy/simple-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "phone": "+919876543210",
  "name": "Customer Name",
  "service": "AC Repair",
  "amount": "500"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Simple booking created successfully! Customer will be contacted to confirm details.",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b8",
    "customerId": "65f7b3b3b3b3b3b3b3b3b3b9",
    "customerName": "Customer Name",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "status": "pending",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "10:00",
    "amount": 500,
    "location": {
      "address": "Address to be confirmed",
      "landmark": "",
      "pincode": ""
    },
    "createdAt": "2024-02-14T12:00:00.000Z",
    "note": "This is a simplified booking. Customer will be contacted to confirm address and other details."
  }
}
```

---

## 📋 4. MINIMAL BOOKING (Phone Only)

### **Endpoint**: `POST /api/aisensy/minimal-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "phone": "+919876543210"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking created! Our team will contact you shortly to confirm details.",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3ba",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "10:00",
    "amount": 500,
    "status": "pending",
    "message": "Customer service will call to confirm address and service details"
  }
}
```

---

## 📋 5. GET CUSTOMER BOOKINGS

### **Endpoint**: `GET /api/aisensy/customer/bookings/{phone}`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **URL Example**:
```
GET /api/aisensy/customer/bookings/+919876543210
```

### **Response**:
```json
{
  "success": true,
  "data": {
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "totalBookings": 2,
    "bookings": [
      {
        "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
        "serviceName": "AC Repair & Maintenance",
        "status": "completed",
        "paymentStatus": "completed",
        "scheduledDate": "2024-02-15T00:00:00.000Z",
        "scheduledTime": "14:30",
        "amount": 750,
        "location": {
          "address": "123 Main Street, Bangalore",
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
        "createdAt": "2024-02-14T10:30:00.000Z",
        "updatedAt": "2024-02-15T16:00:00.000Z",
        "createdVia": "aisensy"
      }
    ]
  }
}
```

---

## 📋 6. GET PARTNER BOOKINGS

### **Endpoint**: `GET /api/aisensy/partner/bookings/{partnerPhone}`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **URL Example**:
```
GET /api/aisensy/partner/bookings/+919876543211
```

### **Response**:
```json
{
  "success": true,
  "data": {
    "partnerPhone": "+919876543211",
    "partnerName": "Partner Name",
    "totalBookings": 3,
    "bookings": [
      {
        "bookingId": "65f7b3b3b3b3b3b3b3b3b3b5",
        "serviceName": "Plumbing Service",
        "status": "accepted",
        "paymentStatus": "pending",
        "scheduledDate": "2024-02-16T00:00:00.000Z",
        "scheduledTime": "10:00",
        "amount": 1200,
        "location": {
          "address": "456 Oak Street, Mumbai",
          "landmark": "Near Park",
          "pincode": "400001"
        },
        "selectedAddOns": [
          {
            "name": "Pipe Replacement",
            "basePrice": 300
          }
        ],
        "customerInfo": {
          "name": "Jane Doe",
          "phone": "+919876543210",
          "email": "jane@example.com"
        },
        "otp": "123456",
        "acceptedAt": "2024-02-14T11:00:00.000Z",
        "completedAt": null,
        "createdAt": "2024-02-14T11:00:00.000Z",
        "updatedAt": "2024-02-14T11:00:00.000Z",
        "createdVia": "aisensy"
      }
    ]
  }
}
```

---

## 📋 7. CANCEL BOOKING

### **Endpoint**: `PUT /api/aisensy/customer/cancel-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "cancellationReason": "Change of plans"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking cancelled successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "status": "cancelled",
    "cancellationReason": "Change of plans",
    "cancellationTime": "2024-02-14T12:30:00.000Z"
  }
}
```

---

## 📋 8. SUBMIT REVIEW

### **Endpoint**: `POST /api/aisensy/customer/submit-review`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "rating": "5",
  "comment": "Excellent service! Very professional and timely."
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Review submitted successfully! It will be published after moderation.",
  "data": {
    "reviewId": "65f7b3b3b3b3b3b3b3b3b3bb",
    "rating": 5,
    "comment": "Excellent service! Very professional and timely.",
    "status": "pending",
    "submittedAt": "2024-02-14T13:00:00.000Z"
  }
}
```

---

## 📋 9. PARTNER ACCEPT BOOKING

### **Endpoint**: `PUT /api/aisensy/partner/accept-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking accepted successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerId": "65f7b3b3b3b3b3b3b3b3b3b7",
    "partnerName": "Partner Name",
    "status": "accepted",
    "acceptedAt": "2024-02-14T14:00:00.000Z",
    "otp": "123456",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair"
  }
}
```

---

## 📋 10. PARTNER COMPLETE BOOKING

### **Endpoint**: `PUT /api/aisensy/partner/complete-booking`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211",
  "otp": "123456",
  "remark": "Work completed successfully"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Booking completed successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerId": "65f7b3b3b3b3b3b3b3b3b3b7",
    "partnerName": "Partner Name",
    "status": "completed",
    "completedAt": "2024-02-14T16:00:00.000Z",
    "remark": "Work completed successfully",
    "customerName": "John Doe",
    "serviceName": "AC Repair"
  }
}
```

---

## �  PAYMENT APIs

## 📋 11. INITIATE PAYMENT

### **Endpoint**: `POST /api/aisensy/payu/initiate-payment`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
Content-Type: application/json
```

### **Request Body** (Exact Format):
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "customerEmail": "john@example.com",
  "amount": "750",
  "productInfo": "Service Booking Payment"
}
```

### **Response**:
```json
{
  "success": true,
  "message": "Payment initiated successfully",
  "data": {
    "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "amount": 750,
    "payuData": {
      "action": "https://secure.payu.in/_payment",
      "params": {
        "key": "81Jp42",
        "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
        "amount": "750",
        "productinfo": "Service Booking Payment - AC Repair",
        "firstname": "John Doe",
        "email": "john@example.com",
        "phone": "+919876543210",
        "surl": "https://nexo.works/api/aisensy/payu/payment-success",
        "furl": "https://nexo.works/api/aisensy/payu/payment-failure",
        "hash": "generated_hash_value"
      }
    },
    "paymentUrl": "https://secure.payu.in/_payment?key=81Jp42&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750&productinfo=Service%20Booking%20Payment%20-%20AC%20Repair&firstname=John%20Doe&email=john@example.com&phone=+919876543210&surl=https://nexo.works/api/aisensy/payu/payment-success&furl=https://nexo.works/api/aisensy/payu/payment-failure&hash=generated_hash_value"
  }
}
```

---

## 📋 12. PAYMENT SUCCESS CALLBACK

### **Endpoint**: `POST /api/aisensy/payu/payment-success`

### **Description**: PayU automatically calls this endpoint when payment is successful

### **Headers**:
```
Content-Type: application/x-www-form-urlencoded
```

### **Request Body** (PayU sends this data):
```
key=81Jp42&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750.00&productinfo=Service%20Booking%20Payment%20-%20AC%20Repair&firstname=John%20Doe&email=john@example.com&phone=+919876543210&mihpayid=403993715527806454&status=success&hash=generated_response_hash&field1=&field2=&field3=&field4=&field5=&field6=&field7=&field8=&field9=&PG_TYPE=CC&bank_ref_num=1708001234567&bankcode=CC&error=E000&cardhash=This_is_the_card_hash_if_payment_is_done_via_CC
```

### **Response** (Redirects customer):
```html
<!DOCTYPE html>
<html>
<head>
    <title>Payment Successful</title>
    <script>
        window.location.href = "https://nexo.works/payment-result?status=success&bookingId=65f7b3b3b3b3b3b3b3b3b3b3&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750";
    </script>
</head>
<body>
    <p>Payment successful! Redirecting...</p>
</body>
</html>
```

---

## 📋 13. PAYMENT FAILURE CALLBACK

### **Endpoint**: `POST /api/aisensy/payu/payment-failure`

### **Description**: PayU automatically calls this endpoint when payment fails

### **Headers**:
```
Content-Type: application/x-www-form-urlencoded
```

### **Request Body** (PayU sends this data):
```
key=81Jp42&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750.00&productinfo=Service%20Booking%20Payment%20-%20AC%20Repair&firstname=John%20Doe&email=john@example.com&phone=+919876543210&mihpayid=&status=failure&hash=generated_response_hash&field1=&field2=&field3=&field4=&field5=&field6=&field7=&field8=&field9=&PG_TYPE=&bank_ref_num=&bankcode=&error=E002&error_Message=Transaction%20cancelled%20by%20user
```

### **Response** (Redirects customer):
```html
<!DOCTYPE html>
<html>
<head>
    <title>Payment Failed</title>
    <script>
        window.location.href = "https://nexo.works/payment-result?status=failed&bookingId=65f7b3b3b3b3b3b3b3b3b3b3&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&reason=Transaction%20cancelled%20by%20user";
    </script>
</head>
<body>
    <p>Payment failed! Redirecting...</p>
</body>
</html>
```

---

## 📋 14. CHECK PAYMENT STATUS BY TRANSACTION ID

### **Endpoint**: `GET /api/aisensy/payu/payment-status/{txnid}`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **URL Example**:
```
GET /api/aisensy/payu/payment-status/AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000
```

### **Response**:
```json
{
  "success": true,
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
    "paymentStatus": "completed",
    "paymentMode": "online",
    "amount": 750,
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "paymentDetails": {
      "gateway": "payu",
      "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
      "amount": 750,
      "initiatedAt": "2024-02-14T10:30:00.000Z",
      "completedAt": "2024-02-14T10:35:00.000Z",
      "initiatedVia": "aisensy",
      "mihpayid": "403993715527806454",
      "bankRefNum": "1708001234567"
    }
  }
}
```

---

## 📋 15. CHECK PAYMENT STATUS BY BOOKING ID

### **Endpoint**: `GET /api/aisensy/payu/booking-payment-status/{bookingId}`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **URL Example**:
```
GET /api/aisensy/payu/booking-payment-status/65f7b3b3b3b3b3b3b3b3b3b3
```

### **Response**:
```json
{
  "success": true,
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
    "paymentStatus": "completed",
    "paymentMode": "online",
    "amount": 750,
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair & Maintenance",
    "paymentDetails": {
      "gateway": "payu",
      "txnid": "AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000",
      "amount": 750,
      "initiatedAt": "2024-02-14T10:30:00.000Z",
      "completedAt": "2024-02-14T10:35:00.000Z",
      "initiatedVia": "aisensy",
      "mihpayid": "403993715527806454",
      "bankRefNum": "1708001234567"
    }
  }
}
```

---

## 💳 PAYMENT WORKFLOW EXAMPLE

### **Step 1: Initiate Payment**
```bash
curl -X POST "https://nexo.works/api/aisensy/payu/initiate-payment" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "amount": "750"
  }'
```

### **Step 2: Redirect Customer to PayU**
Use the `paymentUrl` from the response to redirect customer to PayU payment page.

### **Step 3: PayU Processes Payment**
PayU will automatically call success/failure callback URLs based on payment result.

### **Step 4: Check Payment Status**
```bash
curl -X GET "https://nexo.works/api/aisensy/payu/payment-status/AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

---

## 💳 PAYMENT ERROR RESPONSES

### **Booking Not Found (404)**:
```json
{
  "success": false,
  "message": "Booking not found"
}
```

### **Payment Already Completed (400)**:
```json
{
  "success": false,
  "message": "Payment has already been completed for this booking"
}
```

### **Invalid Amount (400)**:
```json
{
  "success": false,
  "message": "Invalid amount. Must be a positive number"
}
```

### **Customer Phone Mismatch (400)**:
```json
{
  "success": false,
  "message": "Customer phone number does not match booking"
}
```

---

## 💳 PAYMENT STATUS VALUES

| Status | Description |
|--------|-------------|
| `pending` | Payment not yet initiated |
| `initiated` | Payment process started |
| `completed` | Payment successful |
| `failed` | Payment failed |
| `cancelled` | Payment cancelled by user |

---

## 💳 PAYU INTEGRATION DETAILS

### **PayU Configuration**:
- **Merchant Key**: `81Jp42`
- **Environment**: Production (`https://secure.payu.in`)
- **Success URL**: `https://nexo.works/api/aisensy/payu/payment-success`
- **Failure URL**: `https://nexo.works/api/aisensy/payu/payment-failure`

### **Transaction ID Format**:
```
AISENSY_{bookingId}_{timestamp}
```

### **Hash Generation**:
PayU uses SHA512 hash for security. The system automatically generates and verifies hashes.

### **Supported Payment Methods**:
- Credit Cards
- Debit Cards
- Net Banking
- UPI
- Wallets

---

## 📋 16. GET BOOKING STATUS

### **Endpoint**: `GET /api/aisensy/booking/{bookingId}/status`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **URL Example**:
```
GET /api/aisensy/booking/65f7b3b3b3b3b3b3b3b3b3b3/status
```

### **Response**:
```json
{
  "success": true,
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "status": "completed",
    "paymentStatus": "completed",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair",
    "scheduledDate": "2024-02-15T00:00:00.000Z",
    "scheduledTime": "14:30",
    "amount": 750,
    "location": {
      "address": "123 Main Street, Bangalore",
      "landmark": "Near City Mall",
      "pincode": "560001"
    },
    "partnerName": "Partner Name",
    "partnerPhone": "+919876543211",
    "completedAt": "2024-02-15T16:00:00.000Z",
    "createdAt": "2024-02-14T10:30:00.000Z",
    "updatedAt": "2024-02-15T16:00:00.000Z"
  }
}
```

---

## 📋 17. GET AVAILABLE SERVICES

### **Endpoint**: `GET /api/aisensy/services`

### **Headers**:
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

### **Response**:
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "65f7b3b3b3b3b3b3b3b3b3bc",
      "serviceName": "AC Repair & Maintenance",
      "description": "Complete AC repair and maintenance services",
      "basePrice": 500,
      "price": "₹500",
      "addOns": [
        {
          "addOnId": "65f7b3b3b3b3b3b3b3b3b3bd",
          "addOnName": "AC Maintenance",
          "description": "Regular maintenance services",
          "basePrice": 200,
          "price": "₹200"
        },
        {
          "addOnId": "65f7b3b3b3b3b3b3b3b3b3be",
          "addOnName": "Deep Cleaning",
          "description": "Deep cleaning of AC unit",
          "basePrice": 150,
          "price": "₹150"
        }
      ]
    },
    {
      "serviceId": "65f7b3b3b3b3b3b3b3b3b3bf",
      "serviceName": "Plumbing Service",
      "description": "Professional plumbing services",
      "basePrice": 300,
      "price": "₹300",
      "addOns": [
        {
          "addOnId": "65f7b3b3b3b3b3b3b3b3b3c0",
          "addOnName": "Pipe Replacement",
          "description": "Replace damaged pipes",
          "basePrice": 500,
          "price": "₹500"
        }
      ]
    }
  ]
}
```

---

## 🧪 CURL Examples

### Create Customer Booking:
```bash
curl -X POST "https://nexo.works/api/aisensy/customer/create-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "serviceName": "AC Repair",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "locationAddress": "123 Main Street, Bangalore",
    "amount": "750"
  }'
```

### Get Customer Bookings:
```bash
curl -X GET "https://nexo.works/api/aisensy/customer/bookings/+919876543210" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

### Get Partner Bookings:
```bash
curl -X GET "https://nexo.works/api/aisensy/partner/bookings/+919876543211" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

### Initiate Payment:
```bash
curl -X POST "https://nexo.works/api/aisensy/payu/initiate-payment" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "amount": "750"
  }'
```

### Check Payment Status:
```bash
curl -X GET "https://nexo.works/api/aisensy/payu/payment-status/AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

---

## 🔑 Key Points

1. **Token Required**: All endpoints require `aisensy_nexo_2025_secure_token_v1`
2. **String Format**: All numeric values must be sent as strings
3. **Phone Format**: Use international format with + (e.g., `+919876543210`)
4. **Date Format**: Use YYYY-MM-DD format (e.g., `2024-02-15`)
5. **Time Format**: Use HH:mm format (e.g., `14:30`)
6. **Add-ons**: Send as comma-separated string (e.g., `"Deep Cleaning,Gas Refill"`)

---

## 📊 Total Endpoints: 17

- **Customer Actions**: 8 endpoints
- **Partner Actions**: 6 endpoints  
- **Payment Integration**: 5 endpoints
- **General/Information**: 3 endpoints

All endpoints are secured with static token authentication! 🔐
