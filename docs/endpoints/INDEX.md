# API Endpoints Documentation

Complete list of all API endpoints for Varanasi Nagar Nigam WhatsApp Bot.

## Authentication Endpoints

| Endpoint | Method | Purpose | Link |
|----------|--------|---------|------|
| /auth/send-otp | POST | Send OTP to mobile number | [send-otp.md](authentication/send-otp.md) |
| /auth/verify-otp | POST | Verify OTP and get auth token | [verify-otp.md](authentication/verify-otp.md) |

## Property Management Endpoints

| Endpoint | Method | Purpose | Link |
|----------|--------|---------|------|
| /properties/list | GET | Get all properties for user | [list-properties.md](list-properties.md) |

## Dues & Payment Endpoints

| Endpoint | Method | Purpose | Link |
|----------|--------|---------|------|
| /dues/check | POST | Get dues breakdown for property | [check-dues.md](check-dues.md) |
| /payment/initiate | POST | Initiate payment for taxes | [pay-now.md](pay-now.md) |
| /payment/last-payment | POST | Get last payment details | [check-last-payment.md](check-last-payment.md) |

## Invoice Endpoints

| Endpoint | Method | Purpose | Link |
|----------|--------|---------|------|
| /invoice/months | POST | Get list of invoice months | [download-invoice.md](download-invoice.md) |
| /invoice/download | POST | Download invoice copy | [download-invoice.md](download-invoice.md) |

## Notification Endpoints

| Endpoint | Method | Purpose | Link |
|----------|--------|---------|------|
| /notification/send-individual | POST | Send message to user | [../NOTIFICATIONS.md](../NOTIFICATIONS.md) |
| /notification/send-broadcast | POST | Send broadcast message | [../NOTIFICATIONS.md](../NOTIFICATIONS.md) |

---

## Flow Diagram

```
1. AUTHENTICATE
   ├─ Send OTP → send-otp.md
   └─ Verify OTP → verify-otp.md

2. LIST PROPERTIES
   └─ Get Properties → list-properties.md

3. CHECK DUES
   └─ Check Property Dues → check-dues.md

4. PAYMENT OPTIONS
   ├─ Pay Now → pay-now.md
   ├─ Last Payment → check-last-payment.md
   └─ View Invoice → download-invoice.md

5. NOTIFICATIONS (OPTIONAL)
   └─ Send Messages → ../NOTIFICATIONS.md
```

---

## Quick Reference

### Authentication
- Always include JWT token in `Authorization: Bearer <token>` header
- Token validity: 24 hours
- Rate limit: 5 OTP requests per hour

### Response Format
All responses follow standard format:
```json
{
  "status": true/false,
  "message": "...",
  "data": { ... },
  "error_code": "..." (if error)
}
```

### Error Codes
- `INVALID_TOKEN` - JWT token invalid/expired (401)
- `INVALID_MOBILE` - Mobile number invalid (400)
- `PROPERTY_NOT_FOUND` - Property doesn't exist (404)
- `RATE_LIMIT` - Too many requests (429)
- `SERVER_ERROR` - Internal server error (500)

---

## Base URL

```
Production: https://api.varansinagar.gov.in/api
Staging: https://staging-api.varansinagar.gov.in/api
```

---

## Support

- **Documentation Issues**: See [TROUBLESHOOTING.md](../TROUBLESHOOTING.md)
- **API Support**: api-support@varansinagar.gov.in
- **Integration Help**: Contact WhatsApp Bot Team
