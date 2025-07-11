# Chimoney Write-a-Thon Writing Challenge

Welcome to the 1st edition of the [**WriteTech Hub**](https;//writetechhub.org) **Write-A-Thon Writing Challenge**, in partnership with [**Chimoney**](https://chimoney.io/) and [**DevRel Community Africa**](https://devrelcomafrica.xyz/).

This challenge empowers aspiring and experienced tech writers to build real-world writing experience by documenting the Chimoney API through practical, developer-focused content.

## 🗓️ Important Dates

| Date       | Milestone                                             |
|------------|-------------------------------------------------------|
| July 16    | Challenge Kickoff call (with Chimoney + WTH)          |
| July 22    | Mid-challenge AMA session                             |
| July 31    | Submission deadline                                   |
| August 7   | Winners announced                                     |


## Rewards
- $35 USD cash reward for top 3 contributors
- Top 3 contributors Featured on WriteTech Hub + Chimoney blogs and social media
- Community recognition and networking opportunities

## How to participate

### Step 1: Register and join the community
- Register [here](https://oruly.ai/challenge/write-a-thon_challenge) to participate in the challenge.
- (Required) Join the Chimoney Community: [https://discord.gg/TsyKnzT4qV](https://discord.gg/TsyKnzT4qV) to get updates and support.
- Join the WriteTech Hub community: [https://writetechhub.org/community](https://writetechhub.org/community)

### Step 2: Choose your API endpoint
Pick ONE Chimoney API endpoint to focus on:
- [Create Interledger wallet address](https://api.chimoney.io/v0.2.4/api-docs/#/Interledger/post_v0_2_4_accounts_issue_wallet_address)
- [Multi-currency wallet transfer](https://api.chimoney.io/v0.2.4/api-docs/#/Interledger/post_v0_2_4_multicurrency_wallets_transfer)
- [Payout to Interledger wallet address](https://api.chimoney.io/v0.2.4/api-docs/#/Interledger/post_v0_2_4_payouts_interledger_wallet_address)

### Step 3: Create these three deliverables
You’ll create three separate documents (in Markdown) to showcase your technical writing skills:

1. **Use case article (`use-case.md`)**
   - Describe a real-world scenario where your chosen API endpoint is useful
   - Example: gig worker payouts, cross-border remittances

2. **Tutorial guide (`tutorial.md`)**
   - Step-by-step guide showing how to use the API endpoint in code
   - Include sample requests, responses, and clear explanations

3. **Setup guide (`setup.md`)**
   - Guide readers through authentication and environment setup
   - Ensure they can successfully call the API by the end

## Folder structure

Your submission should be placed inside the `submissions/` folder using this format:

```
submissions/
└── [endpoint]-[slackhandle]/
├── use-case.md
├── tutorial.md
└── setup.md
```

**Example:**

```
submissions/
└── payout-zaycodes/
├── use-case.md
├── tutorial.md
└── setup.md
```

## Submission steps

### Step 4: Submit your work on GitHub
1. Fork this repository
1. Create a new branch:  
  Format: `[endpoint]-[your-slackhandle]`  
  Example: `wallet-funmiwrites`
1. Add your folder and three Markdown files inside `submissions/`
1. Open a Pull Request to the `main` branch
1. Fill out the Pull Request template and tag the following reviewers: @Phyleria, @adebayoileri


### Step 6: Stay tuned
- All updates, reviews, and winner announcements will be shared via the community forums.

## Challenge docs & guidelines

To help you succeed:

- [Contribution Guidelines](CONTRIBUTING.md)  
- [API Review Guidelines](guidelines/how-to-explore-and-understand-an-api.md)  
- [Writing Great Use Case Articles](guidelines/how-to-write-a-use-case-article.md)  
- [Writing Great Tutorial Guides](guidelines/how-to-write-a-tutorial-guide.md)  
- [Writing Great Setup Guides](guidelines/how-to-write-a-setup-guide.md)  
- [Reviewer Guidelines](guidelines/reviewing-submissions.md)

## Useful resources

- Chimoney API Docs: [API Docs](https://chimoney.readme.io/reference/introduction)
- Chimoney Community: [https://discord.gg/TsyKnzT4qV](https://discord.gg/TsyKnzT4qV), `#wth-x-chimoney` channel
- WriteTech Hub Community: [https://writetechhub.org/community](https://writetechhub.org/community).
- Challenge Submission Form: Will be shared via the community forums.

## Tips for success

- **Quality > Quantity**: Focus on clarity, depth, and usefulness
- **Real Use Cases**: Anchor your writing in practical examples
- **Beginner-Friendly**: Assume readers are new to Chimoney
- **Engage the Community**: Use Slack to ask questions and get feedback

Ready to showcase your technical writing skills and contribute to Africa's growing tech ecosystem? Join us in the Write-A-Thon Writing Challenge!
