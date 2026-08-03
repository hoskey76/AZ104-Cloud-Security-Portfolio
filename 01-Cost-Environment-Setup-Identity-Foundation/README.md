# Project 1: Cost & Environment Setup | Identity Foundation

## Objective
Stand up a Microsoft Entra ID (Azure AD) tenant for a small-medium business with an org structure — departments as groups, users, and self-service password reset. 

> [!IMPORTANT]
> This first project is the cornerstone that builds the tenant for the remainder of the projects in this series.



## AZ-104 Skills Covered
- Manage Microsoft Entra users and groups
- Manage licenses
- Configure self-service password reset (SSPR)
- Configure Multi-factor Authentication (MFA)



## Architecture

![Architecture Diagram - Identity Foundation](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/aa6a43a6ca985f88a88fbe6ee91e8a4e38dae3ec/docs/architecture-diagrams/01-identity-foundation/01-identity-foundation.svg)


This tenant models a 6 department org structure. Dynamic groups assign membership automatically based on the department attribute rather than manual group management, and every user is subject to an SSPR + MFA registration policy at the tenant level. This mirrors real ERP access administration — automated group assignment reduces manual provisioning overhead while enforcing a consistent baseline security policy on every account from day one.



## What I Built 🚧
**Steps:**
1. Stand up tenant, define a naming convention (documented as an ADR — see documentation section).
2. Create 40-50 test users across 5-7 "departments" 
3. Build **dynamic groups** based on department/job title attributes.
4. Configure Singe Sign-On Password Reset (SSPR) and Multi-factor Authentication (MFA) registration policy.
5. Document the group/attribute schema in a diagram.


## Security/Design Decisions
- I chose to go with a small-medium sized business (40-50 employees) since that is the second-largest grouping of active Azure tenants. 
- Dynamic groups are built based on department/job title attributes. 
- Configured SSPR and MFA as standard user access controls.



## Challenges & What I'd Do Differently 🚧
Spending too much time trying to mimic a "real world" example of a business. 



## Cost
- Set up **Azure Budgets** with an alert at 50%/75%/90% of a monthly cap. (I set the cap here to $20 so I am not hit with an surprise monthly bill; however, this is easily scalable to meet any risk management needs of a business)
- Enable **Cost Management + Billing → Cost alerts** via email.
- Use **B1s burstable VMs** (free-tier eligible, 750 hrs/month for 12 months) and **deallocate (not just stop)** VMs immediately after each lab session — deallocating stops compute billing.
- Prefer **PaaS/serverless free tiers** over always-on VMs wherever project allows it 

> [!NOTE]
> Everything in this repo prioritizes using an Azure Free Account and always-free service tiers. Services that are billed hourly will only be spun up long enough to configure and document.



## Screenshots / Diagrams 🚧
(redact tenant IDs, subscription IDs, and any real emails/usernames)
