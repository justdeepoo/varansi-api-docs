# Water Tax - Check Dues

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/water/dues` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Check the outstanding water dues for a specific water connection. Displays consumer name and outstanding amount.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `connection_no` | String | Yes | Water connection number (e.g., WT-456789) |

### URL Example
```
GET /water/dues?connection_no=WT-456789
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": {
    "connection_no": "WT-456789",
    "consumer_name": "Amit Verma",
    "outstanding_amount": 980,
    "consumer_type": "Residential",
    "last_payment_date": "10-02-2026",
    "last_payment_amount": 500,
    "current_month_usage": 45
  }
}
```

### Error Response Examples

**Connection Not Found (404)**
```json
{
  "status": false,
  "message": "Water connection WT-456789 not found",
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
| `data` | Object | Water connection dues object |
| `data.connection_no` | String | Water connection number |
| `data.consumer_name` | String | Consumer's name |
| `data.outstanding_amount` | Number | Outstanding amount (₹) |
| `data.consumer_type` | String | Type of consumer (Residential/Commercial) |
| `data.last_payment_date` | String | Date of last payment (DD-MM-YYYY) |
| `data.last_payment_amount` | Number | Amount of last payment (₹) |
| `data.current_month_usage` | Number | Current month water usage (units) |

## Usage Notes

- Requires valid JWT token from authentication
- Connection number can be obtained from [Get Connections](get-connections.md)
- Shows current outstanding dues and payment history
- Use outstanding_amount to determine payment amount

## Example cURL Request

```bash
curl -X GET "https://api.varansinagar.gov.in/api/water/dues?connection_no=WT-456789" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Get Connections](get-connections.md) - Get list of all water connections
- [Initiate Payment](initiate-payment.md) - Pay outstanding water dues
