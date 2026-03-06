# Property Tax - Initiate Payment

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | POST |
| **URL** | `/property/payment/initiate` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 10 requests per minute |

## Description
Initiate a payment transaction for property tax. This endpoint generates a payment URL and order ID for redirecting to the payment gateway.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Request Body
```json
{
  "pid": "VN-PT-102345",
  "amount": 4250,
  "mobile_no": "9876543210"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pid` | String | Yes | Property ID |
| `amount` | Number | Yes | Amount to pay (₹) |
| `mobile_no` | String | Yes | 10-digit mobile number |

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "payment_url": "https://paymentgateway.com/pay/abc123",
  "order_id": "ORD987654",
  "amount": 4250,
  "expiry_time": "2026-03-06T13:45:00+05:30"
}
```

### Error Response Examples

**Invalid Amount (400)**
```json
{
  "status": false,
  "message": "Amount must be greater than 0",
  "error_code": "VALIDATION_ERROR"
}
```

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
| `payment_url` | String | URL to redirect user for payment |
| `order_id` | String | Unique order ID for tracking |
| `amount` | Number | Amount to be paid (₹) |
| `expiry_time` | String | Payment link expiry time (ISO 8601) |

## Payment Flow

1. **Initiate Payment** - Get payment URL and order ID
2. **Redirect User** - Send user to `payment_url`
3. **User Completes Payment** - User enters payment details on gateway
4. **Confirmation Webhook** - Payment gateway sends confirmation (registered separately)
5. **Check Transaction** - Verify payment using [Transaction History](../transaction-history.md)

## Amount Validation

- Minimum: ₹100
- Maximum: No limit
- Decimal places: 2 (e.g., 4250.50)
- Must match or be less than outstanding amount

## Usage Notes

- Payment link expires 30 minutes after generation
- Order ID is required for payment verification
- Payment URL is unique to each transaction
- After payment, check [Transaction History](../transaction-history.md) to confirm

## Example cURL Request

```bash
curl -X POST https://api.varansinagar.gov.in/api/property/payment/initiate \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "pid": "VN-PT-102345",
    "amount": 4250,
    "mobile_no": "9876543210"
  }'
```

## Related Endpoints
- [Get Property Details](get-property-details.md) - Get outstanding amount
- [Transaction History](../transaction-history.md) - Verify payment completion
