>> only the things that i am afraid of forgetting about or didn't know about at all

![[Pasted image 20250727093611.png]]
**Scheme**

The **scheme** is the protocol used to access the website. The most common are **HTTP** (Hypertext Transfer Protocol) and **HTTPS** (Hypertext Transfer Protocol Secure). HTTPS is more secure because it encrypts the connection, which is why browsers and cyber security experts recommend it. Websites often enforce HTTPS for added protection.

**User**

Some URLs can include a user’s login details (usually a username) for sites that require authentication. This happens mostly in URLs that need credentials to access certain resources. However, it’s rare nowadays because putting login details in the URL isn’t very safe—it can expose sensitive information, which is a security risk.

**Host/Domain**

The **host** or **domain** is the most important part of the URL because it tells you which website you’re accessing. Every domain name has to be unique and is registered through domain registrars. From a security standpoint, look for domain names that appear almost like real ones but have small differences (this is called **typosquatting**). These fake domains are often used in phishing attacks to trick people into giving up sensitive info.

**Port**

The **port number** helps direct your browser to the right service on the web server. It’s like telling the server which doorway to use for communication. Port numbers range from 1 to 65,535, but the most common are **80** for HTTP and **443** for HTTPS.

**Path**

The **path** points to the specific file or page on the server that you’re trying to access. It’s like a roadmap that shows the browser where to go. Websites need to secure these paths to make sure only authorized users can access sensitive resources.

**Query String**

The **query string** is the part of the URL that starts with a question mark (?). It’s often used for things like search terms or form inputs. Since users can modify these query strings, it’s important to handle them securely to prevent attacks like **injections**, where malicious code could be added.

**Fragment**

The **fragment** starts with a hash symbol (#) and helps point to a specific section of a webpage—like jumping directly to a particular heading or table. Users can modify this too, so like with query strings, it’s important to check and clean up any data here to avoid issues like injection attacks.

---
**Empty Line**

The empty line is a little divider that separates the header from the body. It’s essential because it shows where the headers stop and where the actual content of the message begins. Without this empty line, the message might get messed up, and the client or server could misinterpret it, causing errors.

---
### HTTP Methods

The **HTTP method** tells the server what action the user wants to perform on the resource identified by the URL path. Here are some of the most common methods and their possible security issue:

**GET  
Used to fetch** data from the server without making any changes. Reminder! Make sure you’re only exposing data the user is allowed to see. Avoid putting sensitive info like tokens or passwords in GET requests since they can show up as plaintext.

**POST  
***Sends** data to the server, usually to create or update something. Reminder! Always validate and clean the input to avoid attacks like SQL injection or XSS.

**PUT  
Replaces or updates** something on the server. Reminder! Make sure the user is authorized to make changes before accepting the request.

**DELETE  
***Removes** something from the server. Reminder! Just like with PUT, make sure only authorized users can delete resources.

Besides these common methods, there are a few others used in specific cases:

**PATCH  
Updates part of a resource. It’s useful for making small changes without replacing the whole thing, but always validate the data to avoid inconsistencies.

**HEAD  
Works like GET but only retrieves headers, not the full content. It’s handy for checking metadata without downloading the full response.

**OPTIONS  
Tells you what methods are available for a specific resource, helping clients understand what they can do with the server.

**TRACE  
Similar to OPTIONS, it shows which methods are allowed, often for debugging. Many servers disable it for security reasons.

**CONNECT  
Used to create a secure connection, like for HTTPS. It’s not as common but is critical for encrypted communication

---
- **URL Encoded (application/x-www-form-urlencoded)**  
A format where data is structured in pairs of key and value where (`key=value`). Multiple pairs are separated by an (`&`) symbol, such as `key1=value1&key2=value2`. Special characters are percent-encoded.    
**_Example_**  
```http
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```

- **Form Data (multipart/form-data)**  
Allows multiple data blocks to be sent where each block is separated by a boundary string. The boundary string is the defined header of the request itself. This type of formatting can be used to send binary data, such as when uploading files or images to a web server.
```http
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

- **JSON (application/json)**  
In this format, the data can be sent using the JSON (JavaScript Object Notation) structure. Data is formatted in pairs of name : value. Multiple pairs are separated by commas, all contained within curly braces { }.
```http
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

- **XML (application/xml)**  
In XML formatting, data is structured inside labels called tags, which have an opening and closing. These labels can be nested within each other. You can see in the example below the opening and closing of the tags to send details about a user called Aleksandra.
```http
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

---
### Status Codes and Reason Phrases

The **Status Code** is the number that tells you if the request succeeded or failed, while the **Reason Phrase** explains what happened. These codes fall into five main categories:

**Informational Responses (100-199)**  
These codes mean the server has received part of the request and is waiting for the rest. It’s a "keep going" signal.

**Successful Responses (200-299)**  
These codes mean everything worked as expected. The server processed the request and sent back the requested data.

**Redirection Messages (300-399)**  
These codes tell you that the resource you requested has moved to a different location, usually providing the new URL.

**Client Error Responses (400-499)**  
These codes indicate a problem with the request. Maybe the URL is wrong, or you’re missing some required info, like authentication.

**Server Error Responses (500-599)**  
These codes mean the server encountered an error while trying to fulfil the request. These are usually server-side issues and not the client’s fault.

### Common Status Codes

Here are some of the most frequently seen status codes:

**100 (Continue)**  
The server got the first part of the request and is ready for the rest.

**200 (OK)**  
The request was successful, and the server is sending back the requested resource.

**301 (Moved Permanently)**  
The resource you’re requesting has been permanently moved to a new URL. Use the new URL from now on.

**404 (Not Found)**  
The server couldn’t find the resource at the given URL. Double-check that you’ve got the right address.

**500 (Internal Server Error)**  
Something went wrong on the server’s end, and it couldn’t process your request.

### Status Line

The first line in every HTTP response is called the **Status Line**. It gives you three key pieces of info:

1. **HTTP Version**: This tells you which version of HTTP is being used.
2. **Status Code**: A three-digit number showing the outcome of your request.
3. **Reason Phrase**: A short message explaining the status code in human-readable terms.

---
- To keep things secure, make sure cookies are set with the `HttpOnly` flag (so they can’t be accessed by JavaScript) and the `Secure` flag (so they’re only sent over HTTPS).
### Content-Security-Policy (CSP)

A CSP header is an additional security layer that can help mitigate against common attacks like Cross-Site Scripting (XSS). Malicious code could be hosted on a separate website or domain and injected into the vulnerable website. A CSP provides a way for administrators to say what domains or sources are considered safe and provides a layer of mitigation to such attacks.

Within the header itself, you may see properties such as `default-src` or `script-src` defined and many more. Each of these give an option to an administrator to define at various levels of granularity, what domains are allowed for what type of content. The use of self is a special keyword that reflects the same domain on which the website is hosted.

Looking at an example CSP header:

`Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'`

We see the use of:

- **default-src**  
    - which specifies the default policy of self, which means only the current website.  
      
    
- **script-src**  
    - which specifics the policy for where scripts can be loaded from, which is self along with scripts hosted on `https://cdn.tryhackme.com`  
      
    
- **style-src**  
    - which specifies the policy for where style CSS style sheets can be loaded from the current website (self)  
    

  

### Strict-Transport-Security (HSTS)

The HSTS header ensures that web browsers will always connect over HTTPS. Let's look at an example of HSTS:

`Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`

  
Here’s a breakdown of the example HSTS header by directive:  

- **max-age**  
    - This is the expiry time in seconds for this setting  
      
    
- **includeSubDomains**  
    - An optional setting that instructs the browser to also apply this setting to all subdomains.  
      
    
- **preload  
    **- This optional setting allows the website to be included in preload lists. Browsers can use preload lists to enforce HSTS before even having their first visit to a website.

### X-Content-Type-Options

The X-Content-Type-Options header can be used to instruct browsers not to guess the MIME time of a resource but only use the Content-Type header. Here’s an example:

`X-Content-Type-Options: nosniff`

Here’s a breakdown of the X-Content-Type-Options header by directives:

- **nosniff**  
    - This directive instructs the browser not to sniff or guess the MIME type.

### Referrer-Policy

This header controls the amount of information sent to the destination web server when a user is redirected from the source web server, such as when they click a hyperlink. The header is available to allow a web administrator to control what information is shared.  Here are some examples of Referrer-Policy:

- `Referrer-Policy: no-referrer`
- `Referrer-Policy: same-origin`
- `Referrer-Policy: strict-origin`
- `Referrer-Policy: strict-origin-when-cross-origin`

Here’s a breakdown of the Referrer-Policy header by directives:

- **no-referrer**  
    - This completely disables any information being sent about the referrer  
      
    
- **same-origin**  
    - This policy will only send referrer information when the destination is part of the same origin. This is helpful when you want referrer information passed when hyperlinks are within the same website but not outside to external websites.  
      
    
- **strict-origin**  
    - This policy only sends the referrer as the origin when the protocol stays the same. So, a referrer is sent when an HTTPS connection goes to another HTTPS connection.  
      
    
- **strict-origin-when-cross-origin**  
    - This is similar to strict-origin except for same-origin requests, where it sends the full URL path in the origin header

