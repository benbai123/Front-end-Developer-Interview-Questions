
#### Network Questions:

* Traditionally, why has it been better to serve site assets from multiple domains?
  * Ans: To improve concurrent downloading number, since browsers restrict the number of simultaneous downloads that can take place.
  * Ref: [Performance Research, Part 4: Maximizing Parallel Downloads in the Carpool Lane](http://yuiblog.com/blog/2007/04/11/performance-research-part-4/)
* Do your best to describe the process from the time you type in a website's URL to it finishing loading on your screen.
  * Ans: 
    * browser checks cache; if requested object is in cache and is fresh, skip to #9
    * browser asks OS for server's IP address
    * OS makes a DNS lookup and replies the IP address to the browser
    * browser opens a TCP connection to server (this step is much more complex with HTTPS)
    * browser sends the HTTP request through TCP connection
    * browser receives HTTP response and may close the TCP connection, or reuse it for another request
    * browser checks if the response is a redirect or a conditional response (3xx result status codes), authorization request (401), error (4xx and 5xx), etc.; these are handled differently from normal responses (2xx)
    * if cacheable, response is stored in cache
    * (#9) browser decodes response (e.g. if it's gzipped)
    * browser determines what to do with response (e.g. is it a HTML page, is it an image, is it a sound clip?)
    * browser renders response, or offers a download dialog for unrecognized types
  * Ref: [what happens when you type in a URL in browser](http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser)
* What are the differences between Long-Polling, Websockets and Server-Sent Events?
  * Ans: 
    * Connection:
      * Long-Polling: request -> wait -> response, the traditional http request like AJAX with longer timeout settings, during connection open client can receive data from server.
      * Websockets: client <-> server. Create TCP connection to server, and keep it as long as needed. Server or client can easily close it. It is very efficient if application requires frequent data exchange in both ways.
      * Server-Sent Events: client <- server. Client establishes persistent and long-term connection to server. Only server can send data to client.
    * Cross Domain:
      * Long-Polling: Can be cross domain.
      * Websockets: Can be cross domain.
      * Server-Sent Events: Need to be same domain, some browser do not support CORS with SSE.
    * Browser Support:
      * Long-Polling: Supported in all major browsers..
      * Websockets: Supported by modern browsers [Can I Use WebSocket](http://caniuse.com/#feat=websockets).
      * Server-Sent Events: Supported by modern browsers except IE/Edge [Can I Use SSE](http://caniuse.com/#feat=eventsource).
  * Ref:
    * [HTML5 WebSocket vs Long Polling vs AJAX vs WebRTC vs Server-Sent Events](http://stackoverflow.com/questions/10028770/html5-websocket-vs-long-polling-vs-ajax-vs-webrtc-vs-server-sent-events)
    * [Server-Sent Events at w3.org](https://www.w3.org/TR/2011/WD-eventsource-20110208/#iana-considerations)
    * [Web sockets make ajax/CORS obsolete?](http://stackoverflow.com/questions/4042691/web-sockets-make-ajax-cors-obsolete)
* Explain the following request and response headers:
  * Questions
    * Diff. between Expires, Date, Age and If-Modified-...
    * Do Not Track
    * Cache-Control
    * Transfer-Encoding
    * ETag
    * X-Frame-Options
  * Ans: First of all, I am soooo glad there is a wiki listing all headers...
    * Diff. between Expires, Date, Age and If-Modified-... 
      * From Document:
        * Expires - Gives the date/time after which the response is considered stale (i.e., need to request it again instead of use the cache).
        * Date - The date and time that the message was sent
        * Age - The age the object has been in a proxy cache in seconds, conveys the sender's estimate of the amount of time since the response was generated or successfully validated at the origin server
        * If-Modified-Since - Allows a 304 Not Modified to be returned if content is unchanged. The "If-Modified-Since" header field makes a GET or HEAD request method conditional on the selected representation's modification date being more recent than the date provided in the field-value. Transfer of the selected representation's data is avoided if that data has not changed.
        * By the way, If-Modified-Since is Request fields, others are Response fields
      * More General:
        * Expires - Denotes when should you fetch new data instead of use cache.
        * Date - Denotes the time that the data is generated
        * Age - Denotes how long does the data is cached
        * If-Modified-Since - Ask server just tell you nothing changed if nothing modified since the given time.
    * Do Not Track: Requests a web application to disable their tracking of a user.
    * Cache-Control: Response header. Tells all caching mechanisms from server to client whether they may cache this object. It is measured in seconds.
    * Transfer-Encoding: Response header. The form of encoding used to safely transfer the entity to the user. Currently defined methods are: chunked, compress, deflate, gzip, identity.
    * ETag: Response header. An identifier for a specific version of a resource, often a message digest.
    * X-Frame-Options: Response header. Clickjacking protection: deny - no rendering within a frame, sameorigin - no rendering if origin mismatch, allow-from - allow from specified location, allowall - non-standard, allow from any location.
  * Ref:
    * [List of HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
    * [Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://tools.ietf.org/html/rfc7231)
* What are HTTP actions? List all HTTP actions that you know, and explain them.
  * Ans (cheating): 
    * GET: The GET method requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect. (This is also true of some other HTTP methods.) The W3C has published guidance principles on this distinction, saying, "Web application design should be informed by the above principles, but also by the relevant limitations." See safe methods below.
    * HEAD: The HEAD method asks for a response identical to that of a GET request, but without the response body. This is useful for retrieving meta-information written in response headers, without having to transport the entire content.
    * POST: The POST method requests that the server accept the entity enclosed in the request as a new subordinate of the web resource identified by the URI. The data POSTed might be, for example, an annotation for existing resources; a message for a bulletin board, newsgroup, mailing list, or comment thread; a block of data that is the result of submitting a web form to a data-handling process; or an item to add to a database.
    * PUT: The PUT method requests that the enclosed entity be stored under the supplied URI. If the URI refers to an already existing resource, it is modified; if the URI does not point to an existing resource, then the server can create the resource with that URI.
    * DELETE: The DELETE method deletes the specified resource.
    * TRACE: The TRACE method echoes the received request so that a client can see what (if any) changes or additions have been made by intermediate servers.
    * OPTIONS: The OPTIONS method returns the HTTP methods that the server supports for the specified URL. This can be used to check the functionality of a web server by requesting '*' instead of a specific resource.
    * CONNECT: The CONNECT method converts the request connection to a transparent TCP/IP tunnel, usually to facilitate SSL-encrypted communication (HTTPS) through an unencrypted HTTP proxy.
    * PATCH: The PATCH method applies partial modifications to a resource.
  * Ref: [Hypertext Transfer Protocol - ](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)