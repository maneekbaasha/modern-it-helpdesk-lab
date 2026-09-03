# IT-3 — Solution: Non-Compliant Windows Device with Microsoft Intune

## Objective

Create a Windows compliance baseline in Microsoft Intune, detect a controlled firewall configuration drift on `NOVATECH-W11-01`, investigate the resulting non-compliance, remediate the endpoint, and validate the return to a compliant state.

This walkthrough explains **what was configured, what each step means, and which problem it solves**.

---

## 1. Define the compliance objective

NovaTech requires managed Windows devices to keep Microsoft Defender Firewall enabled.

The lab intentionally focuses on one simple control:

`Microsoft Defender Firewall → Required`

**Meaning:** compliance is evaluated against a defined security expectation.

**Problem solved:** creates a measurable security baseline instead of relying on assumptions about endpoint configuration.

---

## 2. Create the Windows compliance policy

In Microsoft Intune, a new Windows 10/11 compliance policy was created.

**Policy name:**

`NOVATECH-Windows-Compliance-Baseline`

**Platform:**

`Windows 10 and later`

**Profile type:**

`Windows compliance policy`

**Meaning:** Intune now has a policy that can evaluate whether Windows endpoints meet the selected security requirement.

**Problem solved:** establishes a centralized compliance rule for managed Windows devices.

---

## 3. Configure the firewall requirement

Under:

`System Security → Device Security`

the following setting was configured:

`Firewall → Require`

All unrelated settings were left unconfigured for this exercise.

**Meaning:** the policy only evaluates the control required for this lab.

**Problem solved:** keeps the scenario focused and avoids introducing unrelated compliance failures.

---

## 4. Configure the non-compliance action

The default action was retained:

`Mark device non-compliant → Immediately`

No email notification or retirement action was added.

**Meaning:** as soon as Intune evaluates the device as failing the firewall requirement, its compliance state can change to non-compliant.

**Problem solved:** provides immediate visibility of the security drift without triggering unnecessary actions in the lab.

---

## 5. Target a pilot population

The policy was first assigned using the existing pilot approach.

A dedicated device group was then used:

`Intune-Pilot-Devices`

The group contained:

`NOVATECH-W11-01`

**Meaning:** compliance testing is limited to a controlled endpoint population.

**Problem solved:** reduces the blast radius of policy mistakes and mirrors a realistic staged deployment process.

---

## 6. Why a device group was used

The first assignment approach used the user pilot group:

`Intune-Pilot-Users`

The compliance report initially showed no evaluated devices.

For this lab, the assignment was changed to the device group:

`Intune-Pilot-Devices`

containing `NOVATECH-W11-01`.

**Meaning:** user-based and device-based targeting are both valid concepts, but a device group is more direct when the exercise is centered on the compliance state of one specific endpoint.

**Problem solved:** simplifies targeting and reporting for the lab scenario.

---

## 7. Validate the baseline state

Before introducing the fault, `NOVATECH-W11-01` was synchronized with Intune.

The device was confirmed as:

`Compliant`

**Meaning:** the endpoint met the firewall requirement before the controlled change.

**Problem solved:** establishes a clean baseline so that the later non-compliance can be directly correlated with the intentional configuration drift.

---

## 8. Introduce a controlled configuration drift

On `NOVATECH-W11-01`, Windows Security was opened:

`Windows Security → Firewall & network protection`

The active network profile was:

`Public network`

Microsoft Defender Firewall was intentionally disabled for that active profile.

No unrelated security settings were changed.

**Meaning:** the endpoint was deliberately moved away from the defined security baseline.

**Problem solved:** creates a controlled non-compliance event that Intune can detect.

---

## 9. Synchronize the endpoint

After disabling the firewall, synchronization was triggered from Windows:

`Settings → Accounts → Access work or school → Connected account → Info → Sync`

Synchronization was also requested from the Intune device page.

**Meaning:** synchronization accelerates policy and state exchange between the managed endpoint and Intune.

**Problem solved:** reduces the delay between the local configuration change and Intune evaluation.

---

## 10. Detect the non-compliant state

After synchronization and evaluation, the Intune device overview for:

`NOVATECH-W11-01`

changed from:

`Compliant`

to:

`Non-compliant`

**Meaning:** Intune detected that the endpoint no longer satisfied the compliance baseline.

**Problem solved:** proves that the compliance policy is actively evaluating endpoint security posture.

---

## 11. Investigate the non-compliance

The detailed compliance report did not immediately display the expected setting-level data.

This highlighted an important operational point:

**Intune reporting can be delayed even when the device-level compliance state has already changed.**

Instead of waiting only on that report, another Intune security view was used.

**Meaning:** troubleshooting should use multiple evidence sources rather than relying on a single report.

**Problem solved:** avoids incorrectly assuming the detection failed just because one reporting view has not populated yet.

---

## 12. Use the firewall-specific Intune report

The following Intune report was opened:

`Windows MDM devices with firewall disabled`

`NOVATECH-W11-01` appeared in the report.

The firewall state was shown as:

`Limited`

This was consistent with the active public firewall profile being disabled while the other profiles remained enabled.

**Meaning:** Intune had visibility into the firewall configuration drift on the managed endpoint.

**Problem solved:** provides direct evidence that the non-compliant endpoint had an impaired firewall state.

---

## 13. Correlate the evidence

The incident now had multiple independent indicators:

- the endpoint was compliant before the change
- the active firewall profile was intentionally disabled
- Intune later marked the device non-compliant
- the firewall-specific report listed `NOVATECH-W11-01`
- the firewall state was reported as limited

**Meaning:** root-cause analysis should correlate endpoint changes with management-platform evidence.

**Problem solved:** prevents attributing non-compliance to an unrelated setting.

---

## 14. Remediate the device

On `NOVATECH-W11-01`, Microsoft Defender Firewall was re-enabled for the active public network profile.

**Meaning:** the local security configuration was restored to match the expected baseline.

**Problem solved:** removes the configuration drift that caused the compliance failure.

---

## 15. Synchronize after remediation

The endpoint was synchronized again from Windows and Microsoft Intune.

**Meaning:** the management service receives the corrected endpoint state.

**Problem solved:** accelerates compliance re-evaluation after remediation.

---

## 16. Validate the return to compliance

After remediation and synchronization, the device returned to a healthy compliance state.

The lab therefore demonstrated the full lifecycle:

`Compliant → Security drift → Non-compliant → Investigation → Remediation → Compliant`

**Meaning:** compliance is not only about detecting failures; it must also confirm successful remediation.

**Problem solved:** proves that the endpoint can return to the expected security posture after corrective action.

---

## Final workflow

```text
NOVATECH-W11-01
        ↓
Intune compliance policy assigned
        ↓
Firewall required
        ↓
Initial compliant state
        ↓
Public firewall profile disabled
        ↓
Windows / Intune synchronization
        ↓
Device becomes non-compliant
        ↓
Firewall-specific Intune report
        ↓
Configuration drift identified
        ↓
Firewall re-enabled
        ↓
Synchronization
        ↓
Compliance restored
```

---

## Troubleshooting lesson

The important lesson in this lab was not simply that disabling a firewall causes a security problem.

The key support workflow was:

`Establish baseline → Introduce controlled fault → Synchronize → Observe → Correlate evidence → Remediate → Validate`

When the detailed compliance report did not immediately populate, the investigation continued using the firewall-specific Intune report instead of assuming that the compliance policy was broken.

---

## Security and support principles practiced

### Compliance Baselines

Security expectations should be explicitly defined and measurable.

### Pilot Deployment

New policies should be tested on a limited group before broader deployment.

### Controlled Testing

Only the security setting required for the exercise was changed.

### Evidence-Based Troubleshooting

The device state, synchronization history, compliance state and Intune reporting were correlated.

### Minimal Remediation

The corrective action restored only the changed firewall configuration.

### Validation

The incident was not considered resolved until the device returned to compliance.

---

## Skills practiced

- Microsoft Intune
- Windows compliance policies
- Microsoft Defender Firewall
- endpoint compliance monitoring
- device groups
- policy assignment
- MDM synchronization
- security drift detection
- Intune reporting
- non-compliance investigation
- remediation
- validation
- root-cause analysis

---

## Key takeaway

A managed endpoint can be healthy at enrollment and later drift away from the organization's expected security posture.

Microsoft Intune compliance policies help detect that drift.

The operational workflow is:

`Define → Assign → Evaluate → Detect → Investigate → Remediate → Validate`

---

## Next lab

**IT-4 — Software deployment through Microsoft Intune**

The managed Windows endpoint will be reused to practice centralized application deployment and validation.
