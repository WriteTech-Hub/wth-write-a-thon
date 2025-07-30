# Chimoney API Setup Guide: Authentication & Environment Configuration

## Overview

This comprehensive guide will walk you through setting up your development environment to use Chimoney's Interledger payout capabilities. By the end of this guide, you'll have everything configured to make your first API call successfully.

**Time required:** 15-20 minutes  
**Prerequisites:** Basic familiarity with APIs and command line tools

## Step 1: Create Your Chimoney Developer Account

### Sign Up for Sandbox Access

1. **Visit the Chimoney Sandbox**: Navigate to [sandbox.chimoney.io](https://sandbox.chimoney.io)

2. **Create your account**: Click "Sign Up" and provide:
   - Full name
   - Email address
   - Strong password
   - Phone number (for 2FA)

3. **Verify your email**: Check your inbox and click the verification link

4. **Complete your profile**: Add any additional required information

### Explore Your Sandbox Dashboard

Upon login, you'll see:

- **$1,000 USD** in your flexible balance (sandbox money for testing)
- **$500** in mobile money balance
- **$200** in airtime balance
- Transaction history panel
- Navigation menu with API settings

> **Important**: Sandbox funds are for testing only and cannot be converted to real money.

## Step 2: Generate Your API Key

### Access API Settings

1. **Navigate to API Settings**: In your dashboard, click on "Developers" or "API" in the sidebar menu

2. **Create an Organization** (if required):
   - Click "Create New Organization"
   - Organization name: `My Company API`
   - Description: `Development and testing organization`
   - Industry: Select your industry
   - Click "Create"

3. **Generate API Key**:
   - Click "Generate New API Key"
   - Key name: `Interledger Payouts - Development`
   - Permissions: Select "Payouts" and "Wallets"
   - Environment: "Sandbox"
   - Click "Generate Key"

### Secure Your API Key

Your API key will look like this:
```
sk_sandbox_1234567890abcdef1234567890abcdef12345678
```

**Security Best Practices:**
- ✅ Copy the key immediately (it's only shown once)
- ✅ Store it securely in environment variables
- ✅ Never commit it to version control
- ✅ Use different keys for development and production
- ❌ Never share it in emails or chat messages
- ❌ Don't hardcode it in your application

## Step 3: Environment Setup

### Install Required Tools

**Node.js** (v14 or higher):
```bash
# Check if Node.js is installed
node --version

# If not installed, download from nodejs.org
# Or use a version manager like nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install node
nvm use node
```

**npm or yarn** (package manager):
```bash
# npm comes with Node.js
npm --version

# Or install yarn
npm install -g yarn
```

**curl** (for testing API calls):
```bash
# Check if curl is installed
curl --version

# Usually pre-installed on macOS/Linux
# Windows: Download from curl.se or use Git Bash
```

### Create Your Project

```bash
# Create project directory
mkdir chimoney-interledger-setup
cd chimoney-interledger-setup

# Initialize Node.js project
npm init -y

# Install essential dependencies
npm install axios dotenv

# Install development dependencies
npm install --save-dev nodemon jest
```

### Configure Environment Variables

Create `.env.example` (template for other developers):
```bash
# Chimoney API Configuration
CHIMONEY_API_KEY=your_api_key_here
CHIMONEY_BASE_URL=https://api-v2-sandbox.chimoney.io
ENVIRONMENT=sandbox

# Your Application Settings
APP_NAME=My Interledger App
APP_VERSION=1.0.0
LOG_LEVEL=info

# Optional: Webhook Configuration
WEBHOOK_URL=https://your-app.com/webhooks/chimoney
WEBHOOK_SECRET=your_webhook_secret_here

# Optional: Database Configuration (for production)
DATABASE_URL=your_database_connection_string
```

Create your actual `.env` file:
```bash
# Copy the template
cp .env.example .env

# Edit with your real values
nano .env  # or use your preferred editor
```

Your `.env` should look like:
```bash
CHIMONEY_API_KEY=sk_sandbox_1234567890abcdef1234567890abcdef12345678
CHIMONEY_BASE_URL=https://api-v2-sandbox.chimoney.io
ENVIRONMENT=sandbox
APP_NAME=My Interledger App
APP_VERSION=1.0.0
LOG_LEVEL=info
```

### Add .gitignore

Create `.gitignore` to protect sensitive information:
```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Logs
logs/
*.log

# Runtime data
pids/
*.pid
*.seed
*.pid.lock

# Coverage directory used by tools like istanbul
coverage/

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db
```

## Step 4: Test Your API Connection

### Create a Simple Test Script

Create `test-connection.js`:

```javascript
require('dotenv').config();
const axios = require('axios');

async function testConnection() {
  console.log('🚀 Testing Chimoney API Connection...\n');

  // Validate environment variables
  const apiKey = process.env.CHIMONEY_API_KEY;
  const baseURL = process.env.CHIMONEY_BASE_URL;

  if (!apiKey) {
    console.error('❌ CHIMONEY_API_KEY not found in environment variables');
    process.exit(1);
  }

  if (!baseURL) {
    console.error('❌ CHIMONEY_BASE_URL not found in environment variables');
    process.exit(1);
  }

  console.log('✅ Environment variables loaded');
  console.log(`📡 Base URL: ${baseURL}`);
  console.log(`🔑 API Key: ${apiKey.substring(0, 15)}...${apiKey.slice(-4)}`);

  // Test API connection
  try {
    console.log('\n🔗 Testing API connection...');

    const response = await axios.get(`${baseURL}/v0.2/info/ping`, {
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'X-API-KEY': apiKey
      },
      timeout: 10000
    });

    if (response.status === 200) {
      console.log('✅ API connection successful!');
      console.log(`📊 Response: ${JSON.stringify(response.data, null, 2)}`);
    }

  } catch (error) {
    console.error('❌ API connection failed:');
    
    if (error.response) {
      console.error(`   Status: ${error.response.status}`);
      console.error(`   Message: ${error.response.data?.message || error.message}`);
      
      if (error.response.status === 401) {
        console.error('🔑 Authentication failed. Please check your API key.');
      } else if (error.response.status === 403) {
        console.error('🚫 Access forbidden. Check your API key permissions.');
      }
    } else if (error.code === 'ECONNREFUSED') {
      console.error('🌐 Connection refused. Check your internet connection.');
    } else {
      console.error(`   Error: ${error.message}`);
    }
    
    process.exit(1);
  }

  // Test account information
  try {
    console.log('\n👤 Testing account access...');

    const accountResponse = await axios.get(`${baseURL}/v0.2/accounts/transactions`, {
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'X-API-KEY': apiKey
      },
      timeout: 10000
    });

    console.log('✅ Account access successful!');
    console.log(`💰 Account balance: ${JSON.stringify(accountResponse.data.balance, null, 2)}`);

  } catch (error) {
    console.error('❌ Account access failed:', error.response?.data?.message || error.message);
  }

  console.log('\n🎉 Setup test completed successfully!');
  console.log('📚 You\'re ready to proceed with the tutorial guide.');
}

// Run the test
testConnection().catch(console.error);
```

### Run the Connection Test

```bash
# Execute the test
node test-connection.js
```

**Expected successful output:**
```
🚀 Testing Chimoney API Connection...

✅ Environment variables loaded
📡 Base URL: https://api-v2-sandbox.chimoney.io
🔑 API Key: sk_sandbox_123...5678

🔗 Testing API connection...
✅ API connection successful!
📊 Response: {
  "status": "success",
  "message": "Chimoney API is running",
  "timestamp": "2024-01-15T10:30:00Z"
}

👤 Testing account access...
✅ Account access successful!
💰 Account balance: {
  "USD": 1000.00,
  "CAD": 0.00,
  "NGN": 0.00
}

🎉 Setup test completed successfully!
📚 You're ready to proceed with the tutorial guide.
```

## Step 5: Explore Available Endpoints

### Create an Endpoint Explorer

Create `explore-endpoints.js`:

```javascript
require('dotenv').config();
const axios = require('axios');

async function exploreEndpoints() {
  const apiKey = process.env.CHIMONEY_API_KEY;
  const baseURL = process.env.CHIMONEY_BASE_URL;

  const endpoints = [
    {
      name: 'API Ping',
      method: 'GET',
      url: '/v0.2/info/ping',
      description: 'Test API connectivity'
    },
    {
      name: 'Supported Countries',
      method: 'GET', 
      url: '/v0.2/info/countries',
      description: 'List of supported countries for payouts'
    },
    {
      name: 'Exchange Rates',
      method: 'GET',
      url: '/v0.2/info/exchange-rates',
      description: 'Current exchange rates'
    },
    {
      name: 'Account Balance',
      method: 'GET',
      url: '/v0.2/accounts/balance',
      description: 'Your current account balance'
    },
    {
      name: 'Transaction History',
      method: 'GET',
      url: '/v0.2/accounts/transactions',
      description: 'Recent transaction history'
    }
  ];

  console.log('🔍 Exploring Chimoney API Endpoints...\n');

  for (const endpoint of endpoints) {
    try {
      console.log(`📡 Testing: ${endpoint.name}`);
      console.log(`   ${endpoint.method} ${endpoint.url}`);
      console.log(`   Description: ${endpoint.description}`);

      const response = await axios({
        method: endpoint.method,
        url: `${baseURL}${endpoint.url}`,
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          'X-API-KEY': apiKey
        },
        timeout: 10000
      });

      console.log(`   ✅ Status: ${response.status}`);
      console.log(`   📊 Response: ${JSON.stringify(response.data, null, 2).substring(0, 200)}...`);
      console.log();

    } catch (error) {
      console.log(`   ❌ Error: ${error.response?.status || error.message}`);
      console.log();
    }
  }

  console.log('🎯 Key endpoints are working! Ready for Interledger payouts.');
}

exploreEndpoints().catch(console.error);
```

### Test Different Environments

Create `environment-test.js`:

```javascript
require('dotenv').config();

function validateEnvironment() {
  console.log('🔧 Environment Configuration Check\n');

  const config = {
    'API Key': process.env.CHIMONEY_API_KEY,
    'Base URL': process.env.CHIMONEY_BASE_URL,
    'Environment': process.env.ENVIRONMENT,
    'App Name': process.env.APP_NAME,
    'Log Level': process.env.LOG_LEVEL
  };

  let allValid = true;

  Object.entries(config).forEach(([key, value]) => {
    if (value) {
      console.log(`✅ ${key}: ${key === 'API Key' ? value.substring(0, 15) + '...' : value}`);
    } else {
      console.log(`❌ ${key}: Not set`);
      allValid = false;
    }
  });

  // Environment-specific validations
  console.log('\n🔍 Environment-Specific Checks:');

  if (process.env.ENVIRONMENT === 'sandbox') {
    console.log('✅ Sandbox mode: Safe for testing');
    console.log('💰 Using test funds: No real money at risk');
  } else if (process.env.ENVIRONMENT === 'production') {
    console.log('⚠️  Production mode: Real money transactions');
    console.log('🔒 Ensure all security measures are in place');
  }

  // API Key validation
  const apiKey = process.env.CHIMONEY_API_KEY;
  if (apiKey) {
    if (apiKey.startsWith('sk_sandbox_')) {
      console.log('✅ API Key: Sandbox key detected');
    } else if (apiKey.startsWith('sk_live_')) {
      console.log('🔴 API Key: Live/Production key detected');
      console.log('⚠️  Warning: This will process real transactions');
    } else {
      console.log('❌ API Key: Invalid format');
      allValid = false;
    }
  }

  console.log(`\n${allValid ? '🎉' : '❌'} Environment ${allValid ? 'Ready' : 'Needs Attention'}`);
  return allValid;
}

if (require.main === module) {
  validateEnvironment();
}

module.exports = { validateEnvironment };
```

## Step 6: Set Up Development Tools

### Configure Package.json Scripts

Update your `package.json`:

```json
{
  "name": "chimoney-interledger-setup",
  "version": "1.0.0",
  "description": "Chimoney Interledger API setup and testing",
  "main": "index.js",
  "scripts": {
    "test": "jest",
    "test:connection": "node test-connection.js",
    "test:environment": "node environment-test.js",
    "explore": "node explore-endpoints.js",
    "dev": "nodemon index.js",
    "start": "node index.js",
    "lint": "eslint .",
    "format": "prettier --write ."
  },
  "keywords": ["chimoney", "interledger", "payments", "api"],
  "author": "Your Name",
  "license": "MIT",
  "dependencies": {
    "axios": "^1.6.0",
    "dotenv": "^16.3.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0",
    "jest": "^29.7.0",
    "eslint": "^8.50.0",
    "prettier": "^3.0.0"
  }
}
```

### Create Development Commands

Add these helpful npm scripts:

```bash
# Test your API connection
npm run test:connection

# Validate environment variables
npm run test:environment

# Explore available endpoints
npm run explore

# Start development server with auto-reload
npm run dev
```

### Set Up VS Code Configuration (Optional)

Create `.vscode/settings.json`:

```json
{
  "files.exclude": {
    "node_modules": true,
    ".env": false
  },
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  }
}
```

Create `.vscode/launch.json` for debugging:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Node.js",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/test-connection.js",
      "env": {
        "NODE_ENV": "development"
      },
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

## Step 7: Transition to Production

### Production Environment Setup

When ready for production:

1. **Create production API key**:
   - Log into [live.chimoney.io](https://live.chimoney.io)
   - Complete business verification (KYC/KYB)
   - Generate production API key

2. **Update environment variables**:
   ```bash
   CHIMONEY_API_KEY=sk_live_your_production_key
   CHIMONEY_BASE_URL=https://api.chimoney.io
   ENVIRONMENT=production
   ```

3. **Security checklist**:
   - ✅ Use HTTPS for all communications
   - ✅ Implement rate limiting
   - ✅ Set up webhook signature verification
   - ✅ Add request logging and monitoring
   - ✅ Implement proper error handling
   - ✅ Use environment-specific configurations

### Production Configuration Template

Create `.env.production`:

```bash
# Production Configuration
CHIMONEY_API_KEY=sk_live_your_production_key
CHIMONEY_BASE_URL=https://api.chimoney.io
ENVIRONMENT=production

# Security
WEBHOOK_SECRET=your_production_webhook_secret
API_RATE_LIMIT=1000
JWT_SECRET=your_jwt_secret

# Monitoring
LOG_LEVEL=error
SENTRY_DSN=your_sentry_dsn
METRICS_ENDPOINT=your_metrics_endpoint

# Database
DATABASE_URL=your_production_database_url
REDIS_URL=your_redis_url
```

## Troubleshooting Common Issues

### Authentication Problems

**Issue**: `401 Unauthorized`
```bash
# Check API key format
echo $CHIMONEY_API_KEY | head -c 15

# Verify in environment
node -e "console.log(process.env.CHIMONEY_API_KEY)"
```

**Solution**: Ensure API key is correctly set and has proper permissions.

### Network Issues

**Issue**: `ECONNREFUSED` or timeout errors
```bash
# Test network connectivity
curl -I https://api-v2-sandbox.chimoney.io/health

# Check DNS resolution
nslookup api-v2-sandbox.chimoney.io
```

**Solution**: Check firewall settings and internet connection.

### Environment Variable Problems

**Issue**: Variables not loading
```bash
# Verify .env file exists and is readable
ls -la .env
cat .env

# Test dotenv loading
node -e "require('dotenv').config(); console.log(process.env.CHIMONEY_API_KEY)"
```

**Solution**: Ensure `.env` is in the correct directory and properly formatted.

## Next Steps

🎉 **Congratulations!** Your Chimoney API environment is now fully configured and tested.

**You're ready to:**
1. 📚 Follow the [Tutorial Guide](tutorial.md) to implement Interledger payouts
2. 💡 Explore the [Use Case Examples](use-case.md) for inspiration
3. 🔗 Join the [Chimoney Developer Community](https://discord.gg/chimoney)
4. 📖 Browse the [Complete API Documentation](https://chimoney.readme.io)

**Quick validation checklist:**
- ✅ API key generated and secured
- ✅ Environment variables configured
- ✅ Connection test passed
- ✅ Development tools installed
- ✅ Ready for tutorial implementation

Your development environment is production-ready. Time to build something amazing with Interledger payouts! 🚀