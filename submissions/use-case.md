# How Hackathon Organizers Can Automate Payouts with Chimoney

This article shows how hackathon organizers can use the Chimoney `accounts/issue-wallet-address` API to simplify and speed up prize payouts to participants around the world. By pre-issuing Interledger wallets, organizers can offer a smooth payout experience.

## The Challenge

Running a global hackathon comes with a lot of challenges, especially when it’s time to send out prizes. using  traditional payment methods often mean:

- Asking for sensitive banking details from people in many countries.
- Managing slow and expensive international bank transfers.
- Excluding participants in countries with limited access to global payment.

These problems add extra work for organizers and delay winners from receiving their prizes.

## The Solution

Chimoney's `accounts/issue-wallet-address` API offers a better way. Instead of collecting payment details manually, hackathon platforms can automatically create an Interledger (ILP) wallet for each participant as they register.

This means every participant, no matter where they’re from, has a wallet ready to receive money.

## Use Case

Let’s walk through a real-world example.

### Step 1: Registration and Wallet Creation

When someone signs up for the hackathon:

1. They fill in their basic info (like name and email).
2. Behind the scenes, the platform calls the Chimoney `/v0.2.4/accounts/issue-wallet-address` API.
3. The new wallet address is saved in the platform’s database and linked to the user’s profile.

With this, every participant gets a wallet without needing to do anything extra.

Check the setup guide to see how to use the API → [Setup Instructions](setup.md)


### Step 2: Announcing Winners and Sending Payouts

When winners are chosen:

1. The organizer picks the winners and prize amounts.
2. Instead of using slow manual transfers, they send the funds using Chimoney's payout options.
3. The money is sent directly to the Interledger wallets created earlier.

## Why use chimoney's interledger wallet address API 

Using Chimoney’s `accounts/issue-wallet-address` API brings big advantages:

- Open to Everyone: People from anywhere can join.
- Fast and Automated: No need to collect bank details later.
- Fewer Headaches: Organizers don’t have to deal with sensitive data or tricky payment setups.