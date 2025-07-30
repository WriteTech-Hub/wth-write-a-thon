# How to Make Payouts to Interledger Wallets Using Chimoney’s API

In this tutorial, you will learn how to integrate Chimoney’s `/payouts/interledger-wallet-address` endpoint into your existing project and send live payouts using your wallet-enabled Chimoney account.

---

## 🔧 What You’ll Build

- Configure your environment
- Build a payout service with Chimoney’s API
- Handle real-world errors (like currency mismatches)
- Test your first ILP payout

---

## 🧰 Prerequisites

Before you start, make sure you have completed the [Setup Guide](./setup.md). You should already have:
- A Chimoney developer account
- Your API key
- Your ILP wallet address
- Tested the `payouts to interledger wallet address` endpoint

Additionally;
- Basic knowledge on RESTful APIs
- Node.js and npm/yarn installed
- Axios for HTTP requests

With that out of the way, let us dive in.

---

## 🗂 Project Structure

Here’s what your folder might look like:

```bash
chimoney-payouts/
├── .env
├── src/
│   ├── config.js
│   ├── services/
│   │   └── chimoneyService.js
│   └── index.js
├── package.json
└── README.md
```

## 🗂 Step 1: 🔧 Configuration & Setup

### Install Dependencies

```bash
mkdir chimoney-payouts && cd chimoney-payouts
npm init -y
npm install axios dotenv
```

### Environment Variables

Create `.env`:

```js
CHIMONEY_API_KEY=your_api_key_here
CHIMONEY_ENV=sandbox # or "production"

ILP_WALLET_ADDRESS=https://ilp-sandbox.chimoney.com/your-id
```

---

### Configuration Module (`config.js`)

```js
import dotenv from "dotenv";
dotenv.config();

export const config = {
  apiKey: process.env.CHIMONEY_API_KEY,
  baseUrl:
    process.env.CHIMONEY_ENV === "production"
      ? "https://api-v2.chimoney.io/v0.2.4"
      : "https://api-v2-sandbox.chimoney.io/v0.2.4",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json",
    "X-API-KEY": process.env.CHIMONEY_API_KEY,
  },
};
```

---

## 🧾 Step 2: Build the Chimoney Interledger Service
### Create `chimoneyService.js`:

```js
import axios from "axios";
import { config } from "./config.js";

class ChimoneyService {
  constructor() {
    this.client = axios.create({
      baseURL: config.baseUrl,
      headers: config.headers,
    });
  }

  async sendInterledgerPayout({ walletAddress, amount, narration, collectionPaymentIssueID = "" }) {
    try {
      const payload = {
        turnOffNotification: false,
        debitCurrency: "USD",
        interledgerWallets: [
          {
            interledgerWalletAddress: walletAddress,
            currency: "USD",
            amountToDeliver: amount,
            narration,
            collectionPaymentIssueID, // optional but great for tracking payouts
          },
        ],
      };

      const res = await this.client.post("/payouts/interledger-wallet-address", payload);
      return res.data;
    } catch (err) {
      if (err.response) {
        console.error("Chimoney API error:", err.response.data);
        throw err.response.data;
      }
      throw err;
    }
  }
}

export default ChimoneyService;
```

---

## ⚙️ Step 3: Usage Example (`index.js`)

```js
import ChimoneyService from "./chimoneyService.js";
import dotenv from "dotenv";
dotenv.config();

(async () => {
  const service = new ChimoneyService();

  const walletAddress = process.env.ILP_WALLET_ADDRESS; // e.g., https://ilp-sandbox.chimoney.com/your-id
  const amount = 10; // USD
  const narration = "Freelance payout for job #123";
  const collectionPaymentIssueID = "job-123-issue";

  try {
    const result = await service.sendInterledgerPayout({
      walletAddress,
      amount,
      narration,
      collectionPaymentIssueID,
    });
    console.log("✅ Payout Success:", result);
  } catch (error) {
    const res = error.response;
    console.error("❌ Payout Failed:", {
        status: res?.status,
        message: res?.data?.message || res?.data?.error || error.message
    });
    // handle error messages, e.g.:
    // if errorData.message includes "CAD is not enabled"
    // if errorData.error includes "valid Chimoney user ID"
  }
})();
```

### 🧪 Notes & Error Handling
#### Response Object Includes:
- status, message
- data.paymentLink / data.redeemLink
- data.chimoneys or data.payouts with chiRef, issueID, etc.


### ⚠️ Common Errors
Here are a couple of issues you might hit (and how to fix them):

#### 1. ❌ "CAD is not enabled"
**Fix:** Change **currency** to "USD"

#### 2. ❌ "sender must be a valid Chimoney user ID"
**Fix:** Invalid **subAccount**, (remove the field if unused, or request for one through support).


**Wrap error handler to inspect `errorData.message` or `.error` and act accordingly.**

---

## 🔄 How It Works (Behind the Scenes)

1. 🧾 You make a `POST` request to the `/payouts/interledger-wallet-address` endpoint
2. 🔐 Chimoney authenticates your API key
3. 💳 Chimoney deducts the amount from your wallet
4. 🌐 The ILP wallet address receives the payout (in supported currency)
5. 📦 A response is returned with `chiRef`, status, and optionally a `paymentLink`

---

## 🎉 You Did It
You just:

- Set up your environment
- Made a successful payout to an Interledger wallet
- Learned how to handle and debug errors


_🔗 Next up: Extend this into a backend job or admin panel!_

