# Pay Now - Initiate Payment

Initiate payment for selected tax types for a property.

## Endpoint

```
POST /payment/initiate
```

## Authentication
Required. Include JWT token in Authorization header.

## Request

### Header
```
Authorization: Bearer <jwt_token>
Content-Type: application/json
```

### Body
```json
{
  "property_id": "VN-PT-102345",
  "mobile_no": "9876543210",
  "tax_types": ["property_tax", "water_tax"],
  "amount": 3000
}
```

### Parameters
- **property_id** (string, required): The property ID to pay for
- **mobile_no** (string, required): User's mobile number
- **tax_types** (array, required): Array of tax types to pay
  - Allowed values: `property_tax`, `water_tax`, `sewerage_tax`
  - Can select one or multiple taxes or use `all` for all taxes
- **amount** (number, required): Total amount to pay in rupees

### Example Request
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

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Payment initiated successfully",
  "data": {
    "transaction_id": "TXN-20260317-001234",
    "property_id": "VN-PT-102345",
    "address": "123, Sigra, Varanasi",
    "tax_types_selected": ["property_tax", "water_tax"],
    "breakdown": {
      "property_tax": 2000,
      "water_tax": 1000
    },
    "total_amount": 3000,
    "currency": "INR",
    "payment_gateway": "Razorpay",
    "payment_link": "https://rzp.io/l/payment-link-abc123xyz",
    "expires_at": "17-03-2026 23:59:59",
    "status": "PENDING"
  }
}
```

### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "Failed to initiate payment",
  "error_code": "INVALID_AMOUNT",
  "details": {
    "error": "Amount does not match selected tax dues"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.transaction_id | string | Unique transaction identifier |
| data.property_id | string | Property for which payment is made |
| data.address | string | Property address |
| data.tax_types_selected | array | Selected tax types for payment |
| data.breakdown | object | Tax-wise amount breakdown |
| data.total_amount | number | Total payment amount in rupees |
| data.currency | string | Currency code (INR) |
| data.payment_gateway | string | Payment gateway name (Razorpay) |
| data.payment_link | string | Clickable payment link for user |
| data.expires_at | string | Payment link expiry time |
| data.status | string | Payment status (PENDING) |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Payment initiated successfully |
| INVALID_PROPERTY | 400 | Property not found or invalid |
| INVALID_AMOUNT | 400 | Amount doesn't match selected taxes |
| INVALID_TAX_TYPE | 400 | Invalid tax type specified |
| AMOUNT_TOO_LOW | 400 | Minimum payment amount not met |
| INVALID_TOKEN | 401 | JWT token is invalid or expired |
| PAYMENT_GATEWAY_ERROR | 503 | Payment gateway unavailable |
| SERVER_ERROR | 500 | Internal server error |

## Tax Type Code Reference

```
property_tax     - Property Tax
water_tax        - Water Tax
sewerage_tax     - Sewerage Tax
all              - All three taxes
```

## Next Steps

After payment is initiated:
1. User receives **payment_link** in response
2. User clicks the link to complete payment
3. User is redirected to payment gateway
4. After successful payment, user receives payment confirmation
5. Use [Check Payment Status](check-payment-status.md) endpoint to verify payment

## Payment Flow Diagram

```
Check Dues → Select Tax Types → Pay Now → Get Payment Link → 
Complete Payment → Payment Confirmation → Payment History Updated
```
