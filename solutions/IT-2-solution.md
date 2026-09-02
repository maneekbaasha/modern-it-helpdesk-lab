# IT-2 — Solution: New Employee Onboarding with Microsoft Entra ID and Intune

## Objective

Provision a new Finance employee, secure the identity, prepare Microsoft Intune, join a Windows 11 corporate device to Microsoft Entra ID, and enroll the device into Intune.

This walkthrough documents **what was done, what each step means, and which problem it solves**.

---

## 1. Analyze the business requirement

Before creating the account, the employee's actual access requirements were identified.

Lina requires:

- a professional identity
- access to Finance resources
- standard user privileges
- a managed corporate Windows device

She does not require:

- administrative privileges
- HR access
- IT administrative access

**Meaning:** access should follow the employee's real business responsibilities.

**Problem solved:** prevents excessive permissions from being granted during onboarding.

---

## 2. Create a department security group

A security group was created:

`Finance-Users`

**Group type:** Security  
**Membership type:** Assigned

Access model:

`Lina Martin → Finance-Users → Finance resources`

**Meaning:** permissions can be managed through reusable groups instead of assigning access user by user.

**Problem solved:** simplifies onboarding, role changes and offboarding while reducing permission mistakes.

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

The manager attribute was left empty because the fictional manager did not exist as an identity in the lab tenant.

**Meaning:** Microsoft Entra ID becomes the central identity layer for the employee.

**Problem solved:** provides a centrally managed organizational identity instead of relying on a local or personal account.

---

## 4. Assign business access with least privilege

Lina was added to:

`Finance-Users`

No administrative role was assigned to her.

**Meaning:** business access and administrative privileges are separate requirements.

**Problem solved:** being a Finance employee does not accidentally grant IT or tenant administration privileges.

---

## 5. Validate identity provisioning

The following checks were performed:

- user account exists
- UPN is correct
- account is enabled
- user type is Member
- Finance group membership is present
- no administrative role is assigned

**Meaning:** onboarding is not complete until the resulting identity is verified.

**Problem solved:** catches configuration errors before the account is used in later steps.

---

## 6. Discover and separate the administrative identity

During the lab, the original administrator was found to be an external B2B identity. Its Entra representation contained `#EXT#`.

A dedicated internal administrative identity was then created:

`admin.irphan@<tenant>.onmicrosoft.com`

It was deliberately kept separate from business groups such as `Finance-Users`.

The original external administrator was temporarily retained as a recovery path until the new account had been successfully tested.

**Meaning:** privileged administration and normal business access are separate security contexts.

**Problem solved:** reduces accidental use of privileged access and avoids removing the only working administrator before a replacement has been validated.

---

## 7. Protect privileged authentication with MFA

An authentication application using TOTP was registered for the dedicated administrative account.

A private browser session was used to validate a real sign-in requiring:

1. the password
2. an additional authentication factor

Lina also completed MFA registration when required during the Windows onboarding process.

**Meaning:** authentication no longer relies on a password alone.

**Problem solved:** a stolen password by itself is insufficient when MFA is required.

---

## 8. Add the licenses required for endpoint management

The lab was extended from identity provisioning to endpoint management. Trial licensing was activated to provide the required capabilities, including:

- Microsoft Intune
- Microsoft Intune Suite
- Microsoft Entra ID Premium P2

The relevant licenses were assigned to Lina before device enrollment.

**Meaning:** Entra ID provides identity capabilities while Intune provides endpoint-management capabilities; licenses determine which services are available to the user and tenant.

**Problem solved:** prevents attempting MDM enrollment with a user who does not have access to the required services.

> The trials used in this lab are temporary and must be reviewed before any paid renewal.

---

## 9. Create a controlled Intune pilot group

A dedicated security group was created:

`Intune-Pilot-Users`

**Group type:** Security  
**Membership type:** Assigned  
**Member:** Lina Martin

Automatic MDM enrollment was configured with a **Selected/Some users** scope targeting `Intune-Pilot-Users` rather than the entire tenant.

**Meaning:** Intune can be introduced to a small pilot population before a wider rollout.

**Problem solved:** limits the blast radius of configuration mistakes during testing.

---

## 10. Configure Microsoft Entra device settings

For the lab, users were allowed to join devices to Microsoft Entra ID.

The setting that automatically adds the enrolling user to the local Administrators group was changed so that new enrolling users would not intentionally receive local administrator rights through that option.

A local administrator account named `LabUser` was retained on the VM as a controlled recovery account.

**Meaning:** device enrollment and local administrative privilege are separate decisions.

**Problem solved:** a normal employee does not need permanent local administrator rights simply because they enroll a corporate device.

---

## 11. Prepare the Windows 11 endpoint

A Windows 11 Pro ARM64 virtual machine was created with UTM on Apple Silicon.

The machine was renamed:

`NOVATECH-W11-01`

Windows Update was completed and the local `LabUser` account was retained for lab administration and recovery.

**Meaning:** the VM represents a reproducible NovaTech corporate workstation.

**Problem solved:** provides a realistic managed endpoint without requiring a physical enterprise PC.

---

## 12. Start Microsoft Entra Join

From Windows:

`Settings → Accounts → Access work or school → Connect → Join this device to Microsoft Entra ID`

Lina's organizational identity was used for the join.

The initial attempts failed with:

`80180003`

Windows reported that the user was not authorized to be associated with the device.

**Meaning:** the client displayed a generic enrollment authorization failure, not the actual root cause.

**Problem solved:** none yet — this became the troubleshooting scenario.

---

## 13. Troubleshoot error 80180003 systematically

Instead of changing random settings, the enrollment path was checked layer by layer.

### Checks performed

- Windows MDM platform enrollment was allowed
- personal ownership was not blocked for this lab
- Entra users were allowed to join devices
- the configured device enrollment limit was not expected to be reached
- Lina was a direct member of `Intune-Pilot-Users`
- automatic MDM enrollment scope targeted `Intune-Pilot-Users`
- Lina had the required Entra and Intune licenses

Microsoft's recommended troubleshooting step for the Windows MDM platform restriction was also tested by temporarily switching Windows MDM enrollment to **Block**, saving, switching it back to **Allow**, saving again, and waiting for propagation.

The error still occurred.

**Meaning:** the common user, licensing, group and enrollment-restriction causes were being eliminated one by one.

**Problem solved:** narrows the investigation instead of introducing unrelated changes.

---

## 14. Check Windows device-registration state

The device state was checked with:

```powershell
dsregcmd /status
```

Before the successful join, the important values were:

```text
AzureAdJoined : NO
EnterpriseJoined : NO
DomainJoined : NO
WorkplaceJoined : NO
```

**Meaning:** Windows was not currently joined or workplace-registered through the tested account path.

**Problem solved:** reduces the likelihood that an active stale Entra join was causing the failure.

---

## 15. Check Windows MDM event logs

The following Event Viewer log was inspected:

`Applications and Services Logs → Microsoft → Windows → DeviceManagement-Enterprise-Diagnostics-Provider → Admin`

Only older events were present; no new event corresponding to the latest evening enrollment attempt appeared there.

An older Event ID `844` mentioned an incorrect enrollment detected during policy merge, but it did not match the timestamp of the current failed attempt and was therefore not treated as the root cause.

**Meaning:** timestamps matter when correlating logs with a specific incident.

**Problem solved:** avoids incorrectly blaming an older, unrelated event.

---

## 16. Use Intune's Enrollment Failures report

The investigation then moved to:

`Intune → Devices → Monitor → Enrollment failures`

The failed Lina attempts appeared with the failure status:

`Not onboarded into Intune`

Opening the failure details revealed the decisive message:

`Intune mobile device management (MDM) authority is not configured yet.`

**Meaning:** the user and device were reaching Intune, but the tenant itself had not yet been configured to use Intune as its MDM authority.

**Problem solved:** identifies the actual root cause hidden behind Windows error `80180003`.

---

## 17. Confirm the tenant-level problem

In:

`Intune → Tenant administration → Tenant status`

The MDM authority was displayed as:

`Unknown / Inconnu`

The dedicated administration account was also assigned the Microsoft Entra **Intune Administrator** role during troubleshooting so the lab used an appropriate Intune administration role. This role assignment did not by itself change the MDM authority.

**Meaning:** administrative permissions and MDM-authority initialization are separate concepts.

**Problem solved:** prevents confusing a permissions problem with a tenant initialization problem.

---

## 18. Configure Microsoft Intune as the MDM authority

The Microsoft Intune MDM-authority settings page was opened from Microsoft's setup documentation.

The page offered:

- Intune MDM Authority
- Configuration Manager MDM Authority
- None

`None` was selected before the change.

For this Intune-only lab, the following option was selected:

`Intune MDM Authority`

After the change, Tenant Status displayed Intune as the MDM authority.

**Meaning:** Microsoft Intune is now the tenant service responsible for device management.

**Problem solved:** removes the tenant-level blocker that prevented MDM enrollment.

---

## 19. Retry Microsoft Entra Join

The same Windows Entra Join flow was retried with Lina.

This time Windows displayed the successful completion screen and confirmed that the device was connected to the organization's directory.

**Meaning:** `NOVATECH-W11-01` now has an organizational device identity in Microsoft Entra ID.

**Problem solved:** the workstation is no longer only a standalone local Windows machine.

---

## 20. Validate automatic Intune enrollment from Windows

In:

`Settings → Accounts → Access work or school → connected account → Info`

Windows displayed a Microsoft management server address and the device synchronization completed successfully.

**Meaning:** the device has an active MDM management relationship and can communicate with the management service.

**Problem solved:** confirms that Entra Join was followed by Intune enrollment rather than only creating a directory device object.

---

## 21. Validate the device in Microsoft Intune

After propagation, the device appeared in:

`Intune → Devices → All devices`

Validated values included:

- Device name: `NOVATECH-W11-01`
- Primary user: Lina Martin
- Enrolled by: Lina Martin
- Ownership: Corporate
- Operating system: Windows
- Model: QEMU Virtual Machine
- Initial compliance state: Compliant
- successful device check-in

**Meaning:** Intune now recognizes the VM as a managed corporate Windows endpoint associated with Lina.

**Problem solved:** central IT can inventory, configure, evaluate and manage the endpoint.

---

## 22. Validate local administrator separation

The local Administrators group was inspected from an elevated PowerShell session:

```powershell
Get-LocalGroupMember -Group "Administrateurs"
```

The result confirmed that:

- the built-in local Administrator account was present
- `NOVATECH-W11-01\LabUser` remained a local administrator
- Lina was not displayed by name as a direct local administrator entry
- Entra-related SID entries were also present

The unidentified Entra SID entries were not removed because their exact purpose had not been established.

**Meaning:** the known local recovery administrator remains available, while unknown directory principals are not modified blindly.

**Problem solved:** preserves a safe recovery path and avoids destructive privilege changes based on assumptions.

> This check does **not** claim that every SID entry was fully resolved to a named identity. The lab only records what was actually observed.

---

## Final workflow

```text
Business requirement
        ↓
Microsoft Entra ID identity
        ↓
Finance-Users
        ↓
MFA and authentication controls
        ↓
Intune licensing
        ↓
Intune-Pilot-Users
        ↓
Windows 11 corporate endpoint
        ↓
Microsoft Entra Join
        ↓
Automatic Intune enrollment
        ↓
NOVATECH-W11-01 managed by Intune
```

---

## Root-cause analysis of 80180003

The troubleshooting sequence can be summarized as:

`User → License → Group → MDM scope → Enrollment restrictions → Device state → Windows logs → Intune enrollment report → Tenant MDM authority`

The decisive evidence came from the **Intune Enrollment Failures** report, which showed that the tenant had not been onboarded into Intune because the MDM authority was not configured.

The final remediation was:

`MDM Authority: None/Unknown → Intune MDM Authority`

After that change, the same Entra Join succeeded and the device automatically enrolled into Intune.

---

## Security and support principles practiced

### Least Privilege

Users receive only the access required for their role.

### Group-Based Access Management

Access is managed through reusable security groups where appropriate.

### Role-Based Access Control

Administrative capabilities are separated from business access.

### Separation of Duties

A dedicated administrative identity is used instead of mixing privileged and normal access unnecessarily.

### Multi-Factor Authentication

Privileged authentication is protected by an additional factor.

### Pilot Deployment

Automatic MDM enrollment is initially scoped to a dedicated pilot group rather than all users.

### Evidence-Based Troubleshooting

Settings, timestamps, reports and device state are checked before destructive changes are made.

### Safe Recovery

A known local administrator is retained while endpoint-management configuration is being tested.

---

## Skills practiced

- Microsoft Entra ID
- Microsoft Intune
- Identity and Access Management (IAM)
- User provisioning
- Security groups
- Group-based access
- Least Privilege
- Role-Based Access Control (RBAC)
- B2B identities
- Administrative account separation
- MFA
- Windows 11 administration
- UTM virtualization
- Microsoft Entra Join
- automatic MDM enrollment
- Intune enrollment restrictions
- MDM authority configuration
- Event Viewer
- `dsregcmd`
- PowerShell local-group validation
- endpoint inventory
- root-cause analysis
- error `80180003` troubleshooting

---

## Next lab

**IT-3 — Non-compliant Windows device with Microsoft Intune**

The managed endpoint created in IT-2 will be reused to introduce a Windows compliance policy, intentionally create a controlled non-compliant state, investigate the reason, remediate it, and validate the return to compliance.
