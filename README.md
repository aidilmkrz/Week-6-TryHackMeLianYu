# Week-6-TryHackMe: LianYu Walkthrough

A comprehensive walkthrough of the Lian_Yu challenge on TryHackMe. This lab covers network enumeration, directory brute-forcing, forensic file repair, and Linux privilege escalation.

---

## 🌐 Phase 0: Environment Setup

### 1. Establishing Connectivity
Before starting the engagement, I established a secure connection to the TryHackMe lab network using OpenVPN. This ensures that my Kali Linux machine can communicate with the target host.

![VPN Connection Established](image_a05d64.png)

### 2. Target Identification
Once connected, I deployed the **Lian_Yu** machine. This is a beginner-level security challenge themed around the "Arrow" TV series.

![Machine Deployment](image_a05d82.png)

---

## 🔍 Phase 1: Reconnaissance & Enumeration

### 1. Network Scanning
I started with an Nmap scan to identify open ports. This revealed Port 21 (FTP), Port 22 (SSH), and Port 80 (HTTP).

<img width="950" height="433" alt="Nmap Scan Results" src="https://github.com/user-attachments/assets/7143d840-fae1-49f7-bd86-6365a6614cdc" />

### 2. Directory Brute-forcing
Using Gobuster, I enumerated the web server to find hidden directories. This step revealed the primary paths used to find hidden images and credentials.

<img width="951" height="780" alt="Gobuster Discovery" src="https://github.com/user-attachments/assets/626c3703-0fd7-4952-a1d2-2d8cfca6538d" />

---

## 🛠️ Phase 2: Exploitation & Initial Access

### 1. Steganography and SSH
After extracting hidden data from the images found on the web server and repairing corrupted headers, I obtained the SSH credentials. Using the password `M3tahuman` and the username `slade`, I gained initial access to the system.

![SSH Login Successful](image_a210fd.png)

### 2. Capturing User Flag
Once inside, I located the user flag in the home directory.

![User Flag](image_a1be60.png)

---

## ⚡ Phase 3: Privilege Escalation (Root)

### 1. Sudo Rights & Pkexec
I checked the sudo permissions for the user `slade` and found that I could run `/usr/bin/pkexec` as root.

![Sudo Privileges](image_a1affc.png)

### 2. Final Objective: Root Flag
By exploiting the `pkexec` permission, I escalated my privileges to root and captured the final flag in the `/root` directory.

---

## 🏆 Conclusion
Machine 100% Completed! This lab was a great exercise in combining web enumeration with forensic analysis and Linux permission exploitation.
