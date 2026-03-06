# Community Hall - Book Hall

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | POST |
| **URL** | `/community-hall/book` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 10 requests per minute |

## Description
Submit a booking request for a community hall. After successful submission, the request is sent for approval by the Nagar Nigam authorities.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Request Body
```json
{
  "hall_id": 1,
  "booking_date": "2026-03-20",
  "slot": "Morning",
  "applicant_name": "Rahul Sharma",
  "mobile_no": "9876543210",
  "event_type": "Wedding",
  "expected_guests": 150
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hall_id` | Number | Yes | Hall ID from [List Halls](list-halls.md) |
| `booking_date` | String | Yes | Booking date (YYYY-MM-DD format) |
| `slot` | String | Yes | Time slot (Morning/Afternoon/Evening) |
| `applicant_name` | String | Yes | Full name of applicant |
| `mobile_no` | String | Yes | 10-digit mobile number |
| `event_type` | String | No | Type of event (Wedding/Conference/Community Function) |
| `expected_guests` | Number | No | Expected number of guests |

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "booking_id": "HALL-98765",
  "message": "Booking request submitted successfully",
  "approval_status": "Pending",
  "expected_approval_time": "2-3 business days"
}
```

### Error Response Examples

**Slot Not Available (400)**
```json
{
  "status": false,
  "message": "Selected slot is not available for this date",
  "error_code": "VALIDATION_ERROR"
}
```

**Invalid Hall ID (404)**
```json
{
  "status": false,
  "message": "Community hall with ID 1 not found",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

**Guest Count Exceeds Capacity (400)**
```json
{
  "status": false,
  "message": "Expected guests exceed hall capacity",
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
| `booking_id` | String | Unique booking reference ID |
| `message` | String | Confirmation message |
| `approval_status` | String | Current status (Pending/Approved/Rejected) |
| `expected_approval_time` | String | Estimated time for approval |

## Booking Process

1. **Check Availability** - Use [Check Availability](check-availability.md)
2. **Submit Booking** - Submit booking request with details
3. **Receive Confirmation** - Get booking_id for reference
4. **Wait for Approval** - Status updated within 2-3 business days
5. **Receive Confirmation** - SMS/Email sent on approval

## Usage Notes

- Requires valid JWT token from authentication
- Booking must be checked for availability first
- Applicant name should match registered mobile number
- Guest count cannot exceed hall capacity
- Event type helps in hall arrangements
- Booking ID is required for modifications or cancellations

## Booking Status

| Status | Description |
|--------|-------------|
| Pending | Awaiting approval from Nagar Nigam |
| Approved | Booking confirmed and approved |
| Rejected | Application rejected (reason provided) |
| Cancelled | Booking cancelled by applicant or admin |

## Example cURL Request

```bash
curl -X POST https://api.varansinagar.gov.in/api/community-hall/book \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "hall_id": 1,
    "booking_date": "2026-03-20",
    "slot": "Morning",
    "applicant_name": "Rahul Sharma",
    "mobile_no": "9876543210",
    "event_type": "Wedding",
    "expected_guests": 150
  }'
```

## Related Endpoints
- [List Halls](list-halls.md) - Get list of all community halls
- [Check Availability](check-availability.md) - Check hall availability
- [Transaction History](../transaction-history.md) - Track bookings and transactions
