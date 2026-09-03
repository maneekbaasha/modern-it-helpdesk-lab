# IT-3 — Non-Compliant Windows Device with Microsoft Intune

**Difficulty:** Beginner  
**Category:** Security / Endpoint Compliance  
**Environment:** Microsoft Intune, Microsoft Entra ID, Windows 11  
**Topic:** Device Compliance and Firewall Remediation

---

## Scenario

You are an IT Support Technician at NovaTech.

The Windows workstation `NOVATECH-W11-01` is enrolled in Microsoft Intune and managed as a corporate device.

NovaTech wants to validate that Intune can detect when a managed Windows endpoint no longer meets a basic security requirement.

A compliance baseline is created requiring Microsoft Defender Firewall to be enabled.

During the exercise, the active Windows firewall profile is intentionally disabled.

Your task is to confirm that Intune detects the security drift, identify the cause of non-compliance, remediate the device, and validate that it returns to a compliant state.

---

## Environment

| Component | Value |
|---|---|
| Device | `NOVATECH-W11-01` |
| Operating system | Windows 11 Pro |
| Device type | Corporate managed device |
| MDM | Microsoft Intune |
| Identity platform | Microsoft Entra ID |
| Primary user | Lina Martin |
| Pilot device group | `Intune-Pilot-Devices` |
| Compliance policy | `NOVATECH-Windows-Compliance-Baseline` |

---

## Business requirement

NovaTech wants managed Windows devices to maintain an active host firewall.

The endpoint should therefore be considered non-compliant when the firewall requirement is not met.

The compliance rule used for this exercise is intentionally simple:

`Microsoft Defender Firewall → Required`

---

## Your mission

1. Create a Windows 10/11 compliance policy.
2. Require Microsoft Defender Firewall.
3. Apply the policy to a controlled pilot-device group.
4. Validate the initial compliant state.
5. Introduce a controlled configuration drift by disabling the active firewall profile.
6. Synchronize the endpoint with Intune.
7. Confirm that the device becomes non-compliant.
8. Investigate the reason using Intune reporting.
9. Re-enable the firewall.
10. Synchronize again and validate remediation.

---

## Investigation questions

Before opening the solution, answer the following:

1. Why should a compliance policy be tested on a pilot group before tenant-wide deployment?
2. What is the difference between configuring a security control and evaluating its compliance?
3. Why should the endpoint be confirmed as compliant before intentionally introducing the fault?
4. Which Intune views can help identify why a device is non-compliant?
5. Why is synchronization useful after changing a local security setting?
6. How do you prove that remediation was successful?

---

## Expected workflow

```text
Managed Windows endpoint
        ↓
Compliance baseline assigned
        ↓
Firewall requirement evaluated
        ↓
Initial compliant state
        ↓
Controlled firewall change
        ↓
Intune synchronization
        ↓
Non-compliant state detected
        ↓
Investigation and reporting
        ↓
Firewall remediated
        ↓
Synchronization
        ↓
Compliant state restored
```

---

## Security considerations

During the exercise:

- change only the setting required for the scenario
- keep the test limited to a controlled lab endpoint
- do not disable unrelated endpoint protections
- retain a known administrative recovery account
- validate the original state before and after remediation
- do not treat delayed reporting as proof that the configuration failed

---

## Ticket documentation challenge

Once the incident is resolved, document:

- policy name
- compliance requirement
- target group
- initial device state
- configuration change that caused non-compliance
- evidence used during investigation
- remediation performed
- final compliance state
- lessons learned

---

> Do not open the solution until you have completed the exercise.

Solution: `../solutions/IT-3-solution.md`
