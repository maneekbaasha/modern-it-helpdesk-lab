
# Modern IT Helpdesk Lab

A beginner-friendly lab designed to simulate common IT Helpdesk tasks using tools and workflows found in real support environments.

The objective is not only to solve technical issues, but to learn how to:

- understand a user's problem
- qualify and prioritize a ticket
- ask relevant troubleshooting questions
- isolate the source of an incident
- apply the least disruptive fix
- validate the resolution
- document the intervention professionally

## Tools covered

This lab will progressively introduce:

- Jira Service Management
- Microsoft Entra ID
- Microsoft Intune
- Microsoft 365
- Windows troubleshooting
- Apple Business Manager and MDM concepts

Some services may require free trials or simulated steps when a real enterprise tenant is not available.

## Lab structure

Each ticket contains:

1. A user request or incident
2. A troubleshooting exercise
3. A separate solution and explanation

The solution files are intentionally separated to avoid spoilers.

## Current labs

### IT-1 — Outlook authentication loop

Scenario:

A user changed their Microsoft password and Outlook repeatedly asks for authentication.

Topics covered:

- Jira Service Management
- urgency vs impact
- troubleshooting methodology
- Microsoft 365 authentication
- Windows Credential Manager
- ticket documentation

## Troubleshooting methodology

The labs use the following workflow:

1. Understand the symptoms
2. Determine the scope
3. Identify recent changes
4. Form a hypothesis
5. Test the hypothesis
6. Apply the smallest appropriate change
7. Validate the result
8. Document the resolution

### IT-2 — New employee onboarding with Microsoft Entra ID

Scenario:

HR requests the onboarding of a new Finance employee.

Topics covered:

- Microsoft Entra ID
- Identity and Access Management
- user provisioning
- security groups
- Least Privilege
- RBAC
- internal and B2B identities
- administrative account separation
- MFA
- Microsoft 365 licensing concepts

### IT-3 — Non-compliant Windows device with Microsoft Intune

Scenario:

A managed Windows 11 corporate device becomes non-compliant after Microsoft Defender Firewall is intentionally disabled on the active network profile.

Topics covered:

- Microsoft Intune compliance policies
- Windows 11 endpoint management
- Microsoft Defender Firewall
- device compliance monitoring
- pilot device groups
- policy assignment
- MDM synchronization
- security drift detection
- Intune reporting
- remediation and validation

### IT-4 — Software deployment through Microsoft Intune

Scenario:

A managed Windows device receives a centrally deployed application through Intune. An initial VLC deployment fails during applicability/detection evaluation, leading to log analysis and a successful deployment test using Microsoft PowerToys.

Topics covered:

- Microsoft Intune application management
- Microsoft Store apps
- Win32 application deployment
- required assignments
- pilot device groups
- Intune Management Extension
- AppWorkload.log analysis
- deployment troubleshooting
- application applicability
- ARM64 compatibility considerations
- endpoint validation
- Intune reporting

## Planned labs

- IT-5 — macOS device management and Apple Business Manager concepts
- IT-6 — Lost corporate device incident

## Disclaimer

This project is intended for educational purposes and uses fictional users, organizations and incidents.

maneekbaasha
