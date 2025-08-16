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

Getting started with Chimoney begins with creating your developer account and setting up the necessary credentials. This process is straightforward but requires attention to detail to ensure everything is configured correctly for API access.

### Step 1: Create Your Chimoney Account

The first step in your Chimoney journey is creating an account that will serve as your gateway to all API functionality.

**Visit the Chimoney Dashboard**
- Navigate to `dash.chimoney.io`
- Click "Sign Up" to create a new account

**Complete Registration**

You'll need to provide accurate information as this will be used for account verification:

- Full Name
- Email Address
- Phone Number
- Country
- Business/Organization Name (if applicable)
- Password (minimum 8 characters)

> **💡 Pro Tip:** Use a professional email address as this will be your primary contact method for support and important API updates.

**Email Verification**
- Check your email for a verification link
- Click the link to verify your account
- You should now be able to log into the dashboard

**Initial Dashboard Exploration**
- Familiarize yourself with the dashboard layout
- Note the navigation menu and available sections
- Look for the "API" or "Developer" section

### Step 2: Profile Completion

Before requesting API access, it's important to complete your profile to establish credibility and speed up the approval process.

**Complete Your Profile**
- Add your profile picture (optional but recommended)
- Fill in any missing business information
- Verify your phone number if prompted

**Security Settings**

Security is paramount when working with financial APIs. Set up these essential security measures:

- Enable two-factor authentication (2FA) - **Highly Recommended**
- Set up security questions
- Review login history

> **⚠️ Important:** Two-factor authentication is strongly recommended as it protects your account from unauthorized access, especially when handling API keys.

Now that your account is properly set up, let's move on to requesting the API access you'll need for development.

## API Access Request

Unlike many APIs that provide immediate access, Chimoney requires verification before granting API privileges. This extra step ensures security and compliance with financial regulations, protecting both you and your users.

### Step 3: Request API Access

> ⚠️ **Important**: API access requires verification and approval from Chimoney. This process typically takes 1-3 business days and is necessary for regulatory compliance.

**Prepare Your Application**

Before reaching out to Chimoney, gather the following information to expedite the approval process:

- Document your use case clearly
- Prepare your website/application URL
- Have your business documentation ready
- Estimate your expected transaction volume

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

**Timeline**: Typically 1-3 business days
**Follow-up**: If no response within 5 days, send a polite follow-up email
**Approval Notification**: You'll receive an email confirming API access

While waiting for approval, you can proceed with setting up your development environment to be ready when access is granted.

## Development Environment Setup

With your API access request submitted, it's time to prepare your development environment. This section will guide you through setting up the necessary tools and dependencies for your chosen programming language.

> **📝 Note:** You only need to follow the setup instructions for your preferred programming language - you don't need to set up all three options.

### Step 5: Choose Your Development Stack

Select one of the following environments based on your project requirements and team expertise.

#### Node.js Setup

Node.js is excellent for building APIs and web applications with JavaScript. If your team is familiar with JavaScript or you're building a web-based application, this is often the best choice.

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

**Why these dependencies?**
- `axios`: HTTP client for making API requests
- `dotenv`: Loads environment variables securely
- `jest`: Testing framework for unit tests
- `nodemon`: Automatically restarts your application during development

#### Python Setup

Python is ideal for data processing, automation, and backend services. Choose this if your team prefers Python or you're building data-intensive applications.

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

**Why these packages?**
- `requests`: Python's most popular HTTP library
- `python-dotenv`: Manages environment variables
- `pytest`: Powerful testing framework for Python

#### PHP Setup

PHP is perfect for web applications and has strong support for financial integrations. Choose this if you're working with WordPress, Laravel, or other PHP frameworks.

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

**Why these packages?**
- `guzzlehttp/guzzle`: Robust HTTP client for PHP
- `vlucas/phpdotenv`: Environment variable management
- `phpunit/phpunit`: Standard testing framework for PHP

### Step 6: Set Up Your .gitignore File

The `.gitignore` file is crucial for protecting sensitive information like API keys from being accidentally committed to version control. This prevents security breaches and keeps your credentials safe.

Create a `.gitignore` file to protect sensitive information:

```gitignore
# Environment variables - Contains sensitive API keys and secrets
.env
.env.local
.env.*.local

# Dependencies - Third-party packages (can be reinstalled)
node_modules/
vendor/
__pycache__/
*.pyc

# IDE files - Editor-specific configuration files
.vscode/
.idea/
*.swp
*.swo

# OS files - Operating system generated files
.DS_Store
Thumbs.db

# Log files - May contain sensitive debugging information
*.log
logs/

# Testing - Test coverage and cache files
coverage/
.nyc_output/
.pytest_cache/

# Build outputs - Generated files that can be recreated
dist/
build/
```

> **🔒 Security Note:** The `.env` file contains your API keys and other sensitive information. Never commit this file to version control as it could expose your credentials to unauthorized users.

With your development environment now configured, let's move on to setting up authentication with your Chimoney API credentials.

## Authentication Configuration

Now that your development environment is ready, it's time to configure authentication. This involves obtaining your API key from Chimoney and setting up secure credential management in your application.

### Step 7: Obtain Your API Key

Once your API access is approved, you'll need to retrieve your unique API key from the Chimoney dashboard.

**Log into Chimoney Dashboard**
- Go to `dash.chimoney.io`
- Navigate to the "API Keys" or "Developer" section

**Generate API Key**
- Click "Generate New API Key" or similar button
- Copy the generated key immediately
- Store it securely (you may not be able to view it again)

> **⚠️ Critical:** Your API key is like a password - treat it with the same level of security. Never share it publicly or include it directly in your code.

**API Key Format**

Your API key will look something like:
```
sk_live_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```
or
```
sk_test_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

The prefix indicates the environment:
- `sk_test_`: Sandbox/testing environment
- `sk_live_`: Production environment

### Step 8: Configure Environment Variables

Environment variables are a secure way to store sensitive configuration data like API keys. They keep your credentials separate from your code and allow different settings for development, testing, and production environments.

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

# Database Configuration (if needed for your application)
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chimoney_app
DB_USER=your_db_user
DB_PASS=your_db_password

# Logging Configuration
LOG_LEVEL=debug
LOG_FILE=logs/app.log

# Rate Limiting (prevents API abuse)
RATE_LIMIT_WINDOW=15 # minutes
RATE_LIMIT_MAX_REQUESTS=100

# Webhook Configuration (for future use)
WEBHOOK_SECRET=your_webhook_secret
WEBHOOK_URL=https://your-domain.com/webhooks/chimoney
```

**What each configuration does:**
- **CHIMONEY_API_KEY**: Your secret key for API authentication
- **CHIMONEY_BASE_URL**: The API endpoint (usually doesn't change)
- **CHIMONEY_ENVIRONMENT**: Helps track which environment you're using
- **Rate limiting**: Prevents accidentally overwhelming the API with requests
- **Webhook settings**: For receiving real-time notifications (advanced feature)

Create an `.env.example` file for your team (without sensitive values):

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

> **📝 Team Tip:** The `.env.example` file shows your team what environment variables they need to set up without exposing actual credentials. Share this file freely, but never share the actual `.env` file.

Now that authentication is configured, let's test everything to make sure it's working correctly.

## Testing Your Setup

Before diving into complex integrations, it's crucial to verify that your setup is working correctly. This testing phase will catch configuration issues early and save you debugging time later.

### Step 9: Create Your First API Client

Let's create a reusable client that will handle all communication with the Chimoney API. This client will include proper error handling, logging, and security headers.

#### Node.js Client

This client provides a clean interface for making API calls with built-in error handling and logging:

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
      timeout: 30000 // 30 seconds timeout prevents hanging requests
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
      // Test with a simple endpoint that returns available assets
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

#### Python Client

This Python client provides similar functionality with proper error handling and logging:

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
        """Make HTTP request to Chimoney API with proper error handling"""
        url = f"{self.base_url}{endpoint}"
        
        print(f"Making {method.upper()} request to {url}")
        
        try:
            response = requests.request(
                method=method,
                url=url,
                headers=self.headers,
                json=data,
                timeout=30  # 30 seconds timeout
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
        """Test API connection by fetching available assets"""
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

Now let's create a comprehensive test script to verify that everything is working properly. This test will check your environment variables, validate your API key format, and test the connection to Chimoney's servers.

#### Node.js Test

This test script will systematically verify each part of your setup:

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
      console.log('💡 Make sure your .env file exists and contains all required variables');
      return;
    }
    console.log('✅ Environment variables configured correctly\n');
    
    // Test 2: API Key Format
    console.log('2. Validating API key format...');
    const apiKey = process.env.CHIMONEY_API_KEY;
    if (!apiKey.startsWith('sk_')) {
      console.log('⚠️  Warning: API key should start with "sk_"');
      console.log('💡 Double-check that you copied the complete API key from your dashboard');
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
      console.log('💡 Check your internet connection and API key validity');
      return;
    }
    
    console.log('\n🎉 Setup test completed successfully!');
    console.log('You are ready to start integrating with Chimoney API.');
    
  } catch (error) {
    console.error('❌ Setup test failed:', error.message);
    console.log('💡 Review the error message above and check your configuration');
  }
}

// Run the test
testSetup();
```

#### Python Test

Here's the equivalent test for Python developers:

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
            print('💡 Make sure your .env file exists and contains all required variables')
            return
        print('✅ Environment variables configured correctly\n')
        
        # Test 2: API Key Format
        print('2. Validating API key format...')
        api_key = os.getenv('CHIMONEY_API_KEY')
        if not api_key.startswith('sk_'):
            print('⚠️  Warning: API key should start with "sk_"')
            print('💡 Double-check that you copied the complete API key from your dashboard')
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
            print('💡 Check your internet connection and API key validity')
            return
        
        print('\n🎉 Setup test completed successfully!')
        print('You are ready to start integrating with Chimoney API.')
        
    except Exception as error:
        print(f'❌ Setup test failed: {str(error)}')
        print('💡 Review the error message above and check your configuration')

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

If you see this success message, congratulations! Your setup is complete and working correctly. If you encounter any errors, the troubleshooting section below will help you resolve them.

## Environment Variables

Understanding and properly managing environment variables is crucial for maintaining security and flexibility in your application. Let's dive deeper into why they matter and how to use them effectively.

### What Are Environment Variables?

Environment variables are configuration values that exist outside your application code. They're particularly important for storing sensitive information like API keys because they:

- Keep secrets out of your source code
- Allow different configurations for different environments (development, testing, production)
- Can be changed without modifying code
- Are not accidentally shared when you commit code to version control

### Essential Environment Variables for Chimoney

Here's a detailed breakdown of the key environment variables you'll need:

```env
# Core Chimoney API Settings
CHIMONEY_API_KEY=sk_test_your_actual_key_here
CHIMONEY_BASE_URL=https://api.chimoney.io
CHIMONEY_ENVIRONMENT=sandbox

# Application Settings
APP_NAME=My Chimoney Integration
APP_ENV=development
APP_DEBUG=true

# Rate Limiting (prevents API abuse)
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=info
LOG_FILE=logs/chimoney.log
```

**Variable Explanations:**

- **CHIMONEY_API_KEY**: Your unique identifier for API access - treat this like a password
- **CHIMONEY_BASE_URL**: The API server address (rarely changes)
- **CHIMONEY_ENVIRONMENT**: Helps you track which environment you're using
- **APP_DEBUG**: When true, enables detailed logging for troubleshooting
- **RATE_LIMIT_***: Prevents accidentally overwhelming the API with too many requests

### Managing Multiple Environments

As your application grows, you'll need different configurations for different stages:

**Development (.env.development)**
```env
CHIMONEY_API_KEY=sk_test_development_key
APP_DEBUG=true
LOG_LEVEL=debug
```

**Production (.env.production)**
```env
CHIMONEY_API_KEY=sk_live_production_key
APP_DEBUG=false
LOG_LEVEL=error
```

> **🚨 Critical Security Note:** Never use production API keys in development environments, and never commit any `.env` file to version control.

With your environment properly configured, let's set up Postman for easy API testing and exploration.

## Postman Collection

Postman is an excellent tool for testing APIs before integrating them into your application. It allows you to experiment with different requests, save common operations, and share API examples with your team.

### Step 11: Set Up Postman for API Testing

Let's configure Postman to work seamlessly with the Chimoney API, making it easy to test and debug your integration.

**Download Postman**
- Visit `postman.com`
- Download and install Postman (it's free for basic use)

**Create Chimoney Collection**

A collection in Postman is like a folder for organizing related API requests:

- Open Postman
- Click "New" → "Collection"
- Name it "Chimoney API"
- Add description: "Collection for testing Chimoney API endpoints"

**Set Up Environment Variables**

Postman environments let you store variables like API keys that can be reused across requests:

- Click the gear icon (⚙️) in the top-right
- Click "Add" to create a new environment
- Name it "Chimoney Development"
- Add variables:
  - `base_url`: `https://api.chimoney.io`
  - `api_key`: `your_api_key_here`

> **💡 Postman Tip:** Using variables makes it easy to switch between development and production environments without changing individual requests.

**Create Test Requests**

Let's create two essential requests that you'll use frequently:

**1. Test Connection Request:**

This request verifies your API access and returns available assets:

- **Method:** GET
- **URL:** `{{base_url}}/v0.2.4/info/assets`
- **Headers:**
  - `X-API-KEY`: `{{api_key}}`
  - `Content-Type`: `application/json`

**2. Create Wallet Address Request:**

This request demonstrates the main functionality of creating wallet addresses:

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

**Export Collection for Team Sharing**
- Click the three dots (⋯) next to your collection
- Select "Export"
- Choose "Collection v2.1"
- Save the file for your team

> **👥 Team Collaboration:** Sharing the exported collection file ensures everyone on your team uses the same request formats and can quickly get up to speed with the API.

Now that you have your testing tools ready, let's address common issues you might encounter during setup.

## Troubleshooting

Even with careful setup, you might encounter issues along the way. This section covers the most common problems developers face when integrating with the Chimoney API and provides clear solutions.

### Common Issues and Solutions

#### Issue 1: "API key is not defined" Error

**Error Message:**
```json
{
  "status": "error",
  "error": "API key is not defined. Generate a new one from the developer portal."
}
```

**What this means:** The API can't find or recognize your API key.

**Solutions:**
1. **Check your .env file**: Verify your API key is correctly set in `.env` file
2. **Remove extra characters**: Ensure no extra spaces or characters before/after the API key
3. **Restart your application**: Environment variables are loaded when the application starts
4. **Verify header name**: The header must be exactly `X-API-KEY` (case-sensitive)

**Quick Test:**
```javascript
console.log('API Key:', process.env.CHIMONEY_API_KEY ? 'Present' : 'Missing');
```

#### Issue 2: "API Access not enabled" Error

**Error Message:**
```json
{
  "status": "error",
  "error": "API Access not enabled for account Test. Email support@chimoney.io"
}
```

**What this means:** Your account hasn't been approved for API access yet.

**Solutions:**
1. **Wait for approval**: The verification process typically takes 1-3 business days
2. **Check your email**: Look for an approval notification from Chimoney
3. **Follow up**: If it's been more than 5 business days, send a polite follow-up email
4. **Provide more details**: Consider providing additional information about your use case

#### Issue 3: Connection Timeout

**What this means:** Your request is taking too long to complete, often due to network issues.

**Solutions:**
1. **Check internet connection**: Verify you have stable internet access
2. **Verify the base URL**: Ensure you're using `https://api.chimoney.io`
3. **Increase timeout**: Set timeout to at least 30 seconds in your HTTP client
4. **Check firewall**: Ensure your firewall isn't blocking outbound HTTPS requests

**Example timeout configuration:**
```javascript
// Node.js with axios
timeout: 30000 // 30 seconds

// Python with requests
timeout=30
```

#### Issue 4: SSL Certificate Errors

**What this means:** Your system can't verify the security certificate of the API server.

**Solutions:**
1. **Update your HTTP client**: Ensure you're using the latest version
2. **Check system time**: Incorrect system time can cause certificate validation to fail
3. **Use HTTPS URLs**: Always use `https://` instead of `http://`
4. **Update certificates**: Ensure your system has updated root certificates

#### Issue 5: Environment Variables Not Loading

**What this means:** Your application can't access the values you've set in your `.env` file.

**Solutions:**
1. **Check file location**: Ensure `.env` is in your project root directory
2. **Verify file permissions**: Make sure the file is readable by your application
3. **Import dotenv correctly**: Ensure you're loading dotenv at the start of your application
4. **Restart application**: Environment variables are loaded when the application starts

**Quick debugging:**
```javascript
// Node.js
console.log('All env vars:', Object.keys(process.env).filter(key => key.startsWith('CHIMONEY')));

// Python
import os
print('Chimoney vars:', {k: v for k, v in os.environ.items() if k.startswith('CHIMONEY')})
```

#### Issue 6: Rate Limiting Errors (429 Status)

**What this means:** You're making too many requests too quickly.

**Solutions:**
1. **Implement rate limiting**: Add delays between requests
2. **Use exponential backoff**: Gradually increase delay times after errors
3. **Batch operations**: Combine multiple operations when possible
4. **Respect retry headers**: Check for `Retry-After` header in responses

### Debug Mode Configuration

Enable detailed logging to troubleshoot issues more effectively:

#### Node.js Debug Setup

```javascript
// Add to your client configuration
const debug = process.env.APP_DEBUG === 'true';

class ChimoneyClient {
  constructor() {
    // ... existing code ...
    
    if (debug) {
      console.log('🔍 Debug mode enabled');
      console.log('📍 API Key (partial):', this.apiKey.substring(0, 10) + '...');
      console.log('🌐 Base URL:', this.baseURL);
      console.log('⚙️ Environment:', process.env.CHIMONEY_ENVIRONMENT);
    }
  }
}
```

#### Python Debug Setup

```python
import logging
import os

# Configure detailed logging when debug mode is enabled
if os.getenv('APP_DEBUG', 'false').lower() == 'true':
    logging.basicConfig(
        level=logging.DEBUG,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    )
    logger = logging.getLogger(__name__)
    logger.debug('🔍 Debug mode enabled')
    logger.debug(f'📍 API Key (partial): {self.api_key[:10]}...')
    logger.debug(f'🌐 Base URL: {self.base_url}')
```

> **🐛 Debugging Tip:** Enable debug mode only during development. It can expose sensitive information and slow down your application in production.

With troubleshooting knowledge in hand, let's explore essential security practices to protect your integration.

## Security Best Practices

Security is paramount when working with financial APIs. Following these best practices will protect your application, your users' data, and maintain compliance with financial regulations.

### API Key Security

Your API key is the gateway to your Chimoney integration - protecting it properly is essential.

**Never Commit API Keys to Version Control**

API keys should never appear in your source code or be committed to repositories like GitHub:

```javascript
// ❌ NEVER do this
const apiKey = 'sk_live_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'; // Hardcoded - very bad!

// ✅ Always do this
const apiKey = process.env.CHIMONEY_API_KEY; // From environment variables
```

**Why this matters:** Once code is committed to version control, the API key could be exposed to anyone with access to the repository, including future team members or if the repository becomes public.

**Rotate API Keys Regularly**

Implement a regular rotation schedule for your API keys:

- **Generate new keys**: Create new API keys every 90 days
- **Update all environments**: Ensure all systems use the new key
- **Monitor usage**: Track API usage for unusual activity
- **Revoke old keys**: Disable old keys after confirming the new ones work

**Restrict API Key Permissions**

Follow the principle of least privilege:

- **Use separate keys**: Create different keys for different services or environments
- **Monitor access patterns**: Regularly review which endpoints your keys are accessing
- **Set up alerts**: Configure notifications for unusual API usage

**Secure Storage in Production**

For production environments, use professional secrets management:

```javascript
// Example using AWS Secrets Manager
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getChimoneyApiKey() {
  try {
    const secret = await secretsManager.getSecretValue({
      SecretId: 'chimoney-api-key'
    }).promise();
    
    return JSON.parse(secret.SecretString).apiKey;
  } catch (error) {
    console.error('Failed to retrieve API key from secrets manager:', error);
    throw error;
  }
}
```

**Popular secrets management services:**
- AWS Secrets Manager
- Azure Key Vault
- Google Secret Manager
- HashiCorp Vault

### Network Security

Protect your API communications from interception and tampering.

**Always Use HTTPS**

```javascript
// ✅ Correct - uses HTTPS
const baseURL = 'https://api.chimoney.io';

// ❌ Never use HTTP for API calls
const baseURL = 'http://api.chimoney.io'; // Insecure!
```

**Implement Request Signing** (Advanced)

For additional security, you can sign your requests:

```javascript
const crypto = require('crypto');

function signRequest(payload, secret) {
  return crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
}

// Add to request headers
const signature = signRequest(requestBody, process.env.WEBHOOK_SECRET);
headers['X-Signature'] = `sha256=${signature}`;
```

**Validate SSL Certificates**

Ensure your HTTP client validates SSL certificates:

```javascript
// Node.js with axios - this is enabled by default
const client = axios.create({
  baseURL: 'https://api.chimoney.io',
  // SSL verification is enabled by default
  httpsAgent: new https.Agent({
    rejectUnauthorized: true // Ensures SSL certificate validation
  })
});
```

### Rate Limiting and Abuse Prevention

Implement client-side rate limiting to prevent accidental API abuse:

```javascript
class RateLimiter {
  constructor(maxRequests = 100, windowMs = 60000) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
    this.requests = [];
  }
  
  canMakeRequest() {
    const now = Date.now();
    // Remove old requests outside the window
    this.requests = this.requests.filter(time => now - time < this.windowMs);
    
    // Check if we're at the limit
    if (this.requests.length >= this.maxRequests) {
      const oldestRequest = Math.min(...this.requests);
      const waitTime = this.windowMs - (now - oldestRequest);
      console.log(`Rate limit reached. Wait ${waitTime}ms before next request.`);
      return false;
    }
    
    // Record this request
    this.requests.push(now);
    return true;
  }
  
  async waitIfNeeded() {
    if (!this.canMakeRequest()) {
      const waitTime = this.getWaitTime();
      console.log(`Waiting ${waitTime}ms due to rate limiting...`);
      await new Promise(resolve => setTimeout(resolve, waitTime));
    }
  }
  
  getWaitTime() {
    if (this.requests.length === 0) return 0;
    const now = Date.now();
    const oldestRequest = Math.min(...this.requests);
    return Math.max(0, this.windowMs - (now - oldestRequest));
  }
}

// Usage example
const rateLimiter = new RateLimiter(50, 60000); // 50 requests per minute

async function makeApiCall() {
  await rateLimiter.waitIfNeeded();
  // Make your API call here
  return await client.post('/v0.2.4/accounts/issue-wallet-address', data);
}
```

### Input Validation and Sanitization

Always validate and sanitize input data before sending it to the API:

```javascript
function validateUserInput(userID, ilpUsername) {
  const errors = [];
  
  // Validate userID
  if (!userID || typeof userID !== 'string') {
    errors.push('userID must be a non-empty string');
  } else if (userID.length > 100) {
    errors.push('userID must be less than 100 characters');
  }
  
  // Validate ilpUsername
  if (!ilpUsername || typeof ilpUsername !== 'string') {
    errors.push('ilpUsername must be a non-empty string');
  } else if (!/^[a-z0-9_-]+$/.test(ilpUsername)) {
    errors.push('ilpUsername must contain only lowercase letters, numbers, hyphens, and underscores');
  } else if (ilpUsername.length < 3 || ilpUsername.length > 30) {
    errors.push('ilpUsername must be between 3 and 30 characters');
  }
  
  return {
    isValid: errors.length === 0,
    errors: errors
  };
}

// Usage
const validation = validateUserInput(userID, ilpUsername);
if (!validation.isValid) {
  console.error('Validation failed:', validation.errors);
  return { success: false, errors: validation.errors };
}
```

### Logging and Monitoring

Implement comprehensive logging while protecting sensitive data:

```javascript
function createSecureLogger() {
  const winston = require('winston');
  
  return winston.createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: winston.format.combine(
      winston.format.timestamp(),
      winston.format.errors({ stack: true }),
      winston.format.json(),
      winston.format.printf(({ timestamp, level, message, ...meta }) => {
        // Remove sensitive data from logs
        const sanitizedMeta = sanitizeLogData(meta);
        return JSON.stringify({
          timestamp,
          level,
          message,
          ...sanitizedMeta
        });
      })
    ),
    transports: [
      new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
      new winston.transports.File({ filename: 'logs/combined.log' }),
      ...(process.env.NODE_ENV !== 'production' ? [new winston.transports.Console()] : [])
    ]
  });
}

function sanitizeLogData(data) {
  const sensitiveKeys = ['apikey', 'password', 'token', 'secret'];
  const sanitized = { ...data };
  
  Object.keys(sanitized).forEach(key => {
    if (sensitiveKeys.some(sensitive => key.toLowerCase().includes(sensitive))) {
      sanitized[key] = '[REDACTED]';
    }
  });
  
  return sanitized;
}
```

With security measures in place, let's explore what comes next in your Chimoney integration journey.

## Next Steps

Congratulations! You've successfully set up your Chimoney API integration environment. Now it's time to put everything to work and start building amazing financial applications.

### Immediate Actions

Before diving into development, complete these essential tasks:

**1. Complete Your Setup Verification**
- Run the setup test script one final time to ensure everything is working
- Document any configuration specific to your environment
- Create a backup of your working configuration

**2. Join the Chimoney Community**
- Join the [Chimoney Discord community](https://discord.gg/TsyKnzT4qV) for real-time support and discussions
- Follow Chimoney on social media for updates and announcements
- Bookmark the API documentation for quick reference

**3. Set Up Monitoring**
- Configure logging for your API usage
- Set up alerts for API errors or unusual usage patterns
- Create a simple dashboard to track your integration's health

**4. Plan Your Development Approach**
- Define your specific use cases and requirements
- Create a timeline for your integration milestones
- Identify potential challenges and plan solutions

### Development Roadmap

Here's a suggested timeline for building your Chimoney integration:

**Week 1: Foundation Building**
- Complete setup and basic integration testing
- Implement your first wallet address creation
- Set up error handling and logging systems
- Create basic unit tests

**Week 2: Core Functionality**
- Build the main wallet creation workflow
- Implement user management features
- Add input validation and security measures
- Expand your test coverage

**Week 3: Polish and Enhancement**
- Add comprehensive error handling
- Implement rate limiting and optimization
- Build monitoring and alerting systems
- Conduct thorough integration testing

**Week 4: User Experience and Testing**
- Create user-friendly interfaces
- Implement proper error messages for users
- Conduct user acceptance testing
- Prepare for production deployment

### Advanced Features to Explore

As your integration matures, consider these advanced capabilities:

**Webhooks for Real-Time Notifications**

Webhooks allow your application to receive real-time updates about transactions and account changes:

```javascript
// Example webhook handler
app.post('/webhooks/chimoney', (req, res) => {
  const signature = req.headers['x-signature'];
  const payload = req.body;
  
  // Verify webhook signature
  const expectedSignature = createSignature(payload, process.env.WEBHOOK_SECRET);
  
  if (signature === expectedSignature) {
    // Process the webhook
    handleChimoneyWebhook(payload);
    res.status(200).send('OK');
  } else {
    res.status(401).send('Unauthorized');
  }
});
```

**Multi-Currency Wallet Support**

Expand your integration to support multiple currencies and payment methods:

```javascript
async function createMultiCurrencyWallet(userID, currencies = ['USD', 'NGN', 'GHS']) {
  const walletPromises = currencies.map(currency => 
    createCurrencySpecificWallet(userID, currency)
  );
  
  const wallets = await Promise.all(walletPromises);
  return wallets;
}
```

**Bulk Operations for Scale**

Handle multiple operations efficiently:

```javascript
async function createWalletsInBatch(users) {
  const batchSize = 10; // Process 10 at a time
  const results = [];
  
  for (let i = 0; i < users.length; i += batchSize) {
    const batch = users.slice(i, i + batchSize);
    const batchResults = await Promise.allSettled(
      batch.map(user => createInterledgerWalletAddress(user.id, user.username))
    );
    results.push(...batchResults);
    
    // Add delay between batches to respect rate limits
    if (i + batchSize < users.length) {
      await new Promise(resolve => setTimeout(resolve, 1000));
    }
  }
  
  return results;
}
```

**Analytics and Reporting**

Track important metrics about your integration:

```javascript
class ChimoneyAnalytics {
  constructor() {
    this.metrics = {
      walletsCreated: 0,
      apiCalls: 0,
      errors: 0,
      responseTime: []
    };
  }
  
  recordWalletCreation() {
    this.metrics.walletsCreated++;
  }
  
  recordApiCall(responseTime) {
    this.metrics.apiCalls++;
    this.metrics.responseTime.push(responseTime);
  }
  
  recordError() {
    this.metrics.errors++;
  }
  
  getReport() {
    return {
      ...this.metrics,
      averageResponseTime: this.metrics.responseTime.reduce((a, b) => a + b, 0) / this.metrics.responseTime.length || 0,
      errorRate: this.metrics.errors / this.metrics.apiCalls || 0
    };
  }
}
```

**Mobile Integration**

Extend your integration to mobile applications:

```javascript
// React Native example
import { ChimoneyClient } from './chimoney-client';

const MobileWalletService = {
  async createWalletForUser(userData) {
    try {
      const client = new ChimoneyClient();
      const result = await client.createInterledgerWalletAddress(
        userData.id, 
        userData.username
      );
      
      if (result.success) {
        // Store wallet address locally
        await AsyncStorage.setItem(`wallet_${userData.id}`, result.walletAddress);
      }
      
      return result;
    } catch (error) {
      console.error('Mobile wallet creation failed:', error);
      throw error;
    }
  }
};
```

### Support Resources

As you build your integration, these resources will be invaluable:

**Primary Support Channels:**
- **Discord Community**: [https://discord.gg/TsyKnzT4qV](https://discord.gg/TsyKnzT4qV) - Real-time help from the community
- **Email Support**: support@chimoney.io - Official technical support
- **API Documentation**: Comprehensive guides and reference materials

**Development Resources:**
- **GitHub Examples**: Community-contributed integration examples
- **Status Page**: Real-time API status and maintenance updates
- **Developer Blog**: Best practices, tutorials, and feature announcements

**Best Practices for Getting Help:**
1. **Search first**: Check Discord and documentation for similar issues
2. **Provide context**: Include relevant code, error messages, and steps to reproduce
3. **Be specific**: Describe exactly what you're trying to achieve
4. **Share minimal examples**: Create simple test cases that demonstrate the issue

### Preparing for Production

When you're ready to deploy your integration:

**1. Security Checklist**
- [ ] Production API keys are stored securely
- [ ] All sensitive data is properly encrypted
- [ ] Rate limiting is implemented
- [ ] Input validation is comprehensive
- [ ] Error handling doesn't expose sensitive information

**2. Performance Checklist**
- [ ] API calls are optimized and cached where appropriate
- [ ] Timeout values are set appropriately
- [ ] Retry logic is implemented for transient failures
- [ ] Database queries are optimized
- [ ] Monitoring and alerting are configured

**3. User Experience Checklist**
- [ ] Error messages are user-friendly
- [ ] Loading states are implemented
- [ ] Success confirmations are clear
- [ ] Help documentation is available
- [ ] Support contact information is easily accessible

---

## Conclusion

**Congratulations! 🎉** You now have a fully configured development environment for the Chimoney API, complete with security best practices, error handling, and testing capabilities. You're ready to start building amazing financial applications that connect users across the globe.

### Key Achievements

Through this guide, you have:

- ✅ Created and verified your Chimoney developer account
- ✅ Configured a secure development environment
- ✅ Implemented proper API authentication
- ✅ Set up comprehensive testing and debugging tools
- ✅ Learned essential security practices
- ✅ Prepared for scalable, production-ready development

### Remember the Fundamentals

As you continue building:

- **Security First**: Always protect API keys and sensitive data
- **Test Thoroughly**: Verify every integration before going live
- **Monitor Continuously**: Keep track of your API usage and performance
- **Stay Updated**: Follow Chimoney announcements for new features and updates
- **Engage with Community**: Don't hesitate to ask questions and share your experiences

### Your Next Steps

1. **Start Building**: Use the foundation you've created to build your specific use case
2. **Join the Community**: Connect with other developers in the Chimoney Discord
3. **Share Your Success**: Help others by documenting your experiences and solutions
4. **Keep Learning**: Explore advanced features as your needs grow

The combination of Interledger Protocol and Chimoney's robust API provides a powerful foundation for building the next generation of financial applications. Whether you're creating payment solutions for freelancers, building remittance services, or developing e-commerce platforms, you now have the tools and knowledge to succeed.

**Happy coding!** 🚀

---

*Need help along the way? The Chimoney community is here to support you. Join us on [Discord](https://discord.gg/TsyKnzT4qV) or reach out to support@chimoney.io for assistance.*
