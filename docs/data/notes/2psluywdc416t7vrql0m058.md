
## What is it?
Gradle is a build automation tool
- It makes as few assumptions as possible with your project, and thus it is built to be flexible enough to support almost any type of software.

Gradle only re-runs jobs when inputs/outputs have changed.
- therefore, caches are utilized

Gradle runs on the JVM
- an implication here is that build logic can use the standard Java APIs.

At first, we have only a `build.gradle` file. When we run `gradle wrapper`, some files are generated for us:
- `gradle/wrapper/gradle-wrapper.jar`
- `gradle/wrapper/gradle-wrapper.properties`
- `gradlew`
- `gradlew.bat`

### Build Lifecycle
Gradle evaluates and executes build scripts in three build phases of the Build Lifecycle:

#### Initialization
Sets up the environment for the build and determine which projects will take part in it.

#### Configuration
Constructs and configures the task graph for the build. Determines which tasks need to run and in which order, based on the task the user wants to run.

#### Execution
Runs the tasks selected at the end of the configuration phase.

## Terminology
### Projects
Projects are the things that Gradle builds.

Projects contain a build script (`build.gradle`)
- Builds scripts define tasks, dependencies, plugins, and other configuration for that project.
- A single build can contain one or more projects and each project can contain their own subprojects.

### Tasks
Tasks contain the logic for executing some work—​compiling code, running tests or deploying software.

Most often, we'll use existing tasks.
- ex. built-in Java `Test` task, which runs tests.

Tasks themselves consist of:
- *Actions*: pieces of work that do something, like copy files or compile source
- *Inputs*: values, files and directories that the actions use or operate on
- *Outputs*: files and directories that the actions modify or generate

### Plugins
Plugins allow you to introduce new concepts into a build beyond tasks, files and dependency configurations
- ex. most language plugins add the concept of *source sets* to a build.

Plugins provide a means of reusing logic and configuration across multiple projects.
 
### Build

### shadowJar
ShadowJar is a plugin for the Gradle build system that provides a convenient way to create "fat" or "uber" JAR files
- A fat JAR file is a self-contained archive that includes not only your application's compiled classes but also all its dependencies. It allows you to package your application into a single JAR file that can be easily distributed and executed, without requiring users to manage separate JAR files for dependencies.