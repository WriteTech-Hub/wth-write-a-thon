# Implementing Multi-Currency Wallet Transfers with Chimoney API 

Chimoney offers an array of methods to make cross-border payments across the world easy and seamless. With seven continents, 8.2 billion people, and 180 currencies in the world, you cannot control where people you want to send money to will be located. 

In this tutorial, you will learn how to initiate money transfers to other people from a Chimoney multicurrency wallet, which typically supports USD,CAD and NGN currencies. 

What this means is that by the end of this tutorial, you will be able to integrate Chimoney’s multicurrency wallet transfer endpoint into your projects to transfer money from a multicurrency wallet on Chimoney to anyone, anywhere in the world in seconds.

## Pre-requisites

You will be working in the Chimoney sandbox environment, which will give you access to a test account to work with the API outside the live production environment.

Before we begin, let’s ensure that you have these set up for a successful integration:

- A Chimoney developer account: If you don't already have an account, you can sign up [here](https://sandbox.chimoney.io/).
- A new application on your Chimoney developer dashboard: This is to enable you to generate and have access to your API Key.
- An API Key: This is essential for authenticating all your requests, and can be generated from your developer dashboard once you create an application. Learn how to generate and copy an API Key for your app [here](https://chimoney.readme.io/reference/sandbox-environment).
- Node.js installed: A development environment with Node.js installed (LTS version recommended) for running the JavaScript code samples.
- An IDE most preferably VSCode.

Once you have these setup, you are well on your way to successfully consuming the multiwallet transfer endpoint. 

---

## API Request Details

- **Method**: POST  
- **Base URL**: `https://api-v2-sandbox.chimoney.io/v0.2.4/`  
- **Endpoint**: `multicurrency-wallets/transfer`  

---

## Step 1: Install axios

We will be using the axios library to make our request to the multiwallet transfer endpoint. In your terminal, paste the command below to install the axios library.

```bash
npm install axios
```

---

## Step 2: Configure your Authentication

From your Chimoney developer dashboard, copy your API Keys from the details of the app you created. Paste the API KEY in your request header.

```javascript
// API key and headers
const headers = {
  'Content-Type': 'application/json',
  Accept: 'application/json',
  'X-API-KEY': 'YOUR API KEY'
};
```

---

## Step 3: Understanding the Request Body

For a successful request, this endpoint should contain three required body parameters, at least one recipient identifier and the rest being optional and only used depending on what is needed to make the transfer. 

### Required Parameters

The absence of either of these parameters will result in errors when you send your request;

- `amountToSend`: State how much money you want to send to the recipient.
- `originCurrency`: State the ISO currency code of the amount to be sent. Note that as a test developer account you only have access to USD wallet.
- `destinationCurrency`: The currency you want the recipient to receive the money in. Chimoney supports 130+ currencies, so you can choose one e.g NGN, CAD, KES etc.

### Recipient Identification Parameters

To specify the recipient of the transfer, you must provide exactly one of the following identifiers:

- `receiver` (string): The unique identifier of the recipient's existing Chimoney Multi-Currency Wallet. Use this only if the recipient has a multicurrency wallet with Chimoney.
- `email` (string): The email address of the recipient. The recipient will be receive an email to redeem the money sent.
- `PhoneNumber` (string): The phone number of the recipient, in E.164 format (e.g., +2348012345678). Similar to email, the recipient will be notified via Whatsapp to redeem the money.

### Optional Parameters

- `Sender`: The ID of the multicurrency account from which the funds will be sent. If this field is left empty, the money will be transferred from your parent account instead, which is the account you initially created with Chimoney. 
- `Subaccount`: This allows you to send money from your subaccount instead of your multicurrency wallet or parent account. 
- `Narration`: A short note or message that will be sent to the recipient with the transaction notification, so they understand the purpose of the transfer.
- `TurnOffNotification`: This requires a boolean value (true/false). If set to true, Chimoney will not send any emails or notifications about the transaction. 
- `sendViaInterledger`: If true, the transaction will be sent through the (ILP) Interledger Protocol. This requires the sender and receiver to have valid interledger wallet addresses. Learn how to issue an interledger wallet here.

Now that you understand the request body parameters, let us make a request to send $50 to a recipient’s email address.

---

## Step 4: Structure and Send Your Request

Create a JavaScript file (e.g., `server.js`), copy the code below and paste it in your JavaScript file, and then run the following command in your terminal `node server.[your javascript file name]`.

```javascript
const axios = require('axios');

// Request body -- the transfer details

const transferDetials = {
    amountToSend: '50',
  originCurrency: 'USD',
  destinationCurrency: 'USD',
  email: 'queendolineak@gmail.com'
}
// API key and headers
const headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'X-API-KEY': 'YOUR API KEY' 
};

// POST request 
axios.post(
  'https://api-v2-sandbox.chimoney.io/v0.2.4/multicurrency-wallets/transfer',
  transferDetails,
  { headers }
)
.then(response => {
  console.log('Transfer successful:', response.data);
})
.catch(error => {
  console.error('Transfer failed:', error.response?.data || error.message);
});
```

---

In the code above, we have done the following:

- Created `transferDetails` object to hold necessary details needed for this transfer in the request body. In this case, we are sending $50 to the recipient with the email address of queendolineak@gmail.com.

- The API KEY is specified in the authorization header to ensure that this request is successful and for security purposes.

> **Note:** When building a production application, Never hardcode API keys directly into your client-side code or commit them to version control to prevent unauthorized access and potential misuse.

- Using the `post` method from axios to send the POST request, with the full API endpoint URL as the first argument, the request body object as the second argument, and the HTTP header as the third argument.

After running this code in your editor, you should get a successful `200 OK` response as shown below, from the server.

```json
{
  "status": "success",
  "message": "Payout to Chimoney wallets completed successfully.",
  "data": {
    "paymentLink": "https://sandbox.chimoney.io/pay/?issueID=...",
    "chimoneys": [
      {
        "id": "6jLD9Ynyd6qGLV4zHL9U",
        "valueInUSD": "10",
        "email": "queendolineakpan11@gmail.com",
        "destinationCurrency": "NGN",
        "redeemLink": "https://sandbox.chimoney.io/redeem/?chi=..."
      }
    ]
  }
}
```

> **Note**: For a full response object and response schema refer to the [Chimoney API reference documentation](https://chimoney.readme.io/reference/post_v0-2-4-multicurrency-wallets-transfer). 

The recipient also receives an instant notification in their email box as shown below, alerting them of the funds available for them to redeem in any of the available forms they want as offered by Chimoney.

![Recipient's Email Notification](/submissions/images/email_notification.png)
---

## Common Errors and How to Fix Them

API calls are not always guaranteed to be successful, if your request fails you may have hit any of the errors below, and let’s go over how you can fix them.

| HTTP CODE | Error             | Resolution                                                                 |
|-----------|------------------|---------------------------------------------------------------------------|
| 400       | Invalid Request   | Check that you have included the required fields and recipient Identity.  |
| 401       | Invalid API Key   | Make sure the API Key is correct, or generate a new one from the portal.  |
| 500       | Server Error      | Retry the request. If it persists, contact Chimoney support.              |

---

## Conclusion

And that is a wrap, together we have been able to consume the multicurrency wallet transfer endpoint from Chimoney, which is very crucial in this day and age for transferring money in different currencies across borders. 

Visit [the official Chimoney developer documentation](https://chimoney.io/developers-api/) to explore all the services and other endpoints provided by Chimoney that continue to make cross-border remittances a breeze in our modern world.
