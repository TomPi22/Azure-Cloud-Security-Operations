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
