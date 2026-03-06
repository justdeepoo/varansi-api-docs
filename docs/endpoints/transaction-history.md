# Transaction History

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/transactions/history` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Retrieve the complete transaction history for a citizen including all payments made for property tax, water tax, and other services. Useful for tracking payment records and verification.

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
| `from_date` | String | No | Start date (YYYY-MM-DD) |
| `to_date` | String | No | End date (YYYY-MM-DD) |
| `service_type` | String | No | Filter by service (PropertyTax/WaterTax/CommunityHall) |

### URL Examples
```
GET /transactions/history?mobile_no=9876543210
GET /transactions/history?mobile_no=9876543210&from_date=2025-01-01&to_date=2026-03-06
GET /transactions/history?mobile_no=9876543210&service_type=PropertyTax
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": [
    {
      "transaction_id": "PTXN456987",
      "service": "Property Tax",
      "amount": 4250,
      "date": "05-02-2026",
      "time": "14:30:45",
      "status": "Success",
      "reference_id": "ORD987654",
      "property_id": "VN-PT-102345"
    },
    {
      "transaction_id": "WTXN123456",
      "service": "Water Tax",
      "amount": 980,
      "date": "02-02-2026",
      "time": "10:15:20",
      "status": "Success",
      "reference_id": "ORD123456",
      "connection_id": "WT-456789"
    },
    {
      "transaction_id": "CHTXN789012",
      "service": "Community Hall",
      "amount": 5000,
      "date": "01-02-2026",
      "time": "16:45:30",
      "status": "Pending",
      "reference_id": "HALL-98765",
      "booking_id": "HALL-98765"
    }
  ]
}
```

### Error Response Examples

**No Transactions Found (404)**
```json
{
  "status": false,
  "message": "No transactions found for this mobile number",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

**Invalid Date Format (400)**
```json
{
  "status": false,
  "message": "Invalid date format. Use YYYY-MM-DD",
  "error_code": "VALIDATION_ERROR"
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
| `data` | Array | Array of transaction objects |
| `data[].transaction_id` | String | Unique transaction ID |
| `data[].service` | String | Service type (Property Tax/Water Tax/Community Hall) |
| `data[].amount` | Number | Transaction amount (₹) |
| `data[].date` | String | Transaction date (DD-MM-YYYY) |
| `data[].time` | String | Transaction time (HH:MM:SS) |
| `data[].status` | String | Transaction status (Success/Failed/Pending) |
| `data[].reference_id` | String | Payment gateway order ID |
| `data[].property_id` | String | Property ID (for property tax transactions) |
| `data[].connection_id` | String | Connection ID (for water tax transactions) |
| `data[].booking_id` | String | Booking ID (for community hall transactions) |

## Transaction Status

| Status | Description |
|--------|-------------|
| Success | Payment completed successfully |
| Failed | Payment failed or declined |
| Pending | Payment initiated but not confirmed |
| Cancelled | Transaction cancelled by user or admin |

## Usage Notes

- Requires valid JWT token from authentication
- Shows last 100 transactions by default
- Date filters are optional
- Service type filter helps in quick lookup
- All amounts are in Indian Rupees (₹)
- Transactions are sorted by date in descending order (latest first)

## Example cURL Requests

**Get all transactions for a user:**
```bash
curl -X GET "https://api.varansinagar.gov.in/api/transactions/history?mobile_no=9876543210" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

**Get property tax transactions from a specific date range:**
```bash
curl -X GET "https://api.varansinagar.gov.in/api/transactions/history?mobile_no=9876543210&from_date=2026-01-01&to_date=2026-03-06&service_type=PropertyTax" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Property Tax - Initiate Payment](endpoints/property-tax/initiate-payment.md)
- [Water Tax - Initiate Payment](endpoints/water-tax/initiate-payment.md)
- [Community Hall - Book Hall](endpoints/community-hall/book-hall.md)
