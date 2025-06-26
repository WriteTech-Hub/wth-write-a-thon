# ⚙️ How to Write a Setup Guide

A setup guide helps new users get started with your selected Chimoney API endpoint — from authentication to making their first successful request. Think of this as a beginner-friendly onboarding experience for developers.

This guide walks you through how to write a clear, useful, and complete setup guide.

## 🧭 1. Define What the User Will Set Up

Start by introducing the goal of the setup guide.

**Example:**

> This guide will help you set up authentication and environment variables so you can successfully send a payout using Chimoney’s `multi-currency wallet transfer` endpoint.

## 🧰 2. List All Prerequisites

Tell the user what they need before starting.

### Example:

- A Chimoney developer account
- Access to the Chimoney Dashboard
- API key (and where to get it)
- A test wallet or destination
- A tool for testing requests (e.g. Postman, Hoppscotch, curl)

Use a clean bullet list in Markdown like this:

```md
### Prerequisites
- Chimoney API key
- Postman or curl
- A wallet address to test with
```
## 🛠 3. Explain How to Set Up Authentication
Clearly describe how the user should authenticate their API calls.
**Example:**
To authorize requests to the Chimoney API, include your API key in the header:

**Header:**
`Authorization: Bearer YOUR_API_KEY`
Show how to apply this header in a request tool (Postman, curl, etc.)

## 🔗 4. Help Them Make a Test Call

Guide the reader through their first request using the endpoint.

- Show what method and URL to use
- List any required headers and request body
- Provide a sample request and response

### Example Request:

```bash
curl -X POST https://api.chimoney.io/v1/wallets/transfer \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 25,
    "toWallet": "wallet-id-123",
    "currency": "USD"
  }'
```
## 🧪 5. Confirm a Successful Setup
Let users know what success looks like.
**Example:**
> If your setup is complete, you should receive a 200 OK response with a `transaction_id`.
Also, explain what common errors may appear and how to fix them.
## 🧯 6. Troubleshooting (Optional)
You can include a brief section for common setup issues.
**Example:**
| Error | Cause | Fix |
| ----- | ----- | --- |
| 401 Unauthorized | Missing or invalid API key | Double-check the Authorization header |
| 400 Bad Request | Invalid body format | Check that all required fields are included |
## ✅ Setup Guide Checklist
- The setup goal is clearly explained
- All prerequisites are listed
- Authentication is described with examples
- A full sample request is included
- Success and error responses are explained

## 💡 Final Tip
Make your guide easy to follow. Write it like you're helping someone set this up for the first time — because you are.