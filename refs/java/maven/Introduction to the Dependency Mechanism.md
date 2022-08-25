Tags: #reference 
Created: 2022-08-21 18:08

# Introduction to the Dependency Mechanism
Dependency management is a core feature of [[Maven]].

## Transitive Dependencies
Maven automatically adds the dependencies of your depenedncies to *your dependencies*. This means that if you have a dependency `A` that has another dependency `B`, then `B` will automatically be added to your dependencies.

There is no limit to the levels of depth of the dependency graph, the only issue that may happen are **cyclic dependencies**.

Since the dependencies graph can grow very large with this transitivity, there are other features that limit the included dependencies:
- *Dependency Mediation* - Maven picks the **nearest definition** of a dependency when multiple versions of that dependency are encountered.
```
	A
	├── B
	│   └── C
	│       └── D 2.0
	└── E
	  └── D 1.0
```
In the case of a project like above, `D 1.0` will be used when building, because `A -> E -> D 1.0`  is shorter than `A -> B -> C -> D 2.0`.
- *Dependency Management* - this allows authors to manually specify which dependencies are to be used in case they are encountered in a transitive scenario, as specified above.
```
  A
  ├── B
  │   └── C
  │       └── D 2.0
  ├── E
  │   └── D 1.0
  │
  └── D 2.0
```
For example, if you want to force `D 2.0` to be used, you can manually specify it in the `dependencyManagement` section.
- *Dependency Scope* - include dependencies only appropiate for the current stage of the build
- *Dependency Exclusion* - if project `X` depends on `Y` and `Y` depends on `Z`, then `X` can explicitly exclude `Z` as a dependency
- *Optional Dependencies* - if `Y` depends on `Z`, it can mark it as **optional**. This way, if `X` depends on `Y`, `Z` will not be added as a dependency to `X` (optional dependencies are dependencies *excluded yb default*)

## Dependency Scope
Dependency scope is used to limit the transitivity of a dependency and determine when a dependency is included in a [[Classpath]]. By default, there are 6 scopes that you can pick from.

Something interesting is said about the `provided` scope:
*"This is much like `compile`, but indicates you expect the JDK or a container to provide the dependency at runtime. For example, when building a web application for the Java Enterprise Edition, you would set the dependency on the Servlet API and related Java EE APIs to scope `provided` because the web container provides those classes. A dependency with this scope is added to the classpath used for compilation and test, but not the runtime classpath. It is not transitive."*

## Resources
https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html