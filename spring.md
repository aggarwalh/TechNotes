Common factors while adopting any new technology product in house
 - Efficient
 - Secure
 - Cost
 - Support

Spring Ecosystem 
  - Web layer
  - Common layer (constitutes core [context], test, AOP etc)
  - Service layer (spring cloud, spring integration)
  - Data Layer etc

Spring Architecture
  - Core container (Beans, Core, Context, SpEL)
  - Data Access (JDBC [jdbc abstraction layer], ORM, JMS, Transactions)
  - Web
  - Misc (AOP, Aspects, Instrumentation, Messaging, Test)

Spring IoC
  - Dependency Injection ( Given your POJOs and metadata about object creation in xml, POJO, annotations, abstract out object creation, reference and destruction)
     - BeanFactory ( from spring-beans module) [Defaults to lazy construction]
     - ApplicationContext ( from spring-context module) [Defaults to early bean creation]
  - Features
    - Setter injection, Constructor injection
    - Custom bean initialization and destruction code
    - Early vs Lazy evaluation
    - Revaluation of entire context etc. 

Configration style
 - Java config, Annotation based, XML config
 - Java config is now the prescribed mode over old xml style. Annotation mode should supplement java config style.
    - Like XML it helps in segrating config from app logic
    - Pros from xml: Provides compile time check on config issues     
    - Cons from xml: Just the idea that a java file is actually config and not app logic. xml cofig made that more clear.
    - Clearly the pros outweigh the cons.

Java bean configuration style
 - *@Configuration* annotation on a *class* denotes that it is a config file.
 - Each bean def'n is a *method* with *@Bean* def'n. Where method name is the bean id. Return type is the Type being returned (Interface or class). Body of class creates the object using constructor parameterization or setters. You can refer other beans defined in your config as paramerter or setter values.
 - You can separate config for clarity like dao and service. In service config you need to @Import(DaoSpringConfigClassName.class) on ServiceSpringConfig class. 
     - Refering a bean from imported config file in a bean def'n of current file 
     - Style 1: To use a bean defined in dao config in your service layer bean you need to include it as a the beans' function parameter. 
     - Style 2: Add the dependent bean type as data member of given Service layer bean def'n and use Annotation style's to inject the correct bean
         - Style 2.1: @Autowired : means Autowire by type first (then by qualifier and finally by the name) => This would work if there is exactly one bean of that type. To overcome this issue @Autowire is supplemented with @Qualifier stating the exact bean name
         - Style 2.2: @Inject ( java standard)   
         - Style 2.3: @Resource:  a java standard (javax.annotations) . uses "autowire by name" style then by type and finally by Qualifiers (ignored if match is found by name).
 - The default creation style of spring is *singleton*, which is done by spring using a proxy on config class.

Annotation style
 - @Autowire
 - @Inject
 - @Resource
 - @ComponentScan in conjuction with @Component ( or specific types like @Controller, @Service, @Repository)
