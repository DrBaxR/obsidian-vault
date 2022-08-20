---
tags:
 - security
---

A token that only the server can create that contains some serialized information it in (like user information), which is really useful for [[Authorization]].

This token can be sent to the server in a request header in order to authorize user (since even if the user wants to modify the content of the token, like he gives himself admin rights, the server can tell if the token was modified since it was issued).

As long as two servers have the same secrets, jwts are valid on both of them, which means you can do some cool stuff:
-   have a server that has your [[RESTful API]]/whatever
-   have another server that handles the authentication stuff
-   as long as they share the same secret, a token generated on auth server is valid on the [[API]] server