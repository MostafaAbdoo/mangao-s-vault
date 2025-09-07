# Study
A _race condition_ happens when two sections of code that are designed to be executed in a sequence get executed out of sequence. To understand how this works, you need to first understand the concept of concurrency. In computer science, _concurrency_ is the ability to execute different parts of a program simultaneously without affecting the outcome of the program. Concurrency can drastically improve the performance of programs because different parts of the program’s operation can be run at once.
Concurrency has two types: multiprocessing and multithreading.
race conditions happen when the outcome of the execution of one thread depends on the outcome of another thread, and when two threads operate on the same resources without considering that other threads are also using those resources. When these two threads are executed simultaneously, unexpected outcomes can occur.

A race condition becomes a vulnerability when it affects a security control mechanism. In those cases, attackers can induce a situation in which a sensitive action executes before a security check is complete. For this reason, race condition vulnerabilities are also referred to as _time-of-check_ or _time-of-use_ vulnerabilities.
Although race conditions are often associated with financial sites, attackers can use them in other situations too, such as to rig online voting systems.

### Send Simultaneous Requests

You can then test for and exploit race conditions in the target by sending multiple requests to the server simultaneously.

For example, if you have $3,000 in your bank account and want to see if you can transfer more money than you have, you can simultaneously send multiple requests for transfer to the server via the `curl` command. If you’ve copied the command from Burp, you can simply paste the command into your terminal multiple times and insert a `&` character between each one. In the Linux terminal, the `&` character is used to execute multiple commands simultaneously in the background:

```
curl (transfer $3000) & curl (transfer $3000) & curl (transfer $3000)
& curl (transfer $3000) & curl (transfer $3000) & curl (transfer $3000)
```
 or see this video > [Video](https://youtu.be/tclPuS57pU8?si=nlIXyHduvKB_Q8FB)
Be sure to test for operations that should be allowed once, but not multiple times!

## Prevention
The key to preventing race conditions is to protect resources during execution by using a method of _synchronization_, or mechanisms that ensure threads using the same resources don’t execute simultaneously.

Resource locks are one of these mechanisms. They block other threads from operating on the same resource by _locking_ a resource. In the bank transfer example, thread 1 could lock the balance of accounts A and B before modifying them so that thread 2 would have to wait for it to finish before accessing the resources.

Most programming languages that have concurrency abilities also have some sort of synchronization functionality built in. You have to be aware of the concurrency issues in your applications and apply synchronization measures accordingly. Beyond synchronization, following secure coding practices, like the principle of least privilege, can prevent race conditions from turning into more severe security issues.

---
# Hunting
 **Features to test** :
 - Rating a product excessively
 - Voting system
 - Coupon Discount
 - reusing a single CAPTCHA solution
 - bypassing anti-brute-force rate limits

## Detecting and exploiting limit overrun race conditions with Burp Repeater
- The key steps involve identifying a single-use or rate-limited endpoint with security implications and issuing multiple requests quickly to test for limit overruns.
- These race windows are often extremely short, measured in milliseconds.
- Burp Suite 2023.9 introduces enhancements to Burp Repeater to address network jitter issues.
- It automatically adjusts techniques based on the server's HTTP version:
   - For HTTP/1, it uses the last-byte synchronization technique.
   - For HTTP/2, it employs the single-packet attack technique, initially presented by PortSwigger Research at Black Hat USA 2023.
- The single-packet attack neutralizes network jitter by completing 20-30 requests with a single TCP packet.
- Although two requests can often trigger an exploit, sending a larger number helps mitigate internal latency or server-side jitter.

> Send the applied coupon request to Repeater 15 times, group all of the tabs into one group, and then send them all in parallel to obtain a significant discount.

>Turbo Intruder.

## Hidden multi-step sequences
Instead of two separate requests fighting over the same resource at the same time, this is about **one request** that goes through multiple **hidden internal steps (sub-states)** inside the server before it’s finished.
Let’s take the **MFA login example** :
- **You log in** → you enter your username/password and press login.
- **Server creates a temporary state** → “User is logged in, but MFA not completed yet.”
    - You don’t see this; it’s just inside the server.
- **Normally**, the server will now ask you for your MFA code.
- **After you enter MFA correctly**, the server switches you to the “fully logged in” state.
Sometimes that temporary state (“logged in but MFA pending”) is **already valid enough** to do things — maybe even access private data.  
It might exist for **only a fraction of a second**, but…
If an attacker:
- Knows that this state happens,
- And sends another HTTP request _at the right time_,
…they can **use that temporary state** before the MFA check finishes — skipping MFA entirely.

### Methodology: Predict → Probe → Prove
#### **Step 1: Predict Potential Collisions**

- **Purpose:**  
    Figure out where a collision _could_ happen — meaning two (or more) requests affecting the **same internal process or sub-state** at the same time.
    
- **What you do:**
    - Focus only on important endpoints (like login, password reset, payment) that could expose sensitive actions.
        
    - Look for parts of the app where **two requests might interact with the same data record** or step in a process.
        
- **In the hidden multi-step sequence context:**  
    You’d look for endpoints that:
    
    - Transition through temporary sub-states (like “logged in but MFA pending”).
        
    - Could be hit with another request _before_ the final step completes.
- **Example:**  
    In a flawed MFA system, your “predict” step would identify that the login endpoint sets a temporary session before enforcing MFA.
#### **Step 2: Probe for Clues**

- **Purpose:**  
    See if your guess was right — send controlled test requests to spot irregular behavior.
    
- **What you do:**
    
    1. **Benchmark normal behavior** — send single requests slowly and observe expected results.
        
    2. Send requests **back-to-back** (or even timed to the last byte) to try hitting the **same sub-state** simultaneously or before it’s locked down.
        
    3. Look for **odd results** compared to the baseline:
        
        - Different response content
            
        - Email changes
            
        - Application skipping steps (e.g., skipping MFA)
            
- **In the hidden multi-step sequence context:**  
    You might send:
    
    - First request to log in (triggering the “MFA pending” sub-state).
        
    - Second request _immediately_ to access a protected page using that temporary session.  
        If it works, you’ve found a clue.
#### **Step 3: Prove the Concept**

- **Purpose:**  
    Turn the clue into a repeatable, verifiable exploit.
    
- **What you do:**
    - Simplify and analyze your results — make sure they’re not random network artifacts.
    - Reproduce the bypass consistently.
    - Check if the vulnerability is **structural** — meaning it’s a design flaw, not just a one-time glitch.
        
- **In the hidden multi-step sequence context:**  
    You’d show that:
    - You can reliably skip MFA by hitting the temporary state.
    - The same flaw might exist in other flows that use hidden sub-states.

## **Multi-endpoint race conditions**
A **multi-endpoint race condition** happens when two (or more) **different** endpoints in an application interact with the **same underlying data** or state, and the timing of those requests lets you break the intended logic.
### **Example in a hidden multi-step sequence context**

Imagine a banking app:
1. Endpoint A — `/transfer` — starts a money transfer.
2. Endpoint B — `/cancel-transfer` — cancels the transfer.
3. If you send both requests **at the right time**, you might:
    - Cancel the transfer in the UI.
    - Still have it processed in the backend.
    - Possibly get the money sent _and_ keep your balance.
Or with MFA:
- Endpoint A logs you in (creates “MFA pending” session).
- Endpoint B loads your dashboard.
- If sent in parallel, the dashboard request might succeed before the MFA enforcement kicks in.
### **Steps to trigger a multi-endpoint race condition**

1. **Find the two endpoints**
    
    - Example: `/login` (creates temporary session) and `/dashboard` (uses the session).
    - Or `/transfer` and `/cancel-transfer`.
        
2. **Intercept each request** and send them to **Burp Repeater** (or directly to Intruder/HTTP race plugin).
    
3. **Create a single group** in **Burp’s Turbo Intruder** or **HTTP/2 Smuggling tool**:
    
    - Add the first request (Endpoint A).
    - Add the second request (Endpoint B).
    - Duplicate them multiple times inside that group so you send **A and B pairs** in quick succession.
        
4. **Send them in parallel** (not sequentially):
    
    - This is the key — both requests must hit the backend close enough in time that the shared resource or state is still vulnerable.
    - Use low-latency mode like “last-byte sync” if possible.