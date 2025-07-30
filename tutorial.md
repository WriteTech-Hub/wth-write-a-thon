# Tutorial Guide: Integrate Payout to Interledger Wallet API

## Pre-requisites
- Node.js and npm installed
- Chimoney Developer Account
- Chimoney API Key
- Interledger-compatible wallet (e.g. Rafiki)
- Basic JavaScript knowledge

## Step 1: Install Axios
```bash
npm install axios
```

## Step 2: Create the Function
```javascript
const axios = require('axios');
const apiKey = 'YOUR_CHIMONEY_API_KEY';

async function sendInterledgerPayout(walletAddress, amount, currency, reason) {
  const url = 'https://api.chimoney.io/v0.2/payouts/interledger';
  const headers = {
    'Content-Type': 'application/json',
    'x-api-key': apiKey,
  };
  const data = { walletAddress, amount, currency, reason };

  try {
    const response = await axios.post(url, data, { headers });
    if (response.status === 200 && response.data.success) {
      console.log('✅ Payout Successful!');
      console.log('Transaction Reference:', response.data.reference);
    } else {
      console.error('❌ Error:', response.data.message || response.data.error);
    }
  } catch (error) {
    console.error('🚨 Network Error:', error.response?.data || error.message);
  }
}
```

## Step 3: Example Usage
```javascript
sendInterledgerPayout(
  'kenny.rfi.ilp.network',
  25,
  'USD',
  'DAO Translation Bounty'
);
```

## Sample Response
```json
{
  "success": true,
  "message": "Payout sent successfully",
  "reference": "CHM-kjs728s9hds",
  "amount": 25,
  "currency": "USD"
}
```