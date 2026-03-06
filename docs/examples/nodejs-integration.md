# Complete Integration Example - Node.js

This example demonstrates a complete integration with the Varanasi Nagar Nigam Chatbot API using Node.js.

## Installation

```bash
npm install axios
```

## Implementation

```javascript
const axios = require('axios');

const BASE_URL = 'https://api.varansinagar.gov.in/api';

class VaransiNagarNigamClient {
  constructor() {
    this.token = null;
    this.client = axios.create({
      baseURL: BASE_URL,
      headers: {
        'Content-Type': 'application/json'
      }
    });
  }

  // ==================== Authentication ====================
  
  async sendOTP(mobileNo) {
    try {
      const response = await this.client.post('/chatbot/login/send-otp', {
        mobile_no: mobileNo,
        otp_type: 'ChatbotLogin'
      });
      return response.data;
    } catch (error) {
      console.error('Error sending OTP:', error.response?.data || error.message);
      throw error;
    }
  }

  async verifyOTP(mobileNo, otp) {
    try {
      const response = await this.client.post('/chatbot/login/verify-otp', {
        mobile_no: mobileNo,
        otp: otp,
        otp_type: 'ChatbotLogin'
      });
      
      if (response.data.status) {
        this.token = response.data.token;
        this.client.defaults.headers['Authorization'] = `Bearer ${this.token}`;
      }
      
      return response.data;
    } catch (error) {
      console.error('Error verifying OTP:', error.response?.data || error.message);
      throw error;
    }
  }

  // ==================== Property Tax ====================

  async getLinkedProperties(mobileNo) {
    try {
      const response = await this.client.get('/property/mobile-properties', {
        params: { mobile_no: mobileNo }
      });
      return response.data;
    } catch (error) {
      console.error('Error fetching properties:', error.response?.data || error.message);
      throw error;
    }
  }

  async getPropertyDetails(propertyId) {
    try {
      const response = await this.client.get('/property/details', {
        params: { pid: propertyId }
      });
      return response.data;
    } catch (error) {
      console.error('Error fetching property details:', error.response?.data || error.message);
      throw error;
    }
  }

  async initiatePropertyPayment(propertyId, amount, mobileNo) {
    try {
      const response = await this.client.post('/property/payment/initiate', {
        pid: propertyId,
        amount: amount,
        mobile_no: mobileNo
      });
      return response.data;
    } catch (error) {
      console.error('Error initiating payment:', error.response?.data || error.message);
      throw error;
    }
  }

  // ==================== Water Tax ====================

  async getWaterConnections(mobileNo) {
    try {
      const response = await this.client.get('/water/mobile-connections', {
        params: { mobile_no: mobileNo }
      });
      return response.data;
    } catch (error) {
      console.error('Error fetching water connections:', error.response?.data || error.message);
      throw error;
    }
  }

  async getWaterDues(connectionNo) {
    try {
      const response = await this.client.get('/water/dues', {
        params: { connection_no: connectionNo }
      });
      return response.data;
    } catch (error) {
      console.error('Error fetching water dues:', error.response?.data || error.message);
      throw error;
    }
  }

  async initiateWaterPayment(connectionNo, amount, mobileNo) {
    try {
      const response = await this.client.post('/water/payment/initiate', {
        connection_no: connectionNo,
        amount: amount,
        mobile_no: mobileNo
      });
      return response.data;
    } catch (error) {
      console.error('Error initiating payment:', error.response?.data || error.message);
      throw error;
    }
  }

  // ==================== Community Hall ====================

  async listCommunityHalls() {
    try {
      const response = await this.client.get('/community-hall/list');
      return response.data;
    } catch (error) {
      console.error('Error fetching community halls:', error.response?.data || error.message);
      throw error;
    }
  }

  async checkHallAvailability(hallId, bookingDate) {
    try {
      const response = await this.client.post('/community-hall/availability', {
        hall_id: hallId,
        booking_date: bookingDate
      });
      return response.data;
    } catch (error) {
      console.error('Error checking availability:', error.response?.data || error.message);
      throw error;
    }
  }

  async bookCommunityHall(hallId, bookingDate, slot, applicantName, mobileNo, eventType = '', expectedGuests = 0) {
    try {
      const response = await this.client.post('/community-hall/book', {
        hall_id: hallId,
        booking_date: bookingDate,
        slot: slot,
        applicant_name: applicantName,
        mobile_no: mobileNo,
        event_type: eventType,
        expected_guests: expectedGuests
      });
      return response.data;
    } catch (error) {
      console.error('Error booking hall:', error.response?.data || error.message);
      throw error;
    }
  }

  // ==================== Transactions ====================

  async getTransactionHistory(mobileNo, fromDate = null, toDate = null, serviceType = null) {
    try {
      const params = { mobile_no: mobileNo };
      if (fromDate) params.from_date = fromDate;
      if (toDate) params.to_date = toDate;
      if (serviceType) params.service_type = serviceType;

      const response = await this.client.get('/transactions/history', { params });
      return response.data;
    } catch (error) {
      console.error('Error fetching transaction history:', error.response?.data || error.message);
      throw error;
    }
  }
}

// ==================== Usage Example ====================

async function demonstrateAPI() {
  const client = new VaransiNagarNigamClient();
  const mobileNo = '9876543210';

  try {
    // Step 1: Send OTP
    console.log('Step 1: Sending OTP...');
    const otpResponse = await client.sendOTP(mobileNo);
    console.log('OTP Response:', otpResponse);

    // Step 2: Verify OTP (using sample OTP)
    console.log('\nStep 2: Verifying OTP...');
    const verifyResponse = await client.verifyOTP(mobileNo, '458921');
    console.log('Verify Response:', verifyResponse);

    if (verifyResponse.status) {
      // Step 3: Get linked properties
      console.log('\nStep 3: Fetching linked properties...');
      const propertiesResponse = await client.getLinkedProperties(mobileNo);
      console.log('Properties:', propertiesResponse);

      if (propertiesResponse.data && propertiesResponse.data.length > 0) {
        const propertyId = propertiesResponse.data[0].pid;

        // Step 4: Get property details
        console.log('\nStep 4: Fetching property details...');
        const detailsResponse = await client.getPropertyDetails(propertyId);
        console.log('Property Details:', detailsResponse);

        // Step 5: Initiate payment
        console.log('\nStep 5: Initiating payment...');
        const paymentResponse = await client.initiatePropertyPayment(
          propertyId,
          detailsResponse.data.outstanding_amount,
          mobileNo
        );
        console.log('Payment Initiated:', paymentResponse);
      }

      // Step 6: Get water connections
      console.log('\nStep 6: Fetching water connections...');
      const connectionsResponse = await client.getWaterConnections(mobileNo);
      console.log('Water Connections:', connectionsResponse);

      // Step 7: Get community halls
      console.log('\nStep 7: Fetching community halls...');
      const hallsResponse = await client.listCommunityHalls();
      console.log('Community Halls:', hallsResponse);

      // Step 8: Get transaction history
      console.log('\nStep 8: Fetching transaction history...');
      const historyResponse = await client.getTransactionHistory(mobileNo);
      console.log('Transaction History:', historyResponse);
    }
  } catch (error) {
    console.error('API Integration Error:', error.message);
  }
}

// Run the demonstration
demonstrateAPI();

module.exports = VaransiNagarNigamClient;
```

## Key Features

1. **Error Handling** - Try-catch blocks with detailed error logging
2. **Token Management** - Automatic token storage and injection in headers
3. **Modular Methods** - Separate methods for each API endpoint
4. **Parameter Validation** - Optional parameters for filtering
5. **Complete Workflow** - Demonstrates full login and service usage

## Usage

```javascript
const VaransiClient = require('./varansi-client');
const client = new VaransiClient();

// Login
await client.sendOTP('9876543210');
await client.verifyOTP('9876543210', '458921');

// Get properties and pay
const properties = await client.getLinkedProperties('9876543210');
const details = await client.getPropertyDetails(properties.data[0].pid);
const payment = await client.initiatePropertyPayment(
  properties.data[0].pid,
  details.data.outstanding_amount,
  '9876543210'
);

// Redirect user to payment URL
window.location.href = payment.payment_url;
```

## Error Handling

The client includes comprehensive error handling:

```javascript
try {
  const response = await client.sendOTP('9876543210');
  if (response.status) {
    console.log('OTP sent successfully');
  }
} catch (error) {
  if (error.response?.status === 401) {
    console.log('Invalid authentication');
  } else if (error.response?.status === 429) {
    console.log('Rate limit exceeded');
  }
}
```
