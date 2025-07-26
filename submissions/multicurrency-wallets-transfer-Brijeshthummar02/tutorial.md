# Tutorial: Implementing Multi-Currency Wallet Transfers with Chimoney API

This tutorial will guide you through the process of implementing Chimoney's Multi-Currency Wallet Transfer API in your application. By the end of this guide, you'll be able to create wallets in different currencies, get exchange rate quotes, and transfer funds between currencies seamlessly.

> **Developer Journey:** "When I first needed to implement cross-currency payments for my client's application, I spent weeks juggling multiple payment providers and conversion services. That complexity nearly cost us our project deadline. Then I discovered Chimoney's API. What once took 120+ lines of code across multiple services now takes just 15 lines with a single API. That feeling when the first test transfer succeeded is something I want you to experience today."

## Introduction

Chimoney's Multi-Currency Wallet Transfer API allows developers to build applications that can transfer money between different currencies with minimal friction. This functionality is particularly valuable for:

- Global payroll systems
- Cross-border e-commerce platforms
- Remittance applications
- Freelance marketplaces with international users
- Travel applications handling multiple currencies

The API currently supports transfers between USD, CAD, and NGN wallets, with more currencies planned for the future.

## Prerequisites

Before starting this tutorial, make sure you have:

- A Chimoney sandbox account
- API key generated from your Chimoney dashboard
- Basic knowledge of RESTful APIs
- A development environment with your preferred programming language
- HTTP client (like Axios, Fetch, or cURL) for making API requests

Don't worry if you're not an API expert—this tutorial assumes you're new to Chimoney but not new to coding. We'll celebrate each small victory along the way, from your first wallet creation to your first successful cross-currency transfer. Remember: every financial integration expert started exactly where you are now.

If you haven't set up your Chimoney account and API key yet, please refer to the [Setup Guide](./setup.md) first.

## Step 1: Create Multi-Currency Wallets

The first step is to create wallets for each currency you want to work with. For this tutorial, we'll create wallets for USD, NGN, and CAD.

### Creating a USD Wallet

```javascript
// Using Node.js with Axios
const axios = require('axios');

const API_KEY = 'YOUR_CHIMONEY_API_KEY';
const BASE_URL = 'https://sandbox-api.chimoney.io/v0.2.4';

async function createUSDWallet() {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/create`,
      {
        name: 'USD Wallet',
        currency: 'USD'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('USD Wallet created:', response.data);
    return response.data.data.id; // Save this wallet ID for later use
  } catch (error) {
    console.error('Error creating USD wallet:', error.response?.data || error.message);
    throw error;
  }
}
```

**Developer Insight:** "There's something deeply satisfying about that first successful API response. It's like the digital equivalent of laying the foundation for a house—not glamorous yet, but essential for everything that follows."

✓ **Achievement unlocked:** You've created your first digital currency container!

### Creating an NGN Wallet

```javascript
async function createNGNWallet() {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/create`,
      {
        name: 'NGN Wallet',
        currency: 'NGN'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('NGN Wallet created:', response.data);
    return response.data.data.id; // Save this wallet ID for later use
  } catch (error) {
    console.error('Error creating NGN wallet:', error.response?.data || error.message);
    throw error;
  }
}
```

### Creating a CAD Wallet

```javascript
async function createCADWallet() {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/create`,
      {
        name: 'CAD Wallet',
        currency: 'CAD'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('CAD Wallet created:', response.data);
    return response.data.data.id; // Save this wallet ID for later use
  } catch (error) {
    console.error('Error creating CAD wallet:', error.response?.data || error.message);
    throw error;
  }
}
```

## Step 2: List Your Wallets

After creating the wallets, you can list them to verify they were created successfully and check their balances:

```javascript
async function listWallets() {
  try {
    const response = await axios.get(
      `${BASE_URL}/multicurrency-wallets/list`,
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`
        }
      }
    );
    
    console.log('Your wallets:', response.data);
    return response.data.data; // Array of wallet objects
  } catch (error) {
    console.error('Error listing wallets:', error.response?.data || error.message);
    throw error;
  }
}
```

## Step 3: Get a Transfer Quote

Before executing a transfer, it's a good practice to get a quote to show the user the exchange rate, fees, and final amount they'll receive. This step transforms the anxiety-inducing mystery of "How much will this actually cost?" into a transparent transaction where everyone knows exactly what to expect.

Think of it like checking the weather before going out—it helps you prepare for what's ahead and prevents unpleasant surprises:

```javascript
async function getTransferQuote(fromWalletId, amount, destinationCurrency) {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/transfer/quote`,
      {
        fromWallet: fromWalletId,
        amount: amount,
        destinationCurrency: destinationCurrency
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('Transfer quote:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('Error getting transfer quote:', error.response?.data || error.message);
    throw error;
  }
}
```

Example usage:

```javascript
// Get a quote for converting 100 USD to NGN
const quote = await getTransferQuote('usd_wallet_id', 100, 'NGN');
console.log(`100 USD = ${quote.destinationAmount} NGN`);
console.log(`Exchange rate: 1 USD = ${quote.exchangeRate} NGN`);
console.log(`Fee: ${quote.fee} USD`);
console.log(`Total debit: ${quote.totalDebit} USD`);
```

## Step 4: Execute a Wallet-to-Wallet Transfer

Now that we have our wallets and a quote, we can execute a transfer between wallets:

```javascript
async function transferBetweenWallets(fromWalletId, toWalletId, amount, destinationCurrency, description) {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/transfer`,
      {
        fromWallet: fromWalletId,
        toWallet: toWalletId,
        amount: amount,
        destinationCurrency: destinationCurrency,
        description: description || 'Wallet to wallet transfer'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('Transfer successful:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('Error transferring funds:', error.response?.data || error.message);
    throw error;
  }
}
```

Example usage:

```javascript
// Transfer 100 USD to NGN wallet
const transfer = await transferBetweenWallets(
  'usd_wallet_id',
  'ngn_wallet_id',
  100,
  'NGN',
  'Salary payment'
);

console.log(`Transfer ID: ${transfer.id}`);
console.log(`Amount sent: ${transfer.amount} ${transfer.sourceCurrency}`);
console.log(`Amount received: ${transfer.destinationAmount} ${transfer.destinationCurrency}`);
console.log(`Status: ${transfer.status}`);
```

When this code executes successfully, there's a moment of pure developer satisfaction—watching the funds move from one currency to another in seconds rather than days. What might seem like a simple transfer to us represents something much more significant to the recipient: timely access to their earnings, ability to pay bills without delays, or simply the peace of mind that comes with financial stability.

✓ **Achievement unlocked:** You've successfully moved money across currencies in seconds instead of days!

## Step 5: Transfer to Email or Phone Number

One of the powerful features of Chimoney's API is the ability to send money to someone using just their email or phone number, even if they don't have a Chimoney account yet.

This is where the magic of modern fintech truly shines. It's like being able to deliver a package to someone without needing their exact street address—just their name and email. This seemingly small feature can be transformative for businesses working with freelancers in regions with limited banking infrastructure:

```javascript
async function transferToEmail(fromWalletId, email, amount, destinationCurrency, description) {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/transfer`,
      {
        fromWallet: fromWalletId,
        email: email,
        amount: amount,
        destinationCurrency: destinationCurrency,
        description: description || 'Payment via email'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('Transfer to email successful:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('Error transferring to email:', error.response?.data || error.message);
    throw error;
  }
}
```

```javascript
async function transferToPhone(fromWalletId, phoneNumber, amount, destinationCurrency, description) {
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/transfer`,
      {
        fromWallet: fromWalletId,
        phoneNumber: phoneNumber,
        amount: amount,
        destinationCurrency: destinationCurrency,
        description: description || 'Payment via phone'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    console.log('Transfer to phone successful:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('Error transferring to phone:', error.response?.data || error.message);
    throw error;
  }
}
```

Example usage:

```javascript
// Send 50 USD (converted to NGN) to an email
const emailTransfer = await transferToEmail(
  'usd_wallet_id',
  'recipient@example.com',
  50,
  'NGN',
  'Freelance payment'
);

// Send 75 USD (converted to NGN) to a phone number
const phoneTransfer = await transferToPhone(
  'usd_wallet_id',
  '+2348012345678',
  75,
  'NGN',
  'Contract payment'
);
```

## Step 6: Check Transfer Status

After initiating a transfer, you may want to check its status:

```javascript
async function checkTransferStatus(transferId) {
  try {
    const response = await axios.get(
      `${BASE_URL}/multicurrency-wallets/transfer/status?id=${transferId}`,
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`
        }
      }
    );
    
    console.log('Transfer status:', response.data);
    return response.data.data;
  } catch (error) {
    console.error('Error checking transfer status:', error.response?.data || error.message);
    throw error;
  }
}
```

Example usage:

```javascript
// Check the status of a transfer
const status = await checkTransferStatus('transfer_123456');
console.log(`Transfer status: ${status.status}`);
console.log(`Created at: ${new Date(status.createdAt).toLocaleString()}`);
if (status.completedAt) {
  console.log(`Completed at: ${new Date(status.completedAt).toLocaleString()}`);
}
```

## Step 7: Implement Error Handling

We've all felt that moment of panic when a financial transaction fails and we don't know if the money is gone, pending, or refunded. That's why robust error handling isn't just good practice—it's essential for maintaining your users' trust and your own peace of mind.

Proper error handling transforms a potentially anxiety-inducing experience into a manageable situation where problems have clear solutions. Here's how to handle common errors:

```javascript
async function safeTransfer(fromWalletId, toWalletId, amount, destinationCurrency, description) {
  try {
    // First, check if the wallet has sufficient balance
    const walletInfo = await axios.get(
      `${BASE_URL}/multicurrency-wallets/get?id=${fromWalletId}`,
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`
        }
      }
    );
    
    const wallet = walletInfo.data.data;
    if (wallet.balance < amount) {
      throw new Error(`Insufficient balance: ${wallet.balance} ${wallet.currency} available, ${amount} ${wallet.currency} required`);
    }
    
    // Get a quote to check fees
    const quote = await getTransferQuote(fromWalletId, amount, destinationCurrency);
    const totalRequired = quote.totalDebit;
    
    if (wallet.balance < totalRequired) {
      throw new Error(`Insufficient balance including fees: ${wallet.balance} ${wallet.currency} available, ${totalRequired} ${wallet.currency} required (includes ${quote.fee} fee)`);
    }
    
    // Execute the transfer
    const transfer = await transferBetweenWallets(
      fromWalletId,
      toWalletId,
      amount,
      destinationCurrency,
      description
    );
    
    return {
      success: true,
      transfer: transfer,
      quote: quote
    };
  } catch (error) {
    // Handle specific API errors
    if (error.response) {
      const statusCode = error.response.status;
      const errorData = error.response.data;
      
      if (statusCode === 401) {
        return {
          success: false,
          error: 'Authentication failed. Please check your API key.',
          details: errorData
        };
      } else if (statusCode === 404) {
        return {
          success: false,
          error: 'Wallet not found. Please check the wallet IDs.',
          details: errorData
        };
      } else if (statusCode === 400) {
        return {
          success: false,
          error: 'Invalid request. Please check your parameters.',
          details: errorData
        };
      } else {
        return {
          success: false,
          error: `API error (${statusCode}): ${errorData.message || 'Unknown error'}`,
          details: errorData
        };
      }
    } else {
      // Handle network or other errors
      return {
        success: false,
        error: error.message || 'Unknown error occurred',
        details: error
      };
    }
  }
}
```

## Step 8: Building a Complete Transfer Flow

Now let's put everything together to create a complete transfer flow. This is where separate pieces become a cohesive experience—like assembling individual ingredients into a delicious meal that's greater than the sum of its parts.

Your users won't see the individual API calls; they'll experience the outcome: the delight of sending money across borders without complexity, the relief of knowing exactly what fees to expect, and the satisfaction of seeing the recipient receive funds instantly:

```javascript
async function completeTransferFlow() {
  try {
    // Step 1: Create wallets if they don't exist
    console.log('Creating wallets...');
    const usdWalletId = await createUSDWallet();
    const ngnWalletId = await createNGNWallet();
    
    // Step 2: List wallets to check balances
    console.log('Listing wallets...');
    const wallets = await listWallets();
    const usdWallet = wallets.find(w => w.id === usdWalletId);
    const ngnWallet = wallets.find(w => w.id === ngnWalletId);
    
    console.log(`USD Wallet balance: ${usdWallet.balance} USD`);
    console.log(`NGN Wallet balance: ${ngnWallet.balance} NGN`);
    
    // Step 3: Get a transfer quote
    console.log('Getting transfer quote...');
    const amount = 100; // USD
    const quote = await getTransferQuote(usdWalletId, amount, 'NGN');
    
    console.log(`Exchange rate: 1 USD = ${quote.exchangeRate} NGN`);
    console.log(`${amount} USD = ${quote.destinationAmount} NGN`);
    console.log(`Fee: ${quote.fee} USD`);
    console.log(`Total debit: ${quote.totalDebit} USD`);
    
    // Step 4: Confirm with user (simulated here)
    const userConfirmed = true; // In a real app, this would come from user input
    
    if (userConfirmed) {
      // Step 5: Execute the transfer
      console.log('Executing transfer...');
      const transfer = await transferBetweenWallets(
        usdWalletId,
        ngnWalletId,
        amount,
        'NGN',
        'Cross-currency transfer demo'
      );
      
      console.log(`Transfer successful! ID: ${transfer.id}`);
      console.log(`Amount sent: ${transfer.amount} ${transfer.sourceCurrency}`);
      console.log(`Amount received: ${transfer.destinationAmount} ${transfer.destinationCurrency}`);
      
      // Step 6: Check updated balances
      console.log('Checking updated balances...');
      const updatedWallets = await listWallets();
      const updatedUsdWallet = updatedWallets.find(w => w.id === usdWalletId);
      const updatedNgnWallet = updatedWallets.find(w => w.id === ngnWalletId);
      
      console.log(`New USD Wallet balance: ${updatedUsdWallet.balance} USD`);
      console.log(`New NGN Wallet balance: ${updatedNgnWallet.balance} NGN`);
      
      return {
        success: true,
        transfer: transfer,
        usdBalance: updatedUsdWallet.balance,
        ngnBalance: updatedNgnWallet.balance
      };
    } else {
      console.log('Transfer cancelled by user');
      return {
        success: false,
        reason: 'cancelled'
      };
    }
  } catch (error) {
    console.error('Error in transfer flow:', error);
    return {
      success: false,
      error: error.message || 'Unknown error occurred',
      details: error
    };
  }
}
```

## Step 9: Implementing a Transfer UI (Web Example)

The API is powerful, but users interact with interfaces, not endpoints. A thoughtful UI can transform a technical process into an intuitive experience that builds trust and confidence.

Remember: behind every transfer is a real person with real needs—perhaps a freelancer waiting for payment to buy groceries, or a parent sending money to a child studying abroad. Your interface is the bridge between complex financial operations and human needs:

Here's a simple HTML and JavaScript example for implementing a transfer UI in a web application:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Currency Wallet Transfer</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }
    .container {
      background-color: #f9f9f9;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
    .wallets {
      display: flex;
      justify-content: space-between;
      margin-bottom: 20px;
    }
    .wallet {
      background-color: white;
      border-radius: 8px;
      padding: 15px;
      width: 30%;
      box-shadow: 0 1px 5px rgba(0, 0, 0, 0.05);
    }
    .wallet h3 {
      margin-top: 0;
      color: #333;
    }
    .balance {
      font-size: 24px;
      font-weight: bold;
      color: #0066cc;
    }
    .form-group {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: 500;
    }
    input, select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      font-size: 16px;
    }
    button {
      background-color: #0066cc;
      color: white;
      border: none;
      border-radius: 4px;
      padding: 12px 20px;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #0052a3;
    }
    .quote {
      background-color: #e9f7ff;
      border-radius: 8px;
      padding: 15px;
      margin: 20px 0;
      display: none;
    }
    .result {
      background-color: #e6ffe6;
      border-radius: 8px;
      padding: 15px;
      margin-top: 20px;
      display: none;
    }
    .error {
      background-color: #ffe6e6;
      border-radius: 8px;
      padding: 15px;
      margin-top: 20px;
      color: #cc0000;
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Multi-Currency Wallet Transfer</h1>
    
    <div class="wallets">
      <div class="wallet">
        <h3>USD Wallet</h3>
        <div class="balance" id="usd-balance">$1,000.00</div>
      </div>
      <div class="wallet">
        <h3>NGN Wallet</h3>
        <div class="balance" id="ngn-balance">₦0.00</div>
      </div>
      <div class="wallet">
        <h3>CAD Wallet</h3>
        <div class="balance" id="cad-balance">C$0.00</div>
      </div>
    </div>
    
    <form id="transfer-form">
      <div class="form-group">
        <label for="from-wallet">From Wallet</label>
        <select id="from-wallet" required>
          <option value="usd">USD Wallet</option>
          <option value="ngn">NGN Wallet</option>
          <option value="cad">CAD Wallet</option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="to-wallet">To Wallet</label>
        <select id="to-wallet" required>
          <option value="usd">USD Wallet</option>
          <option value="ngn" selected>NGN Wallet</option>
          <option value="cad">CAD Wallet</option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="amount">Amount</label>
        <input type="number" id="amount" min="1" step="0.01" required placeholder="Enter amount to transfer">
      </div>
      
      <div class="form-group">
        <label for="description">Description (optional)</label>
        <input type="text" id="description" placeholder="What's this transfer for?">
      </div>
      
      <button type="button" id="get-quote">Get Quote</button>
    </form>
    
    <div class="quote" id="quote-container">
      <h3>Transfer Quote</h3>
      <p>Exchange Rate: <span id="exchange-rate">-</span></p>
      <p>You Send: <span id="send-amount">-</span></p>
      <p>Recipient Gets: <span id="receive-amount">-</span></p>
      <p>Fee: <span id="fee-amount">-</span></p>
      <p>Total Debit: <span id="total-debit">-</span></p>
      
      <button type="button" id="confirm-transfer">Confirm Transfer</button>
    </div>
    
    <div class="result" id="result-container">
      <h3>Transfer Successful!</h3>
      <p>Transfer ID: <span id="transfer-id">-</span></p>
      <p>Amount Sent: <span id="sent-amount">-</span></p>
      <p>Amount Received: <span id="received-amount">-</span></p>
      <p>Status: <span id="transfer-status">-</span></p>
    </div>
    
    <div class="error" id="error-container">
      <h3>Error</h3>
      <p id="error-message"></p>
    </div>
  </div>

  <script>
    // This would be your actual API integration code
    // For demo purposes, we're simulating the API calls
    
    // Wallet data (would come from your API)
    const wallets = {
      usd: { id: 'wallet_usd123', balance: 1000, currency: 'USD' },
      ngn: { id: 'wallet_ngn456', balance: 0, currency: 'NGN' },
      cad: { id: 'wallet_cad789', balance: 0, currency: 'CAD' }
    };
    
    // Update wallet balances in the UI
    function updateWalletBalances() {
      document.getElementById('usd-balance').textContent = `$${wallets.usd.balance.toFixed(2)}`;
      document.getElementById('ngn-balance').textContent = `₦${wallets.ngn.balance.toFixed(2)}`;
      document.getElementById('cad-balance').textContent = `C$${wallets.cad.balance.toFixed(2)}`;
    }
    
    // Initialize the page
    updateWalletBalances();
    
    // Get quote button click handler
    document.getElementById('get-quote').addEventListener('click', async function() {
      const fromWallet = document.getElementById('from-wallet').value;
      const toWallet = document.getElementById('to-wallet').value;
      const amount = parseFloat(document.getElementById('amount').value);
      
      if (fromWallet === toWallet) {
        showError('Source and destination wallets must be different');
        return;
      }
      
      if (isNaN(amount) || amount <= 0) {
        showError('Please enter a valid amount');
        return;
      }
      
      try {
        // Simulate API call to get quote
        // In a real app, this would be an actual API call
        const quote = simulateGetQuote(fromWallet, amount, wallets[toWallet].currency);
        
        // Display the quote
        document.getElementById('exchange-rate').textContent = `1 ${wallets[fromWallet].currency} = ${quote.exchangeRate} ${wallets[toWallet].currency}`;
        document.getElementById('send-amount').textContent = `${amount} ${wallets[fromWallet].currency}`;
        document.getElementById('receive-amount').textContent = `${quote.destinationAmount} ${wallets[toWallet].currency}`;
        document.getElementById('fee-amount').textContent = `${quote.fee} ${wallets[fromWallet].currency}`;
        document.getElementById('total-debit').textContent = `${quote.totalDebit} ${wallets[fromWallet].currency}`;
        
        // Show the quote container
        document.getElementById('quote-container').style.display = 'block';
        document.getElementById('error-container').style.display = 'none';
      } catch (error) {
        showError(error.message);
      }
    });
    
    // Confirm transfer button click handler
    document.getElementById('confirm-transfer').addEventListener('click', async function() {
      const fromWallet = document.getElementById('from-wallet').value;
      const toWallet = document.getElementById('to-wallet').value;
      const amount = parseFloat(document.getElementById('amount').value);
      const description = document.getElementById('description').value || 'Wallet transfer';
      
      try {
        // Simulate API call to execute transfer
        // In a real app, this would be an actual API call
        const transfer = simulateTransfer(
          wallets[fromWallet].id,
          wallets[toWallet].id,
          amount,
          wallets[toWallet].currency,
          description
        );
        
        // Update wallet balances
        wallets[fromWallet].balance -= transfer.totalDebit;
        wallets[toWallet].balance += transfer.destinationAmount;
        updateWalletBalances();
        
        // Display the result
        document.getElementById('transfer-id').textContent = transfer.id;
        document.getElementById('sent-amount').textContent = `${transfer.amount} ${transfer.sourceCurrency}`;
        document.getElementById('received-amount').textContent = `${transfer.destinationAmount} ${transfer.destinationCurrency}`;
        document.getElementById('transfer-status').textContent = transfer.status;
        
        // Show the result container
        document.getElementById('result-container').style.display = 'block';
        document.getElementById('quote-container').style.display = 'none';
        document.getElementById('error-container').style.display = 'none';
      } catch (error) {
        showError(error.message);
      }
    });
    
    // Helper function to show errors
    function showError(message) {
      document.getElementById('error-message').textContent = message;
      document.getElementById('error-container').style.display = 'block';
      document.getElementById('quote-container').style.display = 'none';
      document.getElementById('result-container').style.display = 'none';
    }
    
    // Simulate getting a quote (in a real app, this would be an API call)
    function simulateGetQuote(fromWallet, amount, destinationCurrency) {
      // Check if wallet has enough balance
      if (wallets[fromWallet].balance < amount) {
        throw new Error(`Insufficient balance in ${wallets[fromWallet].currency} wallet`);
      }
      
      // Exchange rates (simplified for demo)
      const rates = {
        'USD_NGN': 460,
        'USD_CAD': 1.35,
        'NGN_USD': 0.00217,
        'NGN_CAD': 0.00293,
        'CAD_USD': 0.74,
        'CAD_NGN': 340
      };
      
      const sourceCurrency = wallets[fromWallet].currency;
      const rateKey = `${sourceCurrency}_${destinationCurrency}`;
      
      if (!rates[rateKey]) {
        throw new Error(`Unsupported currency conversion: ${sourceCurrency} to ${destinationCurrency}`);
      }
      
      const exchangeRate = rates[rateKey];
      const destinationAmount = amount * exchangeRate;
      const fee = amount * 0.025; // 2.5% fee
      const totalDebit = amount + fee;
      
      return {
        sourceCurrency,
        sourceAmount: amount,
        destinationCurrency,
        destinationAmount,
        exchangeRate,
        fee,
        totalDebit,
        estimatedDeliveryTime: 'instant'
      };
    }
    
    // Simulate executing a transfer (in a real app, this would be an API call)
    function simulateTransfer(fromWalletId, toWalletId, amount, destinationCurrency, description) {
      // Get the wallet objects
      const fromWallet = Object.values(wallets).find(w => w.id === fromWalletId);
      const toWallet = Object.values(wallets).find(w => w.id === toWalletId);
      
      if (!fromWallet || !toWallet) {
        throw new Error('Invalid wallet ID');
      }
      
      // Get a quote first
      const quote = simulateGetQuote(
        Object.keys(wallets).find(k => wallets[k].id === fromWalletId),
        amount,
        destinationCurrency
      );
      
      // Check if wallet has enough balance including fees
      if (fromWallet.balance < quote.totalDebit) {
        throw new Error(`Insufficient balance including fees: ${fromWallet.balance} ${fromWallet.currency} available, ${quote.totalDebit} ${fromWallet.currency} required`);
      }
      
      // Generate a random transfer ID
      const transferId = 'transfer_' + Math.random().toString(36).substring(2, 10);
      
      return {
        id: transferId,
        fromWallet: fromWalletId,
        toWallet: toWalletId,
        amount: amount,
        sourceCurrency: fromWallet.currency,
        destinationAmount: quote.destinationAmount,
        destinationCurrency: destinationCurrency,
        exchangeRate: quote.exchangeRate,
        fee: quote.fee,
        totalDebit: quote.totalDebit,
        description: description,
        status: 'completed',
        createdAt: new Date().toISOString()
      };
    }
  </script>
</body>
</html>
```

## Step 10: Testing Your Implementation

Before deploying to your application, thoroughly test your implementation:

1. **Create wallets in different currencies**
   - Verify wallet creation in USD, NGN, and CAD
   - Check that wallet IDs are correctly returned and stored

2. **Test wallet listing**
   - Ensure all created wallets are visible
   - Verify balances are correctly displayed

3. **Test quote functionality**
   - Request quotes for various amounts and currency pairs
   - Verify exchange rates, fees, and final amounts are reasonable

4. **Test wallet-to-wallet transfers**
   - Transfer between your own wallets in different currencies
   - Verify source wallet is debited correctly (including fees)
   - Verify destination wallet is credited with the converted amount

5. **Test transfers to email/phone**
   - Send test transfers to your own email/phone
   - Verify the recipient notification and claim process

6. **Test error handling**
   - Attempt transfers with insufficient balance
   - Use invalid wallet IDs
   - Try unsupported currency conversions
   - Test with invalid API keys

7. **Test webhook notifications**
   - Set up a test endpoint to receive webhooks
   - Verify that transfer status updates trigger notifications

## The Human Impact of Your Implementation

As you complete this tutorial, it's worth reflecting on what you've actually built:

- For the Nigerian developer, your implementation means receiving payment immediately rather than waiting 5 days
- For the Canadian designer, it means receiving more of their actual earnings instead of losing 5-7% to fees
- For the finance manager, it means spending Thursday afternoon on strategic planning instead of troubleshooting payment issues
- For your company, it means attracting global talent without geographical payment limitations

These aren't just technical achievements—they're human impacts that ripple outward from your code. The time you've invested in this implementation will pay dividends in user satisfaction, operational efficiency, and global accessibility.

✓ **Achievement unlocked:** You've built a borderless financial system that connects people and value across currencies and countries!

## Conclusion

You've now successfully implemented Chimoney's Multi-Currency Wallet Transfer API in your application. This is no small achievement—you've essentially built a bridge across the traditional financial borders that separate people and currencies.

This functionality allows your users to:

- Create wallets in different currencies (USD, NGN, CAD)
- Get real-time exchange rate quotes (transparency that builds trust)
- Transfer funds between wallets with automatic currency conversion (complexity made simple)
- Send money to recipients via email or phone number (accessibility for all)

By following this tutorial, you've built a robust integration that handles the complexities of cross-currency transfers, including exchange rates, fees, and error handling. More importantly, you've created an opportunity for your application to become an essential part of your users' financial lives.

**Final Achievement Unlocked:** You've mastered the Multi-Currency Wallet Transfer API and joined the community of developers building borderless financial experiences!

## Interactive Testing Resources

To help you test and explore the Multi-Currency Wallet Transfer API, Chimoney provides several interactive resources:

1. **Swagger UI**: You can test the API directly through Chimoney's Swagger interface at [https://sandbox-api.chimoney.io/v0.2.4/api-docs/#/Interledger/post_v0_2_4_multicurrency_wallets_transfer](https://api.chimoney.io/v0.2.4/api-docs/#/Interledger/post_v0_2_4_multicurrency_wallets_transfer). This allows you to:
   - Execute API calls directly from your browser
   - See all available parameters and their descriptions
   - Test different parameter combinations
   - View complete response structures

2. **Official API Documentation**: For detailed information about all multi-currency wallet endpoints, refer to the official Chimoney documentation at [https://chimoney.readme.io/reference/post_v0-2-4-multicurrency-wallets-create](https://chimoney.readme.io/reference/post_v0-2-4-multicurrency-wallets-create). The documentation includes:
   - Detailed parameter descriptions
   - Response examples
   - Error codes and their meanings
   - Implementation notes and best practices

These resources are invaluable for both initial implementation and troubleshooting, allowing you to test API behavior before writing any code.

## Troubleshooting Common Issues

When implementing the Multi-Currency Wallet Transfer API, you might encounter these common issues:

### Authentication Problems
- **Issue**: 401 Unauthorized errors
- **Solution**: Verify your API key is correct and properly formatted in the Authorization header as `Bearer YOUR_API_KEY`
- **Prevention**: Store API keys securely and implement key rotation

### Insufficient Funds
- **Issue**: 400 Bad Request with "insufficient_funds" error code
- **Solution**: Always check wallet balance before attempting transfers and include fee calculations
- **Prevention**: Implement the quote-before-transfer pattern shown in this tutorial

### Invalid Wallet IDs
- **Issue**: 404 Not Found when referencing wallet IDs
- **Solution**: Verify wallet IDs exist and belong to your account
- **Prevention**: Store wallet IDs persistently and validate before use

### Currency Conversion Limitations
- **Issue**: Unsupported currency pair errors
- **Solution**: Check the supported currency pairs in the API documentation
- **Prevention**: Only offer supported currency options in your UI

### Rate Limiting
- **Issue**: 429 Too Many Requests
- **Solution**: Implement exponential backoff and retry logic
- **Prevention**: Cache results where appropriate and batch operations when possible

### Webhook Configuration
- **Issue**: Not receiving transfer status updates
- **Solution**: Verify webhook URL is accessible from the internet and properly handling POST requests
- **Prevention**: Test webhook endpoint with sample payloads before relying on it

For any persistent issues, contact Chimoney support through their [Discord community](https://discord.gg/TsyKnzT4qV) in the dedicated support channels.

## Python Implementation Example

For developers working with Python, here's an implementation of the key functionality using the `requests` library:

```python
import requests
import json

class ChimoneyWalletAPI:
    def __init__(self, api_key):
        """Initialize the Chimoney Wallet API client.
        
        Args:
            api_key (str): Your Chimoney API key
        """
        self.api_key = api_key
        self.base_url = "https://sandbox-api.chimoney.io/v0.2.4"
        self.headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
    
    def create_wallet(self, name, currency):
        """Create a new wallet with specified currency.
        
        Args:
            name (str): Name for the new wallet
            currency (str): Currency code (USD, NGN, CAD)
            
        Returns:
            dict: Wallet data including ID and balance
        """
        url = f"{self.base_url}/multicurrency-wallets/create"
        payload = {
            "name": name,
            "currency": currency
        }
        
        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()  # Raise exception for 4XX/5XX responses
        
        return response.json().get("data")
    
    def list_wallets(self):
        """List all wallets in the account.
        
        Returns:
            list: List of wallet objects
        """
        url = f"{self.base_url}/multicurrency-wallets/list"
        
        response = requests.get(url, headers=self.headers)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def get_wallet(self, wallet_id):
        """Get details for a specific wallet.
        
        Args:
            wallet_id (str): ID of the wallet to retrieve
            
        Returns:
            dict: Wallet details including balance
        """
        url = f"{self.base_url}/multicurrency-wallets/get"
        params = {"id": wallet_id}
        
        response = requests.get(url, headers=self.headers, params=params)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def get_transfer_quote(self, from_wallet_id, amount, destination_currency):
        """Get a quote for currency conversion.
        
        Args:
            from_wallet_id (str): Source wallet ID
            amount (float): Amount to transfer
            destination_currency (str): Target currency code
            
        Returns:
            dict: Quote details including exchange rate and fees
        """
        url = f"{self.base_url}/multicurrency-wallets/transfer/quote"
        payload = {
            "fromWallet": from_wallet_id,
            "amount": amount,
            "destinationCurrency": destination_currency
        }
        
        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def transfer_between_wallets(self, from_wallet_id, to_wallet_id, amount, 
                                destination_currency, description=None):
        """Transfer funds between wallets.
        
        Args:
            from_wallet_id (str): Source wallet ID
            to_wallet_id (str): Destination wallet ID
            amount (float): Amount to transfer
            destination_currency (str): Currency to convert to
            description (str, optional): Transfer description
            
        Returns:
            dict: Transfer details
        """
        url = f"{self.base_url}/multicurrency-wallets/transfer"
        payload = {
            "fromWallet": from_wallet_id,
            "toWallet": to_wallet_id,
            "amount": amount,
            "destinationCurrency": destination_currency
        }
        
        if description:
            payload["description"] = description
        
        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def transfer_to_email(self, from_wallet_id, email, amount, 
                         destination_currency, description=None):
        """Transfer funds to an email address.
        
        Args:
            from_wallet_id (str): Source wallet ID
            email (str): Recipient email address
            amount (float): Amount to transfer
            destination_currency (str): Currency to convert to
            description (str, optional): Transfer description
            
        Returns:
            dict: Transfer details
        """
        url = f"{self.base_url}/multicurrency-wallets/transfer"
        payload = {
            "fromWallet": from_wallet_id,
            "email": email,
            "amount": amount,
            "destinationCurrency": destination_currency
        }
        
        if description:
            payload["description"] = description
        
        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def transfer_to_phone(self, from_wallet_id, phone_number, amount, 
                         destination_currency, description=None):
        """Transfer funds to a phone number.
        
        Args:
            from_wallet_id (str): Source wallet ID
            phone_number (str): Recipient phone number (with country code)
            amount (float): Amount to transfer
            destination_currency (str): Currency to convert to
            description (str, optional): Transfer description
            
        Returns:
            dict: Transfer details
        """
        url = f"{self.base_url}/multicurrency-wallets/transfer"
        payload = {
            "fromWallet": from_wallet_id,
            "phoneNumber": phone_number,
            "amount": amount,
            "destinationCurrency": destination_currency
        }
        
        if description:
            payload["description"] = description
        
        response = requests.post(url, headers=self.headers, json=payload)
        response.raise_for_status()
        
        return response.json().get("data")
    
    def check_transfer_status(self, transfer_id):
        """Check the status of a transfer.
        
        Args:
            transfer_id (str): ID of the transfer to check
            
        Returns:
            dict: Transfer status details
        """
        url = f"{self.base_url}/multicurrency-wallets/transfer/status"
        params = {"id": transfer_id}
        
        response = requests.get(url, headers=self.headers, params=params)
        response.raise_for_status()
        
        return response.json().get("data")


# Example usage
if __name__ == "__main__":
    # Initialize the client (use your API key)
    chimoney = ChimoneyWalletAPI(api_key="YOUR_API_KEY")
    
    try:
        # Create wallets
        print("Creating USD wallet...")
        usd_wallet = chimoney.create_wallet("USD Wallet", "USD")
        print(f"USD wallet created with ID: {usd_wallet['id']}")
        
        print("Creating NGN wallet...")
        ngn_wallet = chimoney.create_wallet("NGN Wallet", "NGN")
        print(f"NGN wallet created with ID: {ngn_wallet['id']}")
        
        # List wallets
        print("\nListing wallets:")
        wallets = chimoney.list_wallets()
        for wallet in wallets:
            print(f"{wallet['name']}: {wallet['balance']} {wallet['currency']} (ID: {wallet['id']})")
        
        # Get a transfer quote
        print("\nGetting transfer quote...")
        amount = 100  # USD
        quote = chimoney.get_transfer_quote(usd_wallet['id'], amount, "NGN")
        print(f"Exchange rate: 1 USD = {quote['exchangeRate']} NGN")
        print(f"{amount} USD = {quote['destinationAmount']} NGN")
        print(f"Fee: {quote['fee']} USD")
        print(f"Total debit: {quote['totalDebit']} USD")
        
        # Execute a transfer
        print("\nExecuting transfer...")
        transfer = chimoney.transfer_between_wallets(
            usd_wallet['id'],
            ngn_wallet['id'],
            amount,
            "NGN",
            "Cross-currency transfer demo"
        )
        
        print(f"Transfer successful! ID: {transfer['id']}")
        print(f"Amount sent: {transfer['amount']} {transfer['sourceCurrency']}")
        print(f"Amount received: {transfer['destinationAmount']} {transfer['destinationCurrency']}")
        
        # Check updated balances
        print("\nChecking updated balances...")
        updated_usd_wallet = chimoney.get_wallet(usd_wallet['id'])
        updated_ngn_wallet = chimoney.get_wallet(ngn_wallet['id'])
        
        print(f"New USD Wallet balance: {updated_usd_wallet['balance']} USD")
        print(f"New NGN Wallet balance: {updated_ngn_wallet['balance']} NGN")
        
    except requests.exceptions.HTTPError as e:
        print(f"HTTP Error: {e}")
        if e.response is not None:
            print(f"Response: {e.response.text}")
    except Exception as e:
        print(f"Error: {e}")
```

This Python implementation provides the same functionality as the JavaScript examples, using a class-based approach for better organization and reusability.

## Enhanced Error Handling for Multi-Currency Operations

When working with multi-currency transfers, it's important to implement robust error handling that addresses the specific error types you might encounter. Chimoney's API provides detailed error information that can help you debug and handle issues gracefully.

### Error Types Specific to Multi-Currency Operations

```javascript
// Enhanced error handling function for multi-currency operations
function handleMultiCurrencyError(error) {
  if (!error.response || !error.response.data) {
    console.error('Network error or unexpected error format:', error.message);
    return;
  }
  
  const errorData = error.response.data;
  const statusCode = error.response.status;
  const errorType = errorData.error?.type;
  const errorMessage = errorData.error?.message || 'Unknown error';
  
  switch (errorType) {
    case 'MULTICURRENCY_ERROR':
      console.error(`Multi-currency operation failed: ${errorMessage}`);
      // Handle currency conversion issues
      break;
      
    case 'WALLET_ERROR':
      console.error(`Wallet operation failed: ${errorMessage}`);
      if (errorMessage.includes('insufficient funds')) {
        // Handle insufficient funds specifically
        showInsufficientFundsMessage();
      } else if (errorMessage.includes('wallet not found')) {
        // Handle invalid wallet ID
        promptForWalletSelection();
      }
      break;
      
    case 'CURRENCY_ERROR':
      console.error(`Currency error: ${errorMessage}`);
      // Handle unsupported currency pairs or exchange rate issues
      showSupportedCurrencies();
      break;
      
    case 'VALIDATION_ERROR':
      console.error(`Validation failed: ${errorMessage}`);
      // Parse validation errors and highlight form fields
      highlightInvalidFields(errorData.error.details);
      break;
      
    default:
      if (statusCode === 429) {
        // Rate limit exceeded
        console.error('Rate limit exceeded. Implementing backoff...');
        implementBackoff();
      } else if (statusCode >= 500) {
        // Server error
        console.error('Server error. The request might have been processed partially or not at all.');
        checkTransactionStatus();
      } else {
        console.error(`Error (${statusCode}): ${errorMessage}`);
      }
  }
}
```

### Implementing Idempotency for Safe Retries

For operations like transfers that should not be duplicated, use idempotency keys:

```javascript
// Generate a unique idempotency key
function generateIdempotencyKey() {
  return `transfer-${Date.now()}-${Math.random().toString(36).substring(2, 15)}`;
}

// Modified transfer function with idempotency support
async function transferBetweenWalletsWithIdempotency(fromWalletId, toWalletId, amount, destinationCurrency, description) {
  const idempotencyKey = generateIdempotencyKey();
  
  try {
    const response = await axios.post(
      `${BASE_URL}/multicurrency-wallets/transfer`,
      {
        fromWallet: fromWalletId,
        toWallet: toWalletId,
        amount: amount,
        destinationCurrency: destinationCurrency,
        description: description || 'Wallet to wallet transfer'
      },
      {
        headers: {
          'Authorization': `Bearer ${API_KEY}`,
          'Content-Type': 'application/json',
          'Idempotency-Key': idempotencyKey
        }
      }
    );
    
    console.log('Transfer successful:', response.data);
    return response.data.data;
  } catch (error) {
    handleMultiCurrencyError(error);
    throw error;
  }
}
```

### Retry Strategy with Exponential Backoff

For handling transient errors or rate limits:

```javascript
// Retry function with exponential backoff
async function retryWithBackoff(fn, maxRetries = 3, initialDelay = 1000) {
  let retries = 0;
  
  while (retries < maxRetries) {
    try {
      return await fn();
    } catch (error) {
      // Only retry for certain error types (429 rate limit, or 5xx server errors)
      if (error.response && (error.response.status === 429 || error.response.status >= 500)) {
        retries++;
        
        if (retries >= maxRetries) {
          console.error(`Maximum retries (${maxRetries}) exceeded. Giving up.`);
          throw error;
        }
        
        // Calculate delay with exponential backoff and jitter
        const delay = initialDelay * Math.pow(2, retries - 1) * (1 + Math.random() * 0.1);
        console.log(`Retry ${retries}/${maxRetries} after ${Math.round(delay)}ms delay`);
        
        await new Promise(resolve => setTimeout(resolve, delay));
      } else {
        // Don't retry for other error types
        throw error;
      }
    }
  }
}

// Example usage with a transfer function
async function reliableTransfer(fromWalletId, toWalletId, amount, destinationCurrency, description) {
  return retryWithBackoff(
    () => transferBetweenWalletsWithIdempotency(
      fromWalletId, toWalletId, amount, destinationCurrency, description
    )
  );
}
```

By implementing these error handling patterns, your integration with the Multi-Currency Wallet Transfer API will be more resilient and provide a better experience for your users.

The API you've implemented is more than just code—it's a tool for global financial inclusion. As you continue to refine your implementation, remember that each improvement enhances someone's ability to participate in the global economy with less friction and more freedom.

May your wallets always be funded, your transfers always successful, and your users always delighted with the seamless experience you've created!

✓ **Ultimate Achievement Unlocked:** You've Implemented Multi-Currency Wallet Transfers with Chimoney API that works globally in seconds rather than days!