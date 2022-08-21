Tags: #maven #devops 
Created: 2022-08-20 23:08
References: [[Introduction to the Build Lifecycle]]

# Maven Build Lifecycle
[[Maven]] is based around the central concept of a *build lifecycle*, which means that the process for building and distributing a particular project is clearly defined by Maven.

There are three built-in lifecycles in Maven: `default`, `clean` and `site`. All of those build lifecycles are made up out of *phases*. Basically, a lifecycle is a list of phases.
