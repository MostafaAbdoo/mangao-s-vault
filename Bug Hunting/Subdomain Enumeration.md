*there's a pdf for this topic*
important: https://hossamshady.medium.com/best-recon-methodology-b0e78c9dfd57

- https://Securitytrails.com
- https://Subdomainfinder.c99.nl
- https://shrewdeye.app/search
 You should use all the three sites and gather subdomains from them all then put these subdomains in file
crt.sh <--

# Subfinder
It's a tool in kali Linux
`subfinder –d mars.com -all --recursive`
You can use API with Subfinder to make it more powerful.
Open the file
`` ~/.config/subfinder/provider-config.yaml
You need to register and try to get APIs from sites
![[Pasted image 20250304175531.png]]
# assetfinder
It's also a tool in Linux
`echo "mars.com" | assetfinder --subs-only`
-------------------------------

`service apache2 start`
`cd /var/wwww/html`
You can edit the file called index.html
▪ And that you can open your site by entering
▪ localhost or 127.0.0.1
Where is the file that I can add hossamshady.com over 127.0.0.1
▪ `nano /etc/hosts`
▪ Then add line 127.0.0.1  hossamshady.com
That called virtual host, Virtual host is used inside companies to make subdomains but reachable only by clients inside the company not anyone else
We will fuzz the ip to know all subdomains hosted on it and then try to find that hidden subdomain
▪ We will use tool called `ffuf`
First we need list of subdomains to fuzz and guess from it
▪ You can download this wordlist from google
▪ Just search for (subdomain wordlist)
▪ Or you can download wordlist from
▪ `git clone https://github.com/danielmiessler/SecLists

`ffuf -u http://ffuf.me -w SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.ffuf.me"`
After doing that you will find large number of fake subdomains but let's filter them by -fs 1495 as that is the size of fake subdomains
After while you will find subdomain called redhat then the full subdomain is redhat.ffuf.me
Observe you can't get into the site because it is private for Company Clients. First you need to know the ip for the domain ffuf.me.
`ping ffuf.me`
Once you have the ip then go to /etc/hosts and put ip for redhat.ffuf.me and see
`nano /etc/hosts`
159.65.212.111 redhat.ffuf.me

Try to remove anything and leave only subdomains, Then collect all subdomains and put them in file. Then we need to remove duplicate subdomains
`cat file.txt | anew`
Once you used the tool and removed the duplicate then open new file and put the final result inside it and save
Then we can use `httpx` to see what subdomains are valid or not
`cat subs.txt | httpx`

# Directory and file Enumeration
There are many ways to get directories and files
- `dirb` for directory enumeration
`dirb https://example.com`
- `dirsearch` for file enumeration
`dirsearch -u https://example.com`
-  `ffuf` for fuzzing file and directories
`ffuf -u https://www.example.com/FUZZ -w wordlist/Seclists/Discovery/Web-content/raft-medium-files.txt -mc 200,302,301 -t 1000`

you can use other tools as `gobuster` , `meg` , etc.
# Parameter fuzzing and gathering
- `arjun`
`arjun -u https://www.example.com/file.php `
-  `paramspider`
`paramspider -l domains.txt -s`
-  `gospider`
`gospider -S domains.txt -o gospider`
- `burpsuite paraminer`
	after installing the extension
	go to request and right click >> extensions >> paraminer >> Guess params >> Guess everything
# Collecting all urls related to target
- `waybackurls`
`cat urls.txt | waybackurls`
- `Gau`
`cat urls.txt | gau`
- `Katana`
`katana -list urls.txt -v -jc -o katana`
- `hakrawler`
`cat urls.txt | hakrawler`