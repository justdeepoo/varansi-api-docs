# Complete Integration Example - Python

This example demonstrates a complete integration with the Varanasi Nagar Nigam Chatbot API using Python.

## Installation

```bash
pip install requests
```

## Implementation

```python
import requests
import json
from datetime import datetime
from typing import Optional, Dict, List, Any

class VaransiNagarNigamClient:
    def __init__(self):
        self.base_url = 'https://api.varansinagar.gov.in/api'
        self.token = None
        self.session = requests.Session()
        self.session.headers.update({
            'Content-Type': 'application/json'
        })

    # ==================== Authentication ====================

    def send_otp(self, mobile_no: str) -> Dict[str, Any]:
        """Send OTP to the mobile number"""
        try:
            url = f'{self.base_url}/chatbot/login/send-otp'
            payload = {
                'mobile_no': mobile_no,
                'otp_type': 'ChatbotLogin'
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error sending OTP: {e}')
            raise

    def verify_otp(self, mobile_no: str, otp: str) -> Dict[str, Any]:
        """Verify OTP and get authentication token"""
        try:
            url = f'{self.base_url}/chatbot/login/verify-otp'
            payload = {
                'mobile_no': mobile_no,
                'otp': otp,
                'otp_type': 'ChatbotLogin'
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            
            data = response.json()
            if data.get('status'):
                self.token = data.get('token')
                self.session.headers.update({
                    'Authorization': f'Bearer {self.token}'
                })
            
            return data
        except requests.exceptions.RequestException as e:
            print(f'Error verifying OTP: {e}')
            raise

    # ==================== Property Tax ====================

    def get_linked_properties(self, mobile_no: str) -> Dict[str, Any]:
        """Get all properties linked to mobile number"""
        try:
            url = f'{self.base_url}/property/mobile-properties'
            params = {'mobile_no': mobile_no}
            response = self.session.get(url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching properties: {e}')
            raise

    def get_property_details(self, property_id: str) -> Dict[str, Any]:
        """Get detailed information about a specific property"""
        try:
            url = f'{self.base_url}/property/details'
            params = {'pid': property_id}
            response = self.session.get(url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching property details: {e}')
            raise

    def initiate_property_payment(self, property_id: str, amount: float, mobile_no: str) -> Dict[str, Any]:
        """Initiate property tax payment"""
        try:
            url = f'{self.base_url}/property/payment/initiate'
            payload = {
                'pid': property_id,
                'amount': amount,
                'mobile_no': mobile_no
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error initiating payment: {e}')
            raise

    # ==================== Water Tax ====================

    def get_water_connections(self, mobile_no: str) -> Dict[str, Any]:
        """Get all water connections linked to mobile number"""
        try:
            url = f'{self.base_url}/water/mobile-connections'
            params = {'mobile_no': mobile_no}
            response = self.session.get(url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching water connections: {e}')
            raise

    def get_water_dues(self, connection_no: str) -> Dict[str, Any]:
        """Get outstanding water dues for a connection"""
        try:
            url = f'{self.base_url}/water/dues'
            params = {'connection_no': connection_no}
            response = self.session.get(url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching water dues: {e}')
            raise

    def initiate_water_payment(self, connection_no: str, amount: float, mobile_no: str) -> Dict[str, Any]:
        """Initiate water tax payment"""
        try:
            url = f'{self.base_url}/water/payment/initiate'
            payload = {
                'connection_no': connection_no,
                'amount': amount,
                'mobile_no': mobile_no
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error initiating payment: {e}')
            raise

    # ==================== Community Hall ====================

    def list_community_halls(self) -> Dict[str, Any]:
        """List all available community halls"""
        try:
            url = f'{self.base_url}/community-hall/list'
            response = self.session.get(url)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching community halls: {e}')
            raise

    def check_hall_availability(self, hall_id: int, booking_date: str) -> Dict[str, Any]:
        """Check availability of a hall for a specific date"""
        try:
            url = f'{self.base_url}/community-hall/availability'
            payload = {
                'hall_id': hall_id,
                'booking_date': booking_date
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error checking availability: {e}')
            raise

    def book_community_hall(
        self,
        hall_id: int,
        booking_date: str,
        slot: str,
        applicant_name: str,
        mobile_no: str,
        event_type: str = '',
        expected_guests: int = 0
    ) -> Dict[str, Any]:
        """Book a community hall"""
        try:
            url = f'{self.base_url}/community-hall/book'
            payload = {
                'hall_id': hall_id,
                'booking_date': booking_date,
                'slot': slot,
                'applicant_name': applicant_name,
                'mobile_no': mobile_no,
                'event_type': event_type,
                'expected_guests': expected_guests
            }
            response = self.session.post(url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error booking hall: {e}')
            raise

    # ==================== Transactions ====================

    def get_transaction_history(
        self,
        mobile_no: str,
        from_date: Optional[str] = None,
        to_date: Optional[str] = None,
        service_type: Optional[str] = None
    ) -> Dict[str, Any]:
        """Get transaction history"""
        try:
            url = f'{self.base_url}/transactions/history'
            params = {'mobile_no': mobile_no}
            
            if from_date:
                params['from_date'] = from_date
            if to_date:
                params['to_date'] = to_date
            if service_type:
                params['service_type'] = service_type
            
            response = self.session.get(url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f'Error fetching transaction history: {e}')
            raise


# ==================== Usage Example ====================

def demonstrate_api():
    """Demonstrate complete API workflow"""
    client = VaransiNagarNigamClient()
    mobile_no = '9876543210'

    try:
        # Step 1: Send OTP
        print('Step 1: Sending OTP...')
        otp_response = client.send_otp(mobile_no)
        print(f'OTP Response: {json.dumps(otp_response, indent=2)}')

        # Step 2: Verify OTP
        print('\nStep 2: Verifying OTP...')
        verify_response = client.verify_otp(mobile_no, '458921')
        print(f'Verify Response: {json.dumps(verify_response, indent=2)}')

        if verify_response.get('status'):
            # Step 3: Get linked properties
            print('\nStep 3: Fetching linked properties...')
            properties_response = client.get_linked_properties(mobile_no)
            print(f'Properties: {json.dumps(properties_response, indent=2)}')

            if properties_response.get('data'):
                property_id = properties_response['data'][0]['pid']

                # Step 4: Get property details
                print('\nStep 4: Fetching property details...')
                details_response = client.get_property_details(property_id)
                print(f'Property Details: {json.dumps(details_response, indent=2)}')

                # Step 5: Initiate payment
                print('\nStep 5: Initiating payment...')
                payment_response = client.initiate_property_payment(
                    property_id,
                    details_response['data']['outstanding_amount'],
                    mobile_no
                )
                print(f'Payment Initiated: {json.dumps(payment_response, indent=2)}')

            # Step 6: Get water connections
            print('\nStep 6: Fetching water connections...')
            connections_response = client.get_water_connections(mobile_no)
            print(f'Water Connections: {json.dumps(connections_response, indent=2)}')

            # Step 7: Get community halls
            print('\nStep 7: Fetching community halls...')
            halls_response = client.list_community_halls()
            print(f'Community Halls: {json.dumps(halls_response, indent=2)}')

            # Step 8: Get transaction history
            print('\nStep 8: Fetching transaction history...')
            history_response = client.get_transaction_history(mobile_no)
            print(f'Transaction History: {json.dumps(history_response, indent=2)}')

    except Exception as e:
        print(f'Error: {e}')


if __name__ == '__main__':
    demonstrate_api()
```

## Usage Example

```python
from varansi_client import VaransiNagarNigamClient

# Initialize client
client = VaransiNagarNigamClient()

# Login
client.send_otp('9876543210')
client.verify_otp('9876543210', '458921')

# Get properties
properties = client.get_linked_properties('9876543210')
property_id = properties['data'][0]['pid']

# Get property details
details = client.get_property_details(property_id)
outstanding = details['data']['outstanding_amount']

# Initiate payment
payment = client.initiate_property_payment(property_id, outstanding, '9876543210')
print(f'Pay at: {payment["payment_url"]}')

# Get transaction history
history = client.get_transaction_history('9876543210')
for transaction in history['data']:
    print(f'{transaction["service"]}: ₹{transaction["amount"]}')
```

## Features

- **Session Management** - Persistent session with automatic header updates
- **Token Handling** - Automatic token injection after login
- **Error Handling** - Comprehensive exception handling
- **Type Hints** - Full type annotations for better code clarity
- **Flexible Parameters** - Optional parameters for advanced filtering

## Exception Handling

```python
from requests.exceptions import RequestException, Timeout, ConnectionError

try:
    response = client.send_otp('9876543210')
except Timeout:
    print('Request timed out')
except ConnectionError:
    print('Connection error')
except RequestException as e:
    print(f'Request failed: {e}')
```
