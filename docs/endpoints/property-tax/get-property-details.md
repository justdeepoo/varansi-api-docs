# Property Tax - Get Property Details

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/property/details` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Retrieve detailed information about a specific property including owner name, address, and outstanding tax amount.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pid` | String | Yes | Property ID (e.g., VN-PT-102345) |

### URL Example
```
GET /property/details?pid=VN-PT-102345
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": {
    "pid": "VN-PT-102345",
    "owner_name": "Rahul Sharma",
    "address": "Sigra, Varanasi",
    "outstanding_amount": 4250,
    "property_type": "Residential",
    "area_sqft": 1500,
    "last_payment_date": "05-02-2025",
    "last_payment_amount": 2000
  }
}
```

### Error Response Examples

**Property Not Found (404)**
```json
{
  "status": false,
  "message": "Property with ID VN-PT-102345 not found",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

**Unauthorized (401)**
```json
{
  "status": false,
  "message": "Invalid or expired authentication token",
  "error_code": "AUTH_FAILED"
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | Boolean | Indicates success or failure |
| `data` | Object | Property details object |
| `data.pid` | String | Property ID |
| `data.owner_name` | String | Owner's name |
| `data.address` | String | Property address |
| `data.outstanding_amount` | Number | Outstanding tax amount (₹) |
| `data.property_type` | String | Type of property (Residential/Commercial) |
| `data.area_sqft` | Number | Property area in square feet |
| `data.last_payment_date` | String | Date of last payment (DD-MM-YYYY) |
| `data.last_payment_amount` | Number | Amount of last payment (₹) |

## Usage Notes

- Requires valid JWT token from authentication
- Property ID can be obtained from [Get Linked Properties](get-properties.md)
- Shows complete property information including payment history
- Use outstanding_amount to determine if payment is needed

## Example cURL Request

```bash
curl -X GET "https://api.varansinagar.gov.in/api/property/details?pid=VN-PT-102345" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Get Linked Properties](get-properties.md) - Get list of all properties
- [Initiate Payment](initiate-payment.md) - Pay outstanding property tax
