# Project Kingfisher

Project Kingfisher is a self-contained cybersecurity lab demonstrating 
end-to-end phishing simulation and detection engineering. Three virtual 
machines work together across an isolated NAT network to simulate the 
full attacker-defender loop, from initial email delivery through to 
SIEM-based detection rule authoring.

**DISCLAIMER:** This project is conducted entirely within an isolated lab 
environment against decoy accounts under the author's own ownership. 
All techniques demonstrated are for educational purposes only.

## Lab Environment

| VM | Role | OS | IP |
|---|---|---|---|
| Kali_Linux | Attacker | Kali Linux | 192.168.18.129 |
| Windows 10 x64 | Victim | Windows 10 | 192.168.18.128 |
| Ubuntu_Ult | Defender | Ubuntu Server 22.04.5 LTS | 192.168.18.130 |

## Phases

| Phase | Focus | Status |
|---|---|---|
| Phase 1 | Attacker Infrastructure & Phishing Campaign | Completed |
| Phase 2 | Victim Instrumentation & Telemetry Capture | In Progress |
| Phase 3 | Detection Engineering & SIEM Rule Authoring | Pending |

---

# Project Kingfisher | Phase 1: Attacker Infrastructure & Phishing Campaign

**Duration:** 19/04/2026 → 27/04/2026  

**Platform:** Kali Linux (VMware Workstation Pro), Windows 10 (VMware Workstation Pro)  

**Tools:** Gophish, Gmail SMTP, Custom HTML Landing Page

## Overview

Deployed a fully operational phishing infrastructure on an isolated lab network. Executed two real phishing campaigns against decoy victims, capturing credentials and observing live defensive mechanisms across both runs.

---

## Infrastructure

| Component | Detail |
|---|---|
| Phishing Framework | Gophish 0.12.1 on Kali Linux |
| Email Delivery | Gmail SMTP (smtp.gmail.com:587) with app password auth |
| Landing Page | Cloned Microsoft 365 login page (Harvest #1) → custom HTML form (Harvest #2) |
| Attacker IP | 192.168.18.129 |
| Victims | 2 decoy Gmail accounts |

---

## Campaign Results

### Harvest #1

| Victim | Sent | Opened | Clicked | Submitted |
|---|---|---|---|---|
| James Smith | OK | OK | OK | Blocked |
| Sarah Chen | OK | OK | OK | Blocked |

**Campaign dashboard showing 2 sent, 2 opened, 2 clicked, 0 submitted.**

![Harvest #1 results dashboard](project-kingfisher-screenshot-phase1/harvest1-dashboard.png)

**Email Delivery to James Smith:**  
Email delivered directly to the primary inbox with personalised greeting and functioning phishing link.

![James' inbox](project-kingfisher-screenshot-phase1/james-inbox.png)

**Email Delivery to Sarah Chen:**  
Email automatically routed to Gmail's spam folder with reputation-based filtering banner. Subsequently retrieved manually from spam by the victim.

![Sarah's spam](project-kingfisher-screenshot-phase1/sarah-spam.png)

**Victim Interaction to Cloned Landing Page:**  
Both victims clicked the phishing link, loading the cloned Microsoft 365 login page served from the attacker's infrastructure (192.168.18.129).

**Defensive Block to Client-Side Validation:**  
Credential submission was blocked by Microsoft's retained client-side JavaScript validation, which attempted to authenticate Gmail addresses against Microsoft's directory service.

![Microsoft client-side validation error](project-kingfisher-screenshot-phase1/validation-error.png)

---

### Harvest #2

| Victim | Sent | Opened | Clicked | Submitted |
|---|---|---|---|---|
| James Smith | OK | OK | OK | OK |
| Sarah Chen | OK | OK | OK | OK |

**Campaign dashboard showing 2 sent, 2 opened, 2 clicked, 2 submitted.**

![Harvest #2 results dashboard](project-kingfisher-screenshot-phase1/harvest2-dashboard.png)

**Custom HTML Landing Page:**  
Cloned Microsoft 365 page replaced with externally-sourced custom HTML form, bypassing the client-side validation block from Harvest #1.

![Custom HTML landing page on victim Win10](project-kingfisher-screenshot-phase1/harvest2-landing-page.png)

**Security Awareness Redirect:**  
Upon credential submission, victims were redirected to a custom security awareness page hosted on the attacker's infrastructure (192.168.18.129:8080), confirming the simulated attack was successful.

![Security awareness redirect page](project-kingfisher-screenshot-phase1/awareness-redirect.png)

**Victim Timeline of James Smith:**  
Full attack chain captured with OS and browser fingerprinting on each event (Windows 10, Chrome 147.0.0.0).

![James' timeline](project-kingfisher-screenshot-phase1/james-timeline.png)

**Victim Timeline of Sarah Chen:**  
Full attack chain captured with OS and browser fingerprinting on each event (Windows 10, Chrome 147.0.0.0).

![Sarah's timeline](project-kingfisher-screenshot-phase1/sarah-timeline.png)

---

## Key Observations

**Harvest #1 → Harvest #2 demonstrates the attacker adaptation loop:**  
Initial deployment surfaced two defensive signals (spam filtering, JS validation). Both were analysed and mitigated in the second attempt that reflects the real-world offensive iteration.

A more sophisticated attacker would additionally:
- Strip validation JavaScript before page deployment
- Serve pages over HTTPS with a convincing domain
- Host all assets locally for full visual fidelity

---

## MITRE ATT&CK Coverage

| Technique | ID |
|---|---|
| Phishing: Spearphishing Link | T1566.002 |
| User Execution: Malicious Link | T1204.001 |
| Input Capture: Web Portal Capture | T1056.003 |
| Valid Accounts | T1078 |

---

## Conclusion

Project Kingfisher demonstrates the full attacker-defender loop across 
a self-contained isolated lab environment. Phase 1 executed two real 
phishing campaigns against decoy victims, observed live defensive 
mechanisms, and achieved complete credential capture through iterative 
attack adaptation. Phases 2 and 3 will extend this into defensive 
instrumentation and SIEM-based detection engineering. Therefore, building the 
complete picture from initial phishing delivery through to detection 
rule authoring.

---
