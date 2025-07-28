## Gobuster
used for **brute-forcing directories, files, DNS subdomains, and virtual hosts** on web servers.

**Directory and File Brute-Forcing (`dir` mode):**
- Scans for hidden directories and files on a web server by brute-forcing paths based on a wordlist.
- Useful for uncovering misconfigured or unprotected areas of a web application.
- `gobuster dir -u http://example.com -w /usr/share/wordlists/dirb/common.txt -x php,html,txt`
- `-x`: Specify file extensions to test (e.g., `.php`, `.html`).
**DNS Subdomain Enumeration (`dns` mode):**
- Brute-forces subdomains of a domain to find potential entry points for attacks.
- `gobuster dns -d example.com -w /usr/share/wordlists/dns/subdomains.txt`
**Virtual Host Enumeration (`vhost` mode):**
- Identifies virtual hosts on a target server by brute-forcing hostnames.
- `gobuster vhost -u http://example.com -w /usr/share/wordlists/vhosts.txt`
**S3 Bucket Enumeration (`s3` mode):**
- Identifies publicly accessible AWS S3 buckets.
- `gobuster s3 -b example`
---------
- `ssh -l <urname> <website> -p <port>`
- `nc <website> <port>`
- `strings` command in Linux is used to extract printable (human-readable) strings from binary files. > `strings strings | grep "pico"`

---
## Hydra
Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.
According to its [official repository](https://github.com/vanhauser-thc/thc-hydra), Hydra supports, i.e., has the ability to brute force the following protocols: “Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.”
### Hydra Commands
The options we pass into Hydra depend on which service (protocol) we’re attacking. For example, if we wanted to brute force FTP with the username being `user` and a password list being `passlist.txt`, we’d use the following command:

`hydra -l user -P passlist.txt ftp://10.10.155.86`
`hydra -l <username> -P <full path to pass> 10.10.155.86 -t 4 ssh` > ssh
`-l` > specifies the (SSH) username for login
`-P` > indicates a list of passwords
`-t` > sets the number of threads to spawn

> `hydra -l <username> -P <wordlist> 10.10.155.86 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V`
