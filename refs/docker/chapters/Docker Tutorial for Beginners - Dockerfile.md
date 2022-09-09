Tags: #reference 
Created: 2022-08-28 14:08

# Dockerfile
This section will simulate what the CI tool would do once you commited the code (building the artifacts and creating a container out of it).

A [[Dockerfile]] is a blueprint for creating [[Docker Image]]s. This file can contain multiple commands, some of them being:
- `FROM` - the first thing in a dockerfile, it specifies from which image the current image should start
- `ENV` - set environment variables
- `RUN` - run a command *in the container* (you can have multiple in a dockerfile)
- `COPY` - copy some files *from the current directory* into the *container*
- `CMD` - the last thing in a dockerfile, the *entry point command* (only one per dockerfile)

**Note:** The fact that all the images are based on other images (the `FROM` command needs to be the first thing in a dockerfile) explains the whole *a container is a stack of images* thing in [[Docker Tutorial for Beginners - What is a Container]].

Here's how a simple [[Dockerfile]] for the project from [[Docker Tutorial for Beginners - Developing with Containers]] would look like
```dockerfile
FROM node

RUN mkdir -p /home/app

COPY . /home/app

CMD ["node", "/home/app/server.js"]
```

In order to build a [[Docker Image]] from the project use this command
```sh
docker build -t my-app:1.0 .
```

To delete a container you can use
```sh
docker rm 1aef09ecbb65
```

To delete an image use this
```sh
docker rmi 7bef89ecbb65
```

## Resources
[[Docker Tutorial for Beginners]]