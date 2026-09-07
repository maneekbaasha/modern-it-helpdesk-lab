# IT-5 — macOS Device Management with Microsoft Intune

**Difficulty:** Beginner  
**Category:** Device Management / Apple  
**Environment:** Microsoft Intune, macOS, Apple Push Notification service  
**Topic:** macOS Enrollment, MDM, Configuration Profiles and Apple Business Manager Concepts

---

## Scenario

You are an IT Support Technician at NovaTech.

The IT department wants to manage a corporate Mac using Microsoft Intune.

A new lab endpoint has been created:

`NOVATECH-MAC-01`

The goal is to:

- enroll the Mac into Microsoft Intune
- validate MDM communication
- apply a basic macOS security configuration
- verify successful policy deployment
- understand how Apple Business Manager would automate this process in a real company

Because this is a lab environment without Apple Business Manager ownership of the device, the Mac will first be enrolled manually through Microsoft Company Portal.

---

## Environment

| Component | Value |
|---|---|
| Device | `NOVATECH-MAC-01` |
| Operating system | macOS Sequoia 15.6.1 |
| Device type | Virtual machine |
| Virtualization | UTM |
| MDM | Microsoft Intune |
| Identity platform | Microsoft Entra ID |
| Primary user | Lina Martin |
| Enrollment method | Company Portal |
| Apple MDM Push certificate | Configured |
| Pilot group | `Intune-Pilot-macOS-Devices` |
| Configuration policy | `NOVATECH-macOS-Security-Baseline` |

---

## Business requirement

NovaTech wants company Macs to be centrally managed.

Managed macOS devices should support:

- centralized enrollment
- security configuration
- policy deployment
- device inventory
- compliance visibility
- remote administration
- consistent security baselines

---

## Your mission

1. Prepare a dedicated macOS lab endpoint.
2. Install Microsoft Company Portal.
3. Configure Apple MDM Push connectivity.
4. Enroll the Mac into Microsoft Intune.
5. Verify that the device appears in the Intune device inventory.
6. Confirm the device is communicating successfully with Intune.
7. Create a dedicated macOS pilot device group.
8. Create a macOS configuration profile.
9. Configure a simple session-lock security baseline.
10. Assign the policy to the pilot group.
11. Synchronize the endpoint.
12. Validate that the configuration profile is successfully applied.
13. Understand how Apple Business Manager and Automated Device Enrollment would change this workflow.

---

## Enrollment architecture

The manual enrollment workflow used in this lab is:

```text
macOS device
      ↓
Microsoft Company Portal
      ↓
Microsoft Entra authentication
      ↓
Intune enrollment
      ↓
Apple Push Notification service
      ↓
MDM profile installation
      ↓
Managed macOS endpoint
```

---

## Apple Push Notification service

Apple devices require Apple Push Notification service connectivity for MDM communication.

The Intune tenant must therefore have a valid:

`Apple MDM Push certificate`

The certificate connects:

```text
Microsoft Intune
      ↕
Apple Push Notification service
      ↕
Managed Apple device
```

Without this certificate, Apple devices cannot be properly managed through Intune.

---

## Initial enrollment issue

During the first Company Portal enrollment attempt, the device returned:

```text
AccountNotOnboarded
```

The enrollment could not continue.

The investigation identified that the Intune tenant did not yet have an Apple MDM Push certificate configured.

After the certificate was created and uploaded to Intune, enrollment was retried.

---

## Expected enrollment result

After successful enrollment:

- `NOVATECH-MAC-01` should appear in Intune
- management authority should show Intune
- the device should report macOS as its operating system
- the primary user should be Lina Martin
- the device should successfully check in
- the Mac should display an installed Management Profile

---

## Ownership consideration

Because the Mac is manually enrolled using Company Portal, Intune may classify the device as:

`Personal`

This is different from an automatically provisioned corporate device.

In a production environment using Apple Business Manager and Automated Device Enrollment, the device would normally be treated as a corporate-owned device.

---

## Security baseline objective

A simple configuration policy will be used to demonstrate actual MDM configuration.

Policy name:

`NOVATECH-macOS-Security-Baseline`

The profile should configure screen-lock behavior.

---

## Configuration settings

The following settings are used:

```text
Require password = True
Password delay = 5 seconds
Login window inactivity duration = 300 seconds
Module name = com.apple.screensaver
```

This creates a simple baseline requiring authentication after inactivity.

---

## Assignment model

The policy should not be assigned to every macOS device immediately.

A dedicated pilot group is used:

`Intune-Pilot-macOS-Devices`

The group contains:

`NOVATECH-MAC-01`

The deployment model is:

```text
Configuration policy
        ↓
Pilot device group
        ↓
NOVATECH-MAC-01
```

---

## Investigation questions

Before opening the solution, answer the following:

1. Why do Apple devices require an Apple MDM Push certificate?
2. What role does Microsoft Company Portal play during manual macOS enrollment?
3. What is installed on macOS when the device becomes managed?
4. Why might a manually enrolled Mac appear as Personal in Intune?
5. Why should a configuration profile first target a pilot group?
6. How can you verify that a macOS configuration profile was successfully applied?
7. What is the difference between device enrollment and device configuration?
8. What role would Apple Business Manager play in a production environment?
9. What is Automated Device Enrollment?
10. Why is automated enrollment preferable for company-owned Macs?

---

## Validation checklist

Before closing the ticket, verify:

- the Mac is running successfully
- Company Portal is installed
- Apple MDM Push certificate is configured
- the Management Profile is installed
- the Mac appears in Intune
- `NOVATECH-MAC-01` reports successfully
- the operating system is detected as macOS
- Lina Martin is associated with the endpoint
- the macOS pilot device group exists
- the Mac belongs to the pilot group
- the security baseline exists
- the policy is assigned to the pilot group
- the policy reports success
- there are no policy conflicts
- the lock-screen behavior works as expected

---

## Troubleshooting workflow

If macOS enrollment fails:

```text
Company Portal
      ↓
User authentication
      ↓
Intune licensing
      ↓
Apple MDM Push certificate
      ↓
Enrollment restrictions
      ↓
Management Profile
      ↓
Device check-in
```

If a configuration policy fails:

```text
Policy configuration
      ↓
Assignment
      ↓
Group membership
      ↓
Device synchronization
      ↓
Applicability
      ↓
Conflict
      ↓
Policy status
```

---

## Useful Intune views

Review:

- Devices → All devices
- Devices → macOS
- Devices → Enrollment
- Apple enrollment
- Apple MDM Push certificate
- Configuration profiles
- Device configuration status
- Per-setting status

---

## Apple Business Manager concept

Apple Business Manager is not required for the manual enrollment used in this lab.

However, in a real organization, Apple Business Manager allows company-owned Apple devices to be linked to an MDM platform such as Microsoft Intune.

A typical production workflow is:

```text
Company purchases Mac
        ↓
Device appears in Apple Business Manager
        ↓
Device assigned to Microsoft Intune
        ↓
Automated Device Enrollment profile assigned
        ↓
Employee turns on the Mac
        ↓
Setup Assistant detects company ownership
        ↓
MDM enrollment occurs automatically
        ↓
Corporate configuration is applied
```

---

## Manual enrollment vs Automated Device Enrollment

### Manual enrollment

```text
User installs Company Portal
→ User signs in
→ User installs Management Profile
→ Device becomes managed
```

Characteristics:

- user interaction required
- easier for labs and BYOD
- device may appear as personal
- MDM profile may be removable

### Automated Device Enrollment

```text
Device assigned through Apple Business Manager
→ User starts Mac
→ Setup Assistant contacts Apple
→ MDM enrollment automatically begins
→ Corporate policies are applied
```

Characteristics:

- designed for corporate-owned devices
- reduced user interaction
- stronger ownership assurance
- scalable for fleet deployment
- supports supervised management

---

## Security considerations

For production Apple device management:

- use a dedicated Apple administrative account for APNs
- protect the Apple account with MFA
- renew the APNs certificate before expiration
- renew using the same Apple account
- use Apple Business Manager for corporate device ownership
- use pilot groups before broad configuration deployment
- avoid using personal Apple IDs for organizational MDM administration
- document certificate ownership and renewal procedures

---

## Ticket documentation challenge

Once the lab is complete, document:

- macOS VM creation
- device name
- macOS version
- Company Portal installation
- initial enrollment failure
- APNs certificate configuration
- successful Management Profile installation
- device inventory result
- device ownership classification
- macOS pilot group
- configuration policy
- configured security settings
- assignment
- synchronization
- policy deployment result
- lessons learned
- Apple Business Manager / ADE comparison

---

## Success criteria

The exercise is complete when:

1. `NOVATECH-MAC-01` is enrolled into Intune
2. the Management Profile is installed
3. Intune successfully inventories the Mac
4. the device is successfully checking in
5. the security baseline is applied
6. the configuration report shows success
7. the configured behavior is validated
8. the difference between manual enrollment and Automated Device Enrollment is understood

---

> Do not open the solution until you have completed the exercise.

Solution: `../solutions/IT-5-solution.md`
