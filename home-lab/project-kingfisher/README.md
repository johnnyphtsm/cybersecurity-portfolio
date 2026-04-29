# Project Kingfisher

Project Kingfisher is a self-contained cybersecurity lab demonstrating 
end-to-end phishing simulation and detection engineering. Three virtual 
machines work together across an isolated NAT network to simulate the 
full attacker-defender loop, from initial email delivery through to 
SIEM-based detection rule authoring.

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
| Landing Page | Cloned Microsoft 365 login page → custom HTML form |
| Attacker IP | 192.168.18.129 |
| Victims | 2 decoy Gmail accounts |

---

## Campaign Results

### Harvest #1

![Campaign results dashboard](screenshots/phase1/10-campaign-results.png)

![James inbox](screenshots/phase1/11-james-inbox.png)

![Sarah spam](screenshots/phase1/12-sarah-spam.png)

| Victim | Sent | Opened | Clicked | Submitted |
|---|---|---|---|---|
| James Smith | OK | OK | OK | Blocked |
| Sarah Chen | OK | OK | OK | Blocked |

**Defensive observations:**
- Gmail routed Sarah's email to spam via reputation-based filtering — Sarah retrieved it manually, defeating the filter through user behaviour
- Microsoft's retained client-side JavaScript validation blocked credential submission for non-tenant emails

### Harvest #2

![Harvest 2 results dashboard](screenshots/phase1/16-harvest2-results.png)

![James timeline](screenshots/phase1/19-james-timeline.png)

![Sarah timeline](screenshots/phase1/20-sarah-timeline.png)

| Victim | Sent | Opened | Clicked | Submitted |
|---|---|---|---|---|
| James Smith | OK | OK | OK | OK |
| Sarah Chen | OK | OK | OK | OK |

Replaced cloned page with a custom HTML login form to bypass JavaScript validation. Both victims submitted credentials. Per-victim timelines confirmed full attack chain with OS and browser fingerprinting (Windows 10, Chrome 147.0.0.0).

---

## Key Findings

**Harvest #1 → Harvest #2 demonstrates the attacker adaptation loop:**  
Initial deployment surfaced two defensive signals (spam filtering, JS validation). Both were analysed and mitigated in the second attempt — reflecting real-world offensive iteration.

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
instrumentation and SIEM-based detection engineering — building the 
complete picture from initial phishing delivery through to detection 
rule authoring.

This project is independently built alongside a first-year Bachelor of 
Cybersecurity at the University of Technology Sydney, targeting a career 
path in SOC analysis, security engineering, and purple team operations.

---
