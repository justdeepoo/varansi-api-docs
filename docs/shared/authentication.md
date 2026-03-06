# Authentication Guide

## Overview
The Varanasi Nagar Nigam Chatbot API uses a two-step OTP (One-Time Password) based authentication mechanism for citizen login.

## Authentication Flow

```
1. Send OTP Request
   ↓
2. Verify OTP & Receive JWT Token
   ↓
3. Use JWT Token for Authenticated Requests
```

## Headers Required

### For Login Endpoints
```
Content-Type: application/json
```

### For Authenticated Endpoints
```
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>
```

## Token Management

- **Token Type**: JWT (JSON Web Token)
- **Token Validity**: 24 hours
- **Refresh**: Obtain new token by re-logging in
- **Storage**: Store securely on client side

## Security Best Practices

1. **Never share OTP**: OTP is personal and should not be shared
2. **Secure Token Storage**: Store JWT tokens in secure storage (HTTPOnly cookies recommended for web apps)
3. **Use HTTPS**: Always use HTTPS for API calls
4. **Token Rotation**: Generate new tokens periodically
5. **Mobile Number Privacy**: Treat mobile numbers as sensitive information
