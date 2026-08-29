# IT-1 — Solution: Outlook Authentication Loop

## Root cause

The issue appeared after the user changed their Microsoft 365 password.

The Microsoft account itself was working correctly:

- email worked on the user's phone
- Microsoft 365 authentication worked through the web browser
- the new password was accepted

This indicated that the problem was local to the Windows workstation and, more specifically, the Outlook authentication process.

---

## Investigation

### 1. Check another device

The user confirmed that email worked correctly on their phone.

**Conclusion:** the mailbox and Microsoft account were still accessible.

### 2. Check for recent changes

The user had changed their Microsoft password the previous day.

**Conclusion:** the authentication issue could be related to credentials stored on the workstation.

### 3. Validate the new password

The user successfully authenticated to Microsoft 365 through a web browser.

**Conclusion:** the new password was valid.

### 4. Restart Outlook

Outlook was completely closed and reopened.

The authentication loop continued.

**Conclusion:** a simple application restart did not resolve the incident.

---

## Hypothesis

Because:

- the account worked on another device
- web authentication worked
- the issue appeared after a password change
- only Outlook on the Windows workstation was affected

the most likely cause was stale Microsoft Office authentication information stored locally on Windows.

---

## Remediation

Open:

**Control Panel → User Accounts → Credential Manager → Windows Credentials**

Identify the relevant Microsoft Office credential.

In this lab, the affected entry was:

`MicrosoftOffice`

Remove only the credential associated with Microsoft Office.

Do not delete unrelated credentials.

Restart Outlook and authenticate again using the current Microsoft 365 credentials.

---

## Result

After authentication:

- Outlook opened normally
- email synchronization resumed
- new messages were received
- the authentication prompt no longer appeared

The incident was resolved.

---

## Why not reset the password again?

The password had already been validated successfully through Microsoft 365 on the web and another device.

Resetting it again would introduce another change without evidence that the account password was the problem.

---

## Why not clear the browser cache?

The browser authentication was already working.

Changing the configuration of a functioning component would not help isolate the Outlook-specific issue.

---

## Key lesson

Troubleshooting should reduce the scope of a problem before making changes.

In this incident:

**Microsoft 365 service?** Working  
**User account?** Working  
**Password?** Working  
**Mobile email?** Working  
**Web authentication?** Working  
**Outlook on this workstation?** Failing

This progressively isolated the problem to the local Outlook/Windows authentication environment.

---

## Example internal note

> Authentication issue affecting Outlook following a Microsoft 365 password change.
>
> Microsoft 365 web authentication and mobile email were functioning correctly, isolating the issue to the Outlook client on the Windows workstation.
>
> The Microsoft Office credential stored in Windows Credential Manager was removed. Outlook was restarted and the user authenticated again using the current credentials.
>
> Authentication succeeded, email synchronization resumed and the authentication loop disappeared.

---

## Example user response

> Hello Sofia,
>
> The Outlook authentication issue has been resolved. We confirmed that email synchronization is working correctly and the password prompt is no longer appearing.
>
> The ticket can now be closed.

---

## Skills practiced

- Jira Service Management
- Incident qualification
- Impact and urgency assessment
- User questioning
- Troubleshooting methodology
- Microsoft 365 authentication troubleshooting
- Windows Credential Manager
- Least-change troubleshooting
- Technical documentation
- User communication
