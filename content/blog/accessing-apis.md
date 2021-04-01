---
title: Accessing APIs
kind: article
created_at: 2014-03-02
---

I was curious about all the great sites I know of releasing their API. Twitter, Google, Facebook, Yelp, are only a few examples of websites providing a way to access their services in a programmatic way. In this post, I have investigated a simple but very interesting API, offered by [Rotten Tomatoes](http://rottentomatoes.com "Rotten Tomatoes"), a website about movies.

<!-- more -->

### Before Coding
Apparently, most if not all the websites offering API access to their services, require you to register as a developer, in order to provide you with an API key. The registration form for Rotten Tomatoes can be found, for example, at the [Developers' page](http://bit.ly/1eoIiOB "Rotten Tomatoes API").  Once you register an account and request an API key, you can use it to access the API. This kind of accountability is important, because the damage you can do by making tons of requests to a website can easily become an example of DoS attacks. The number of requests is limited, as well (5 per seconds and 10,000 per day).

### The POCO Libraries
In order to more easily manipulate HTTP requests and response, I have tried out the [POCO libraries](http://pocoproject.org/ "POCO libraries"), a nice set of wrappers around extremely useful features, that range from Base64 encoding/decoding to socket programming, to XML manipulation, to DBMS interactions. The simple code I provide here uses the *Net* classes, that offer useful abstractions like `HTTPRequest`, `HTTPResponse`, `HTMLForm`, and the likes.

~~~ cpp
int main()
{
  try {
    HTTPRequest movierequest(HTTPRequest::HTTP_GET,
        "/api/public/v1.0/movies.json",
        HTTPMessage::HTTP_1_1);
    HTMLForm form;
    form.add("apikey", "{your_api_key}"); // use your API key here!!
    form.add("page", "1");
    form.add("page_limit", "10");
    form.add("q", "Toy+Story");
    form.prepareSubmit(movierequest);

    HTTPClientSession s("api.rottentomatoes.com");
    s.sendRequest(movierequest);
    HTTPResponse resp;
    istream& response_stream = s.receiveResponse(resp);
    cout << resp.getStatus() << " " << resp.getReason() << endl;
    ofstream result("toystory.txt");
    Poco::StreamCopier::copyStream(response_stream, result);
  } catch (Poco::Exception& ex) {
    cerr << "Exception: " << ex.className() << " -- " <<
      ex.displayText() << endl;
    return -1;
  }
  return 0;
}
~~~

Let's examine this code from closer. The complete source code is provided as a [Github gist](https://gist.github.com/sturmer/8755127).

### Making a GET request
Let's dissect the code presented above.

~~~ cpp
HTTPRequest movierequest(HTTPRequest::HTTP_GET,
    "/api/public/v1.0/movies.json",
    HTTPMessage::HTTP_1_1);
~~~

We make an `HTTP GET` request in order to retrieve data from the web service by passing the proper parameters in a query string. The POCO libraries give us access to an `HTTPRequest`, that is constructed by passing it the HTTP method, the last part of the query url, and the version of HTTP, for which two enum values are provided, `HTTP_1_0` and `HTTP_1_1`, for versions 1.0 and 1.1.


~~~ cpp
HTMLForm form;
form.add("apikey", "{your_api_key}");
form.add("page", "1");
form.add("page_limit", "10");
form.add("q", "Toy+Story");
form.prepareSubmit(movierequest);
~~~

The `HTMLForm` is useful for passing parameters to the request. This
is just done by providing key-value pairs for each relevant parameter. As you
see, when we are done, we call a `prepareSubmit()` method to pass
the values to the request object.

### Calling the Server, Reading the Response
In order to call the API service, we need to instantiate a `HTTPClientSession` object, that is initialized with the name of the host providing the service.

~~~ cpp
HTTPClientSession s("api.rottentomatoes.com");
s.sendRequest(movierequest);
~~~

The request is sent by calling the method `sendRequest()`. It is interesting that, in case we wanted to pass a body (for example in a POST request), we could have simply called the get `operator<<()`, like `s.sendRequest() << req_body;`.

~~~ cpp
HTTPResponse resp;
istream& response_stream = s.receiveResponse(resp);
cout << resp.getStatus() << " " << resp.getReason() << endl;
ofstream result("toystory.txt");
Poco::StreamCopier::copyStream(response_stream, result);
~~~

Finally, we want to get the response. We instantiate an object of type `HTTPResponse`, not surprisingly, and then call the `receiveResponse()` on the client-initiated session object. The `StreamCopier` is useful to print the result into an output stream, that we can then parse at will.

The resulting file looks something like the following:


~~~ javascript
{
    "total" : 3,
    "movies" : [
        {"id" : "770672122",
        "title" : "Toy Story 3",
        "year" : 2010,
        "mpaa_rating" : "G",
        "runtime" : 103,
        "critics_consensus" : "Deftly blending comedy, adventure, and honest emotion...",
        "release_dates" : {
            "theater" : "2010-06-18",
            "dvd" : "2010-11-02"},
        "ratings" : ...
~~~

### Conclusions
The POCO libraries have been an interesting surprise. They are very well documented, and simplify several tasks which prove fundamental when dealing with the network.

I have tried also to access the Twitter API, but that deserves a post apart, because I had some problems with the authentication that is more sophisticated than Rotten Tomatoes', also because of the huge amount of data that you can query. A great tool for debugging your programs can be found at the [IO Docs page](http://developer.rottentomatoes.com/io-docs), that generates the request string according to the parameter provided in a form, executes the request, and finally displays the result; a similar tool is present also for Twitter.
