# Azure Cloud Security Operations

Welcome to my Cloud Security portfolio. This repository documents my hands-on experience and architectural implementations in Microsoft Azure, focusing on Identity and Access Management (IAM), Threat Detection (SIEM), and Zero Trust principles. 

These projects align with the core competencies required for SOC Analyst and Cloud Security Engineering roles, drawing upon frameworks from CompTIA Security+, AZ-900, and AZ-500.

> 🔗 **[For my foundational on-premises infrastructure and NOC projects, click here.](https://github.com/TomPi22/IT_Support_HomeLab)**

---

## 🛡️ Project 8: Zero Trust Architecture & Geofencing (Conditional Access)

**Objective:** Implement a Zero Trust perimeter using Microsoft Entra ID (formerly Azure AD) Conditional Access Policies to protect administrative accounts from foreign unauthorized access, even in the event of credential compromise.

**Scenario:** The organization operates strictly within Ireland. A "Geofencing" security policy was designed to block any authentication attempts originating outside of the Republic of Ireland. 

### 1. The Attacker's Perspective (The Block)
An authentication attempt was simulated from a foreign IP address (Brazil) using valid, compromised credentials. The Conditional Access Policy instantly intercepted the login, proving that identity is the new security perimeter.
<img width="1894" height="934" alt="image" src="https://github.com/user-attachments/assets/44501fcf-7c4a-4746-9979-7ac541c0dfd3" />


### 2. The Analyst's Perspective (Telemetry & Tracking)
Monitoring the Azure Sign-in logs, the SOC can clearly trace the unauthorized attempt. The telemetry accurately mapped the origin IP and applied the `Block-Non-Irish-Logins` policy as the enforcement mechanism.
<img width="1905" height="935" alt="image" src="https://github.com/user-attachments/assets/09b895db-7595-4f3f-8ab1-bc2c07f764af" />


### 3. Forensic Evidence (Error Code 53003)
Deep diving into the authentication details reveals the exact point of failure. The presence of **Error Code 53003** provides forensic proof that the login was dropped out-of-band by the Conditional Access engine, successfully preventing a potential account takeover (ATO).
<img width="1908" height="930" alt="image" src="https://github.com/user-attachments/assets/2451bd22-2a13-4f85-b876-a8aa61bb3607" />


**Skills Applied:** Identity and Access Management (IAM), Microsoft Entra ID P2, Zero Trust Principles, Log Analysis, Security Policy Enforcement.


---

## ⏱️ Project 9: Zero Standing Privileges & Just-In-Time (JIT) Access (Azure PIM)

**Objective:** Mitigate the risk of compromised administrative accounts by eliminating permanent access (Standing Privileges). Implemented Azure Privileged Identity Management (PIM) to enforce Just-In-Time (JIT) role elevation.

**Framework & Compliance Alignment:** * **NIST Cybersecurity Framework (PR.AC-4):** Identity management, authentication, and access control.
* **Principle of Least Privilege (PoLP):** Ensuring identities have zero permissions at rest.
* **GDPR Compliance (DPC Ireland focus):** Ensuring strict, auditable, and time-bound access logs to infrastructure handling sensitive data.

### 1. The Baseline: Zero Standing Privileges (ZSP)
By default, even high-tier security identities are kept in an "Eligible" (dormant) state. When attempting to provision infrastructure (e.g., creating a Resource Group or Virtual Machine) without active elevation, the Azure Resource Manager (ARM) inherently blocks the action.
> *Proof of ZSP: The user `sec_admin` is denied access at the Management Plane level.*
<img width="1910" height="938" alt="image" src="https://github.com/user-attachments/assets/bcf0fa5f-d6c9-45da-a1a6-babe2a2bb54d" />


### 2. The JIT Elevation Workflow
To perform administrative duties, the identity must request temporary elevation. This workflow enforces a strict verification process, requiring Multi-Factor Authentication (MFA) and a documented business justification for the audit trail.
> *The 3-stage Azure PIM activation sequence validating the request.*
<img width="1907" height="933" alt="image" src="https://github.com/user-attachments/assets/883ad9f2-29e8-4a7b-b262-2d97e0bff24d" />
<img width="1912" height="934" alt="image" src="https://github.com/user-attachments/assets/0237c62c-11a2-4d0d-a2b0-7a10927c52f6" />
<img width="1908" height="933" alt="image" src="https://github.com/user-attachments/assets/123a1869-4bc1-4e9a-80d0-5898c0be9869" />


### 3. Time-Bound Active Assignment
Once approved, the identity is granted the `Contributor` role for a strictly limited window (configured to a maximum of 2 hours to minimize the attack surface). The Azure control plane actively tracks the expiration timestamp.
> *Active role assignment tracking showing the exact minute the privilege will be automatically revoked.*
<img width="1910" height="936" alt="image" src="https://github.com/user-attachments/assets/a4de8731-1b5f-4492-931a-e579d93bba8e" />


### 4. Elevated Execution
With the JIT access successfully provisioned, the architectural block is lifted, allowing the administrator to deploy critical infrastructure within the approved time window safely.
> *Successful bypass of the RBAC restriction post-elevation, allowing the creation of the `TesteHacker_group`.*
<img width="1908" height="936" alt="image" src="https://github.com/user-attachments/assets/a7ba2abc-fd5e-4101-8a81-58ee94dd6d48" />


**Skills Applied:** Privileged Access Management (PAM), Azure PIM, JIT Access, NIST PR.AC, Audit Compliance.
