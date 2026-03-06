# Complete API Reference - Varanasi Nagar Nigam Chatbot API

**API Version**: 1.0  
**Base URL**: https://api.varansinagar.gov.in/api  
**Last Updated**: 6 March 2026

---

## Table of Contents
1. [Authentication APIs](#authentication-apis)
2. [Property Tax APIs](#property-tax-apis)
3. [Water Tax APIs](#water-tax-apis)
4. [Community Hall APIs](#community-hall-apis)
5. [Transaction APIs](#transaction-apis)

---

## Authentication APIs

### 1. Send OTP
**Endpoint**: `POST /chatbot/login/send-otp`  
**Authentication**: Not Required  
**Rate Limit**: 5 requests per 5 minutes  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/send-otp \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp_type": "ChatbotLogin"
  }'
```

**Request Body**:
```json
{
  "mobile_no": "9876543210",
  "otp_type": "ChatbotLogin"
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| mobile_no | String | Yes | 10-digit mobile number |
| otp_type | String | Yes | Value: `ChatbotLogin` |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "message": "OTP sent successfully"
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Invalid mobile number format",
  "error_code": "INVALID_MOBILE"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | OTP sent successfully |
| 400 | INVALID_MOBILE | Invalid mobile number format |
| 429 | RATE_LIMIT_EXCEEDED | Too many OTP requests |

---

### 2. Verify OTP
**Endpoint**: `POST /chatbot/login/verify-otp`  
**Authentication**: Not Required  
**Rate Limit**: 10 attempts per OTP  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/verify-otp \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp": "458921",
    "otp_type": "ChatbotLogin"
  }'
```

**Request Body**:
```json
{
  "mobile_no": "9876543210",
  "otp": "458921",
  "otp_type": "ChatbotLogin"
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| mobile_no | String | Yes | 10-digit mobile number |
| otp | String | Yes | 6-digit OTP |
| otp_type | String | Yes | Value: `ChatbotLogin` |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI5ODc2NTQzMjEwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTcxMzIwODAwfQ.NX3-rI0G79pz8IKv2K0Qs3pj8H6_lVS_FVbSsR-1l4E"
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Invalid or expired OTP",
  "error_code": "OTP_INVALID"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Login successful |
| 400 | OTP_INVALID | Incorrect OTP |
| 400 | OTP_EXPIRED | OTP has expired |
| 429 | RATE_LIMIT_EXCEEDED | Maximum attempts exceeded |

---

## Property Tax APIs

### 3. Get Linked Properties
**Endpoint**: `GET /property/mobile-properties`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| mobile_no | String | Yes | 10-digit mobile number |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": [
    {
      "pid": "VN-PT-102345",
      "address": "Sigra, Varanasi",
      "outstanding_amount": 4250
    },
    {
      "pid": "VN-PT-102346",
      "address": "Assi Ghat, Varanasi",
      "outstanding_amount": 0
    }
  ]
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "No properties linked to this mobile number",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Properties retrieved |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | No properties found |

---

### 4. Get Property Details
**Endpoint**: `GET /property/details`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/details?pid=VN-PT-102345" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pid | String | Yes | Property ID |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": {
    "pid": "VN-PT-102345",
    "owner_name": "Rahul Sharma",
    "address": "Sigra, Varanasi",
    "outstanding_amount": 4250,
    "property_type": "Residential",
    "area_sqft": 1500,
    "last_payment_date": "05-02-2025",
    "last_payment_amount": 2000
  }
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "Property with ID VN-PT-102345 not found",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Property details retrieved |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Property not found |

---

### 5. Initiate Property Payment
**Endpoint**: `POST /property/payment/initiate`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 10 requests per minute  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/property/payment/initiate \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "pid": "VN-PT-102345",
    "amount": 4250,
    "mobile_no": "9876543210"
  }'
```

**Request Body**:
```json
{
  "pid": "VN-PT-102345",
  "amount": 4250,
  "mobile_no": "9876543210"
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pid | String | Yes | Property ID |
| amount | Number | Yes | Amount to pay (₹) |
| mobile_no | String | Yes | 10-digit mobile number |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "payment_url": "https://paymentgateway.com/pay/abc123",
  "order_id": "ORD987654",
  "amount": 4250,
  "expiry_time": "2026-03-06T13:45:00+05:30"
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Amount must be greater than 0",
  "error_code": "VALIDATION_ERROR"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Payment initiated |
| 400 | VALIDATION_ERROR | Invalid amount |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Property not found |

---

## Water Tax APIs

### 6. Get Water Connections
**Endpoint**: `GET /water/mobile-connections`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/water/mobile-connections?mobile_no=9876543210" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| mobile_no | String | Yes | 10-digit mobile number |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": [
    {
      "connection_no": "WT-456789",
      "address": "Sigra, Varanasi",
      "consumer_type": "Residential"
    },
    {
      "connection_no": "WT-456790",
      "address": "Assi Ghat, Varanasi",
      "consumer_type": "Commercial"
    }
  ]
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "No water connections linked to this mobile number",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Connections retrieved |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | No connections found |

---

### 7. Check Water Dues
**Endpoint**: `GET /water/dues`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/water/dues?connection_no=WT-456789" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| connection_no | String | Yes | Water connection number |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": {
    "connection_no": "WT-456789",
    "consumer_name": "Amit Verma",
    "outstanding_amount": 980,
    "consumer_type": "Residential",
    "last_payment_date": "10-02-2026",
    "last_payment_amount": 500,
    "current_month_usage": 45
  }
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "Water connection WT-456789 not found",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Dues retrieved |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Connection not found |

---

### 8. Initiate Water Payment
**Endpoint**: `POST /water/payment/initiate`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 10 requests per minute  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/water/payment/initiate \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "connection_no": "WT-456789",
    "amount": 980,
    "mobile_no": "9876543210"
  }'
```

**Request Body**:
```json
{
  "connection_no": "WT-456789",
  "amount": 980,
  "mobile_no": "9876543210"
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| connection_no | String | Yes | Water connection number |
| amount | Number | Yes | Amount to pay (₹) |
| mobile_no | String | Yes | 10-digit mobile number |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "payment_url": "https://paymentgateway.com/pay/xyz123",
  "order_id": "ORD123456",
  "amount": 980,
  "expiry_time": "2026-03-06T13:45:00+05:30"
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Amount must be greater than 0",
  "error_code": "VALIDATION_ERROR"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Payment initiated |
| 400 | VALIDATION_ERROR | Invalid amount |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Connection not found |

---

## Community Hall APIs

### 9. List Community Halls
**Endpoint**: `GET /community-hall/list`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET https://api.varansinagar.gov.in/api/community-hall/list \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**: None

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": [
    {
      "hall_id": 1,
      "hall_name": "Sigra Community Hall",
      "capacity": 200,
      "location": "Sigra, Varanasi"
    },
    {
      "hall_id": 2,
      "hall_name": "Assi Community Hall",
      "capacity": 150,
      "location": "Assi Ghat, Varanasi"
    }
  ]
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "No community halls available",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Halls retrieved |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | No halls found |

---

### 10. Check Hall Availability
**Endpoint**: `POST /community-hall/availability`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/community-hall/availability \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "hall_id": 1,
    "booking_date": "2026-03-20"
  }'
```

**Request Body**:
```json
{
  "hall_id": 1,
  "booking_date": "2026-03-20"
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| hall_id | Number | Yes | Hall ID |
| booking_date | String | Yes | Date (YYYY-MM-DD) |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": [
    {
      "slot": "Morning",
      "available": true,
      "time": "06:00 AM - 12:00 PM"
    },
    {
      "slot": "Afternoon",
      "available": true,
      "time": "12:00 PM - 06:00 PM"
    },
    {
      "slot": "Evening",
      "available": false,
      "time": "06:00 PM - 12:00 AM"
    }
  ]
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Booking date must be in future",
  "error_code": "VALIDATION_ERROR"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Availability retrieved |
| 400 | VALIDATION_ERROR | Invalid date |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Hall not found |

---

### 11. Book Community Hall
**Endpoint**: `POST /community-hall/book`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 10 requests per minute  

#### Request
```bash
curl -X POST https://api.varansinagar.gov.in/api/community-hall/book \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "hall_id": 1,
    "booking_date": "2026-03-20",
    "slot": "Morning",
    "applicant_name": "Rahul Sharma",
    "mobile_no": "9876543210",
    "event_type": "Wedding",
    "expected_guests": 150
  }'
```

**Request Body**:
```json
{
  "hall_id": 1,
  "booking_date": "2026-03-20",
  "slot": "Morning",
  "applicant_name": "Rahul Sharma",
  "mobile_no": "9876543210",
  "event_type": "Wedding",
  "expected_guests": 150
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| hall_id | Number | Yes | Hall ID |
| booking_date | String | Yes | Date (YYYY-MM-DD) |
| slot | String | Yes | Time slot (Morning/Afternoon/Evening) |
| applicant_name | String | Yes | Applicant full name |
| mobile_no | String | Yes | 10-digit mobile number |
| event_type | String | No | Event type |
| expected_guests | Number | No | Expected guest count |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "booking_id": "HALL-98765",
  "message": "Booking request submitted successfully",
  "approval_status": "Pending",
  "expected_approval_time": "2-3 business days"
}
```

**Error (400)**:
```json
{
  "status": false,
  "message": "Selected slot is not available for this date",
  "error_code": "VALIDATION_ERROR"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Booking submitted |
| 400 | VALIDATION_ERROR | Invalid data |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | Hall not found |

---

## Transaction APIs

### 12. Get Transaction History
**Endpoint**: `GET /transactions/history`  
**Authentication**: Required (JWT Token)  
**Rate Limit**: 30 requests per minute  

#### Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/transactions/history?mobile_no=9876543210" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

**Query Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| mobile_no | String | Yes | 10-digit mobile number |
| from_date | String | No | Start date (YYYY-MM-DD) |
| to_date | String | No | End date (YYYY-MM-DD) |
| service_type | String | No | Filter by service |

#### Response

**Success (200 OK)**:
```json
{
  "status": true,
  "data": [
    {
      "transaction_id": "PTXN456987",
      "service": "Property Tax",
      "amount": 4250,
      "date": "05-02-2026",
      "time": "14:30:45",
      "status": "Success",
      "reference_id": "ORD987654",
      "property_id": "VN-PT-102345"
    },
    {
      "transaction_id": "WTXN123456",
      "service": "Water Tax",
      "amount": 980,
      "date": "02-02-2026",
      "time": "10:15:20",
      "status": "Success",
      "reference_id": "ORD123456",
      "connection_id": "WT-456789"
    }
  ]
}
```

**Error (404)**:
```json
{
  "status": false,
  "message": "No transactions found for this mobile number",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

| HTTP Status | Error Code | Description |
|-------------|-----------|-------------|
| 200 | N/A | Transactions retrieved |
| 400 | VALIDATION_ERROR | Invalid date format |
| 401 | AUTH_FAILED | Invalid token |
| 404 | RESOURCE_NOT_FOUND | No transactions found |

---

## Global HTTP Status Codes

| Status Code | Meaning |
|------------|---------|
| 200 | OK - Request successful |
| 201 | Created - Resource created |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid/missing token |
| 403 | Forbidden - No permission |
| 404 | Not Found - Resource not found |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

## Global Error Codes

| Error Code | HTTP Status | Description |
|------------|-------------|-------------|
| INVALID_MOBILE | 400 | Invalid mobile number format |
| OTP_INVALID | 400 | Incorrect OTP |
| OTP_EXPIRED | 400 | OTP has expired |
| AUTH_FAILED | 401 | Invalid/expired token |
| VALIDATION_ERROR | 400 | Request validation failed |
| RESOURCE_NOT_FOUND | 404 | Resource not found |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests |
| SERVER_ERROR | 500 | Internal server error |

## Request/Response Headers

### Required Headers (All Authenticated Endpoints)
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Response Headers
```
Content-Type: application/json
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1615038000
```

## Authentication Flow

```
1. Send OTP (POST /chatbot/login/send-otp)
   ↓
2. Verify OTP (POST /chatbot/login/verify-otp) → Receive JWT Token
   ↓
3. Use Token in All Subsequent Requests (Authorization: Bearer <token>)
```

## Rate Limiting

| Endpoint Type | Limit |
|--------------|-------|
| OTP endpoints | 5 requests per 5 minutes |
| GET endpoints | 30 requests per minute |
| Payment endpoints | 10 requests per minute |
| Standard | 60 requests per minute |

## Date & Time Formats

- **Dates**: YYYY-MM-DD (2026-03-06)
- **Display Dates**: DD-MM-YYYY (06-03-2026)
- **Time**: HH:MM:SS in IST (14:30:45)
- **ISO DateTime**: 2026-03-06T14:30:45+05:30

## Service Types (for filtering)

- PropertyTax
- WaterTax
- CommunityHall

## Transaction Status Values

- Success
- Failed
- Pending
- Cancelled

## Community Hall Slots

- Morning: 06:00 AM - 12:00 PM
- Afternoon: 12:00 PM - 06:00 PM
- Evening: 06:00 PM - 12:00 AM

---

## Quick Reference Table

| # | Endpoint | Method | Auth | Rate Limit |
|---|----------|--------|------|-----------|
| 1 | `/chatbot/login/send-otp` | POST | No | 5/5m |
| 2 | `/chatbot/login/verify-otp` | POST | No | 10/otp |
| 3 | `/property/mobile-properties` | GET | Yes | 30/m |
| 4 | `/property/details` | GET | Yes | 30/m |
| 5 | `/property/payment/initiate` | POST | Yes | 10/m |
| 6 | `/water/mobile-connections` | GET | Yes | 30/m |
| 7 | `/water/dues` | GET | Yes | 30/m |
| 8 | `/water/payment/initiate` | POST | Yes | 10/m |
| 9 | `/community-hall/list` | GET | Yes | 30/m |
| 10 | `/community-hall/availability` | POST | Yes | 30/m |
| 11 | `/community-hall/book` | POST | Yes | 10/m |
| 12 | `/transactions/history` | GET | Yes | 30/m |

---

**Generated**: 6 March 2026  
**Status**: Complete  
**For Support**: api-support@varansinagar.gov.in
