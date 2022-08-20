Tags: #reference 
Created: 2022-08-20 21:08

# Maven Default Lifecycle Phases
[[Maven]]'s default lifecycle's phases are:
- `validate` - validate the project is correct and all necessary information is available
- `compile` - compile the source code of the project
- `test` - test compiles source code using a usit testing framework (tests **sould not** require the code to be packaged or deployed)
- `package` - take compiled code and package it to its distributable format ([[JAR]], for example)
- `verify` - run any checks on results of integration tests to ensure the quality criteria are met
- `install` - install the package into the local repository (so it can be used as a dependency in other projects locally)
- `deploy` - done in the build environment, copies final package to the remote [[Maven Repository]] for sharing with other developments and projects

## Resources
https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#a-build-lifecycle-is-made-up-of-phases