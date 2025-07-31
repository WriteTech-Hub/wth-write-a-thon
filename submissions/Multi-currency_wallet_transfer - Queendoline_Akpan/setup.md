# Setup Guide: Multi-Currency Wallet Transfers with Chimoney API

This guide will walk you through the essential steps to prepare your environment and obtain the necessary credentials before you begin integrating with Chimoney's Multi-Currency Wallet Transfer API.

## Prerequisites

Before you start coding, ensure you have the following set up:

- **A Chimoney Developer Account**: If you don't have one, you'll need to sign up at the Chimoney developer portal.  
- **A New Application on Your Chimoney Developer Dashboard**: This is crucial for generating and accessing your unique API Key.  
- **A Chimoney API Key**: This is for authenticating all your API requests.  
- **Node.js Installed**: We recommend the latest LTS version for running the JavaScript code samples. You can download it from [nodejs.org](https://nodejs.org).  
- **An Integrated Development Environment (IDE)**: VSCode is highly recommended for its features and ease of use.  

## 2. Get Your Chimoney API Key

Your API Key authenticates your requests and links them to your Chimoney account.

- **Sign Up/Log In**: Go to the Chimoney developer portal (e.g., `sandbox.chimoney.io` for testing).  
- **Create an Application**: Navigate to your developer dashboard, create a new application, and generate your API Key.  
- **Copy Your API Key**: Once generated, copy this key. You'll need it for all your API calls.  

> **Note**: For testing, you'll typically use a Sandbox API Key, which works with test currency and doesn't involve real money.

## 3. Set Up Your Development Environment

You'll need `axios` to make HTTP requests in JavaScript.

- **Install Axios**: Open your terminal or command prompt.  
- Navigate to your project directory.  
- Run the following command to install the axios library:  
  ```bash
  npm install axios
## 4.  Configure Your Authentication

Your API Key needs to be included in the headers of your API requests for successful authentication.

**Place Your API Key**: In your JavaScript code, you'll include your API Key in the `X-API-KEY` header.

```javascript
const headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'X-API-KEY': 'YOUR_API_KEY' // Replace 'YOUR_API_KEY' with your actual key
};
```


Once you have these steps completed, your environment will be ready to start making multi-currency wallet transfer requests with the Chimoney API!

Let's learn how to make your first multicurrency wallet transfer with Chimoney in this [tutorial](/submissions/Multi-currency_wallet_transfer%20-%20Queendoline_Akpan/tutorial.md).
