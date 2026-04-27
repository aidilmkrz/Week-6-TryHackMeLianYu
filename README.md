# Week-6-TryHackMe: LianYu Walkthrough

## 🌐 Phase 0: Environment Setup

The first step in any penetration test is ensuring a secure and stable connection to the target network. 

### 1. Connecting to the Lab (OpenVPN)
I used the **OpenVPN** protocol to establish a tunnel between my local attack machine and the TryHackMe laboratory infrastructure. This allows me to access the private IP addresses assigned to the vulnerable hosts.

---<img width="951" height="780" alt="ss2" src="https://github.com/user-attachments/assets/4929270d-8949-4eda-a688-827ad34475e5" />

---<img width="950" height="433" alt="ss1" src="https://github.com/user-attachments/assets/9c8fb292-aa74-4f2b-93e7-5b943dc60ffc" />

**Technical Details:**
* **VPN Server:** Europe (Ireland)
* **Status:** Connected
* **Virtual IP Address:** 10.10.x.x (As shown in the internal virtual IP field)

## 🔍 Phase 1: Reconnaissance & Enumeration

### 1. Network Scanning (Nmap)
With the connection established, the first step was to identify the open ports and services running on the target machine. I performed an aggressive service detection scan to map out the attack surface.

**Command:**
```bash
nmap -sV -sC -A 10.80.133.131
<img width="641" height="505" alt="ss3" src="https://github.com/user-attachments/assets/02886a1f-6f42-47c9-91f3-6cb7a7a725ad" />

