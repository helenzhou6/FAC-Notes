# Week Two
_Resource:_ [_week three schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-3)  _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-3/learning-outcomes.md) _and have a look at_ [_the resources_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-3/resources.md).

It's API themed week!

## Day One
### API
_Resources:_
* _Introduction to Request and response pattern, HTTP, XMLHttpRequests, Asynchronous code and fixing a broken API request excercise [here]_(https://github.com/foundersandcoders/api-workshop)
* [_XHR workshop_](https://github.com/foundersandcoders/xhr-workshop) - HTTP GET request to the Giphy API. 
* [_API workshop using GitHub API_](https://github.com/emilyb7/workshop-APIs)

### HTTP
* > The Hypertext Transfer Protocol (HTTP) is the mechanism through which data is requested and provided on the World Wide Web. Protocol: `http://`

    #### HTTP Methods
    * **GET** : 'gets' resources, doesn't modify them. Sent as part of URL.
    * **POST** : sends data to the server. Not displayed in URL
        * > The data values are sent in the body of the request, in the format that the Content-Type header specifies. This way the server knows how to decode the information.
        * > A Content-Length header is also required so that the server knows when it has received all of the information (a large amount of data might be split across multiple requests).
    * **PUT** Places data at exact path specified in request (usually to overwrite something that already exists)
        * > with POST the path specified in the request will handle the data and the server can put the data where it likes. A PUT request is basically a request to update the resource at the specified path with the data provided.
    * **DELETE** the client requests the server to delete a resource at the path specified.
* > When sending an API request, we use a URL to identify the specific server where we want to send the request (kind of like the address on an envelope) and the rest of the url specifies what it is we want the server to send back. These are called parameters or queries.
* > DNS uses the resource name (e.g. www.google.com) to look up the server to which the request is addressed. Anything beyond the resource name is part of the query or filepath the specifies exactly what data or resources are being requested (including an access token for increased rate limit)



#### XMLHttpRequest
* `var xhr = new XMLHttpRequest();` - using the object constructor, `xhr` is a new instance of the object `XMLHttpRequest()` that has all associated methods.
* There can change properties of the object, including `xhr.open` and `xhr.onreadystatechange`
* `xhr.onreadystatechange` - is an event listener for every time the ready state changes (keeps track of the request from **readyState** 0 to 4, and the response back (i.e. the **HTTP status code** received)). So `(xhr.readyState == 4 && xhr.status == 200)` = complete this function if request completed and a [status code](https://github.com/foundersandcoders/api-workshop/blob/master/02-http.md#http-status-codes) has been received and is 'OK' (not an error like the status code 404.
* Finally, use `xhr.send()` to then send the HTTP request.
* For example: 
```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
      document.getElementById("demo").innerHTML =
      xhr.responseText;
    }
};
xhr.open("GET", "xmlhttp_info.txt", true);
xhr.send();
```
* `xhr.responseText` = the response, but it needs to be parsed using: `JSON.parse(xhr.responseText)` so that the response can be accessed like a JS object.