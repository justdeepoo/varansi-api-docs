# Quick Start Guide

This guide will help you integrate with the Varanasi Nagar Nigam WhatsApp Bot API in 5 minutes.

## The 5-Step Flow

```
1. Authenticate → 2. List Properties → 3. Check Dues → 4. Last Payment → 5. Download Invoice
```

---

## Step 1: Authenticate User

### Send OTP

```bash
curl -X POST "https://api.varansinagar.gov.in/api/auth/send-otp" \
  -H "Content-Type: application/json" \
  -d '{"mobile_no": "9876543210"}'
```

**Response:**
```json
{
  "status": true,
  "message": "OTP sent successfully",
  "data": {
    "request_id": "REQ-20260317-001234",
    "otp_expiry": "5 minutes"
  }
}
```

### Verify OTP

```bash
curl -X POST "https://api.varansinagar.gov.in/api/auth/verify-otp" \
  -H "Content-Type: application/json" \
  -d '{
    "mobile_no": "9876543210",
    "otp": "458921",
    "request_id": "REQ-20260317-001234"
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "OTP verified successfully",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 86400
  }
}
```

**Save the token for next 24 hours of API calls.**

---

## Step 2: List Properties

Show user all their properties.

```bash
curl -X GET "https://api.varansinagar.gov.in/api/properties/list?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

**Response:**
```json
{
  "status": true,
  "message": "Properties retrieved successfully",
  "data": {
    "properties": [
      {
        "property_id": "VN-PT-102345",
        "address": "123, Sigra, Varanasi",
        "outstanding_amount": 3500
      },
      {
        "property_id": "VN-PT-102346",
        "address": "456, Dashashwamedh, Varanasi",
        "outstanding_amount": 1200
      }
    ]
  }
}
```

**User selects one property to proceed.**

---

## Step 3: Check Dues

Get detailed dues breakdown for selected property.

```bash
curl -X POST "https://api.varansinagar.gov.in/api/dues/check" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210"
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "Dues retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "address": "123, Sigra, Varanasi",
    "property_type": "Residential",
    "last_payment_date": "05-02-2025",
    "last_payment_amount": 1500,
    "total_outstanding": 3500,
    "dues": {
      "property_tax": 2000,
      "water_tax": 1000,
      "sewerage_tax": 500
    }
  }
}
```

**From here, user can:**
- **Pay Now** → Go to Step 3.1
- **Check Last Payment** → Go to Step 4
- **Download Invoice** → Go to Step 5

---

## Step 3.1: Pay Now (Optional)

Initiate payment for selected taxes.

```bash
curl -X POST "https://api.varansinagar.gov.in/api/payment/initiate" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210",
    "tax_types": ["property_tax", "water_tax"],
    "amount": 3000
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "Payment initiated successfully",
  "data": {
    "transaction_id": "TXN-20260317-001234",
    "amount": 3000,
    "payment_link": "https://rzp.io/l/payment-link-abc123xyz",
    "status": "PENDING"
  }
}
```

**Share the `payment_link` with user. User completes payment on that link.**

---

## Step 4: Check Last Payment

Show user their last payment details.

```bash
curl -X POST "https://api.varansinagar.gov.in/api/payment/last-payment" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210"
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "Last payment retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "last_payment": {
      "transaction_id": "TXN-20250205-000856",
      "payment_date": "05-02-2025",
      "amount_paid": 1500,
      "status": "SUCCESS"
    }
  }
}
```

---

## Step 5: Download Invoice

Get list of invoices and download.

### Get Invoice Months

```bash
curl -X POST "https://api.varansinagar.gov.in/api/invoice/months" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210"
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "Invoice months retrieved successfully",
  "data": {
    "available_months": [
      {
        "month_year": "02-02-2026",
        "display_text": "February 2026",
        "amount": 1200
      },
      {
        "month_year": "01-01-2026",
        "display_text": "January 2026",
        "amount": 1200
      }
    ]
  }
}
```

### Download Invoice

```bash
curl -X POST "https://api.varansinagar.gov.in/api/invoice/download" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210",
    "month_year": "02-02-2026",
    "format": "pdf"
  }'
```

**Response:**
```json
{
  "status": true,
  "message": "Invoice retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "invoice_number": "INV-VN-102345-202602",
    "invoice_url": "https://storage.varansinagar.gov.in/invoices/INV-VN-102345-202602.pdf",
    "download_link": "https://storage.varansinagar.gov.in/invoices/INV-VN-102345-202602.pdf",
    "summary": {
      "property_tax": 0,
      "water_tax": 1200,
      "sewerage_tax": 0,
      "total_amount": 1200
    }
  }
}
```

**Share the `download_link` with user to download invoice.**

---

## Send Notifications to Users

You can send notifications when:
- User makes a payment → Send payment confirmation
- New invoice is generated → Send notification
- Payment is due → Send reminder

See [Notifications Documentation](docs/NOTIFICATIONS.md) for detailed format.

**Example: Send Payment Confirmation**

```bash
curl -X POST "https://api.varansinagar.gov.in/api/notification/send-individual" \
  -H "Authorization: Bearer admin-token" \
  -H "Content-Type: application/json" \
  -d '{
    "notification_type": "payment_confirmation",
    "recipient_type": "individual",
    "mobile_no": "9876543210",
    "content": {
      "owner_name": "Rahul Sharma",
      "property_id": "VN-PT-102345",
      "transaction_id": "TXN-20260317-001234",
      "amount_paid": 3500,
      "payment_date": "17-03-2026"
    }
  }'
```

---

## Common Error Codes

| Code | Meaning | Solution |
|------|---------|----------|
| INVALID_TOKEN | Token expired or invalid | Ask user to login again |
| INVALID_MOBILE | Wrong mobile format | Validate 10-digit number |
| PROPERTY_NOT_FOUND | Property doesn't exist | Show error message |
| INVALID_AMOUNT | Amount mismatch | Verify dues amount |
| RATE_LIMIT | Too many requests | Wait and retry |

---

## Integration Checklist

- [ ] Implement OTP send and verify
- [ ] Implement property listing
- [ ] Implement check dues
- [ ] Implement pay now (with payment gateway)
- [ ] Implement last payment check
- [ ] Implement invoice months & download
- [ ] Add error handling for all responses
- [ ] Add notification system
- [ ] Test with live server
- [ ] Deploy to production

---

## Support

For API issues: api-support@varansinagar.gov.in
For integration help: Contact Varanasi Nagar Nigam WhatsApp Team

### This Week
1. Review full API_COMPLETE_REFERENCE.md
2. Share documentation with team
3. Start integration planning

### This Month
1. Deploy documentation
2. Integrate API into applications
3. Set up monitoring

---

## 💡 Pro Tips

### For Testing
- Use Postman collection - all endpoints are pre-configured
- Check TROUBLESHOOTING.md for common issues
- Test error scenarios with invalid inputs

### For Integration
- Start with code examples in docs/examples/
- Use openapi.yaml to generate client SDK
- Reference API_COMPLETE_REFERENCE.md for details

### For Hosting
- GitHub Pages is easiest (free)
- ReadTheDocs is most professional
- Swagger UI provides interactive testing

### For Team
- Share README.md for overview
- Share Postman collection for testing
- Share INDEX.md for navigation

---

## 📞 Support

### Finding Help
| Issue | File |
|-------|------|
| How to use files | USING_API_FILES.md |
| API details | API_COMPLETE_REFERENCE.md |
| Endpoint specific | docs/endpoints/ |
| Errors/FAQ | docs/TROUBLESHOOTING.md |
| Code examples | docs/examples/ |
| Navigation | INDEX.md |

### External Support
- **Email**: api-support@varansinagar.gov.in
- **Hours**: Monday - Friday, 9:00 AM - 5:00 PM IST

---

## 📝 Summary

You have **everything needed** to:
- ✅ Understand the API
- ✅ Test the API
- ✅ Integrate the API
- ✅ Host the documentation
- ✅ Share with your team
- ✅ Build applications

The documentation is **production-ready** and can be used **immediately**.

---

## 🎊 Thank You!

Your API documentation is complete and ready to use.

**Questions?** Start with [README.md](README.md) or [INDEX.md](INDEX.md)

**Ready to test?** Import [Postman_Collection.json](Postman_Collection.json)

**Want details?** Read [API_COMPLETE_REFERENCE.md](API_COMPLETE_REFERENCE.md)

---

**Status**: ✅ Complete  
**Version**: 1.0  
**Last Updated**: 6 March 2026  
**Ready to Deploy**: Yes  

---

**Start here**: [README.md](README.md)
