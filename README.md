# MaritiniAD — Active Directory Lab Walkthrough

**Author:** Daniel Etim
**Platform:** [HackSmarter](https://hacksmarter.org)  
**Target:** `DC01.DRY.MARTINI.BARS` (`10.1.248.72`)  

---

## Overview

This repository contains a complete penetration testing walkthrough of the **MaritiniAD** lab, originally hosted on **HackSmarter**. This lab focuses on Active Directory enumeration, Kerberoasting, and privilege escalation through misconfigurations.

The environment is a Windows Server 2025 Domain Controller with strict LDAP signing and TLS enforcement, which required troubleshooting modern Python LDAP libraries and utilizing alternative tools.

> **Disclaimer:** The content in this repository is for educational purposes only and is intended to be used in authorized lab environments. All credit for the environment design and infrastructure goes to the **HackSmarter** team. 

---

## Attack Path Summary

1. **Initial Access:** Enumerated a Guest-accessible SMB share (`notes`) and discovered plaintext credentials.
2. **Domain Enumeration:** Bypassed LDAP signing and TLS issues by compiling `RustHound-CE` to ingest AD data into BloodHound.
3. **Kerberoasting:** Identified a Kerberoastable Domain Admin service account (`ATHENA_SVC`) and cracked its RC4 hash offline.
4. **Privilege Escalation:** Discovered password reuse across Domain Admin accounts (`ATHENA_SVC` and `athena.t0`).
5. **Domain Compromise:** Abused the reused credentials to perform a DCSync attack and extract the `krbtgt` NTLM hash.

---

## Tools Used

- **RustScan / Nmap** – Network & Port Discovery
- **NetExec (nxc)** – SMB, LDAP, and WinRM Enumeration/Authentication
- **RustHound-CE** – Active Directory data collection for BloodHound
- **John the Ripper** – Offline password cracking
- **Impacket** – DCSync and remote execution
- **Pandoc** – PDF report generation

---

## Repository Structure

- `README.md` – Lab overview and attack summary.
- `report.md` – The complete, detailed walkthrough in Markdown.
- `MaritiniAD_Report.pdf` – The formatted PDF version of the report.
- `images/` – Contains all visual evidence (BloodHound graphs, terminal outputs).

---

## Key Takeaways

This lab highlighted several critical Active Directory security concepts:
- **Excessive Privileges:** Guest users should never have write access to SMB shares.
- **Weak Service Accounts:** Service accounts with Domain Admin rights must use strong, unique passwords (or gMSA).
- **Password Reuse:** Reusing passwords across administrative accounts is a critical vulnerability.
- **Modern Hardening:** Windows Server 2025 strict LDAP signing breaks many legacy Python tools; modern alternatives like RustHound-CE are necessary.

---

## Credits

- **Lab Creator:** [HackSmarter](https://hacksmarter.org)
- **Walkthrough Author:** Daniel Etim

If you find this walkthrough helpful, please star ⭐ the repository and check out the other amazing labs on HackSmarter!
