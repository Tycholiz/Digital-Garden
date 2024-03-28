
Java Environment (ie. Java Version Manager)

You can configure the JDK at 3 different levels, switching the JDK version that is used when running a `java` command:
- Global – Switch the JDK version globally
- Local - Switch the JDK version in the current directory only
- Shell - Switch the JDK version in the current shell instance only

## Steps
1. install the particular of Java you want
```sh
$ brew install openjdk@17
```

2. Add that version to jenv
```sh
$ jenv add /opt/homebrew/opt/openjdk@17
```

3. Get versions of Java registered with jenv
```sh
$ jenv versions 
* system
  17.0
  17.0.7
  openjdk64-17.0.7
```

4. Set version of Java
```sh
$ jenv local 17.0
```

## Use with Gradle/Maven
Maven and [[gradle]] both use the system JDK to run. Therefore, we have to enable their respective plugins:
```sh
jenv enable-plugin gradle
# or
jenv enable-plugin maven
```

this plugin will ensure that the `JAVA_HOME` variable is set correctly.

## Resources
- https://www.baeldung.com/jenv-multiple-jdk