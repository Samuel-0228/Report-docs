# Incident

## Overview

Incident is a practical cybersecurity project built to simulate a system security incident and work through the process of investigating a compromise and improving system security.

The project is an interactive investigation environment developed with React, TypeScript, and Tailwind CSS. It models a realistic internal security event, allowing an investigator to inspect telemetry, examine evidence artifacts, trace an attack path, make containment decisions, and review post-incident findings. The project was built as an educational tool to apply cybersecurity concepts through software development rather than as a production security platform.

## What I Worked On

To build Incident, I designed and implemented a complete simulation of a corporate security compromise (case INC-2026-0825 at a fictitious organization named Northstar Systems). The project models both the technical artifacts left by an attacker and the workflows used by security analysts to investigate and contain the threat.

The implementation includes the following components and security workflows:

### Incident Detection and Triage
- Implemented an incident overview dashboard that displays initial case metadata, threat severity levels, affected systems, and investigation objectives.
- Built a chronological timeline viewer tracking events from initial email delivery at 09:42 UTC to final data exfiltration. Events are categorized by telemetry type, including authentication, network traffic, file activity, identity changes, and endpoint logs.

### Evidence Inspection and Behavioral Analysis
- Developed an evidence explorer to inspect raw artifacts across multiple categories. This includes spoofed email headers showing Sender Policy Framework (SPF) validation failures, Windows security event logs (Event ID 4768 Kerberos ticket requests and Event ID 4624 network logons), and network capture summaries.
- Created a malware analysis module that displays file metadata, SHA-256 hashes, behavioral indicators, and an interactive process tree. The process tree models a parent process (EXCEL.EXE) executing a macro that spawns cmd.exe and PowerShell, which subsequently downloads and executes a secondary binary (updater.exe).
- Implemented a virtual filesystem browser that allows inspection of directory paths, suspicious downloads, temporary staging folders, and corporate policy documents.

### Network and Identity Mapping
- Built an interactive network topology map displaying internal subnets, gateway nodes, domain controllers, file servers, database servers, and external command and control (C2) hosts.
- Created an endpoint inventory tracking operating systems, hostnames, IP addresses, open network ports, and individual compromise statuses.
- Implemented an identity explorer showing user accounts, privilege levels, failed logon attempts, and anomalous logins from external IP addresses.

### Attack Path Reconstruction and Response Decisions
- Modeled an attack chain tracker based on common attack stages: initial access, phishing, credential theft, account compromise, lateral movement, privilege escalation, data collection, and exfiltration.
- Built a decision prompt system that presents containment choices during active lateral movement. The system evaluates the effectiveness of actions such as full host isolation and account disabling compared to incomplete responses like perimeter-only IP blocking or passive monitoring.
- Implemented a challenge board with guided investigative objectives and flag verification to validate analytical findings.
- Developed a post-incident report view summarizing investigation accuracy, reviewed evidence, executive findings, and the reconstructed timeline.

## Skills Applied

### Incident Analysis
Building the project required understanding how an intrusion progresses across an internal network. I mapped out the relationships between different forensic artifacts, such as correlating an initial phishing email with macro execution, identifying credential harvesting in memory, tracking remote NTLM authentication, and tracing lateral SMB connections to file shares.

### Vulnerability and Security Analysis
The simulation required identifying specific security weaknesses that allow an attack to succeed. These weaknesses included:
- Allowing macro execution in downloaded spreadsheets without policy restrictions.
- Inadequate SPF verification on inbound email relays allowing address spoofing.
- Accepting legacy NTLM authentication across external boundaries.
- Storing unencrypted sensitive archives in accessible temporary directories.
- Unrestricted outbound network egress allowing HTTPS connections to arbitrary external IP addresses.

### System Hardening
Working through the incident highlighted the defensive configurations needed to prevent similar compromises. I incorporated these considerations into the project logic and response actions:
- Enforcing application control policies and restricting macro execution in office applications.
- Hardening Active Directory authentication by disabling NTLM where feasible and requiring Kerberos with pre-authentication.
- Enforcing the principle of least privilege on user accounts so finance analysts cannot access unrestricted administrative shares.
- Implementing host isolation procedures and disabling compromised user accounts immediately upon detection.
- Restricting outbound network traffic at the firewall so endpoints cannot initiate connections directly to unknown external IP addresses.

### Defensive Security
The project emphasized the perspective of an incident responder. Rather than focusing solely on exploitation, I focused on detection mechanisms, telemetry interpretation, and containment strategy. This included modeling why partial mitigations (such as blocking an external IP address while an active persistence mechanism remains on the endpoint) fail to stop an intrusion.

### Security Engineering
I translated theoretical cybersecurity concepts into working, structured code. This involved designing TypeScript data schemas to represent security events, raw logs, network nodes, and attack stages. It also involved managing application state to simulate dynamic investigation progress, evidence unlocking, and scoring based on response decisions.

## Connection to the INSA Summer Camp

Developing Incident served as a practical bridge between the security topics studied during the INSA Summer Camp and hands-on software development. While the project does not implement every topic covered during the camp, the security perspective used to design the simulation drew directly from several areas of study:

- Cisco Networking Academy: Network security concepts, including subnet design, IP addressing, perimeter firewall rules, and switch security, informed how the network topology and packet capture artifacts were structured.
- TryHackMe Exercises: Practical labs covering reconnaissance, Nmap scanning, offensive workflows, and defensive log analysis provided the foundational understanding needed to model realistic attack steps and forensic artifacts.
- Web Application Security: Concepts surrounding input handling, trust boundaries, and data flow tracking influenced how evidence verification and input validation were implemented.
- PHP Vulnerability and CVE Research: Studying runtime exploitation mechanisms reinforced the importance of understanding low-level execution flow, which informed the malware process tree simulation.
- OWASP Concepts: Security principles such as broken access control, security misconfigurations, and credential protection guided the identification of simulated system weaknesses and hardening recommendations.

## What I Learned

Building Incident gave me a clearer, more practical view of how security works in practice. Studying attacks in theory or following predefined lab steps is useful, but designing a simulated incident forced me to think through the details: what logs an action leaves behind, how different components connect, and why small misconfigurations can lead to a wider compromise.

It also helped me understand security from both sides. Seeing how an attacker moves from a phishing attachment to credential theft and lateral movement made it easier to understand why layered defense is necessary. A single control, like blocking an IP address, is rarely enough if the underlying system has not been properly isolated and hardened. Building the application helped connect these defensive principles into a concrete, cohesive workflow.

## Repository

Incident GitHub Repository:
https://github.com/Samuel-0228/Incident
