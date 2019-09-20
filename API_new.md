# API
**API** – stands for **Application Program Interface** – is a set of routines, protocols and tools for building software applications. Its core concept is to have defined rules by which if a client makes a request in a specific format, it will always get a specific response, which provides different applications the means to communicate with each other. That, in turn, allows software developers to access, interact and process outer data and solutions to build on in their own projects. For example, you can embed Google Maps or integrate YouTube videos on your own webpage by using the Google Maps API or YouTube API’s respectively.
# RESTful API

REST – **Representational State Transfer** – is an architectural pattern for developing API's. It's a set of constraints according to which web services should be developed. A RESTful API uses representations of its resources to expose information about itself. It also let's the client operate with these resources such as creating new ones or changing existing resources.
Any RESTful web service is built on a client-server model.
 - **Client** - a person or a software that uses the API. It can be a developer or a web browser. A developer can use different API's to work with specific data, a web browser calls the API and uses the returned data to render the information on screen. The client requests the data and maintains the user state.
 - **Server** - the required software and hardware that stores the data and the API to work with it.
 **Resource** — a resource can be any object the API can provide information about. It stores information about itself and other resources that are linked with it. For example, the resource "users" would store links to all "user" resources included.
 
The server always transfers the **representation** of the requested resource instead of the resource itself. It allows using different data formats, such as **JSON, XML**, etc.
To operate with a resource, the client has to provide the server with the means to find the resource - it's **URL** -  and the type of operation they want to perform in a form of a **HTTP method**. The most common ones are **GET, POST, PUT, DELETE**

Following REST constrains makes discovering and using your API's much easier for any developer.
 ----
As mentioned above, any RESTful web service should give a client the access to server’s resources through a common approach, such as HTTP requests. To get a better understanding of how they work, let’s use an example.
Imagine you have a RESTful web service that manages a database of students. You want to use the students' data for your other app. To do that, your app would have to communicate with web service's API in a form of HTTP requests.
To take a better look at how it works let's start with the **GET** request.

----
**GET** request is used when a client needs to obtain data from the server. Such data is called resource and can be anything, such as an image or a text file. To do so, it sends a message with different specifications of where, how, and what to obtain to the server and gets a response afterwards.
Let's breakdown a simple GET request for our web service:

    GET / HTTP/1.1
    Accept: application/json

As you can see here, our app sends a **message** to the server that specifies what resource we want to get a representation of. Here we requested a base resource: `/` which should contain links to other resources and some additional data. We specify which version of HTTP protocol is used with `HTTP/<version>`. Moreover, both client and server need to have a common data interchange format to follow the uniform interface constraint. In this request the client would accept data in JSON format, which is defined with a `Accept: application/json` header. Headers are an integral part of HTTP requests and are used to specify what and how information is to be processed in a request or response. There are many types of headers used for different purposes. 
After sending the request, the client would get a **response**:

```html
HTTP/1.1 200 OK
Content-Type: application/json
{
    "version": "2.0",
    "links": [
        {
            "href": "/students",
            "rel": "list",
            "method": "GET"
        },
        {
            "href": "/students",
            "rel": "create",
            "method": "POST"
        }
    ]
}
```
This response contains a **response status code** `200 OK` which indicates the request being successful. For GET method, it means the client has received the resource. The representation of the resource is specified in the `Content-Type` header and follows the dataformat the client requested in the `Accept` header. More importantly, the answer contains links to related resources in the `"links"` section. It is important because this structure allows the client to work with any resources they have the access to without knowing the inner structure of the API. In other words, the client didn't have to know of the `/student` resource being present and how it can be modified or accessed - this information is present in the link entry field via `href, rel, method`fields.
For example, we can list all the present students with an another GET request:

    GET /students HTTP/1.1
    Accept: application/json
   If we have one student in the database with their ID, name and faculty, the response would look like this:
   
    HTTP/1.1 200 OK
        Content-Type: application/json
        
        {
            "students": [
                {
                    "id": 1,
                    "name": "Kate",
                    "faculty: "FICT",
                    "links": [
                        {
                            "href": "/student/1",
                            "rel": "self",
                            "method": "GET"
                        },
                        {
                            "href": "/student/1",
                            "rel": "edit",
                            "method": "PUT"
                        },
                        {
                            "href": "/student/1",
                            "rel": "delete",
                            "method": "DELETE"
                        }
                    ]
                }
	    "links": [
	            {
	                "href": "/student",
	                "rel": "create",
	                "method": "POST"
	            }
Now we can see all the data the students have and the means to change it. 
To change the existing resource instead of adding a new one the **PUT** method is used. In our example, we can change the existing student's data. Let's say our Kate has changed her faculty to FEL. Let's update the data:

    PUT /students/1 HTTP/1.1
    Accept: application/json
    Content-Type: application/json
    
    {
        "name": "Kate",
        "faculty: "FEL"
    }
   The server response would reflect the change:

    HTTP/1.1 200 OK 
    Content-Type: application/json
    
    {
        "student": {
            "id": 1,
            "name": "Kate",
            "faculty: "FICT",
            "links": [
                {
                    "href": "/student/1",
                    "rel": "self",
                    "method": "GET"
                },
                {
                    "href": "/student/1",
                    "rel": "edit",
                    "method": "PUT"
                },
                {
                    "href": "/student/1",
                    "rel": "delete",
                    "method": "DELETE"
                }
            ]
        },
        "links": {
           "href": "/student",
           "rel": "list",
           "method": "GET"
        }
    }
   What if we want to make a new student? To do so our app would use the **POST** method. The POST method creates a new resource instead of updating the old one. For example, let's add a new student:
   

    POST /students HTTP1.1
    Accept: application/json
    Content-Type: application/json
    {
        "name": "Igor",
        "faculty: "FICT"
    }
   The response would contain a new resource with the same fields except "id", that is used as an unique identifier and is automatically updated. To access the new student's entry, we would use the following GET method:
   

    GET /students/2 HTTP/1.1
    Accept: application/json

  Finally, to remove resources the **DELETE** method is used. For example, let's remove our new student from the database:
  

    DELETE /students/2 HTTP/1.1
    Accept: application/json
The response would contain a response status code that would indicate whether the resource has been deleted.

---
Now, as you can see, our app has all the means to get, input, update or delete whatever data from the student database it may need through the RESTful API and simple HTTP requests. This showcases the beauty of the RESTful architecture - simplicity, uniform, efficiency and reliability and the ways it can be used.
