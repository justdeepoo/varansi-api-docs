# Varanasi Nagar Nigam Chatbot API Documentation

Welcome to the comprehensive API documentation for the Varanasi Nagar Nigam Chatbot API. This API enables citizens to access municipal services including property tax management, water tax payments, and community hall bookings through a chatbot interface.

## Overview

The Varanasi Nagar Nigam Chatbot API provides a convenient way for residents to:
- Authenticate using mobile-based OTP
- View and manage property tax records
- Check and pay water bills
- Book community halls for events
- Track payment history

## Quick Start

### 1. Authentication
Every API request (except login endpoints) requires authentication using a JWT token.

```bash
# Step 1: Send OTP
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/send-otp \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210", "otp_type": "ChatbotLogin"}'

# Step 2: Verify OTP
curl -X POST https://api.varansinagar.gov.in/api/chatbot/login/verify-otp \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210", "otp": "458921", "otp_type": "ChatbotLogin"}'

# Response will include your JWT token
```

### 2. Use the Token
Include the token in all subsequent requests:

```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210" \
  -H "Authorization: Bearer <your_jwt_token>" \
  -H "Content-Type: application/json"
```

## API Structure

The API is organized into the following services:

### Authentication
- [Send OTP](endpoints/authentication/send-otp.md) - Initiate login
- [Verify OTP](endpoints/authentication/verify-otp.md) - Complete login and get token

### Property Tax Management
- [Get Linked Properties](endpoints/property-tax/get-properties.md) - List all properties
- [Get Property Details](endpoints/property-tax/get-property-details.md) - View property information
- [Initiate Payment](endpoints/property-tax/initiate-payment.md) - Pay property tax

### Water Tax Management
- [Get Connections](endpoints/water-tax/get-connections.md) - List all water connections
- [Check Dues](endpoints/water-tax/check-dues.md) - View outstanding water dues
- [Initiate Payment](endpoints/water-tax/initiate-payment.md) - Pay water tax

### Community Hall Booking
- [List Halls](endpoints/community-hall/list-halls.md) - View available halls
- [Check Availability](endpoints/community-hall/check-availability.md) - Check hall availability
- [Book Hall](endpoints/community-hall/book-hall.md) - Submit booking request

### Transactions
- [Transaction History](endpoints/transaction-history.md) - View payment history

## Base Information

- **Base URL**: https://api.varansinagar.gov.in/api
- **API Version**: v1.0
- **Authentication**: JWT Bearer Token
- **Content-Type**: application/json
- **Rate Limit**: 60 requests per minute per user

## Shared Resources

- [Base Configuration](shared/base-config.md) - API base URLs and settings
- [Common Responses](shared/common-responses.md) - Standard response formats and error codes
- [Authentication Guide](shared/authentication.md) - Detailed authentication information

## Code Examples

### JavaScript/Node.js
```javascript
// Using fetch API
const response = await fetch(
  'https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210',
  {
    method: 'GET',
    headers: {
      'Authorization': `Bearer ${jwtToken}`,
      'Content-Type': 'application/json'
    }
  }
);
const data = await response.json();
```

### Python
```python
import requests

headers = {
    'Authorization': f'Bearer {jwt_token}',
    'Content-Type': 'application/json'
}

response = requests.get(
    'https://api.varansinagar.gov.in/api/property/mobile-properties',
    params={'mobile_no': '9876543210'},
    headers=headers
)
data = response.json()
```

### cURL
```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210" \
  -H "Authorization: Bearer <jwt_token>" \
  -H "Content-Type: application/json"
```

## Common Workflows

### Check and Pay Property Tax
1. [Send OTP](endpoints/authentication/send-otp.md)
2. [Verify OTP](endpoints/authentication/verify-otp.md) → Get token
3. [Get Linked Properties](endpoints/property-tax/get-properties.md)
4. [Get Property Details](endpoints/property-tax/get-property-details.md)
5. [Initiate Payment](endpoints/property-tax/initiate-payment.md)
6. [Check Transaction History](endpoints/transaction-history.md)

### Check and Pay Water Tax
1. [Send OTP](endpoints/authentication/send-otp.md)
2. [Verify OTP](endpoints/authentication/verify-otp.md) → Get token
3. [Get Connections](endpoints/water-tax/get-connections.md)
4. [Check Dues](endpoints/water-tax/check-dues.md)
5. [Initiate Payment](endpoints/water-tax/initiate-payment.md)
6. [Check Transaction History](endpoints/transaction-history.md)

### Book Community Hall
1. [Send OTP](endpoints/authentication/send-otp.md)
2. [Verify OTP](endpoints/authentication/verify-otp.md) → Get token
3. [List Halls](endpoints/community-hall/list-halls.md)
4. [Check Availability](endpoints/community-hall/check-availability.md)
5. [Book Hall](endpoints/community-hall/book-hall.md)
6. [Check Transaction History](endpoints/transaction-history.md)

## Error Handling

All errors follow a standard format. See [Common Responses](shared/common-responses.md) for detailed error codes and HTTP status codes.

Example error response:
```json
{
  "status": false,
  "message": "Invalid or expired authentication token",
  "error_code": "AUTH_FAILED"
}
```

## Security Considerations

1. Always use HTTPS for API requests
2. Store JWT tokens securely (HTTPOnly cookies recommended)
3. Never share OTP with anyone
4. Keep mobile numbers confidential
5. Implement token refresh mechanism

## Support

For API support and issues, contact:
- **Email**: api-support@varansinagar.gov.in
- **Phone**: +91-XXXX-XXXX-XXX
- **Hours**: Monday - Friday, 9:00 AM - 5:00 PM IST

## API Changelog

### Version 1.0 (Current)
- Initial release
- Property tax management
- Water tax management
- Community hall booking
- Transaction history

## License

This API documentation is provided by Varanasi Nagar Nigam. Unauthorized use or reproduction is prohibited.
