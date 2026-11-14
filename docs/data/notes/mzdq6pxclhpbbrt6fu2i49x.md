
A build is an execution of a collection of tasks in a Gradle project. 

You run a build via the command line interface (CLI) or an IDE by specifying task selectors.

The gradle runtime is only necessary to be installed if we don't already have a Wrapper.


### Gradle Wrapper
The Wrapper is a script that invokes a declared version of Gradle, downloading it beforehand if necessary.

The Wrapper is the preferred way to execute a Gradle build.
- purpose is to ensure a reliable, controlled and standardized execution of the build.

Purposes:
- Standardizes a project on a given Gradle version, leading to more reliable and robust builds.
- Provisioning a new Gradle version to different users and execution environment (e.g. IDEs or Continuous Integration servers) is as simple as changing the Wrapper definition.

The generated Wrapper properties file, `gradle/wrapper/gradle-wrapper.properties`, stores the information about the Gradle distribution.
- The server hosting the Gradle distribution.
- The type of Gradle distribution. By default that’s the -bin distribution containing only the runtime but no sample code and documentation.
- The Gradle version used for executing the build. By default the wrapper task picks the exact same Gradle version that was used to generate the Wrapper files.
- Optionally, a timeout in ms used when downloading the gradle distribution.