Tags: #maven #devops 
Created: 2022-08-21 19:08
References: [[Introduction to the Dependency Mechanism]]

# Maven Dependency Mechanism
Dependency management is a core feature of [[Maven]]. You only need to specify the dependencies and Maven will deal with getiing them and including them in the final artifact.

Dependencies are transitive, which means that you also depend on your dependencies' dependencies.

You can limit the transitivity of a dependency by specifying its *dependency scope*. There are 6 scopes from which you can chose from (`compile`, `runtime`, `test` etc).