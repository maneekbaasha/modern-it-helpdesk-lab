# IT-6 — Solution: Lost Corporate Device Incident

## Objective

Assess the risk associated with a lost corporate Windows device, review Microsoft Intune remote actions, select the appropriate response, and document the incident without executing a destructive action on the lab endpoint.

This lab focuses on decision-making as much as technical execution.

---

## 1. Identify the affected endpoint

The reported device was:

`NOVATECH-W11-01`

The Intune device record showed:

- ownership: Corporate
- management authority: Intune
- compliance: Compliant
- operating system: Windows
- primary user: Lina Martin
- device model: QEMU Virtual Machine

**Meaning:** the device is organization-managed and is clearly associated with a known user.

**Problem solved:** confirms that the correct endpoint has been selected before any remote action is considered.

---

## 2. Record the pre-incident state

Before taking any action, the current device state was reviewed.

The device was healthy and compliant before being reported lost.

The last available check-in was also recorded.

**Meaning:** incident response should begin with evidence collection and state validation.

**Problem solved:** avoids making destructive decisions based on incomplete device information.

---

## 3. Assess the risk

The device is:

- corporate-owned
- reported lost
- potentially accessible by an unauthorized person
- likely to contain organizational data or cached credentials

This creates a confidentiality risk.

**Meaning:** the objective is no longer simply device management. It is protection of company data.

**Problem solved:** establishes the security justification for remote data removal.

---

## 4. Review available Intune actions

The Intune device menu exposed several actions.

Under data-removal actions, the relevant choices included:

```text
Retire
Wipe
Delete
```

Other actions such as Autopilot Reset were not appropriate for this incident.

**Meaning:** Intune provides multiple actions with different purposes and levels of impact.

**Problem solved:** prevents selecting an action based only on its name.

---

## 5. Understand Retire

The action:

`Retire`

removes organizational management and corporate data where supported while attempting to preserve personal user data.

Typical use cases include:

- BYOD
- employee departure
- removing company access from a personally owned device

For this scenario, Retire was not selected.

**Why:** `NOVATECH-W11-01` is a lost corporate device, and preserving local user data would not sufficiently reduce the exposure risk.

---

## 6. Understand Wipe

The action:

`Wipe`

resets the device and removes data and management configuration.

Typical use cases include:

- lost corporate devices
- stolen devices
- device decommissioning
- device redeployment
- suspected compromise

For the current incident, Wipe is the preferred response.

**Meaning:** the device should no longer retain local data if it is outside company control.

**Problem solved:** reduces the risk of unauthorized access to corporate or user data.

---

## 7. Understand Delete

The action:

`Delete`

removes the device object from Microsoft Intune.

This is an administrative record-management action and should not be treated as equivalent to secure data destruction.

Deleting the object too early can also remove management visibility before a remote action has completed.

**Meaning:** removing a management record is not the same as sanitizing the physical device.

**Problem solved:** avoids confusing inventory cleanup with incident containment.

---

## 8. Compare the three actions

| Action | Main purpose | Keeps user data | Removes management | Suitable for lost corporate device |
|---|---|---:|---:|---:|
| Retire | Remove corporate access/data | Usually yes | Yes | Usually no |
| Wipe | Reset and remove data | No | Yes | Yes |
| Delete | Remove Intune object | N/A | Removes record | No, not by itself |

---

## 9. Review wipe options

The Intune wipe confirmation page presented several options.

### Option 1 — Reset device and remove all data

This option removes:

- user data
- organizational data
- management configuration

This is the preferred option for the lost corporate device scenario.

---

### Option 2 — Reset device but keep user data

This option preserves local user files.

It was rejected.

**Reason:** preserving user data is not appropriate when the device is outside company control.

---

### Option 3 — Secure wipe

A stronger erasure option was also available.

This may be appropriate where preventing data recovery is especially important.

It is more destructive and was not required for this lab exercise.

---

## 10. Selected incident response

The selected operational response was:

```text
Wipe
→ Reset device and remove all data
```

**Meaning:** if this were a real lost corporate endpoint, the organization would request a full reset to protect locally stored information.

**Problem solved:** aligns the remote action with the device ownership and incident severity.

---

## 11. Why the wipe was not executed

The wipe command was deliberately not sent.

`NOVATECH-W11-01` is an active lab endpoint reused across multiple exercises.

Executing the wipe would:

- destroy the current Windows configuration
- remove the enrolled test device
- reduce repeatability of previous labs
- require rebuilding the VM

The exercise therefore stopped at the final confirmation stage.

**Meaning:** a lab can validate the decision and workflow without destroying reusable infrastructure.

**Problem solved:** preserves the test environment while still demonstrating the incident response process.

---

## 12. What would happen in a real incident

In a production environment, the workflow would continue:

```text
Incident confirmed
        ↓
Authorization obtained
        ↓
Wipe issued
        ↓
Command queued by Intune
        ↓
Device checks in
        ↓
Remote reset begins
        ↓
Action status monitored
        ↓
Device data removed
        ↓
Incident documented
        ↓
Device record reviewed / removed when appropriate
```

If the device were offline, the remote action would remain pending until the device could contact the management service again.

---

## 13. Why last check-in matters

The device's last check-in helps determine whether remote actions are likely to reach it.

A device that has not checked in recently may be:

- powered off
- disconnected from the Internet
- intentionally isolated
- no longer communicating with Intune

**Meaning:** a wipe request does not guarantee immediate erasure if the device cannot receive the command.

**Problem solved:** sets realistic expectations during incident response.

---

## 14. Incident response decision model

A useful decision flow is:

```text
Device reported lost
        ↓
Corporate or personal?
        ↓
Sensitive data exposure?
        ↓
Expected recovery?
        ↓
Still checking in?
        ↓
Select Retire / Wipe / other action
        ↓
Confirm impact
        ↓
Obtain approval
        ↓
Execute
        ↓
Monitor
        ↓
Document
```

---

## 15. Operational safeguards

Before issuing a destructive action, verify:

- correct device name
- correct user
- correct ownership
- incident legitimacy
- last check-in
- expected impact
- required approval
- whether evidence must be preserved first

**Meaning:** destructive remote actions should never be treated as routine clicks.

**Problem solved:** reduces the risk of wiping the wrong device.

---

## 16. Final lab workflow

```text
Lost device reported
        ↓
NOVATECH-W11-01 identified
        ↓
Corporate ownership confirmed
        ↓
Compliance and user verified
        ↓
Risk assessed
        ↓
Retire reviewed
        ↓
Wipe reviewed
        ↓
Delete reviewed
        ↓
Full wipe selected
        ↓
Final confirmation screen reviewed
        ↓
Destructive action intentionally cancelled
        ↓
Decision documented
```

---

## Troubleshooting and incident-response lessons

### Device management is not the same as incident response

The device may still be compliant at the time it is lost.

Compliance does not mean the endpoint is physically secure.

### Ownership changes the decision

Retire may make sense for BYOD.

Wipe is generally more appropriate for a lost corporate-owned endpoint.

### Delete is not a wipe

Removing an Intune record does not guarantee that local data has been erased.

### Offline devices require patience

Remote actions depend on the device being able to receive the management command.

### Evidence comes before destruction

Before wiping a device, determine whether logs, forensic evidence or other incident information must first be preserved.

---

## Skills practiced

- Microsoft Intune
- endpoint incident response
- remote device actions
- lost-device handling
- Retire vs Wipe vs Delete
- corporate ownership assessment
- risk-based decision making
- remote action impact analysis
- incident documentation
- destructive-action safety
- device check-in analysis
- endpoint lifecycle management

---

## Key takeaway

The correct response to a lost endpoint depends on context.

For this lab:

```text
Corporate-owned
+ Lost
+ Potential data exposure
= Full wipe preferred
```

The technical action is important, but the more important skill is choosing the correct action and understanding its consequences.

---

## Final validation status

The lost-device incident was fully assessed.

`NOVATECH-W11-01` was confirmed as:

- corporate-owned
- Intune-managed
- assigned to Lina Martin
- compliant before the incident

The available remote data-removal actions were reviewed.

The selected response was:

`Wipe → Reset device and remove all data`

The wipe command was intentionally not executed to preserve the reusable lab VM.

IT-6 is therefore validated as a simulated incident-response exercise.

---

## Project status

IT-6 completes the initial Modern IT Helpdesk Lab series.

The project now includes:

- IT-1 — Outlook authentication troubleshooting
- IT-2 — Entra ID employee onboarding
- IT-3 — Windows compliance monitoring
- IT-4 — Intune software deployment and troubleshooting
- IT-5 — macOS device management
- IT-6 — Lost corporate device incident response
