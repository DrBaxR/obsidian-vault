Tags: #reference 
Created: 2022-08-27 16:08

# What is a Container?
A container is made up of **layers** of [[Docker Image]]s that are mostly [[Linux]] **base images** (because they are small in size and therefore portable).

Besides the base image, the image stack also contains a layer called the **application image**, which is the actual application that runs on the base image. *This is a simplified explaination, it actually contains more intermediary images between the base layer and the application layer*.

### Example
To run Postgres v9.6 with Docker, you can use the command (ignore the `-e` parameter)
```sh
docker run -e POSTGRES_PASSWORD=test postgres:9.6
```

This will download the container from Docker Hub and start it on your machine. You can check that is it running by using
```sh
docker ps
```

### Image vs Container
A [[Docker Image]] is the application artifact that can be moved around (it is not running) and a [[Container]] is what gets created once you **run** the image on your machine.

## Resources
[[Docker Tutorial for Beginners]]