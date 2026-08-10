# Active Directory & DHCP Home Lab

A self-built Windows Server home lab demonstrating core sysadmin/helpdesk skills: Active Directory Domain Services, DNS, DHCP, Organizational Units, Group Policy, and domain-joined client management — built entirely from scratch using VirtualBox.

## Why I built this

I wanted hands-on proof of the concepts covered in my Google IT Support Professional Certificate and IS&T coursework, so I built a working two-machine domain environment: a Windows Server domain controller and a domain-joined Windows client, then layered on DHCP, user/group management, and Group Policy enforcement.

## Network Diagram

![Network Diagram](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/f94d08f574c9e72b119d521d6cf9b9b6fa78411a/Project%20Diagram.drawio.png)

*Isolated virtual switch (`intnet`) connecting DC01 (Windows Server 2022, AD DS/DNS/DHCP) and PC01 (Windows 11, DHCP client), showing the domain join and applied GPO.*

## Environment

| Component | Details |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Domain Controller | Windows Server 2022 Standard (Evaluation) — `DC01` (192.168.10.10) |
| Client | Windows 11 Pro — `PC01` |
| Domain | `lab.local` |
| Network | Internal Network (`intnet`), `192.168.10.0/24` |

## Project 1: Active Directory Domain Controller Build

**Goal:** Stand up a functioning domain controller with AD DS + DNS, then join a client machine to the domain.

### VM Environment
VM Overview![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%209.png)
*DC01 and PC01 both running in VirtualBox, connected over an isolated internal network.*

### Domain Controller Configuration
DC Properties![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%202.png)
*DC01 running Windows Server 2022 Standard Evaluation with a static IP (192.168.10.10) and the domain `lab.local` established.*

### Promoting to Domain Controller
AD DS Prerequisites Check![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%201.png)
*Active Directory Domain Services Configuration Wizard — prerequisites check passed before promoting DC01 to a domain controller and creating the `lab.local` forest.*

### Verifying Active Directory
ADUC Domain View![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%204.png)
*Active Directory Users and Computers confirming the lab.local domain is live, with default containers (Builtin, Computers, Domain Controllers, Users).*

DNS Manager![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%203.png)
*DNS Manager showing the forward lookup zones for lab.local and the _msdcs.lab.local subdomain, both running as Active Directory-integrated zones.*

### Client Configuration & Domain Join
Client IP Config![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%208.png)
*PC01's `ipconfig /all` output confirming a static IP (192.168.10.20) with DNS pointed to the domain controller (192.168.10.10) — required for the domain join to succeed.*

Connectivity Test![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%205.png)
*Verifying connectivity from PC01 with `ping` and `nslookup` against the domain controller — the nslookup query timed out briefly on the first attempt but still resolved lab.local correctly before proceeding with the domain join.*
Domain Join Confirmation![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%207.png)
*Settings → System → About on PC01 showing the full device name as `PC01.lab.local`, confirming a successful domain join.*

Domain Login![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%206.png)
*PC01 login screen post domain-join, ready to authenticate with a domain account.*

## Project 2: DHCP, Organizational Units & Group Policy

**Goal:** Extend the environment with centralized IP management and real-world AD administration tasks.

### DHCP
Configured a DHCP scope on DC01 (192.168.10.100–192.168.10.200) with DNS pointed to the domain controller, then switched PC01 to obtain its IP automatically via DHCP.

### Organizational Units, Users & Groups
Created OUs for Users and Computers in ADUC, and added a test user account (`jdoe`) to validate Group Policy targeting and login functionality.

### Group Policy Enforcement
GPO Update Success![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%2010.png)
*Applying the updated Group Policy on PC01 as user `jdoe` with `gpupdate /force` — both computer and user policy updates completed successfully.*

GPO Enforced — Control Panel Blocked![image alt](https://github.com/Aryaan314/Active-Directory-Home-Lab/blob/658104a8afcaeb27547917787d5e89990bc9698c/PJ%20Image%2011.png)
*Confirming the GPO took effect: Control Panel access is blocked for `jdoe`, showing the "Restrictions" dialog when attempting to open a restricted operation.*

## Skills Demonstrated

- Windows Server installation & configuration
- Active Directory Domain Services (AD DS) design and deployment
- DNS zone management
- DHCP scope configuration
- Organizational Unit structure & user/group administration
- Group Policy creation and enforcement
- Client-side domain join troubleshooting (static IP, DNS resolution, connectivity testing)
- Network diagramming (draw.io)

## Common Issues I Ran Into

- On the first `nslookup lab.local` attempt, the request timed out after 2 seconds — but it ultimately succeeded and returned the correct DNS record (192.168.10.10). This was a good reminder that a slow/delayed response on the first query isn't necessarily a misconfiguration, just normal DNS resolution lag in a lab environment.
- Confirmed GPO propagation with `gpupdate /force` rather than waiting for the default refresh interval, which sped up testing.

## Tools Used

- Oracle VirtualBox
- Windows Server 2022 (Evaluation)
- Windows 11 Pro
- draw.io (network diagram)

---

*Built by Aryaan Sheikh — [LinkedIn](https://www.linkedin.com/in/aryaan-sheikh-74b29120b/) · [Resume](Aryaan%20Sheikh's%20Resume.docx.pdf)*
