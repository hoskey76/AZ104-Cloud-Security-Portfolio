# Project 2: RBAC & Governance 🚧 

## Objective 🚧 
One or two sentences: what problem this solves / what capability it demonstrates.

## AZ-104 Skills Covered 🚧 
- Bullet list mapped to the official exam skills outline

## Architecture 🚧 
### Management Groups
Created 3 management groups to sit above the subscription. Policy and RBAC assignments made at the management group level apply automatically to everything below without per-resource-group configuration.


![Management Groups Architectural Diagram](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/02-RBAC-Goverance/Management%20Groups%20Diagram.svg)

### Policy
Bundled five policies into a single initiative and assigned it at the `Workloads` management group.


![Policy Initiative Architectural Diagram](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/02-RBAC-Goverance/Baseline%20Policy%20Intiative.svg)


## What I Built 🚧 
Step-by-step narrative — explain *why*, not just *what*.

## Security/Design Decisions 🚧 


## Challenges & What I'd Do Differently 🚧 
Genuine reflection

## Cost 🚧 
What this cost to run, and how you kept it near-zero.

## Screenshots / Diagrams 🚧 
(redact tenant IDs, subscription IDs, and any real emails/usernames)

### Custom Role (Storage Auditor)
The JSON files for this role are located in [Roles](./Custom-Roles)


Insert images of custom role settings from azure portal below


### Custom Policy Definitions
The JSON files for policy definitions are located in [Policies](./Policies)


Insert images of Policy definition from Azure portal below
