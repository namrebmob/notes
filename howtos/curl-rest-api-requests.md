# Using Curl to make REST API requests

An application program interface (API) is a set of definitions and protocols that allows software programs to communicate with each other.

The term REST stands for representational state transfer. It is an architectural style that consists of a [set of constraints](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) to be used when creating web services.

RESTful API is an API that follows the REST architecture. Typically REST APIs use the HTTP protocol for sending and retrieving data and JSON formatted responses. You can use the standard HTTP methods to create, view, update, or delete resources through the API.

To test and interact with the RESTful APIs, you can use any library or tool that can make HTTP requests.

API requests are made up of four different parts:

The endpoint. This is the URL that the client uses to communicate with the server.
The HTTP method. It tells the server what action the client wants to perform. The most common methods are `GET` `POST` `PUT` `DELETE` and `PATCH`.
The headers. Used to pass additional information between the server and the client, such as authorization.
The body. The data sent to the server.

In this article, we’re going to discuss how to use curl to interact with RESTful APIs. [curl](https://curl.se/docs/) is a command-line utility for transferring data from or to a remote server. It is installed by default on macOS and most Linux distributions.

## Curl Options

The syntax for the curl command is as follows:

`curl [options] [URL...]`

Here are the options that we’ll use when making requests:

`-X`, `--request` - The HTTP method to be used.
`-i`, `--include` - Include the response headers.
`-d`, `--data` - The data to be sent.
`-H`, `--header` - Additional header to be sent.

### HTTP GET

The `GET` method requests a specific resource from the server.

`GET` is the default method when making HTTP requests with curl. Here is an example of making a `GET` request to the JSONPlaceholder API to a JSON representation of all posts:

```sh
API_URL=$API_URL
```

```sh
curl $API_URL
```

To filter the results use query params:

```sh
curl $API_URL?userId=1
```

### HTTP POST

The `POST` method is used to create a resource on the server. If the resource exists, it is overridden.

The following command makes a `POST` request using the data specified with the -d option:

```sh
curl -X POST \
    -d "userId=5&title=Hello World&body=Post body." $API_URL
```

The type of the request body is specified using the `Content-Type` header. By default when this header is not given curl uses `Content-Type: application/x-www-form-urlencoded`.

To send a JSON formatted data set the body type to application/json:

```sh
curl -X POST \
    -H "Content-Type: application/json" \
    -d '{"userId": 5, "title": "Hello World", "body": "Post body."}' $API_URL
```

### HTTP PUT

The `PUT` method is used to update or replace a resource on the server. It replaces all data of the specified resource with the request data.

```sh
curl -X PUT \
    -d "userId=5&title=Hello World&body=Post body." $API_URL/5
```

### HTTP PATCH

The `PUT` method is used to make partial updates to the resource on the server.

```sh
curl -X PUT \
    -d "title=Hello Universe" $API_URL/5
```

### HTTP DELETE

The `DELETE` method removes the specified resource from the server.

```sh
curl -X DELETE $API_URL/5
```

### Authentication

If the API endpoint requires authentication, you’ll need to obtain an access key. Otherwise, the API server will respond with the `“Access Forbidden”` or `“Unauthorized”` response message.

The process of obtaining an access key depends on the API you’re using. Once you have your access token you can send it in the header:

```sh
curl -X GET \
    -H "Authorization: Bearer {ACCESS_TOKEN}" \
    "https://api.server.io/posts"`
```
