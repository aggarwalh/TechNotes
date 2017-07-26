#### Common factors while adopting any new technology product in house
 - Efficient
 - Secure
 - Cost
 - Support

#### Spring Ecosystem 
  - Web layer
  - Common layer (constitutes core [context], test, AOP etc)
  - Service layer (spring cloud, spring integration)
  - Data Layer etc

#### Spring Architecture
  - Core container (Beans, Core, Context, SpEL)
  - Data Access (JDBC [jdbc abstraction layer], ORM, JMS, Transactions)
  - Web
  - Misc (AOP, Aspects, Instrumentation, Messaging, Test)

#### Spring IoC
  - Dependency Injection ( Given your POJOs and metadata about object creation in xml, POJO, annotations, abstract out object creation, reference and destruction)
     - BeanFactory ( from spring-beans module) [Defaults to lazy construction]
     - ApplicationContext ( from spring-context module) [Defaults to early bean creation]
  - Features
    - Setter injection, Constructor injection
    - Custom bean initialization and destruction code
    - Early vs Lazy evaluation
    - Revaluation of entire context etc. 

#### Configration style
 - Java config, Annotation based, XML config
 - XML is the old way of doing things. Only pros being separation of code and config, but that is the very cons too as it very verbose (being xml) and also that a lot of wiring can be done by spring itself.
 - Between the Java Config style and Annotation based style 
    - **Verbose/Readability** Java config style is verbose (like xml), but you do get to catch most issues at compile time unlike xml. Annotation based style using ComponentScan on @Component and it's avatars are less verbose. 
    - **Auto Injection** Annotation style embraces this and lets the framework do this. 
    
    
**Java bean configuration style**
 - **@Configuration** annotation on a **class** denotes that it is a config file.
 - Each bean def'n is a **method** with **@Bean** def'n. Where method name is the bean id. Return type is the Type being returned (Interface or class). Body of class creates the object using constructor parameterization or setters. You can refer other beans defined in your config as paramerter or setter values.
 - You can separate config for clarity like dao and service. In service config you need to **@Import(DaoSpringConfigClassName.class)** on ServiceSpringConfig class. 
     - Refering a bean from imported config file in a bean def'n of current file 
     - Style 1: To use a bean defined in dao config in your service layer bean you need to include it as a the beans' function parameter. 
     - Style 2: Add the dependent bean type as data member of given Service layer bean def'n and use Annotation style's to inject the correct bean
 - The default creation style of spring is *singleton*, which is done by spring using a proxy on config class.

**Annotation style**
 - Dependency Injection (Field/Method(Setter)/Constructor)
    - **@Autowired** : means Autowire by type first (then by qualifier and finally by the name) => This would work if there is exactly one bean of that type. To overcome this issue @Autowire is supplemented with @Qualifier stating the exact bean name
    - **@Inject** (java standard, i.e. javax.annotations) A replacement of @Autowire which is spring specific annotation
    - **@Resource**: (also a java standard) . uses "autowire by name" style then by type and finally by Qualifiers (ignored if match is found by name).
    - **Contrasting them with use-cases**: TODO
 - Declaring a Java class as a bean
     - **@Component**: Annotation applied on a class
     - **@Repository**: Specialization of @Component applied to DAO layer classes. 
         - Serves as high level functional segregation on nature of component for both Humans (readability) and machine (add extra facilities specific to annotation)
         - It also makes the unchecked exceptions (thrown from DAO methods) eligible for translation into Spring DataAccessException
     - **@Service**: Specialization of @Component. Currently no extra feature on top of @Component apart from clarity.
     - **@Controller**: Specialization of @Component. This annotation marks a class as a Spring Web MVC controller. You can use another annotations i.e. @RequestMapping; to map URLs to instance methods of a class.
     - **Contract use-case**: Prefer the three specialization of @Component instead of @Component itself for clarity and extra benefits discussed above. 
 - Enable detection of these beans and pulling them in DI container.
      - **@ComponentScan**: TODO


 
 ** Modern injection style 1: Annotation based as primary and Java config as secondary**
  - Prefer Annotation style for all classes in your repo. 
  - Use Java config when you can't use above:
      - For classes not in our source code like say configure java.util.Date.
      - For cases when you want multiple beans instances of same Class with different constuction.
      
  ** Style 2: Use Java config as primary and Annotation config as secondary**
   - This is mostly done when writing libraries and not end application 
   - Organization Patterns for java beans style
      - Organization of bean definitions in different java files based on layers and/or functionality. This has already been discussed above.
      - Abstract dependency definitions when writing a library. You may want user to plug in some dependencies and to abstract them up in an Interface that defines getters to the dependency and you Autowire on this interface in your library's spring context. The client can provide his implementation and even switch among various one's using @Profile and run-time jvm args. 
 
