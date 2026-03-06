# Citizen Login - Verify OTP

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | POST |
| **URL** | `/chatbot/login/verify-otp` |
| **Authentication** | Not Required |
| **Rate Limit** | 10 attempts per OTP |

## Description
Verify the OTP sent to the citizen's mobile number and obtain a JWT authentication token. This is the second step of the login process.

## Request

### Request Headers
```
Content-Type: application/json
```

### Request Body
```json
{
  "mobile_no": "9876543210",
  "otp": "458921",
  "otp_type": "ChatbotLogin"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mobile_no` | String | Yes | 10-digit mobile number |
| `otp` | String | Yes | 6-digit OTP received via SMS |
| `otp_type` | String | Yes | Type of OTP. Value: `ChatbotLogin` |

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI5ODc2NTQzMjEwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTcxMzIwODAwfQ.NX3-rI0G79pz8IKv2K0Qs3pj8H6_lVS_FVbSsR-1l4E"
}
```

### Error Response Examples

**Invalid OTP (400)**
```json
{
  "status": false,
  "message": "Invalid or expired OTP",
  "error_code": "OTP_INVALID"
}
```

**OTP Expired (400)**
```json
{
  "status": false,
  "message": "OTP has expired. Please request a new one",
  "error_code": "OTP_EXPIRED"
}
```

**Too Many Attempts (429)**
```json
{
  "status": false,
  "message": "Maximum OTP verification attempts exceeded",
  "error_code": "RATE_LIMIT_EXCEEDED"
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | Boolean | Indicates success or failure |
| `message` | String | Description of the result |
| `token` | String | JWT token for authentication (only on success) |

## Token Details

- **Token Type**: Bearer Token
- **Validity**: 24 hours from generation
- **Storage**: Store securely in HTTPOnly cookie or secure storage
- **Usage**: Include in `Authorization` header for authenticated requests as `Bearer <token>`

## Usage Notes

- Maximum 10 verification attempts per OTP
- OTP must be verified within 10 minutes of generation
- Successful verification returns a JWT token valid for 24 hours
- Use the returned token for all subsequent authenticated API calls
- On token expiration, user must login again

## Example cURL Request

```bash
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/verify-otp \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp": "458921",
    "otp_type": "ChatbotLogin"
  }'
```

## Example Usage of Token

After receiving the token, use it in subsequent requests:

```bash
curl -X GET https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

## Related Endpoints
- [Send OTP](send-otp.md) - First step of login
- [Get Linked Properties](../property-tax/get-properties.md) - Example authenticated endpoint
