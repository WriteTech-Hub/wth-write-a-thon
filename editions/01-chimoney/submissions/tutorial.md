# Tutorial: Sending Funds to an Interledger Wallet with Chimoney

## Overview
This guide walks you through sending money to an interledger payment (ILP) pointer using Chimoney’s endpoint.

You’ll learn how to:
- Authenticate with the endpoint.
- Structure your POST request.
- Send a payout to an ILP wallet.
- Handle common responses and errors.

## Prerequisites
Before starting, make sure you have:
- A Chimoney developer account and API key (Sandbox or Production).
- A valid sender wallet ID (you can find this in your Chimoney multi-currency wallet).
- An ILP-compatible payment pointer (i.e., `lily$wallet.chimoney.io`).
- A tool to make HTTP requests (i.e., Postman, cURL, or a programming language like Python or Node.js).

Once you have everything you need, you can start working with our API. The steps below will help you get connected with the endpoint, so you can start sending money right away.

## Step 1: Set Up the Request
You will need to set up your request to the API. This request must contain the following components in order to return a response:
- Endpoint
- Required Headers
- Request Body

### Endpoint
Use the following endpoint when you want to connect to the Interledger Wallet Address Endpoint:

```
POST https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address
```

### Required Headers
You need the following headers in your API request:

```
accept: application/json
X-API-KEY: YOUR_API_KEY_HERE
Content-Type: application/json
```

### Request Body
You will need the following information in your request body:

- `subAccount`: An optional field to specify the sub-account for your wallet, if any.
- `turnOffNotification`: If true, disables any notifications from Chimoney (i.e., email). If false, Chimoney will send these notifications.
- `debitCurrency`: The debited wallet currency (i.e., USD, CAD).
- `interledgerWallets`: The payload for the Interledger Wallet Address. Contains the `interledgerWalletAddress`, `currency`, `amountToDeliver`, `narration`, and `collectionPaymentsIssueID` parameters.
  - `interledgerWalletAddress`: Interledger wallet address (or payment pointer) to settle the payment. Issued by Chimoney.
  - `currency`: The currency that will be delivered back to the user (i.e. USD, CAD).
  - `amountToDeliver`: The amount to deliver to the user.
  - `narration`: A description of the payment.
  - `collectionPaymentIssueID`: The issue ID for the initiated payout.

**Deprecated Parameters**:
- `amount`
- `valueinUSD`

#### Example Request Body
```json
{
  "subAccount": "1234567",
  "turnOffNotification": false,
  "debitCurrency": "CAD",
  "interledgerWallets": [
    {
      "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username",
      "currency": "USD",
      "amountToDeliver": 10,
      "amount": 200,
      "valueInUSD": 2500,
      "narration": "Interledger Salary Payment",
      "collectionPaymentIssueID": "1209992222"
    }
  ]
}
```

## Step 2: Send the Information to the Endpoint
You can use the following methods to contact the endpoint:
- cURL
- Python
- Node.js
- JavaScript
- Postman

### Example cURL Command
```bash
curl -X 'POST'   'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address'   -H 'accept: application/json'   -H 'X-API-KEY: YOUR_API_KEY_HERE'   -H 'Content-Type: application/json'   -d '{
  "subAccount": "1234567",
  "turnOffNotification": false,
  "debitCurrency": "CAD",
  "interledgerWallets": [
    {
      "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username",
      "currency": "USD",
      "amountToDeliver": 10,
      "amount": 200,
      "valueInUSD": 2500,
      "narration": "Interledger Salary Payment",
      "collectionPaymentIssueID": "1209992222"
    }
  ]
}'
```

### Example Python Command
```python
import requests

url = "https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address"
headers = {
    "accept": "application/json",
    "X-API-KEY": "YOUR_API_KEY_HERE",
    "Content-Type": "application/json"
}
payload = {
    "subAccount": "1234567",
    "turnOffNotification": False,
    "debitCurrency": "CAD",
    "interledgerWallets": [
        {
            "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username",
            "currency": "USD",
            "amountToDeliver": 10,
            "amount": 200,
            "valueInUSD": 2500,
            "narration": "Interledger Salary Payment",
            "collectionPaymentIssueID": "1209992222"
        }
    ]
}

response = requests.post(url, headers=headers, json=payload)
print("Status Code:", response.status_code)
print("Response:", response.json())
```

### Example Node.js Command
```javascript
const axios = require('axios');

const url = 'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address';

const headers = {
  'accept': 'application/json',
  'X-API-KEY': 'YOUR_API_KEY_HERE',
  'Content-Type': 'application/json'
};

const data = {
  subAccount: '1234567',
  turnOffNotification: false,
  debitCurrency: 'CAD',
  interledgerWallets: [
    {
      interledgerWalletAddress: 'https://ilp-sandbox.chimoney.com/username',
      currency: 'USD',
      amountToDeliver: 10,
      amount: 200,
      valueInUSD: 2500,
      narration: 'Interledger Salary Payment',
      collectionPaymentIssueID: '1209992222'
    }
  ]
};

axios.post(url, data, { headers })
  .then(response => {
    console.log('Status:', response.status);
    console.log('Data:', response.data);
  })
  .catch(error => {
    if (error.response) {
      console.error('Error Status:', error.response.status);
      console.error('Error Data:', error.response.data);
    } else {
      console.error('Error:', error.message);
    }
  });
```

### Example JavaScript Command
```javascript
const url = 'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address';

const headers = {
  'accept': 'application/json',
  'X-API-KEY': 'YOUR_API_KEY_HERE',
  'Content-Type': 'application/json'
};

const data = {
  subAccount: '1234567',
  turnOffNotification: false,
  debitCurrency: 'CAD',
  interledgerWallets: [
    {
      interledgerWalletAddress: 'https://ilp-sandbox.chimoney.com/username',
      currency: 'USD',
      amountToDeliver: 10,
      amount: 200,
      valueInUSD: 2500,
      narration: 'Interledger Salary Payment',
      collectionPaymentIssueID: '1209992222'
    }
  ]
};

fetch(url, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(data)
})
.then(response => {
  if (!response.ok) {
    throw new Error(`HTTP error! Status: ${response.status}`);
  }
  return response.json();
})
.then(result => {
  console.log('Success:', result);
})
.catch(error => {
  console.error('Error:', error);
});
```

### Example Postman Request
1. Open Postman and create a new **POST** request.
2. Enter the **URL**: `https://api.chimoney.io/v0.2.4/payouts/interledger-address`
3. Set the **Headers**:
   - `accept`: `application/json`
   - `X-API-KEY`: `sk_test_ABC123`
   - `Content-Type`: `application/json`
4. In Body tab, select `raw`, then `JSON` and paste this body:

```json
{
  "subAccount": "1234567",
  "turnOffNotification": false,
  "debitCurrency": "CAD",
  "interledgerWallets": [
    {
      "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username",
      "currency": "USD",
      "amountToDeliver": 10,
      "amount": 200,
      "valueInUSD": 2500,
      "narration": "Interledger Salary Payment",
      "collectionPaymentIssueID": "1209992222"
    }
  ]
}
```

## Step 3: Receive a Response from the API

### Sample Successful Response
```json
{
  "status": "success",
  "message": "Payout to Chimoney wallets completed successfully.",
  "data": {
    "paymentLink": "undefined/pay/?issueID=XQTUvRBntIU3AHgQ3jFuN9ZcHIC3_2_1661118346647",
    "data": [{}],
    "chimoneys": [{}],
    "error": "None",
    "payouts": {
      "additionalProperties": {},
      "issueID": "eaabe2eb-e774-4f81-8ba9-ede2ea2d0058_11_1726579317119"
    }
  }
}
```

## Notes
- Replace `sk_test_ABC123` with your actual API key.
- Ensure `interledgerWalletAddress` represents a Chimoney wallet.
- A 200 status code with `status: "success"` confirms a successful payout.

## Error Handling

| Code | Status | Message |
|------|--------|---------|
| 200  | Success | Payout to Chimoney wallets completed successfully. |
| 400  | Error   | The request parameters are invalid. |
| 401  | Error   | API key is not defined. Generate a new one from the developer portal. |
| 403  | Error   | API Access not enabled for account Test. Email support@chimoney.io. |
| 500  | Error   | An internal server error has occurred. |

> **TIP**: Always inspect the `status` and `issueID` in the response to confirm payment processed successfully.

## Step 4: Wrap Up
You’ve successfully issued a payout to an Interledger wallet using Chimoney’s Interledger Wallet Address API.

### What’s Next?
- Use `POST /v0.2.4/payouts/status` with the `issueID` to verify final delivery.
- Log each transaction for reporting and reconciliation.
- Explore Chimoney’s off-ramp alternatives (i.e., airtime, mobile money, or gift cards).

## Summary
This endpoint kicks off the ILP payment process. It streamlines complex cross-border transactions by telling Chimoney:
- Who to pay (ILP wallet address)
- How much to send
- Currency type
- Which Chimoney wallet disperses funds

Once ILP has this information, it routes the payment to its destination.

>**NOTE:** If you want to learn more, please consult our [Swagger documentation](https://api.chimoney.io/v0.2.4/api-docs/#/Payouts/post_v0_2_4_payouts_interledger_wallet_address).