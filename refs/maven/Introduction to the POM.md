Tags: #reference

# Introduction to the POM

- XML file that contains information about the project and configuration details used by maven to build project
- the super POM is Maven's default POM, which all other POMs extend by default
- the minimal POM has to contain the following at least:
```xml
<project>
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example.project</groupId>
	<artifactId>appName</artifactId>
	<version>1</version>
</project>
```
if configuration details are not specified, Maven uses the super POM ones. For example if packaging type is not specified, it will default to jar
- projects can be declared as being the parents of other projects by using the child's `parent` property in the POM. Examples [here](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#example-1)
- project aggregation is similar to project inheritance, the difference being that you specify in the parent's POM all the modules it has. For this you need to change parent's packaging to the value 'pom'. If you run a command against the parent, that command is run against all its modules as well. Examples [here](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#example-3)
- inheritance vs aggregation: if you have several maven projects that share common configs, you can extract those commong configs into a parent and have the projects inherit that parent. If you have projects that are built or processed together, create a parent project and specify the others as its modules; this way you can only build the parent and the rest will follow.
- inheritance and aggregation can be used at once as well: specify in all children who is the parent; change parent's packaging to 'pom'; specify the parent's modules. Examples [here](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#example-3)
- you can use your own and [pre-defined](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html#available-variables) variables in the POM
```xml
<version>${project.version}</version>
```
- these variables are processed AFTER inheritance, which means that if a parent has defined a variable, its child can overwrite the value of that variable

## Resources
https://maven.apache.org/guides/introduction/introduction-to-the-pom.html