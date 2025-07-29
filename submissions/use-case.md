# 💸 Use Case: Paying Freelancers via Chimoney’s Interledger Wallet Address

**Goal:** Seamlessly send USD payouts to freelancers or remote workers using Chimoney’s `/payouts/interledger-wallet-address` API.

---

## 📘 Scenario

You run a global platform that pays freelancers for their services. Rather than managing complex banking setups for each country, you want to send fast, secure payments directly to Interledger-enabled wallets using Chimoney.

---

## 🚀 Why This Use Case?

- 🌎 Pay workers across borders without worrying about banking differences
- ⚡ Instant value transfer via Interledger Protocol (ILP)
- 💵 Support multiple currencies like USD
- 📱 Easy integration into your existing backend or platform

---

## 🔄 Flow Overview

### 🪜 Flow - Step-by-Step

1. ✅ **Task Completed:** A freelancer completes a gig on your platform.
2. 🧮 **Payout Calculated:** Your backend calculates how much to send.
3. 🛰 **API Call Sent:** Your app sends a `POST` request to the `/payouts/interledger-wallet-address` endpoint with:
   - Their ILP address
   - Amount in USD
   - Narration
4. 💸 **Chimoney Handles Transfer:** Chimoney routes and completes the payout to the ILP wallet.
5. 📬 **Notification (Optional):** You can trigger an email or in-app notification to the freelancer.

---

## 🧰 Prerequisites

- ✅ Chimoney Developer Account ([Create one](https://sandbox.chimoney.io/))
- 🔑 API Key (Grab from dashboard)
- 🌐 Valid Interledger Wallet Address (`https://ilp-sandbox.chimoney.com/<your-id>`)
- 🛠 Postman, Curl, or your preferred HTTP client

---

## 📤 Sending the Payout

Here’s a real request example using the `/payouts/interledger-wallet-address` endpoint:

```json
POST /v0.2.4/payouts/interledger-wallet-address
Host: api-v2-sandbox.chimoney.io

Headers:
  Authorization: Bearer YOUR_API_KEY

Body:
{
  "turnOffNotification": false,
  "debitCurrency": "USD",
  "interledgerWallets": [
    {
      "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/your-id",
      "currency": "USD",
      "amountToDeliver": 10,
      "narration": "July freelance payout",
      "collectionPaymentIssueID": "job-001"
    }
  ]
}
```

✅ Success Response

```json
{
  "status": "success",
  "message": "Chimoney payout to Interledger wallet completed successfully",
  "data": {
    "paymentLink": "https://sandbox.chimoney.io/pay/?issueID=...",
    ...
  }
}
```

---

## ❗ Common Errors

### 🛑 CAD is not enabled

```json
{
  "status": "error",
  "message": "CAD is not enabled for the interledger Wallet Address..."
}
```

**Fix:** Use "USD" for both currency and debitCurrency

### 🛑 Invalid Chimoney User ID
```json
{
  "error": "sender must be a valid Chimoney user ID"
}
```
**Fix:** Remove or correct the subAccount field.

---

## 💡 Wrap-Up
With just a few lines of code, you can automate freelancer payouts globally. Chimoney + Interledger makes cross-border payments smooth, developer-friendly, and scalable.

---

## Next Step:
🎯 Integrate this API flow into your backend and trigger payouts after job completion!