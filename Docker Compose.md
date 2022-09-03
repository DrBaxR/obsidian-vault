Tags: #devops 
Created: 2022-09-03 15:09
References: [[Docker Tutorial for Beginners - Docker Compose]]

# Docker Compose
[[Docker]] compose is a tool that comes with docker that makes wirking with multiple docker [[Container]]s much simpler.

Since starting containers is done by commands that can get pretty lengthy it can get pretty tediour to start many containers each time you want to use them in a project. For this reason **docker compose** allows you to transform those commands in a [[YAML]] file that contains all the information used in them, which can then be used in the `docker-compose` command.

As an added benefit, the `docker-compose` command also puth all the containers in the same network, which it also creates on startup and destroys on stopping.