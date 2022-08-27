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
	-e ME_CONFIG_MONGODB_SERVER \
	mongo-express
```

## Resources
[[Docker Tutorial for Beginners]]