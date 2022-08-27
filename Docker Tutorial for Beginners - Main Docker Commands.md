Tags: #reference 
Created: 2022-08-27 16:08

# Main Docker Commands
### Container vs Image
A container is a **running environment** for an image. Is has its own things like a file system, application image and a port bound to it, so you can talk to the application that is running inside it.

This command is used to pull a Docker image locally (a Redis image)
```sh
docker pull redis
```

This command is used to see what images are installed locally
```sh
docker images
```

Create a container where you will run the (Redis) image in the attached mode
```sh
docker run redis
```
To start a container in detached mode, use the `-d` parameter, which will start the container in the background
```sh
docker run -d redis
```
You can also run a different version of the image by soecifying its tag
```sh
docker run redis:4.0
```

Check what containers are running on the machine and some imformation about them
```sh
docker ps
```
In order to see *all* the containers (either running or not running), you can use the `-a` parameter
```sh
docker ps -a
```

To stop a container, you can use this command with the ID of the container (which can be obtained from the `docker ps` command)
```sh
docker stop 70284bf65174
```

To start the same container that you just stopped, you can use this command with the same ID of the container you just stopped
```sh
docker start 70284bf65174
```

### Ports
Containers have different (their own) ports from the **host**, since containers run on the nost. In order to make the applications accessible from outside of the host, the ports of the *container* in which the application is running needs to be **bound**
to one of the *host*'s ports.

In order to bind a host port to the container's port, you need to specify it in the `run` command by using the parameter `-p[host]:[container]`, which when running `docker ps` will reveal the mapping like `0.0.0.0:6000->6379/tcp`
```sh
docker run -d -p6000:6379 redis
```

## Resources
[[Docker Tutorial for Beginners]]