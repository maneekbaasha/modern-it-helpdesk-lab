# IT-4 — Software Deployment through Microsoft Intune

**Difficulty:** Beginner  
**Category:** Application Management / Troubleshooting  
**Environment:** Microsoft Intune, Windows 11  
**Topic:** Centralized Software Deployment

---

## Scenario

You are an IT Support Technician at NovaTech.

The IT department wants to deploy an approved Windows application to a managed workstation without requiring the user to manually download or install the software.

The target endpoint is:

`NOVATECH-W11-01`

The device is already:

- Microsoft Entra joined
- enrolled in Microsoft Intune
- managed as a corporate device
- member of the `Intune-Pilot-Devices` group
- equipped with Microsoft Intune Management Extension

Your task is to deploy the application centrally through Microsoft Intune, monitor the deployment, troubleshoot any installation failure, and validate the final result on the endpoint.

---

## Business requirement

NovaTech wants common applications to be centrally managed instead of relying on users to install software manually.

Centralized software deployment should provide:

- consistent software availability
- controlled deployment scope
- reduced user intervention
- centralized visibility
- easier troubleshooting
- standardized endpoint configuration
- safer software rollout

---

## Environment

| Component | Value |
|---|---|
| Device | `NOVATECH-W11-01` |
| Operating system | Windows 11 Pro |
| Architecture | ARM64 |
| Device type | Corporate |
| MDM | Microsoft Intune |
| Identity platform | Microsoft Entra ID |
| Primary user | Lina Martin |
| Pilot device group | `Intune-Pilot-Devices` |
| Application source | Microsoft Store (new) |
| Deployment type | Required |
| Installation context | System |

---

## Your mission

1. Add an approved Windows application through Microsoft Intune.
2. Review the application metadata.
3. Confirm the installation context.
4. Assign the application to a controlled pilot device group.
5. Configure the application as required.
6. Synchronize the managed endpoint.
7. Monitor deployment status.
8. Troubleshoot any deployment or applicability failure.
9. Validate successful installation on the endpoint.
10. Document the final result and lessons learned.

---

## Deployment model

The application should be deployed using:

```text
Microsoft Intune
        ↓
Microsoft Store application
        ↓
Required assignment
        ↓
Intune-Pilot-Devices
        ↓
NOVATECH-W11-01
```

The application should be installed automatically without requiring the end user to manually launch an installer.

---

## Assignment requirements

Use the following deployment model:

**Assignment type:**

`Required`

**Target group:**

`Intune-Pilot-Devices`

**Installation context:**

`System`

**Deployment timing:**

`As soon as possible`

---

## Expected workflow

```text
Application selected
        ↓
Application added to Intune
        ↓
Application metadata reviewed
        ↓
Pilot device group assigned
        ↓
Deployment marked as Required
        ↓
Managed endpoint synchronizes
        ↓
Application workload received
        ↓
Applicability evaluated
        ↓
Installation attempted
        ↓
Deployment status reported
        ↓
Endpoint validation
```

---

## Investigation questions

Before opening the solution, answer the following:

1. What is the difference between a Required application and an Available application?
2. Why should application deployment first target a pilot group?
3. What does the installation context `System` mean?
4. What role does Microsoft Intune Management Extension play in Win32 application deployment?
5. Why can an application assignment take time to reach an endpoint?
6. What should be checked if the application does not install?
7. How can you confirm that the device actually received the application workload?
8. What is the difference between:
   - targeting failure
   - applicability failure
   - detection failure
   - installation failure
9. Why should application compatibility and processor architecture be considered during troubleshooting?
10. Why should portal reporting be correlated with endpoint evidence?

---

## Validation checklist

Before closing the ticket, verify:

- the application exists in Microsoft Intune
- the expected publisher is selected
- the application type is correct
- installation context is set to System
- `Intune-Pilot-Devices` is assigned
- assignment type is Required
- `NOVATECH-W11-01` belongs to the target group
- the endpoint is enrolled in Intune
- Microsoft Intune Management Extension is present
- the endpoint successfully checks in
- the application workload reaches the endpoint
- application applicability is evaluated
- the application installs successfully
- the application is visible on Windows
- Intune reporting eventually reflects the deployment result

---

## Troubleshooting workflow

If the application does not install, investigate each stage separately.

```text
1. Application configuration
        ↓
2. Assignment
        ↓
3. Group membership
        ↓
4. Device enrollment
        ↓
5. Device check-in
        ↓
6. Intune Management Extension
        ↓
7. Application workload reception
        ↓
8. Applicability evaluation
        ↓
9. Detection
        ↓
10. Installation
        ↓
11. Reporting
```

Do not repeatedly change the deployment configuration before identifying which stage is failing.

---

## Useful Intune views

During troubleshooting, review:

- application properties
- assignments
- application overview
- device installation status
- managed device status
- device synchronization information

---

## Useful endpoint logs

Microsoft Intune Management Extension logs are stored in:

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs
```

Useful files include:

```text
IntuneManagementExtension.log
AppWorkload.log
```

These logs can help determine whether:

- the agent is communicating with Intune
- the application assignment was received
- the application is targeted
- applicability evaluation occurred
- detection failed
- installation failed
- authentication problems occurred

---

## Important troubleshooting principle

A failed application deployment does not automatically mean that Microsoft Intune is misconfigured.

The problem can exist at several different layers:

```text
Intune configuration
Device targeting
Device synchronization
Management agent
Application applicability
Architecture compatibility
Application detection
Application installation
Reporting
```

Each layer should be validated independently.

---

## Architecture considerations

The lab endpoint runs Windows 11 on ARM64.

When troubleshooting application deployment, consider whether the selected package supports:

- ARM64
- x64 emulation
- the Windows version used
- the installation context
- the virtualized environment

A package that works correctly on a standard x64 workstation may behave differently on an ARM64 virtual machine.

---

## Security considerations

Application deployments should follow controlled change-management practices.

Use:

- trusted application sources
- known publishers
- pilot groups
- limited deployment scope
- centralized management
- validation after installation
- evidence-based troubleshooting

Avoid deploying unverified applications to all corporate endpoints without testing.

---

## Ticket documentation challenge

Once the deployment is complete, document:

- initial application selected
- application source
- publisher
- assignment type
- installation context
- target group
- synchronization performed
- initial deployment result
- troubleshooting performed
- logs reviewed
- error stage identified
- compatibility considerations
- alternative application tested if required
- final endpoint result
- final Intune reporting state
- lessons learned

---

## Success criteria

The exercise is complete when:

1. an approved application is centrally deployed through Intune
2. `NOVATECH-W11-01` receives the application workload
3. any deployment failure is investigated using evidence
4. a working application is installed on the endpoint
5. the installation is validated locally
6. the deployment result is documented

---

> Do not open the solution until you have completed the exercise.

Solution: `../solutions/IT-4-solution.md`
