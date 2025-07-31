
# Chimoney Multi-Currency Wallet Transfer Setup Guide

This guide will help you set up authentication and environment variables so you can successfully send a payout using Chimoney’s multi-currency wallet transfer endpoint.

## Prerequisites

- Chimoney developer account ([sign up here](https://sandbox.chimoney.io/auth/signup?next=/))
- Access to the Chimoney Dashboard
- Sandbox API key
- Node.js 18+
- npm
- A tool for making API requests (curl, Postman, etc.)

## Step 1: Authentication

Generate a Sandbox API key from the Chimoney dashboard.
[Watch this video](https://www.loom.com/share/436303eb69c44f0d9757ea0c655bed89?sid=b6a0f661-721c-4731-9873-ae6f2d25780) to learn how to generate your API key.


## Step 2: Environment Setup

Check your setup:

```bash
node --version
npm --version
```

Create a new project folder and install dependencies:

```bash
mkdir chimoney-demo
cd chimoney-demo
npm init -y
npm install dotenv
```

Create a `.env` file in your project root, copy your API-KEY from the Developers tab in your sandbox and replace the placeholder.

```env
CHIMONEY_API_KEY=your_sandbox_api_key_here
CHIMONEY_BASE_URL=https://api-v2-sandbox.chimoney.io
```

To authorize requests, you have to include your API key in the header; you will see how to do so in the example below.

## Step 3: Make a Test API Call

Create a file called `testConnection.js`:

```js
require('dotenv').config();

const url = `${process.env.CHIMONEY_BASE_URL}/v0.2.4/multicurrency-wallets/transfer`;
const headers = {
  'Content-Type': 'application/json',
  'X-API-KEY': process.env.CHIMONEY_API_KEY,
};

const body = 
    {
        "amountToSend": "2",
        "originCurrency": "USD",
        "email": "user@mail.com",
        "phoneNumber": "+16471234567",
        "destinationCurrency": "NGN",
        "narration": "multi-currency transfer",
};

fetch(url, {
  method: 'POST',
  headers,
  body: JSON.stringify(body),
})
  .then(res => res.json())
  .then(data => {
    if (data && data.status === 'success') {
      console.log('API call successful:', data.data.data[0]);
    } else {
      console.error('API call failed:', data);
    }
  })
  .catch(err => console.error('Error:', err));
```

Run your test:

```bash
node testConnection.js
```

If your setup is complete, you should see a response with quote details (including the amount and currencies).

## What Success Looks Like

You should receive a 200 OK response.

```json
API call successful: {
  id: 'RiDW88uCwemwZu2tRuLv',
  turnOffNotification: false,
  valueInUSD: '2',
  wallet: true,
  quote: {
    expiresAt: '7/31/2025, 6:00:00 PM UTC',
    expiresAtTimestamp: 1753984800000,
    rate: 1480.339745,
    receiver: '',
    sender: 'VUla2glefVPCtwzqpwm3mD95TOM2',
    originCurrency: 'USD',
    originCurrencyDebitAmount: 2,
    destinationCurrency: 'NGN',
    destinationCurrencyCreditAmount: 2960.67949,
    markups: {},
    fee: 1,
    fixedFees: null
  },
  email: 'user@mail.com',
  swapped: true,
  narration: 'multi-currency transfer',
  redeemData: {},
  chimoney: 2000,
  issuer: 'VUla2glefVPCtwzqpwm3mD95TOM2',
  issueID: 'VUla2glefVPCtwzqpwm3mD95TOM2_2_1753984331035',
  type: 'chimoney',
  initiatedBy: 'VUla2glefVPCtwzqpwm3mD95TOM2',
  integration: { appID: 'iq6vJJQYosEQNsjbXuQR', reference: '' },
  fee: 1,
  personalizedMessage: 'multi-currency transfer',
  chiRef: '01f41117-38ff-42c3-868d-bd9f8d6caeb6',
  issueDate: '2025-07-31T17:52:11.341Z',
  t_id: 235483111251582,
  meta: { payer: 'VUla2glefVPCtwzqpwm3mD95TOM2', outgoingPaymentID: {} },
  updatedDate: '2025-07-31T17:52:12.319Z',
  paymentDate: '2025-07-31T17:52:12.319Z',
  message: { mId: '', mType: 'email', status: 'sent' },
  redeemLink: 'https://sandbox.chimoney.io/redeem/?chi=01f41117-38ff-42c3-868d-bd9f8d6caeb6'
}
```

If you get an error, check your API key, base URL, and request body.

## Troubleshooting

| Error              | Cause                        | Fix                                      |
|--------------------|-----------------------------|-------------------------------------------|
| 401 Unauthorized   | Missing/invalid API key      | Double-check the `X-API-KEY` header       |
| 400 Bad Request    | Invalid body format          | Check that all required fields are present|
| Insufficient Funds | Not enough wallet balance    | Try a smaller amount or add test funds    |

For more help, visit the [Chimoney docs](https://chimoney.readme.io/reference/introduction) or contact their support.

## Helpful Links
- https://chimoney.readme.io/reference/post_v0-2-4-multicurrency-wallets-transfer