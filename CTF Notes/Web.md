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
`ssh -l <urname> <website> -p <port>`
`nc <website> <port>`
