# sharepoint-zero-day-cs50-cybersecurity
CS50 Cybersecurity Final Project - Analysis of CVE-2025-53770 SharePoint Zero-Day Attacks
# The 2025 Microsoft SharePoint Zero-Day Attacks (CVE-2025-53770)

#### CS50's Introduction to Cybersecurity — Final Project
#### Student Name: Nasim Gharebegi
#### edX Username: thiconickid0
#### GitHub Username: thiconickid
#### Video Presentation: [Watch on YouTube](https://youtu.be/wtCOoKn2TIA?is=xAJd8j-mF9-Mbf6X)

---

## Project Overview

This project provides an in-depth cybersecurity analysis of the zero-day attacks targeting on-premises **Microsoft SharePoint Server** installations, initially reported by Microsoft in July 2025. The core vulnerability driving these attacks is tracked as **CVE-2025-53770**, a high-severity Remote Code Execution (RCE) flaw stemming from insecure/unsafe deserialization of untrusted input.

The primary objective of this project is to bridge real-world cyber threat intelligence with the foundational security principles taught in **CS50 Cybersecurity**, specifically focusing on software vulnerabilities, authentication bypass mechanisms, the attack lifecycle, the CIA triad, and defense-in-depth strategies.

---

## Video Presentation

A comprehensive voice-over presentation detailing this incident has been recorded and uploaded as part of the CS50 Cybersecurity final submission requirements:

* **YouTube URL:** [Watch on YouTube](https://youtu.be/wtCOoKn2TIA?is=xAJd8j-mF9-Mbf6X)
* **Recording Date:** October 16, 2026

---

## Technical Deep Dive: Vulnerability Mechanism

### CVE-2025-53770 (Remote Code Execution)
* **Root Cause:** Unsafe deserialization of untrusted data within on-premises Microsoft SharePoint Server.
* **Mechanism:** When a server accepts serialized objects from a user or client without validating their integrity or type, an attacker can construct malicious payload streams. Upon deserialization, the application executes arbitrary code with the privilege level of the underlying service account.
* **Authentication Bypass (ToolShell):** Attackers chained CVE-2025-53770 alongside authentication bypass flaws (such as CVE-2025-53771) and exposed endpoints (e.g., `ToolPane.aspx`). This allowed unauthenticated, network-adjacent attackers to achieve RCE over HTTP/HTTPS.

---

## Attack Lifecycle & Observed Threat Activity

1. **Initial Access & Exploitation:** Attackers identified internet-facing SharePoint Server instances and submitted crafted requests exploiting CVE-2025-53770 to gain unauthenticated RCE.
2. **Persistence & Web Shell Deployment:** Successful exploitation led to the installation of web shells, notably `spinstall0.aspx`.
3. **Credential Harvesting & Key Extraction:** The malicious scripts targeted ASP.NET cryptographic machine keys (`ValidationKey` and `DecryptionKey`), compromising session security and application-level verification.
4. **Lateral Movement & Ransomware:** 
   * Threat groups **Linen Typhoon** and **Violet Typhoon** utilized the access for cyber espionage and data exfiltration.
   * Financially motivated threat actor **Storm-2603** leveraged this foot-hold to move laterally and deploy **Warlock Ransomware**.

---

## Mapping to CS50 Cybersecurity Principles

| Security Principle | Incident Impact & Real-World Application |
| :--- | :--- |
| **Confidentiality** | Sensitive organizational documents stored on SharePoint, employee records, and ASP.NET MachineKeys were directly exposed to unauthorized actors. |
| **Integrity** | Attackers modified web application files by dropping ASPX web shells (`spinstall0.aspx`) and altering server configurations. |
| **Availability** | The deployment of Warlock Ransomware encrypted critical corporate infrastructure, causing major operational disruption. |
| **Defense in Depth** | Demonstrates that firewalls alone are insufficient when web endpoints are vulnerable; security requires patching, AMSI, network segmentation, and log auditing. |

---

## Mitigation and Defense Recommendations

Organizations managing on-premises SharePoint deployment should enforce the following defensive measures:

1. **Emergency Patch Management:** Apply Microsoft's official security updates for supported SharePoint Server versions immediately.
2. **AMSI Integration:** Ensure the Antimalware Scan Interface (AMSI) is enabled and integrated with Microsoft Defender Antivirus or third-party EDR solutions.
3. **ASP.NET Key Rotation:** Rotate compromised ASP.NET `MachineKeys` and restart IIS services to invalidate hijacked session tokens.
4. **Threat Hunting & Incident Response:** Inspect IIS access logs for requests targeting `ToolPane.aspx` and search for suspicious file modifications (`spinstall0.aspx`).
5. **Least Privilege & Isolation:** Restrict SharePoint service account privileges and isolate server nodes from sensitive internal networks.

---

## References & Sources

1. **Microsoft Security Response Center (MSRC):** *Customer guidance for SharePoint vulnerability CVE-2025-53770.*  
   `https://www.microsoft.com/en-us/msrc/blog/2025/07/customer-guidance-for-sharepoint-vulnerability-cve-2025-53770/`
2. **Microsoft Threat Intelligence:** *Disrupting active exploitation of on-premises SharePoint vulnerabilities.*  
   `https://www.microsoft.com/en-us/security/blog/2025/07/22/disrupting-active-exploitation-of-on-premises-sharepoint-vulnerabilities/`
3. **CISA Known Exploited Vulnerabilities Catalog:** *CVE-2025-53770 Assessment.*  
   `https://www.cisa.gov/known-exploited-vulnerabilities-catalog`
4. **CS50 Introduction to Cybersecurity:** *Final Project Requirements.*  
   `https://cs50.harvard.edu/cybersecurity/project/`
