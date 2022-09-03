Tags: #devops 
Created: 2022-09-03 14:09
References: [[Docker Tutorial for Beginners - Developing with Containers]]

# Docker Network
By default, [[Docker]] provides some networking capabilities: the **docker network** is separate from the rest of the system.

Docker [[Container]]s that are running on the machine and that are living in the same *docker network* can reference each other only by using the name given to them.

For example, if you have a container that runs on the machine called `test-container` and has its `8080` port exposed, another container living inside the same *docker network* will address that container with `test-container` instead of `localhost:8080`.