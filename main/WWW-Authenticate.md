Tags: #security 
Created: 2022-11-12 18:11
References: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/WWW-Authenticate

# WWW-Authenticate
This is a [[HTTP]] header that is attached to a response that has a status of `401 Unauthorized`. This header's value contains at least one challenge, to indicate what authentication schemes can be used to access the requested resource.

After receiving the `WWW-Authenticate` header, a client will typically prompt the user for credentials, and then re-request the resource. This new request uses the `Authorization` header to supply the credentials to the server, encoded appropriately for the selected "challenge" authentication method.