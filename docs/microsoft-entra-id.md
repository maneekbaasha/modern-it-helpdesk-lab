# Microsoft Entra ID — Helpdesk & IAM Fundamentals

Microsoft Entra ID is Microsoft's cloud-based identity and access management service.

It allows organizations to manage identities, authentication and access to resources.

---

## Identity

An identity represents a user, application, device or service that can be authenticated.

For an employee, an Entra identity can contain:

- display name
- User Principal Name (UPN)
- department
- job title
- manager
- group memberships
- assigned roles
- authentication methods

---

## User Principal Name (UPN)

The UPN is commonly used as the user's sign-in identifier.

Example:

`lina.martin@company.com`

A consistent naming convention makes identity management easier.

Common convention:

`firstname.lastname@company.com`

---

## Member vs External User

### Member

A Member normally represents an identity belonging to the organization.

Example:

`lina.martin@company.com`

### External / B2B Identity

An external identity represents someone whose original identity comes from outside the tenant.

External identities can be invited to collaborate with an organization.

Their internal Entra representation may contain:

`#EXT#`

External identities and internal employee identities should not be confused.

---

## Security Groups

Security groups allow administrators to manage access for multiple users.

Instead of:

`User → Permission`

prefer:

`User → Security Group → Permission`

Example:

`Lina Martin → Finance-Users → Finance resources`

Benefits include:

- easier onboarding
- easier offboarding
- consistent permissions
- reduced administrative work
- fewer permission mistakes

---

## Assigned Membership

With assigned membership, an administrator explicitly adds or removes users from a group.

Example:

`Administrator → Add Lina → Finance-Users`

---

## Dynamic Membership

Dynamic groups can automatically determine membership based on user attributes when supported by the organization's licensing and configuration.

Conceptual example:

`department = Finance → Finance-Users`

This can automate onboarding and role changes in larger environments.

---

## Roles vs Groups

Groups and administrative roles serve different purposes.

### Group

Represents access to resources or a business function.

Example:

`Finance-Users`

### Administrative role

Grants administrative capabilities inside Microsoft services.

Examples include:

- Global Administrator
- User Administrator
- Groups Administrator
- Intune Administrator

An employee's department does not justify an administrative role.

---

## Least Privilege

Users and administrators should receive only the permissions necessary to perform their tasks.

Example:

A Finance employee needs Finance resources.

They do not automatically need:

- HR resources
- IT resources
- administrative privileges

---

## RBAC

Role-Based Access Control assigns privileges according to responsibilities.

Instead of giving every technician maximum privileges, organizations can assign specialized administrative roles.

Conceptually:

`Helpdesk technician → appropriate support role`

rather than:

`Helpdesk technician → Global Administrator`

---

## Administrative Accounts

Privileged administration should be separated from normal business activity where practical.

Example:

`firstname.lastname@company.com`

for normal work

and:

`admin.firstname@company.com`

for privileged administration.

The administrative account should not automatically belong to business groups simply because the same person owns both identities.

---

## Multi-Factor Authentication

MFA requires more than one authentication factor.

Example:

`Password + Authenticator`

A compromised password alone is therefore insufficient to authenticate when MFA is enforced.

Privileged accounts are particularly important to protect with MFA.

---

## Authentication Method vs MFA Enforcement

Registering an authentication method does not necessarily mean MFA will be requested for every authentication.

Two concepts must be distinguished:

**Authentication method**

How the user can prove their identity.

**Authentication policy**

When additional authentication is required.

---

## Microsoft Entra ID vs Microsoft 365

Microsoft Entra ID provides the identity layer.

Creating an Entra user does not automatically create every Microsoft 365 service.

Conceptually:

`Entra ID → Identity`

while licensed services can provide:

`Exchange Online → Mailbox`

`Microsoft 365 → Productivity services`

`Intune → Endpoint management`

Licensing must therefore be checked during onboarding.

---

## Onboarding Checklist

Before completing an employee onboarding:

1. Verify the HR request.
2. Identify required resources.
3. Create or verify the identity.
4. Configure employee attributes.
5. Assign appropriate groups.
6. Avoid unnecessary administrative roles.
7. Check required licenses.
8. Configure required authentication controls.
9. Validate access.
10. Document the onboarding.

---

## Offboarding Principle

Identity management also applies when an employee leaves.

A typical offboarding workflow may include:

`Disable identity → Revoke sessions → Remove access → Handle devices → Document`

The exact workflow depends on the organization's policies and services.

---

## Core Principle

**Identity → Authentication → Authorization → Access**

Ask four questions:

**Who are you?**

Identity.

**Can you prove it?**

Authentication.

**What are you allowed to do?**

Authorization.

**Which resource can you actually use?**

Access.
