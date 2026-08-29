# IT-2 — New Employee Onboarding with Microsoft Entra ID

**Difficulty:** Beginner  
**Category:** Service Request  
**Environment:** Microsoft Entra ID  
**Topic:** Identity and Access Management (IAM)

---

## Scenario

You are an IT Support Technician at NovaTech.

The HR department submits the following request.

### HR request

**Subject:** New employee onboarding — Finance

Lina Martin is joining the Finance department as a Junior Accountant.

Please prepare her professional account and the access required for her role.

**Employee information:**

| Field | Value |
|---|---|
| Name | Lina Martin |
| Department | Finance |
| Job title | Junior Accountant |
| Manager | Sofia Martin |
| Account type | Internal employee |

---

## Business requirements

Lina needs:

- a professional identity
- access to resources required by the Finance department
- standard user privileges

She must not receive:

- administrative privileges
- access to HR resources
- access to IT administrative resources
- unnecessary permissions

---

## Your mission

Provision Lina's identity while following the principle of least privilege.

Before creating the account, determine:

1. What resources does the employee actually need?
2. Should permissions be assigned directly or through groups?
3. What naming convention should be used for the account?
4. What employee attributes should be recorded?
5. Does the employee require any administrative role?
6. How will you validate the provisioning?

---

## Access model

Instead of assigning Finance permissions individually to every employee, design a reusable group-based access model.

Example:

Employee → Department security group → Department resources

---

## Investigation notes

### Identity

**UPN:**

-

**Account type:**

-

**Department:**

-

**Job title:**

-

### Access

**Security groups:**

-

**Administrative roles:**

-

### Validation

Verify:

- account exists
- account is enabled
- correct UPN is configured
- employee attributes are correct
- correct group membership is present
- no unnecessary administrative role is assigned

---

## Security considerations

Consider the following principles during the exercise:

- Least Privilege
- Role-Based Access Control (RBAC)
- Separation of administrative and business accounts
- Group-based access management
- Multi-Factor Authentication for privileged accounts

---

## Ticket documentation challenge

Once onboarding is complete, document:

- identity created
- groups assigned
- privileges assigned
- validation performed
- any limitations encountered in the lab environment

---

> Do not open the solution until you have completed the exercise.

Solution: `../solutions/IT-2-solution.md`
