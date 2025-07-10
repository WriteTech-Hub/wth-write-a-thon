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

An Interledger wallet address is like an email address for money. It's a unique identifier that allows users to send and receive payments across different networks and currencies seamlessly. When you create an Interledger wallet address through Chimoney, you're essentially giving a user a global payment endpoint that works 24/7.

### Key Benefits

- Universal compatibility across Interledger-enabled systems
- Instant cross-border transactions
- Minimal transaction fees
- Support for multiple currencies
- Programmable payment conditions

## Prerequisites

Before you begin, ensure you have:

- **Chimoney Developer Account**: Sign up at [dash.chimoney.io](https://dash.chimoney.io)
- **API Access**: Request verification and API access by emailing support@chimoney.io
- **API Key**: Obtain from your Chimoney developer dashboard
- **Development Environment**: Node.js, Python, or your preferred programming language
- **Basic Understanding**: REST APIs and HTTP requests

## API Endpoint Overview

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

Implement helper functions to ensure proper username formatting:

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

Here's a complete example showing how to integrate wallet address creation into a user registration flow:

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

Implement comprehensive error handling for different scenarios:

```javascript
function handleChimoneyError(error) {
  if (error.response) {
    const { status, data } = error.response;
    
    switch (status) {
      case 400:
        return {
          type: 'validation_error',
          message: 'Invalid request parameters',
          details: data
        };
      case 401:
        return {
          type: 'authentication_error',
          message: 'Invalid or missing API key',
          details: data
        };
      case 403:
        return {
          type: 'authorization_error',
          message: 'API access not enabled for account',
          details: data
        };
      case 500:
        return {
          type: 'server_error',
          message: 'Internal server error occurred',
          details: data
        };
      default:
        return {
          type: 'unknown_error',
          message: `Unexpected error: ${status}`,
          details: data
        };
    }
  } else if (error.request) {
    return {
      type: 'network_error',
      message: 'No response received from server',
      details: error.message
    };
  } else {
    return {
      type: 'client_error',
      message: 'Error setting up the request',
      details: error.message
    };
  }
}
```

## Testing Your Integration

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
