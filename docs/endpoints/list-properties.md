# List Your Properties

Fetch all properties linked to the user's mobile number.

## Endpoint

```
GET /properties/list
```

## Authentication
Required. Include JWT token in Authorization header.

## Request

### Header
```
Authorization: Bearer <jwt_token>
Content-Type: application/json
```

### Query Parameters
```
mobile_no: string (required)
  - The mobile number of the user
```

### Example Request
```bash
curl -X GET "https://api.varansinagar.gov.in/api/properties/list?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Response

### Success Response (200 OK)
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
      },
      {
        "property_id": "VN-PT-102347",
        "address": "789, Lanka, Varanasi",
        "outstanding_amount": 0
      }
    ],
    "total_count": 3
  }
}
```

### Error Response (400/401/500)
```json
{
  "status": false,
  "message": "Failed to retrieve properties",
  "error_code": "INVALID_TOKEN",
  "details": {
    "error": "Authentication token expired or invalid"
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| data.properties | array | List of properties |
| properties[].property_id | string | Unique property identifier |
| properties[].address | string | Full property address |
| properties[].outstanding_amount | number | Total outstanding dues in rupees |
| data.total_count | number | Total number of properties |

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Properties retrieved successfully |
| INVALID_TOKEN | 401 | JWT token is invalid or expired |
| INVALID_MOBILE | 400 | Mobile number format is invalid |
| NO_PROPERTIES | 200 | User has no properties registered |
| SERVER_ERROR | 500 | Internal server error |

## Next Steps

After retrieving properties, select a property ID to:
1. **Check Dues** - Call check-dues endpoint with property_id
2. **Check Last Payment** - Call check-last-payment endpoint with property_id
