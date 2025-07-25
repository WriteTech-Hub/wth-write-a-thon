# Chimoney Interledger Wallet Address  Setup Guide

## Generate a Interledger Wallet Address using Chimoney Sandbox API

> **Setup Guide:** This walkthrough will help you authenticate and use Chimoney’s  
> `/accounts/issue-wallet-address` endpoint to generate an ILP wallet address for a user.

---

### 🔐 **Step 1: Create a Sandbox Account & Get Your API Key**

### 1. Go to [sandbox.chimoney.io](https://sandbox.chimoney.io)

Click *“Sign Up”* or *“Sign In”*.

📸 **Screenshot Example:** 

![Chimoney Sandbox Sign Up](https://i.postimg.cc/Wz4ZYZVy/image.png)

### 2. Developer Dashboard

Once logged in, you'll land on the **Developer Dashboard**.  
Navigate to the **Developers & API** section from the sidebar.

📸 **Screenshot Example:**  

![Chimoney Developer Portal Navigation](https://i.postimg.cc/Nf99yBjR/image.png)

### 3. Create an App

Click **“Add New App”** and enter an App name.

Once created:

- Copy your **API Key**
- Copy your **User ID** (labelled as `teamId`)

📸 **Screenshot Example:** 

![Create App and Copy API Key](https://i.postimg.cc/C19Z7QvR/image.png)

> You’ll use the `teamId` as your `userId` in requests.

---

### 🧰 Prerequisites

- Chimoney Sandbox developer account
- Your **API key** from the Developer dashboard
- Your **teamId** (used as `userId`)
- A unique `ilpUsername` (e.g. `jane.doe`)
- Postman or cURL for testing the endpoint

---

### 📤 **Step 2: Make Your First API Call**

### 🔐 1. Set Up Authentication in Postman

- Open your collection or create a new **POST** request.
- Under **Headers**, add:

  | Key          | Value                  |
  | ------------ | ---------------------- |
  | X-API-KEY    | `YOUR_SANDBOX_API_KEY` |
  | Content-Type | `application/json`     |

**Screenshot:**  
![Postman Header Setup](https://i.postimg.cc/dQdDvnH9/image.png)

### 🚀 2. Configure the Request

- **Method & URL**

  POST <https://api-v2-sandbox.chimoney.io/v0.2.4/accounts/issue-wallet-address>

- **Body (raw JSON)**

  ```json
  {
    "userID": "your_team_id",
    "ilpUsername": "jane.doe"
  }
  ```

**Screenshot:**  

![Postman Request Body Setup](https://i.postimg.cc/Pfmf0JJ3/image.png)

### ✅ **3. Send & Verify**

- **Hit **Send** in Postman**
- **Look for 200 OK response**

  ```json
  {
    "status": "success",
    "message": "Wallet Address issued succesfully",
    "data": {
      "id": "",
      "createdAt": "2025-07-22T22:09:21.756Z",
      "publicName": "Public Name",
      "url": "https://ilp-sandbox.chimoney.com/user",
      "status": "ACTIVE",
      "asset": {
        "code": "USD",
        "createdAt": "2024-08-24T19:57:56.102Z",
        "id": "",
        "scale": 2,
        "withdrawalThreshold": null
      },
      "additionalProperties": []
    }
  }
  ```

**Screenshot:**  
![Postman Response](https://i.postimg.cc/0N3CLxZn/image.png)

### ❌ **4. Common Errors & Fixes**

| HTTP Status | Type             | Code                    | Message                                       | Likely Cause                             | Suggested Fix                                     |
|-------------|------------------|--------------------------|-----------------------------------------------|-------------------------------------------|--------------------------------------------------|
| 400         | Validation Error | INVALID_REQUEST          | The request parameters are invalid.           | `userId` or `ilpUsername` is invalid or missing | Double-check the request body and field formats |
| 401         | Unauthorized     | –                        | Unauthorized access                           | API key is missing or incorrect           | Ensure `X-API-KEY` is present and valid in header |
| 500         | Server Error     | INTERNAL_SERVER_ERROR    | An internal server error occurred.            | Issue on Chimoney’s end or malformed request | Try again later or contact Chimoney support       |

### 🛠 **5. Troubleshooting Tips**

- ✅ Use your exact teamId from the app settings — not a random string.
- 🔑 If you get 401, re-copy the API key from the dashboard.
- 🔁 Use a unique ilpUsername per user (you can reuse the same userId for multiple ILP addresses).