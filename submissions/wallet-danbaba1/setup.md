# Complete Setup Guide: Chimoney API Authentication and Environment Configuration

This comprehensive setup guide will walk you through everything you need to get started with the Chimoney API, from account creation to making your first successful API call. By the end of this guide, you'll have a fully configured development environment ready to create Interledger wallet addresses.

## Table of Contents

1. [Account Setup](#account-setup)
2. [API Access Request](#api-access-request)
3. [Development Environment Setup](#development-environment-setup)
4. [Authentication Configuration](#authentication-configuration)
5. [Testing Your Setup](#testing-your-setup)
6. [Environment Variables](#environment-variables)
7. [Postman Collection](#postman-collection)
8. [Troubleshooting](#troubleshooting)
9. [Security Best Practices](#security-best-practices)
10. [Next Steps](#next-steps)

## Account Setup

### Step 1: Create Your Chimoney Account

**Visit the Chimoney Dashboard**
- Navigate to `dash.chimoney.io`
- Click "Sign Up" to create a new account

**Complete Registration**

Required Information:
- Full Name
- Email Address
- Phone Number
- Country
- Business/Organization Name (if applicable)
- Password (minimum 8 characters)

**Email Verification**
- Check your email for a verification link
- Click the link to verify your account
- You should now be able to log into the dashboard

**Initial Dashboard Exploration**
- Familiarize yourself with the dashboard layout
- Note the navigation menu and available sections
- Look for the "API" or "Developer" section

### Step 2: Profile Completion

**Complete Your Profile**
- Add your profile picture (optional)
- Fill in any missing business information
- Verify your phone number if prompted

**Security Settings**
- Enable two-factor authentication (2FA) - **Highly Recommended**
- Set up security questions
- Review login history

## API Access Request

### Step 3: Request API Access

> ⚠️ **Important**: API access requires verification and approval from Chimoney.

**Prepare Your Application**
- Document your use case clearly
- Prepare your website/application URL
- Have your business documentation ready

**Send Verification Request**

**To:** support@chimoney.io  
**Subject:** API Access Request - [Your Company/Project Name]

```
Dear Chimoney Team,

I am requesting API access for my [business/project] to integrate Chimoney's 
payment services. Here are the details:

Company/Project Name: [Your Name]
Website: [Your Website URL]
Use Case: [Describe your specific use case, e.g., "Creating Interledger 
wallet addresses for freelancers to receive international payments"]
Expected Volume: [Estimated monthly transaction volume]
Target Markets: [Countries/regions you plan to serve]

Additional Information:
- [Any relevant business licenses or registrations]
- [Previous experience with payment APIs]
- [Technical team size and expertise]

Thank you for your consideration.

Best regards,
[Your Name]
[Your Title]
[Your Contact Information]
```

**Alternative: Book a Demo**
- Visit Chimoney's demo booking page
- Schedule a call to discuss your use case
- This can expedite the approval process

### Step 4: Wait for Approval

- **Timeline**: Typically 1-3 business days
- **Follow-up**: If no response within 5 days, send a polite follow-up email
- **Approval Notification**: You'll receive an email confirming API access

## Development Environment Setup

### Step 5: Choose Your Development Stack

**Recommended Environments:**

#### Node.js Setup:

```bash
# Check Node.js version (recommended: 16+)
node --version

# If Node.js is not installed, visit https://nodejs.org/
# Create a new project directory
mkdir chimoney-integration
cd chimoney-integration

# Initialize npm project
npm init -y

# Install required dependencies
npm install axios dotenv
npm install --save-dev jest nodemon

# Create basic project structure
mkdir src
mkdir tests
mkdir config

# Create essential files
touch .env
touch .env.example
touch .gitignore
touch src/chimoney-client.js
touch src/wallet-service.js
touch tests/wallet.test.js
```

#### Python Setup:

```bash
# Check Python version (recommended: 3.8+)
python --version

# Create virtual environment
python -m venv chimoney-env

# Activate virtual environment
# On Windows:
chimoney-env\Scripts\activate
# On macOS/Linux:
source chimoney-env/bin/activate

# Install required packages
pip install requests python-dotenv pytest

# Create project structure
mkdir chimoney_integration
mkdir tests
mkdir config

# Create essential files
touch .env
touch .env.example
touch .gitignore
touch chimoney_integration/__init__.py
touch chimoney_integration/client.py
touch chimoney_integration/wallet_service.py
touch tests/test_wallet.py
touch requirements.txt
```

#### PHP Setup:

```bash
# Check PHP version (recommended: 8.0+)
php --version

# Install Composer (if not already installed)
# Visit https://getcomposer.org/

# Create project directory
mkdir chimoney-integration
cd chimoney-integration

# Initialize composer
composer init

# Install dependencies
composer require guzzlehttp/guzzle vlucas/phpdotenv
composer require --dev phpunit/phpunit

# Create project structure
mkdir src
mkdir tests
mkdir config

# Create essential files
touch .env
touch .env.example
touch .gitignore
touch src/ChimoneyClient.php
touch src/WalletService.php
touch tests/WalletTest.php
```

### Step 6: Set Up Your .gitignore File

Create a `.gitignore` file to protect sensitive information:

```gitignore
# Environment variables
.env
.env.local
.env.*.local

# Dependencies
node_modules/
vendor/
__pycache__/
*.pyc

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

# Log files
*.log
logs/

# Testing
coverage/
.nyc_output/
.pytest_cache/

# Build outputs
dist/
build/
```

## Authentication Configuration

### Step 7: Obtain Your API Key

Once your API access is approved:

**Log into Chimoney Dashboard**
- Go to `dash.chimoney.io`
- Navigate to the "API Keys" or "Developer" section

**Generate API Key**
- Click "Generate New API Key" or similar button
- Copy the generated key immediately
- Store it securely (you may not be able to view it again)

**API Key Format**

Your API key will look something like:
```
sk_live_1234567890abcdef1234567890abcdef
```
or
```
sk_test_1234567890abcdef1234567890abcdef
```

### Step 8: Configure Environment Variables

Create your `.env` file with the following structure:

```env
# Chimoney API Configuration
CHIMONEY_API_KEY=your_api_key_here
CHIMONEY_BASE_URL=https://api.chimoney.io
CHIMONEY_ENVIRONMENT=sandbox  # or 'production'

# Application Configuration
APP_NAME=Chimoney Integration
APP_ENV=development
APP_DEBUG=true

# Database Configuration (if needed)
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chimoney_app
DB_USER=your_db_user
DB_PASS=your_db_password

# Logging Configuration
LOG_LEVEL=debug
LOG_FILE=logs/app.log

# Rate Limiting
RATE_LIMIT_WINDOW=15 # minutes
RATE_LIMIT_MAX_REQUESTS=100

# Webhook Configuration (for future use)
WEBHOOK_SECRET=your_webhook_secret
WEBHOOK_URL=https://your-domain.com/webhooks/chimoney
```

Create an `.env.example` file for your team:

```env
# Copy this file to .env and fill in your actual values
CHIMONEY_API_KEY=sk_test_your_api_key_here
CHIMONEY_BASE_URL=https://api.chimoney.io
CHIMONEY_ENVIRONMENT=sandbox
APP_NAME=Chimoney Integration
APP_ENV=development
APP_DEBUG=true
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chimoney_app
DB_USER=your_db_user
DB_PASS=your_db_password
LOG_LEVEL=debug
LOG_FILE=logs/app.log
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX_REQUESTS=100
WEBHOOK_SECRET=your_webhook_secret_here
WEBHOOK_URL=https://your-domain.com/webhooks/chimoney
```

## Testing Your Setup

### Step 9: Create Your First API Client

#### Node.js Client:

```javascript
// src/chimoney-client.js
const axios = require('axios');
require('dotenv').config();

class ChimoneyClient {
  constructor() {
    this.apiKey = process.env.CHIMONEY_API_KEY;
    this.baseURL = process.env.CHIMONEY_BASE_URL || 'https://api.chimoney.io';
    
    if (!this.apiKey) {
      throw new Error('CHIMONEY_API_KEY is required in environment variables');
    }
    
    this.client = axios.create({
      baseURL: this.baseURL,
      headers: {
        'X-API-KEY': this.apiKey,
        'Content-Type': 'application/json',
        'User-Agent': 'Chimoney-Integration/1.0'
      },
      timeout: 30000 // 30 seconds
    });
    
    // Add request interceptor for logging
    this.client.interceptors.request.use(
      (config) => {
        console.log(`Making ${config.method.toUpperCase()} request to ${config.url}`);
        return config;
      },
      (error) => {
        console.error('Request error:', error);
        return Promise.reject(error);
      }
    );
    
    // Add response interceptor for error handling
    this.client.interceptors.response.use(
      (response) => {
        console.log(`Response received: ${response.status}`);
        return response;
      },
      (error) => {
        console.error('Response error:', error.response?.data || error.message);
        return Promise.reject(error);
      }
    );
  }
  
  async testConnection() {
    try {
      // Test with a simple endpoint
      const response = await this.client.get('/v0.2.4/info/assets');
      return {
        success: true,
        message: 'Connection successful',
        data: response.data
      };
    } catch (error) {
      return {
        success: false,
        message: 'Connection failed',
        error: error.response?.data || error.message
      };
    }
  }
}

module.exports = ChimoneyClient;
```

#### Python Client:

```python
# chimoney_integration/client.py
import os
import requests
import json
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

class ChimoneyClient:
    def __init__(self):
        self.api_key = os.getenv('CHIMONEY_API_KEY')
        self.base_url = os.getenv('CHIMONEY_BASE_URL', 'https://api.chimoney.io')
        
        if not self.api_key:
            raise ValueError('CHIMONEY_API_KEY is required in environment variables')
        
        self.headers = {
            'X-API-KEY': self.api_key,
            'Content-Type': 'application/json',
            'User-Agent': 'Chimoney-Integration/1.0'
        }
        
    def _make_request(self, method, endpoint, data=None):
        """Make HTTP request to Chimoney API"""
        url = f"{self.base_url}{endpoint}"
        
        print(f"Making {method.upper()} request to {url}")
        
        try:
            response = requests.request(
                method=method,
                url=url,
                headers=self.headers,
                json=data,
                timeout=30
            )
            
            print(f"Response received: {response.status_code}")
            
            # Parse JSON response
            response_data = response.json()
            
            if response.status_code == 200:
                return {
                    'success': True,
                    'data': response_data,
                    'status_code': response.status_code
                }
            else:
                return {
                    'success': False,
                    'error': response_data,
                    'status_code': response.status_code
                }
                
        except requests.exceptions.RequestException as e:
            print(f"Request error: {str(e)}")
            return {
                'success': False,
                'error': f'Request failed: {str(e)}'
            }
        except json.JSONDecodeError as e:
            print(f"JSON decode error: {str(e)}")
            return {
                'success': False,
                'error': f'Invalid JSON response: {str(e)}'
            }
    
    def test_connection(self):
        """Test API connection"""
        try:
            result = self._make_request('GET', '/v0.2.4/info/assets')
            
            if result['success']:
                return {
                    'success': True,
                    'message': 'Connection successful',
                    'data': result['data']
                }
            else:
                return {
                    'success': False,
                    'message': 'Connection failed',
                    'error': result['error']
                }
                
        except Exception as e:
            return {
                'success': False,
                'message': 'Connection test failed',
                'error': str(e)
            }
```

### Step 10: Test Your Configuration

Create a test script to verify everything is working:

#### Node.js Test:

```javascript
// tests/setup-test.js
const ChimoneyClient = require('../src/chimoney-client');

async function testSetup() {
  console.log('🧪 Testing Chimoney API Setup...\n');
  
  try {
    // Test 1: Environment Variables
    console.log('1. Checking environment variables...');
    const requiredVars = ['CHIMONEY_API_KEY', 'CHIMONEY_BASE_URL'];
    const missingVars = requiredVars.filter(varName => !process.env[varName]);
    
    if (missingVars.length > 0) {
      console.log('❌ Missing environment variables:', missingVars);
      return;
    }
    console.log('✅ Environment variables configured correctly\n');
    
    // Test 2: API Key Format
    console.log('2. Validating API key format...');
    const apiKey = process.env.CHIMONEY_API_KEY;
    if (!apiKey.startsWith('sk_')) {
      console.log('⚠️  Warning: API key should start with "sk_"');
    }
    console.log('✅ API key format looks valid\n');
    
    // Test 3: API Connection
    console.log('3. Testing API connection...');
    const client = new ChimoneyClient();
    const connectionTest = await client.testConnection();
    
    if (connectionTest.success) {
      console.log('✅ API connection successful!');
      console.log('📊 Available assets:', connectionTest.data?.data?.length || 0);
    } else {
      console.log('❌ API connection failed:', connectionTest.error);
      return;
    }
    
    console.log('\n🎉 Setup test completed successfully!');
    console.log('You are ready to start integrating with Chimoney API.');
    
  } catch (error) {
    console.error('❌ Setup test failed:', error.message);
  }
}

// Run the test
testSetup();
```

#### Python Test:

```python
# tests/test_setup.py
import os
import sys
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from chimoney_integration.client import ChimoneyClient

def test_setup():
    print('🧪 Testing Chimoney API Setup...\n')
    
    try:
        # Test 1: Environment Variables
        print('1. Checking environment variables...')
        required_vars = ['CHIMONEY_API_KEY', 'CHIMONEY_BASE_URL']
        missing_vars = [var for var in required_vars if not os.getenv(var)]
        
        if missing_vars:
            print(f'❌ Missing environment variables: {missing_vars}')
            return
        print('✅ Environment variables configured correctly\n')
        
        # Test 2: API Key Format
        print('2. Validating API key format...')
        api_key = os.getenv('CHIMONEY_API_KEY')
        if not api_key.startswith('sk_'):
            print('⚠️  Warning: API key should start with "sk_"')
        print('✅ API key format looks valid\n')
        
        # Test 3: API Connection
        print('3. Testing API connection...')
        client = ChimoneyClient()
        connection_test = client.test_connection()
        
        if connection_test['success']:
            print('✅ API connection successful!')
            assets_count = len(connection_test['data'].get('data', [])) if connection_test['data'] else 0
            print(f'📊 Available assets: {assets_count}')
        else:
            print(f'❌ API connection failed: {connection_test["error"]}')
            return
        
        print('\n🎉 Setup test completed successfully!')
        print('You are ready to start integrating with Chimoney API.')
        
    except Exception as error:
        print(f'❌ Setup test failed: {str(error)}')

if __name__ == '__main__':
    test_setup()
```

**Run your test:**

```bash
# Node.js
node tests/setup-test.js

# Python
python tests/test_setup.py
```

**Expected output:**

```
🧪 Testing Chimoney API Setup...

1. Checking environment variables...
✅ Environment variables configured correctly

2. Validating API key format...
✅ API key format looks valid

3. Testing API connection...
Making GET request to /v0.2.4/info/assets
Response received: 200
✅ API connection successful!
📊 Available assets: 15

🎉 Setup test completed successfully!
You are ready to start integrating with Chimoney API.
```

## Postman Collection

### Step 11: Set Up Postman for API Testing

**Download Postman**
- Visit `postman.com`
- Download and install Postman

**Create Chimoney Collection**
- Open Postman
- Click "New" → "Collection"
- Name it "Chimoney API"
- Add description: "Collection for testing Chimoney API endpoints"

**Set Up Environment Variables**
- Click the gear icon (⚙️) in the top-right
- Click "Add" to create a new environment
- Name it "Chimoney Development"
- Add variables:
  - `base_url`: `https://api.chimoney.io`
  - `api_key`: `your_api_key_here`

**Create Test Requests**

**Test Connection Request:**
- **Method:** GET
- **URL:** `{{base_url}}/v0.2.4/info/assets`
- **Headers:**
  - `X-API-KEY`: `{{api_key}}`
  - `Content-Type`: `application/json`

**Create Wallet Address Request:**
- **Method:** POST
- **URL:** `{{base_url}}/v0.2.4/accounts/issue-wallet-address`
- **Headers:**
  - `X-API-KEY`: `{{api_key}}`
  - `Content-Type`: `application/json`
- **Body (raw JSON):**
  ```json
  {
    "userID": "test_user_001",
    "ilpUsername": "test_user_001"
  }
  ```

**Export Collection**
- Click the three dots (⋯) next to your collection
- Select "Export"
- Choose "Collection v2.1"
- Save the file for your team

## Troubleshooting

### Common Issues and Solutions

#### Issue 1: "API key is not defined" Error

```json
{
  "status": "error",
  "error": "API key is not defined. Generate a new one from the developer portal."
}
```

**Solutions:**
- Verify your API key is correct in `.env` file
- Ensure no extra spaces or characters in the API key
- Check that your environment variables are being loaded properly
- Verify the header name is exactly `X-API-KEY`

#### Issue 2: "API Access not enabled" Error

```json
{
  "status": "error",
  "error": "API Access not enabled for account Test. Email support@chimoney.io"
}
```

**Solutions:**
- Your account hasn't been approved for API access yet
- Email `support@chimoney.io` with your account details
- Wait for verification process to complete

#### Issue 3: Connection Timeout

**Solutions:**
- Check your internet connection
- Verify the base URL is correct
- Increase timeout in your HTTP client
- Check if there are any firewall restrictions

#### Issue 4: SSL Certificate Errors

**Solutions:**
- Ensure you're using HTTPS URLs
- Update your HTTP client library
- Check system date and time settings

#### Issue 5: Environment Variables Not Loading

**Solutions:**
- Verify `.env` file is in the correct directory
- Check file permissions
- Ensure dotenv is properly configured
- Restart your application after making changes

### Debug Mode

Enable debug logging for detailed troubleshooting:

#### Node.js Debug:

```javascript
// Add to your client
const debug = process.env.APP_DEBUG === 'true';

if (debug) {
  console.log('Debug mode enabled');
  console.log('API Key (partial):', this.apiKey.substring(0, 10) + '...');
  console.log('Base URL:', this.baseURL);
}
```

#### Python Debug:

```python
import logging

# Configure logging
if os.getenv('APP_DEBUG', 'false').lower() == 'true':
    logging.basicConfig(level=logging.DEBUG)
    logger = logging.getLogger(__name__)
    logger.debug('Debug mode enabled')
    logger.debug(f'API Key (partial): {self.api_key[:10]}...')
    logger.debug(f'Base URL: {self.base_url}')
```

## Security Best Practices

### API Key Security

**Never commit API keys to version control**
- Always use environment variables
- Add `.env` to `.gitignore`
- Use different keys for different environments

**Rotate API keys regularly**
- Generate new keys every 90 days
- Update keys in all environments
- Monitor usage for unusual activity

**Restrict API key permissions**
- Use the principle of least privilege
- Create separate keys for different services
- Monitor and log API key usage

**Secure storage in production**
- Use secrets management services (AWS Secrets Manager, Azure Key Vault)
- Encrypt environment variables
- Implement proper access controls

### Network Security

```javascript
// Add request signing (example)
const crypto = require('crypto');

function signRequest(payload, secret) {
  return crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
}

// Add to request headers
headers['X-Signature'] = signRequest(requestBody, process.env.WEBHOOK_SECRET);
```

### Rate Limiting

```javascript
// Implement client-side rate limiting
class RateLimiter {
  constructor(maxRequests = 100, windowMs = 60000) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
    this.requests = [];
  }
  
  canMakeRequest() {
    const now = Date.now();
    this.requests = this.requests.filter(time => now - time < this.windowMs);
    
    if (this.requests.length >= this.maxRequests) {
      return false;
    }
    
    this.requests.push(now);
    return true;
  }
}
```

## Next Steps

### Immediate Actions

- Complete the setup test to ensure everything is working
- Join the Chimoney Discord community for support
- Bookmark the API documentation for reference
- Set up monitoring for your API usage

### Development Roadmap

- **Week 1:** Complete setup and basic integration
- **Week 2:** Implement wallet address creation
- **Week 3:** Add error handling and logging
- **Week 4:** Build user interface and testing

### Advanced Features to Explore

- **Webhooks:** Set up real-time notifications
- **Multi-currency wallets:** Expand payment options
- **Bulk operations:** Handle multiple transactions
- **Analytics:** Track usage and performance
- **Mobile SDKs:** Extend to mobile applications

### Support Resources

- **Discord Community:** https://discord.gg/TsyKnzT4qV
- **Email Support:** support@chimoney.io
- **API Documentation:** Chimoney Developer Docs
- **Status Page:** Check for API status updates
- **GitHub Examples:** Look for community examples and integrations

---

**Congratulations! 🎉** You now have a fully configured development environment for the Chimoney API. You're ready to start building amazing financial applications that connect users across the globe.

Remember to keep your API keys secure, test thoroughly, and reach out to the community when you need help. Happy coding!
