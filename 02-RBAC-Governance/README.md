# Project 2: RBAC & Governance 🚧 

## Objective
Design and enforce a least-privilege access model across a multi-level Azure environment — using management group hierarchy, a custom RBAC role, and Azure Policy — rather than relying on Azure's built-in roles and default (unrestricted) subscription behavior.

## AZ-104 Skills Covered
- Manage role-based access control (RBAC)
- Create and manage custom RBAC role definitions
- Manage subscriptions and governance:
    - Management groups
    - Resource tags
    - Azure Policy
    - Resource locks

## Architecture 


This builds directly on Project 1's identity foundation: the departments and dynamic groups created there are the security principals this project assigns access to. A management group hierarchy (`Sandbox` → `Security` → `Workloads`) sits above the subscription, giving policy and role assignments a place to live above individual resource groups. A custom role — **Storage Auditor** — is scoped so an auditor-type identity can read storage configuration without being able to modify it, which the built-in Reader/Contributor roles don't cleanly express. With an Entra ID P2 trial active for this project series, the Storage Auditor role is assigned through **Privileged Identity Management for Azure resources** as an eligible (not standing) assignment, so the "auditor" identity has no access at all until it actively activates the role for a time-boxed window — a deliberately stronger starting point than Project 3 originally called for. An Azure Policy initiative bundling five policies (require a tag, deny public blob access, restrict allowed locations, restrict allowed VM SKUs, and enforce HTTPS-only storage) is assigned at the management group level so it inherits down to every subscription and resource group underneath, rather than being reapplied resource by resource.


### Management Groups
Created 3 management groups to sit above the subscription. Policy and RBAC assignments made at the management group level apply automatically to everything below without per-resource-group configuration.


![Management Groups Architectural Diagram](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/02-RBAC-Goverance/Management%20Groups%20Diagram.svg)

### Policy
Bundled five policies into a single initiative and assigned it at the `Workloads` management group.


![Policy Initiative Architectural Diagram](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/02-RBAC-Goverance/Baseline%20Policy%20Intiative.svg)


## What I Built


1. **Management group hierarchy** — created `Sandbox`, `Security`, and `Workloads` management groups and moved my subscription under `Workloads`, so policy and RBAC assignments made at the management group level apply automatically to everything below without per-resource-group configuration.

2. **Custom RBAC role (`Storage Auditor`)** — wrote a role definition JSON with `Microsoft.Storage/storageAccounts/read` and related read-only actions, explicitly excluding write/delete actions. Rather than assigning it directly to a test user from Project 1's IT department group, I onboarded the subscription to **PIM for Azure resources** and made the assignment **eligible** instead of active — the test account has zero standing access until it requests activation, and the assignment auto-expires after a set duration. Confirmed the account could view storage configuration only during an activated window, and had no access outside of it.

3. **Azure Policy initiative** — bundled five policies into a single initiative and assigned it at the `Workloads` management group:
   - Require a specified tag (`environment`) on all resources
   - Deny public network access to storage accounts
   - Allowed locations (restricted to one region, to also keep costs/data residency predictable)
   - Allowed VM SKUs (restricted to burstable/free-tier-eligible sizes, tying governance directly into cost control)
   - Require secure transfer (HTTPS-only) on storage accounts

4. **Compliance testing** — deliberately created one non-compliant resource (a storage account without the required tag) to confirm the policy flagged it, then remediated it and re-checked compliance state.


## Security/Design Decisions


- **Management groups over per-subscription policy assignment:** I chose to assign policy at the management group level instead of the subscription level, even with only one subscription in this lab, because it's the pattern that scales — in a real environment with multiple subscriptions (e.g., separate dev/prod), this is the only way to guarantee consistent governance without manually re-applying policy to each one.
- **Deny vs. Audit effect:** I used a `Deny` effect for the public storage access policy (blocks non-compliant resources outright) but an `Audit` effect for the tagging policy (flags but doesn't block) — this reflects a real tradeoff between enforcing hard security boundaries versus allowing softer governance nudges that don't block legitimate work while a tagging convention is still being adopted.
- **Eligible (PIM) assignment over a standing role assignment:** with the P2 trial active for the whole series, I chose to make the Storage Auditor assignment eligible rather than active from the start, even though PIM for Azure resources isn't formally introduced until Project 3. A permanent RBAC assignment is standing access whether or not it's being used at any given moment; an eligible assignment means the access literally doesn't exist until someone requests and justifies it. For a role built specifically to model least privilege, a standing assignment would have undercut the point of the project.
- **Custom role instead of built-in Reader:** the built-in Reader role grants read access to *all* resource types in scope, not just storage. A custom role scoped specifically to storage read actions is a better real-world least-privilege fit for an auditor persona who has no legitimate reason to see network, compute, or identity configuration.
- **Restricting allowed VM SKUs at the policy level:** this doubles as both a governance control and a cost control — it makes it structurally impossible for a future project (or a mistaken portal click) to accidentally deploy an expensive VM size in this lab environment.


## Challenges & What I'd Do Differently
The biggest challenge was a scoping issue with PIM. I initially assigned the Storage Auditor role at the management group level, and the role wasn't showing up when I went to manage it through PIM — even after confirming the assignment existed for the correct user and group. The root cause: the role definition includes `notDataActions`, which Azure only supports at the subscription level, not the management group level. On top of that, I was managing the assignment from the wrong scope in the PIM console itself, which masked the real issue for a while. Tracking this down took a few hours of working through Microsoft's documentation and testing configuration changes systematically until I isolated the actual cause. It was a good reminder that RBAC scope requirements aren't always symmetric across resource types — what works at one scope for a built-in role doesn't necessarily hold for a custom one with data-action restrictions.

Looking back, there are two things I'd change. First, I'd deploy the RBAC and policy definitions through the Azure CLI from the start rather than the portal — it's the more repeatable approach and the one I'd actually use in production. That said, working through the portal directly surfaced more of these scope-and-configuration "gotchas" than a scripted deployment would have, which arguably made this a better learning exercise for the exam even if it's not how I'd do it operationally. Second, I'd expose the initiative's policy effects as top-level initiative parameters instead of hardcoding them into each policy reference. As deployed, changing any policy's effect later means editing and redeploying the initiative definition itself rather than just adjusting the assignment — a small design choice upfront that would have made the whole initiative easier to tune going forward.

Even knowing the CLI would mean less friction, I'm planning to keep working through the portal for the rest of this series, specifically for the exposure it gives me navigating Azure's UI. Where I notice a meaningful difference between the portal and CLI approach, I'm calling it out directly in these READMEs — it reinforces the distinction in my head and helps commit the CLI equivalents to memory rather than just reading about them. I expect fewer scoping surprises like this one going forward, but if they come up, they're a legitimate part of the learning process.

## Cost


$0. Management groups, RBAC role definitions/assignments, Azure Policy, and PIM for Azure resources are all included at no extra cost with an active Entra ID P2 license (this lab used the 30-day P2 trial, shared across this project and Project 3). The only resource created for testing (a small storage account) was deleted after confirming compliance behavior.


## Screenshots / Diagrams 🚧 


### Custom Role (Storage Auditor)
Below are screenshots from the Azure portal where the custom role `Storage Auditor` was defined, this can also be implemented through `Azure CLI`. The `JSON` files for this role are located in [Custom-Roles](./Custom-Roles)

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


#### Tag Policy Parameters
Once the policy has been created you can see the parameters defined here, under the `Parameters` tab.

![Screenshot of tag policy parameters in azure portal](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/02-RBAC-Governance/Files/Tag%20policy%20parameters.png)


### Policy Initiative
This initiative takes the 5 policies I created and bundles them together, assigns the initiative to `Workloads` and groups the policies together based on common attributes.

#### Basics
Define location, name, and description


![Screenshot of initiative definition basics in azure portal]

#### Policy
Add policy definitions to initiative


![Screenshot of initiative definition policy in azure portal]

#### Groups
Policy groups help to organize


![Screenshot of initiative definition groups in azure portal]

#### Confirmation

![Screenshot of initiative definition confirmation in azure portal]
