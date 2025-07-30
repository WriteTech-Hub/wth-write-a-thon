# Setup Guide: Chimoney Interledger Wallet Payout Integration

## Step 1: Create Chimoney Account & Get API Key
- Sign up at [chimoney.io/developers](https://chimoney.io/developers)
- Go to Dashboard → API Keys
- Copy your sandbox or production `x-api-key`

## Step 2: Setup Development Environment
```bash
mkdir chimoney-interledger-payout
cd chimoney-interledger-payout
npm init -y
npm install axios dotenv
```

## Step 3: Store API Key Securely
Create a `.env` file:
```
CHIMONEY_API_KEY=your_api_key_here
```

Use it in your code:
```javascript
require('dotenv').config();
const apiKey = process.env.CHIMONEY_API_KEY;
```

## Step 4: Get Interledger Wallet
- Sign up at [rafiki.money](https://rafiki.money)
- Use an address like `username.rfi.ilp.network`

## Step 5: Make Your First Test Call
```javascript
const axios = require('axios');
require('dotenv').config();

const apiKey = process.env.CHIMONEY_API_KEY;

async function sendInterledgerPayout() {
  const url = 'https://api.chimoney.io/v0.2/payouts/interledger';
  const data = {
    walletAddress: 'example.rfi.ilp.network',
    amount: 10,
    currency: 'USD',
    reason: 'Test payout from setup guide'
  };
  const headers = {
    'Content-Type': 'application/json',
    'x-api-key': apiKey,
  };

  try {
    const response = await axios.post(url, data, { headers });
    if (response.status === 200 && response.data.success) {
      console.log('✅ Setup Successful!');
      console.log('Reference:', response.data.reference);
    } else {
      console.error('❌ Setup failed:', response.data.message);
    }
  } catch (error) {
    console.error('🚨 Error:', error.response?.data || error.message);
  }
}

sendInterledgerPayout();
```