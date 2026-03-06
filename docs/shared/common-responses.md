# Common Response Formats

## Success Response Structure
All successful API responses follow this standard format:

```json
{
  "status": true,
  "message": "Success message or description",
  "data": {
    // Response data object (varies by endpoint)
  }
}
```

## Error Response Structure
All error responses follow this standard format:

```json
{
  "status": false,
  "message": "Error description",
  "error_code": "ERROR_CODE",
  "details": {
    // Additional error information (optional)
  }
}
```

## HTTP Status Codes

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Missing or invalid authentication token |
| 403 | Forbidden | User does not have permission |
| 404 | Not Found | Resource not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |
| 503 | Service Unavailable | Service temporarily unavailable |

## Common Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_MOBILE | Mobile number format is invalid |
| OTP_EXPIRED | OTP has expired |
| OTP_INVALID | OTP is incorrect |
| AUTH_FAILED | Authentication failed |
| RESOURCE_NOT_FOUND | Requested resource not found |
| VALIDATION_ERROR | Request validation failed |
| RATE_LIMIT_EXCEEDED | Too many requests |
| SERVER_ERROR | Internal server error |

## Common Fields

### Mobile Number
- **Type**: String
- **Format**: 10-digit Indian mobile number
- **Example**: "9876543210"
- **Validation**: Must start with 6-9

### OTP (One-Time Password)
- **Type**: String
- **Format**: 6-digit numeric code
- **Validity**: 10 minutes from generation
- **Example**: "458921"

### JWT Token
- **Type**: String
- **Format**: Bearer token
- **Validity**: 24 hours from generation
- **Usage**: Include in Authorization header: `Bearer <token>`
