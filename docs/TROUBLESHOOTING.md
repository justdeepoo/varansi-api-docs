# Troubleshooting & FAQ

## Frequently Asked Questions

### Authentication Issues

#### Q: How long is the JWT token valid?
**A:** JWT tokens are valid for 24 hours from the time of generation. After expiration, you need to login again using the OTP flow.

#### Q: Can I refresh the token without re-logging in?
**A:** No, currently tokens cannot be refreshed. You must complete the OTP verification process again to get a new token.

#### Q: What if I get "Auth_Failed" error?
**A:** This usually means:
- Your token has expired (regenerate using OTP)
- Your token is malformed or invalid
- The token is missing from the Authorization header
- The Bearer prefix is missing in the header

**Solution:**
```bash
# Check token format
Authorization: Bearer <your_valid_token>

# Not correct:
Authorization: <token_without_bearer>
Authorization: Bearer<token_no_space>
```

#### Q: How many OTP requests can I make?
**A:** Maximum 5 OTP requests per mobile number in 5 minutes. If you exceed this, you must wait 5 minutes before requesting another OTP.

---

### OTP Issues

#### Q: OTP has expired. What should I do?
**A:** OTP is valid for 10 minutes. If it expires, request a new one using the Send OTP endpoint.

#### Q: I didn't receive the OTP SMS. What should I do?
**A:** 
1. Wait a few minutes - SMS delivery can take time
2. Check your spam/junk folder
3. Ensure the mobile number is registered with Nagar Nigam
4. Try requesting OTP again after 5 minutes
5. Contact support if the issue persists

#### Q: Can I use the same OTP multiple times?
**A:** No, each OTP is single-use. Once verified, it becomes invalid.

---

### Payment Issues

#### Q: What payment methods are accepted?
**A:** The payment gateway accepts:
- Credit/Debit Cards (Visa, Mastercard, RuPay)
- Net Banking
- UPI
- Digital Wallets

#### Q: Payment URL expired. What should I do?
**A:** Payment URLs are valid for 30 minutes. If expired, initiate a new payment by calling the Initiate Payment endpoint again.

#### Q: My payment failed. Will I be charged?
**A:** No, you will only be charged on successful payment. If payment fails, your bank will reverse the transaction within 1-3 business days.

#### Q: How do I verify if my payment was successful?
**A:** Use the Transaction History endpoint with your mobile number. Successful transactions will show "Success" status.

```bash
GET /transactions/history?mobile_no=9876543210&from_date=2026-03-05
```

#### Q: Payment shows pending. When will it be confirmed?
**A:** Pending payments are usually confirmed within 5-10 minutes. If status doesn't change after 30 minutes, contact support with the order ID.

---

### Property & Water Issues

#### Q: I don't see my property in the list. Why?
**A:** Possible reasons:
- Property is not registered under your mobile number
- Mobile number doesn't match Nagar Nigam records
- Property was recently added (may take 24-48 hours to sync)

**Solution:** 
1. Verify your mobile number is correct
2. Contact Nagar Nigam office to update your property records
3. Try again after 48 hours

#### Q: Outstanding amount doesn't match my records. Why?
**A:** The outstanding amount is calculated as:
- Total tax due - All payments made so far
- If recent payment is made, it may take 24-48 hours to reflect

**Solution:**
1. Check transaction history for recent payments
2. Wait 24-48 hours for amount to update
3. Contact support with proof of payment

#### Q: Can I pay partial amount?
**A:** Yes, you can pay any amount up to the outstanding amount. No minimum payment required (except ₹100 minimum for transaction processing).

---

### Community Hall Issues

#### Q: When can I book a community hall?
**A:** 
- Minimum advance booking: 2 days from today
- Maximum advance booking: 90 days from today

#### Q: What if no slots are available?
**A:** 
1. Check other dates
2. Try other available community halls from the list
3. Contact Nagar Nigam office for special requests

#### Q: How long does booking approval take?
**A:** Typically 2-3 business days. You'll receive SMS/Email notification once approved or rejected.

#### Q: Can I cancel or modify my booking?
**A:** This functionality is not available through the API currently. Please contact Nagar Nigam office with your booking ID (HALL-XXXXX) for cancellations or modifications.

---

## Error Codes Reference

### Authentication Errors

| Error Code | HTTP Status | Description | Solution |
|------------|-------------|-------------|----------|
| `AUTH_FAILED` | 401 | Invalid or expired token | Re-login with OTP |
| `INVALID_MOBILE` | 400 | Invalid mobile number format | Use 10-digit valid number |
| `OTP_INVALID` | 400 | Incorrect OTP | Verify OTP and try again |
| `OTP_EXPIRED` | 400 | OTP validity expired | Request new OTP |

### Validation Errors

| Error Code | HTTP Status | Description | Solution |
|------------|-------------|-------------|----------|
| `VALIDATION_ERROR` | 400 | Invalid request parameters | Check request format |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests | Wait and retry |
| `INVALID_DATE` | 400 | Date format or value invalid | Use YYYY-MM-DD format |

### Resource Errors

| Error Code | HTTP Status | Description | Solution |
|------------|-------------|-------------|----------|
| `RESOURCE_NOT_FOUND` | 404 | Resource doesn't exist | Verify ID and retry |

### Server Errors

| Error Code | HTTP Status | Description | Solution |
|------------|-------------|-------------|----------|
| `SERVER_ERROR` | 500 | Server error | Retry after some time |

---

## Common Integration Issues

### Issue 1: CORS Error in Browser

**Error:** `No 'Access-Control-Allow-Origin' header`

**Cause:** API doesn't support cross-origin requests from browsers

**Solution:** 
- Call API from backend server, not directly from frontend
- Use a backend proxy/middleware
- Contact support to enable CORS if needed

### Issue 2: Token in Headers Not Being Recognized

**Incorrect:**
```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties" \
  -H "Authorization: Bearereyj..." \
  # Missing space after Bearer
```

**Correct:**
```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties" \
  -H "Authorization: Bearer eyj..." \
  # Proper space and format
```

### Issue 3: Timeout Errors

**Cause:** Request taking too long to complete

**Solutions:**
1. Check your internet connection
2. Increase timeout duration in your client (default: 30 seconds)
3. Retry the request
4. Contact support if persistent

### Issue 4: 500 Internal Server Error

**Cause:** Server-side issue

**Solutions:**
1. Wait a few minutes and retry
2. Verify your request format is correct
3. Check API status at [status.varansinagar.gov.in](https://status.varansinagar.gov.in)
4. Contact support with request details

---

## Best Practices

### Security
✓ Always use HTTPS for API calls
✓ Store tokens in secure storage (HTTPOnly cookies)
✓ Never log or display tokens in console
✓ Regenerate tokens periodically
✓ Use different mobile numbers for testing

### Performance
✓ Cache property/hall information locally
✓ Implement retry logic with exponential backoff
✓ Use connection pooling for multiple requests
✓ Batch requests when possible

### Error Handling
✓ Always check `status` field in response
✓ Implement proper exception handling
✓ Log errors with request details for debugging
✓ Show user-friendly error messages

### Rate Limiting
✓ Implement request queuing
✓ Add delays between requests if needed
✓ Cache responses to reduce API calls
✓ Monitor rate limit headers

---

## Support

If you can't find the answer to your issue:

1. **Check this documentation** - Most issues are covered here
2. **Check API Status** - Visit [status.varansinagar.gov.in](https://status.varansinagar.gov.in)
3. **Contact Support**:
   - **Email**: api-support@varansinagar.gov.in
   - **Phone**: +91-XXXX-XXXX-XXX
   - **Hours**: Monday - Friday, 9:00 AM - 5:00 PM IST
   - **Response Time**: Usually within 24 hours

When contacting support, please include:
- Error message and code
- HTTP status code
- Endpoint URL
- Request/Response body (sanitized)
- Screenshots if applicable
- Mobile number (last 4 digits only for privacy)
