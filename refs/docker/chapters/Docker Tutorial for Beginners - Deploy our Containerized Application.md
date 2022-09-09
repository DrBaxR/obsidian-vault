Tags: #reference 
Created: 2022-08-28 19:08

# Deploy our Containerized Application
In order to deploy, on a server, you first need to log in with `docker login` on that server, so you can pull the privare image that was created.

The deployment process basically consists of having a [[Docker Compose]] file on the server where you want to deploy your stuff which contains your application AND all the other services on which it depends on.

```yaml
version: '3.9'
services:
  my-app:
    image: my-app:1.0
    ports: 3000:3000
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=test
      - MONGO_INITDB_ROOT_PASSWORD=test
  mongo-express:
    restart: always
    image: mongo-express
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=test
      - ME_CONFIG_MONGODB_ADMINPASSWORD=test
      - ME_CONFIG_MONGODB_SERVER=mongodb
```

Before being able to run the `docker-compose` command to start all the containers, you need to modify the database URL in the `server.js` file, since the server will be run in a container that lives in the same network with the database

```js
mongoose.connect(
    'mongodb://mongodb',
    {
      dbName: 'header',
      authSource: 'admin',
      user: "test",
      pass: "test",
    }
)
```

So, as a recap, the flow of deploying a new version of the application would look something like this:
1. access the server where you want to deploy the new version
2. login to the private repo where the image of the application is via `docker login`
3. create a `docker-compose.yaml` file with all the containers that you need
4. (optional, if you haven't done it) change the connection URLs in the application so they work in a docker network
5. run `docker-compose -f ./docker-compose.yaml up`

## Resources
[[Docker Tutorial for Beginners]]