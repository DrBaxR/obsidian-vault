Tags: #reference 
Created: 2022-09-01 19:09

# Volumes Demo
In order to create a named [[Docker Volume]] via [[Docker Compose]], you can do something like this: create a `volumes` property on the same level as `services` where you specify the name of the volume and the driver (`local` means that it's on the host machine).

```yaml
version: '3.9'
services:
  mongodb:
    image: mongo
    # ...
    volumes:
      - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    # ...
volumes:
  mongo-data:
    driver: local
```

Now you can start the containers via the same old command `docker-compose -f ./mongo.yaml up`, and if you do some changes to the database, stop the containers and start them again you'll see that the database was not wiped.

Volumes can be found at a specific location in the file system: `/var/lib/docker/volumes`, which is where you can delete them if you no longer need them.

## Resources
[[Docker Tutorial for Beginners]]