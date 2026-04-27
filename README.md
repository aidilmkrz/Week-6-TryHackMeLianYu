# Week-6-TryHackMe: LianYu Walkthrough

## 🌐 Phase 0: Environment Setup

Before launching the attack, it is essential to establish a secure connection to the laboratory environment and identify the target parameters.

### 1. Establishing Connectivity (OpenVPN)
To interact with the target machine, I utilized **OpenVPN** to connect to the TryHackMe internal network. This creates a secure tunnel between my Kali Linux attack box and the laboratory infrastructure.

![VPN Connection Status](ss1.png)
> **Note:** Confirmed "Connected" status to the Europe (Ireland) server, assigning my machine a virtual IP within the lab subnet.

### 2. Target Identification
Once the network tunnel was established, I deployed the **Lian_Yu** machine. Lian_Yu is a beginner-level security challenge themed around the "Arrow" TV series. 

![Machine Deployment](image_a04b97.png)
* **Target IP Address:** `10.10.x.x` (Dynamic based on deployment)
* **Difficulty:** Beginner
* **Objective:** Capture the User and Root flags.

![Sudo Privileges](image_a1affc.png)

### 2. Final Objective: Root Flag
By exploiting the `pkexec` permission, I escalated my privileges to root and captured the final flag in the `/root` directory.

---

## 🏆 Conclusion
Machine 100% Completed! This lab was a great exercise in combining web enumeration with forensic analysis and Linux permission exploitation.
