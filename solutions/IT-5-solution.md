# IT-5 — Solution: macOS Device Management with Microsoft Intune

## Objective

Enroll a macOS endpoint into Microsoft Intune, configure the Apple Push Notification service requirement, deploy a macOS security configuration profile, validate successful policy application, and understand how Apple Business Manager and Automated Device Enrollment would improve the process in production.

This lab demonstrates both the practical enrollment workflow and the management concepts behind enterprise Apple device administration.

---

## 1. Prepare the macOS lab endpoint

A dedicated macOS virtual machine was created in UTM.

**Device name:**

`NOVATECH-MAC-01`

**Operating system:**

`macOS Sequoia 15.6.1`

**Local administrator account:**

`novatechadmin`

A personal Apple Account was not added to the VM.

**Meaning:** the lab uses a dedicated environment that is isolated from the host Mac and personal Apple data.

**Problem solved:** avoids enrolling a personal production device into the Intune test tenant.

---

## 2. Install Microsoft Company Portal

Microsoft Company Portal was downloaded and installed on the Mac.

The user authenticated using the NovaTech Entra identity:

`Lina Martin`

**Meaning:** Company Portal provides the manual user-driven enrollment workflow for the macOS endpoint.

**Problem solved:** allows the user identity and device to be associated with the Intune tenant.

---

## 3. Start macOS enrollment

Company Portal initiated the device setup workflow.

The enrollment process included:

1. privacy information
2. management profile installation
3. device settings validation

During the first attempt, enrollment failed.

The error displayed was:

```text
AccountNotOnboarded
```

**Meaning:** the tenant was not fully prepared to manage Apple devices.

**Problem solved:** the error provided the first troubleshooting indicator for the failed enrollment.

---

## 4. Investigate the enrollment failure

The Intune Apple enrollment configuration was reviewed.

The tenant did not yet have an Apple MDM Push certificate configured.

Microsoft Intune requires Apple Push Notification service connectivity to manage Apple devices.

The missing configuration was found under:

```text
Devices
→ Enrollment
→ Apple
→ Apple MDM Push certificate
```

**Meaning:** Apple MDM communication depends on the Apple Push Notification service.

**Problem solved:** identified the tenant-side prerequisite preventing macOS enrollment.

---

## 5. Generate the Intune CSR

The Microsoft authorization requirement was accepted.

An Intune Certificate Signing Request was downloaded.

This CSR is used to request an Apple Push Notification service certificate from Apple.

**Meaning:** the CSR links the Microsoft Intune tenant to the Apple MDM certificate creation process.

**Problem solved:** provides the trusted certificate request required by Apple.

---

## 6. Create the Apple MDM Push certificate

An Apple administrative account dedicated to MDM administration was used.

The Intune CSR was uploaded to Apple's Push Certificates portal.

Apple generated the MDM Push certificate.

The certificate was downloaded and then uploaded back into Intune.

The same Apple Account used to create the certificate was recorded for future renewal.

**Meaning:** Intune can now send MDM notifications to enrolled Apple devices through Apple Push Notification service.

**Problem solved:** enables Apple device enrollment and ongoing MDM communication.

---

## 7. Retry Company Portal enrollment

Company Portal was restarted after the Apple MDM Push certificate was configured.

Enrollment was initiated again.

This time, Company Portal successfully progressed to the management profile installation stage.

**Meaning:** the APNs prerequisite was now satisfied.

**Problem solved:** confirms that the missing push certificate was the cause of the original `AccountNotOnboarded` failure.

---

## 8. Install the macOS Management Profile

macOS opened:

```text
System Settings
→ General
→ Device Management
```

The downloaded profile appeared as:

`Management Profile`

The profile was manually installed and approved using the local administrator credentials.

After installation, macOS displayed:

```text
This Mac is supervised and managed by:
Répertoire par défaut
```

A management profile with multiple managed settings was visible.

**Meaning:** the Mac is now enrolled into MDM and accepts management commands from Intune.

**Problem solved:** establishes the trust relationship between the Mac and Microsoft Intune.

---

## 9. Validate device inventory in Intune

The Intune device inventory was reviewed.

`NOVATECH-MAC-01` appeared under:

```text
Devices
→ All devices
```

The device reported:

| Property | Result |
|---|---|
| Device name | `NOVATECH-MAC-01` |
| Managed by | Intune |
| Operating system | macOS |
| OS version | 15.6.1 |
| Primary user | Lina Martin |
| Compliance | Compliant |
| Ownership | Personal |

**Meaning:** the endpoint successfully enrolled and started reporting inventory information.

**Problem solved:** proves that the device is communicating with Intune.

---

## 10. Understand device ownership

The device appeared in Intune as:

`Personal`

This occurred because the Mac was manually enrolled using Company Portal rather than automatically provisioned as a corporate device through Apple Business Manager.

**Meaning:** enrollment method affects how ownership is represented and how strongly the organization controls the device.

**Problem solved:** demonstrates an important distinction between BYOD/manual enrollment and enterprise corporate enrollment.

---

## 11. Create a macOS pilot group

A dedicated device group was created:

`Intune-Pilot-macOS-Devices`

The group contained:

`NOVATECH-MAC-01`

**Meaning:** configuration testing is limited to a controlled macOS population.

**Problem solved:** reduces the risk of applying untested settings to every Mac in the tenant.

---

## 12. Create the macOS security baseline

A new macOS configuration profile was created.

**Policy name:**

`NOVATECH-macOS-Security-Baseline`

**Platform:**

`macOS`

**Profile type:**

`Settings catalog`

**Description:**

`Baseline security configuration for NovaTech managed macOS devices.`

**Meaning:** Intune can centrally configure operating system settings on enrolled Macs.

**Problem solved:** moves beyond simple device enrollment into active endpoint management.

---

## 13. Configure screen-lock security settings

The following Screensaver configuration settings were selected:

```text
Require password = True
Password delay = 5 seconds
Login window inactivity duration = 300 seconds
Module name = com.apple.screensaver
```

**Meaning:** authentication is required after inactivity, reducing the risk of unattended access.

**Problem solved:** provides a simple and measurable security control for the lab.

---

## 14. Assign the security baseline

The configuration profile was assigned to:

`Intune-Pilot-macOS-Devices`

No broad tenant-wide assignment was used.

**Meaning:** the baseline is deployed only to the pilot Mac.

**Problem solved:** follows staged deployment and change-control principles.

---

## 15. Synchronize the Mac

Company Portal was used to trigger a device check-in.

The configuration profile was then evaluated by the Mac.

**Meaning:** synchronization accelerates policy retrieval and status reporting.

**Problem solved:** reduces the delay between policy creation and endpoint validation.

---

## 16. Validate configuration deployment

The Intune configuration profile status was reviewed.

For:

`NOVATECH-macOS-Security-Baseline`

the result showed:

```text
Success: 1
Error: 0
Conflict: 0
Not applicable: 0
Pending: 0
```

**Meaning:** the policy was successfully processed by the target Mac.

**Problem solved:** proves that the MDM channel can deliver and enforce macOS configuration settings.

---

## 17. Validate endpoint behavior

The Mac was left inactive for the configured period.

The screen-lock behavior was then tested.

The endpoint required authentication after inactivity according to the applied policy.

**Meaning:** the policy was not only reported as successful by Intune; the expected endpoint behavior was also validated.

**Problem solved:** confirms that configuration reporting matches actual device behavior.

---

## 18. Final practical workflow

```text
macOS VM created
        ↓
Company Portal installed
        ↓
Enrollment attempted
        ↓
AccountNotOnboarded
        ↓
Apple MDM Push certificate missing
        ↓
CSR generated
        ↓
APNs certificate created
        ↓
Certificate uploaded to Intune
        ↓
Enrollment retried
        ↓
Management Profile installed
        ↓
Mac appears in Intune
        ↓
Pilot group created
        ↓
macOS baseline assigned
        ↓
Device synchronized
        ↓
Policy Success: 1
        ↓
Security behavior validated
```

---

# Apple Business Manager and Automated Device Enrollment

The practical lab used manual Company Portal enrollment.

A real enterprise environment would usually use Apple Business Manager for company-owned Macs.

---

## 19. What is Apple Business Manager?

Apple Business Manager is Apple's enterprise service for managing organizational ownership of Apple devices and connecting those devices to an MDM platform.

It can integrate with Microsoft Intune.

The general relationship is:

```text
Apple Business Manager
        ↓
Company-owned Apple devices
        ↓
Microsoft Intune
        ↓
MDM configuration and security policies
```

**Meaning:** Apple Business Manager establishes organizational ownership before the employee starts using the device.

---

## 20. Automated Device Enrollment

Automated Device Enrollment allows the enrollment process to begin during macOS Setup Assistant.

A typical company workflow is:

```text
Company purchases Mac
        ↓
Mac appears in Apple Business Manager
        ↓
Device assigned to Microsoft Intune
        ↓
ADE enrollment profile assigned
        ↓
Employee turns on Mac
        ↓
Setup Assistant contacts Apple
        ↓
Device detects organization ownership
        ↓
MDM enrollment begins automatically
        ↓
Corporate configuration is applied
```

This removes much of the manual Company Portal workflow.

---

## 21. Manual enrollment vs ADE

### Manual Company Portal enrollment

```text
User receives Mac
        ↓
User installs Company Portal
        ↓
User signs in
        ↓
User downloads Management Profile
        ↓
User approves profile
        ↓
Mac becomes managed
```

Advantages:

- easy to test
- useful for BYOD
- does not require Apple Business Manager
- suitable for lab environments

Limitations:

- user interaction required
- easier for the user to remove management
- device may appear as Personal
- less suitable for large corporate fleets

---

### Automated Device Enrollment

```text
Corporate Mac
        ↓
Apple Business Manager
        ↓
Microsoft Intune
        ↓
Setup Assistant
        ↓
Automatic enrollment
        ↓
Corporate policies
```

Advantages:

- optimized for corporate-owned devices
- minimal user interaction
- stronger ownership assurance
- scalable
- consistent deployment
- management begins during initial setup

---

## 22. Why Apple Business Manager was simulated

The lab Mac is a virtual machine and is not a real corporate device purchased and assigned through Apple Business Manager.

Therefore:

- real ABM device ownership was not available
- ADE could not be fully implemented
- Company Portal enrollment was used instead

The architecture and workflow were documented conceptually so the difference between a lab enrollment and a production enterprise deployment is understood.

---

## Troubleshooting lessons

Several useful troubleshooting principles were practiced.

### Verify platform prerequisites first

The original enrollment failure was caused by a missing Apple MDM Push certificate.

Before investigating the endpoint deeply, confirm that the tenant is ready to manage the platform.

### Separate enrollment from configuration

Successful device enrollment does not automatically mean configuration policies are working.

Validate:

```text
Enrollment
→ Device inventory
→ MDM communication
→ Policy assignment
→ Policy status
→ Endpoint behavior
```

### Use pilot groups

Configuration profiles should be tested on a limited device set before broad deployment.

### Validate both portal and endpoint

A successful Intune report should be correlated with actual device behavior.

---

## Security and operational considerations

For production Apple management:

- use a dedicated organizational Apple Account for the APNs certificate
- enable MFA on the Apple administrative account
- record which account owns the APNs certificate
- renew the APNs certificate before expiration
- always renew the existing certificate instead of replacing it unnecessarily
- use Apple Business Manager for corporate devices
- use Automated Device Enrollment where possible
- deploy policies through pilot groups first
- document certificate lifecycle and renewal responsibilities
- avoid linking corporate MDM infrastructure to personal Apple Accounts

---

## Skills practiced

- macOS virtualization
- Microsoft Intune
- Microsoft Company Portal
- Apple MDM
- Apple Push Notification service
- APNs certificates
- macOS device enrollment
- Management Profiles
- Intune device inventory
- device ownership concepts
- device groups
- macOS Settings Catalog
- configuration profiles
- policy assignments
- MDM synchronization
- configuration reporting
- endpoint validation
- Apple Business Manager concepts
- Automated Device Enrollment concepts
- troubleshooting

---

## Key takeaway

Managing a Mac through Intune requires more than installing Company Portal.

The complete management chain is:

```text
Tenant readiness
→ Apple Push connectivity
→ Enrollment
→ Management Profile
→ Device inventory
→ Configuration assignment
→ Policy application
→ Validation
```

For a small lab or BYOD scenario, Company Portal is sufficient.

For corporate fleets, the preferred enterprise model is:

```text
Apple Business Manager
→ Automated Device Enrollment
→ Microsoft Intune
→ Zero-touch or low-touch deployment
```

---

## Final validation status

`NOVATECH-MAC-01` is successfully enrolled into Microsoft Intune.

The device is:

- visible in Intune
- compliant
- managed through an installed MDM profile
- assigned to a macOS pilot group
- receiving the NovaTech macOS security baseline

The configuration profile reports:

```text
Success: 1
Error: 0
Conflict: 0
Not applicable: 0
Pending: 0
```

The configured lock-screen behavior was also validated locally.

IT-5 is therefore fully validated.

---

## Next lab

**IT-6 — Lost Corporate Device Incident**

The next exercise will focus on device security operations and incident response actions through Microsoft Intune.
