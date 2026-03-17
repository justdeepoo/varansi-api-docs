# Download Billing Invoice

Download or retrieve billing invoice copy for a specific month.

## Step 1: Get Invoice Months

Get list of available invoice months for a property.

### Endpoint
```
POST /invoice/months
```

### Request
```json
{
  "property_id": "VN-PT-102345",
  "mobile_no": "9876543210"
}
```

### Response
```json
{
  "status": true,
  "message": "Invoice months retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "available_months": [
      {
        "month_year": "02-02-2026",
        "display_text": "February 2026",
        "amount": 1200,
        "status": "Available"
      },
      {
        "month_year": "01-01-2026",
        "display_text": "January 2026",
        "amount": 1200,
        "status": "Available"
      },
      {
        "month_year": "12-12-2025",
        "display_text": "December 2025",
        "amount": 1200,
        "status": "Available"
      },
      {
        "month_year": "10-10-2025",
        "display_text": "October 2025",
        "amount": 1200,
        "status": "Available"
      },
      {
        "month_year": "08-08-2025",
        "display_text": "August 2025",
        "amount": 1200,
        "status": "Available"
      }
    ],
    "total_available": 5
  }
}
```

---

## Step 2: Download Invoice

Download the invoice for selected month.

### Endpoint
```
POST /invoice/download
```

### Request
```json
{
  "property_id": "VN-PT-102345",
  "mobile_no": "9876543210",
  "month_year": "02-02-2026",
  "format": "pdf"
}
```

### Parameters
- **property_id** (string, required): Property ID
- **mobile_no** (string, required): User's mobile number
- **month_year** (string, required): Month in format MM-DD-YYYY
- **format** (string, optional): Invoice format - `pdf` (default) or `image`

### Example Request
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

### Response

#### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Invoice retrieved successfully",
  "data": {
    "property_id": "VN-PT-102345",
    "invoice_month": "February 2026",
    "invoice_number": "INV-VN-102345-202602",
    "invoice_date": "15-02-2026",
    "address": "123, Sigra, Varanasi",
    "owner_name": "Rahul Sharma",
    "bill_period": "01-02-2026 to 28-02-2026",
    "due_date": "10-03-2026",
    "invoice_url": "https://storage.varansinagar.gov.in/invoices/INV-VN-102345-202602.pdf",
    "download_link": "https://storage.varansinagar.gov.in/invoices/INV-VN-102345-202602.pdf",
    "summary": {
      "property_tax": 0,
      "water_tax": 1200,
      "sewerage_tax": 0,
      "total_amount": 1200
    },
    "payment_status": "UNPAID",
    "can_pay_online": true,
    "format": "pdf"
  }
}
```

#### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "Failed to retrieve invoice",
  "error_code": "INVOICE_NOT_FOUND",
  "details": {
    "error": "Invoice not available for selected month"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.property_id | string | Property identifier |
| data.invoice_month | string | Month and year of invoice |
| data.invoice_number | string | Unique invoice number |
| data.invoice_date | string | Invoice issue date |
| data.address | string | Property address |
| data.owner_name | string | Property owner name |
| data.bill_period | string | Billing period date range |
| data.due_date | string | Payment due date |
| data.invoice_url | string | Direct invoice URL/link |
| data.download_link | string | Download link for invoice |
| data.summary | object | Tax-wise breakdown |
| data.payment_status | string | PAID or UNPAID |
| data.can_pay_online | boolean | Whether online payment is available |
| data.format | string | Invoice format (pdf/image) |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Invoice retrieved successfully |
| PROPERTY_NOT_FOUND | 404 | Property not found |
| INVOICE_NOT_FOUND | 404 | Invoice not available for month |
| INVALID_MONTH | 400 | Invalid month format |
| INVALID_TOKEN | 401 | JWT token is invalid or expired |
| SERVER_ERROR | 500 | Internal server error |

## Invoice Content

Invoice contains:
- Property details and owner information
- Billing period
- Tax charges breakdown (Property Tax, Water Tax, Sewerage Tax)
- Total amount due
- Payment due date
- Payment instructions
- Previous payment history

## Usage Notes

1. **Get Available Months First** - Call get months endpoint to show user available invoices
2. **User Selects Month** - User selects from the list
3. **Download Invoice** - System returns URL or direct PDF/image
4. **Share with User** - Send invoice via WhatsApp or email

## Integration with Payment

If invoice status is **UNPAID**:
- User can click "Pay Now" to settle the amount
- Use [Pay Now](pay-now.md) endpoint with selected month's amount
