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


---

## 🕵️‍♂️ Project 11: Dark Web Credential Leak Mitigation (User Risk Policy)

**Objective:** Architect an automated containment strategy against credential harvesting and dark web credential leaks using Entra ID Conditional Access User Risk evaluation.

**Framework Alignment:** * **MITRE ATT&CK:** Credential Access (TA0006) -> Credentials from Password Stores (T1555).
* **Defensive Mechanism:** Automated Account Remediation & Forced Password Reset.

### 1. The Threat Landscape
When an identity's credentials are computationally verified to be leaked on the public internet or dark web, Microsoft's Threat Intelligence flags the identity with a "High User Risk" status. A compromised password means the traditional perimeter has already failed.

### 2. The Architectural Defense (Automated Containment)
Due to strict security testing ethics, simulating a real credential leak on public pastebins is prohibited. However, the defensive architecture was deployed via Conditional Access to ensure zero-day mitigation.

The `MITRE-User-Risk-PasswordReset` policy is configured to intercept any authentication attempt from an identity flagged with High User Risk. Instead of merely blocking access (which causes operational downtime), the policy forces a secure **Password Change** flow, requiring the user to prove their identity via MFA before establishing a new, uncompromised password.

<img width="1912" height="1029" alt="image" src="https://github.com/user-attachments/assets/f3c574b7-0d68-4785-b2bc-38f6517c8e0f" />


**Skills Applied:** Threat Intelligence Integration, Conditional Access User Risk, Automated Incident Remediation (SOAR concepts), Credential Access Mitigation.


---

## 🔗 Project 12: SIEM Integration & Centralized Visibility (Microsoft Sentinel)

**Objective:** Centralize threat intelligence and security alerts by ingesting Microsoft Entra ID Protection telemetry into Microsoft Sentinel (SIEM) for unified SOC monitoring and incident response.

**Framework Alignment:** * **SOC Operations:** Single Pane of Glass monitoring.
* **Microsoft Cloud Adoption Framework:** Security visibility and reporting.

### 1. Breaking Data Silos (Data Connectors)
In an enterprise SOC environment, analysts rely on a centralized SIEM to correlate events across the kill chain. Keeping Identity Risk alerts isolated within the IAM portal creates blind spots. To resolve this, the **Microsoft Entra ID Protection Data Connector** was configured within Microsoft Sentinel.

### 2. Telemetry Ingestion & KQL Threat Hunting
With the pipeline established, the high-risk and medium-risk alerts generated by the AI (such as the Anonymous IP login attempt from Project 10) are now natively streamed into the `SecurityAlert` table in the Sentinel Log Analytics Workspace.

By executing targeted KQL (Kusto Query Language) queries, the SOC can instantly hunt for Identity Protection Center (IPC) alerts, extracting critical entities for rapid containment.

<img width="1911" height="1022" alt="image" src="https://github.com/user-attachments/assets/70fdc142-4312-478a-bdae-30f6922c5254" />


**Skills Applied:** SIEM Configuration, Data Connector Engineering, KQL (Kusto Query Language), Security Operations (SecOps), Alert Triage.


---

## ⚖️ Project 13: Identity Governance & Automated Access Reviews

**Objective:** Implement a continuous governance framework to eliminate "Privilege Creep" by automating the periodic re-validation of user access rights, specifically targeting sensitive security groups.

**Framework & Compliance Alignment:**
* **ISO 27001 (A.9.2.5):** Review of user access rights.
* **GDPR (Data Minimization & Integrity):** Ensuring only currently authorized personnel retain access to environments mapped to the Dublin operations.

### 1. The Governance Gap
Manual access audits are prone to human error and often result in orphaned accounts retaining high-level permissions. To mitigate this vulnerability, an automated **Access Review** workflow was engineered within Entra ID Identity Governance.

### 2. Live Campaign Execution
The workflow `Quarterly-Security-Audit-Dublin` was deployed and is actively monitoring the `Auditores-Irlanda` security group. The engine is configured with a continuous monthly recurrence, ensuring compliance without manual IT intervention.
> *Evidence of the Active Identity Governance campaign targeting the specific security group.*

<img width="1919" height="1026" alt="image" src="https://github.com/user-attachments/assets/f1f98195-9926-4a77-8dab-e0e38c1d1f01" />


### 3. Automated Remediation Logic ("Deny by Default")
The policy is strictly configured for automated remediation. If a designated reviewer fails to validate a user's continued need for access within the 3-day review window, the system defaults to a secure posture: the identity is automatically evicted from the security group.

### 4. AI-Assisted Decision Making
Utilizing Azure's "Inactive User" telemetry, the system provides reviewers with data-driven recommendations, highlighting identities that haven't performed a sign-in within the last 30 days, further reducing the organization's attack surface.

**Skills Applied:** Identity Governance, Compliance Automation (ISO/GDPR), Access Review Orchestration, Risk Mitigation.

---

## 🛡️ Project 14: Cloud Workload Protection Platform (CWPP) via Microsoft Defender

**Objective:** Secure IaaS (Infrastructure as a Service) resources by enabling automated threat detection, vulnerability assessment, and malware defense across all Azure Virtual Machines.

**Framework & Compliance Alignment:**
* **CIS Controls (v8):** Control 9 (Malware Defenses) and Control 7 (Continuous Vulnerability Management).
* **NIST SP 800-53:** System and Information Integrity (SI) controls.

### 1. Enabling CWPP (Microsoft Defender for Servers)
To shift from a reactive to a proactive defense posture, Microsoft Defender for Cloud was enabled at the subscription level, specifically activating the **Defender for Servers** plan. This operationalizes the Cloud Workload Protection Platform (CWPP).

### 2. Automated Agent Provisioning (Zero-Touch Security)
Security agents should not rely on manual deployment. The environment was configured for auto-provisioning. Upon the creation of any new virtual machine (Linux or Windows), the Azure fabric will automatically inject the Microsoft Defender for Endpoint agent and the Vulnerability Assessment scanner.

> *Proof of architectural configuration: Defender for Servers activated at the subscription level.*

<img width="1912" height="1031" alt="image" src="https://github.com/user-attachments/assets/ac99bb22-2c44-4b21-ba67-0b73a73f8e42" />

<img width="1915" height="1029" alt="image" src="https://github.com/user-attachments/assets/fb98c58b-a305-4739-b967-d37b657a7900" />

<img width="1916" height="1027" alt="image" src="https://github.com/user-attachments/assets/273836a1-ecab-4ae5-b084-1b53cb7ed2f9" />

### 3. Continuous Vulnerability Management
With this architecture in place, any compute workload deployed for future NOC/SOC labs will be immediately assessed for OS misconfigurations, missing patches, and active malware threats, streaming alerts directly back to our Microsoft Sentinel SIEM.

**Skills Applied:** Microsoft Defender for Cloud (MDC), CWPP, Auto-provisioning, Vulnerability Management, IaaS Security.
