![[Pasted image 20250704013750.png]]
https://github.com/Quitten/Autorize > read about itttt
# IDOR Checklist

- Check URL > Try edit your ID
- Check HTTPS Request > Try edit some headers
- Check redirect endpoints
- The frontend says no but the backend checks on `X-Original-URL` : Write the full path in it but if there's any parameter leave it above
![[Pasted image 20250705001519.png]]
- access or download media/files of others : For example an image that it's ID related to a user's ID
- try with two accounts and then try without any account
- try with the features that allow data accessing or data modifying
- try guessing the ID or to see if the IDs are hashed and try to guess them > Parameters might be encoded with a more sophisticated scheme than base64 or it might be hashed. In this case, you can try tools like [CyberChef](https://gchq.github.io/CyberChef/) or [hashes.com](https://hashes.com/en/decrypt/hash)
- see how you could know another user's ID and try to use it
- offer the application an ID even if it didn't ask for one:
  `GET /api/messages` > `GET /api/messages?user_id=1234` and see if there's any difference.
  Replace generic terms with specific IDs. Often, endpoints use placeholders like `self` or `user` to refer to the current session's user
  You can find parameter names to try by deleting or editing other objects and seeing the parameter names used.
- try Blind IDORs > see the book if u forgot
- change request method
- Send a wildcard (`*`, `%`, `.`, `_`) instead of an ID, some backend might respond with the data of all the users. > `GET /api/users/* HTTP/1.1`
- Don’t forget to try create/update/delete operations on objects that are publicly readable but shouldn’t be writable. Can you PUT to /api/products and change a price?
- Content-type: application/xml → Content-type: application/json
- {“id”:**19**} → {“id”:**[19]**}
- Understand how they use ID's, hashes, and their API. Do this by looking at the API Documentations if they have one.
- If endpoint has a name like /api/users/myinfo, check for /api/admins/myinfo
- try copy attacker's session
- Burp's Sequencer > checks randomness of a cookie or a token 

## Top IDOR parameters to look out for
```
id=
uid=
gid=
user=
account=
number=
order=
no=
doc=
file=
key=
email=
group=
profile=
edit=
report=
```

## UUIDs
UUIDs are Universally Unique Identifiers, and you will encounter them often when hunting for IDORs. They are designed to be non-guessable, which might seem to shut down avenues for exploitation. Many bug bounty programs do not consider IDORs on UUIDs.

But don’t be deterred; here are a few tricks to test these seemingly secure IDs
- Look for Leaked UUIDs can sometimes be exposed in other parts of the application. Check logs, error messages, and the page source.
- Don’t assume all UUIDs are created perfectly. Test if they are truly non-guessable. Sometimes, developers implement custom UUID generation methods that may not be as random as expected.
- Try simpler modifications. Replace a complex UUID in the URL with basic numeric sequences or predictable patterns like `0000000-0000-0000-000000000000`. You’d be surprised how often the default values are overlooked in access controls.
- Dig in the archives. Utilize tools like the `Wayback Machine` or `Common Crawl`. These archives might hold versions of the application where UUIDs were exposed.

## Parameter Pollution
Consider the following endpoint:
`/api/messages?user_id=<USER_ID>`

If you can’t find an IDOR on the `user_id` parameter, try to add another `user_id`
`/api/messages?user_id=<USER_ID>&user_id=<OTHER_ID>`
Or involve lists :
`/api/messages?user_ids[]=<USER_ID>&user_ids[]=<OTHER_ID>`

## Fuzzing
Sometimes fuzzing can expose overlooked endpoints.
Consider `/api/v1/messages/view`, There are two potential points to fuzz here
Another example is using a filter parameter to only display a specific user’s documents (e.g. `uid_filter=1`), which can also be manipulated to show other users' documents or even completely removed to show all documents at once.
check out this tool > https://github.com/0xsapra/fuzzparam
## AJAX Calls
We may also be able to identify unused parameters or APIs in the front-end code in the form of JavaScript AJAX calls. Some web applications developed in JavaScript frameworks may insecurely place all function calls on the front-end and use the appropriate ones based on the user role.

For example, if we did not have an admin account, only the user-level functions would be used, while the admin functions would be disabled. However, we may still be able to find the admin functions if we look into the front-end JavaScript code and may be able to identify AJAX calls to specific end-points or APIs that contain direct object references. If we identify direct object references in the JavaScript code, we can test them for IDOR vulnerabilities.

This is not unique to admin functions, of course, but can also be any functions or calls that may not be found through monitoring HTTP requests. The following example shows a basic example of an AJAX call:
```javascript
function changeUserPassword() {  
    $.ajax({  
        url:"change_password.php",  
        type: "post",  
        dataType: "json",  
        data: {uid: user.uid, password: user.password, is_admin: is_admin},  
        success:function(result){  
            //  
        }  
    });  
}
```
The above function may never be called when we use the web application as a non-admin user. However, if we locate it in the front-end code, we may test it in different ways to see whether we can call it to perform changes, which would indicate that it is vulnerable to IDOR.

## Understand Hashing/Encoding in JavaScript code
On the other hand, the object reference may be hashed, like (`download.php?filename=c81e728d9d4c2f636f067f89cc14862c`). At a first glance, we may think that this is a secure object reference, as it is not using any clear text or easy encoding. However, if we look at the source code, we may see what is being hashed before the API call is made:
```javascript
$.ajax({  
    url:"download.php",  
    type: "post",  
    dataType: "json",  
    data: {filename: CryptoJS.MD5('file_1.pdf').toString()},  
    success:function(result){  
        //  
    }  
});
```
For example, if the above hash was being calculated on the front-end, we can study the function and then replicate what it's doing to calculate the same hash. Luckily for us, this is precisely the case in this web application.
```javascript
function downloadContract(uid) {  
    $.redirect("/download.php", {  
        contract: CryptoJS.MD5(btoa(uid)).toString(),  
    }, "POST", "_self");  
}
```
This function appears to be sending a `POST` request with the `contract` parameter, which is what we saw above. The value it is sending is an `md5` hash using the `CryptoJS` library, which also matches the request we saw earlier. So, the only thing left to see is what value is being hashed.

In this case, the value being hashed is `btoa(uid)`, which is the `base64` encoded string of the `uid` variable, which is an input argument for the function. Going back to the earlier link where the function was called, we see it calling `downloadContract('1')`. So, the final value being used in the `POST` request is the `base64` encoded string of `1`, which was then `md5` hashed.

---
ex)
it's a PUT method since it's updating details function
```json
{  
    "uid": 1,  
    "uuid": "40f5888b67c748df7efba008e7c2f9d2",  
    "role": "employee",  
    "full_name": "Yourname",  
    "email": "yourname@gmail.com",  
    "about": "A Release is like a boat. 80% of the holes plugged is not good enough."  
}
```
Let’s start by changing our `uid` to another user's `uid` (e.g. `"uid": 2`). However, any number we set other than our own `uid` gets us a response of `uid mismatch`
The web application appears to be comparing the request’s `uid` to the API endpoint (`/1`).
Perhaps we can try changing another user’s details. We’ll change the API endpoint to `/profile/api.php/profile/2`, and change `"uid": 2` to avoid the previous `uid mismatch`.
Next, let’s see if we can create a new user with a `POST` request to the API endpoint. We can change the request method to `POST`, change the `uid` to a new `uid`, and send the request to the API endpoint of the new `uid`
We get an error message saying `Creating new employees is for admins only`. The same thing happens when we send a `Delete` request, as we get `Deleting employees is for admins only`.
Finally, let’s try to change our `role` to `admin`/`administrator` to gain higher privileges. Unfortunately, without knowing a valid `role` name, we get `Invalid role` in the HTTP response, and our `role` does not update
Let’s send a `GET` request with another `uid=2`
As we can see, this returned the details of another user, with their own `uuid` and `role`, confirming an `IDOR Information Disclosure vulnerability.`
we can change this user's details by sending a `PUT` request to `/profile/api.php/profile/2` with the above details along with any modifications we made
We don’t get any access control error messages this time, and when we try to `GET` the user details again, we see that we did indeed update their details

So let us see what just happened:
- trying to change another user's details by modifying the json parameters and header methods
- we were stuck at the role parameter so that we send another GET request to retrieve the data and then used these missing data at the original PUT request to update another user's details
----
ex)
After a while, poking around the registration endpoint, I noticed that the `POST` request is sent to `/api/leads`
While registering, when we try to put the existing user’s email address, it leaked their`ID` with the error code  `lead_already_verified`
![[Pasted image 20250709064014.png]]
**Step 1**: Get the user’s email you want to update the contact number of and then send a `POST` request to `/api/leads` which would return his/her unguessable Lead `id` .
**Step 2**: With above lead `id` send request `PUT` to `/api/lead/<id>/phone-number` with desired phone number on the body.

----
ex)
After playing with functions of main website, I took a look back on my Burp History
Do you notice that? There is no parameter or URL path contains ID, but there is one thing causes my eyes. These API had a common pattern
**/something/something/self/something**
These APIs return my information or doing some actions on behalf of me. I ask myself what if I replace that **self** word with my **user ID** ( user ID could be found on JWT token). For example, **/ngprofile/aggregate/31337/fullProfile** .
And **BOOM! The response return my full profile information.**
I tried to replace my user ID with other user ID ( the user ID is increment). So I could read other users’ s profile information.
I observed that all API contain **self** word is vulnerable to that IDOR, even the Change Email API.
>>> **Try to understand applications ( how could this API/request authorize users, why there is no parameter, etc.), analyze carefully requests/responses. You could find more IDORs.**

---
ex)
_Scenario_1

when I replace my user ID with other user ID in API **/accounts/0001176361**, the server’s response **“Invalid account number”**
when I add **“/”** character append to this user ID, the server’s response return all information about this user. Maybe the **“/”** character breaks logic of the regex or pattern that server used to restrict access.
_Scenario_2

when I replace my application ID with other’s application ID (18385027) in API **api/applications/18385027**, the server’s response **“access_denied”** with **HTTP code 401**
after fuzzing all character, I could bypass authorization control by appending **%20, %09, %0b, %0c, %1c, %1d, %1e, %1f** to application ID in this API. The server would return full information of that application.
>> **(Old but gold): Don’t just replace IDs and wait for luck. Try to fuzz all possible character ( my list is %00 -> %ff) to break the logic of the regex or pattern that server used to restrict access. The more you fuzz, the more you luck.**

---
## The big list of unpredictable ID sources
A triager will often say “But how would the attacker get the ID?”
Just because an ID is unpredictable, that doesn’t mean it can’t be found. Here’s the many ways in which unpredictable IDs can be found.
- **Wayback machine**: Archives URLs and pages. Unpredictable IDs are often in the parameters or in URL path.
- **URLScan**: A website scanner for suspicious and malicious URLs. Companies submit data to it. This often contains unpredictable IDs in the parameters or paths.
- **Common Crawl**: Archives URLs and pages. Unpredictable IDs are often in the parameters or in URL path.
- **Google search**: Google indexes links which might have IDs in the path or parameters, caches pages which might have IDs on them, and indexes websites (like forums) where people often post URLs or full requests which include IDs.
- **GitHub search**: GitHub public repos often include users posting their requests for debugging purposes in the Issues section, hardcoding unpredictable IDs into scripts, etc.
- **Referrer header**: Referrer headers with the ID in the path or GET parameters would leak the ID to other servers
- **It might not be unpredictable**: Any cryptography expert will tell you “random” with computers is very hard. It’s one reason why [Cloudflare uses lava lamps](https://blog.cloudflare.com/randomness-101-lavarand-in-production/). Many “unpredictable” IDs may actually have a design flaw which leads to predictability.
- **Clickjacking to steal the ID**: Clickjacking can leak unpredictable IDs.
- **Org-IDs in OAuth buttons**: OAuth “Sign in with” buttons can leak the org UUID in the generated URL
---
## MongoDB Object IDs Prediction
![[Pasted image 20250709132513.png]]
For example, here’s how we can dissect an actual Object ID returned by an application: 5f2459ac9fa6dc2500314019

1. 5f2459ac: 1596217772 in decimal = Friday, 31 July 2020 17:49:32
2. 9fa6dc: Machine Identifier
3. 2500: Process ID
4. 314019: An incremental counter

Of the above elements, machine identifier will remain the same for as long as the database is running the same physical/virtual machine. Process ID will only change if the MongoDB process is restarted. Timestamp will be updated every second.

The only challenge in guessing Object IDs by simply incrementing the counter and timestamp values,
> use this tool : https://github.com/andresriancho/mongo-objectid-predict

With the default setting, given a seed Object ID, it sends back about 1000 probable Object IDs that could have possibly been assigned to your victim’s resource. Here’s what the output looks like:
![[Pasted image 20250709132753.png]]

----
## Find patterns in API route naming to discover new endpoints
If you see an endpoint that exposes a resource in typical fashion such as:
```
GET /api/albums/<album_id>/photos/<photo_id>
```
Think about what other directories and endpoints there are likely to be in the API. Tools such as [Burp Suite Intruder](https://portswigger.net/support/using-burp-to-test-for-insecure-direct-object-references) or [FFUF](https://github.com/ffuf/ffuf) are great here when combined with API-specific wordlists. If regular API wordlists are not finding anything, then consider using a tool like [CeWL](https://github.com/digininja/CeWL) which will generate custom wordlists for individual applications. Sometimes there will be endpoints that the web app itself rarely hits, but you can send your own requests to them if you find one. These can be gold mines! In the above example, you could try looking for:
```
GET /api/posts/<post_id>/comment/…
GET /api/users/<user_id>/details/…
```
We can also provide alternative values for each section and test to see if they exist. For example, version 1 of the API may have the appropriate access controls in place, but perhaps version 2 is not fully rolled out yet. You may find that version 2 of the API is still accessible if you make calls directly to it and that it lacks the access controls as it is not finished.
`/service/v1/users/<user_id>`
Where:
- service: application context
- v1: version
- users: resource
- <user_id>: parameter
---
## Try replacing parameter names

Once you have spent a decent amount of time testing an application, you may start to remember parameters that have been used throughout it. You can try these parameters in other areas and see if they will be accepted. For example, if you see something like:
`GET /api/albums?**album_id**=<album id>`
You could replace the album_id parameter with something completely different and potentially get other data.
`GET /api/albums?**account_id**=<account id>`
This could be used to obtain a list of a user’s album_ids, which you could then use in the original query. There is a Burp extension called [Paramalyzer](https://portswigger.net/bappstore/0ac13c45adff4e31a3ca8dc76dd6286c) which will help with this by remembering all the parameters you have passed to a host.

---
## Adding someone to a chat?
Alarm bells! Pay close attention to these types of actions as they are often where you can find IDORs. Can add yourself to another chat simply by changing values?

---
## Does the app ask for non-numeric IDs? Use numeric IDs instead
There may be multiple ways of referencing objects in the database and the application only has access controls on one. Try numeric IDs anywhere non-numeric IDs are accepted:
```
username=user1 → username=1234
account_id=7541A92F-0101-4D1E-BBB0-EB5032FE1686 → account_id=5678
album_id=MyPictures → album_id=12
```

---
## Error message?
Do not despair. Sometimes applications will throw an error message regardless of if the action you attempted was successful or not. Even if you get an error message, check manually to see if the action worked.

---
## Continuity
Make sure if you are replacing one ID with another that you replace it in **every part** of the request, not just in one instance. This is an easy mistake to make and could cause you to overlook potential vulnerabilities.

---
Note: To find URL parameters for endpoints, use tools like [Arjun](https://github.com/s0md3v/Arjun) or Parameth to brute force common IDOR URL parameter names.
IDOR's are commonly found on API endpoints with JSON parameters, not URL.
```
{“id”:111} --> 401 Unauthriozied
{“id”:[111]} --> 200 OK

{“id”:111} --> 401 Unauthriozied
{“id”:{“id”:111}} --> 200 OK

/v3/users_data/1234 --> 403 Forbidden
/v1/users_data/1234 --> 200 OK

POST /api/get_profile
Content-Type: application/json
{“user_id”:<legit_id>,”user_id”:<victim’s_id>}

GET /admin/profile --> 401 Unauthorized
GET /ADMIN/profile --> 200 OK

POST /users/delete/VICTIM_ID --> 403 Forbidden
POST /users/delete/MY_ID/../VICTIM_ID --> 200 OK

GET /graphql HTTP/1.1
Host: example.com
GET /graphql.php?query= HTTP/1.1
Host: example.com

GET /file?id=90ri2-xozifke-29ikedaw0d HTTP/1.1
Host: example.com
GET /file?id=302
Host: example.com
```

---
## Revisit this lap > Method-based access control can be circumvented

But in short, if you gonna change request method (by clicking on change method option in burp) you need first to edit the session cookie to by the attacker's

---
## rs0n Video

- make accounts using your bugcrowd account +demo1 , +demo2
- get rid of headers until u reach the least amount of headers that still be able to identify you and your note to pointers section
  ![[Pasted image 20250721115527.png]]
- try to remove or edit the signature (salt) because maybe it's doesn't validate it
- jwt.io > to see the URL encoding and to see which of the json parameters they are validating and compare with the two users
- 
 
-----
## Resources
- https://adipsharif.medium.com/unveiling-all-techniques-to-find-idors-in-web-applications-578d2b8aa28a
- https://16521092.medium.com/some-ways-to-find-more-idor-da16c93954e5
- [IDORs with unpredictable IDs are valid vulnerabilities · Joseph Thacker](https://josephthacker.com/hacking/cybersecurity/2022/08/18/unpredictable-idors.html)
- https://www.aon.com/en/insights/cyber-labs/finding-more-idors-tips-and-tricks