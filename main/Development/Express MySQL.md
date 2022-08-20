Tags: #db #backend #nodejs 
Created: 2022-08-20 15:08
References: [[Reseting MySQL Password]]

# Express MySQL
This note describes how you can use a [[MySQL]] [[Database]] in [[NodeJS]]. If you are using mysql in nodejs express

```js
const express = require('express');
const mysql = require('mysql');
const cors = require('cors');
const app = express();

app.use(express.json());
app.use(cors());
  
const db = mysql.createConnection({
	user: 'root',
	host: 'localhost',
	password: 'root',
	database: 'loginsystem',
});
```

And you get this when making a query

```js
// code

db.query(
	'INSERT INTO users (username, password) VALUES (?, ?)',
	[username, password],
	(err, res) => {
		console.log(err);
	}
);

// error
Error: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
// ...
```

Run this on your MySQL server (where 'password' is _your password_)

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
flush privileges;
```