API is a set of rules that allow one application to communicate with another. They enable applications to share data in a controlled way. Using APIs, applications on the internet can take advantage of other applications’ resources to build more complex features.
For example, if a developer wants their code to retrieve the contents of a tweet from Twitter’s database, they can use a Twitter API endpoint that returns tweet information by sending a GET request to the Twitter API server located at _api.twitter.com_ 
```http
GET /1.1/statuses/show.json?id=210462857140252672
Host: api.twitter.com
```
This URL indicates that the developer is using Twitter’s API version 1.1 and requesting the resource called `statuses` (which is what Twitter calls its tweets) with the ID 210462857140252672. APIs usually return data in JSON or XML format.
APIs often require users to authenticate before accessing their services. Typically, users include access tokens in their API requests to prove their identities. Other times, users are required to use special authentication headers or cookies.
### REST APIs
REST APIs usually have defined structures for queries that make it easy for users to predict the specific endpoints to which they should send their requests
You can see here that all of Twitter’s API endpoints are structured in the same way *https://api.twitter.com/1.1/RESOURCE/ACTION*
REST APIs can also use various HTTP methods; Get, Post, Put and Delete.
### SOAP APIs
_SOAP_ is an API architecture that is less commonly used in modern applications. But plenty of older apps and IoT apps still use SOAP APIs. SOAP APIs use XML to transport data, and their messages have a header and a body. A simple SOAP request looks like this:
```
DELETE / HTTPS/1.1
Host: example.s3.amazonaws.com

<DeleteBucket xmlns="http://doc.s3.amazonaws.com/2006-03-01">
  <Bucket>quotes</Bucket>
  <AWSAccessKeyId> AKIAIOSFODNN7EXAMPLE</AWSAccessKeyId>
  <Timestamp>2006-03-01T12:00:00.183Z</Timestamp>
  <Signature>Iuyz3d3P0aTou39dzbqaEXAMPLE=</Signature>
  </DeleteBucket>
```
This example request is taken from Amazon S3’s SOAP API documentation. It deletes an S3 bucket named _quotes_. As you can see, API request parameters are passed to the server as tags within the XML document
### GraphQL APIs
_GraphQL_ is a newer API technology that allows developers to request the precise resource fields they need, and to fetch multiple resources with just a single API call.
GraphQL APIs use a custom query language and a single endpoint for all the API’s functionality. These endpoints are commonly located at /graphql, _/gql_, or _/g_. GraphQL has two main kinds of operations: queries and mutations. _Queries_ fetch data, just like the GET requests in REST APIs. _Mutations_ create, update, and delete data, just like the POST, PUT, and DELETE requests in REST APIs.
_https://SHOPNAME.myshopify.com/admin/api/API_VERSION/graphql.json_
```
query {
   shop {
     name
     primaryDomain {
       url
       host
     }
   }
 }
```

```
{
   "data": {
     "shop": {
       "name": "example",
       "primaryDomain": {
         "url": "https://example.myshopify.com",
         "host": "example.myshopify.com"
       }
     }
   }
}
```
Notice that the response doesn’t contain all the object’s fields, but instead the exact fields the user has requested.
Mutations, used to edit data, can have arguments and return values. Let’s take a look at an example of a mutation taken from _graphql.org_. This mutation creates a new customer record and takes three input parameters: `firstName`, `lastName`, and `email`. It then returns the ID of the newly created customer:

```
mutation {
  customerCreate(
    input: {
      firstName: "John",
      lastName: "Tate",
      email: "john@johns-apparel.com" })
     {
     customer {
       id
     }
   }
}
```
GraphQL APIs also include a great reconnaissance tool for bug hunters: a feature called _introspection_ that allows API users to ask a GraphQL system for information about itself.
### API-Centric Applications
Instead of retrieving complete HTML documents from the server, API-centric apps consist of a client-side component that requests and renders data from the server by using API calls.
When a company already has a web app, using an API-centric approach to build mobile apps saves time. APIs allow developers to separate the app’s rendering and data-transporting tasks: developers can use API calls to transport data and then build a separate rendering mechanism for mobile, instead of reimplementing the same functionalities.
Yet the rise of API-centric applications means that companies and applications expose more and more of their data and functionalities through APIs. APIs often leak sensitive data and the application logic of the hosting application

---
## Enumerating
**REST** -> using wordlists to find endpoints
**GraphQL** -> Enumerating GraphQL is much easier
- To the GraphQL endpoint send an introspection query
- This will return every query that can be run on the database and what parameters these queries need to work
- Using tools like GraphQL Voyager allows you to see a visual representation of the result
---
If you see an endpoint like: `/api/v3/users/1/edit`
It may be worth checking for `/api/v1/users/1/edit`

---
## Hunting for API Vulnerabilities
there are many open source API development and testing tools that you can use to make the API testing process more efficient. Postman ([https://www.postman.com/](https://www.postman.com/)) is a handy tool that will help you test APIs. You can use Postman to craft complex API requests from scratch and manage the large number of test requests that you will be sending. GraphQL Playground ([https://github.com/graphql/graphql-playground/](https://github.com/graphql/graphql-playground/)) is an IDE for crafting GraphQL queries that has autocompletion and error highlighting.
ZAP has a GraphQL add-on ([https://www.zaproxy.org/blog/2020-08-28-introducing-the-graphql-add-on-for-zap/](https://www.zaproxy.org/blog/2020-08-28-introducing-the-graphql-add-on-for-zap/)) that automates GraphQL introspection and test query generation. Clairvoyance ([https://github.com/nikitastupin/clairvoyance/](https://github.com/nikitastupin/clairvoyance/)) helps you gain insight into a GraphQL API’s structure when introspection is disabled.
### Performing Recon
If you’re attacking a REST or SOAP API, or if introspection is disabled on the GraphQL API you’re attacking, start by enumerating the API. To enumerate the API, start by reading the API’s public documentation if it has one. Companies with public APIs often publish detailed documentation about the API’s endpoints and their parameters.

Try using Swagger ([https://swagger.io/](https://swagger.io/)), a toolkit developers use for developing APIs. Swagger includes a tool for generating and maintaining API documentation that developers often use to document APIs internally. Sometimes companies don’t publicly publish their API documentation but forget to lock down internal documentation hosted on Swagger. In this case, you can find the documentation by searching the internet for _company_name inurl:swagger_.

The next thing you can do is go through all the application workflows to capture API calls. You might find API calls used in the application’s workflow that aren’t in public documentation.

If both _/posts/POST_ID/read_ and /_posts/POST_ID/delete_ exist, is there an endpoint called _/posts/POST_ID/edit_? Similarly, if you find blog posts located at _/posts/1234_ and _/posts/1236_, does /_posts/1235_ also exist?
Next, search for other API endpoints by using recon techniques from Chapter 5, such as studying JavaScript source code or the company’s public GitHub repositories. You can also try to generate error messages in hopes that the API leaks information about itself. Fuzzing techniques can also help you find additional API endpoints by using a wordlist *https://gist.github.com/yassineaboukir/8e12adefbd505ef704674ad6ad48743d* 
### Testing for Broken Access Control and Info Leaks
For example, sometimes API tokens aren’t validated after the server receives them. Other times, API tokens are not randomly generated and can be predicted. Finally, some API tokens aren’t invalidated regularly, so attackers who’ve stolen tokens maintain access to the system indefinitely.

Another issue is broken resource or function-level access control. Sometimes API endpoints don’t have the same access-control mechanisms as the main application. For example, say a user with a valid API key can retrieve data about themselves. Can they also read data about other users? Or can they perform actions on another’s behalf through the API? Finally, can a regular user without admin privileges read data from endpoints restricted to admins? Separately from REST or SOAP APIs, the GraphQL API of an application may have its own authorization mechanisms and configuration. This means that you can test for access-control issues on GraphQL endpoints even though the web or REST API of an application is secure

Other times still, an API offers multiple ways to perform the same action, and access control isn’t implemented across all of them. For example, let’s say that a REST API has two ways of deleting a blog post: sending a POST request to _/posts/POST_ID/delete_ and sending a DELETE request to _/posts/POST_ID_. You should ask yourself: are the two endpoints subject to the same access controls?

Another common API vulnerability is information leaks. API endpoints often return more information than they should, or than is needed to render the web page.

Make a list of the endpoints that should be restricted by some form of access control. For each of these endpoints, create two user accounts with different levels of privilege: one that should have access to the functionality and one that shouldn’t. Test whether you can access the restricted functionality with the lower-privileged account. If your lower-privileged user can’t access the restricted functionality, try removing access tokens, or adding additional parameters like the cookie `admin=1` to the API call. You can also switch out the HTTP request methods, including GET, POST, PUT, PATCH, and DELETE, to see if access control is properly implemented across all methods

Try to view, modify, and delete other users’ info by switching out user IDs or other user identification parameters found in the API calls. If IDs used to identify users and resources are unpredictable, try to leak IDs through info leaks from other endpoints
### Testing for Rate-Limiting Issues
APIs often lack rate limiting; in other words, the API server doesn’t restrict the number of requests a client or user account can send within a short time frame. A lack of rate limiting in itself is a low-severity vulnerability unless it’s proven to be exploitable by attackers. But on critical endpoints, a lack of rate limiting means that malicious users can send large numbers of requests to the server to harvest database information or brute-force credentials.

To test for rate-limiting issues, make large numbers of requests to the endpoint. You can use the Burp intruder or `curl` to send 100 to 200 requests in a short time. Make sure you repeat the test in different authentication stages, because users with different privilege levels can be subject to different rate limits. Be really careful when you are testing for rate-limiting issues because it’s very possible to accidentally launch a DoS attack on the app by drowning it with requests.
### Testing for Technical Bugs
APIs are therefore susceptible to many of the other vulnerabilities that affect regular web applications too. Since APIs are another way applications accept user input, they become another way for attackers to smuggle malicious input into the application’s workflow.

If an API endpoint can access external URLs, it might be vulnerable to SSRF, so you should check whether its access to internal URLs isn’t restricted. Race conditions can also happen within APIs. If you can use API endpoints to access application features affected by race conditions, these endpoints can become an alternative way to trigger the race condition.

Other vulnerabilities, like path traversal, file inclusion, insecure deserialization issues, XXE, and XSS can also happen. If an API endpoint returns internal resources via a file path, attackers might use that endpoint to read sensitive files stored on the server. If an API endpoint used for file uploads doesn’t limit the data type that users can upload, attackers might upload malicious files, such as web shells or other malware, to the server. APIs also commonly accept user input in serialized formats such as XML. In this case, insecure deserialization or XXEs can happen. RCEs via file upload or XXEs are commonly seen in API endpoints. Finally, if an API’s URL parameters are reflected in the response, attackers can use that API endpoint to trigger reflected XSS on victims’ browsers.

The process of testing for these issues will be similar to testing for them in a regular web app. You’ll simply supply the payloads to the application in API form.