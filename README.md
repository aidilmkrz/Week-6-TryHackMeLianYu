<img width="620" height="323" alt="ss9" src="https://github.com/user-attachments/assets/b67e1c22-ea89-4818-a658-986a3d6df829" /><img width="620" height="323" alt="ss9" src="https://github.com/user-attachments/assets/76cb5ab7-283d-4592-a4a7-5e51119b5669" /># Week-6-TryHackMe: LianYu Walkthrough

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
```
### 3. Web Content Analysis
After discovering the `/island` directory, I navigated to the page in the browser. The page appeared to be a dead end at first glance, containing only a cryptic message.

<img width="908" height="517" alt="ss6" src="https://github.com/user-attachments/assets/15ce54b5-7bf3-48e3-97fe-3989f0334bff" />


### 4. Inspecting the Source Code
To find hidden clues, I viewed the page source (`Ctrl + U`). This revealed a hidden "Code Word" that was styled to be invisible on the main page (white text on a white background).

<img width="1002" height="502" alt="ss7" src="https://github.com/user-attachments/assets/b36bdd97-b148-4451-8354-a632e9674213" />

**Key Discovery:**
* **Code Word:** `vigilante`
* **Comment Hint:** Found an HTML comment `` suggesting further action was required.

---

## 🛠️ Phase 2: Exploitation & Initial Access

### 1. Utilizing the Code Word
The code word `vigilante` served as a directory name, leading to another hidden layer of the web server where I discovered further forensic challenges.

<img width="620" height="323" alt="ss10" src="https://github.com/user-attachments/assets/185612f8-2b64-4eba-b179-161227ec0f78" />
### 2. Recursive Enumeration (/island/2100)
After discovering the keyword, I performed a targeted scan on the `/island` directory. This revealed a sub-directory named `2100`.

**Command:**
```bash
gobuster dir -u [http://10.80.133.131/island/](http://10.80.133.131/island/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt







