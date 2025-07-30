# Chimoney Create Interledger Wallet Address Tutorial

In this tutorial, you’ll learn how to use Chimoney’s create interledger wallet address endpoint to send money to an Interledger wallet address.

This guide will walk you through the process step-by-step, from setting up your account to making your first API call.


## Overview
Chimoney’s Interledger Wallet Address is a digital account associated with a user that enables seamless, secure, and instant cross-border transactions using the Interledger Protocol (ILP). This wallet address acts like a virtual bank account on the blockchain or digital asset network, allowing users to send and receive international payments efficiently.

### Purpose of the Tutorial
This tutorial is designed to help you set up your Chimoney account, obtain your API key, and make your first API call to create an Interledger wallet address. 


## Tutorial Steps
This section will walk you through the step by step process to create an Interledger wallet address using Chimoney’s API.

### Step 1: Create a Sandbox Account & Get Your API Key
![Chimoney API Key Setup](./Create-Interledger-wallet-address-Motunrayo/assets/api-setup.gif)

1. Go to the [sandbox.chimoney.io](https://sandbox.chimoney.io) and sign up for a new account.
2. Once the account is created, log in to the Chimoney dashboard.
3. Navigate to the API section and generate a new API key. Make sure to save this key securely, as it’s needed to authenticate the API requests.
4. There is need `Team ID`, which is used as the `userId` in API requests.
5. Ensure a unique Interledger protocol `ilpUsername` (e.g., `jane.doe`) that will be used in the API requests.



### Step 2: Make Your First API Call

![Chimoney API Testing Setup](./Create-Interledger-wallet-address-Motunrayo/assets/postman-setup.gif)

_Remember to create a workspace to test your API calls in [Postman](https://postman.co/workspace/create)_



### 1. Set Up Authentication in Postman
1. Open Postman and create a new request.
2. In the request settings, go to the "Authorization" tab.
3. Select "API-KEY" as the type, X-API-KEY as the key and enter your API key in the "value" field.

### 2. Configure Request Body
1. Set the request method to **POST**.
2. Enter the following URL: `https://api-v2-sandbox.chimoney.io/v0.2.4/accounts/issue-wallet-address`
3. In the "Body" tab, select "raw" and choose "JSON" as the format.
4. Enter the following JSON payload:
```json
{
  "userID": "user123",
  "ilpUsername": "chimoney_user123"
}
```
Replace `userID` with the Team ID of the user and `ilpUsername` with the desired Interledger username.



### 3. Send & Verify
1. Click the "Send" button to make the API call.
2. If the request is successful with a 200 OK status, you will receive a response containing the new Interledger wallet address.

### 4. Common Errors & Fixes

- **Error 400: Bad Request**: This error indicates that the request payload is malformed. Ensure that you are sending a valid JSON payload with the required fields.
- **Error 401: Unauthorized**: This error occurs when the API key is missing or invalid. Double-check your API key and ensure it is included in the request headers.
- **Error 500: Internal Server Error**: This error is returned when there is an issue with the Chimoney API. If you encounter this error, try again later or contact Chimoney support.

### 5. Troubleshooting Tips
- Ensure that you are using the correct API endpoint and HTTP method.
- Double-check your request headers and payload for any errors.
- If you continue to experience issues, consult the [Chimoney API documentation](https://chimoney.readme.io/reference/post_v0-2-4-accounts-issue-wallet-address) or reach out to their support team for assistance.


