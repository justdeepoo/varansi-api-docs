# Verify OTP

Verify the OTP and get authentication token for subsequent API calls.

## Endpoint

```
POST /auth/verify-otp
```

## Request

### Header
```
Content-Type: application/json
```

### Body
```json
{
  "mobile_no": "9876543210",
  "otp": "458921",
  "request_id": "REQ-20260317-001234"
}
```

### Parameters
- **mobile_no** (string, required): User's 10-digit mobile number
- **otp** (string, required): 6-digit OTP received
- **request_id** (string, required): Request ID from send-otp response

### Example Request
```bash
curl -X POST "https://api.varansinagar.gov.in/api/auth/verify-otp" \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp": "458921",
    "request_id": "REQ-20260317-001234"
  }'
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "OTP verified successfully",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 86400,
    "mobile_no": "9876543210",
    "user_id": "USR-102345"
  }
}
```

### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "OTP verification failed",
  "error_code": "INVALID_OTP",
  "details": {
    "error": "OTP is invalid or expired",
    "attempts_remaining": 2
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.token | string | JWT authentication token |
| data.token_type | string | Token type (Bearer) |
| data.expires_in | number | Token validity in seconds (24 hours) |
| data.mobile_no | string | Authenticated mobile number |
| data.user_id | string | User identifier |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | OTP verified successfully |
| INVALID_OTP | 400 | OTP is incorrect |
| OTP_EXPIRED | 400 | OTP has expired |
| INVALID_REQUEST_ID | 400 | Request ID is invalid or expired |
| TOO_MANY_ATTEMPTS | 429 | Too many failed attempts (max 3) |
| INVALID_TOKEN | 401 | Authorization failed |
| SERVER_ERROR | 500 | Internal server error |

## Using the Token

After successful verification, include the token in all API requests:

```bash
curl -X GET "https://api.varansinagar.gov.in/api/properties/list?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Token Details

- **Validity**: 24 hours from issuance
- **Type**: JWT (JSON Web Token)
- **Refresh**: Not applicable (user must re-authenticate)
- **Scope**: All user endpoints for the authenticated user

## Notes

- Maximum 3 OTP verification attempts
- After 3 failed attempts, user must request new OTP
- Token is single-use per user
- Logging out is not required, token automatically expires
