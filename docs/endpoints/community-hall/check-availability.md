# Community Hall - Check Availability

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | POST |
| **URL** | `/community-hall/availability` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Check the availability of a specific community hall for a given booking date. Shows available time slots for the selected date.

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
  "booking_date": "2026-03-20"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `hall_id` | Number | Yes | Hall ID from [List Halls](list-halls.md) |
| `booking_date` | String | Yes | Date to check (YYYY-MM-DD format) |

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": [
    {
      "slot": "Morning",
      "available": true,
      "time": "06:00 AM - 12:00 PM"
    },
    {
      "slot": "Afternoon",
      "available": true,
      "time": "12:00 PM - 06:00 PM"
    },
    {
      "slot": "Evening",
      "available": false,
      "time": "06:00 PM - 12:00 AM"
    }
  ]
}
```

### Error Response Examples

**Hall Not Found (404)**
```json
{
  "status": false,
  "message": "Community hall with ID 1 not found",
  "error_code": "RESOURCE_NOT_FOUND"
}
```

**Invalid Date (400)**
```json
{
  "status": false,
  "message": "Booking date must be in future",
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
| `data` | Array | Array of time slot objects |
| `data[].slot` | String | Slot name (Morning/Afternoon/Evening) |
| `data[].available` | Boolean | Whether slot is available for booking |
| `data[].time` | String | Time range for the slot |

## Time Slots

| Slot | Time Range |
|------|-----------|
| Morning | 6:00 AM - 12:00 PM |
| Afternoon | 12:00 PM - 6:00 PM |
| Evening | 6:00 PM - 12:00 AM |

## Usage Notes

- Requires valid JWT token from authentication
- Booking date must be in the future (minimum 2 days ahead)
- Maximum booking advance is 90 days
- Multiple slots can be available on same date
- Use slot name for booking

## Example cURL Request

```bash
curl -X POST https://api.varansinagar.gov.in/api/community-hall/availability \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "hall_id": 1,
    "booking_date": "2026-03-20"
  }'
```

## Related Endpoints
- [List Halls](list-halls.md) - Get list of all community halls
- [Book Hall](book-hall.md) - Submit booking request for available slot
