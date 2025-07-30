# Use Case

You can use Chimoney to facilitate seamless cross-border gig worker payouts via your Interledger wallet. By doing this, you can prevent unnecessary fees, slow processing times, and intermediary remediation.

## **The Challenge**

International gig platforms often experience high fees and slow settlement due to intermediaries such as SWIFT, ACH, and mobile money. These platforms typically have limited-access for freelancers living in low-banked regions. As a result, the international contractor payout process can be inefficient and exhausting for both companies and freelancers.

## **Our Solution**

This endpoint facilitates a smooth Interledger transaction by giving companies direct access to the ILP via an API call.

By using the endpoint, companies and platforms can:

*   Pay freelancers anywhere in the world via an ILP payment pointer.
    
*   Pay freelancers instantly through a seamless API call.
    
*   Avoid traditional banking delays due to intermediary intervention.
    
*   Reduce fees for each transaction.
    
*   Offer access to recipients without formal banking infrastructure using Chimoney’s off-ramps alternatives (i.e., mobile money, gift cards, airtime, and more).
    

## **User Story Scenario: Freelance Platform Payouts**

### **Background**

Meet Lily, a contractor who provides software development services to 10/10 Development Solutions, a software as a service (SaaS) company that builds APIs for financial clients. 10/10 Development Solutions hires contractors like Lily to develop its internal web APIs. As a result, the company works with developers all around the world.

### **Problem**

Lily lives in a different country than 10/10 Development Solutions, as do many other freelancers. The company pays its freelancers through an online payment portal, but this incurs high fees and takes considerable time to process. Many freelancers complain about the fees, as it reduces the amount they get paid. Freelancers also complain about the length of time each transaction takes, because due to intermediaries, transaction times vary.

### **Solution**

As a result, 10/10 Development Solutions needs an alternative to their current payment platform, so they can pay developers quickly and incur less fees. Developers have indicated that they want to take home more pay. 10/10 Development Solutions looks to Chimoney to solve the issue, which will mitigate the problems stemming from other payment platforms.

In four easy steps, 10/10 Development Solutions pays Lily and other contractors using the Interledger Wallet Address endpoint:

1.  Lily signs up with Chimoney, generating an Interledger payment pointer, lily$wallet.chimoney.io.
    
2.  At payroll time, 10/10 Development Solutions  issues a payout via the endpoint. This payout specifies the payment pointer, currency, and amount.
    
3.  Chimoney transfers funds via the ILP to Lily.
    
4.  Lily redeems her currency using the available local options (i.e., bank, mobile money, or airtime).
    

## **Benefits**

Chimoney’s Interledger Wallet Address endpoint has the following benefits:

*   **Speed:** The ILP supports instant transactions.
    
*   **Cost-Effective:** No intermediary fees or foreign exchange markup.
    
*   **Inclusivity:** Supports users without bank accounts.
    
*   **Scalable Global Payout:** One integration handles payouts across regions.
    

The Interledger Wallet Address endpoint mitigates the following pain points:

| Pain Point | Traditional Method | Chimoney’s Solution |
| --- | --- | --- |
| Cost | High, and sometimes hidden, banking fees | Low, transparent API fees. |
| Speed | Transactions via SWIFT can take days | The ILP offers near-instant transactions. |
| Adoption Barrier | Bank accounts are required for transactions | Bank accounts are not required; alternative payout methods are supported. |

## **Additional Use Cases**

We’ve highlighted some successful user stories across various industries and professions:

#### _**Maria's Story - UX Designer, Mexico City**_

"Before Chimoney, I had to factor in a week of waiting and $40 in fees for every payment. Now I receive my earnings instantly and keep almost every dollar I earn. This has completely changed how I manage my freelance business."

#### _**Kwame's Story - Full-Stack Developer, Accra**_

"I was unable to work with many international clients because of banking restrictions. Chimoney opened up a whole new world of opportunities. I can now compete globally and get paid just as quickly as developers in major financial centers."

#### _**Robert, Platform Owner, Nigeria**_

"Implementing Chimoney's Interledger Wallet Address API was a game-changer. We went from spending 30% of our customer service resources on payment issues to less than 5%. Our freelancers are happier, we're more profitable, and we can serve markets we never thought possible."