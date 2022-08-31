Tags: #reference 
Created: 2022-08-31 20:08

# Docker Volumes
The problem with [[Docker]] [[Container]]s is that whenever you stop them and start them again, you lose all your data (which you might have noticed by now) that you stored in them at runtime, since it uses the container's own *virtual* file system, which is an issue in the case of any persistence oriented application (like databases).

The solution are [[Docker Volume]]s, which are basically a way to tell the container to use a specific location on the host's file system to store the data that would be stored on the container's virtual file system.

There are 3 volume types (illustrated with the `docker run` command parameters that you would use):
- **Host volumes**, where you specify how a directory on the host system and map it to a container's directory `-v /home/mount/data:/var/lib/mysql`
- **Anonymous volumes**, where you only specify the directory in the container and docker deals with creating a volume `-v /var/lib/mysql`
- ***Named volumes***, where you assign a name to the volume and specify which path from the container it should deal with `-v name:/var/lib/mysql`. These are the most popular and recommended way to use volumes in production.

If you want to use volumes in [[Docker Compose]], here is how your `.yaml` file should look like:

```yaml
version: '3'
services:
	mongodb:
		image: mongo
		ports:
			- 27017:27017
		volumes:
			- db-data:/var/lib/mysql/data
	mongo-express:
		image: mongo-express
		# ...
volumes:
	db-data
```

As you can see, you need to give the volumes a name and also specify the volumes that you used in the file in the `volume` property. You can also use a volume for multiple containers in this case.

## Resources
[[Docker Tutorial for Beginners]]