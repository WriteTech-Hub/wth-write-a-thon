# How to Use Chimoney’s Interledger Wallet Address Payout Endpoint
> Endpoint: `/payouts/interledger-wallet-address`

In this tutorial, we’ll walk through how to integrate Chimoney's API to send payouts to an Interledger Wallet Address. This will allow you to make cross-border payments using a digital wallet. By the end, you'll know how to test the endpoint, troubleshoot common errors, and integrate it into your app.

---

## 🧰 Prerequisites

Before diving in, make sure you have the following:

- 🔐 **Chimoney Developer Account**  
  Create one here: [chimoney.io/developers-api](https://chimoney.io/developers-api)
  
 Your sandbox account comes preloaded with $1000 (10,000 Chimoney) for testing purposes.

- 🔑 **API Key**  
  Grab it from your dashboard. Need help? [Watch this video](https://www.loom.com/share/436303eb69c44f0d9757ea0c655bed89?sid=b6a0f661-721c-4731-9873-ae6f2d25780)

- 💳 **Interledger Wallet Address**  
  Found on your dashboard after login. It will look something like:  
  `https://ilp-sandbox.chimoney.com/your-unique-id`

- 🧪 **API Testing Tools (Optional)**  
  You can use the following tools to test the API:
  - **Postman:** Import the collection [here]([link-to-collection](https://documenter.getpostman.com/view/26097715/2sA3kXCzD2#c311f506-2938-440b-abe9-dc232530a84f))
  - **cURL:** Example command:
    ```bash
    curl -X POST https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address -H "Authorization: Bearer <your_api_key>" -d '{"amount": "100", "currency": "USD", ...}'
    ```
  - Your favorite HTTP client (e.g. Axios, Fetch)

- 🧠 **Basic Knowledge of REST APIs**  
  You will be working with `POST` requests and JSON payloads.

---

## 🚀 Test the Endpoint in the API Explorer

Let us make a test call using Chimoney’s Swagger (API explorer):

1. Go to [Chimoney’s API Explorer](https://api.chimoney.io/v0.2.4/api-docs/#/Payouts/post_v0_2_4_payouts_interledger_wallet_address)
2. Switch to `sandbox mode` (top-left dropdown)

![Sandbox dropdown](./images/server-dropdown-sandbox.png)

3. Click **Authorize** and paste your API key
This step authenticates your requests, letting the API know that you have permission to interact with the endpoint.

5. [Click here](https://api.chimoney.io/v0.2.4/api-docs/#/Payouts/post_v0_2_4_payouts_interledger_wallet_address) to go directly to the `POST /payouts/interledger-wallet-address` section in the API Explorer.
6. Replace the `interledgerWalletAddress` field with your own ILP address
7. Hit **Execute**!

Now, your first test payout should go through if everything is set up correctly!

---

## 🛠 Common Errors and How to Fix Them

### ❌ Error: `CAD is not enabled for the interledger Wallet Address`

```json
{
  "status": "error",
  "message": "CAD is not enabled for the interledger Wallet Address (Payment Pointer), https://ilp-sandbox.chimoney.com/********"
}
```

### 💡 Why it happens:
Some wallets are configured to only support certain currencies. For instance, if your ILP wallet is set up for USD, it won’t process requests in CAD due to system limitations or regulatory reasons.

### ✅ Fix: Change both currency fields to USD:
```json
"currency": "USD",
"debitCurrency": "USD"
```

**🕵️‍♀️ Pro tip:** Open your ILP wallet address in a browser to confirm supported assets.


![ILP assets](./images/ilp-browser.png)

### ❌ Error: `sender must be a valid Chimoney user ID`

### 💡 Why it happens:
You have either included an invalid subAccount or do not need one.

### ✅ Fix:

- Don’t have a subAccount? Just remove the field.

- Or go to your dashboard → Transactions tab → copy your user ID.

---

## Other API Responses
If everything checks out, you should see a success response like this:

### ✅ 200 OK – Success
```json
{
  "status": "success",
  "message": "Chimoney payout to Interledger wallet completed successfully",
  "data": {
    "paymentLink": "https://sandbox.chimoney.io/pay/?issueID=...",
    "chimoneys": [
      {
        "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/your-id",
        "amount": 200,
        "currency": "USD",
        "chiRef": "some-unique-id"
      }
    ],
    "redeemLink": "https://sandbox.chimoney.io/redeem/?chi=..."
  }
}
```
Use the `paymentLink` or `redeemLink` to simulate or test payouts end-to-end.

In addition to successful payouts, you may encounter a variety of other responses from the API. Below are some examples and how to troubleshoot them.
### ⚠️ 400 / 401 / 403 – Invalid or Unauthorized Request

```json
{
  "status": "error",
  "error": "sender must be a valid Chimoney user ID"
}
```

Or:

```json
{
  "status": "error",
  "message": "Unauthorized – API key missing or invalid"
}
```

Refer to the **Common Errors** section above to resolve these.

### 💥 500 – Internal Server Error
```json
{
  "status": "error",
  "type": "InternalError",
  "code": "SERVER_ERROR",
  "message": "Something went wrong"
}
```
If this happens, double-check your payload format or retry later.

---

## Wrapping Up
That is it! At this point, you should be able to:

- Create a Chimoney developer account and generate an API key

- Authenticate using your API key

- Locate and use your ILP wallet address

- Make a test payout request via the API Explorer

- Troubleshoot common errors like currency mismatch or sender ID issues

### 🎯 Next Step: Integrate the API in your project

Now that you are all set up, let us write some code and make payouts from your app using this [tutorial guide](./tutorial.md).

**Happy building!** 🙌
