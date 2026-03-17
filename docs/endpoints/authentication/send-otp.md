# Send OTP

Send a one-time password to user's mobile number for authentication.

## Endpoint

```
POST /auth/send-otp
```

## Request

### Header
```
Content-Type: application/json
```

### Body
```json
{
  "mobile_no": "9876543210"
}
```

### Parameters
- **mobile_no** (string, required): User's 10-digit mobile number

### Example Request
```bash
curl -X POST "https://api.varansinagar.gov.in/api/auth/send-otp" \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210"}'
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "OTP sent successfully",
  "data": {
    "mobile_no": "9876543210",
    "otp_expiry": "5 minutes",
    "request_id": "REQ-20260317-001234"
  }
}
```

### Error Response (400/500)
```json
{
  "status": false,
  "message": "Failed to send OTP",
  "error_code": "INVALID_MOBILE",
  "details": {
    "error": "Mobile number format is invalid"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.mobile_no | string | Mobile number OTP was sent to |
| data.otp_expiry | string | OTP validity duration |
| data.request_id | string | Request reference ID |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | OTP sent successfully |
| INVALID_MOBILE | 400 | Mobile number format is invalid |
| MOBILE_NOT_REGISTERED | 400 | Mobile number not registered |
| TOO_MANY_ATTEMPTS | 429 | Too many OTP requests (max 5 per hour) |
| SERVER_ERROR | 500 | Internal server error |

## Notes

- OTP is valid for 5 minutes
- Maximum 5 OTP requests per hour from same number
- OTP is sent via SMS + WhatsApp for redundancy
- User must verify OTP within expiry time
