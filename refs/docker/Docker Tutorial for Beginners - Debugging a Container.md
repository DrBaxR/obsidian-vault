Tags: #reference 
Created: 2022-08-27 17:08

# Debugging a Container
In order to see the logs of a [[Container]] (in case the application does not behave as it should or smt), you can use this command with the ID of the container:
```sh
docker logs 0b2d4e208098
```

It is also possible to name a container however you want by using the `--name` parameter with the `run` command
```sh
docker run -d -p6000:6379 --name redis-container redis
```

This name can later be used instead of the ID in other commands like `logs`.

If you want to get inside the container via a terminal, you can use the following command with the ID of the container of its name
```sh
docker exec -it redis-container /bin/bash
```

## `run` vs `start`
The difference between those two commands are that `docker run` **creates a new container from an image** and `docker start` only **starts an already existing container**.

## Resources
[[Docker Tutorial for Beginners]]