# 📚 Nexo AiSensy WhatsApp API Documentation

## 🌐 Base Information

**Base URL**: `https://nexo.works/api/aisensy`

**Authentication**: Bearer Token (AiSensy Token)

**Content-Type**: `application/json`

---

## 🔐 Authentication

Include token in one of these ways:
- **Header**: `Authorization: Bearer YOUR_TOKEN`
- **Header**: `x-api-token: YOUR_TOKEN`  
- **Query**: `?token=YOUR_TOKEN`

---

## 📱 Phone Number Format

All phone numbers are normalized automatically:
- ✅ `917238861147` → `7238861147`
- ✅ `+917238861147` → `7238861147`
- ✅ `7238861147` → `7238861147`

---

## 📋 Table of Contents

1. [Team Member APIs](#team-member-apis)
2. [Partner APIs](#partner-apis)
3. [Customer APIs](#customer-apis)
4. [General APIs](#general-apis)

---

# Team Member APIs

## 1️⃣ Check Team Member Status

**Endpoint**: `POST /team-member/check`

**Description**: Check if technician is registered and their approval status

**Request**:
```json
{
  "mobileNumber": "917238861147"
}
```

**Response (Not Registered)**:
```json
{
  "success": true,
  "status": "not_registered",
  "message": "Welcome to Nexo Technician! Please register to start receiving jobs.",
  "action": "register",
  "link": "https://nexo.works/partner/onboard"
}
```

**Response (Pending Approval)**:
```json
{
  "success": true,
  "status": "pending_approval",
  "message": "Your registration is under review.",
  "action": "contact_support"
}
```

**Response (Approved)**:
```json
{
  "success": true,
  "status": "approved",
  "message": "Welcome back! Select an action.",
  "actions": ["view_jobs", "check_earnings", "my_profile"],
  "teamMember": {
    "id": "abc123",
    "name": "John Technician",
    "phone": "7238861147",
    "role": "technician"
  }
}
```

---

## 2️⃣ Get Team Member Jobs

**Endpoint**: `GET /team-member/jobs?mobileNumber=917238861147`

**Description**: Get all jobs assigned to the team member

**Query Parameters**:
- `mobileNumber` (required): Team member's mobile number

**Response**:
```json
{
  "success": true,
  "message": "Jobs retrieved successfully",
  "data": {
    "teamMember": {
      "id": "abc123",
      "name": "John Technician",
      "phone": "7238861147",
      "role": "technician"
    },
    "totalJobs": 5,
    "jobs": [
      {
        "jobId": "job123",
        "service": "AC Repair",
        "customer": {
          "name": "Rahul Kumar",
          "phone": "9876543210",
          "email": "rahul@example.com"
        },
        "location": {
          "address": "123 Main St, Andheri West",
          "landmark": "Near City Mall",
          "pincode": "400001"
        },
        "status": "accepted",
        "paymentStatus": "pending",
        "amount": 1500,
        "scheduledDate": "2026-01-20",
        "scheduledTime": "14:00",
        "otp": "123456",
        "specialInstructions": "Call before arriving"
      }
    ]
  }
}
```

---

## 3️⃣ Team Member Job Actions

**Endpoint**: `POST /team-member/job/action`

**Description**: Perform actions on assigned jobs

**Available Actions**:
- `send_quotation` - Send quotation to customer
- `start_job` - Start working on job
- `pause_job` - Pause work temporarily
- `resume_job` - Resume paused job
- `upload_photo` - Upload work photos
- `complete_job` - Mark job as completed
- `cancel_job` - Cancel the job

### Action: Send Quotation

**Request**:
```json
{
  "mobileNumber": "917238861147",
  "jobId": "job123",
  "action": "send_quotation",
  "data": {
    "amount": 2500,
    "description": "AC repair with gas refill",
    "items": [
      {
        "description": "Gas Refill",
        "quantity": 1,
        "unitPrice": 1500,
        "totalPrice": 1500
      }
    ]
  }
}
```

**Response**:
```json
{
  "success": true,
  "message": "Quotation sent successfully",
  "data": {
    "jobId": "job123",
    "jobStatus": "quotation_sent",
    "allowedNextActions": ["start_job"]
  }
}
```

---

### Action: Start Job

**Request**:
```json
{
  "mobileNumber": "917238861147",
  "jobId": "job123",
  "action": "start_job"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Job started successfully",
  "data": {
    "jobId": "job123",
    "jobStatus": "in_progress",
    "allowedNextActions": ["pause_job", "upload_photo", "complete_job", "cancel_job"]
  }
}
```

### Action: Complete Job

**Request**:
```json
{
  "mobileNumber": "917238861147",
  "jobId": "job123",
  "action": "complete_job"
}
```

### Action: Cancel Job

**Request**:
```json
{
  "mobileNumber": "917238861147",
  "jobId": "job123",
  "action": "cancel_job",
  "data": {
    "reason": "Customer not available"
  }
}
```

---

## 4️⃣ Get Team Member Earnings

**Endpoint**: `GET /team-member/earnings?mobileNumber=917238861147`

**Description**: Get earnings and payment information

**Response**:
```json
{
  "success": true,
  "data": {
    "earnings": {
      "totalEarnings": 15000,
      "totalCompletedJobs": 10,
      "pendingPayments": 2500,
      "pendingJobsCount": 2,
      "averageEarningPerJob": "1500.00"
    }
  }
}
```

---

## 5️⃣ Update Team Member Profile

**Endpoint**: `POST /team-member/profile/update`

**Request**:
```json
{
  "mobileNumber": "917238861147",
  "name": "John Updated",
  "email": "john@example.com",
  "address": "456 New Street",
  "city": "Mumbai",
  "pincode": "400002"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Profile updated successfully"
}
```

---

# Partner APIs

## 1️⃣ Create Partner Booking

**Endpoint**: `POST /partner/create-booking`

**Description**: Create booking and assign to partner

**Request**:
```json
{
  "partnerPhone": "919876543210",
  "customerPhone": "917238861147",
  "customerName": "Rahul Kumar",
  "customerEmail": "rahul@example.com",
  "serviceName": "AC Repair",
  "scheduledDate": "2026-01-20",
  "scheduledTime": "14:00",
  "locationAddress": "123 Main St, Andheri",
  "locationLandmark": "Near Mall",
  "locationPincode": "400001",
  "amount": "1500",
  "paymentMode": "cash"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Booking created and assigned to partner",
  "data": {
    "bookingId": "booking123",
    "status": "accepted",
    "otp": "123456"
  }
}
```

---

## 2️⃣ Partner Accept Booking

**Endpoint**: `PUT /partner/accept-booking`

**Request**:
```json
{
  "bookingId": "booking123",
  "partnerPhone": "919876543210"
}
```

## 3️⃣ Partner Reject Booking

**Endpoint**: `PUT /partner/reject-booking`

**Request**:
```json
{
  "bookingId": "booking123",
  "partnerPhone": "919876543210",
  "rejectionReason": "Not available at that time"
}
```

## 4️⃣ Partner Complete Booking

**Endpoint**: `PUT /partner/complete-booking`

**Request**:
```json
{
  "bookingId": "booking123",
  "partnerPhone": "919876543210",
  "otp": "123456",
  "remark": "Work completed successfully"
}
```

## 5️⃣ Partner Create Quotation

**Endpoint**: `POST /partner/create-quotation`

**Request**:
```json
{
  "bookingId": "booking123",
  "partnerPhone": "919876543210",
  "totalAmount": "2500",
  "items": [
    {
      "description": "AC Gas Refill",
      "quantity": "1",
      "unitPrice": "1500",
      "totalPrice": "1500"
    }
  ],
  "notes": "Additional work required"
}
```

---

## 6️⃣ Get Partner Dashboard Jobs

**Endpoint**: `GET /partner/dashboard/jobsall/:partnerPhone`

**Query Parameters**:
- `filter` (optional): all, pending, accepted, in_progress, completed
- `includeTeamJobs` (optional): true/false

**Example**: `GET /partner/dashboard/jobsall/919876543210?filter=accepted&includeTeamJobs=true`

**Response**:
```json
{
  "success": true,
  "data": {
    "partner": {
      "id": "partner123",
      "phone": "9876543210",
      "name": "Partner Name",
      "teamMembersCount": 3
    },
    "statistics": {
      "total": 25,
      "pending": 3,
      "accepted": 5,
      "completed": 10,
      "totalEarnings": 15000
    },
    "jobs": []
  }
}
```

## 7️⃣ Get Partner Bookings

**Endpoint**: `GET /partner/bookings/:partnerPhone`

**Example**: `GET /partner/bookings/919876543210`

---

# Customer APIs

## 1️⃣ Create Customer Booking

**Endpoint**: `POST /customer/create-booking`

**Request**:
```json
{
  "customerPhone": "917238861147",
  "customerName": "Rahul Kumar",
  "customerEmail": "rahul@example.com",
  "serviceName": "AC Repair",
  "scheduledDate": "2026-01-20",
  "scheduledTime": "14:00",
  "locationAddress": "123 Main St",
  "locationLandmark": "Near Mall",
  "locationPincode": "400001",
  "paymentMode": "online",
  "specialInstructions": "Call before arriving"
}
```

**Response**:
```json
{
  "success": true,
  "message": "Booking created successfully!",
  "data": {
    "bookingId": "booking123",
    "paymentLink": "https://nexo.works/payment/xyz",
    "amount": 1500
  }
}
```

---

## 2️⃣ Cancel Customer Booking

**Endpoint**: `PUT /customer/cancel-booking`

**Request**:
```json
{
  "bookingId": "booking123",
  "customerPhone": "917238861147",
  "cancellationReason": "Change of plans"
}
```

## 3️⃣ Submit Customer Review

**Endpoint**: `POST /customer/submit-review`

**Request**:
```json
{
  "bookingId": "booking123",
  "customerPhone": "917238861147",
  "rating": "5",
  "comment": "Excellent service!",
  "videoUrl": "https://example.com/video.mp4"
}
```

## 4️⃣ Customer Quotation Action

**Endpoint**: `PUT /customer/quotation-action`

**Request**:
```json
{
  "quotationId": "quot123",
  "customerPhone": "917238861147",
  "action": "accept"
}
```

*action can be: "accept" or "reject"*

## 5️⃣ Get Customer Bookings

**Endpoint**: `GET /customer/bookings/:phone`

**Example**: `GET /customer/bookings/917238861147`

---

# General APIs

## 1️⃣ Get Available Services

**Endpoint**: `GET /services`

**Response**:
```json
{
  "success": true,
  "data": [
    {
      "serviceId": "service123",
      "serviceName": "AC Repair & Maintenance",
      "description": "Complete AC service",
      "basePrice": 500,
      "addOns": [
        {
          "addOnId": "addon123",
          "addOnName": "Gas Refill",
          "basePrice": 1000
        }
      ]
    }
  ]
}
```

## 2️⃣ Get Booking Status

**Endpoint**: `GET /booking/:bookingId/status`

**Example**: `GET /booking/booking123/status`

**Response**:
```json
{
  "success": true,
  "data": {
    "bookingId": "booking123",
    "status": "accepted",
    "paymentStatus": "pending",
    "customerName": "Rahul Kumar",
    "serviceName": "AC Repair",
    "amount": 1500,
    "otp": "123456"
  }
}
```

---

## 3️⃣ Send OTP

**Endpoint**: `POST /send-otp`

**Request**:
```json
{
  "phone": "919876543210",
  "userType": "user",
  "firstName": "John"
}
```

*userType can be: "user", "partner", or "vendor"*

**Response**:
```json
{
  "success": true,
  "message": "OTP sent successfully",
  "data": {
    "phone": "919876543210",
    "otpSent": true,
    "expiresIn": "10 minutes"
  }
}
```

---

# 📊 Job Status Lifecycle

```
pending → quotation_sent → accepted → in_progress → paused → work_completed → completed
                                                    ↓
                                                cancelled
```

## Allowed Actions by Status

| Status | Allowed Actions |
|--------|----------------|
| pending | send_quotation |
| accepted | start_job, send_quotation, cancel_job |
| quotation_sent | start_job |
| in_progress | pause_job, upload_photo, complete_job, cancel_job |
| paused | resume_job, cancel_job |
| work_completed | upload_photo |
| completed | (none) |
| cancelled | (none) |

---

# 🔴 Error Responses

All endpoints return consistent error format:

```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error (development only)"
}
```

## Common Error Codes

| Code | Description |
|------|-------------|
| 400 | Bad Request - Missing or invalid parameters |
| 401 | Unauthorized - Invalid or missing token |
| 403 | Forbidden - Not authorized for this action |
| 404 | Not Found - Resource doesn't exist |
| 500 | Internal Server Error |

---

# 🧪 Testing Examples

## cURL Examples

### Check Team Member Status
```bash
curl -X POST https://nexo.works/api/aisensy/team-member/check \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mobileNumber": "917238861147"}'
```

### Get Team Member Jobs
```bash
curl -X GET "https://nexo.works/api/aisensy/team-member/jobs?mobileNumber=917238861147" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Start Job
```bash
curl -X POST https://nexo.works/api/aisensy/team-member/job/action \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "mobileNumber": "917238861147",
    "jobId": "job123",
    "action": "start_job"
  }'
```

---

# 📝 Notes & Best Practices

## Phone Number Handling
- Always send phone numbers with or without country code
- System automatically normalizes to 10-digit format
- Both `917238861147` and `7238861147` work identically

## Authentication
- Token is required for all endpoints (except public payment pages)
- Token can be passed via header or query parameter
- Keep token secure and don't expose in client-side code

## Rate Limiting
- API has rate limiting to prevent abuse
- Recommended: Max 100 requests per minute per token

## Webhooks
- Configure webhooks in AiSensy dashboard
- Receive real-time updates for booking status changes
- Webhook payload includes full booking details

## Support
- Technical Support: tech@nexo.works
- API Issues: api@nexo.works
- Documentation: https://nexo.works/api-docs

---

# 🚀 Quick Start Guide

## Step 1: Get API Token
Contact Nexo team to get your AiSensy API token

## Step 2: Test Authentication
```bash
curl -X GET "https://nexo.works/api/aisensy/services" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Step 3: Check Team Member Status
```bash
curl -X POST https://nexo.works/api/aisensy/team-member/check \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mobileNumber": "YOUR_PHONE"}'
```

## Step 4: Integrate with WhatsApp Bot
Use the API responses to build conversational flows in AiSensy

---

**Last Updated**: January 2026  
**Version**: 1.0.0  
**API Status**: Production Ready ✅
