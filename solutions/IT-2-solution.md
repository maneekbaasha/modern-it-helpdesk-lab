# IT-2 — Solution: New Employee Onboarding with Microsoft Entra ID

## Objective

Provision a new Finance employee in Microsoft Entra ID while following the principle of least privilege and using group-based access management.

---

## 1. Analyze the business requirement

Before creating the account, the employee's actual access requirements were identified.

Lina requires:

- a professional identity
- access to Finance resources
- standard user privileges

She does not require:

- administrative privileges
- HR access
- IT administrative access

This prevents unnecessary permissions from being assigned during onboarding.

---

## 2. Create a department security group

A security group was created:

`Finance-Users`

**Group type:** Security  
**Membership type:** Assigned

The purpose of the group is to manage Finance access centrally instead of assigning permissions individually to each employee.

Access model:

`Lina Martin → Finance-Users → Finance resources`

This makes future onboarding and offboarding easier to manage.

---

## 3. Create the employee identity

A new internal user was created in Microsoft Entra ID.

**Display name:** Lina Martin  
**UPN:** `lina.martin@<tenant>.onmicrosoft.com`  
**User type:** Member  
**Department:** Finance  
**Job title:** Junior Accountant  
**Account status:** Enabled

A consistent `firstname.lastname` naming convention was used for the UPN.

The manager attribute was not populated because the fictional manager did not exist as an identity in the lab tenant.

---

## 4. Assign access

Lina was added to:

`Finance-Users`

No administrative role was assigned.

This follows the principle of least privilege.

Being a Finance employee does not justify administrative access to Microsoft Entra ID.

---

## 5. Validate provisioning

After account creation, the following checks were performed:

- user account exists
- UPN is correct
- account is enabled
- user type is Member
- Finance group membership is present
- no administrative role is assigned

The account was successfully provisioned.

---

## 6. Microsoft 365 licensing limitation

The tenant uses Microsoft Entra ID Free and does not currently contain Microsoft 365 subscriptions or available licenses.

Therefore, creating the Entra identity does not automatically provide:

- an Exchange Online mailbox
- Outlook services
- Microsoft 365 desktop applications
- other licensed Microsoft 365 services

This lab validates identity provisioning and access management only.

No Microsoft 365 functionality is claimed to have been provisioned.

---

## 7. Administrative account discovery

During the lab, the existing administrator account was found to be an external B2B identity.

Its UPN contained:

`#EXT#`

This indicated that a personal Microsoft account had been invited into the tenant as an external identity and granted an administrative role.

The external identity could administer Microsoft Entra but could not be used as a standard internal organizational account for Microsoft 365 administration.

---

## 8. Create a dedicated administrative identity

A separate internal administrative account was created:

`admin.irphan@<tenant>.onmicrosoft.com`

The account was intentionally kept separate from business groups such as:

`Finance-Users`

Administrative privileges and business-resource access represent different requirements and should not be mixed unnecessarily.

The administrative account was then assigned the required administrative role for management of the lab tenant.

---

## 9. Validate administrative access

The new administrative identity was tested independently.

The following were validated:

- Microsoft Entra administration access
- Microsoft 365 Admin Center access
- internal Member identity
- administrative role assignment

The original external administrator was kept temporarily as a recovery path while the new administrative account was tested.

Administrative access should never be removed from the only working administrator before a replacement account has been validated.

---

## 10. Multi-Factor Authentication

The dedicated administrative account was checked for additional authentication protection.

An authentication application using TOTP was registered.

A private browsing session was then used to perform a real authentication test.

The login required:

1. the account password
2. the additional authentication factor

MFA was therefore validated successfully for the privileged account.

---

## Security principles practiced

### Least Privilege

Users receive only the access required for their role.

### Group-Based Access Management

Permissions should be associated with reusable groups rather than manually assigned to every employee.

### Role-Based Access Control

Administrative capabilities should only be assigned when required.

### Separation of Duties

Administrative identities should remain separate from normal business access where possible.

### Privileged Account Protection

Administrative accounts require stronger authentication controls such as MFA.

### Safe Administrative Changes

Existing administrative access should remain available until replacement access has been successfully tested.

---

## Key lessons

An employee account is not simply a username and password.

A proper onboarding process considers:

`Identity → Attributes → Groups → Permissions → Licensing → Security → Validation`

Microsoft Entra ID provides the identity layer.

Microsoft 365 licenses provide access to additional services.

These concepts should not be confused.

---

## Skills practiced

- Microsoft Entra ID
- Identity and Access Management (IAM)
- User provisioning
- User Principal Names (UPN)
- Security groups
- Group-based access
- Least Privilege
- Role-Based Access Control (RBAC)
- B2B identities
- Internal vs external identities
- Administrative account separation
- Microsoft 365 licensing concepts
- Multi-Factor Authentication
- TOTP
- Privileged account security
- Onboarding validation
