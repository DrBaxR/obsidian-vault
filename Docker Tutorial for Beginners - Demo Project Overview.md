Tags: #reference 
Created: 2022-08-27 17:08

# Demo Project Overview
This section describes how [[Docker]] fits in a development/deployment workflow.

Let's say that you are developing a JavaScript application that depends on a MongoDB database.

In the developing phase, you use Docker to pull an image of MongoDB locally from Docker Hub so you ca use the database.

After you add your feature, you commit your code which triggers some CI chain (let's say Jenkins) which builds the artifacts from your code, uses those artifacts to create a [[Docker Image]] from them and push that docker image to a private repository.

The server on which the changes need to be deployed then pulls that docker image from the private repo AND also pulls the docker image of MongoDB from Docker Hub, on which the application depends

## Resources
[[Docker Tutorial for Beginners]]