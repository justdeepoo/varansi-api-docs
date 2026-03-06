# Property Tax - Get Linked Properties

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/property/mobile-properties` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Retrieve all properties linked to the citizen's mobile number. Shows a summary of each property including address and outstanding amount.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mobile_no` | String | Yes | 10-digit mobile number |

### URL Example
```
GET /property/mobile-properties?mobile_no=9876543210
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": [
    {
      "pid": "VN-PT-102345",
      "address": "Sigra, Varanasi",
      "outstanding_amount": 4250
    },
    {
      "pid": "VN-PT-102346",
      "address": "Assi Ghat, Varanasi",
      "outstanding_amount": 0
    }
  ]
}
```

### Error Response Examples

**Unauthorized (401)**
```json
{
  "status": false,
  "message": "Invalid or expired authentication token",
  "error_code": "AUTH_FAILED"
}
```

**No Properties Found (404)**
```json
{
  "status": false,
  "message": "No properties linked to this mobile number",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | Boolean | Indicates success or failure |
| `data` | Array | Array of property objects |
| `data[].pid` | String | Property ID (unique identifier) |
| `data[].address` | String | Property address |
| `data[].outstanding_amount` | Number | Outstanding tax amount (₹) |

## Usage Notes

- Requires valid JWT token from authentication
- Returns all properties associated with the mobile number
- Properties with zero outstanding amount are included
- Click on `pid` to get detailed property information using [Get Property Details](get-property-details.md)

## Example cURL Request

```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/mobile-properties?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Get Property Details](get-property-details.md) - Get detailed information about a specific property
- [Initiate Payment](initiate-payment.md) - Pay property tax
