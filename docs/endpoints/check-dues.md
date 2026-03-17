# Check Dues

Get detailed dues information for a specific property including property tax, water tax, and sewerage tax.

## Endpoint

```
POST /dues/check
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
curl -X POST "https://api.varansinagar.gov.in/api/dues/check" \
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
    },
    "breakdown": {
      "property_tax": {
        "amount": 2000,
        "period": "Current Financial Year"
      },
      "water_tax": {
        "amount": 1000,
        "period": "Last 2 Months"
      },
      "sewerage_tax": {
        "amount": 500,
        "period": "Last 2 Months"
      }
    }
  }
}
```

### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "Failed to retrieve dues",
  "error_code": "PROPERTY_NOT_FOUND",
  "details": {
    "error": "Property not linked to this mobile number"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.property_id | string | Property identifier |
| data.address | string | Complete property address |
| data.property_type | string | Type of property (Residential/Commercial) |
| data.last_payment_date | string | Date of last payment (DD-MM-YYYY) |
| data.last_payment_amount | number | Amount paid in last payment |
| data.total_outstanding | number | Total outstanding amount in rupees |
| data.dues.property_tax | number | Property tax due |
| data.dues.water_tax | number | Water tax due |
| data.dues.sewerage_tax | number | Sewerage tax due |
| data.breakdown | object | Detailed breakdown of each tax type |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Dues retrieved successfully |
| PROPERTY_NOT_FOUND | 404 | Property not found |
| INVALID_PROPERTY | 400 | Property not linked to mobile number |
| INVALID_TOKEN | 401 | JWT token is invalid or expired |
| SERVER_ERROR | 500 | Internal server error |

## Pay Now Feature

To pay the dues, use the **Pay Now** endpoint and specify which taxes to pay:
- Pay only property tax
- Pay only water tax
- Pay only sewerage tax
- Pay all taxes together

Example payload for Pay Now:
```json
{
  "property_id": "VN-PT-102345",
  "tax_types": ["property_tax", "water_tax"],
  "amount": 3000
}
```

See [Pay Now](pay-now.md) endpoint documentation for details.
