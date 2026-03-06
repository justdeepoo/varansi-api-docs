# Citizen Login - Send OTP

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | POST |
| **URL** | `/chatbot/login/send-otp` |
| **Authentication** | Not Required |
| **Rate Limit** | 5 requests per minute per mobile |

## Description
Send an OTP to the citizen's registered mobile number. This is the first step of the login process.

## Request

### Request Headers
```
Content-Type: application/json
```

### Request Body
```json
{
  "mobile_no": "9876543210",
  "otp_type": "ChatbotLogin"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mobile_no` | String | Yes | 10-digit mobile number |
| `otp_type` | String | Yes | Type of OTP request. Value: `ChatbotLogin` |

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "OTP sent successfully"
}
```

### Error Response Examples

**Invalid Mobile Number (400)**
```json
{
  "status": false,
  "message": "Invalid mobile number format",
  "error_code": "INVALID_MOBILE"
}
```

**Rate Limit Exceeded (429)**
```json
{
  "status": false,
  "message": "Too many OTP requests. Try again after 5 minutes",
  "error_code": "RATE_LIMIT_EXCEEDED"
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | Boolean | Indicates success or failure |
| `message` | String | Description of the result |

## Usage Notes

- OTP is valid for 10 minutes
- Maximum 5 OTP requests per mobile number per 5 minutes
- OTP is sent via SMS to the registered mobile number
- After successful OTP send, proceed to [Verify OTP endpoint](verify-otp.md)

## Example cURL Request

```bash
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/send-otp \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp_type": "ChatbotLogin"
  }'
```

## Related Endpoints
- [Verify OTP](verify-otp.md) - Second step of login
