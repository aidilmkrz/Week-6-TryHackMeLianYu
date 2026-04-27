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


### 2. Recursive Enumeration (/island/2100)
<img width="620" height="323" alt="ss10" src="https://github.com/user-attachments/assets/185612f8-2b64-4eba-b179-161227ec0f78" />
After discovering the keyword, I performed a targeted scan on the `/island` directory. This revealed a sub-directory named `2100`.

**Command:**
```bash
gobuster dir -u [http://10.80.133.131/island/](http://10.80.133.131/island/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

### 3. Analyzing the /2100 Directory
Upon navigating to `http://10.80.133.131/island/2100/`, I was met with a page titled **"How Oliver Queen finds his way to Lian_Yu?"** featuring a broken YouTube video link.

<img width="857" height="750" alt="ss12" src="https://github.com/user-attachments/assets/78c5377d-3064-4371-a26c-e792c037b64d" />

**Observation:**
The video being unavailable is a clear indicator that the solution is not in the video itself, but likely hidden in the directory's metadata or background files.

### 4. Extracting the ".ticket" Clue
Upon inspecting the source code of the `/island/2100/` page (`Ctrl + U`), I discovered a hidden HTML comment that revealed the next step of the challenge.

<img width="723" height="377" alt="ss13" src="https://github.com/user-attachments/assets/a190c3bd-5fbf-43a0-8b1c-87eb078185c3" />

**Key Discovery:**
* **Hidden Comment:** ``
* **Analysis:** This comment explicitly mentions a `.ticket` file. In a directory enumeration context, this suggests that there is a file named `main.ticket`, `island.ticket`, or similar, which likely contains credentials or further instructions.

### 5. Finding the Ticket File
Following the hint found in the source code, I ran a targeted Gobuster scan on the `/island/2100/` directory, specifically searching for files with the `.ticket` extension.

<img width="632" height="347" alt="ss14" src="https://github.com/user-attachments/assets/3f8ad10c-9afe-4ad0-a00e-829f7dc86923" />

**Command:**
```bash
gobuster dir -u [http://10.80.133.131/island/2100/](http://10.80.133.131/island/2100/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket
```

### 6. Recovering FTP Credentials
By accessing `http://10.80.133.131/island/2100/green_arrow.ticket` in the browser, I successfully retrieved the "token" required for the next stage of the mission.

<img width="770" height="303" alt="ss15" src="https://github.com/user-attachments/assets/b8b79839-32bb-498a-a11c-752a416eb76b" />

**Credential Leak:**
* **Username:** `vigilante` (Found previously in the `/island` source code)
* **Token/Password:** `RTy8yhBQdscX`<img width="626" height="436" alt="ss18" src="https://github.com/user-attachments/assets/d399d5d0-5bec-44bc-991f-a81c44fe7026" />


* **Context:** The file mentions this is a "token to get into Queen's Gambit(Ship)," which hints at using these credentials for a service login.

### 7. Decoding the FTP Password
The token found in the `.ticket` file (`RTy8yhBQdscX`) appeared to be encoded. I used **CyberChef** to test various encoding schemes and determined it was **Base58**. 

!<img width="972" height="785" alt="ss16" src="https://github.com/user-attachments/assets/95a682c9-ca06-4290-8e65-2b72a96562a1" />


**Decryption Results:**
* **Encoded String:** `RTy8yhBQdscX`
* **Decoding Method:** Base58
* **Cleartext Password:** `!#th3h00d`

### 8. Successful FTP Login
With the decoded password `!#th3h00d`, I logged into the FTP service (vsFTPd 3.0.2) identified in the initial reconnaissance.

<img width="626" height="436" alt="ss17" src="https://github.com/user-attachments/assets/63df8369-61a7-45ba-96f1-ecda0d2eed96" />

**Command:**
```bash
ftp 10.80.133.131
```

### 9. Data Exfiltration via FTP
Once logged in as `vigilante`, I performed a directory listing and identified three image files that appeared to be targets for steganographic analysis. I switched the FTP session to **binary mode** to ensure file integrity during the download process.

<img width="637" height="506" alt="ss20" src="https://github.com/user-attachments/assets/3ea4388c-31a3-43fe-a358-baf2dbd1738d" />
<img width="535" height="160" alt="ss22" src="https://github.com/user-attachments/assets/4fa57d51-1cbe-442a-9b07-63a10e8862bb" />

**Command:**
```bash
ftp> binary
ftp> get Leave_me_alone.png
ftp> get Queen's_Gambit.png
ftp> get aa.jpg
```

### 10. Discovery of Additional User Data
During the FTP session, I identified a hidden file named `.other_user`. I downloaded this file along with the image assets for further review.

<img width="632" height="320" alt="ss24" src="https://github.com/user-attachments/assets/dcd22e1f-2e0b-4e3b-a80f-38a540f65a8e" />
<img width="636" height="431" alt="ss25" src="https://github.com/user-attachments/assets/26930a1c-19fc-4236-a9d3-30823f318517" />

**Command:**
```bash
ftp> get .other_user
```










