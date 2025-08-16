# Complete Tutorial: Creating Interledger Wallet Addresses with Chimoney API

This comprehensive tutorial will walk you through the process of creating Interledger wallet addresses using the Chimoney API. By the end of this guide, you'll understand how to integrate this functionality into your applications and provide users with seamless cross-border payment capabilities.

## Table of Contents

1. [Understanding Interledger Wallet Addresses](#understanding-interledger-wallet-addresses)
2. [Prerequisites](#prerequisites)
3. [API Endpoint Overview](#api-endpoint-overview)
4. [Step-by-Step Implementation](#step-by-step-implementation)
5. [Error Handling](#error-handling)
6. [Testing Your Integration](#testing-your-integration)
7. [Best Practices](#best-practices)
8. [Next Steps](#next-steps)

## Understanding Interledger Wallet Addresses

Before diving into the technical implementation, it's essential to understand what we're building and why it matters. An Interledger wallet address is like an email address for money. It's a unique identifier that allows users to send and receive payments across different networks and currencies seamlessly. When you create an Interledger wallet address through Chimoney, you're essentially giving a user a global payment endpoint that works 24/7.

### Key Benefits

- Universal compatibility across Interledger-enabled systems
- Instant cross-border transactions
- Minimal transaction fees
- Support for multiple currencies
- Programmable payment conditions

## Prerequisites

Now that you understand the concept, let's ensure you have everything needed to start building. Before you begin, ensure you have:

- **Chimoney Developer Account**: Sign up at [dash.chimoney.io](https://dash.chimoney.io)
- **API Access**: Request verification and API access by emailing support@chimoney.io
- **API Key**: Obtain from your Chimoney developer dashboard
- **Development Environment**: Node.js, Python, or your preferred programming language
- **Basic Understanding**: REST APIs and HTTP requests

## API Endpoint Overview

With your prerequisites in place, let's examine the specific API endpoint we'll be working with. Understanding the endpoint structure and expected parameters is crucial for successful integration.

**Endpoint**: `POST /v0.2.4/accounts/issue-wallet-address`  
**Base URL**: `https://api.chimoney.io/`  
**Authentication**: API Key (X-API-KEY header)

### Request Parameters

```json
{
  "userID": "string",
  "ilpUsername": "string"
}
```

- `userID`: Unique identifier for the user in your system
- `ilpUsername`: Desired username for the Interledger address (should follow naming conventions)

### Response Format

```json
{
  "status": "success",
  "message": "Wallet Address issued successfully", 
  "data": "walletAddress123"
}
```

## Step-by-Step Implementation

Now that we've covered the fundamentals, let's build the actual implementation. We'll start by setting up our development environment and then create the core functionality step by step.

### Step 1: Set Up Your Environment

First, let's set up the basic structure for making API calls to Chimoney.

#### For Node.js:

```javascript
// Install required dependencies
// npm install axios dotenv

const axios = require('axios');
require('dotenv').config();

// Configuration
const CHIMONEY_BASE_URL = 'https://api.chimoney.io';
const API_KEY = process.env.CHIMONEY_API_KEY;

// Create axios instance with default headers
const chimoneyAPI = axios.create({
  baseURL: CHIMONEY_BASE_URL,
  headers: {
    'X-API-KEY': API_KEY,
    'Content-Type': 'application/json'
  }
});
```

#### For Python:

```python
# Install required dependencies
# pip install requests python-dotenv

import requests
import os
from dotenv import load_dotenv
import json

# Load environment variables
load_dotenv()

# Configuration
CHIMONEY_BASE_URL = 'https://api.chimoney.io'
API_KEY = os.getenv('CHIMONEY_API_KEY')

# Default headers
headers = {
    'X-API-KEY': API_KEY,
    'Content-Type': 'application/json'
}
```

### Step 2: Create the Wallet Address Function

With our environment configured, we can now create the main function that will handle wallet address creation. This function will handle the API communication and provide meaningful feedback about the operation's success or failure.

#### Node.js Implementation:

```javascript
async function createInterledgerWalletAddress(userID, ilpUsername) {
  try {
    // Validate input parameters
    if (!userID || !ilpUsername) {
      throw new Error('Both userID and ilpUsername are required');
    }

    // Prepare request payload
    const payload = {
      userID: userID,
      ilpUsername: ilpUsername
    };

    console.log('Creating Interledger wallet address...');
    console.log('Payload:', JSON.stringify(payload, null, 2));

    // Make API request
    const response = await chimoneyAPI.post('/v0.2.4/accounts/issue-wallet-address', payload);

    // Handle successful response
    if (response.data.status === 'success') {
      console.log('✅ Wallet address created successfully!');
      console.log('Wallet Address:', response.data.data);
      
      return {
        success: true,
        walletAddress: response.data.data,
        message: response.data.message
      };
    } else {
      throw new Error(response.data.message || 'Failed to create wallet address');
    }

  } catch (error) {
    console.error('❌ Error creating wallet address:', error.message);
    
    if (error.response) {
      // Handle specific error responses
      console.error('Status:', error.response.status);
      console.error('Error details:', error.response.data);
      
      return {
        success: false,
        error: error.response.data,
        statusCode: error.response.status
      };
    }
    
    return {
      success: false,
      error: error.message
    };
  }
}
```

#### Python Implementation:

```python
def create_interledger_wallet_address(user_id, ilp_username):
    """
    Create an Interledger wallet address for a user
    
    Args:
        user_id (str): Unique identifier for the user
        ilp_username (str): Desired username for the Interledger address
    
    Returns:
        dict: Response containing success status and wallet address or error details
    """
    try:
        # Validate input parameters
        if not user_id or not ilp_username:
            raise ValueError('Both user_id and ilp_username are required')

        # Prepare request payload
        payload = {
            'userID': user_id,
            'ilpUsername': ilp_username
        }

        print('Creating Interledger wallet address...')
        print(f'Payload: {json.dumps(payload, indent=2)}')

        # Make API request
        response = requests.post(
            f'{CHIMONEY_BASE_URL}/v0.2.4/accounts/issue-wallet-address',
            headers=headers,
            json=payload,
            timeout=30
        )

        # Parse response
        response_data = response.json()

        # Handle successful response
        if response.status_code == 200 and response_data.get('status') == 'success':
            print('✅ Wallet address created successfully!')
            print(f'Wallet Address: {response_data["data"]}')
            
            return {
                'success': True,
                'wallet_address': response_data['data'],
                'message': response_data['message']
            }
        else:
            # Handle error response
            error_message = response_data.get('message', 'Failed to create wallet address')
            raise Exception(error_message)

    except requests.exceptions.RequestException as e:
        print(f'❌ Request error: {str(e)}')
        return {
            'success': False,
            'error': f'Request failed: {str(e)}'
        }
    except Exception as e:
        print(f'❌ Error creating wallet address: {str(e)}')
        return {
            'success': False,
            'error': str(e)
        }
```

### Step 3: Username Validation and Generation

Before creating wallet addresses, we need to ensure that usernames meet the required format standards. This step is crucial because invalid usernames will cause the API request to fail. Let's implement helper functions to validate and generate proper usernames.

#### Node.js:

```javascript
function validateILPUsername(username) {
  // ILP usernames should be lowercase alphanumeric with underscores/hyphens
  const regex = /^[a-z0-9_-]+$/;
  
  if (!regex.test(username)) {
    return {
      valid: false,
      message: 'Username must contain only lowercase letters, numbers, hyphens, and underscores'
    };
  }
  
  if (username.length < 3 || username.length > 30) {
    return {
      valid: false,
      message: 'Username must be between 3 and 30 characters'
    };
  }
  
  return { valid: true };
}

function generateILPUsername(userID, email) {
  // Generate a suggested username based on user data
  let baseUsername;
  
  if (email) {
    baseUsername = email.split('@')[0].toLowerCase();
  } else {
    baseUsername = `user_${userID}`;
  }
  
  // Clean the username
  const cleanUsername = baseUsername
    .replace(/[^a-z0-9_-]/g, '_')
    .replace(/_{2,}/g, '_')
    .replace(/^_|_$/g, '');
  
  return cleanUsername || `user_${userID}`;
}
```

#### Python:

```python
import re

def validate_ilp_username(username):
    """Validate ILP username format"""
    pattern = r'^[a-z0-9_-]+$'
    
    if not re.match(pattern, username):
        return {
            'valid': False,
            'message': 'Username must contain only lowercase letters, numbers, hyphens, and underscores'
        }
    
    if len(username) < 3 or len(username) > 30:
        return {
            'valid': False,
            'message': 'Username must be between 3 and 30 characters'
        }
    
    return {'valid': True}

def generate_ilp_username(user_id, email=None):
    """Generate a suggested ILP username"""
    if email:
        base_username = email.split('@')[0].lower()
    else:
        base_username = f'user_{user_id}'
    
    # Clean the username
    clean_username = re.sub(r'[^a-z0-9_-]', '_', base_username)
    clean_username = re.sub(r'_{2,}', '_', clean_username)
    clean_username = clean_username.strip('_')
    
    return clean_username or f'user_{user_id}'
```

### Step 4: Complete Integration Example

Now let's bring everything together in a comprehensive example that shows how to integrate wallet address creation into a real-world user registration flow. This example demonstrates how all the components work together seamlessly.

#### Node.js:

```javascript
async function registerUserWithWallet(userData) {
  try {
    const { userID, email, firstName, lastName } = userData;
    
    // Generate suggested username
    const suggestedUsername = generateILPUsername(userID, email);
    
    // Validate username
    const validation = validateILPUsername(suggestedUsername);
    if (!validation.valid) {
      throw new Error(`Username validation failed: ${validation.message}`);
    }
    
    console.log(`Registering user ${userID} with ILP username: ${suggestedUsername}`);
    
    // Create Interledger wallet address
    const walletResult = await createInterledgerWalletAddress(userID, suggestedUsername);
    
    if (walletResult.success) {
      // Save wallet address to your database
      const userRecord = {
        userID,
        email,
        firstName,
        lastName,
        interledgerAddress: walletResult.walletAddress,
        ilpUsername: suggestedUsername,
        createdAt: new Date(),
        status: 'active'
      };
      
      // Here you would save to your database
      console.log('User registered successfully with wallet:', userRecord);
      
      return {
        success: true,
        user: userRecord,
        walletAddress: walletResult.walletAddress
      };
    } else {
      throw new Error(`Failed to create wallet: ${walletResult.error}`);
    }
    
  } catch (error) {
    console.error('Registration failed:', error.message);
    return {
      success: false,
      error: error.message
    };
  }
}

// Usage example
async function example() {
  const result = await registerUserWithWallet({
    userID: 'user_12345',
    email: 'john.doe@example.com',
    firstName: 'John',
    lastName: 'Doe'
  });
  
  if (result.success) {
    console.log('🎉 User registration completed!');
    console.log('Wallet Address:', result.walletAddress);
  } else {
    console.log('❌ Registration failed:', result.error);
  }
}
```

## Error Handling

Robust error handling is critical for a production-ready integration. When working with the Chimoney API, you'll encounter various types of errors, each requiring different approaches to resolve. Understanding these errors and how to handle them properly will save you significant debugging time and provide a better user experience.

### Common Error Types and Solutions

**Authentication Errors (401 Unauthorized)**
This occurs when your API key is invalid, missing, or has been revoked. The API will return a 401 status code with details about the authentication failure.

*How to fix:*
- Verify your API key is correctly set in your environment variables
- Ensure the API key hasn't expired or been revoked
- Check that you're using the correct header format (`X-API-KEY`)
- Contact Chimoney support if your account needs API access activation

**Validation Errors (400 Bad Request)**
These happen when the request parameters don't meet the API's requirements. This could be due to missing required fields, invalid data formats, or constraint violations.

*How to fix:*
- Check that both `userID` and `ilpUsername` are provided
- Ensure the `ilpUsername` follows the naming conventions (lowercase, alphanumeric, underscores, and hyphens only)
- Verify the username length is between 3-30 characters
- Validate that the `userID` is a valid string identifier

**Authorization Errors (403 Forbidden)**
Even with a valid API key, you might not have permission to perform certain operations. This typically occurs when your account doesn't have the required privileges.

*How to fix:*
- Contact Chimoney support to enable wallet creation permissions for your account
- Verify your account is in good standing and meets the requirements for API access
- Check if there are any account-level restrictions or limitations

**Duplicate Resource Errors**
If you try to create a wallet address for a user who already has one, or use a username that's already taken, you'll receive an error indicating the resource already exists.

*How to fix:*
- Implement checks to see if a user already has a wallet before creating a new one
- Generate alternative usernames if the preferred one is taken
- Consider using UUIDs or timestamps to make usernames unique

**Network and Server Errors (500+ Status Codes)**
These indicate problems on Chimoney's end or network connectivity issues. They're usually temporary and can be resolved with retry logic.

*How to fix:*
- Implement exponential backoff retry logic for server errors
- Check your internet connection and firewall settings
- Monitor Chimoney's status page for any ongoing service issues
- Set appropriate timeouts for your requests (recommended: 30 seconds)

**Rate Limiting Errors (429 Too Many Requests)**
If you exceed the API rate limits, you'll receive a 429 status code. This protects both your application and Chimoney's infrastructure.

*How to fix:*
- Implement rate limiting in your application to stay within API limits
- Use queue systems for batch operations
- Consider caching results to reduce API calls
- Respect the `Retry-After` header in the response

### Implementing Comprehensive Error Handling

```javascript
function handleChimoneyError(error) {
  if (error.response) {
    const { status, data } = error.response;
    
    switch (status) {
      case 400:
        return {
          type: 'validation_error',
          message: 'Invalid request parameters',
          details: data,
          userMessage: 'Please check your input and try again.'
        };
      case 401:
        return {
          type: 'authentication_error',
          message: 'Invalid or missing API key',
          details: data,
          userMessage: 'Authentication failed. Please contact support.'
        };
      case 403:
        return {
          type: 'authorization_error',
          message: 'API access not enabled for account',
          details: data,
          userMessage: 'Account permissions insufficient. Contact support.'
        };
      case 429:
        return {
          type: 'rate_limit_error',
          message: 'Too many requests',
          details: data,
          userMessage: 'Please wait a moment before trying again.'
        };
      case 500:
        return {
          type: 'server_error',
          message: 'Internal server error occurred',
          details: data,
          userMessage: 'Service temporarily unavailable. Please try again later.'
        };
      default:
        return {
          type: 'unknown_error',
          message: `Unexpected error: ${status}`,
          details: data,
          userMessage: 'An unexpected error occurred. Please try again.'
        };
    }
  } else if (error.request) {
    return {
      type: 'network_error',
      message: 'No response received from server',
      details: error.message,
      userMessage: 'Network error. Please check your connection and try again.'
    };
  } else {
    return {
      type: 'client_error',
      message: 'Error setting up the request',
      details: error.message,
      userMessage: 'Request setup failed. Please try again.'
    };
  }
}
```

## Testing Your Integration

Before deploying to production, thorough testing is essential to ensure your integration works correctly under various conditions. Let's create comprehensive tests that cover both successful operations and error scenarios.

### Unit Tests

```javascript
// Jest unit test example
describe('Interledger Wallet Address Creation', () => {
  test('should validate username correctly', () => {
    expect(validateILPUsername('valid_username')).toEqual({ valid: true });
    expect(validateILPUsername('INVALID')).toEqual({
      valid: false,
      message: expect.any(String)
    });
  });
  
  test('should generate username from email', () => {
    const username = generateILPUsername('123', 'test@example.com');
    expect(username).toBe('test');
  });
  
  test('should handle API errors gracefully', async () => {
    // Mock API failure
    const result = await createInterledgerWalletAddress('', '');
    expect(result.success).toBe(false);
    expect(result.error).toBeDefined();
  });
});
```

### Integration Testing

```javascript
async function testWalletCreation() {
  console.log('🧪 Testing wallet address creation...');
  
  const testCases = [
    {
      name: 'Valid user data',
      userID: 'test_user_001',
      ilpUsername: 'test_user_001',
      expectedSuccess: true
    },
    {
      name: 'Invalid username',
      userID: 'test_user_002',
      ilpUsername: 'INVALID USERNAME!',
      expectedSuccess: false
    },
    {
      name: 'Missing userID',
      userID: '',
      ilpUsername: 'valid_username',
      expectedSuccess: false
    }
  ];
  
  for (const testCase of testCases) {
    console.log(`\nTesting: ${testCase.name}`);
    
    const result = await createInterledgerWalletAddress(
      testCase.userID, 
      testCase.ilpUsername
    );
    
    const passed = result.success === testCase.expectedSuccess;
    console.log(passed ? '✅ PASS' : '❌ FAIL');
    
    if (!passed) {
      console.log('Expected:', testCase.expectedSuccess, 'Got:', result.success);
      console.log('Result:', result);
    }
  }
}
```

## Best Practices

As you prepare to deploy your integration to production, following these best practices will help ensure reliability, security, and maintainability of your wallet address creation system.

### 1. Security Considerations

```javascript
// Store API keys securely
const API_KEY = process.env.CHIMONEY_API_KEY;
if (!API_KEY) {
  throw new Error('CHIMONEY_API_KEY environment variable is required');
}

// Validate input data
function sanitizeInput(input) {
  if (typeof input !== 'string') {
    throw new Error('Input must be a string');
  }
  return input.trim().toLowerCase();
}
```

### 2. Rate Limiting

```javascript
// Implement rate limiting to avoid API throttling
const rateLimit = require('express-rate-limit');

const walletCreationLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 10, // Limit each IP to 10 wallet creations per windowMs
  message: 'Too many wallet creation requests, please try again later'
});
```

### 3. Logging and Monitoring

```javascript
function logWalletCreation(userID, success, walletAddress = null, error = null) {
  const logData = {
    timestamp: new Date().toISOString(),
    action: 'wallet_creation',
    userID,
    success,
    walletAddress,
    error: error ? error.message : null
  };
  
  console.log(JSON.stringify(logData));
  
  // Send to monitoring service (e.g., DataDog, New Relic)
  // monitoring.track('wallet_creation', logData);
}
```

### 4. Caching and Database Storage

```javascript
async function createAndStoreWallet(userID, ilpUsername) {
  // Check if wallet already exists
  const existingWallet = await db.findWalletByUserID(userID);
  if (existingWallet) {
    return {
      success: true,
      walletAddress: existingWallet.address,
      message: 'Wallet already exists'
    };
  }
  
  // Create new wallet
  const result = await createInterledgerWalletAddress(userID, ilpUsername);
  
  if (result.success) {
    // Store in database
    await db.saveWallet({
      userID,
      ilpUsername,
      walletAddress: result.walletAddress,
      createdAt: new Date(),
      provider: 'chimoney'
    });
  }
  
  return result;
}
```

## Next Steps

Congratulations on successfully implementing Interledger wallet address creation! Now that you have a solid foundation, here are some logical next steps to enhance your payment infrastructure and provide even more value to your users.

After successfully implementing Interledger wallet address creation, consider these enhancements:

- **Wallet Management**: Implement functions to retrieve and update wallet information
- **Payment Integration**: Add functionality to send and receive payments to/from these addresses
- **Multi-Currency Support**: Explore Chimoney's multi-currency wallet features
- **Analytics**: Track wallet usage and transaction patterns
- **User Interface**: Build a user-friendly interface for wallet management
- **Mobile Integration**: Implement mobile SDK for wallet functionality

## Conclusion

You now have the knowledge and tools to integrate Interledger wallet address creation into your applications using the Chimoney API. This functionality opens up a world of possibilities for cross-border payments, remittances, and global financial services.

Remember to:
- Always validate input data
- Handle errors gracefully
- Implement proper security measures
- Test thoroughly before going to production
- Monitor your integration for issues

The combination of Interledger Protocol and Chimoney's robust API provides a powerful foundation for building the next generation of financial applications.

---

*Need help? Join the Chimoney Discord community [Chimoney](https://discord.gg/TsyKnzT4qV) or check out our comprehensive setup guide for additional support.*
