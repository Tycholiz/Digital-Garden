
## Overview
Central to the Spring Framework is its [[IoC|general.patterns.IoC]] container, which provides a consistent means of configuring and managing Java objects using reflection.
- The container is responsible for managing object lifecycles of specific objects: creating these objects, calling their initialization methods, and configuring these objects by wiring them together.

Spring beans are objects that are created, managed and destroyed by the Spring Container
- The container can be configured by loading XML files or detecting specific Java annotations on configuration classes. 
  - These data sources contain the bean definitions which provide the information required to create the beans.

## Commands
### Start app
```sh
# Gradle
$ ./gradlew bootRun

# Maven
$ ./mvnw spring-boot:run
```

### Controllers
A key difference between a traditional MVC controller and the RESTful web service controller from Springboot is the way that the HTTP response body is created. 
```java
@GetMapping("/greeting")
public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
  return new Greeting(counter.incrementAndGet(), String.format(template, name));
}
```

- Rather than relying on a View technology to perform server-side rendering of the greeting data to HTML, this RESTful web service controller populates and returns a `Greeting` object. The object data will be written directly to the HTTP response as JSON. To do this, the `Greeting` object must be converted to JSON
  - Spring’s out-of-the-box HTTP message converter support allows us to do this. This works out of the box because Jackson 2 (the Java JSON package) is in the classpath

#### `@RestController`
The `@RestController` annotation marks the class as a controller where every method returns a domain object instead of a view.
- it is shorthand for including both `@Controller` and `@ResponseBody`.