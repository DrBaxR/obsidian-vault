Tags: #reference 
Created: 2022-08-27 18:08

# Developing with Containers
## Initial Project
First, we have a simple express application that serves some static `index.html` file
```js
const express = require('express')
const path = require('path')
const app = express()
const port = 3000

app.use('/', express.static(path.join(__dirname, 'public')))

app.listen(port, () => {
  console.log(`Listening on port ${port}`)
})
```

Pull the containers that we need for the databse
```sh
docker pull mongo
docker pull mongo-express
```

## Docker Network
By default, [[Docker]] provides some networking capabilities: the [[Docker Network]] is separate from the rest of the system.

[[Container]]s that are running on the machine can tak to each other only by using each other's names, while other applications running on the system (that are not running in containers) can talk to the applications running in the [[Docker Network]] via the ports that they were bound to.

You can check the networks on the machine with `docker network ls`.

To create a network, run this command with the name you want to assign to the network
```
docker network create mongo-network
```

Create a container from the mongo image, bind the port, set some environment variables for the users of the database, set the container's name and specify the network in which the container lives with this command (more info about mongo container [here](https://hub.docker.com/_/mongo))
```sh
docker run -d \
	-p 27017:27017 \
	-e MONGO_INITDB_ROOT_USERNAME=test \
	-e MONGO_INITDB_ROOT_PASSWORD=test \
	--name mongodb \
	--net mongo-network \
	mongo
```

In order to be able moify the atabase from a UI, use this. It will create a container, bind the port, specify the connection parameters needed for the mongo container, specify the network in which it will live and its name (more info on mongo-express container [here](https://hub.docker.com/_/mongo-express))
```sh
docker run -d \
	-p 8081:8081 \
	-e ME_CONFIG_MONGODB_ADMINUSERNAME=test \
	-e ME_CONFIG_MONGODB_ADMINPASSWORD=test \
	--net mongo-network \
	--name mongo-express \
	-e ME_CONFIG_MONGODB_SERVER=mongodb \
	mongo-express
```

After having these two containers up and running, go to `http://localhost:8081` to check if mongo express container works. If it does, then create a `header` databse, and a `headers` collection in that databse and create a document `{ headerid: 1, text: 'My text from MongoDB' }`.

Once all that is done, change the contents of the `server.js` and `index.html`
`server.js`
```js
const express = require('express')
const path = require('path')
const app = express()
const mongoose = require('mongoose');
const port = 3000

mongoose.connect(
  'mongodb://localhost:27017',
  {
    dbName: 'header',
    authSource: 'admin',
    user: "test",
    pass: "test",
  }
);

const HeaderSchema = new mongoose.Schema({
  headerid: Number,
  text: String,
});

const HeaderModel = mongoose.model('Header', HeaderSchema);

app.use('/', express.static(path.join(__dirname, 'public')))

app.get('/header', (req, res) => {
  HeaderModel.find({ headerid: 1 }, (err, headers) => {
    res.send(headers[0]);
  })
})

app.listen(port, () => {
  console.log(`Listening on port ${port}`)
})
```

`index.html`
```html
<!DOCTYPE html>
<html lang="en">
  
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible"
        content="IE=edge">
  <meta name="viewport"
        content="width=device-width, initial-scale=1.0">
  <title>Docker is kinda cool ngl</title>
</head>

<body>
  <h1 class="header">This is some sample text</h1>
  <script>
    const headerElement = document.querySelector('.header');

    fetch("/header").then(res => {
      if (!res.ok) throw new Error('ERROR!!!');

      return res.json();
    }).then(header => {
      headerElement.innerHTML = header.text;
    });
  </script>
</body>

</html>
```

Now visiting `http://localhost:3000` should show a page with a header set to the text in the database.

If you want to check the logs of a container in real time, you can run this command
```sh
docker logs -f
```

## Resources
[[Docker Tutorial for Beginners]]