# Azure Cloud Security & IAM Portfolio — AZ-104 Project Series

**Live portfolio site:** [Coming Soon](#) &nbsp;|&nbsp; **LinkedIn:** [Rashaun Hoskey](https://www.linkedin.com/in/rashaun-hoskey/) &nbsp;|&nbsp; <!-- **Resume:** [resume.pdf](#) -->

---

## About This Repo

I'm a security/IAM professional (currently completing the Microsoft AZ-104: Azure Administrator certification) building toward roles in **Identity & Access Management, Cloud Security, and AI-adjacent security engineering**. This repo is a series of Azure projects — each one deployed, documented, and (mostly) torn back down to stay within free-tier limits — that together cover the full AZ-104 exam scope while going deeper on the identity, security, and automation work those target roles actually care about.

Every project follows the same pattern: **build it → secure it → document why → tear down what costs money.** Nothing here is a screenshot of a tutorial — each README explains the design decisions and tradeoffs I made, including where I chose a cheaper/simpler option over the "textbook" one and why.

This work extends operational IAM experience I already have administering ERP access and RBAC with business units — the goal here was to take that hands-on background and rebuild it, from scratch, in a documented, reproducible, Azure-native form.

After studying inconsistently through the first half of 2026, I decided to take a more serious, structured approach to building Azure skills. I spent time working with AI tools (ChatGPT and Claude) to design the framework behind this project series — mapping each project to specific AZ-104 exam objectives and to the skills that matter most for IAM, cloud security, and AI-adjacent roles — and then built it out hands-on from there.

> [!IMPORTANT]
> This portfolio is a work in progress. Some directories, pages, or links below may be incomplete or not yet live.
---

## Project Index

| # | Project | Core Skills | Status |
|---|---------|-------------|--------|
| 01 | [Cost & Environment Setup / Identity Foundation](./01-Cost-Environment-Setup-Identity-Foundation) | Entra ID users/groups, dynamic groups, MFA | ✅ |
| 02 | [RBAC & Governance](./02-RBAC-Governance) | Management groups, custom RBAC roles, Azure Policy | 🚧 |
| 03 | [Conditional Access & PIM](./03-conditional-access-pim) | Conditional Access, Privileged Identity Management, access reviews | 🚧 |
| 04 | [Secure Storage Design](./04-secure-storage) | SAS tokens, CMK encryption, lifecycle management | 🚧 |
| 05 | [Hub-Spoke Network Security](./05-hub-spoke-network) | VNets, NSGs, UDRs, segmentation | 🚧 |
| 06 | [Hardened Compute Baseline](./06-compute-baseline) | Defender for Cloud, JIT access, disk encryption | 🚧 |
| 07 | [Monitoring & Sentinel (SIEM)](./07-monitoring-sentinel) | Log Analytics, KQL, Sentinel analytics rules | 🚧 |
| 08 | [Backup & Disaster Recovery](./08-backup-recovery) | Recovery Services Vault, restore testing, DR design | 🚧 |
| 09 | [Governance-as-Code](./09-iac-bicep) | Bicep, PowerShell, GitHub Actions CI | 🚧 |
| 10 | [AI-Assisted Security Triage](./10-ai-triage-assistant) | Azure AI/OpenAI, alert summarization & prioritization | 🚧 |
| 11 | [Access Review Capstone](./11-access-review) | Audit simulation, findings & remediation report | 🚧 |
| 12 | [Portfolio Static Website](./12-portfolio-site) | Azure Static Web Apps, CI/CD deployment | 🚧 |

<!-- (Mark projects in progress with 🚧 and update as they are completed — the build timeline is part of the story.) -->

---

## Why These Projects

- **IAM roles:** Projects 1, 2, 3, and 11 form a full identity lifecycle — provisioning, least-privilege access design, just-in-time elevation, and periodic access review/remediation.
- **Cloud security roles:** Projects 4, 5, 6, and 7 cover the core defense-in-depth layers (data, network, compute, detection) with explicit threat/cost reasoning behind each control.
- **AI-adjacent roles:** Project 10 applies AI directly to a security operations problem (alert triage), with an honest discussion of where a human review step stays non-negotiable.
- **Engineering maturity:** Project 9 re-platforms the earlier manual work into Bicep/PowerShell/CI, showing this isn't a one-time lab but reproducible infrastructure.

---

## How to Navigate This Repo

Each project folder contains its own README with:
- **Objective** — what it demonstrates
- **Architecture** — diagram + design rationale
- **What I Built** — step-by-step narrative
- **Security/Design Decisions** — explicit tradeoffs (especially cost vs. "production-grade")
- **Challenges & What I'd Do Differently**
- **Cost** — what it actually cost to run

`docs/architecture-diagrams/` holds the full-size diagrams referenced across projects. `09-iac-bicep/` holds the scripts that automate projects.

---

## Tech & Services Used

Microsoft Entra ID · Azure RBAC · Azure Policy · Azure Storage · Key Vault · Virtual Network · NSGs · Azure Firewall · Azure Bastion · Virtual Machines · Microsoft Defender for Cloud · Log Analytics · Microsoft Sentinel · Recovery Services Vault · Bicep · Azure CLI/PowerShell · GitHub Actions · Azure AI Services/Azure OpenAI · Azure Static Web Apps

---

## Cost Note

Everything in this repo was built and documented using the Azure Free Account and always-free service tiers. Where a service is billed hourly (Azure Firewall, Bastion, Private Endpoints), I spun it up only long enough to configure and document it, then tore it down — each affected project's README calls this out explicitly along with what the equivalent always-on production configuration would look like.

---

## Contact

Open to IAM, cloud security, and AI-security roles. Reach out via [LinkedIn](https://www.linkedin.com/in/rashaun-hoskey/) or [Email](rashaun.hoskey@gmail.com).
