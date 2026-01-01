# AiSensy Complete API Reference

## 🔗 Base URL
```
https://nexo.works/api/aisensy
```

## 🔐 Authentication
**Required for ALL endpoints**
```
Authorization: Bearer aisensy_nexo_2025_secure_token_v1
```

---

# 📋 CUSTOMER APIs (8 Endpoints)

## 1. Create Customer Booking
**Create a new service booking for a customer**

**POST** `/customer/create-booking`

**Request**:
```json
{
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "serviceName": "AC Repair",
  "scheduledDate": "2024-02-15",
  "scheduledTime": "14:30",
  "locationAddress": "123 Main Street, Bangalore"
}
```

**Optional**:
```json
{
  "customerEmail": "john@example.com",
  "serviceId": "65f7b3b3b3b3b3b3b3b3b3bc",
  "addOnNames": "Deep Cleaning,Gas Refill",
  "locationLandmark": "Near City Mall",
  "locationPincode": "560001",
  "amount": "750",
  "paymentMode": "cash",
  "specialInstructions": "Please call before arriving",
  "lat": "12.9716",
  "lng": "77.5946"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Booking created successfully with auto-calculated amount",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerId": "65f7b3b3b3b3b3b3b3b3b3b4",
    "customerName": "John Doe",
    "customerPhone": "+919876543210",
    "serviceName": "AC Repair",
    "selectedAddOns": [
      {
        "name": "Deep Cleaning",
        "basePrice": 150,
        "type": "subservice"
      },
      {
        "name": "Gas Refill",
        "basePrice": 300,
        "type": "addon"
      }
    ],
    "status": "pending",
    "paymentStatus": "pending",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "amount": 950,
    "amountBreakdown": {
      "baseService": 500,
      "addOnsSubServices": 450,
      "total": 950,
      "calculationMethod": "calculated"
    },
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

## 2. Simple Booking
**Create booking with minimal data - auto-fills missing details**

**POST** `/simple-booking`

**Request**:
```json
{
  "phone": "+919876543210"
}
```

**Optional**:
```json
{
  "name": "Customer Name",
  "service": "AC Repair",
  "amount": "500"
}
```

**Response**:
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

## 3. Minimal Booking
**Create booking with phone only - everything auto-filled**

**POST** `/minimal-booking`

**Request**:
```json
{
  "phone": "+919876543210"
}
```

**Response**:
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

## 4. Cancel Booking
**Cancel an existing booking**

**PUT** `/customer/cancel-booking`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "cancellationReason": "Change of plans"
}
```

**Response**:
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

## 5. Submit Review
**Submit review for completed booking**

**POST** `/customer/submit-review`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "rating": "5",
  "comment": "Excellent service! Very professional and timely."
}
```

**Response**:
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

## 6. Quotation Action
**Accept or reject partner quotation**

**PUT** `/customer/quotation-action`

**Request**:
```json
{
  "quotationId": "65f7b3b3b3b3b3b3b3b3b3bb",
  "customerPhone": "+919876543210",
  "action": "accept"
}
```

**For Rejection**:
```json
{
  "quotationId": "65f7b3b3b3b3b3b3b3b3b3bb",
  "customerPhone": "+919876543210",
  "action": "reject",
  "rejectionReason": "Too expensive"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Quotation accepted successfully",
  "data": {
    "quotationId": "65f7b3b3b3b3b3b3b3b3b3bb",
    "status": "accepted",
    "action": "accept",
    "actionDate": "2024-02-14T15:00:00.000Z"
  }
}
```

---

## 7. Get Customer Bookings
**Get all bookings for a customer by phone**

**GET** `/customer/bookings/{phone}`

**Example**: `GET /customer/bookings/+919876543210`

**Response**:
```json
{
  "success": true,
  "data": {
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "totalBookings": 3,
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

## 8. Get Customer Actions
**Get all available customer API actions**

**GET** `/customer/actions`

**Response**:
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
        "requiredFields": ["customerPhone", "customerName", "serviceName", "scheduledDate", "scheduledTime", "locationAddress"],
        "optionalFields": ["customerEmail", "serviceId", "addOnNames", "locationLandmark", "locationPincode", "paymentMode", "specialInstructions", "lat", "lng"]
      },
      {
        "action": "simple-booking",
        "method": "POST",
        "endpoint": "/api/aisensy/simple-booking",
        "description": "Create booking with minimal data (auto-fills missing details)",
        "requiredFields": ["phone"],
        "optionalFields": ["name", "service", "amount"]
      }
    ]
  }
}
```

---

# 🔧 PARTNER APIs (6 Endpoints)

## 9. Create Partner Booking
**Create booking and assign directly to partner**

**POST** `/partner/create-booking`

**Request**:
```json
{
  "partnerPhone": "+919876543211",
  "customerPhone": "+919876543210",
  "customerName": "Jane Doe",
  "serviceName": "Plumbing Service",
  "scheduledDate": "2024-02-16",
  "scheduledTime": "10:00",
  "locationAddress": "456 Oak Street, Mumbai"
}
```

**Optional**:
```json
{
  "customerEmail": "jane@example.com",
  "serviceId": "65f7b3b3b3b3b3b3b3b3b3bf",
  "addOnNames": "Pipe Replacement,Emergency Service",
  "locationLandmark": "Near Park",
  "locationPincode": "400001",
  "amount": "1200",
  "paymentMode": "online",
  "specialInstructions": "Urgent repair needed",
  "lat": "19.0760",
  "lng": "72.8777"
}
```

**Response**:
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

## 10. Accept Booking
**Partner accepts a pending booking**

**PUT** `/partner/accept-booking`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211"
}
```

**Response**:
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

## 11. Reject Booking
**Partner rejects a booking assignment**

**PUT** `/partner/reject-booking`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211",
  "rejectionReason": "Not available at that time"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Booking rejected successfully",
  "data": {
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerId": "65f7b3b3b3b3b3b3b3b3b3b7",
    "partnerName": "Partner Name",
    "status": "pending",
    "rejectionReason": "Not available at that time",
    "rejectedAt": "2024-02-14T14:30:00.000Z"
  }
}
```

---

## 12. Complete Booking
**Partner completes booking using customer OTP**

**PUT** `/partner/complete-booking`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211",
  "otp": "123456"
}
```

**Optional**:
```json
{
  "remark": "Work completed successfully"
}
```

**Response**:
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

## 13. Create Quotation
**Partner creates quotation for additional work**

**POST** `/partner/create-quotation`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "partnerPhone": "+919876543211",
  "items": [
    {
      "description": "Additional repair work",
      "quantity": "1",
      "unitPrice": "200",
      "totalPrice": "200"
    },
    {
      "description": "Replacement parts",
      "quantity": "2",
      "unitPrice": "150",
      "totalPrice": "300"
    }
  ],
  "totalAmount": "500"
}
```

**Optional**:
```json
{
  "notes": "Additional work required for complete repair"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Quotation created successfully",
  "data": {
    "quotationId": "65f7b3b3b3b3b3b3b3b3b3bb",
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerId": "65f7b3b3b3b3b3b3b3b3b3b7",
    "partnerName": "Partner Name",
    "items": [
      {
        "description": "Additional repair work",
        "quantity": 1,
        "unitPrice": 200,
        "totalPrice": 200
      },
      {
        "description": "Replacement parts",
        "quantity": 2,
        "unitPrice": 150,
        "totalPrice": 300
      }
    ],
    "totalAmount": 500,
    "notes": "Additional work required for complete repair",
    "customerStatus": "pending",
    "adminStatus": "pending",
    "createdAt": "2024-02-14T15:30:00.000Z"
  }
}
```

---

## 14. Get Partner Bookings
**Get all bookings assigned to partner**

**GET** `/partner/bookings/{partnerPhone}`

**Example**: `GET /partner/bookings/+919876543211`

**Response**:
```json
{
  "success": true,
  "data": {
    "partnerPhone": "+919876543211",
    "partnerName": "Partner Name",
    "totalBookings": 5,
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

## 15. Get Partner Actions
**Get all available partner API actions**

**GET** `/partner/actions`

**Response**:
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
        "requiredFields": ["partnerPhone", "customerPhone", "customerName", "serviceName", "scheduledDate", "scheduledTime", "locationAddress"],
        "optionalFields": ["customerEmail", "serviceId", "addOnNames", "locationLandmark", "locationPincode", "paymentMode", "specialInstructions", "lat", "lng"]
      },
      {
        "action": "accept-booking",
        "method": "PUT",
        "endpoint": "/api/aisensy/partner/accept-booking",
        "description": "Accept a pending booking",
        "requiredFields": ["bookingId", "partnerPhone"],
        "optionalFields": []
      }
    ]
  }
}
```

---

# 💳 PAYMENT APIs (5 Endpoints)

## 16. Initiate Payment
**Start PayU payment process for booking**

**POST** `/payu/initiate-payment`

**Request**:
```json
{
  "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
  "customerPhone": "+919876543210",
  "customerName": "John Doe",
  "amount": "750"
}
```

**Optional**:
```json
{
  "customerEmail": "john@example.com",
  "productInfo": "Service Booking Payment"
}
```

**Response**:
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

## 17. Payment Success Callback
**PayU calls this when payment succeeds (automatic)**

**POST** `/payu/payment-success`

**PayU sends this data**:
```
key=81Jp42&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750.00&productinfo=Service%20Booking%20Payment&firstname=John%20Doe&email=john@example.com&phone=+919876543210&mihpayid=403993715527806454&status=success&hash=generated_response_hash
```

**Response** (Redirects customer):
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

## 18. Payment Failure Callback
**PayU calls this when payment fails (automatic)**

**POST** `/payu/payment-failure`

**PayU sends this data**:
```
key=81Jp42&txnid=AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000&amount=750.00&productinfo=Service%20Booking%20Payment&firstname=John%20Doe&email=john@example.com&phone=+919876543210&mihpayid=&status=failure&hash=generated_response_hash&error=E002&error_Message=Transaction%20cancelled%20by%20user
```

**Response** (Redirects customer):
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

## 19. Check Payment Status by Transaction ID
**Check payment status using PayU transaction ID**

**GET** `/payu/payment-status/{txnid}`

**Example**: `GET /payu/payment-status/AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000`

**Response**:
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

## 20. Check Payment Status by Booking ID
**Check payment status using booking ID**

**GET** `/payu/booking-payment-status/{bookingId}`

**Example**: `GET /payu/booking-payment-status/65f7b3b3b3b3b3b3b3b3b3b3`

**Response**:
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

# 📊 INFORMATION APIs (3 Endpoints)

## 21. Get Available Services
**Get list of all available services with add-ons**

**GET** `/services`

**Response**:
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

## 22. Get Booking Status
**Get detailed status of specific booking**

**GET** `/booking/{bookingId}/status`

**Example**: `GET /booking/65f7b3b3b3b3b3b3b3b3b3b3/status`

**Response**:
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
    "acceptedAt": "2024-02-14T14:00:00.000Z",
    "completedAt": "2024-02-15T16:00:00.000Z",
    "createdAt": "2024-02-14T10:30:00.000Z",
    "updatedAt": "2024-02-15T16:00:00.000Z"
  }
}
```

---

## 23. Get All Actions
**Get complete list of all available API actions**

**GET** `/actions`

**Response**:
```json
{
  "success": true,
  "message": "All available AiSensy API actions",
  "data": {
    "totalActions": 17,
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
          "action": "get-bookings",
          "method": "GET",
          "endpoint": "/api/aisensy/customer/bookings/{phone}",
          "description": "Get all customer bookings",
          "category": "information"
        },
        {
          "action": "get-customer-actions",
          "method": "GET",
          "endpoint": "/api/aisensy/customer/actions",
          "description": "Get available customer actions",
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
          "action": "get-all-actions",
          "method": "GET",
          "endpoint": "/api/aisensy/actions",
          "description": "Get all available actions",
          "category": "information"
        }
      ]
    }
  }
}
```

---

# 🧪 CURL Test Commands

## Customer APIs
```bash
# 1. Create Customer Booking
curl -X POST "https://nexo.works/api/aisensy/customer/create-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "serviceName": "AC Repair",
    "scheduledDate": "2024-02-15",
    "scheduledTime": "14:30",
    "locationAddress": "123 Main Street"
  }'

# 2. Simple Booking
curl -X POST "https://nexo.works/api/aisensy/simple-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{"phone": "+919876543210"}'

# 3. Minimal Booking
curl -X POST "https://nexo.works/api/aisensy/minimal-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{"phone": "+919876543210"}'

# 4. Cancel Booking
curl -X PUT "https://nexo.works/api/aisensy/customer/cancel-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "cancellationReason": "Change of plans"
  }'

# 5. Submit Review
curl -X POST "https://nexo.works/api/aisensy/customer/submit-review" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "rating": "5",
    "comment": "Excellent service!"
  }'

# 6. Quotation Action
curl -X PUT "https://nexo.works/api/aisensy/customer/quotation-action" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "quotationId": "65f7b3b3b3b3b3b3b3b3b3bb",
    "customerPhone": "+919876543210",
    "action": "accept"
  }'

# 7. Get Customer Bookings
curl -X GET "https://nexo.works/api/aisensy/customer/bookings/+919876543210" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"

# 8. Get Customer Actions
curl -X GET "https://nexo.works/api/aisensy/customer/actions" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

## Partner APIs
```bash
# 9. Create Partner Booking
curl -X POST "https://nexo.works/api/aisensy/partner/create-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "partnerPhone": "+919876543211",
    "customerPhone": "+919876543210",
    "customerName": "Jane Doe",
    "serviceName": "Plumbing Service",
    "scheduledDate": "2024-02-16",
    "scheduledTime": "10:00",
    "locationAddress": "456 Oak Street"
  }'

# 10. Accept Booking
curl -X PUT "https://nexo.works/api/aisensy/partner/accept-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerPhone": "+919876543211"
  }'

# 11. Reject Booking
curl -X PUT "https://nexo.works/api/aisensy/partner/reject-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerPhone": "+919876543211",
    "rejectionReason": "Not available"
  }'

# 12. Complete Booking
curl -X PUT "https://nexo.works/api/aisensy/partner/complete-booking" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerPhone": "+919876543211",
    "otp": "123456"
  }'

# 13. Create Quotation
curl -X POST "https://nexo.works/api/aisensy/partner/create-quotation" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "partnerPhone": "+919876543211",
    "items": [
      {
        "description": "Additional repair",
        "quantity": "1",
        "unitPrice": "200",
        "totalPrice": "200"
      }
    ],
    "totalAmount": "200"
  }'

# 14. Get Partner Bookings
curl -X GET "https://nexo.works/api/aisensy/partner/bookings/+919876543211" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"

# 15. Get Partner Actions
curl -X GET "https://nexo.works/api/aisensy/partner/actions" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

## Payment APIs
```bash
# 16. Initiate Payment
curl -X POST "https://nexo.works/api/aisensy/payu/initiate-payment" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1" \
  -H "Content-Type: application/json" \
  -d '{
    "bookingId": "65f7b3b3b3b3b3b3b3b3b3b3",
    "customerPhone": "+919876543210",
    "customerName": "John Doe",
    "amount": "750"
  }'

# 19. Check Payment Status by Transaction ID
curl -X GET "https://nexo.works/api/aisensy/payu/payment-status/AISENSY_65f7b3b3b3b3b3b3b3b3b3b3_1708000000000" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"

# 20. Check Payment Status by Booking ID
curl -X GET "https://nexo.works/api/aisensy/payu/booking-payment-status/65f7b3b3b3b3b3b3b3b3b3b3" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

## Information APIs
```bash
# 21. Get Available Services
curl -X GET "https://nexo.works/api/aisensy/services" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"

# 22. Get Booking Status
curl -X GET "https://nexo.works/api/aisensy/booking/65f7b3b3b3b3b3b3b3b3b3b3/status" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"

# 23. Get All Actions
curl -X GET "https://nexo.works/api/aisensy/actions" \
  -H "Authorization: Bearer aisensy_nexo_2025_secure_token_v1"
```

---

# 📋 Data Format Rules

## Required Formats
- **Phone**: `+919876543210` (international format with +)
- **Date**: `2024-02-15` (YYYY-MM-DD format)
- **Time**: `14:30` (HH:mm 24-hour format)
- **Amount**: `"750"` (string format, not number)
- **Rating**: `"5"` (string format, 1-5 scale)
- **Quantity**: `"1"` (string format in quotations)

## Add-ons Format
- **Single**: `"Deep Cleaning"`
- **Multiple**: `"Deep Cleaning,Gas Refill,Filter Replacement"`
- **Note**: Comma-separated, no spaces after commas

## Authentication
- **Header**: `Authorization: Bearer aisensy_nexo_2025_secure_token_v1`
- **Required**: For ALL endpoints

---

# 📊 API Summary

| Category | Count | Endpoints |
|----------|-------|-----------|
| **Customer** | 8 | Create, Simple, Minimal, Cancel, Review, Quotation, Get Bookings, Get Actions |
| **Partner** | 6 | Create, Accept, Reject, Complete, Quotation, Get Bookings |
| **Payment** | 5 | Initiate, Success Callback, Failure Callback, Check Status (2 ways) |
| **Information** | 3 | Get Services, Get Booking Status, Get All Actions |
| **TOTAL** | **22** | **Complete booking management system** |

---

# 🔑 Key Features

## Automatic Amount Calculation
When `amount` is not provided in customer booking:
```
Total = Base Service Price + Selected Add-ons/Sub-services Price
```

## Three Booking Types
1. **Full Booking**: Complete details required (6 fields)
2. **Simple Booking**: Minimal data, system fills gaps (1 field + optional)
3. **Minimal Booking**: Phone only, everything auto-filled (1 field)

## String-Only Format
All data sent as strings (AiSensy requirement):
- ✅ `"750"` (correct)
- ❌ `750` (incorrect)

## Comprehensive Coverage
- **Customer Journey**: Create → Track → Cancel → Review
- **Partner Journey**: Accept → Complete → Create Quotations
- **Payment Flow**: Initiate → Process → Verify
- **Information**: Services, Status, Actions

All APIs are **production-ready** with comprehensive error handling! 🚀
