# IT-6 — Lost Corporate Device Incident

**Difficulty:** Beginner  
**Category:** Endpoint Security / Incident Response  
**Environment:** Microsoft Intune, Windows 11  
**Topic:** Lost Device Response and Remote Actions

---

## Scenario

You are an IT Support Technician at NovaTech.

Lina Martin reports that her corporate Windows laptop has been lost.

The affected device is:

`NOVATECH-W11-01`

The device is currently:

- managed by Microsoft Intune
- corporate-owned
- compliant
- assigned to Lina Martin
- actively enrolled in the NovaTech tenant

Your task is to assess the risk, review the available Intune remote actions, determine the correct response, and document the decision without unnecessarily destroying the lab endpoint.

---

## Business requirement

NovaTech must protect company data if a managed endpoint is lost or stolen.

The IT team should be able to:

- identify the affected device
- confirm ownership and management state
- assess exposure
- choose the appropriate remote action
- understand the impact of each action
- avoid accidental data destruction
- document the incident response decision

---

## Environment

| Component | Value |
|---|---|
| Device | `NOVATECH-W11-01` |
| Operating system | Windows 11 |
| Ownership | Corporate |
| MDM | Microsoft Intune |
| Primary user | Lina Martin |
| Compliance | Compliant |
| Device type | QEMU Virtual Machine |
| Incident | Lost corporate device |

---

## Your mission

1. Verify the device record in Microsoft Intune.
2. Confirm ownership, compliance and primary user.
3. Review the available remote data actions.
4. Compare Retire, Wipe and Delete.
5. Determine which action is appropriate for a lost corporate device.
6. Review the wipe confirmation options.
7. Document the chosen response.
8. Do not execute destructive actions on the lab VM.

---

## Initial device validation

Before taking any action, verify:

- device name
- ownership
- management authority
- compliance state
- operating system
- primary user
- last check-in

This establishes the known-good state before the incident response decision.

---

## Available actions

Intune provides several actions that may appear under device management menus.

Relevant data-removal actions include:

### Retire

Removes organizational data and management settings while attempting to preserve personal data.

Typical use case:

- BYOD
- employee offboarding
- removing corporate access without wiping the entire device

### Wipe

Resets the device and removes data and management configuration.

Typical use case:

- lost or stolen corporate-owned device
- device redeployment
- suspected compromise where local data should not remain accessible

### Delete

Removes the device record from Intune.

Delete should not be confused with a secure wipe.

Removing the management record alone is not sufficient protection for a lost device if local company data may still exist.

---

## Decision criteria

The following questions should be considered:

1. Is the device corporate-owned or personal?
2. Is the device expected to be recovered?
3. Is sensitive company data stored locally?
4. Is the user still authorized?
5. Is remote wipe appropriate?
6. Will removing the management record reduce or increase risk?
7. Does the device still check in with Intune?

---

## Selected response

For this scenario:

- the device is corporate-owned
- the device is reported lost
- local company data may be exposed
- the device should no longer be trusted

The preferred response is:

`Wipe the device and remove all data`

This is more appropriate than Retire because retaining local user data is not desirable on a lost corporate endpoint.

---

## Wipe options reviewed

The Intune wipe confirmation screen provided several choices.

### Reset device and remove all data

Performs a full reset and removes user and organizational data.

This is the selected response for the incident scenario.

### Reset device but keep user data

Preserves user files while removing management configuration.

This is not appropriate for a lost corporate device because local data remains available.

### Secure wipe

Uses a stronger data-erasure workflow intended for higher-risk scenarios.

This may be considered when data recovery must be strongly prevented.

---

## Lab safety decision

The wipe action was **not executed**.

`NOVATECH-W11-01` is an active lab endpoint used by previous exercises.

Destroying the VM would reduce the repeatability of the project.

The incident response decision was therefore documented and validated up to the final confirmation step without sending the destructive command.

---

## Investigation questions

Before opening the solution, answer:

1. What is the difference between Retire and Wipe?
2. Why is Delete not equivalent to securely erasing a device?
3. Why is keeping user data risky on a lost device?
4. When might Retire be preferable to Wipe?
5. What should be verified before issuing a destructive remote action?
6. Why should the device's last check-in be recorded?
7. What happens if a lost device is offline when a wipe is requested?
8. Why is documenting the decision important?

---

## Incident response workflow

```text
Lost device reported
        ↓
Identify endpoint
        ↓
Verify ownership and user
        ↓
Check management and compliance state
        ↓
Assess data exposure
        ↓
Review remote actions
        ↓
Select Wipe
        ↓
Review wipe options
        ↓
Approval / confirmation
        ↓
Remote action issued
        ↓
Monitor action status
        ↓
Document closure
```

In this lab, the workflow stops before issuing the destructive action.

---

## Validation checklist

Before closing the exercise, confirm:

- the correct device record was selected
- ownership is Corporate
- Intune management is active
- Lina Martin is the primary user
- the device was compliant before the incident
- Retire was reviewed
- Wipe was reviewed
- Delete was reviewed
- wipe options were understood
- full data removal was selected as the appropriate response
- the destructive action was not executed in the lab
- the incident response rationale was documented

---

## Security considerations

Remote device actions can be destructive and irreversible.

Before using them:

- verify device identity carefully
- verify ownership
- confirm the incident
- understand the impact
- obtain required approval
- preserve evidence where needed
- avoid deleting the Intune object before required remote actions complete
- monitor action status
- document the final outcome

---

## Success criteria

The exercise is complete when:

1. the lost device is correctly identified
2. the security risk is assessed
3. available Intune actions are compared
4. Wipe is selected as the appropriate response
5. wipe options are reviewed
6. no destructive command is executed on the lab VM
7. the decision is documented

---

> Do not open the solution until you have completed the exercise.

Solution: `../solutions/IT-6-solution.md`
