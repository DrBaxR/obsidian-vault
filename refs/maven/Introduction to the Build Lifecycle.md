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

`TODO: ...`

## Resources
https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html