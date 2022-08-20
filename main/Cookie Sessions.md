Tags: #nodejs #security 
Created: 2022-08-20 15:08
References: 

# Cookie Sessions in NodeJS
This note contains informations regarding how you can use [[NodeJS]] to implement [[Session Authentication]].

## Backend
You'll need some extra packages: express-session, body-parser, cookie-parser:

```sh
npm install express-session body-parser cookie-parser
```

```js
// import
const bodyParser = require('body-parser');
const cookieParser = require('cookie-parser');
const session = require('express-session');

// config
// cors
app.use(cors({
	origin: ['http://localhost:3000'],
	methods: ['GET', 'POST'],
	credentials: true
}));
// other stuff
app.use(cookieParser());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(session({
	key: 'userId',
	secret: 'some_secret',
	resave: false,
	saveUninitialized: false,
	cookie: {
		maxAge: 10000,
	},
}))
```

When user makes a request to log in, backend creates a session and saved whatever data it wants to save before sending a response to the login request

```js
req.session.user = result[0]
console.log('session created...')
res.send(result[0]);
```

Make another endpoint in the backend where clients can send requests to check if they are logged in (if session is still valid) via the [[Cookie]] that they got from the backend

```js
app.get('/login', (req, res) => {
	if (req.session.user) {
		res.send({ loggedIn: true, user: req.session.user });
	} else {
		res.send({ loggedIn: false });
	}
})
```

## Frontend
Config Axios to work with cookies

```js
Axios.defaults.withCredentials = true;
```

Send request at the *session endpoint* if page reloaded to check if user is logged in

```js
useEffect(() => {
	Axios.get('http://localhost:3001/login').then(res => {
		if (res.data.loggedIn) {
			setLoginStatus(`${res.data.user.username}'s session maintained`)
		}
	})
}, [])
```