> This guide was built base on the [Micronaut Cassandra Guide](https://micronaut-projects.github.io/micronaut-cassandra/latest/guide/)

## A - Overview

Micronaut is a modern, JVM-based, full stack Java framework designed for building modular, easily testable JVM applications with support for Java, Kotlin, and Groovy. Micronaut is developed by the creators of the Grails framework and takes inspiration from lessons learnt over the years building real-world applications from monoliths to microservices using Spring, Spring Boot and Grails. For more information refer to the [user guide](https://docs.micronaut.io/latest/guide/)

The [micronaut-cassandra](https://micronaut-projects.github.io/micronaut-cassandra/latest/guide/) module includes support for integrating Micronaut services with Cassandra.

## B - Prerequisites

- You should have an [Astra account](http://astra.datastax.com/)
- You should [Create and Astra Database](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-AstraDB-Instance)
- You should [Have an Astra Token](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-Astra-Token)
- You should [Download your Secure bundle](https://github.com/datastaxdevs/awesome-astra/wiki/Download-the-secure-connect-bundle)
- You should install a `Java JDK 1.8+` and [Apache Maven](https://maven.apache.org/)

## C - Configuration

> You can find a working sample [here](https://github.com/clun/astra-native-java/tree/main/astra-todo-micronaut)

**✅ Step 1: Create your project**

- To create a micronaut project and CLI `mn` is provided. You can install it using `sdkman` as describe in the [doc](https://micronaut.io/download/)

```bash
#Download SDKMan
curl -s https://get.sdkman.io | bash

#Setup SDKMan
source "$HOME/.sdkman/bin/sdkman-init.sh"

#Download Micronaut
sdk install micronaut
```

- To generate a new project use `mn create-app` adding the feature cassandra

```bash
mn create-app astra-todo-micronaut --features cassandra
```

- Notice that in your `pom.xml` you now have the following

```xml
<dependency>
   <groupId>io.micronaut.cassandra</groupId>
   <artifactId>micronaut-cassandra</artifactId>
   <scope>compile</scope>
</dependency>
```

**✅ Step 2: Setup your project**

All configuration of your project will be defined in `application.yaml` in `src/main/resources`. The module is clever enough to **load all properties as if it was the driver configuration file**.

You can defined multiple profiles and each profile will be identified with key `cassandra.${profile_name}`. There is a `default` profile. In the following sample file we provide 2 profiles one for local and one for Astra. There is no extra code needed, simply configuration.

```yaml
cassandra:
  default:
    basic:
      session-keyspace: micronaut
      contact-points:
        - "localhost:9042"
      load-balancing-policy:
        local-datacenter: datacenter1
  astra:
    basic:
      request:
        timeout: 5 seconds
        consistency: LOCAL_QUORUM
        page-size: 5000
      session-keyspace: micronaut
      cloud:
        secure-connect-bundle: /Users/cedricklunven/Downloads/secure-connect-workshops.zip
    advanced:
      auth-provider:
        class: PlainTextAuthProvider
        username: token
        password: "AstraCS:blahblahblah"
      connection:
        init-query-timeout: 10 seconds
        set-keyspace-timeout: 10 seconds
      control-connection.timeout: 10 seconds
```

**✅ Step 3: Application Startup**

At startup you may want create the different tables needed for you application. _In Astra you can only create keyspaces from the devops API or the user interface._.

To enable content at startup simple implement `ApplicationEventListener<ServiceReadyEvent>` as shown below

```java
@Singleton
public class TodoApplicationStartup  implements ApplicationEventListener<ServiceReadyEvent> {

    /** Logger for the class. */
    private static final Logger LOGGER = LoggerFactory.getLogger(TodoApplicationStartup.class);

    @Property(name = "todo.cassandra.create_schema", defaultValue="false")
    private boolean createTable;

    @Inject
    private CqlSession cqlSession;

    /** {@inheritDoc} */
    @Override
    public void onApplicationEvent(final ServiceReadyEvent event) {
        LOGGER.info("Startup Initialization");
        if (createTable) {
            TodoServiceCassandraCql.createTableTodo(cqlSession);
            LOGGER.info("+ Table TodoItems created if needed.");
        }
        LOGGER.info("[OK]");
    }
}
```

**✅ Step 4: Use Cassandra**

To use Cassandra you will reuse the `CqlSession` from the Datastax drivers. You can simply inject it where you needed as [shown in this sample code](https://github.com/clun/astra-native-java/blob/main/astra-todo-micronaut/src/main/java/com/datastaxdev/todo/TodoRestController.java)

```java
@Validated
@Controller("/api/v1")
public class TodoRestController {

    /** Logger for our Client. */
    private static final Logger LOGGER = LoggerFactory.getLogger(TodoRestController.class);

    /** CqlSession initialized from application.yaml */
    @Inject
    private CqlSession cqlSession;
```

Happy coding.
