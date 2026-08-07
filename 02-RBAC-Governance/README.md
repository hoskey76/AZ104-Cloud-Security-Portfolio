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
Below are screenshots from the Azure portal where the custom role `Storage Auditor` was defined, this can also be implemented through `Azure CLI`. 


The `JSON` files for this role are located in [Custom-Roles](./Custom-Roles)

#### Basics
Role name and description are defined here.


![Screenshot of create custom role | basics](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Create%20custom%20role-basics.png)


#### Permissions
`actions`,`notActions`,`dataActions`, and `notDataActions` are defined here.


![Screenshot of create custom role | permissions](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Create%20custom%20role-permissions.png)


#### Assignable Scopes
Assigned the `Storage Auditor` role to the management group level.


![Screenshot of create custom role | assignable scopes](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Create%20custom%20role-assignable%20scopes.png)


#### JSON
Defines the path to the permissions. You will not see the full `JSON` for a custom role here since some of the parameters are defined on the previous tabs. If you administer custom roles through `Azure CLI` you will need all the parameters in a `JSON` file, you can see the differences in parameters and the use of wildcards in [Custom-Roles](./Custom-Roles)


![Screenshot of create custom role | JSON](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Create%20custom%20role-JSON.png)


#### Role in PIM
Confirmed role eligibility after it was assigned. Assigned to a user group and to an individual.


![Screenshot of Storage Auditor role in PIM](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Confirm%20Custom%20Role%20in%20PIM.png)


#### Role Settings in PIM
Parameters set for Storage Auditor role.


![Screenshot of Storage Auditor settings in PIM](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/PIM%20Role%20Settings-Storage%20Auditor.png)


### Custom Policy Definitions
Below is only one of the policies that I created as part of the `Workloads` management initiative. This policy requires the tags `Environment` and `Department` for all resources existing in the `Workloads` management group, then flags the resource for non-compliance. The `JSON` files for all policy definitions are located in [Policies](./Policies).

#### Required Tags Policy Definition
This is where basic parameters and policy rule `JSON` is defined in the Azure portal. Just like `Custom Roles` there are a couple more parameters that would need to be set if you chose to implement a policy through `Azure CLI`.


![Screenshot of tag policy definition in azure portal](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Tag%20definition.png)

#### Assignments
Here the policy is assigned at the management group level to `Workloads` as part of the baseline governance initiative.


![Screenshot of assignment in azure portal]()


#### Tag Policy Parameters
Once the policy has been created you can see the parameters defined here, under the `Parameters` tab.

![Screenshot of tag policy parameters in azure portal](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Tag%20policy%20parameters.png)
