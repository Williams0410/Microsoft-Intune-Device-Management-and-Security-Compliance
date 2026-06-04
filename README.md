# Microsoft-Intune-Device-Management-and-Security-Compliance


# Windows Device Configuration Profile

## Project Summary  Overview
In this project i configured micorosoft Intune for centralized device managem,ent and enforce basic security requirements for windows devices before production deployment. 
The goal is to demonsatrate hands-on experience on:

* Activate Microsoft Intune in the tenant.
* Configure device enrollment.
* Create compliance policies requiring:
    * Device PIN
    * Device encryption (BitLocker)
* Create a device configuration profile.
* Test deployment using two Windows 11 virtual machines.
* Verify successful enrollment and compliance reporting.


## Scenario
We have procured 10 Windows devices and need to make them security compliant. 
Your task now is to set up Microsoft Intune in our Tenant to manage devices. 
Create a basic policy that says "every device needs a PIN and encryption."
Enroll at least 2 Windows test machines in VirtualBox and try enrolling them in your sandbox environment so you can prove the system works before deploying on the Tenant.
* Objective 1: Activate Intune
* Objective 2: Allow Devices to Enroll
* Objective 3: Create a Basic Compliance Policy
* Objective 4: Create a Basic Device Configuration Profile
* Objective 5: Build the Policy Roadmap
* Objective 6: Create Windows 11 VMs in VirtualBox
* Objective 7: Join VMs to Azure AD (and Auto-Enroll in Intune) 
* Objective 8: Verify Devices in Endpoint Manager 
* Objective 9: Verify Compliance Status of Devices


##  Prerequisites

* A Microsoft 365 tenant with Intune licenses (e.g., Microsoft 365 Business Premium, E3, E5, or Intune Plan 1).
* Global Administrator or Intune Administrator rights.
* Windows 10/11 Pro, Enterprise, or Education devices.
* Access to the Microsoft Intune Admin Center.





## Licensing

Ensure users are assigned one of the following licenses:

* Microsoft 365 Business Premium
* Microsoft 365 E3/E5
* Microsoft Intune Plan 1 or higher


# Step 1: Enable Intune Enrollment

## Configure MDM Enrollment

Navigate to:

```text
Microsoft Entra Admin Center
→ Mobility (MDM and MAM)
→ Microsoft Intune
```

Configure:

| Setting        | Value |
| -------------- | ----- |
| MDM User Scope | All   |
| MAM User Scope | None  |

Save the configuration.

This allows users to automatically enroll devices into Intune when joining Microsoft Entra ID.

---

# Step 2: Create Device Security Group

Navigate to:

```text
Microsoft Entra Admin Center
→ Groups
→ New Group
```

Create:

```text
Group Type: Security
Group Name: Windows-Managed-Devices
Membership Type: Assigned
```

Add all company-managed Windows devices to this group.

---

# Step 3: Create Windows Compliance Policy

Navigate to:

```text
Intune Admin Center
→ Devices
→ Compliance Policies
→ Create Policy
```
## Platform

```text
Windows 10 and later
```
## Policy Name

```text
Windows Compliance Baseline
```
## Configuration
### Device Health
```text
Require BitLocker = Yes
```
### Microsoft Defender (Recommended)
```text
Require Antivirus = Yes
Require Real-time Protection = Yes
```
### System Security (Recommended)

```text
Require Secure Boot = Yes
```
Assign policy to:
```text
Windows-Managed-Devices
```
---
# Step 4: Create Windows Hello for Business PIN Policy

Navigate to:
```text
Intune Admin Center
→ Devices
→ Configuration Profiles
→ Create Profile
```
## Platform

```text
Windows 10 and later
```
## Profile Type
```text
Settings Catalog
```
## Policy Name
```text
Windows PIN Policy
```
Search for:
```text
Windows Hello for Business
```
Configure:
| Setting                        | Value    |
| ------------------------------ | -------- |
| Use Windows Hello for Business | Enabled  |
| Minimum PIN Length             | 6        |
| Maximum PIN Length             | 127      |
| Digits Required                | Yes      |
| Lowercase Letters              | Optional |
| Uppercase Letters              | Optional |
| Special Characters             | Optional |
Assign policy to:

```text
Windows-Managed-Devices
```
---
# Step 5: Create BitLocker Encryption Policy
Navigate to:

```text
Intune Admin Center
→ Devices
→ Configuration Profiles
→ Create Profile
```
## Platform
```text
Windows 10 and later
```
## Profile Type
```text
Endpoint Protection
```
## Policy Name
```text
BitLocker Encryption Policy
```
## Configuration
### Operating System Drive
```text
Enable BitLocker = Yes
```
### Startup Authentication
```text
TPM Required = Yes
```
### Recovery Options
```text
Store Recovery Information in Microsoft Entra ID = Yes
Recovery Password = Enabled
```
Assign policy to:
```text
Windows-Managed-Devices
```
---
# Step 6: Configure Conditional Access
## Purpose
Ensure only compliant devices can access corporate resources.
Navigate to:
```text
Microsoft Entra Admin Center
→ Protection
→ Conditional Access
→ New Policy
```
### Policy Name
```text
Require Compliant Device
```
### Assignments
```text
Users: All Users
Target Resources: Microsoft 365
```
### Grant Controls
```text
Require Device To Be Marked As Compliant
```
Enable the policy.
---
# Step 7: Enroll Windows Devices
On each Windows device:
```text
Settings
→ Accounts
→ Access Work or School
→ Connect
```
Choose:
```text
Join this device to Microsoft Entra ID
```
Authenticate using a company account.
The device will:
* Join Microsoft Entra ID
* Enroll in Microsoft Intune
* Receive assigned security policies
---
# Step 8: Validate Compliance
Navigate to:
```text
Intune Admin Center
→ Devices
→ All Devices
```
Verify the following:
| Check             | Expected Result |
| ----------------- | --------------- |
| Device Managed    | Yes             |
| Compliance State  | Compliant       |
| Windows Hello PIN | Configured      |
| BitLocker Status  | Enabled         |

---
# Security Baseline Summary

| Control              | Required |
| -------------------- | -------- |
| Intune Enrollment    | Yes      |
| Windows Hello PIN    | Yes      |
| BitLocker Encryption | Yes      |
| Microsoft Defender   | Yes      |
| Windows Firewall     | Yes      |
| Automatic Updates    | Yes      |
| Conditional Access   | Yes      |

---
# Expected Outcome

After implementation:

* All Windows devices are centrally managed through Microsoft Intune.
* Users must authenticate using a Windows Hello PIN.
* Device storage is protected using BitLocker encryption.
* Recovery keys are stored securely in Microsoft Entra ID.
* Only compliant devices can access Microsoft 365 resources.
* IT administrators can monitor compliance and security posture from a single management console.




















The Windows Device Configuration Profile establishes a baseline security configuration for all corporate Windows devices managed through Microsoft Intune. The profile is designed to enforce essential security controls, protect organizational data, and ensure compliance with internal security requirements.

## Profile Information

| Attribute           | Value                                  |
| ------------------- | -------------------------------------- |
| Profile Name        | Windows – Basic Security Configuration |
| Platform            | Windows 10 and Windows 11              |
| Management Solution | Microsoft Intune                       |
| Assignment Scope    | All Intune-managed Windows devices     |

## Configuration Settings

### Authentication

Windows Hello for Business is enabled to provide secure user authentication through a PIN-based sign-in method.

| Setting                    | Configuration        |
| -------------------------- | -------------------- |
| Windows Hello for Business | Enabled              |
| Minimum PIN Length         | 6 Digits             |
| PIN Requirement            | Numeric PIN Required |

### Device Encryption

BitLocker Drive Encryption is enabled to protect data stored on corporate devices and reduce the risk of unauthorized access in the event of device loss or theft.

| Setting              | Configuration      |
| -------------------- | ------------------ |
| BitLocker Encryption | Enabled            |
| Recovery Key Backup  | Microsoft Entra ID |

### Endpoint Protection

Microsoft Defender Antivirus and Windows Firewall are enabled to provide baseline protection against malware, unauthorized access, and network-based threats.

| Setting                      | Configuration |
| ---------------------------- | ------------- |
| Microsoft Defender Antivirus | Enabled       |
| Windows Firewall             | Enabled       |

### Operating System Updates

Automatic Windows Updates are enabled to ensure devices receive security patches, quality updates, and feature improvements in a timely manner.

| Setting                   | Configuration |
| ------------------------- | ------------- |
| Automatic Windows Updates | Enabled       |

## Expected Outcome

Implementation of this profile ensures that all managed Windows devices:

* Utilize secure PIN-based authentication.
* Maintain full disk encryption through BitLocker.
* Store BitLocker recovery keys securely within Microsoft Entra ID.
* Operate with Microsoft Defender Antivirus and Windows Firewall enabled.
* Receive security updates and patches automatically.
* Adhere to the organization's minimum endpoint security requirements.

* ### Success Criteria

* 100% of corporate devices enrolled in Intune
* Device ownership and compliance status visible to IT administrators


