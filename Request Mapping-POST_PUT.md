### Request Mapping Types

### Get

### Features of GET

```scala
 Here, are the important features of GET:
- It is very easy to bookmark data using GET method.
- The length restriction of GET method is limited.
- You can use this method only to retrieve data from the address bar in the browser.
- This method enables you to easily store the data.

```

 

| **S.No.** | **                      GET Request**    | **                               POST Request** |
| --------- | ---------------------------------------- | ---------------------------------------- |
| 1.        | GET retrieves a representation of the specified resource. | POST is for writing data, to be processed to the identified resource. |
| 2.        | **It typically has relevant information in the URL of the request.** | **It typically has relevant information in the body of the request.** |
| 3.        | **It is limited by the maximum length of the URL supported by the browser and web server.** | **It does not have such limits.**        |
| 4.        | It is the default HTTP method.           | In this, we need to specify the method as POST to send a request with the POST method. |
| 5.        | You can bookmark GET requests.           | You cannot bookmark POST requests.       |
| 6.        | **It is less secure because data sent is part of the URL** | I**t is a little safer because the parameters are not stored in browser history or in web server logs.** |
| 7.        | It is cacheable.                         | It is not cacheable.                     |
| 8.        | For eg. GET the page showing a particular question. | For eg. Send a POST request by clicking the “Add to cart” button. |



| PUT                                      | POST                                     |
| ---------------------------------------- | ---------------------------------------- |
| **This method is idempotent.**           | **This method is not idempotent.**       |
| **PUT method is call when you have to modify a single resource, which is already a part of resource collection.*** | **POST method is call when you have to add a child resource under resources collection.** |
| RFC-2616 depicts that the PUT method sends a request for an enclosed entity stored in the supplied request URI. | This method requests the server to accept the entity which is enclosed in the request. |
| PUT method syntax is PUT /questions/{question-id} | POST method syntax is POST /questions    |
| **PUT method answer can be cached.**     | **You cannot cache POST method responses.** |
| PUT /vi/juice/orders/1234 indicates that you are updating a resource which is identified by “1234”. | POST /vi/juice/orders indicates that you are creating a new resource and return an identifier to describe the resource. |
| **if you send the same request multiple times, the result will remain the same.** | **If you send the same POST request more than one time, you will receive different results.** |
|                                          |                                          |
| We use UPDATE query in PUT.              | We use create query in POST.             |
| In PUT method, the client decides which URI resource should have. | In POST method, the server decides which URI resource should have. |

**PUT HTTP Request:** PUT is a method of modifying resources where the client sends data that updates the entire resource. 

**PATCH HTTP Request:** PUT is a method of modifying some part of the resources where the client sends data .



| PUT                                      | PATCH                                    |
| ---------------------------------------- | ---------------------------------------- |
| PUT is a method of modifying resource where the client sends data that updates the **entire resource** . | PATCH is a method of modifying resources where the client sends **partial data** that is to be updated without modifying the entire data. |
| In a PUT request, the enclosed entity is considered to be a modified version of the resource stored on the origin server, and the client is requesting that the stored version be replaced | With PATCH, however, the enclosed entity contains a set of instructions describing how a resource currently residing on the origin server should be modified to produce a new version. |
| **HTTP PUT is said to be idempotent, So if you send retry a request multiple times, that should be equivalent to a single request modification** | **HTTP PATCH is basically said to be non-idempotent. So if you retry the request N times, you will end up having N resources with N different URIs created on the server.** |

