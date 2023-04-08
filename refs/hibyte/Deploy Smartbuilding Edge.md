## Introduction
This document assumes that the Smartbuilding Simulator is already deployes in a docker container and that the container is part of an **ipvlan** docker netowrk called *smvlan*.

## Build Image
The `Dockerfile` for the project looks like this:

```dockerfile
FROM tomcat:8.5.83-jdk11-corretto-al2  
COPY target/smartbuilding-edge-0.0.3.war /usr/local/tomcat/webapps  
RUN mkdir -p /opt/tomcat/conf  
RUN mkdir -p /home/data  
EXPOSE 8080  
CMD ["catalina.sh", "run"]
```

In order to build a new image you need to have run `mvn clean install` on the **Smartbuilding Shared Lib** and **Smartbuilding Edge** projects (in this order).

## Deployment
After having built the image, you need to tag it and push it to the repository:

```sh
docker build -t edge --no-cache .
docker tag edge docker.hq-hydra.hibyte.ro/hibyte/smartbuilding-edge:edgev1
docker push docker.hq-hydra.hibyte.ro/hibyte/smartbuilding-edge:edgev1
```

After having pushed the image, you need to switch to the server and pull the image:

```sh
docker pull docker.hq-hydra.hibyte.ro/hibyte/smartbuilding-edge:edgev1
```

Finally, you can run the container:

```sh
docker run \
	-d \
	-v /root/config/smartbuilding.properties:/opt/tomcat/conf/smartbuilding.properties \
	-v /root/config/data/vSensor2.json:/home/data/sensors.json \
	--network smvlan docker.hq-hydra.hibyte.ro/hibyte/smartbuilding-edge:edgev1
```

You need to make sure that the edge container is in the same *ipvlan* network as the simulator containers. The two paths that you can attach volumes to are:
1. `/opt/tomcat/conf/smartbuilding.properties` - the properties file
2. `/home/data/sensors.json` - the virtual sensors file

Remember to set `mqtt.broker.hostUrl` and `vsensor.json.file` in `smartbuilding.properties`.