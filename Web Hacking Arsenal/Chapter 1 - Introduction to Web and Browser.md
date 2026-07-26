GET can be used to send content to the server. `GET /login.php username=myusername&password=mypassword`
- Browsers and intermediary proxies may cache GET requests.
- GET requests are logged in server logs.
## HTTP 2
- Multiplexing, which allows multiple resources to be delivered concurrently over a single connection.
- server push, which allows the server to send content to the client by predicting what the client will need. On the contrary, server push can be abused by the server by potentially pushing malicious resources to the client or spoofing existing objects.
## EVOLUTION OF MODERN WEB APPLICATIONS
#### **Shift in Architecture**
from monolithic structures-meaning that all the components were tightly coupled and interdependent- to microservices architecture. This is achieved by breaking down applications
into smaller, independently functioning units.
### **Shift in Stacks**
from LAMP Stack an acronym for Linux, Apache, MySQL, and PHP/Perl/Python to MEAN (MongoDB, Express.js, Angular, Node.js) and MERN (MongoDB, Express.js, React, Node.js) Stacks except for Linux.
These stacks offer a unified language, that is, JavaScript, across both client and server sides, making development more efficient and streamlined. The adoption of NoSQL databases such as MongoDB, Cassandra, and Redis provides alternatives to traditional relational databases. While MERN has become the popular choice, it has brought its own set of problems concerning security, for instance, the introduction of a new class of vulnerabilities such as Node.js injection, NoSQL injection, dependency injection, and so on.
### **Single-Page Applications (SPAs)**
Unlike traditional websites, a new page is fetched from the server every time you navigate or submit a form. SPA initially loads the entire web page and all the components just once. After the initial load, SPA dynamically updates content on the same page, eliminating the need for reloading the web page. Due to the heavy reliance upon JavaScript to render and dynamically update content, SPAs are often vulnerable to DOM-based cross-site scripting.
### **Use of Cloud Components**
### **Serverless Architecture**
Despite its name, serverless architecture does involve servers. In this model, the cloud provider is responsible for managing the servers; developers write code and only pay for the time code is executing. Serverless architecture comes with its own set of security challenges

## UNDERSTANDING DATA ENCODING
### **URL encoding**
In URL encoding, characters not part of the allowed set for URLs are replaced with a percentage symbol (%) followed by their hexadecimal value.
`https://example.com/login.php?username=tmgm&password=t&mgm`
In this example, the password contains ampersand “&”, which if not encoded will be treated as a parameter and will set a password to “t” and “mgm”. To prevent this, ampersand “&” is encoded as “%26”, resulting in the final URL: `https://example.com/login.php?username=tmgm&password=t%26mgm`
### **Double Encoding**
In double encoding, the characters are encoded twice: the first level of encoding converts the character into a percent-encoded form. The second level of encoding is applied to the percent-encoded characters. In other words, this means that it will apply to the percent sign (%) itself, along with the encoded character hexadecimal value being encoded another time.
``` example
'<' > '%3C' > '%253C'
```
### **HTML Encoding**
In HTML, certain characters have special meanings and can lead to ambiguities if not handled correctly. For instance, characters “<” and “>” can represent the opening and closing of an HTML tag. To ensure that these characters are displayed in textual format, rather than being interpreted as HTML syntax, they have to be HTML encoded. HTML encoding involves replacing these special characters with character entities. For example, a less-than sign “<” in HTML can be replaced with “&lt”, and for a great-than sign “>”, we would use “&gt”. Additionally, HTML encoding is not just limited to characters that have special meaning in HTML syntax. It can also represent characters not readily available on standard keyboards. For example, the copyright symbol “©” can be represented in HTML as “&copy”. As per HTML specification, all character references must start with an ampersand “&” sign, this can be followed by multiple variations such as decimal and hexadecimal encoding. Here are various ways to represent these characters.
![[Pasted image 20260408204806.png]]
### **Base64 Encoding**
Base64 encoding can be used to allow binary data to be represented as an ASCII string.
It is worth mentioning that base64 is commonly confused by developers as an encryption scheme rather than an encoding scheme.
### **Unicode Encoding**
Unicode contains a large number of characters from numerous languages around the world. For instance, if you wish to include Arabic or Persian text in the web page, you will find that these characters are not part of ASCII as it was primarily developed for English and has limited character sets. This is where Unicode comes into play. To effectively map such a large set of characters, Unicode utilizes several encodings such as UTF-8, UTF-16, and UTF-32.
![[Pasted image 20260408205354.png]]
Understanding how encodings work can provide great aid in bypassing WAFs and filters that rely upon blacklists.
## INTRODUCTION TO BROWSERS
### **User Interface**
This represents the HTML and CSS and displays the parsed content on the screen. For example, the address bar, back/forward buttons, and bookmarking menu are all parts of the user interface.
### **Browser Engine**
It acts as an interface between the UI and the rendering engine. For example, when a user interacts with the browser interface, such as typing a URL, clicking a link, or interacting with the form, the browser engine is responsible for processing the command.
### **Rendering Engine**
A rendering engine is an integral part of the web browsers, it essentially converts HTML, CSS, and JavaScript into a visual and interactive pages.
### **Networking**
This component is responsible for making underlying network calls such as HTTP and DNS.
### **UI Backend**
This is used to access underlying operating system methods such as combo boxes, user boxes, and so on.
### **JavaScript Interpreter**
This component is responsible for parsing and executing JavaScript code present within web pages.
### **Data Storage**
This component is responsible for storing data on the client side. This includes mechanisms such as cookies, Web Storage, IndexedDB, WebSQL, and FileSystem.
## CORE BROWSER SECURITY POLICIES AND MECHANISMS
### **Same-Origin Policy**
The policy in its essence prevents web pages in one origin from being able to access properties in a different origin.
![[Pasted image 20260408234342.png]]
Note: In the table, store.example.com and about:blank are shown to be on the same origin. This
might be confusing for some readers. “About:blank” has no origin and inherits the origin of the
document that created it. For instance, if a page at https://store.example.com opens a new window to
about:blank, then the about:blank page will inherit the origin of https://store.example.com.
### **Content Security Policy**
CSP can be set to only allow browsers to include JavaScript from a specified resource and can be implemented through “Content-Security-Policy” headers
**CSP Level 1**—This level introduced basic directives that would allow developers to specify the sources to be whitelisted. This includes scripts, styles, images, and more.
**CSP Level 2**—This level offered developers more control over which origins can embed your site into frames.
**CSP Level 3**—This level is currently under development, and only parts of it have been implemented under modern browsers and offer more granular control of resource interactions.

### **HTTP Cookies**
A cookie is defined based on Domain and Path attributes in contrast to the Same-Origin Policy (SOP), which relies on the scheme, host, and port. For instance, consider the following cookie.
### **Iframe Sandbox**
allows websites to embed web pages into the current document. When a page is loaded in an Iframe, it will load all the contents from the destination address including HTML, CSS, and JavaScript, which adds a security risk.
To address this issue, HTML5 specifications introduced the “sandbox” attribute for Iframe. This offers a granular control over the type of content that should be loaded. 
The default settings of the sandbox attribute are very restrictive in nature. These include blocking JavaScript execution and disabling form submissions among other constraints. However, it also provides developers the flexibility to fine-tune the allowed content through various attributes such as allow forms, allow-popups, allow-same-origin, and allow-scripts. Additionally, CSP includes a sandbox directive which, when implemented, applies similar restrictions across the entire document.
### **Subresource Integrity Check**
Web applications often load external resources such as JavaScript libraries and CSS files. These resources are sometimes hosted on third-party servers like code.jquery.com for jQuery. However, if such a domain is compromised and its content is replaced with a malicious version, any website embedding these resources could also be compromised.
SRI ensures that scripts have not been modified since they were first loaded. This is accomplished through the integrity attribute, which takes in input that contains the checksum or hash value (such as SHA-256, SHA-384, or SHA-512) of the external file
### **HTTP Strict Transport Layer Security (HSTS)**
Websites can use HTTP to HTTPS, and forced redirect can also ensure that the website is only accessible over HTTPS. However, this alone will not prevent protocol downgrade attacks. This is a form of cryptographic attack, used to downgrade an encrypted connection to a weak mode of operation, hence making it trivial for an attacker to intercept and decrypt the data. To address this issue, browsers introduced HTTP Strict Transport Security (HSTS), which instructs browsers to convert all HTTP requests to HTTPS and hence prevent attackers from exploiting insecure HTTP connections. HSTS policy can be set using the “Strict-Transport-Security” header in the HTTP response.
HSTS can contain a “preload” directive, which is a list that is hardcoded into browsers to always use HTTPS, even before any interaction with the website