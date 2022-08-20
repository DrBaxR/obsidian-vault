Tags: #reference 
Created: 2022-08-20 20:08

# Introduction to Archetypes

Archetype is a [[Maven]] project templating *toolkit*. An archetype is a *model from which all other things of the same kind are made*.

Archetypes can be used to create templates of projects and parametrized versions of those project templates. They are also a great way to get started with a new project type as quickly as possible and a way to provide consistent best practices.

Preople that made Maven also tried to make this whole system **additive**, which basically means that you can use an archetype in an already existing project: create a project using the *maven quickstart archetype* and if you want the site functionality, add the *site archetype* the the project you created.

To create a project using an archetype, use the command:
```sh
mvn archetype:generate
```

## Resources
https://maven.apache.org/guides/introduction/introduction-to-archetypes.html