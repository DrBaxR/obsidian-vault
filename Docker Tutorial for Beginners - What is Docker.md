Tags: #reference 
Created: 2022-08-27 16:08

# What is Docker?
A [[Container]] is a way to package an application and all of its dependencies in a single place, which is **portable**.

A [[Container Repository]] is a place where containers can be stored (this is where containers *live*).

### Setup Improvements
Before containers, developing an application across a team of developers would mean that each of the developers would have to manually install all of the services that the application uses manually on their machine (stuff like Postgres, Redis etc.), which can be a tedious process and can lead to slowing down the process of development, since the installation process is error prone and OS specific.

After containers, the process of setting up a project is much more straight forward, since all of the dependencies that the project uses are stored in their own container (that contains the actual service, the its configuration and the start script), which means that you no longer need to actually install the service locally. *All you need to do is to get the container and run it on your machine to get the service that your application depends on*.

### Deployment Improvements
Before containers, the flow of deployment of an application would look something like this:
- the development team would develop a new version of an application
- the development team would need to instruct the operations team on what things are needed to deploy the application (things like what server is needed to run the application, what configuration is needed to be done in order to start the application)
- the operations team would then need to first set up the deployment environment like the development team instructed them to (**error prone**: setting up is OS dependent and may lead to errors) and then need to follow the steps of configuring and deploying the application to the environment (**error prone**: there may be communication issues between the two teams and the process itself may lead to errors)

After containers, the process is much more straight forward, since the developers and the operations teams are actually merged in a single group (the *devops team*). This new team works to develop and **package** the application into a container, which is then stored to a repository. After this is done, deploying the container is really simple, since all the team has to do is pull the container on the server and run it, no setup needed on the server (except the *initial effort* of installing the [[Docker]] runtime of the server).

## Resources
[[Docker Tutorial for Beginners]]