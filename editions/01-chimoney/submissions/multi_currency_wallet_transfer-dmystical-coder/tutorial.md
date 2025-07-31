
# Tutorial: Using the Chimoney Multi-Currency Wallet Transfer API

This tutorial will walk you through making a multi-currency wallet transfer using Chimoney’s API. You’ll see how to request a quote and then execute a transfer, with code samples and explanations for each step.

---

## 1. Prerequisites

Before you start, make sure you have:
- A Chimoney Sandbox API key
- Node.js and npm installed
- The setup steps from `setup.md` completed

---

## 2. Request a Transfer Quote

First, you need to get a quote for your transfer. This tells you how much the recipient will get after conversion and fees.

**Sample code (`getQuote.js`):**


```js
require('dotenv').config();

const url = `${process.env.CHIMONEY_BASE_URL}/v0.2.4/multicurrency-wallets/transfer/quote`;
const headers = {
  'Content-Type': 'application/json',
  'X-API-KEY': process.env.CHIMONEY_API_KEY,
};

const body = {
  amountToSend: '50.00',
  originCurrency: 'USD',
  destinationCurrency: 'NGN',
  email: 'recipient@example.com',
};

fetch(url, {
  method: 'POST',
  headers,
  body: JSON.stringify(body),
})
  .then(async res => {
    const data = await res.json();
    if (!res.ok) {
      // Handle HTTP errors
      console.error('Error:', res.status, data);
      return;
    }
    if (data.status !== 'success') {
      // Handle API-level errors
      console.error('API error:', data);
      return;
    }
    console.log('Quote response:', data);
  })
  .catch(err => console.error('Network error:', err));
```

**Expected response:**
```json
Quote response: {
  status: 'success',
  message: 'Quote generated successfully',
  data: {
    expiresAt: '7/31/2025, 7:00:00 PM UTC',
    expiresAtTimestamp: 1753988400000,
    rate: 1480.340184,
    receiver: '',
    sender: 'VUla2glefVPCtwzqpwm3mD95TOM2',
    originCurrency: 'USD',
    originCurrencyDebitAmount: 50,
    destinationCurrency: 'NGN',
    destinationCurrencyCreditAmount: 74017.0092,
    markups: {},
    fee: 5,
    fixedFees: null
  }
}
```

---

## 3. Execute the Transfer

Once you have the quote and are ready to send, make the transfer call.

**Sample code (`transfer.js`):**


```js
require('dotenv').config();

const url = `${process.env.CHIMONEY_BASE_URL}/v0.2.4/multicurrency-wallets/transfer`;
const headers = {
  'Content-Type': 'application/json',
  'X-API-KEY': process.env.CHIMONEY_API_KEY,
};

const body = {
  amountToSend: '50.00',
  originCurrency: 'USD',
  destinationCurrency: 'NGN',
  email: 'recipient@example.com',
  narration: 'Test transfer',
};

fetch(url, {
  method: 'POST',
  headers,
  body: JSON.stringify(body),
})
  .then(async res => {
    const data = await res.json();
    if (!res.ok) {
      // Handle HTTP errors
      console.error('Error:', res.status, data);
      return;
    }
    if (data.status !== 'success') {
      // Handle API-level errors
      console.error('API error:', data);
      return;
    }
    console.log('Transfer response:', data);
  })
  .catch(err => console.error('Network error:', err));
```

**Expected response:**
```json
Transfer response: {
  status: 'success',
  message: 'Payout to Chimoney wallets completed successfully.',
  data: {
    paymentLink: 'https://sandbox.chimoney.io/pay/?issueID=VUla2glefVPCtwzqpwm3mD95TOM2_50_1753985771489',
    data: [ [Object], [Object] ],
    chimoneys: [ [Object], [Object] ],
    error: 'None',
    payouts: {
      '0': [Object],
      '1': [Object],
      issueID: 'VUla2glefVPCtwzqpwm3mD95TOM2_50_1753985771489'
    }
  }
}
```

---


## 4. Notes, Tips, and Error Handling

- Always check the HTTP status and the `status` field in the response.
- Handle errors gracefully in your code (see above for examples).
- Use the quote endpoint before every transfer to get up-to-date rates and fees.
- For production, switch to your live API key and endpoint.

**Common errors:**

| Error code / message      | What it means                        | How to fix                        |
|--------------------------|--------------------------------------|-----------------------------------|
| 401 Unauthorized         | API key missing or invalid           | Check your `X-API-KEY` header     |
| 400 Bad Request          | Invalid request body or parameters   | Check your request fields         |
| Insufficient Funds       | Not enough balance in your wallet    | Try a smaller amount or add funds |
| Network error            | Could not reach the API              | Check your internet connection    |

---

You’ve now seen how to request a quote and make a multi-currency wallet transfer using Chimoney’s API. For more information, see the [Chimoney API docs](https://chimoney.readme.io/reference/introduction).