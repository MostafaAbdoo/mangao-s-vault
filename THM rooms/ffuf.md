SecLists is a collection of multiple types of lists used during security assessments. List types include usernames, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more. 
SecLists is already included in the following Linux distributions:

- BlackArch
- Pentoo
- Kali
- Parrot
- Search [Repology](https://repology.org/project/seclists/versions) for other distributions

If it's not included in your Linux distribution you can deploy it manually following [the installation instructions](https://github.com/danielmiessler/SecLists#install).

# Basics
The Help page can be displayed using `ffuf -h` and it will be useful as we will use a lot of options.
At a minimum we're required to supply two options: `-u` to specify an URL and `-w` to specify a wordlist. The default keyword `FUZZ` is used to tell ffuf where the wordlist entries will be injected. We can append it to the end of the URL like so:
`ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt`
`-mc 200` — to Match HTTP status code 200 only.

# Finding pages and directories
One approach you could take would be to start enumerating with a generic list of files such as raft-medium-files-lowercase.txt.

`ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt`

However, using a large generic wordlist containing irrelevant file extensions is not very efficient.

Instead, we can usually assume `index.<extension>` is the default page on most websites so we can try common extensions for just the index page. With this method, we can usually determine what programming language or languages the site uses.

For example, we can append the extension after index.

`head /usr/share/seclists/Discovery/Web-Content/web-extensions.txt 
`.asp   .aspx   .bat   .c   .cfm   .cgi   .css   .com   .dll   .exe`  


`ffuf -u http://MACHINE_IP/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt`

Now that we know the extensions supported we can try a list of generic words (without of extension) and apply the extensions we know works (found from Q2) + some common ones like `.txt`.

We'll exclude the 4 letter extensions from this wordlist as it will result in many false positives.

`ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt`  

Directory names are not always dependent on the type of environment you're enumerating and is often a good starting point before attempting to fuzz for files. If we wanted to fuzz directories we only need to provide a wordlist.

`ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`

# Using filters
By adding `-fc 403` (filter code) we'll hide from the output all 403 [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).
you could use `-mc 200` (match code) instead of having a long list of filtered codes.
we can use `-fs 0` (filter size).
We often see there are false positives with files beginning with a dot (eg. `.htgroups`, `.php`, etc.). They throw a 403 Forbidden error, however those files don't actually exist. It's tempting to use `-fc 403` but this could hide valuable files we don't have access to yet. So instead we can use a regexp to match all files beginning with a dot.

# Fuzzing parameters
Discovering a vulnerable parameter could lead to file inclusion, path disclosure, XSS, SQL injection, or even command injection. Since ffuf allows you to put the keyword anywhere we can use it to fuzz for parameters.
`$ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39   $ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -fw 39   `

Now that we found a parameter accepting integer values we'll start fuzzing values.

At this point, we could generate a wordlist and save a file containing integers. To cut out a step we can use `-w -` which tells ffuf to read a wordlist from [stdout](https://www.gnu.org/software/libc/manual/html_node/Standard-Streams.html). This will allow us to generate a list of integers with a command of our choice then pipe the output to ffuf. Below is a list of 5 different ways to generate numbers 0 - 255.

`$ ruby -e '(0..255).each{|i| puts i}' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33   $ ruby -e 'puts (0..255).to_a' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33   $ for i in {0..255}; do echo $i; done | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33   $ seq 0 255 | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33   $ cook '[0-255]' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33`  
  
We can also use ffuf for wordlist-based brute-force attacks, for example, trying passwords on an authentication page.
  
﻿`$ ffuf -u http://MACHINE_IP/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded'`

Here we have to use the POST method (specified with `-X`) and to give the POST data (with `-d`) where we include the `FUZZ` keyword in place of the password.

We also have to specify a custom header `-H 'Content-Type: application/x-www-form-urlencoded'` because ffuf doesn't set this content-type header automatically as curl does.

# Finding vhosts and subdomains
ffuf may not be as efficient as specialized tools when it comes to subdomain enumeration but it's possible to do.

`$ ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`

Some subdomains might not be resolvable by the DNS server you're using and are only resolvable from within the target's local network by their private DNS servers. So some virtual hosts (vhosts) may exist with private subdomains so the previous command doesn't find them. To try finding private subdomains we'll have to use the Host HTTP header as these requests might be accepted by the web server.  
**Note**: [virtual hosts](https://httpd.apache.org/docs/2.4/en/vhosts/examples.html) (vhosts) is the name used by Apache httpd but for Nginx the right term is [Server Blocks](https://www.nginx.com/resources/wiki/start/topics/examples/server_blocks/).  

You could compare the results obtained with direct subdomain enumeration and with vhost enumeration:

`$ ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs 0   $ ffuf -u http://mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.mydomain.com' -fs 0`  

For example, it is possible that you can't find a sub-domain with direct subdomain enumeration (1st command) but that you can find it with vhost enumeration (2nd command).

Vhost enumeration technique shouldn't be discounted as it may lead to discovering content that wasn't meant to be accessed externally.

# Proxifying ffuf traffic
Whether it's for [network pivoting](https://blog.raw.pm/en/state-of-the-art-of-network-pivoting-in-2019/) or for using Burp Suite plugins you can send all the ffuf traffic through a web proxy (HTTP or SOCKS5).  

`$ ffuf -u http://MACHINE_IP/FUZZ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080`

It's also possible to send only matches to your proxy for replaying:

`$ ffuf -u http://MACHINE_IP/FUZZ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080`

This may be useful if you don't need all the traffic to traverse an upstream proxy and want to minimize resource usage or to avoid polluting your proxy history.

----
As you start to use ffuf more, some options will prove to be very useful depending on your situation. For example, `-ic` allows you to ignore comments in wordlists that such as headers, copyright notes, comments, etc
We've only reviewed a small subset of the useful features and options ffuf has to offer for fuzzing.  
Use ffuf -h to discover the other options that might be useful for you and to answer the remaining questions in this task.
