Tags: #reference 
Created: 2022-08-20 21:08

# Introduction to the Build Lifecycle
## Basics
[[Maven]] is based around the central concept of a [[Maven Build Lifecycle]], which means that the process for building and distributing a particular artifact (project) is clearly defined by Maven.

Maven has 3 built-in lifecycles:
- `default` - handles project deployment
- `clean` - handles project cleaning
- `site` - hanles creation of the project's website

### Phases
All build lifecycles are basically a *list of phases*. A phase is a stage of a lifecycle. For example here are the [[Maven Default Lifecycle Phases]].

The phases of a lifecycle are run sequentially once you run the lifecycle.

### Plugin Goals
Even though a build phase is responsible of a step in the build process, the manner in which it carries out those responsibilities may vary. This is done by declaring the *plugin goals* ([[Maven Goal]]) bound to those phases.

A plugin goal represents a task, which is finer than a build phase, that contributes to building/managing the project.

If a goal is bound to one or more build phases, it will be called in all those phases. If a build phase has no goals bound to it, then it will not get executed.

## Setting up your project to use Build Lifecycle
Them most common thing to do is set up packaging of the project via the `<packaging>` element in the [[POM]]. This actually binds goals to build phases, the actial bindings can be found in the resources.

### Plugins
Plugins are artifacts that provide goals that can be run as part of your build. Adding a plugin in the [[POM]] is not enough information - you must also specify the goals you want to integrate into your build.

You can specify which goals of a plugin you want to execute in the `<executions>` element.

Some goals can only be executed during a phase, for which you do not need to specify the phase. 
```xml
<plugin>
<groupId>org.codehaus.modello</groupId>
<artifactId>modello-maven-plugin</artifactId>
<version>1.8.1</version>
<executions>
 <execution>
   <configuration>
	 <models>
	   <model>src/main/mdo/maven.mdo</model>
	 </models>
	 <version>4.0.0</version>
   </configuration>
   <goals>
	 <goal>java</goal>
   </goals>
 </execution>
</executions>
</plugin>
```

However, other goals could be used in many phases. For those you need to manually specify the phase in which you want to use in the `<phase>` element.
```xml
<plugin>
<groupId>com.mycompany.example</groupId>
<artifactId>display-maven-plugin</artifactId>
<version>1.0</version>
<executions>
 <execution>
   <phase>process-test-resources</phase>
   <goals>
	 <goal>time</goal>
   </goals>
 </execution>
</executions>
</plugin>
```

## Resources
https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html