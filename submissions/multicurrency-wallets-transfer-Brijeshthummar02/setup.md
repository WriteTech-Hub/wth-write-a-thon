# Setting Up Chimoney's Multi-Currency Wallet Transfer API

This guide will walk you through the process of setting up and authenticating with Chimoney's API to use the Multi-Currency Wallet Transfer functionality. By the end of this guide, you'll have everything configured to start making transfers between wallets in different currencies.

> **Developer Insight:** "I remember staring at a blank code editor, knowing I needed to implement cross-border payments but dreading the complexity ahead. This setup guide is exactly what I wish I'd had then – a clear path from zero to your first successful API call."

## Prerequisites

Before you begin, make sure you have:

- Basic understanding of APIs and HTTP requests
- A tool for making API requests (Postman, cURL, or your preferred programming language)
- A Chimoney account (don't worry, we'll cover how to create one—it takes less than 2 minutes and doesn't require a credit card for the sandbox)

## Step 1: Create a Chimoney Account

1. Visit [https://sandbox.chimoney.io/](https://sandbox.chimoney.io/) to create a sandbox account
2. Click "Sign Up" and complete the registration process
3. Verify your email address

The sandbox environment comes with $1,000 of test funds automatically loaded into your account, allowing you to experiment without using real money. This means you can focus on learning and testing without the anxiety of potentially losing funds or needing approval for every test transaction.

✓ **Achievement unlocked:** You've taken your first step toward frictionless global payments!

## Step 2: Create an Organization

1. Log in to your Chimoney sandbox account
2. Navigate to the "Organizations" tab in the dashboard
3. Click "Create Organization" or "Create Team"
4. Fill in the required details:
   - Organization name
   - Country of operation - (Full address)
   - Contact information - (Email of team members)
5. Submit the form to create your organization

Creating an organization is important as it helps organize your API keys and wallet structures, especially if you're building a platform that will handle payments for multiple users or departments. Think of it as creating a solid foundation for your payment architecture – proper organization now will save you countless headaches as your application scales.

## Step 3: Generate Your API Key

1. In your Chimoney dashboard, navigate to the "Developers" tab
2. Click "Create App" to generate a new API key
3. Fill in the application details:
   - App name (e.g., "Payment Platform")
   - Description (optional)
   - Webhook URL (optional)
4. Once created, you'll see your API key displayed
5. Copy and securely store this key—you'll need it for all API requests

> ⚠️ **Security Note**: Never expose your API key in client-side code or public repositories. Always store it securely as an environment variable or in a secure vault. Many developers have learned this lesson the hard way, with exposed keys leading to unauthorized transactions or account lockouts.

## Step 4: Understanding the API Environment

### Sandbox Environment

- Base URL: `https://sandbox-api.chimoney.io/v0.2.4/`
- Use for development and testing
- Transactions don't involve real money
- Automatically funded with test money
- Perfect for integrating and testing your application

For this guide, we'll use the sandbox environment to safely experiment with the API. The relief of knowing you can test freely without financial consequences makes the development process much more enjoyable and encourages thorough testing.

## Step 5: Setting Up Authentication

All requests to the Chimoney API require authentication using your API key in the request header.

### Header Format

```
Authorization: Bearer YOUR_API_KEY
```

Replace `YOUR_API_KEY` with the key you generated in Step 3.

### Testing Authentication

Let's verify your API key is working by making a simple request to check your account information:

```bash
curl -X GET https://sandbox-api.chimoney.io/v0.2.4/info/assets \
  -H "Authorization: Bearer YOUR_API_KEY"
```

If successful, you should receive a JSON response with a list of supported assets.

**Developer Insight:** "The first time I saw a successful response from the API, I felt a surprising sense of relief. That green status code meant I was on the right track, and suddenly the task ahead seemed manageable."

✓ **Achievement unlocked:** You've successfully authenticated with the Chimoney API!

## Step 6: Create Multi-Currency Wallets

Before you can transfer funds between currencies, you need to create wallets for each currency you want to work with. The Chimoney API currently supports USD, CAD, and NGN for multi-currency wallets.

Imagine these wallets as specialized containers designed to hold different types of currencies – each with its own rules and characteristics, but all managed through a single unified interface.

### Creating a USD Wallet

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/create \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "USD Wallet",
    "currency": "USD"
  }'
```

### Creating a NGN Wallet

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/create \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "NGN Wallet",
    "currency": "NGN"
  }'
```

### Creating a CAD Wallet

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/create \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "CAD Wallet",
    "currency": "CAD"
  }'
```

### Expected Response

```json
{
  "status": "success",
  "data": {
    "id": "wallet_abc123",
    "name": "USD Wallet",
    "currency": "USD",
    "balance": 0,
    "createdAt": "2023-07-23T12:34:56.789Z"
  }
}
```

Save the wallet IDs returned in the responses—you'll need them for transfers. These IDs are your digital keys to access each wallet, so keep them organized and accessible in your code.

✓ **Achievement unlocked:** You've created multi-currency wallets that can hold different currencies!

## Step 7: Funding Your Wallets

In the sandbox environment, your account is automatically funded with test money. You can verify your wallet balances using:

```bash
curl -X GET https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/list \
  -H "Authorization: Bearer YOUR_API_KEY"
```

This will return a list of all your wallets with their current balances.

## Step 8: Setting Up Webhook Notifications (Recommended)

It's recommended to set up webhooks to receive real-time notifications about transaction status changes.

Webhooks are like having a dedicated assistant who taps you on the shoulder the moment something important happens, rather than you having to constantly check for updates. They're especially valuable for payment systems where timely information can make the difference between a good and great user experience.

1. Create an endpoint in your application that can receive POST requests
2. In your Chimoney dashboard, go to the Developers tab
3. Update your application with the webhook URL
4. Specify the events you want to receive notifications for (e.g., "transfer.completed", "transfer.failed")

Your webhook endpoint should return a 200 OK response to acknowledge receipt of the notification.

### Sample Webhook Payload

```json
{
  "event": "transfer.completed",
  "data": {
    "id": "transfer_123456",
    "fromWallet": "wallet_abc123",
    "amount": 100,
    "sourceCurrency": "USD",
    "destinationAmount": 46000,
    "destinationCurrency": "NGN",
    "recipient": "recipient@example.com",
    "status": "completed",
    "createdAt": "2023-07-23T14:25:36.789Z"
  }
}
```

## Step 9: Making Your First Test Transfer

Now that everything is set up, let's make a test transfer between your wallets:

### First, Get a Transfer Quote

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/transfer/quote \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "fromWallet": "YOUR_USD_WALLET_ID",
    "amount": 10,
    "destinationCurrency": "NGN"
  }'
```

This will return a quote with the exchange rate, fees, and final amount:

```json
{
  "status": "success",
  "data": {
    "sourceCurrency": "USD",
    "sourceAmount": 10,
    "destinationCurrency": "NGN",
    "destinationAmount": 4600,
    "exchangeRate": 460,
    "fee": 0.25,
    "totalDebit": 10.25,
    "estimatedDeliveryTime": "instant"
  }
}
```

**Developer Insight:** "Seeing the 'estimatedDeliveryTime': 'instant' in my first quote response was eye-opening. After years of building around 3-5 day transfer delays, the possibility of instant cross-border transfers changed my entire approach to the payment flow design."

### Then, Execute the Transfer

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/transfer \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "fromWallet": "YOUR_USD_WALLET_ID",
    "toWallet": "YOUR_NGN_WALLET_ID",
    "amount": 10,
    "destinationCurrency": "NGN",
    "description": "Test transfer"
  }'
```

If successful, you should receive a response with the transfer details, including the converted amount and transaction ID.

✓ **Achievement unlocked:** You've successfully executed your first cross-currency transfer!

## Step 10: Verifying the Transfer

Check that the transfer was successful by getting the details of your wallets:

```bash
curl -X GET https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/get?id=YOUR_USD_WALLET_ID \
  -H "Authorization: Bearer YOUR_API_KEY"
```

```bash
curl -X GET https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/get?id=YOUR_NGN_WALLET_ID \
  -H "Authorization: Bearer YOUR_API_KEY"
```

You should see the updated balances reflecting the transfer you made.

## Step 11: Sending Money to Email or Phone Number

One of the powerful features of the Multi-Currency Wallet Transfer API is the ability to send money to someone using just their email or phone number, even if they don't have a Chimoney account yet.

This is where the magic of modern fintech truly shines – removing the requirement for recipients to have specific accounts or wallets before they can receive funds. It's like being able to send a package to someone without needing their exact street address – just their name and email.

### Sending to an Email

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/transfer \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "fromWallet": "YOUR_USD_WALLET_ID",
    "email": "recipient@example.com",
    "amount": 10,
    "destinationCurrency": "NGN",
    "description": "Payment for services"
  }'
```

### Sending to a Phone Number

```bash
curl -X POST https://sandbox-api.chimoney.io/v0.2.4/multicurrency-wallets/transfer \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "fromWallet": "YOUR_USD_WALLET_ID",
    "phoneNumber": "+2348012345678",
    "amount": 10,
    "destinationCurrency": "NGN",
    "description": "Payment for services"
  }'
```

Recipients will receive a notification with instructions on how to claim their funds.

**Developer Insight:** "When I first implemented the email transfer feature, our customer support tickets for payment issues dropped by nearly 40%. Users no longer needed to navigate complex wallet setups just to receive their money."

✓ **Achievement unlocked:** You can now send money to anyone with just their email or phone number!

## Common Setup Issues and Solutions

We've all experienced that moment of frustration when something doesn't work as expected. Here's how to overcome common obstacles:

| Issue | Solution |
|-------|----------|
| "Unauthorized" error | Double-check your API key and ensure it's correctly formatted in the Authorization header |
| "Wallet not found" error | Verify the wallet ID is correct and belongs to your account |
| "Insufficient funds" error | Check your wallet balance and ensure you have enough funds for the transfer including fees |
| "Invalid currency" error | Ensure you're using supported currencies (USD, CAD, NGN) |
| Webhook not receiving events | Verify your webhook URL is publicly accessible and returns a 200 OK response |

## Integration with Programming Languages

### Node.js Setup

```javascript
// Install required packages
// npm install axios dotenv

require('dotenv').config(); // Load environment variables from .env file
const axios = require('axios');

// Set up API configuration
const API_KEY = process.env.CHIMONEY_API_KEY;
const BASE_URL = 'https://sandbox-api.chimoney.io/v0.2.4';

// Create headers with authentication
const headers = {
  'Authorization': `Bearer ${API_KEY}`,
  'Content-Type': 'application/json'
};

// Example function to create a wallet
async function createWallet(name, currency) {
  try {
    const response = await axios.post(`${BASE_URL}/multicurrency-wallets/create`, {
      name,
      currency
    }, { headers });
    
    return response.data;
  } catch (error) {
    console.error('Error creating wallet:', error.response?.data || error.message);
    throw error;
  }
}

// Example function to transfer between wallets
async function transferBetweenWallets(fromWallet, toWallet, amount, currency) {
  try {
    // First get a quote
    const quoteResponse = await axios.post(`${BASE_URL}/multicurrency-wallets/transfer/quote`, {
      fromWallet,
      amount,
      destinationCurrency: currency
    }, { headers });
    
    console.log('Transfer quote:', quoteResponse.data);
    
    // Execute the transfer
    const transferResponse = await axios.post(`${BASE_URL}/multicurrency-wallets/transfer`, {
      fromWallet,
      toWallet,
      amount,
      destinationCurrency: currency,
      description: 'Transfer from Node.js application'
    }, { headers });
    
    return transferResponse.data;
  } catch (error) {
    console.error('Error transferring funds:', error.response?.data || error.message);
    throw error;
  }
}

// Example usage
async function main() {
  try {
    // Create USD wallet
    const usdWallet = await createWallet('USD Wallet', 'USD');
    console.log('Created USD wallet:', usdWallet.data.id);
    
    // Create NGN wallet
    const ngnWallet = await createWallet('NGN Wallet', 'NGN');
    console.log('Created NGN wallet:', ngnWallet.data.id);
    
    // Transfer from USD to NGN
    const transfer = await transferBetweenWallets(
      usdWallet.data.id,
      ngnWallet.data.id,
      10,
      'NGN'
    );
    
    console.log('Transfer completed:', transfer);
  } catch (error) {
    console.error('Setup failed:', error);
  }
}

main();
```

### Python Setup

```python
# Install required packages
# pip install requests python-dotenv

import os
import requests
import json
from dotenv import load_dotenv

# Load environment variables from .env file
load_dotenv()

# Set up API configuration
API_KEY = os.getenv('CHIMONEY_API_KEY')
BASE_URL = 'https://sandbox-api.chimoney.io/v0.2.4'

# Create headers with authentication
headers = {
    'Authorization': f'Bearer {API_KEY}',
    'Content-Type': 'application/json'
}

# Example function to create a wallet
def create_wallet(name, currency):
    try:
        response = requests.post(
            f'{BASE_URL}/multicurrency-wallets/create',
            headers=headers,
            json={'name': name, 'currency': currency}
        )
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f'Error creating wallet: {e}')
        if hasattr(e, 'response') and e.response:
            print(f'Error details: {e.response.text}')
        raise

# Example function to transfer between wallets
def transfer_between_wallets(from_wallet, to_wallet, amount, currency):
    try:
        # First get a quote
        quote_response = requests.post(
            f'{BASE_URL}/multicurrency-wallets/transfer/quote',
            headers=headers,
            json={
                'fromWallet': from_wallet,
                'amount': amount,
                'destinationCurrency': currency
            }
        )
        quote_response.raise_for_status()
        quote = quote_response.json()
        print(f'Transfer quote: {json.dumps(quote, indent=2)}')
        
        # Execute the transfer
        transfer_response = requests.post(
            f'{BASE_URL}/multicurrency-wallets/transfer',
            headers=headers,
            json={
                'fromWallet': from_wallet,
                'toWallet': to_wallet,
                'amount': amount,
                'destinationCurrency': currency,
                'description': 'Transfer from Python application'
            }
        )
        transfer_response.raise_for_status()
        return transfer_response.json()
    except requests.exceptions.RequestException as e:
        print(f'Error transferring funds: {e}')
        if hasattr(e, 'response') and e.response:
            print(f'Error details: {e.response.text}')
        raise

# Example usage
def main():
    try:
        # Create USD wallet
        usd_wallet = create_wallet('USD Wallet', 'USD')
        usd_wallet_id = usd_wallet['data']['id']
        print(f'Created USD wallet: {usd_wallet_id}')
        
        # Create NGN wallet
        ngn_wallet = create_wallet('NGN Wallet', 'NGN')
        ngn_wallet_id = ngn_wallet['data']['id']
        print(f'Created NGN wallet: {ngn_wallet_id}')
        
        # Transfer from USD to NGN
        transfer = transfer_between_wallets(
            usd_wallet_id,
            ngn_wallet_id,
            10,
            'NGN'
        )
        
        print(f'Transfer completed: {json.dumps(transfer, indent=2)}')
    except Exception as e:
        print(f'Setup failed: {e}')

if __name__ == '__main__':
    main()
```

## Security Best Practices

1. **Store API keys securely**: Never hardcode them in your application or expose them in client-side code. The temporary convenience isn't worth the potential security breach.
2. **Use HTTPS**: Always make API requests over HTTPS to prevent data interception
3. **Implement rate limiting**: Protect your application from potential abuse
4. **Log API interactions**: Keep detailed logs for auditing and troubleshooting
5. **Set up monitoring**: Monitor API usage and set alerts for unusual activity
6. **Validate inputs**: Always validate user inputs before sending them to the API
7. **Implement idempotency**: Use unique identifiers for transfers to prevent duplicate transactions
8. **Regular audits**: Periodically audit your wallet balances and transaction history

## Additional Resources

- [Chimoney API Documentation](https://chimoney.readme.io/reference/introduction)
- [Sandbox Environment](https://sandbox.chimoney.io/)
- [API Status Page](https://chimoney.github.io/chimoney-status/)
- [Community Support](https://discord.gg/TsyKnzT4qV)

By following this setup guide, you now have everything you need to start using Chimoney's Multi-Currency Wallet Transfer API in your applications. Whether you're building a global payroll system, a marketplace, or a remittance platform, you can now transfer funds seamlessly across different currencies.

✓ **Final Achievement Unlocked:** You've successfully set up the Chimoney Multi-Currency Wallet Transfer API and taken the first step toward building borderless financial experiences for your users!