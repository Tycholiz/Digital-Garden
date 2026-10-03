
Maven is a build tool like [[gradle]]

Helps:
- download 3rd party packages
- give the project structure through a set of conventions

Maven projects have a `pom.xml` file 

## `pom.xml`

### Properties
- `<source>11</source>` means "project should be built *in* Java 11
- `<target>11</target>` means "project should be built *for* Java 11

### Build
Allows you to specify plugins, which are extensions to Maven which help it build our artifact.
- ex. `maven-jar-plugin`, which is used to make a jar out of your compiled classes and resources

### Dependencies
A dependency is just a Jar file which will be added to the classpath while executing the tasks.

A depdendency can include a `<scope>` field, which specifies when a dependency should be installed
- ex. `<scoope>test</scope>`, meaning the dependency will only be installed during tests.
- ex. `<scope>compile</scope>`