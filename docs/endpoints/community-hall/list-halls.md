# Community Hall - List Halls

## Endpoint Details

| Property | Value |
|----------|-------|
| **Method** | GET |
| **URL** | `/community-hall/list` |
| **Authentication** | Required (JWT Token) |
| **Rate Limit** | 30 requests per minute |

## Description
Retrieve a list of all available community halls in Varanasi. Shows hall ID and name for each available facility.

## Request

### Request Headers
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Query Parameters
None required

### URL Example
```
GET /community-hall/list
```

## Response

### Success Response (200 OK)
```json
{
  "status": true,
  "data": [
    {
      "hall_id": 1,
      "hall_name": "Sigra Community Hall",
      "capacity": 200,
      "location": "Sigra, Varanasi"
    },
    {
      "hall_id": 2,
      "hall_name": "Assi Community Hall",
      "capacity": 150,
      "location": "Assi Ghat, Varanasi"
    }
  ]
}
```

### Error Response Examples

**No Halls Available (404)**
```json
{
  "status": false,
  "message": "No community halls available",
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
| `data` | Array | Array of community hall objects |
| `data[].hall_id` | Number | Hall ID (unique identifier) |
| `data[].hall_name` | String | Name of the community hall |
| `data[].capacity` | Number | Maximum seating capacity |
| `data[].location` | String | Hall location/address |

## Usage Notes

- Requires valid JWT token from authentication
- Returns all available community halls
- Use `hall_id` to check availability and make bookings
- Capacity information helps in selecting appropriate hall

## Example cURL Request

```bash
curl -X GET https://api.varansinagar.gov.in/api/community-hall/list \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

## Related Endpoints
- [Check Availability](check-availability.md) - Check hall availability for specific dates
- [Book Hall](book-hall.md) - Submit booking request
