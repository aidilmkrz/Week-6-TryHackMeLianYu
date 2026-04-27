# Week-6-TryHackMe: LianYu Walkthrough

## 🌐 Phase 0: Environment Setup

The first step in any penetration test is ensuring a secure and stable connection to the target network. 

### 1. Connecting to the Lab (OpenVPN)
I used the **OpenVPN** protocol to establish a tunnel between my local attack machine and the TryHackMe laboratory infrastructure. This allows me to access the private IP addresses assigned to the vulnerable hosts.

---<img width="951" height="600" alt="ss2" src="https://github.com/user-attachments/assets/4929270d-8949-4eda-a688-827ad34475e5" />

---<img width="950" height="433" alt="ss1" src="https://github.com/user-attachments/assets/9c8fb292-aa74-4f2b-93e7-5b943dc60ffc" />

**Technical Details:**
* **VPN Server:** Europe (Ireland)
* **Status:** Connected
* **Virtual IP Address:** 10.10.x.x (As shown in the internal virtual IP field)

## 🔍 Phase 1: Reconnaissance & Enumeration
<img width="641" height="505" alt="ss3" src="https://github.com/user-attachments/assets/3fac0cbd-b22f-41b0-b5ef-6e7ddd67cc87" />
<img width="643" height="506" alt="ss4" src="https://github.com/user-attachments/assets/4045e9d2-3c9b-4307-91c3-81ba64b0e1dd" />

### 1. Network Scanning (Nmap)
With the connection established, the first step was to identify the open ports and services running on the target machine. I performed an aggressive service detection scan to map out the attack surface.

**Command:**
```bash
nmap -sV -sC -A 10.80.133.131
```

### 2. Web Directory Enumeration (Gobuster)
<img width="627" height="378" alt="ss5" src="https://github.com/user-attachments/assets/d2608f75-0a62-4b75-a894-46b88fc78428" />
With Port 80 open, I used **Gobuster** to hunt for hidden directories. I chose the `directory-list-2.3-medium.txt` wordlist to ensure a thorough scan of the web server.

**Command:**
```bash
gobuster dir -u [http://10.80.133.131](http://10.80.133.131) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt






