# Project 3: Conditional Access & PIM

## Objective
Enforce risk-based, conditional sign-in requirements across the tenant, and apply just-in-time elevation to a **directory-level** administrative role using PIM for Microsoft Entra roles — the identity-plane counterpart to the PIM-for-Azure-resources work already built into Project 2.
 
## AZ-104 Skills Covered
- Manage Microsoft Entra identities and access (Conditional Access fundamentals)
- Entra ID Premium P2 features
 
## Note on scope vs. Project 2
Project 2 already introduced PIM for **Azure resource roles** (the custom Storage Auditor role, assigned as eligible rather than standing). To avoid repeating the same concept, this project deliberately focuses on the two pieces Project 2 didn't cover:
1. **Conditional Access** — policy-based sign-in requirements, which apply regardless of whether PIM is involved at all.
2. **PIM for Microsoft Entra roles** — eligible/time-boxed elevation to a **directory role** (e.g., User Administrator), which is a structurally different assignment surface than Azure resource roles and is what most IAM job postings mean by "PIM."

## Architecture

### Conditional Access
![conditional access diagram image](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/03-Conditional-Access-PIM/03-Conditional-Access-Diagram.svg)

### Privileged Identity Elevation
![Privileged Identity Elevation diagram image](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/03-Conditional-Access-PIM/03-PIM-Elevation-Diagram.svg)

### PIM & Access Review
![PIM & Access Review diagram image](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/docs/architecture-diagrams/03-Conditional-Access-PIM/03-PIM-Elevation-Access-Review-Diagram.svg)

## What I Built
1. **Confirm Entra ID P2 licensing** is still active for the tenant
2. **Conditional Access policies:**
   - Require MFA for all users and any users assigned administrative directory roles.
   - Block legacy authentication protocols tenant-wide.
   - Used **What-IF** tool to confirm scenarios where policy would apply.
   - Test policy with a pilot group prior to tenant-wide enforcement
   
3. **PIM for Microsoft Entra roles:**
   - Enable PIM for a directory role — **User Administrator**
   - Configure the assignment as **eligible**, not active, with a defined maximum activation duration (1 hr), required justification, and an approval step.
   - Test through a full activate → use → auto-expire cycle.
4. **Access review:** run a time-boxed access review against the eligible assignment to confirm the review workflow functions correctly.
5. **Documentation:** capture the full request → approve/justify → time-boxed elevation → expiry flow in screenshots.

## Security/Design Decisions
- MFA-for-admins and legacy-auth-block as the first two policies: these two address the highest-leverage attack paths rather than the most visible ones. Compromised admin credentials are the highest-impact account takeover scenario in any tenant, so requiring MFA specifically on directory role sign-ins (not just a blanket "MFA for everyone" policy) closes that gap first. Blocking legacy authentication matters for a related reason: legacy protocols (POP, IMAP, older Exchange clients) can't enforce MFA at all, so they're a common bypass path around an MFA policy. Doing both together means an attacker can't just fall back to a legacy protocol to route around the MFA requirement.
- User Administrator as the PIM-eligible role: chosen over a higher-privilege role like Global Administrator because it's the role that actually pairs with real work in this environment — managing the Project 1 user/group lifecycle — without granting broader tenant-wide control that this lab has no legitimate use for. This reflects the least-privilege principle at the role-selection level, not just at the assignment-type level: the question isn't only "should this be eligible instead of standing," it's "does this identity need this specific role at all."
- Eligible assignment with justification + MFA + required approval: justification and MFA are the non-negotiable floor on every activation. Approval, by contrast, isn't enabled by default in PIM — it's an explicit choice you have to configure — so requiring it here was a deliberate decision, not a default I left in place. I made that call because even in a single-admin lab, walking through the full requester-and-approver flow was worth the setup to prove the mechanism end-to-end, and it's the setting I'd consider closer to mandatory in any real multi-admin environment for anything above User Administrator-level impact.


## Challenges & What I'd Do Differently
Unlike Projects 1 and 2, nothing significant went wrong here — this project felt closer to day-to-day administrative and governance work than a lab exercise, which was itself a useful signal that the earlier projects' groundwork had actually stuck. Working through Azure resource roles, Conditional Access, and PIM for Microsoft Entra roles side by side made the distinctions between them concrete in a way that reading about them hadn't — particularly how differently "eligible access" behaves depending on whether it's scoped to an Azure resource or a directory role.

The one thing I'd change is documentation format, not the technical work. This project generated far more screenshots than Projects 1 and 2 combined — every policy's report-only test, the What If tool output, the full PIM activation/approval sequence, and the access review outcome all needed separate evidence. For a multi-step workflow like this one, a short screen recording would tell the story more clearly than a long chain of static screenshots, and it's worth switching to that format for any future project with a similar type flow.

## Cost
Expected $0 — Conditional Access and PIM for Microsoft Entra roles are both included with the active Entra ID P2 trial carried over from Project 2. No billable Azure resources are required for this project.


## Screenshots / Diagrams

### Conditional Access
Added 4 policies to serve as base sign-in requirements under conditional access.

#### Policies
![Conditional Access Policies Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Conditional%20access%20-%20policies.png)

#### Policy Example - Block Legacy Authentication
![Block Legacy Authentication Policy Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Conditional%20access%20-%20block%20legacy%20authentication.png)

#### What-IF Tool - Parameters
This tool is extremely useful, kept me from having to manually test functionality. Thanks Claude for the suggestion.
![What-If tool parameters Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/What-If%20Tool%20-%20Parameters.png)

#### What-IF Tool - Results
![What-if tool results Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/What-If%20Tool%20-%20Results.png)

### PIM for Microsoft Entra Roles
Test/confirmation that the `User Administrator` role eligibility/activation workflow works as intended.

#### User - Role Not Active
![User - Role Not Active Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20role%20not%20active.png)

#### User - Role Eligible
![User - Role Eligible Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20role%20eligible.png)

#### User - Activate Role
![User - Activate Role Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20activate%20role.png)

#### User - Role Pending Approval
![User - Role Pending Approval Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20role%20pending%20approval.png)

#### Administrator - Approve Request
![Administrator - Approve Request Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20admin%20approve%20role.png)

#### Administrator - Grant Approval
![Administrator - Grant Approval Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20admin%20approving%20request.png)

#### User - Role Active Assignment
![User - Role Active Assignment Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20role%20now%20active.png)

#### PIM Audit History - Confirmation of Role Expiration
![PIM Audit History - Confirmation of Role Expiration Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/test%20user%20-%20confirm%20PIM%20activation%20expired.png)

### Access Review

#### Creation
![Access Review creation Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Access%20review%20-%20create.png)

#### Results
![Access Review results Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Access%20review%20-%20results.png)

#### Approval
![Access Review approval Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Access%20review%20-%20approval.png)

#### Outcome
![Access Review outcome Screenshot](https://github.com/hoskey76/AZ104-Cloud-Security-Portfolio/blob/main/03-Conditional-Access-PIM/Files/Access%20review%20-%20outcome.png)
