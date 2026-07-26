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