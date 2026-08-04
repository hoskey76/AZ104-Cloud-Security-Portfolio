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



## What I Built 🚧
**Steps:**
1. Stand up tenant, define a naming convention.
2. Setup budget/cost limit notifications (I set the cap here to $20 so I am not hit with an surprise monthly bill; however, this is easily scalable to meet any risk management needs of a business)
3. Bulk import users
4. 24 test users across 5 departments
5. Build **dynamic groups** based on department/job title attributes.
7. Multi-factor Authentication (MFA) registration policy.
8. Document the group/attribute schema in a diagram.


## Security/Design Decisions
- I chose to go with a small sized business (<25 employees) since that is the largest grouping of active Azure tenants. 
- Dynamic groups are built based on department/job title attributes. 
- MFA as standard user access controls.



## Challenges & What I'd Do Differently 🚧
Spending too much time trying to mimic a "real world" example of a business. 



## Cost
- Set up **Azure Budgets** with an alert at 50%/75%/90% of a monthly cap. 
- Use **B2s burstable VMs** (free-tier eligible, 750 hrs/month for 12 months) and **deallocate (not just stop)** VMs immediately after each lab session — deallocating stops compute billing.
- Prefer **PaaS/serverless free tiers** over always-on VMs wherever project allows it 

> [!NOTE]
> Everything in this repo prioritizes using an Azure Free Account, always-free service tiers, and a 30-day trial P2 License. Services that are billed hourly will only be spun up long enough to configure and document.



## Screenshots / Diagrams 🚧
(redact tenant IDs, subscription IDs, and any real emails/usernames)
