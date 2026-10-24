# Metasploitable 2 — Vulnerability Assessment & Penetration Testing

### IqraSec Academy | Phase 2 — WAPT Internship

**Author:** Kinza Noor  
**Project:** Vulnerable Machine Deployment & Practical Penetration Testing  
**Target:** Metasploitable 2  
**Environment:** Kali Linux | Oracle VirtualBox  
**Assessment Type:** Authorized, Isolated Laboratory Penetration Testing

---

## Project Overview

This project documents a practical Vulnerability Assessment and Penetration Testing (VAPT) exercise conducted as part of the IqraSec Academy Phase 2 internship.

The objective was to deploy an intentionally vulnerable machine, identify exposed services, investigate security weaknesses, perform controlled exploitation, and provide practical remediation recommendations.

All testing was conducted against Metasploitable 2 in an authorized virtual laboratory environment.

## Lab Environment

| Component | Configuration |
|---|---|
| Attacking Machine | Kali Linux |
| Target Machine | Metasploitable 2 |
| Virtualization | Oracle VirtualBox |
| Network | Host-Only Laboratory Network |
| Attacker IP | 192.168.56.103 |
| Target IP | 192.168.56.105 |
| Tools | Nmap, Metasploit Framework, Netcat, Linux CLI |

## Assessment Methodology

The assessment followed these phases:

1. **Lab Deployment:** Configured Kali Linux and Metasploitable 2 in a virtualized environment.
2. **Reconnaissance:** Identified the target and verified network connectivity.
3. **Scanning and Enumeration:** Identified open ports, running services, and potential attack surfaces.
4. **Vulnerability Analysis:** Investigated vulnerable services and insecure configurations.
5. **Exploitation:** Performed controlled exploitation against the authorized training target.
6. **Post-Exploitation Verification:** Validated shell access and privileges through commands such as `whoami`, `id`, and `hostname`.
7. **Reporting:** Documented technical findings, security impact, and remediation recommendations.

## Tools and Technologies

- **Nmap:** Host discovery, port scanning, and service version detection.
- **Metasploit Framework:** Vulnerability investigation and controlled exploitation.
- **Netcat:** Manual service interaction and SMTP testing.
- **Linux CLI:** System verification and post-exploitation checks.
- **NFS Utilities:** RPC/NFS enumeration and filesystem mount verification.
- **Oracle VirtualBox:** Virtual machine deployment and isolation.

## Vulnerability Assessment Findings

| ID | Vulnerability | Severity | Result |
|---|---|---|---|
| F-01 | vsFTPd 2.3.4 Backdoor | Critical | Successful root-level shell |
| F-02 | SSH Default Credentials | High | Successful SSH authentication |
| F-03 | Insecure NFS Root Export | Critical — configuration risk | Root filesystem mounted read-only |
| F-04 | SMTP Username Enumeration | Low | User enumeration behavior demonstrated |
| F-05 | PHP-CGI Argument Injection | High | Shell obtained as `www-data` |

*Severity ratings are qualitative assessments within the laboratory context.*

## Exploitation and Verification

### 1. vsFTPd 2.3.4 Backdoor

The FTP service running on port 21 was identified as vulnerable to a known backdoor.

The Metasploit `vsftpd_234_backdoor` module was used to exploit the target successfully.

**Result:** Root-level command execution was confirmed.

```text
whoami
root

id
uid=0(root) gid=0(root)

hostname
metasploitable
```

### 2. SSH Default Credentials

The SSH service on port 22 was assessed using Metasploit's `ssh_login` auxiliary module.

Authentication succeeded with the documented default Metasploitable 2 credentials.

**Result:** An authenticated SSH session was established as `msfadmin`.

### 3. Insecure NFS Configuration

RPC and NFS enumeration revealed that the target exported its entire root filesystem.

The configuration included:

```text
/ *(rw,sync,no_root_squash,no_subtree_check)
```

The filesystem was successfully mounted from Kali Linux.

**Result:** Unauthorized root filesystem exposure was demonstrated. The client mount was read-only, and remote write exploitation was not performed.

### 4. SMTP Username Enumeration

The Postfix SMTP service on port 25 permitted username verification requests through the `VRFY` command.

Different responses were observed for probable user accounts and an invalid username.

**Result:** Potential username information disclosure was demonstrated. No SMTP-based shell access was established.

### 5. HTTP and PHP-CGI Exploitation

The HTTP service exposed Apache httpd 2.2.8 on port 80.

The `php_cgi_arg_injection` Metasploit module was tested. Following an unsuccessful initial attempt, the reverse handler configuration was corrected.

**Reported result:** A Meterpreter session was established with `www-data` privileges (UID 33). The full report contains the supporting testing context; this result should be considered confirmed only alongside the corresponding module and session evidence.

## Risk Assessment and Remediation

| Finding | Recommended Remediation |
|---|---|
| vsFTPd Backdoor | Remove the compromised service and install a trusted, maintained replacement |
| SSH Default Credentials | Replace default passwords and restrict administrative access |
| Insecure NFS Export | Remove unrestricted root exports and enable `root_squash` |
| SMTP Enumeration | Restrict unnecessary `VRFY` functionality and monitor probing |
| PHP-CGI Injection | Upgrade vulnerable PHP components and disable unnecessary CGI functionality |

## Project Outcome

The assessment successfully met its principal objective of compromising an intentionally vulnerable target in a controlled laboratory environment.

The exercise demonstrated practical skills in reconnaissance, service enumeration, vulnerability investigation, controlled exploitation, privilege verification, and security reporting.

The findings also highlight the importance of secure service configuration, strong authentication, patch management, least privilege, and network isolation.

## Report and Evidence

The full VAPT report contains the testing methodology, screenshots, vulnerability findings, exploitation evidence, risk ratings, and remediation recommendations.

**Full Report:** [View VAPT Report](reports/IqraSec_Phase2_VAPT_Report.pdf)

## Ethical Disclaimer

This project was conducted exclusively within an authorized, isolated cybersecurity training laboratory.

Metasploitable 2 is intentionally vulnerable and must not be deployed on public or production networks.

The techniques documented in this repository are intended for educational purposes and authorized security testing only.

---

**IqraSec Academy — Phase 2 | Vulnerability Assessment & Penetration Testing**
