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



<img width="1906" height="952" alt="image" src="https://github.com/user-attachments/assets/132c358b-fb55-47fe-94b7-db3fdf108991" />
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

<img width="1904" height="959" alt="Screenshot 2026-06-05 143606" src="https://github.com/user-attachments/assets/40e6b216-1dd6-4180-b91d-e7be38385ab9" />


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




# Microsoft Intune Policy Roadmap

## Purpose
This roadmap outlines the phased implementation of Microsoft Intune policies to establish a secure and manageable Windows device environment. The objective is to achieve a baseline security posture while minimizing operational impact and ensuring a positive user experience.

---

# Phase 1 – Foundation and Device Enrollment

## Objective
Establish centralized device management and ensure all corporate devices are enrolled in Microsoft Intune.

### Deliverables
* Microsoft Intune tenant configuration
* Microsoft Entra ID integration
* Device enrollment process established
* Corporate Windows devices enrolled in Intune
* Device inventory and asset visibility

### Success Criteria

* 100% of corporate devices enrolled in Intune
* Device ownership and compliance status visible to IT administrators

---

# Phase 2 – Baseline Security Controls
## Objective

Implement minimum security requirements across all managed devices.

### Policies Implemented

#### Compliance Policy

* Require BitLocker encryption
* Require Microsoft Defender Antivirus
* Require Windows Firewall

#### Device Configuration Profile

* Enable Windows Hello for Business
* Enforce minimum 6-digit PIN
* Enable BitLocker encryption
* Store BitLocker recovery keys in Microsoft Entra ID
* Enable Microsoft Defender Antivirus
* Enable Windows Firewall

### Success Criteria

* All managed devices encrypted
* All users configured with Windows Hello PIN
* Devices report as compliant within Intune

---

# Phase 3 – Conditional Access

## Objective

Restrict access to organizational resources from non-compliant or unmanaged devices.

### Policies Implemented

* Require compliant device for Microsoft 365 access
* Block access from unmanaged devices
* Require multifactor authentication (MFA)

### Success Criteria

* Corporate resources accessible only from compliant devices
* MFA enabled for all users

---

# Phase 4 – Endpoint Protection

## Objective

Strengthen endpoint security and reduce exposure to threats.

### Policies Implemented

* Microsoft Defender Antivirus policies
* Attack Surface Reduction (ASR) rules
* Tamper Protection
* Web content filtering
* Endpoint detection and response (where licensed)

### Success Criteria

* Enhanced malware and ransomware protection
* Reduced attack surface across endpoints
---

# Phase 5 – Update Management
## Objective

Ensure devices remain secure and supported through controlled update deployment.

### Policies Implemented

* Windows Update Rings
* Feature Update Policies
* Quality Update Policies

### Success Criteria

* Timely deployment of security updates
* Reduced vulnerability exposure
* Standardized operating system versions

---
# Phase 6 – Application Management

## Objective

Standardize and secure software deployment.

### Policies Implemented

* Corporate application deployment
* Application update management
* Removal of unauthorized software
* Microsoft 365 application deployment

### Success Criteria
* Consistent software inventory
* Reduced manual software installation effort

---

# Phase 7 – Advanced Security and Compliance

## Objective
Implement advanced controls to support organizational security and compliance requirements.

### Policies Implemented

* Device control policies (USB restrictions)
* Data Loss Prevention (DLP)
* Endpoint Privilege Management
* Security baselines
* Compliance reporting and auditing

### Success Criteria

* Improved protection of sensitive data
* Enhanced compliance reporting capabilities
* Reduced risk of data leakage
---
# Roadmap Timeline

| Phase                                    | Timeline  |
| ---------------------------------------- | --------- |
| Phase 1 – Enrollment & Foundation        | Week 1    |
| Phase 2 – Baseline Security Controls     | Week 1–2  |
| Phase 3 – Conditional Access & MFA       | Week 2    |
| Phase 4 – Endpoint Protection            | Week 3    |
| Phase 5 – Update Management              | Week 3–4  |
| Phase 6 – Application Management         | Month 2   |
| Phase 7 – Advanced Security & Compliance | Month 2–3 |













