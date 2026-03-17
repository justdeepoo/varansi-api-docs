# Varanasi Nagar Nigam WhatsApp Bot API Documentation

Welcome to the API documentation for Varanasi Nagar Nigam WhatsApp Bot. This simplified API enables citizens to check property taxes, water bills, and manage payments through WhatsApp.

## Overview

The WhatsApp Bot API provides a convenient way for residents to:
- Authenticate using mobile-based OTP
- List their properties
- Check dues (property tax, water tax, sewerage tax)
- View last payment details
- Download billing invoice copies
- Receive notifications about payment updates

## API Flow

The integration follows this simple 5-step flow:

1. **Authenticate** - Send OTP and verify to get authentication token
2. **List Your Properties** - Fetch all properties linked to mobile number
3. **Check Dues** - Select a property and view outstanding dues
4. **Check Last Payment** - View last payment details for a property
5. **Download Billing Invoice** - Download invoice copy for a specific month

## Quick Start

### Step 1: Authentication
Every API request requires authentication using a JWT token.

```bash
# Send OTP
curl -X POST https://api.varansinagar.gov.in/api/auth/send-otp \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210"}'

# Verify OTP and get token
curl -X POST https://api.varansinagar.gov.in/api/auth/verify-otp \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210", "otp": "458921"}'
```

### Step 2: Use the Token
Include the token in all subsequent requests:

```bash
curl -X GET "https://api.varansinagar.gov.in/api/properties/list" \
  -H "Authorization: Bearer <your_jwt_token>" \
  -H "Content-Type: application/json"
```

## API Endpoints Overview

### 1. Authentication Endpoints
- [Send OTP](docs/endpoints/authentication/send-otp.md) - Send OTP to mobile number
- [Verify OTP](docs/endpoints/authentication/verify-otp.md) - Verify OTP and get JWT token

### 2. Property Management
- [List Your Properties](docs/endpoints/list-properties.md) - Get all properties for user

### 3. Dues Management
- [Check Dues](docs/endpoints/check-dues.md) - Get dues breakdown for property

### 4. Payment Processing
- [Pay Now](docs/endpoints/pay-now.md) - Initiate payment for taxes
- [Check Last Payment](docs/endpoints/check-last-payment.md) - View last payment details

### 5. Invoice Management
- [Download Invoice](docs/endpoints/download-invoice.md) - Get and download invoices

### 6. Notifications
- [Send Notifications](docs/NOTIFICATIONS.md) - Send messages to users (broadcast/individual)

---

## API Endpoint URL Structure

### Base URL
```
Production: https://api.varansinagar.gov.in/api
Staging: https://staging-api.varansinagar.gov.in/api
```

### Endpoint URL Pattern
```
{BASE_URL}/{RESOURCE}/{ACTION}

Examples:
- POST   {BASE_URL}/auth/send-otp
- POST   {BASE_URL}/auth/verify-otp
- GET    {BASE_URL}/properties/list
- POST   {BASE_URL}/dues/check
- POST   {BASE_URL}/payment/initiate
- POST   {BASE_URL}/payment/last-payment
- POST   {BASE_URL}/invoice/months
- POST   {BASE_URL}/invoice/download
- POST   {BASE_URL}/notification/send-individual
- POST   {BASE_URL}/notification/send-broadcast
```

### Complete Endpoint Reference

| S.No | Resource | Action | Method | Endpoint | Purpose |
|------|----------|--------|--------|----------|----------|
| 1 | `auth` | `send-otp` | POST | `/auth/send-otp` | Send OTP for authentication |
| 2 | `auth` | `verify-otp` | POST | `/auth/verify-otp` | Verify OTP and get JWT token |
| 3 | `properties` | `list` | GET | `/properties/list` | List all properties for user |
| 4 | `dues` | `check` | POST | `/dues/check` | Get dues details for property |
| 5 | `payment` | `initiate` | POST | `/payment/initiate` | Initiate payment (Pay Now) |
| 6 | `payment` | `last-payment` | POST | `/payment/last-payment` | Get last payment information |
| 7 | `invoice` | `months` | POST | `/invoice/months` | Get available invoice months |
| 8 | `invoice` | `download` | POST | `/invoice/download` | Download invoice copy |
| 9 | `notification` | `send-individual` | POST | `/notification/send-individual` | Send message to individual user |
| 10 | `notification` | `send-broadcast` | POST | `/notification/send-broadcast` | Send broadcast message to users |

> **Note**: Final endpoint URLs and structure will be decided and confirmed by Varanasi Nagar Nigam Technical Team. Above URLs are recommended based on REST best practices and WhatsApp Bot integration requirements.

## API Specifications

- **Base URL**: https://api.varansinagar.gov.in/api
- **API Version**: 1.0
- **Total Endpoints**: 10
- **Authentication**: JWT Bearer Token
- **Token Validity**: 24 hours
- **Content-Type**: application/json
- **Rate Limit**: 60 requests/minute per user
- **OTP Validity**: 5 minutes
- **OTP Rate Limit**: 5 requests/hour per mobile

## Documentation Files

- **README.md** (this file) - Overview and quick reference
- **QUICK_START.md** - 5-minute integration guide
- **DOCUMENTATION_SUMMARY.md** - Complete changes and summary
- **docs/endpoints/INDEX.md** - All endpoints with table reference
- **docs/endpoints/** - Detailed documentation for each endpoint
- **docs/NOTIFICATIONS.md** - Message format for notifications
- **docs/TROUBLESHOOTING.md** - FAQ and troubleshooting
- **openapi.yaml** - OpenAPI 3.0 specification (for automation)

## Additional Resources

- [Endpoints Reference Guide](docs/endpoints/INDEX.md) - Complete endpoint documentation
- [Notifications Guide](docs/NOTIFICATIONS.md) - Message format and templates
- [Troubleshooting & FAQ](docs/TROUBLESHOOTING.md) - Common questions and issues
- [Quick Start Guide](QUICK_START.md) - 5-minute integration guide
- [Documentation Summary](DOCUMENTATION_SUMMARY.md) - Overview of all changes

## API Integration Example

For complete step-by-step integration examples, see [QUICK_START.md](QUICK_START.md)

Basic authentication pattern:
```bash
# Step 1: Send OTP
curl -X POST https://api.varansinagar.gov.in/api/auth/send-otp \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210"}'

# Get token from verify-otp response
# Step 2: Use token in all subsequent requests
curl -X GET https://api.varansinagar.gov.in/api/properties/list \
  -H "Authorization: Bearer <jwt_token>" \
  -H "Content-Type: application/json"
```

## WhatsApp Bot Integration Flow

The API is designed for this 5-step user flow:

```
User sends /start to WhatsApp Bot
    ↓
1. Bot asks for phone number
    ↓ (User provides number)
2. Bot sends OTP (Send OTP endpoint)
    ↓ (User enters OTP)
3. Bot verifies OTP, gets JWT token (Verify OTP endpoint)
    ↓
4. Bot shows menu:
   ├─ Check Dues → List Properties → Check Dues endpoint
   ├─ Pay Now → Select Tax → Pay Now endpoint
   ├─ Last Payment → Check Last Payment endpoint
   └─ Download Invoice → Get Months → Download Invoice endpoint
    ↓
5. Bot sends notifications (Notification endpoint)
```

## Error Handling

All endpoints return standard response format:

**Success Response:**
```json
{
  "status": true,
  "message": "Operation successful",
  "data": { ... }
}
```

**Error Response:**
```json
{
  "status": false,
  "message": "Error description",
  "error_code": "ERROR_CODE",
  "details": { ... }
}
```

Common error codes:
- `INVALID_TOKEN` (401) - JWT token invalid or expired
- `INVALID_MOBILE` (400) - Phone number format invalid
- `PROPERTY_NOT_FOUND` (404) - Property doesn't exist
- `RATE_LIMIT` (429) - Too many requests
- `SERVER_ERROR` (500) - Internal server error

See [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) for complete error reference.

## Technical Notes for Implementation

### URL Structure Recommendation
The recommended endpoint structure follows REST best practices:
- Resource-based URLs (e.g., `/properties`, `/dues`, `/payment`)
- Actions as sub-resources (e.g., `/auth/send-otp`)
- Consistent HTTP methods (GET for retrievals, POST for actions)

### Request Headers Required
```
Content-Type: application/json
Authorization: Bearer <jwt_token>  (All endpoints except auth)
```

### Payment Integration
When user initiates payment:
1. Call `/payment/initiate` endpoint with tax details
2. Endpoint returns payment link (Razorpay URL)
3. Redirect user to payment link
4. After payment, send confirmation via notification

### Notification Integration
Send notifications for:
- Payment confirmations
- Payment reminders (due date approaching)
- Invoice generation alerts
- System maintenance notices
- Promotional offers

See [NOTIFICATIONS.md](docs/NOTIFICATIONS.md) for message templates.

---

## Support & Contact

For API support and issues:
- **Email**: api-support@varansinagar.gov.in
- **Documentation Issues**: Check [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
- **Integration Help**: Contact Varanasi Nagar Nigam Technical Team
- **Hours**: Monday - Friday, 9:00 AM - 5:00 PM IST
