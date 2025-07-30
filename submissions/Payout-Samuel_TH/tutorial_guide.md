# Tutorial: Implementing Interledger Payouts with Chimoney API

## Overview

This tutorial will guide you through implementing Chimoney's Payout to Interledger wallet address endpoint. You'll learn how to send instant, cross-border payments using Interledger payment pointers, eliminating traditional banking complexities.

**What you'll build:** A payment system that can send money globally using simple payment pointers like `$recipient.chimoney.io`.

**Time required:** 30-45 minutes

**Prerequisites:**
- Basic knowledge of HTTP APIs and JSON
- Node.js installed (v14 or higher)
- A code editor
- Completed Chimoney API setup (see setup.md)

## Understanding Interledger Payment Pointers

Before diving into code, let's understand payment pointers. They work like email addresses for money:

- **Traditional banking**: "Send $100 to Account: 1234567890, Routing: 021000021, Swift: CHASUS33, Bank: Chase Manhattan..."
- **Interledger**: "Send $100 to `$sarah.chimoney.io`"

Payment pointers are:
- **Universal**: Work across different payment providers
- **Simple**: Human-readable addresses
- **Secure**: Built-in encryption and validation
- **Instant**: Real-time settlement through the Interledger network

## Step 1: Project Setup

Create a new project directory and install dependencies:

```bash
mkdir chimoney-interledger-tutorial
cd chimoney-interledger-tutorial
npm init -y
npm install axios dotenv
```

Create your project structure:
```
chimoney-interledger-tutorial/
├── .env
├── .env.example
├── index.js
├── utils/
│   ├── validatePaymentPointer.js
│   └── chimoney-client.js
└── examples/
    ├── single-payout.js
    ├── bulk-payouts.js
    └── payment-status.js
```

## Step 2: Environment Configuration

Create `.env.example`:
```bash
# Chimoney API Configuration
CHIMONEY_API_KEY=your_api_key_here
CHIMONEY_BASE_URL=https://api-v2-sandbox.chimoney.io
ENVIRONMENT=sandbox

# Optional: Webhook configuration
WEBHOOK_URL=https://your-app.com/webhooks/chimoney
```

Copy to `.env` and add your actual API key:
```bash
cp .env.example .env
# Edit .env with your real API key
```

## Step 3: Create the Chimoney Client

Create `utils/chimoney-client.js`:

```javascript
const axios = require('axios');
require('dotenv').config();

class ChimoneyClient {
  constructor() {
    this.baseURL = process.env.CHIMONEY_BASE_URL;
    this.apiKey = process.env.CHIMONEY_API_KEY;
    
    if (!this.apiKey) {
      throw new Error('CHIMONEY_API_KEY is required');
    }

    this.client = axios.create({
      baseURL: this.baseURL,
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'X-API-KEY': this.apiKey
      },
      timeout: 30000 // 30 seconds
    });

    // Add response interceptor for error handling
    this.client.interceptors.response.use(
      (response) => response,
      (error) => {
        console.error('API Error:', {
          status: error.response?.status,
          data: error.response?.data,
          message: error.message
        });
        throw error;
      }
    );
  }

  /**
   * Send payout to Interledger wallet address
   * @param {Object} payoutData - Payout configuration
   * @returns {Promise<Object>} API response
   */
  async sendInterledgerPayout(payoutData) {
    try {
      const response = await this.client.post('/v0.2/payouts/interledger', payoutData);
      return response.data;
    } catch (error) {
      throw new Error(`Interledger payout failed: ${error.message}`);
    }
  }

  /**
   * Get payout status by transaction ID
   * @param {string} transactionId - Transaction identifier
   * @returns {Promise<Object>} Transaction status
   */
  async getPayoutStatus(transactionId) {
    try {
      const response = await this.client.get(`/v0.2/payouts/status/${transactionId}`);
      return response.data;
    } catch (error) {
      throw new Error(`Failed to get payout status: ${error.message}`);
    }
  }

  /**
   * Validate Interledger payment pointer
   * @param {string} paymentPointer - Payment pointer to validate
   * @returns {Promise<Object>} Validation result
   */
  async validatePaymentPointer(paymentPointer) {
    try {
      const response = await this.client.post('/v0.2/wallets/validate-pointer', {
        paymentPointer
      });
      return response.data;
    } catch (error) {
      throw new Error(`Payment pointer validation failed: ${error.message}`);
    }
  }
}

module.exports = ChimoneyClient;
```

## Step 4: Payment Pointer Validation Utility

Create `utils/validatePaymentPointer.js`:

```javascript
/**
 * Validate Interledger payment pointer format
 * @param {string} pointer - Payment pointer to validate
 * @returns {Object} Validation result
 */
function validatePaymentPointerFormat(pointer) {
  // Payment pointer format: $domain.com/path or $domain.com
  const paymentPointerRegex = /^\$[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}(\/[a-zA-Z0-9._~!$&'()*+,;=:@-]*)*$/;
  
  const isValid = paymentPointerRegex.test(pointer);
  
  return {
    isValid,
    pointer,
    errors: isValid ? [] : ['Invalid payment pointer format. Must start with $ followed by a valid domain']
  };
}

/**
 * Extract domain from payment pointer
 * @param {string} pointer - Payment pointer
 * @returns {string} Domain name
 */
function extractDomain(pointer) {
  if (!pointer.startsWith('$')) return null;
  
  const withoutDollar = pointer.substring(1);
  const domainPart = withoutDollar.split('/')[0];
  return domainPart;
}

/**
 * Check if payment pointer is a Chimoney address
 * @param {string} pointer - Payment pointer
 * @returns {boolean} True if Chimoney address
 */
function isChimoneyAddress(pointer) {
  const domain = extractDomain(pointer);
  return domain && domain.includes('chimoney.io');
}

module.exports = {
  validatePaymentPointerFormat,
  extractDomain,
  isChimoneyAddress
};
```

## Step 5: Single Payout Implementation

Create `examples/single-payout.js`:

```javascript
const ChimoneyClient = require('../utils/chimoney-client');
const { validatePaymentPointerFormat } = require('../utils/validatePaymentPointer');

async function sendSinglePayout() {
  const chimoney = new ChimoneyClient();

  // Payout configuration
  const payoutData = {
    // Recipient details
    paymentPointer: '$sarah.chimoney.io',
    
    // Amount and currency
    amount: 150.00,
    currency: 'USD',
    
    // Transaction details
    reference: 'PROJ-2024-LOGO-001',
    description: 'Logo design project payment',
    
    // Optional: Metadata for tracking
    metadata: {
      projectId: 'proj_001',
      clientId: 'client_123',
      invoiceNumber: 'INV-2024-001',
      category: 'design_services'
    },
    
    // Optional: Webhook for status updates
    webhookUrl: process.env.WEBHOOK_URL,
    
    // Optional: Email notifications
    notifyRecipient: true,
    customMessage: 'Thank you for the excellent logo design work!'
  };

  try {
    // Step 1: Validate payment pointer format
    console.log('🔍 Validating payment pointer...');
    const validation = validatePaymentPointerFormat(payoutData.paymentPointer);
    
    if (!validation.isValid) {
      throw new Error(`Invalid payment pointer: ${validation.errors.join(', ')}`);
    }
    console.log('✅ Payment pointer format is valid');

    // Step 2: Validate with Chimoney API (checks if recipient exists)
    console.log('🌐 Validating payment pointer with Chimoney...');
    const pointerValidation = await chimoney.validatePaymentPointer(payoutData.paymentPointer);
    
    if (!pointerValidation.valid) {
      throw new Error(`Payment pointer not found: ${pointerValidation.message}`);
    }
    console.log('✅ Payment pointer exists and is active');
    console.log(`📧 Recipient: ${pointerValidation.recipientInfo.name}`);

    // Step 3: Send the payout
    console.log('💸 Sending Interledger payout...');
    const payoutResponse = await chimoney.sendInterledgerPayout(payoutData);
    
    console.log('🎉 Payout sent successfully!');
    console.log('📊 Transaction Details:');
    console.log(`   Transaction ID: ${payoutResponse.transactionId}`);
    console.log(`   Status: ${payoutResponse.status}`);
    console.log(`   Amount: ${payoutResponse.amount} ${payoutResponse.currency}`);
    console.log(`   Recipient: ${payoutResponse.recipientPointer}`);
    console.log(`   Processing Fee: ${payoutResponse.processingFee} ${payoutResponse.currency}`);
    console.log(`   Exchange Rate: ${payoutResponse.exchangeRate || 'N/A'}`);
    console.log(`   Estimated Arrival: ${payoutResponse.estimatedArrival}`);

    // Step 4: Monitor transaction status
    console.log('⏳ Monitoring transaction status...');
    await monitorTransactionStatus(chimoney, payoutResponse.transactionId);

  } catch (error) {
    console.error('❌ Payout failed:', error.message);
    
    // Handle specific error types
    if (error.response?.status === 400) {
      console.error('📝 Validation Error Details:', error.response.data);
    } else if (error.response?.status === 401) {
      console.error('🔑 Authentication failed. Check your API key.');
    } else if (error.response?.status === 429) {
      console.error('⚡ Rate limit exceeded. Please wait before retrying.');
    } else if (error.response?.status >= 500) {
      console.error('🔧 Server error. Please try again later.');
    }
  }
}

/**
 * Monitor transaction status until completion
 * @param {ChimoneyClient} chimoney - Chimoney client instance
 * @param {string} transactionId - Transaction ID to monitor
 */
async function monitorTransactionStatus(chimoney, transactionId) {
  const maxAttempts = 10;
  const pollInterval = 3000; // 3 seconds
  
  let attempts = 0;
  
  while (attempts < maxAttempts) {
    try {
      const status = await chimoney.getPayoutStatus(transactionId);
      
      console.log(`📊 Status Update (${attempts + 1}/${maxAttempts}): ${status.status}`);
      
      if (status.status === 'completed') {
        console.log('✅ Transaction completed successfully!');
        console.log(`💰 Final amount delivered: ${status.deliveredAmount} ${status.currency}`);
        console.log(`⏰ Completion time: ${status.completedAt}`);
        break;
      } else if (status.status === 'failed') {
        console.error('❌ Transaction failed:', status.failureReason);
        break;
      } else if (status.status === 'processing') {
        console.log('⏳ Transaction is being processed...');
      }
      
      attempts++;
      
      if (attempts < maxAttempts) {
        await new Promise(resolve => setTimeout(resolve, pollInterval));
      }
      
    } catch (error) {
      console.error('Error checking status:', error.message);
      break;
    }
  }
  
  if (attempts >= maxAttempts) {
    console.log('⏰ Status monitoring timeout. Check transaction manually.');
  }
}

// Run the example
if (require.main === module) {
  sendSinglePayout();
}

module.exports = { sendSinglePayout };
```

## Step 6: Bulk Payouts Implementation

Create `examples/bulk-payouts.js`:

```javascript
const ChimoneyClient = require('../utils/chimoney-client');
const { validatePaymentPointerFormat } = require('../utils/validatePaymentPointer');

async function sendBulkPayouts() {
  const chimoney = new ChimoneyClient();

  // Multiple recipients for bulk payout
  const recipients = [
    {
      paymentPointer: '$alice.chimoney.io',
      amount: 250.00,
      currency: 'USD',
      reference: 'FREELANCE-PROJ-001',
      description: 'Web development project - Phase 1',
      metadata: { projectPhase: 1, skillType: 'frontend' }
    },
    {
      paymentPointer: '$bob.chimoney.io',
      amount: 180.00,
      currency: 'USD',
      reference: 'FREELANCE-PROJ-002',
      description: 'UI/UX design consultation',
      metadata: { projectPhase: 1, skillType: 'design' }
    },
    {
      paymentPointer: '$charlie.chimoney.io',
      amount: 320.00,
      currency: 'USD',
      reference: 'FREELANCE-PROJ-003',
      description: 'Backend API development',
      metadata: { projectPhase: 1, skillType: 'backend' }
    }
  ];

  const bulkPayoutData = {
    recipients,
    batchReference: `BATCH-${Date.now()}`,
    description: 'Monthly freelancer payments - January 2024',
    notifyRecipients: true,
    webhookUrl: process.env.WEBHOOK_URL,
    metadata: {
      payrollPeriod: '2024-01',
      department: 'engineering',
      approvedBy: 'manager@company.com'
    }
  };

  try {
    console.log(`💼 Processing bulk payout for ${recipients.length} recipients...`);

    // Step 1: Validate all payment pointers
    console.log('🔍 Validating payment pointers...');
    const validationPromises = recipients.map(async (recipient, index) => {
      const validation = validatePaymentPointerFormat(recipient.paymentPointer);
      if (!validation.isValid) {
        throw new Error(`Recipient ${index + 1}: ${validation.errors.join(', ')}`);
      }
      
      // Validate with API
      const apiValidation = await chimoney.validatePaymentPointer(recipient.paymentPointer);
      if (!apiValidation.valid) {
        throw new Error(`Recipient ${index + 1}: Payment pointer not found`);
      }
      
      return { index, valid: true, recipientInfo: apiValidation.recipientInfo };
    });

    const validationResults = await Promise.all(validationPromises);
    console.log('✅ All payment pointers validated successfully');

    // Step 2: Calculate totals
    const totalAmount = recipients.reduce((sum, recipient) => sum + recipient.amount, 0);
    const estimatedFees = totalAmount * 0.015; // ~1.5% estimated fees
    
    console.log('📊 Bulk Payout Summary:');
    console.log(`   Recipients: ${recipients.length}`);
    console.log(`   Total Amount: $${totalAmount.toFixed(2)} USD`);
    console.log(`   Estimated Fees: $${estimatedFees.toFixed(2)} USD`);
    console.log(`   Estimated Total Cost: $${(totalAmount + estimatedFees).toFixed(2)} USD`);

    // Step 3: Send bulk payout
    console.log('💸 Sending bulk payout...');
    const bulkResponse = await chimoney.sendBulkInterledgerPayout(bulkPayoutData);
    
    console.log('🎉 Bulk payout initiated successfully!');
    console.log(`📋 Batch ID: ${bulkResponse.batchId}`);
    console.log(`📊 Status: ${bulkResponse.status}`);
    console.log(`💰 Total Amount: $${bulkResponse.totalAmount} ${bulkResponse.currency}`);
    console.log(`💸 Processing Fee: $${bulkResponse.totalFees} ${bulkResponse.currency}`);

    // Step 4: Monitor individual transactions
    console.log('⏳ Monitoring individual transaction statuses...');
    const transactionIds = bulkResponse.transactions.map(t => t.transactionId);
    
    await monitorBulkTransactionStatus(chimoney, transactionIds, bulkResponse.batchId);

  } catch (error) {
    console.error('❌ Bulk payout failed:', error.message);
    
    if (error.response?.data?.validationErrors) {
      console.error('📝 Validation Errors:');
      error.response.data.validationErrors.forEach((err, index) => {
        console.error(`   Recipient ${index + 1}: ${err.message}`);
      });
    }
  }
}

/**
 * Monitor bulk transaction statuses
 * @param {ChimoneyClient} chimoney - Chimoney client instance
 * @param {string[]} transactionIds - Array of transaction IDs
 * @param {string} batchId - Batch identifier
 */
async function monitorBulkTransactionStatus(chimoney, transactionIds, batchId) {
  console.log(`📊 Monitoring ${transactionIds.length} transactions...`);
  
  const maxAttempts = 15;
  const pollInterval = 5000; // 5 seconds for bulk operations
  
  let attempts = 0;
  let completedCount = 0;
  
  while (attempts < maxAttempts && completedCount < transactionIds.length) {
    try {
      const statusPromises = transactionIds.map(id => 
        chimoney.getPayoutStatus(id).catch(err => ({ 
          transactionId: id, 
          status: 'error', 
          error: err.message 
        }))
      );
      
      const statuses = await Promise.all(statusPromises);
      
      // Count completed transactions
      const newCompletedCount = statuses.filter(s => 
        s.status === 'completed' || s.status === 'failed'
      ).length;
      
      if (newCompletedCount > completedCount) {
        completedCount = newCompletedCount;
        console.log(`📈 Progress: ${completedCount}/${transactionIds.length} transactions completed`);
      }
      
      // Show status summary
      const statusSummary = statuses.reduce((acc, status) => {
        acc[status.status] = (acc[status.status] || 0) + 1;
        return acc;
      }, {});
      
      console.log(`📊 Status Summary (Attempt ${attempts + 1}):`, statusSummary);
      
      // Check if all completed
      if (completedCount === transactionIds.length) {
        console.log('✅ All transactions completed!');
        
        const successful = statuses.filter(s => s.status === 'completed').length;
        const failed = statuses.filter(s => s.status === 'failed').length;
        
        console.log(`✅ Successful: ${successful}`);
        console.log(`❌ Failed: ${failed}`);
        
        if (failed > 0) {
          console.log('❌ Failed transactions:');
          statuses.filter(s => s.status === 'failed').forEach(s => {
            console.log(`   ${s.transactionId}: ${s.failureReason}`);
          });
        }
        break;
      }
      
      attempts++;
      
      if (attempts < maxAttempts) {
        await new Promise(resolve => setTimeout(resolve, pollInterval));
      }
      
    } catch (error) {
      console.error('Error monitoring bulk status:', error.message);
      break;
    }
  }
  
  if (attempts >= maxAttempts) {
    console.log('⏰ Bulk monitoring timeout. Check remaining transactions manually.');
  }
}

// Run the example
if (require.main === module) {
  sendBulkPayouts();
}

module.exports = { sendBulkPayouts };
```

## Step 7: Error Handling and Best Practices

Create a comprehensive error handling example in `examples/error-handling.js`:

```javascript
const ChimoneyClient = require('../utils/chimoney-client');

/**
 * Demonstrate comprehensive error handling
 */
async function demonstrateErrorHandling() {
  const chimoney = new ChimoneyClient();
  
  const testCases = [
    {
      name: 'Invalid Payment Pointer Format',
      data: { paymentPointer: 'invalid-pointer', amount: 100, currency: 'USD' }
    },
    {
      name: 'Non-existent Payment Pointer',
      data: { paymentPointer: '$nonexistent.chimoney.io', amount: 100, currency: 'USD' }
    },
    {
      name: 'Invalid Amount',
      data: { paymentPointer: '$test.chimoney.io', amount: -50, currency: 'USD' }
    },
    {
      name: 'Unsupported Currency',
      data: { paymentPointer: '$test.chimoney.io', amount: 100, currency: 'INVALID' }
    }
  ];
  
  for (const testCase of testCases) {
    console.log(`\n🧪 Testing: ${testCase.name}`);
    
    try {
      await chimoney.sendInterledgerPayout(testCase.data);
      console.log('❌ Expected error but request succeeded');
    } catch (error) {
      console.log('✅ Error handled correctly:');
      console.log(`   Type: ${error.constructor.name}`);
      console.log(`   Message: ${error.message}`);
      
      if (error.response) {
        console.log(`   Status: ${error.response.status}`);
        console.log(`   Error Code: ${error.response.data?.errorCode || 'N/A'}`);
        console.log(`   Details: ${JSON.stringify(error.response.data?.details || {})}`);
      }
    }
  }
}

if (require.main === module) {
  demonstrateErrorHandling();
}
```

## Step 8: Testing Your Implementation

Create `test-runner.js`:

```javascript
const { sendSinglePayout } = require('./examples/single-payout');
const { sendBulkPayouts } = require('./examples/bulk-payouts');

async function runTests() {
  console.log('🚀 Starting Chimoney Interledger Payout Tests\n');
  
  try {
    // Test 1: Single payout
    console.log('=== TEST 1: Single Payout ===');
    await sendSinglePayout();
    console.log('\n✅ Single payout test completed\n');
    
    // Wait between tests
    await new Promise(resolve => setTimeout(resolve, 2000));
    
    // Test 2: Bulk payouts
    console.log('=== TEST 2: Bulk Payouts ===');
    await sendBulkPayouts();
    console.log('\n✅ Bulk payout test completed');
    
  } catch (error) {
    console.error('❌ Test suite failed:', error.message);
  }
}

runTests();
```

## Step 9: Production Considerations

### Rate Limiting
```javascript
// Add rate limiting to your client
const rateLimit = require('express-rate-limit');

const payoutLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many payout requests, please try again later.'
});
```

### Webhook Handling
```javascript
// Handle Chimoney webhooks for real-time status updates
app.post('/webhooks/chimoney', express.json(), (req, res) => {
  const { transactionId, status, amount, currency } = req.body;
  
  console.log(`Transaction ${transactionId}: ${status}`);
  
  // Update your database
  updateTransactionStatus(transactionId, status);
  
  res.status(200).json({ received: true });
});
```

### Monitoring and Logging
```javascript
// Add comprehensive logging
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'payouts.log' }),
    new winston.transports.Console()
  ]
});

// Log all payout attempts
logger.info('Payout initiated', {
  transactionId,
  amount,
  currency,
  recipient: paymentPointer
});
```

## Next Steps

Congratulations! You've successfully implemented Chimoney's Interledger payout functionality. Here's what to explore next:

1. **Set up webhooks** for real-time transaction updates
2. **Implement idempotency** to prevent duplicate payments
3. **Add retry logic** for failed transactions
4. **Build a dashboard** to monitor payment statuses
5. **Integrate with your user management system**

## Troubleshooting

**Common Issues:**

- **401 Unauthorized**: Check your API key in `.env`
- **400 Bad Request**: Validate payment pointer format
- **429 Rate Limited**: Implement exponential backoff
- **Network timeouts**: Increase axios timeout settings

**Getting Help:**
- Check the [Chimoney API Documentation](https://chimoney.readme.io)
- Join the [Chimoney Developer Community](https://discord.gg/chimoney)
- Email support: support@chimoney.io

You're now ready to revolutionize your payment infrastructure with instant, global Interledger payouts!