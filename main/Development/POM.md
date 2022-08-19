Tags: #java #maven #devops 
Created: 2022-08-19 21:08
References: [[Introduction to the POM]]

# POM
## What is the POM?
The POM is a XML file used by [[Maven]] that contains information about the project and configuration details used to build the project.

All POM files extend a special POM file, called the **super POM**, which is [[Maven]]'s default configuration. The least amout of information that a POM file has to contain is:
- `modelVersion` - set to 4.0.0
- `groupId` - the domain of the organization
- `artifactId` - name of the application/project
- `version` - the version of the artifact
Here is how a **minimal POM** may look like:

```xml
<project>
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example.project</groupId>
	<artifactId>appName</artifactId>
	<version>1</version>
</project>
```

If configuration details are not specified, [[Maven]] uses the super POM ones. For example if *packaging* type is not specified, it will default to _jar_.

## Inheritance
Projects can be declared as being the parents of other projects by using the child's `parent` property in the POM. [Example](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#example-1).

## Aggregation
Project aggregation is similar to project inheritance, the difference being that you specify in the parent's POM all the modules it has, or all the projects it _aggregates_ to.

To achieve this, you need to change parent's packaging to the value `pom`. Once you've done that, if you run a command against the parent, the command is run against all its modules as well. [Example](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#example-3).

## Inheritance vs aggregation
A case where **inheritance** can be used is in case you have several [[Maven]] projects that share common configs, you can extract those common configs into a parent and have the projects inherit that parent.

A case where **aggregation** is useful is if you have projects that are built or processed together. The way you can apply it in this case is you create a parent project and specify the others as being its modules. This way you can only build the parent and the rest will follow.

## Variables
[[Maven]] has the capability to interpolate your own and [pre-defined](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#available-variables) variables in the POM. You can also reference all the properties that exist in the POM, for example, to reference the project's version you can do this:

```xml
<version>${project.version}</version>
```

To create your own variables, you need to specify them in the `properties` element.

**Note:** These variables are processed *after* inheritance, which means that if a parent has defined a variable, its child can overwrite the value of that variable. 