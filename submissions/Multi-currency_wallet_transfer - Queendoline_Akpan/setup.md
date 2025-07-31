# Setup Guide: Multi-Currency Wallet Transfers with Chimoney API

This guide will walk you through the essential steps to prepare your environment and obtain the necessary credentials before you begin integrating with Chimoney's Multi-Currency Wallet Transfer API.

## Prerequisites

Before you start coding, ensure you have the following set up:

- **A Chimoney developer account**: If you don't have one, you'll need to sign up at the Chimoney developer portal.  
- **A new application on Your Chimoney developer dashboard**: This is crucial for generating and accessing your unique API Key.  
- **An API Key**: This is for authenticating all your API requests.  
- **Node.js Installed**: We recommend the latest LTS version for running the JavaScript code samples. You can download it from [nodejs.org](https://nodejs.org).  
- An IDE most preferably VSCode.

## 2. Get Your Chimoney API Key

Your API Key authenticates your requests and links them to your Chimoney account.

- **Sign Up/Log In**: Go to the Chimoney developer portal (e.g., `sandbox.chimoney.io` for testing).  
- **Create an Application**: Navigate to your developer dashboard, create a new application, and generate your API Key.  
- **Copy Your API Key**: Once generated, copy this key. You'll need it for all your API calls.  

> **Note**: For testing, you'll typically use a Sandbox API Key, which works with test currency and doesn't involve real money.

## 3. Set Up Your Development Environment

You'll need a library or tool to make HTTP requests in JavaScript. While we are using `axios` for the code samples in this  [tutorial](/submissions/Multi-currency_wallet_transfer%20-%20Queendoline_Akpan/tutorial.md)
, you can use any other HTTP client you prefer (e.g., the browser's `fetch` API).

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
S