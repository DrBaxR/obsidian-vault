---
tags:
 - js
 - security
 - nodejs
---

This note describes how you can implement an [[Authentication]] [[Back End]] that uses [[JWT]]s to authenticate users.

First off, you need to generate some secrets that are going to be used to generate and validate tokens. To to this in [[NodeJS]], you can simply use the **crypto** library:

```sh
> require('crypto').randomBytes(64).toString('hex')
'33a972acfa94aab156c5312fd13e178c3fef2f2e504cef3446cae66f152cac74d1409210c2ec11f74f47ac3a7aa79977afbeaf0aa7bd499c97c6a878e879174f'
```

In order to store secrets or other sensible information in [[NodeJS]], you can create a _.env_ file and use **dotenv** library to read them from there. A _.env_ file contains key value pairs:

```
ACCESS_TOKEN_SECRET=33a972acfa94aab156c5312fd13e178c3fef2f2e504cef3446cae66f152cac74d1409210c2ec11f74f47ac3a7aa79977afbeaf0aa7bd499c97c6a878e879174f

REFRESH_TOKEN_SECRET=208fdb377a7d25f00c082927a9c838185b4bdc7237aabc4bd81e2b5067f7f602dff56aadcfe3a8d6383b3e2ee267732d528a3336e5bf149c3ccaf67490a70f92
```

To access the contents of the file, you can do this:

```js
require('dotenv').config();

const mySecret = process.env.ACCESS_TOKEN_SECRET
```

To create a [[JWT]] **jsonwebtoken** library can be used:

```js
const jwt = require('jsonwebtoken');
// ...
const user = { name: username };
jwt.sign(user, process.env.ACCESS_TOKEN_SECRET);
```

An [[OAuth]] flow usually also has a [[Refresh Token]] besides the [[Access Token]]. [[JWT]]s can be used as both of those.

So, a flow of authentication would be (based on the example in the video)
1.  User logs in and gets an accessToken and refreshToken
2.  User makes requests with accessToken until it expires
3.  When accessToken expires, user gets a new one from /token by using refreshToken
4.  To invalidate refreshToken, user makes request at /logout

### Resources
[https://youtu.be/mbsmsi7l3r4](https://youtu.be/mbsmsi7l3r4)
[https://github.com/DrBaxR/node-jwt-authentication](https://github.com/DrBaxR/node-jwt-authentication)