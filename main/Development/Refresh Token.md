---
tags:
 - security
---

These are used in [[Authorization]] flows: You can make [[JWT]]s expire after some time (like 15s) and make clients need to reauthenticate after that time (since their token is no longer valid and they won't be able to make requests) - this part is taken care of by [[JWT]] automatically.

In order to not make users relog every time their token expires, make another token (**refresh token**) which is used at another endpoint (/token) to get a new access token. these refresh tokens do not expire, and you hold in a [[Database]]/cache all the valid ones.

When a request is made at /token, you check if the refresh token is in the cache to see if it is still valid.

To invalidate a refresh token (remove it from the database/cache), a user will need to make a request at another endpoint (/logout).