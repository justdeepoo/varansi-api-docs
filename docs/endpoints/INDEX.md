# API Endpoints Index

Complete list of all API endpoints organized by service category.

## Authentication Endpoints

### Citizen Login
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/chatbot/login/send-otp` | [Send OTP](authentication/send-otp.md) |
| POST | `/chatbot/login/verify-otp` | [Verify OTP](authentication/verify-otp.md) |

## Property Tax Endpoints

### Property Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/property/mobile-properties` | [Get Linked Properties](property-tax/get-properties.md) |
| GET | `/property/details` | [Get Property Details](property-tax/get-property-details.md) |
| POST | `/property/payment/initiate` | [Initiate Payment](property-tax/initiate-payment.md) |

## Water Tax Endpoints

### Water Connection Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/water/mobile-connections` | [Get Connections](water-tax/get-connections.md) |
| GET | `/water/dues` | [Check Dues](water-tax/check-dues.md) |
| POST | `/water/payment/initiate` | [Initiate Payment](water-tax/initiate-payment.md) |

## Community Hall Endpoints

### Hall Booking
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/community-hall/list` | [List Halls](community-hall/list-halls.md) |
| POST | `/community-hall/availability` | [Check Availability](community-hall/check-availability.md) |
| POST | `/community-hall/book` | [Book Hall](community-hall/book-hall.md) |

## Transaction Endpoints

### Transaction Management
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/transactions/history` | [Transaction History](transaction-history.md) |

## Authentication Requirements

- **No Authentication**: `/chatbot/login/send-otp`, `/chatbot/login/verify-otp`
- **JWT Required**: All other endpoints

## Rate Limiting

- **Standard endpoints**: 30 requests per minute
- **Payment endpoints**: 10 requests per minute
- **OTP endpoints**: 5 requests per 5 minutes

See [Base Configuration](../shared/base-config.md) for detailed rate limiting information.
