<img width="950" height="433" alt="ss1" src="https://github.com/user-attachments/assets/3f2cdc61-a28c-4010-b679-2b1818bb3284" />
# Week-6-TryHackMe: LianYu Walkthrough

A detailed walkthrough of the Lian_Yu CTF on TryHackMe. This lab involves forensic file repair, steganography, and Linux privilege escalation.

## 🛠️ Phase 1: Enumeration & Initial Access

### 1. Web Discovery & Steganography
After scanning the target, we discovered several hidden files. One particular image, `aa.jpg`, contained a hidden zip archive. We used `stegseek` with the `rockyou.txt` wordlist to crack the passphrase and extract the contents.

### 2. Identifying Credentials
The extracted files contained hints pointing toward the password **M3tahuman**. By applying "Arrow" show lore, we identified the username **slade**.

**Command:**
```bash
ssh slade@<target_ip>
