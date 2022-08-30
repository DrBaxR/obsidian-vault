Tags: #reference 
Created: 2022-08-30 15:08

# Cross-Origin resource sharing (CORS)
The **same-origin policy** is a security policy enforced by client-side web apps (like browsers) that prevents interactions between resources from different resources. This policy also blocks *legitimate interactions between known origins*, which is bad.

For this reason [[CORS]] exists - to get around this limitation.

## How it works
There are two types of requests: **simple** and **preflighted**. Simple requests can be initiated directly, while preflighted requests MUST send a preliminary **preflight requests**, which is used to request permission.

A request is preflighted if any of these are true:
- is something other than `GET`, `HEAD` or `POST`
- it uses `POST` with a `Content-Type` other than `text/plain`, `application/x-www-form-urlencoded`, or `multipart/form-data`
- it sets custom headers

In case a *simple request* is being sent, this process occurs:
1. Browser adds the `Origin` header to the request, which contains the origin place that wishes to access the resource. Example: `Origin:https://www.example.appspot.com`
2. Server compares the *[[HTTP]] method* and the *`Origin` header* of the request with their respective values that it has set as *allowed* to check if there are matches. If there are, the server includes the `Access-Control-Allow-Origin` header in its response, which contains the value of the `Origin` in the original request
3. Browser receives response and checks if the `Access-Control-Allow-Origin` matches the domain specified in the original request. If they match it's all good, if they don't or the header is not even present, the request *fails*.

For *preflighted requests*:
1. Browser sends `OPTIONS` request containing headers `Requested Method` and `Requested Headers` of the primary request.
2. Server responds back with the HTTP methods and headers that are allowed by the origin. If any of the methods or headers in the preflight aren't in the set received from the server, request fails AND the ***primary request is not sent***.


## Resources
https://cloud.google.com/storage/docs/cross-origin