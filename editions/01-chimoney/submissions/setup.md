# Setup Guide: Environment and Authentication for Chimoney ILP Payouts

This guide will walk you through creating your Chimoney developer environment and verifying authentication, so you can start making payouts to Interledger-compatible wallets.

---

## Step 1: Create a Chimoney Developer Account

To make your Chimoney Developer Account, follow these steps:

1. Visit the [Chimoney Sandbox](https://sandbox.chimoney.io) or [Chimoney Dash](https://dash.chimoney.io/auth/signin).
2. Sign up for an account and verify your email address.
3. Navigate to **Developers and API > API Credentials**.
4. Click on **Add New App**.
5. Locate your **API Key** beneath the App Name field.
6. Copy the API Key.

> **TIP**: If this is your first time using this endpoint, we recommend using the [Chimoney Sandbox](https://sandbox.chimoney.io).


---

## Step 2: Install Tools

Choose a client to test and interact with the Chimoney endpoint:

- **cURL**
- **Python**
- **Node.js**
- **JavaScript**
- **Postman**

### cURL  
Recommended for quick testing. Available by default on most Unix systems.

### Python  
Recommended for developers working with automation or backend systems.

You will need to install Python and its dependencies:

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install requests python-dotenv
```

### Node.js  
Recommended for script-based testing.

If using Node.js, you will need the following package:

```bash
npm init -y
npm install axios
```

### JavaScript  
Recommended for front-end or modern full-stack environments. Works in browsers or with Node.js 18+ using native fetch.

Modern browsers and up-to-date Node.js environments support JavaScript out of the box.

### Postman  
Recommended for GUI-based API interaction. Download and install Postman, then set up your environment and add your `X-API-KEY` to the headers.

---

## Step 3: Verify Authentication

Test your API key by sending a basic request. Example authentication requests follow, using the preceding tools.

### cURL Example

Enter your `API key` and `Content-Type`. You can leave the request empty, as shown below.

```bash
curl -X 'POST' \
  'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address' \
  -H 'accept: application/json' \
  -H 'X-API-KEY: YOUR_API_KEY_HERE' \
  -H 'Content-Type: application/json' \
  -d '{}'
```

You should receive a **400 error code** when querying the API using this response. This expected response confirms your API key works.

---

### Python Example

You can verify your API key works by sending test data with Python.

```python
import os
import requests
from dotenv import load_dotenv

load_dotenv()
key = os.getenv("CHIMONEY_API_KEY")

res = requests.post(
  "https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address",
  headers={
    "accept": "application/json",
    "X-API-KEY": key,
    "Content-Type": "application/json"
  },
  json={
    "subAccount": "1234567",
    "turnOffNotification": True,
    "debitCurrency": "CAD",
    "interledgerWallets": []
  }
)

print(res.status_code)
print(res.json())
```

You should receive a **400 error code**. This expected response confirms your API key works.

---

### Node.js Example

Once you have installed the Axios package, enter the following request to validate your API key.

```js
const axios = require('axios');

axios.post(
  'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address',
  {},
  { headers: { 'X-API-KEY': process.env.CHIMONEY_API_KEY } }
)
.then(res => console.log('Success:', res.data))
.catch(err => console.error('Error:', err.response.data));
```

You should receive a **400 error code**. This expected response confirms your API key works.

---

### JavaScript (Fetch) Example

Perform a fetch request using test data to see if your API key works.

```js
fetch('https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address', {
  method: 'POST',
  headers: {
    'accept': 'application/json',
    'X-API-KEY': 'your_api_key_here',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    subAccount: '1234567',
    turnOffNotification: true,
    debitCurrency: 'CAD',
    interledgerWallets: []
  })
})
  .then(res => {
    if (res.status === 401) throw new Error('Invalid API Key');
    return res.json();
  })
  .then(data => console.log('Key is valid:', data))
  .catch(err => console.error(err));
```

You should receive a **400 error code**. This expected response confirms your API key works.

---

### Postman Setup

Use the following information to set up your Postman environment:

1. For the **Method**, select `POST`.
2. In the **URL** field, enter:  
   `https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address`
3. In the **Headers** tab, enter the following:
   - `Accept: application/json`
   - `Content-Type: application/json`
   - `X-API-KEY: YOUR_API_KEY_HERE`
4. In the **Body** tab, insert the following JSON:

```json
{
  "subAccount": "1234567",
  "turnOffNotification": true,
  "debitCurrency": "CAD",
  "interledgerWallets": []
}
```

---

## Step 4: Set Environment Variables

As a security best practice, store your API keys as environment variables.

Example to export your API key in Unix-like shells:

```bash
export CHIMONEY_API_KEY=sk_test_ABC123456
```

When referencing your environment variables in code, use `process.env.YOUR_API_KEY` or your language’s equivalent.

---

## Step 5: Ready to Payout

Once you’ve confirmed your authentication and securely stored your API keys, perform a request to the API.

We recommend the following to get the most out of the API:

1. Proceed to the **Tutorial** to perform a full Interledger payout.
2. Ensure `interledgerWalletAddress` references a valid, funded multi-currency Chimoney wallet.

---

## Next Steps

To prevent interruptions or payment issues, consider these best practices:

- Move from **Sandbox** to **Production** and update your API key to the Production (live) version.
- Add retry logic for transient errors (such as 500 response codes or timeouts).
- Securely store and rotate credentials as a part of your deployment lifecycle.

---
