# Spring Boot

## Speeding Boot Time

[Official Blog post](https://spring.io/blog/2018/12/12/how-fast-is-spring)

You may or may not have the luxury (based on team/firm code/library standards) or need to do these.

### Classpath exclusions from Spring Boot starter

<details>
Spring Boot follows the convention over configuration paradigm and by simply placing a library in your class path can cause Spring Boot to attempt to configure a module to use the library. Example: by doing something as simple as annotating your class with @RestController will trigger Spring Boot to auto-configure the entire Spring MVC stack.

* How to detect what additional stuff is being added that you don't need 
  * You can also specify debug=true in your application.properties. 
  * In addition, set the logging level in application.properties in development phase `logging.level.org.springframework.web: DEBUG`
  * Don't forget to remove them back when moving to prod.
* **Sample** 
  * Classpath exclusions from Spring Boot web starters:
    * Hibernate Validator
    * Jackson (but Spring Boot actuators depend on it). Use Gson if you need JSON rendering (only works with MVC out of the box).
    * Logback: use slf4j-jdk14 instead

</details>

### Unpack the fat jar and run with an explicit classpath

### JVM Level optimization

* Run the JVM with -noverify. 
* Consider -XX:TieredStopAtLevel=1 (that will slow down the JIT later at the expense of the saved startup time).

### Enable lazy bean initialization

<details>
<summary>How/When</summary>

  * How: 
    * Set `spring.main.lazy-initialization` as true in application properties (since Spring Boot 2.2 and Spring 5.2)
    * Legacy approach: `@Lazy` in bean def'n (entire context or specific bean) and `@Lazy` at point of injection **both**.
    * [official doc](https://spring.io/blog/2019/03/14/lazy-initialization-in-spring-boot-2-2)
  * Pros: Faster context load
    * Dev phase: Hot swap JVM using dev tools.
    * Integration tests: In case contexts are not boxed/isolated properly and even generally, with lazy load you end up initializing only beans actually touched by a test.
    * Prod: 
  * Cons
    * Mask problems that previously would have been identified at startup like Out of memory errors, no class def found errors, misconfiguration related errors.
    * First request/call that initializes a bean will be slow.   
    
</details>

### Choice of Container
 
Note that boot-up time may not be the primary concern when choosing container. Also you may have to stick to firmwide standard.

[Containers compared](https://examples.javacodegeeks.com/enterprise-java/spring/tomcat-vs-jetty-vs-undertow-comparison-of-spring-boot-embedded-servlet-containers/)
