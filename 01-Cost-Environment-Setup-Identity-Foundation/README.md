# Project 1: Cost & Environment Setup | Identity Foundation

## Objective
Stand up a Microsoft Entra ID (Azure AD) tenant for a small business with an org structure — departments as groups, users, and Multi-factor Authentication (MFA). 

> [!IMPORTANT]
> This first project is the cornerstone that builds the tenant for the remainder of the projects in this series.



## AZ-104 Skills Covered
- Managing Microsoft Entra users and groups
- Managing licenses
- Multi-factor Authentication (MFA)



## Architecture

![Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/01-identity-foundation/01-identity-foundations.svg)


This tenant models a 5 department org structure. Dynamic groups assign membership automatically based on user attribute rather than manual group management, and every user is subject to a MFA registration policy at the tenant level. This mirrors real ERP access administration — automated group assignment reduces manual provisioning overhead while enforcing a consistent baseline security policy on every account from day one.



## What I Built 

1. Create tenant for a small business
    - Created a Azure free account
    - Started a P2 license trail for 30 days to get full access to some of the tools needed to reach the goals of later projects.
2. Setup budget/cost limits
    - Setup a budget
    - Usage limits (I set the cap here to $20 so I am not hit with any surprise monthly charges; however, this is easily scalable to meet any risk management needs of a business.)
    - Email alerts set to send notifications at 50%/75%/90% thresholds.
3. Bulk import users
    - I used Claude to create a CSV that assigned the attributes corresponding to the bulk import template, downloadable from within Azure. All users were assigned a name, department, job title, and principle user name (email@domain.com), and an initial temporary password.
    - 24 test users across 5 departments (IT, Marketing, Sales, Engineering, Customer Service)
4. Build **dynamic groups** based on department/job title attributes.
    - Rather than manual group administration, users were assigned to groups using dynamic membership rules based on their department and/or job title.
    - One Microsoft 365 group was created for each department with a rule to only add users to each group based on the department attribute assigned. Created Microsoft 365 groups instead of security groups allows for each group to have an email and also to allow dynamic rules to be enabled.
    - I made a group for "Leadership" users, using a custom rule, to group users that hold a title containing "Director", "Manager", "Senior", or "Supervisor". (This was done so I could try writing a rule manually versus using the rule builder.)
5. Multi-factor Authentication (MFA) registration policy.
    - All users have MFA enabled by default at the tenant level (This can be expanded on in later projects.)
    - Authentication methods enabled - Passkey(FIDO2), Microsoft Authenticator, Software OATH tokens, Email OTP


## Security/Design Decisions
- I chose to go with a small sized business (<25 employees) since that is the largest grouping of active Azure tenants. 
- Dynamic groups are built based on department/job title attributes. 
- MFA as standard user access controls.



## Challenges & What I'd Do Differently
- The main challenge with this first project was that I did too much overthinking. I thought I needed to have an answer for everything prior to getting started
- Spending too much time trying to mimic a real example of a business. I spent almost 2 hours trying to make sure that the user list created by Claude had enough relevant information thinking it would all be necessary for an easy import. The focus quickly became "what can I get AI to do!?" instead of working in the tenant.
- Forgot to register for the trial tenant license when I was creating the tenant, so I had to stop in the middle of creating groups just to do it all over again.
- I should have read through the documentation more thoroughly prior to creating the dynamic groups. I only skimmed it and I thought it would be more intuitive than it was. However, since I decided to create a dynamic rule for the Leadership group manually rather than using the rule builder I still got the exposure I needed. (FYI -  there are more operators you can use when building rules than shown in the rule builder list.)



## Cost
$0.00
    - **Azure Budgets** with an alert at 50%/75%/90% of a monthly cap. 
    - Azure free account
    - P2 30-day trial license

> [!NOTE]
> Everything in this repo prioritizes using an Azure Free Account, always-free service tiers, and a 30-day trial P2 License. Services that are billed hourly will only be spun up long enough to configure and document.



## Screenshots / Diagrams 


### Budget Creation
![Dynamic Group Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/01-Cost-Environment-Setup-Identity-Foundation/Files/Create%20a%20Budget%201.png)


### Budget Alerts
![Dynamic Group Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/01-Cost-Environment-Setup-Identity-Foundation/Files/Create%20a%20Budget%202.png)


### Group Architecture
![Dynamic Group Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/01-Cost-Environment-Setup-Identity-Foundation/Files/Dynamic%20groups.svg)


### Dynamic Group Rule (Rule Builder)
![Dynamic Group Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/01-Cost-Environment-Setup-Identity-Foundation/Files/Common%20Dynamic%20Rule.png)


### Dynamic Group Rule (Manual Query)
![Dynamic Group Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/01-Cost-Environment-Setup-Identity-Foundation/Files/Leadership%20Dynamic%20Rule.png)
