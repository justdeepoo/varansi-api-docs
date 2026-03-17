# Check Last Payment

Retrieve the last payment details for a specific property.

## Endpoint

```
POST /payment/last-payment
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
  "mobile_no": "9876543210"
}
```

### Example Request
```bash
curl -X POST "https://api.varansinagar.gov.in/api/payment/last-payment" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "property_id": "VN-PT-102345",
    "mobile_no": "9876543210"
  }'
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Last payment retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "address": "123, Sigra, Varanasi",
    "last_payment": {
      "transaction_id": "TXN-20250205-000856",
      "payment_date": "05-02-2025",
      "payment_time": "14:35:22",
      "amount_paid": 1500,
      "tax_paid": {
        "property_tax": 1000,
        "water_tax": 500
      },
      "payment_method": "Online (Razorpay)",
      "receipt_number": "RCP-20250205-000856",
      "status": "SUCCESS"
    },
    "previous_payments": [
      {
        "transaction_id": "TXN-20241215-000745",
        "payment_date": "15-12-2024",
        "amount_paid": 2000,
        "status": "SUCCESS"
      },
      {
        "transaction_id": "TXN-20241025-000634",
        "payment_date": "25-10-2024",
        "amount_paid": 1800,
        "status": "SUCCESS"
      }
    ]
  }
}
```

### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "Failed to retrieve payment information",
  "error_code": "NO_PAYMENT_HISTORY",
  "details": {
    "error": "No payment history found for this property"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.property_id | string | Property identifier |
| data.address | string | Property address |
| data.last_payment | object | Most recent payment details |
| last_payment.transaction_id | string | Transaction reference number |
| last_payment.payment_date | string | Date of payment (DD-MM-YYYY) |
| last_payment.payment_time | string | Time of payment (HH:MM:SS) |
| last_payment.amount_paid | number | Amount paid in rupees |
| last_payment.tax_paid | object | Breakdown of taxes paid |
| last_payment.payment_method | string | Method used for payment |
| last_payment.receipt_number | string | Receipt/Reference number |
| last_payment.status | string | Payment status (SUCCESS/FAILED) |
| data.previous_payments | array | List of past 2-3 payments |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Last payment retrieved successfully |
| PROPERTY_NOT_FOUND | 404 | Property not found |
| NO_PAYMENT_HISTORY | 404 | No payments made for this property |
| INVALID_TOKEN | 401 | JWT token is invalid or expired |
| SERVER_ERROR | 500 | Internal server error |

## Notes

- This endpoint shows the most recent payment and previous 2-3 payments
- For complete payment history, use the [Payment History](payment-history.md) endpoint
- Users can download payment receipts from the WhatsApp bot menu

## Related Endpoints

- [Pay Now](pay-now.md) - Initiate new payment
- [Check Dues](check-dues.md) - View current outstanding dues
- [Download Invoice](download-invoice.md) - Download billing invoice
