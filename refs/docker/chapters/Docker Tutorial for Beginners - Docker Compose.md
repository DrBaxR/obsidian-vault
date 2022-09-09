Tags: #reference 
Created: 2022-08-28 13:08

# Docker Compose
The [[Docker Compose]] is a [[YAML]] file that instructs docker how to create multiple containers. This file does not have to contain information regarding the network in which the containers will live, since the Docker compose will automaticallt create a network in which the containers will be automatically added.

Here is the docker compose for the `docker run` commands in [[Docker Tutorial for Beginners - Developing with Containers]]. This file contains 2 services: one for the mongo image and one for the mongo-express image. The mongo-express service has its `restart` property set to `always` because it depends on the mongo service (it needs to have it running to be running). This property makes the container restart if it fails during startup.

`mongo.yaml`
```yaml
version: '3.9'
services:
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

In order use this file, you need to run the `docker-compose` command which will also create the network in which the two services will live (it can also be run with the `-d` parameter)
```sh
docker-compose -f mongo.yaml up
```

This is how you can stop the containers with one command. This will also remove the network that the `up` command created
```sh
docker-compose -f mongo.yaml down
```

## Resources
[[Docker Tutorial for Beginners]]