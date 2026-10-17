
## Java Development Kit (JDK)
- implements the Java Language Specification (JLS) 
- implements the the Java Virtual Machine Specification (JVMS)
- provides the Standard Edition (SE) of the Java API

It provides software for working with Java applications. Examples of included software are:
- the virtual machine
- a compiler
- performance monitoring tools
- a debugger

run the binary `/usr/libexec/java_home` to know where in the system Java is installed

check version:
```sh
$ javac -version
```

### javac
- the main compiler included within the JDK
- converts source code into Java bytecode

Due to *dynamic binding*, the compiler will not catch `ClassCastExceptions`
- dynamic binding is the Java feature of being able to include new objects that weren't even known to the original programmer

### Classpath
Classpath is a parameter in the JVM or the Java compiler that specifies the location of user-defined classes and packages.
- it is a set of paths where the java compiler and the JVM will find needed classes to compile or execute other classes.
- classes that the JVM needs for compilation and/or executing other classes can be found at these paths.
- naturally, we wouldn't expect the JVM to search through every folder on our computer (and nor would we even want that), so we provide a classlist so it knows where to search.
- A given Java program isn't in a single file, it has access to everything in the classpath

In Java, we make other classes available to our classes by `import`ing them at the top of our source file:
```java
// single class import
import org.javaguy.coolframework.MyClass;
// bulk import
import org.javaguy.coolframework.*;
```

Because of this `import` declaration, the JVM will know where to find the compiled class (ie. `.class` file).
- ex. if our project has a `output/` directory, and our class is located at `output/org/javaguy/coolframework/MyClass.class`, then `output/` is as deep as we need to go when specifying the path in the classpath. The rest of the directory path is obtained from the actual `import` statement.

The classpath can also contain `.jar` files


## Java Runtime Environment (JRE)
The JRE is included in the JDK, though is can also be installed independently of the JDK
- end-users need to install the JRE to run the Java program, while the whole JDK is for developers

The JRE consists of:
- a Java Virtual Machine (called HotSpot) 
- all of the class libraries present in the production environment, 
- additional libraries only useful to developers, such as the internationalization libraries and the IDL libraries.


check version:
```sh
$ java -version
```

* * *

## Files
### JAR (Java Archive) file
JAR files exist so that we don't have to send an end-user all 100 different classes of our application and tell them to run the app with `gradle` or `maven`. 
- Instead, we can bundle them into a JAR (basically just a zip format) and just get them to run it as an executable
- in the JAR file, we include a *manifest* which defines which class in that JAR holds the `main()` method

### WAR (Web Application Resource, or Web Application Archive) file
a file used to distribute a collection of JAR-files, JavaServer Pages, Java Servlets, Java classes, XML files, tag libraries, static web pages (HTML and related files) and other resources that together constitute a web application.

## Other
### JavaBeans (a.k.a Beans)
Beans are classes that encapsulate one or more objects into a single standardized object (the bean)
- This standardization allows the beans to be handled in a more generic fashion, allowing easier code reuse and introspection.
- Essentially, beans are just data objects with getters/setters with little to no logic in them. 
- Your typical `Car` class can be considered a Java bean. Java beans are typically more or less 1:1 mapped to database tables.