Tags: #devops 
Created: 2022-09-03 15:09
References: [[Docker Tutorial for Beginners - Docker Volumes]]

# Docker Volume
Once you stop [[Docker]] [[Container]]s, all the data stored in them is lost, which is really annoying especially for persistance containers (like [[Database]]s).

The reason why this happens is because containers have their own virtual file systems, which get wiped once containers are stopped.

**Docker volumes** are the solution to this issue. They are basically mounting (or linking) a location in the host machine to a location in the container. This means that any operations in the locations linked are visible in both container and host.