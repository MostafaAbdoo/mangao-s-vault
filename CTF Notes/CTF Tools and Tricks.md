**CTF Essentials Toolkit for TryHackMe (Beginner to Medium)**

---

Welcome! Here's the ultimate beginner-to-intermediate list of **essential tools, techniques, and tricks** you should learn to solve most **easy and medium CTFs** on **TryHackMe (THM)**.

This guide is written assuming you are **new** and want a full roadmap of **what to learn**, **why it's useful**, and **how it's used in practice**.

---

## ✨ Mindset First

Before we start with tools, remember:

- CTFs are **not about memorizing**, but about **recognizing patterns**.
    
- It’s OK to Google and learn on the fly.
    
- Get comfortable with breaking things, testing, and trying new commands.
    
- Don’t fear "getting stuck" — it's part of the game.
    

---

## ⚙️ TOOLS (Grouped by Category)

### ⚡ Enumeration Tools

These are your **go-to tools** when you first encounter a target.

#### 1. `nmap`

- Port scanning
    
- Version detection
    
- Script scanning
    
- Useful flags: `-sV -sC -A -p-`
    

#### 2. `gobuster` or `dirb` or `ffuf`

- Directory/file enumeration on websites
    
- Used for brute-forcing paths
    
- Trick: Always check `/robots.txt`, `/backup/`, `/dev/`, `/admin/`
    

#### 3. `whatweb` / `wappalyzer` / `BuiltWith`

- Detect web technologies (CMS, frameworks)
    

#### 4. `nikto`

- Web vulnerability scanner (not stealthy but useful in CTFs)
    

#### 5. `enum4linux` / `smbclient`

- SMB/Windows shares enumeration
    
- Can read open shares and files
    

#### 6. `ldapsearch`, `rpcclient`, `nmblookup`

- For Active Directory and domain enumeration
    

#### 7. `netcat (nc)`

- Network interaction (manual requests, setting up listeners)
    
- Trick: Also used for reverse shells!
    

---

### 🧰 Web Hacking Tools

#### 8. `burpsuite` (Community Edition is fine)

- Intercepting requests
    
- Repeating/modifying parameters
    
- Bruteforcing (via Intruder)
    

#### 9. `curl`

- Sending crafted HTTP requests
    
- Trick: Used to test for command injection, SSRF, and more
    

#### 10. `sqlmap`

- Automated SQL Injection tool
    
- Flags: `--dbs --tables --dump --batch`
    

#### 11. `wfuzz` / `ffuf`

- URL fuzzing (for parameters, hidden fields)
    

---

### 🚀 Exploitation and Post-Exploitation

#### 12. `searchsploit`

- Search Exploit-DB for known vulnerabilities
    
- Also helps you find local privilege escalation exploits
    

#### 13. `msfconsole` (Metasploit)

- Exploits, reverse shells, and post-exploitation tools
    
- Used often in THM boxes for known vulns
    

#### 14. `linpeas.sh` / `linenum.sh` / `Linux Smart Enumeration`

- Automated Linux privilege escalation scripts
    
- Always run them after getting a low-priv shell
    

#### 15. `winPEAS.exe` / `Seatbelt.exe`

- Windows post-exploitation / privilege escalation
    

#### 16. `john` / `hashcat`

- Password cracking tools
    
- Often used when you find `/etc/shadow`, hashes in DBs, etc.
    

#### 17. `Hydra`

- Bruteforce login (FTP, SSH, HTTP, etc.)
    
- Example: `hydra -l admin -P rockyou.txt ftp://target`
    

---

## 🪤 Knowledge-Based Tricks

These aren’t tools, they’re **conceptual tricks and habits** that often help in CTFs.

### 🔎 Basic Recon Habits

- Always check `/robots.txt`, `/sitemap.xml`, `/server-status`.
    
- View page source for comments or hidden fields.
    
- Try `http://IP:port` with and without `/`, trailing slashes can matter.
    

### ⚠ Login Page Tricks

- Default creds: `admin:admin`, `admin:password`, etc.
    
- Try SQLi: `' OR 1=1 -- -`
    
- Try bypasses: `admin'#`, `admin"--`, etc.
    

### 🧡 Hidden/Backup Files

- `index.php~`, `config.php.bak`, `.git`, `.env`, `.DS_Store`
    
- Tools like `dirbuster` may find them.
    

### 🔢 User Enumeration Tricks

- Check `/etc/passwd` from LFI or file disclosure
    
- Look for usernames in web pages or config files
    
- Try common names: `john`, `james`, `mike`, etc.
    

### 🔐 Password Reuse

- Users often reuse passwords between services (FTP/SSH/web login)
    
- Always try the same creds on multiple ports/services
    

### 🔢 Reverse Shell Tricks

- Have your payloads ready: PHP, Python, Bash, Netcat
    
- Know how to catch them with `nc -lvnp PORT`
    
- Try payloads from: [revshells.com](https://www.revshells.com)
    

---

## 🤍 Extra Tools to Explore Later

These aren't essential at first, but very useful as you go:

- `BloodHound` for Active Directory attack paths
    
- `Impacket` tools like `secretsdump.py`, `psexec.py`
    
- `crackmapexec` for lateral movement
    
- `docker` to run vulnerable services locally
    
- `gdb` / `pwndbg` for binary exploitation
    

---

## 📖 Final Tips

- Always read the **room description** and **task hints** on TryHackMe
    
- Take **notes** using CherryTree, Obsidian, or even a .md file
    
- Build a **cheat sheet** of useful commands and payloads
    
- Don’t skip writeups. Read both official and community writeups (but try first!)
    
- Practice: Easy rooms help you build intuition. Medium rooms train you to chain multiple vulnerabilities.
    

---

## 📊 Suggested Learning Path (Practical Labs)

### Start With These Rooms:

- Introduction to Researching
    
- Basic Pentesting
    
- Nmap
    
- Web Fundamentals
    
- Linux Privilege Escalation
    
- Windows Privilege Escalation
    
- OWASP Top 10
    
- TryHackMe’s "Pre-Security" or "Jr Penetration Tester" path
    

---

CTFs are as much about **learning how to learn** as they are about hacking.  
Stick with it, build your toolset, and you'll go from solving Easy boxes to Hard ones in no time.

Let me know when you’re ready for a personalized practice path or want challenges based on what you've learned.