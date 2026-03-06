# Water Tax - Get Connections

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/water/mobile-connections` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Retrieve all water connections linked to the citizen's mobile number. Shows connection number and address for each connection.

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
GET /water/mobile-connections?mobile_no=9876543210
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": [
    {
      "connection_no": "WT-456789",
      "address": "Sigra, Varanasi",
      "consumer_type": "Residential"
    },
    {
      "connection_no": "WT-456790",
      "address": "Assi Ghat, Varanasi",
      "consumer_type": "Commercial"
    }
  ]
}
```

### Error Response Examples

**No Connections Found (404)**
```json
{
  "status": false,
  "message": "No water connections linked to this mobile number",
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
| `data` | Array | Array of water connection objects |
| `data[].connection_no` | String | Water connection number (unique identifier) |
| `data[].address` | String | Connection address |
| `data[].consumer_type` | String | Type of consumer (Residential/Commercial) |

## Usage Notes

- Requires valid JWT token from authentication
- Returns all water connections associated with the mobile number
- Use `connection_no` to check dues or initiate payment
- Click on connection_no to get detailed information

## Example cURL Request

```bash
curl -X GET "https://api.varansinagar.gov.in/api/water/mobile-connections?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Check Dues](check-dues.md) - Get outstanding water dues
- [Initiate Payment](initiate-payment.md) - Pay water tax
