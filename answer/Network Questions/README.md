
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
* Explain the following request and response headers:
  * Diff. between Expires, Date, Age and If-Modified-...
  * Do Not Track
  * Cache-Control
  * Transfer-Encoding
  * ETag
  * X-Frame-Options
* What are HTTP actions? List all HTTP actions that you know, and explain them.
