# Send Notifications/Messages to Users

Send broadcast or individual notification messages to users via WhatsApp Bot.

## Overview

This endpoint allows the Varanasi Nagar Nigam to send notifications to users regarding:
- Payment reminders
- New bills/invoices generated
- Payment confirmations
- System updates
- Important notices

Two types of messaging are supported:
1. **Broadcast Message** - Send same message to multiple users
2. **Individual Message** - Send personalized message to specific user

---

## 1. Send Individual Message

Send a targeted message to a specific user.

### Endpoint
```
POST /notification/send-individual
```

### Authentication
Required. Admin/System authentication token.

### Request Header
```
Authorization: Bearer <admin_token>
Content-Type: application/json
```

### Request Body

#### Notification Types and Their Payloads

**Type 1: Payment Reminder**
```json
{
  "notification_type": "payment_reminder",
  "recipient_type": "individual",
  "mobile_no": "9876543210",
  "message_template": "payment_reminder",
  "content": {
    "owner_name": "Rahul Sharma",
    "property_id": "VN-PT-102345",
    "address": "123, Sigra, Varanasi",
    "outstanding_amount": 3500,
    "due_date": "10-03-2026",
    "taxes": {
      "property_tax": 2000,
      "water_tax": 1000,
      "sewerage_tax": 500
    },
    "payment_link": "https://pay.varansinagar.gov.in/p/VN-PT-102345"
  }
}
```

**Type 2: Payment Confirmation**
```json
{
  "notification_type": "payment_confirmation",
  "recipient_type": "individual",
  "mobile_no": "9876543210",
  "message_template": "payment_confirmation",
  "content": {
    "owner_name": "Rahul Sharma",
    "property_id": "VN-PT-102345",
    "transaction_id": "TXN-20260317-001234",
    "amount_paid": 3500,
    "payment_date": "17-03-2026",
    "payment_time": "14:35:22",
    "taxes_paid": {
      "property_tax": 2000,
      "water_tax": 1000,
      "sewerage_tax": 500
    },
    "receipt_url": "https://storage.varansinagar.gov.in/receipts/TXN-20260317-001234.pdf"
  }
}
```

**Type 3: New Invoice Generated**
```json
{
  "notification_type": "new_invoice",
  "recipient_type": "individual",
  "mobile_no": "9876543210",
  "message_template": "new_invoice_generated",
  "content": {
    "owner_name": "Rahul Sharma",
    "property_id": "VN-PT-102345",
    "invoice_number": "INV-VN-102345-202602",
    "invoice_month": "February 2026",
    "invoice_amount": 1200,
    "invoice_url": "https://storage.varansinagar.gov.in/invoices/INV-VN-102345-202602.pdf"
  }
}
```

**Type 4: General Notice**
```json
{
  "notification_type": "general_notice",
  "recipient_type": "individual",
  "mobile_no": "9876543210",
  "message_template": "custom_message",
  "content": {
    "title": "System Maintenance",
    "message": "The payment system will be under maintenance on 18-03-2026 from 10 PM to 2 AM. Please plan your payments accordingly.",
    "priority": "HIGH"
  }
}
```

### Example Request
```bash
curl -X POST "https://api.varansinagar.gov.in/api/notification/send-individual" \
  -H "Authorization: Bearer admin-token-xyz" \
  -H "Content-Type: application/json" \
  -d '{
    "notification_type": "payment_reminder",
    "recipient_type": "individual",
    "mobile_no": "9876543210",
    "message_template": "payment_reminder",
    "content": {
      "owner_name": "Rahul Sharma",
      "property_id": "VN-PT-102345",
      "address": "123, Sigra, Varanasi",
      "outstanding_amount": 3500,
      "due_date": "10-03-2026",
      "payment_link": "https://pay.varansinagar.gov.in/p/VN-PT-102345"
    }
  }'
```

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Message sent successfully",
  "data": {
    "notification_id": "NOTIF-20260317-004567",
    "mobile_no": "9876543210",
    "notification_type": "payment_reminder",
    "status": "DELIVERED",
    "sent_at": "17-03-2026 15:30:22",
    "whatsapp_message_id": "wamid.HBEUGExampleMessageId123"
  }
}
```

---

## 2. Send Broadcast Message

Send same message to multiple users at once.

### Endpoint
```
POST /notification/send-broadcast
```

### Authentication
Required. Admin/System authentication token.

### Request Header
```
Authorization: Bearer <admin_token>
Content-Type: application/json
```

### Request Body

#### Broadcast Type 1: Payment Reminder to All Users with Due Payments
```json
{
  "notification_type": "payment_reminder",
  "recipient_type": "broadcast",
  "target_audience": "all_users_with_dues",
  "message_template": "payment_reminder_broadcast",
  "filter": {
    "outstanding_amount_min": 100,
    "property_type": null
  },
  "content": {
    "title": "⏰ Payment Reminder",
    "message": "You have outstanding tax dues. Pay online now to avoid penalties.",
    "cta_text": "Pay Now",
    "cta_link": "https://pay.varansinagar.gov.in/dashboard"
  }
}
```

#### Broadcast Type 2: System Notification to All Users
```json
{
  "notification_type": "system_notification",
  "recipient_type": "broadcast",
  "target_audience": "all_users",
  "message_template": "system_update",
  "content": {
    "title": "📢 Important Notice",
    "message": "Water supply will be disrupted on 20-03-2026 from 6 AM to 6 PM for pipeline maintenance.",
    "priority": "HIGH"
  }
}
```

#### Broadcast Type 3: Invoice Generation Notification
```json
{
  "notification_type": "new_invoice",
  "recipient_type": "broadcast",
  "target_audience": "users_with_property_ids",
  "message_template": "invoice_generated_batch",
  "filter": {
    "property_ids": ["VN-PT-102345", "VN-PT-102346", "VN-PT-102347"]
  },
  "content": {
    "title": "📄 New Invoice Generated",
    "message": "Your February 2026 bill is now ready. Check online anytime.",
    "cta_text": "View Invoice",
    "cta_link": "https://pay.varansinagar.gov.in/invoices"
  }
}
```

#### Broadcast Type 4: Quarterly Campaign
```json
{
  "notification_type": "campaign",
  "recipient_type": "broadcast",
  "target_audience": "all_active_users",
  "message_template": "quarterly_campaign",
  "content": {
    "title": "💰 Early Payment Discount",
    "message": "Get 2% discount if you pay your property tax by 31-03-2026. Limited time offer!",
    "cta_text": "Claim Offer",
    "cta_link": "https://pay.varansinagar.gov.in/offers"
  }
}
```

### Example Request
```bash
curl -X POST "https://api.varansinagar.gov.in/api/notification/send-broadcast" \
  -H "Authorization: Bearer admin-token-xyz" \
  -H "Content-Type: application/json" \
  -d '{
    "notification_type": "payment_reminder",
    "recipient_type": "broadcast",
    "target_audience": "all_users_with_dues",
    "message_template": "payment_reminder_broadcast",
    "content": {
      "title": "⏰ Payment Reminder",
      "message": "You have outstanding tax dues. Pay online now to avoid penalties.",
      "cta_text": "Pay Now",
      "cta_link": "https://pay.varansinagar.gov.in/dashboard"
    }
  }'
```

### Success Response (200 OK)
```json
{
  "status": true,
  "message": "Broadcast message queued successfully",
  "data": {
    "campaign_id": "CAMP-20260317-008901",
    "notification_type": "payment_reminder",
    "target_audience": "all_users_with_dues",
    "total_recipients": 2456,
    "status": "QUEUED",
    "scheduled_at": "17-03-2026 16:00:00",
    "estimated_delivery_time": "17-03-2026 16:30:00",
    "message": "Broadcast will be delivered to 2456 users over next 30 minutes"
  }
}
```

---

## Notification Types Reference

| Type | Use Case | Default Template |
|------|----------|------------------|
| `payment_reminder` | Remind users about due payment | payment_reminder |
| `payment_confirmation` | Confirm successful payment | payment_confirmation |
| `new_invoice` | Notify new invoice generation | new_invoice_generated |
| `general_notice` | General announcements | custom_message |
| `system_notification` | System updates/maintenance | system_update |
| `campaign` | Promotional campaigns | campaign_template |

## Broadcast Target Audiences

| Audience | Description |
|----------|-------------|
| `all_users` | All registered users |
| `all_users_with_dues` | Users with outstanding dues |
| `all_users_without_dues` | Users with no dues (clear status) |
| `users_with_property_ids` | Specific users in filter.property_ids |
| `all_active_users` | Users active in last 30 days |

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| status | boolean | Request success indicator |
| message | string | Response message |
| notification_id | string | Unique notification identifier |
| campaign_id | string | Campaign ID for broadcast |
| mobile_no | string | Recipient mobile number |
| status | string | DELIVERED/QUEUED/FAILED |
| sent_at | string | Timestamp of sending |
| total_recipients | number | Total users in broadcast |

## Message Template Variables

Use these variables in messages, they will be automatically replaced:

```
{owner_name}           - Property owner name
{property_id}          - Property identifier
{address}              - Property address
{outstanding_amount}   - Pending dues amount
{total_recipients}     - Total recipients (broadcast only)
{invoice_number}       - Invoice number
{transaction_id}       - Transaction ID
{payment_date}         - Payment date
{due_date}             - Payment due date
```

Example:
```
Hi {owner_name}, your tax dues for property {property_id} 
at {address} is Rs. {outstanding_amount}. Due date: {due_date}
```

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| SUCCESS | 200 | Message sent successfully |
| INVALID_MOBILE | 400 | Invalid mobile number |
| INVALID_TEMPLATE | 400 | Message template not found |
| INVALID_TOKEN | 401 | Unauthorized request |
| RATE_LIMIT | 429 | Too many requests sent |
| GATEWAY_ERROR | 503 | WhatsApp gateway unavailable |
| SERVER_ERROR | 500 | Internal server error |

## Notes

- Individual messages are sent immediately
- Broadcast messages are queued and delivered within 30 minutes
- All messages must follow WhatsApp Business API guidelines
- Emojis are supported in messages
- Message delivery confirmation is tracked
- Failed messages are retried automatically (up to 3 times)
