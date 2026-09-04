# IT-4 — Solution: Software Deployment through Microsoft Intune

## Objective

Deploy a Windows application centrally through Microsoft Intune to the managed endpoint `NOVATECH-W11-01`, troubleshoot an initial deployment failure, and validate a successful application installation on the endpoint.

This lab demonstrates both the normal Intune application deployment workflow and a real troubleshooting path when the first application does not install successfully.

---

## 1. Define the deployment objective

NovaTech wants to centrally deploy software to managed Windows devices.

The goal is to avoid requiring end users to manually download and install approved applications.

The deployment should provide:

- centralized software management
- controlled targeting
- automatic installation
- deployment visibility
- reduced user intervention
- easier troubleshooting

---

## 2. Select an application source

The Microsoft Store (new) integration in Intune was used.

Navigation:

`Applications → All applications → Add`

Application type:

`Microsoft Store app (new)`

**Meaning:** Intune can retrieve supported applications directly from the Microsoft Store catalog.

**Problem solved:** avoids manually downloading and packaging an installer for simple Store-supported applications.

---

## 3. Initial application choice: VLC

The first application selected was:

`VLC`

Publisher:

`VideoLAN`

Application type:

`Win32`

Package identifier:

`XPDM1ZW6815MQM`

**Meaning:** VLC was retrieved as a Win32 Microsoft Store application.

**Problem solved:** provided a realistic third-party application for testing centralized deployment.

---

## 4. Configure installation context

The installation behavior was configured as:

`System`

**Meaning:** the application is installed in the device context rather than only for a specific user profile.

**Problem solved:** makes the software available as a corporate-managed application at machine level.

---

## 5. Assign the application to a pilot device group

The deployment was configured as:

`Required`

Target group:

`Intune-Pilot-Devices`

Installation deadline:

`As soon as possible`

The group contains:

`NOVATECH-W11-01`

**Meaning:** Intune should automatically install the application on devices included in the pilot group.

**Problem solved:** limits the deployment scope and avoids tenant-wide impact during testing.

---

## 6. Synchronize the managed endpoint

The device was synchronized from Windows and from the Intune portal.

Navigation on Windows:

`Settings → Accounts → Access work or school → Connected account → Info → Sync`

**Meaning:** synchronization requests a new management check-in.

**Problem solved:** reduces the delay before the endpoint evaluates the new application assignment.

---

## 7. Initial deployment result

VLC did not appear on `NOVATECH-W11-01`.

The Intune application overview later reported:

`Failed: 1`

However, the detailed device installation report initially displayed no device row.

**Meaning:** aggregated Intune reporting may update before detailed reporting views.

**Problem solved:** demonstrates why troubleshooting should not rely on one portal view alone.

---

## 8. Verify application assignment

The VLC application properties were reviewed.

The assignment showed:

`Required → Intune-Pilot-Devices → Active`

**Meaning:** the deployment configuration was successfully saved and the target group was correct.

**Problem solved:** ruled out an incorrect or missing assignment.

---

## 9. Verify Intune Management Extension

On `NOVATECH-W11-01`, Microsoft Intune Management Extension was confirmed to be installed.

Log directory:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs
```

**Meaning:** the endpoint has the Intune agent responsible for processing Win32 application workloads.

**Problem solved:** ruled out a missing Intune Management Extension as the cause of the deployment failure.

---

## 10. Review Intune Management Extension logs

The following log was reviewed:

```text
IntuneManagementExtension.log
```

The log showed successful communication with Microsoft management services.

It also contained authentication-related entries such as:

```text
Failed to get AAD token
Need user interaction to continue
```

The endpoint was currently logged in using the local account:

`LabUser`

**Meaning:** the agent attempted to retrieve Entra authentication information while a local user session was active.

**Problem solved:** provided an initial hypothesis that user authentication context could be affecting processing.

---

## 11. Test with the Entra user session

The endpoint was signed in using the Entra user:

`Lina Martin`

A new Intune synchronization was triggered.

VLC still did not install.

**Meaning:** the local `LabUser` session was not the only cause of the deployment failure.

**Problem solved:** eliminated the user-session hypothesis as the primary root cause.

---

## 12. Inspect application workload logs

The following log was reviewed:

```text
AppWorkload.log
```

The log confirmed that the Intune Management Extension received the VLC application assignment.

Entries included:

```text
Name: VLC
Targeted: 1
PackageIdentifier: XPDM1ZW6815MQM
SourceName: msstore
```

**Meaning:** the application was successfully delivered to the endpoint's Intune application workload.

**Problem solved:** ruled out policy delivery and group targeting problems.

---

## 13. Identify the VLC evaluation error

Additional AppWorkload log entries showed:

```text
DetectionErrorOccurred = true
DetectionErrorCode = 2016215017

ApplicabilityErrorOccurred = true
ApplicabilityErrorCode = 2016215017
```

**Meaning:** the application failed during detection and applicability evaluation before a successful installation could occur.

**Problem solved:** narrowed the failure from a generic Intune deployment issue to an application-specific evaluation problem.

---

## 14. Consider endpoint architecture

The lab endpoint is a Windows 11 virtual machine running on ARM64 hardware.

Because the VLC Store package failed during applicability and detection evaluation, application compatibility became the leading hypothesis.

**Meaning:** not every Win32 package behaves identically across processor architectures and virtualized environments.

**Problem solved:** guided the troubleshooting toward testing a known ARM64-compatible application instead of repeatedly modifying the same failed deployment.

---

## 15. Replace VLC with Microsoft PowerToys

VLC was removed from the Intune application list.

A second application was selected:

`Microsoft PowerToys`

Publisher:

`Microsoft Corporation`

Application type:

`Win32`

PowerToys was deployed using the same model:

```text
Assignment: Required
Target group: Intune-Pilot-Devices
Installation context: System
```

**Meaning:** the same Intune deployment pipeline was reused with a different application.

**Problem solved:** provides a controlled comparison that helps determine whether the issue is application-specific or platform-wide.

---

## 16. Validate PowerToys on the endpoint

After synchronization, PowerToys appeared on:

`NOVATECH-W11-01`

**Meaning:** the application was successfully installed on the managed Windows endpoint.

**Problem solved:** proves that the Intune deployment pipeline itself is functional.

At the time of documentation, the Intune portal reporting had not yet updated to reflect the successful installation.

This reporting delay does not invalidate the local installation evidence.

---

## 17. Root-cause conclusion

The troubleshooting evidence shows:

- Intune assignment was correct
- the pilot device received the application workload
- Intune Management Extension was functioning
- VLC reached the endpoint
- VLC failed during detection/applicability evaluation
- changing the signed-in user did not resolve the issue
- PowerToys deployed successfully using the same Intune infrastructure

The most likely conclusion is:

**The VLC deployment failure was application-specific, likely related to applicability or compatibility on the ARM64 virtual machine, rather than a general Intune configuration failure.**

---

## Final workflow

```text
VLC selected
        ↓
Required assignment to Intune-Pilot-Devices
        ↓
NOVATECH-W11-01 synchronizes
        ↓
Application workload received
        ↓
VLC applicability / detection failure
        ↓
IME and AppWorkload logs reviewed
        ↓
User-session hypothesis tested
        ↓
Architecture / application compatibility considered
        ↓
PowerToys selected
        ↓
Same deployment model applied
        ↓
PowerToys installed successfully
        ↓
Intune reporting pending
```

---

## Troubleshooting lesson

A failed Intune application deployment does not automatically mean that Intune itself is broken.

A structured investigation should validate each layer:

```text
Assignment
→ Device targeting
→ Device check-in
→ Intune Management Extension
→ Application workload reception
→ Applicability
→ Detection
→ Installation
→ Reporting
```

In this lab, the failure was isolated to the application evaluation stage rather than the Intune management pipeline.

---

## Security and support principles practiced

### Pilot Deployment

Applications were assigned only to a controlled device group.

### Evidence-Based Troubleshooting

Portal status and endpoint logs were correlated before changing configuration.

### Layered Diagnosis

Targeting, authentication, agent health, workload delivery and application applicability were checked separately.

### Minimal Change

The deployment configuration was not repeatedly modified without evidence.

### Comparative Testing

A second application was used to determine whether the issue was application-specific.

### Validation

Successful installation on the endpoint was used as direct evidence while portal reporting was still pending.

---

## Skills practiced

- Microsoft Intune
- Microsoft Store application deployment
- Win32 application management
- device groups
- required application assignments
- Intune Management Extension
- AppWorkload.log analysis
- endpoint troubleshooting
- application applicability
- ARM64 compatibility awareness
- policy synchronization
- deployment validation
- root-cause analysis

---

## Key takeaway

The most important lesson from IT-4 is not simply how to deploy an application.

It is how to troubleshoot the deployment path when something fails:

```text
Target → Deliver → Evaluate → Install → Report
```

Each stage should be validated independently.

---

## Current validation status

PowerToys is installed on `NOVATECH-W11-01`.

Intune portal reporting is still pending at the time of documentation.

Once the portal reports:

`Installed`

the deployment can be considered fully validated from both the endpoint and management-console perspectives.

---

## Next lab

**IT-5 — macOS device management and Apple Business Manager concepts**
