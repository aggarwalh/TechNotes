### What is maven ? 

  1. **Build tool**
      - Create a packaged software (jar, war)
      - Deploy applications to remote repositories
      - Provides a build process which involves several others steps apart from compiling
       and packaging like generating source docs, running unit and integration tests.
      - Integrate with other build tools like CI tool like jenkins
      
  2. **Dependency Management tool**: Your project can depend on other lib (jars). 
     - Resolve version to use
     - Download it recursively
     - Put to class path. (All three are tedious if done mannually)
     - In what all phases/goals a dependency is added to the classpath can be set via scope
  
  3. Project Management tool (primitive in this aspect):
      - Project(artifact) versioning

### Basic principle: 
  **Standardize approach to building software**: Convention over configuration 
  1. Standardizing project layout
  2. Standardizing LifeCycle and phases (Maven binds certain phases to lifecycle)
  3. Maven binds certain goals to certain phases based on packaging. 
  4. Enforce on order of execution of phases in sequence.
     
       
  
### DEPENDENCY MANAGEMENT: 
 
   1. **Dependency Resolution**(dependency mediation): dependencies tag
  
                 BFS A->B(1.0)-> C(2.0)
                                 -> D(4.0)
                         ->C(3.0)
                         ->E     -> D(5.0)
                                 
                 Graph creation note: Sibling order decided by order in which dependency is written
                           
                            C shall be resolved to 3.0
                            D shall be resolved to 4.0        
    
   2. **dependecyManagement** tag: (say spring to v1.0 in current module or it's parent)
      - If a dependecy comes up in SAME project, you don't need to specify the version (if you do then that will take precedence). 
        - USE CASE: Typically used to abstract out version settings to common parent which typically has pom packaging.
      - If a transitive dependency comes up with v2.0 then, dependencyManagement shall override this to v1.0. 
        - USE CASE: Hard set of version for transitive dependencies. i.e dependencyManagement overried dependencyMediation for transitive dependencies
      - NOTE: it doesn't define a dependency itself, only a version if a dependency is defined.
      - child's dependencyManagement overrides parents dependencyManagement       
        
   3. **Scopes**: Dependency scope is used to limit the transitivity of a dependency and also to affect the classpath used for various build tasks(compilation, test). 
        
      1. compile (default scope) dependencies are available in all classpaths of a project and are transitive.
      2. provided:  Not transitive, same as compile for classpaths, but not in packaging (like servlet api in wars)
      3. test: transitive, only available for the test-compile and test execution phases. 
      4. import (only available in Maven 2.0.9 or later) N.A for transitivity
         - This scope is only used on a dependency of type pom in the <dependencyManagement> section. 
         - It indicates that the specified POM should be replaced with the dependencies in that POM's <dependencyManagement> section. 
         - USE CASE 
            - When you got dependencyManage a lot of versions (possibly different) of sub-projects of same project (say srping) that you depend on, then you can IMPORT BOM pom (Bill Of Materials) that is parent pom of root pom(parent pom of modules say spring),
            - This bom pom specifes dependencyManagement for all sub-projects.
            - example: spring-framework-bom

### Best practices
1. Segregating what changes to what doesn't + DRY
  1. Versions extracted to properties in parent ( even your module's own version need not be written, unless ofcourse your parent versioning is not under your direct control). 
  2. Common dependencies managed in parent
  3. Use of  BOM pom (import scope)
  4. Plugin config setup in parent
2. Stability of dependencies
  1. Dependencies on external lib even of different teams in same firm should be dependent on stable release unless you explicitly need some feature not present in latest stable release. 
  2. This also applies to your parent pom if it's release cycle is not under your direct control.
  3. Root of all poms enforces all default restrictions laid out by maven. org/apache/maven/model/pom-4.0.0.xml present in lib/maven-model-builder-***.jar
3. See effective pom `$$ mvn help:effective-pom`
                       

### PROJECT BUILD                     
                    
**Concepts** 
**Plugins**: 
 - Additional software component apart from core software providing extra functionality. 
 - They are downloaded by maven core system, thus keeping maven core software size small.
**Goals**: Each plugin can perform a set of goals/tasks.  
 ` $$ mvn help:describe -Dplugin=javadoc ## To see goals in a given plugin`
               
**Life Cycle of build**: A set of stages or steps (called PHASES) to achieve a task
 - Maven has three LifeCycles 
     1. clean:  3 phases
     2. default: 23 phases
     3. site
 - Running a phase in a life-cycle executes all previous phases of that lifecycle
 - Execution of a phase means execution of all goals bound to it. 
    - Some goals are bound by default based on packaging. 
    - Other goals can be added to a phase via "execution" tags.

```
$$ mvn help:describe -Dcmd=default // describe the various phases
$$ mvn <phase1> <plugin:goal> <phase2> // can run phases and goals
```        
        
**Profiles** 
What/Why ?  You can add customize build process via profiles. Add your custom build conditions within the profile.
How to set a profile ? 
 -`$$ mvn <phase> -P<profileId`
 - "activation" tag based on 5 available activation conditions namely  activeByDefault,file,jdk,os,property
                
**Properties** (variables for runtime configuration setup)
Extract out variables and substitute them from 
 - system variables: ${java.version}
 - environment variables: prefix "env." with the environment variable, like 
 - maven project variables: ${project.artifactId},${project.version}
 - custom variables set using properties tag or in filter(property files) files
   
   
### PROJECT MANAGEMENT
             
- Basic Metadata: GAVP (Group,Artifact,Version, Packaging)
- More textual info: name, description
- More info: developers, ci tool, scm , license
    
    
### Commands: 
  
   1. Building application 
    ` $ mvn clean package`
  
   2. Running tests: (Running a single test in integration phase)
      - ` $ mvn integration-test -Dtest=SampleIntegrationTest -Dskip.integration.tests=false -DfailIfNoTests=fals`
      - -Dtest is used to point to a specific test case (Not passing this shall run all available test cases)
      - -Dskip.integration.tests is a possible placeholder that determines if integration tests shall be skipped. It's not in maven vocab but a common practice. See Appendix##. 
      - -DfailIfNoTests is needed to avoid failure when running this from top level project which has several modules none of whose (except one) test cases you are running. 
 
   3.  Running mvn builds/tests with an open debug port that starts the build process only after you connect the remote debugger to it (by setting suspend=n)
        ```
         $ mvn integration-test -Dmaven.surefire.debug="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8001 -Xnoagent -Djava.compiler=NONE" 
        ```
 
### Setup: 
  
### APPENDIX: 

**Plugin Config**

1. COMPILER:  Configure maven compiler to compile the source as a particular java version (assertion till that versions shall be considered valid) and byte code targeted to a particular version (compatibility). 
  - Note: Both source and target need to be specified that code actually runs on a JRE with specified versions. Pitfall is uninted usage of API available in newer versions which can cause linkage errors at runtime.
  - Following needs to be done in pluginsManagementSection

   ```       
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>${maven.compiler.plugin.version}</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<compilerArgument>-parameters</compilerArgument>
				</configuration>
   	                </plugin>
                </plugins>
   ```    

2. TEST RUNNERS (surefire, failsafe)
   
   - Configure plugin to bind it's goals to one or more phases custom configurations
   ```
          <plugin>
               <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>${maven.surefire.plugin.version}</version>
                <configuration>  <!--Disable to default unit-test goal and enable your own version, i.e one with you id-->
                    <skip>true</skip>
                </configuration>
                <executions>
                    <execution>
                        <id>unit-test</id>  <!--Defining you own name for unit test and configuring it-->
                        <phase>test</phase> <!--Define the phase to bind to-->
                        <goals>
                            <goal>test</goal>
                        </goals>
                        <configuration>
                            <argLine>
                                    -d64 -Xms2048m -Xmx4096m -XX:PermSize=512m ${jacoco.agent.argLine}
                            </argLine>
                            <excludedGroups>in.devjcloud.IntegrationTest</excludedGroups>   <!--Exclude specific tests from running -->
                        </configuration>
                    </execution>
                    <!--Define similar executions for integration-test phase-->

   ```
 
### References
- https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html
   
