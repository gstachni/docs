<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/css/font-awesome.min.css">

### 1. Overview

<img src="../../../../img/tile-java.png" align="left" height="180px"/>

Astra provides include **multiple services** (Database, Streaming), and, for each, there are **multiple Api and interfaces**. Frameworks and tools to connect to Astra are related to the Apis you choose.

Pick the interface in the table to get relevant instructions. In most case you download a working sample. Those are standalone examples designed as simple as possible. please note that a _Software developement KIT (SDK)_ is also available for you to reduce boilerplate. More information [here](https://github.com/datastax/astra-sdk-java/wiki).

### 2. Interfaces list

Pick the interface you want to use from the list:

|      Component      | Interface                                                                        | Description                                 |
| :-----------------: | :------------------------------------------------------------------------------- | :------------------------------------------ |
|    **Astra DB**     | [:fontawesome-solid-paper-plane: Cassandra Drivers](#3-cql-cassandra-drivers)    | Main connection to Cassandra                |
|    **Astra DB**     | [:octicons-arrow-switch-16: Rest API](#4-stargate-rest-api)                      | CQL exposes as stateless rest resources     |
|    **Astra DB**     | [:material-code-json: Document Api](#5-stargate-document-api)                    | Use Cassandra as a Document DB              |
|    **Astra DB**     | [:material-graphql: GraphQL Api](#6-stargate-graphql)                            | Create tables and use generated CRUD        |
|    **Astra DB**     | [:octicons-code-square-16: Grpc Apis](#7-stargate-grpc)                          | CQL exposes through serialized protobuf     |
| **Astra Streaming** | [:fontawesome-solid-envelopes-bulk: Pulsar Client](#8-pulsar-client)             | Create Producer, Consumers, Subscriptions.. |
| **Astra Streaming** | [:fontawesome-brands-connectdevelop: Pulsar Admin](#9-pulsar-admin)              | Administrate your Pulsar cluster            |
|   **Astra Core**    | [:fontawesome-solid-users: Devops Users and Roles](#10-devops-organization-apis) | Manage users and roles                      |
|   **Astra Core**    | [:material-database: Devops Database](#11-devops-database-api)                   | Manage Databases                            |
|   **Astra Core**    | [:material-tools: Devops Streaming](#12-devops-streaming-api)                    | Manage Streaming                            |

## 3. CQL Cassandra Drivers

Drivers reference documentation can be found [HERE](https://docs.datastax.com/en/developer/java-driver/4.13/), this page is focused on connectivity with Astra DB only.

???+ abstract "Prerequisites"

      **Astra**

      - You should have an [Astra account](http://astra.datastax.com/)
      - You should [Create and Astra Database](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-AstraDB-Instance)
      - You should [Have an Astra Token](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-Astra-Token)
      - You should [Download your Secure bundle](https://github.com/datastaxdevs/awesome-astra/wiki/Download-the-secure-connect-bundle)

      **Development environment**

      - You should install **Java Development Kit (JDK) 8**: Use the [reference documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) to install a **Java Development Kit**, Validate your installation with

      ```bash
      java --version
      ```

      - You should install **Apache Maven**: Use the [reference documentation](https://maven.apache.org/install.html) and validate your installation with

      ```bash
      mvn -version
      ```

### 3.1 Drivers 4.x

!!! important "Version 4.x is recommended version"

Version 4 is major redesign of the internal architecture. As such, it is not binary compatible with previous versions. However, most of the concepts remain unchanged, and the new API will look very familiar to 2.x and 3.x users.

??? abstract annotate "Setup `pom.xml`"

      - Any version `4.x` should be compatible with Astra.

      - Update your `pom.xml` file with the latest version of the 4.x libraries: [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss/java-driver-core/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss/java-driver-core)

      ```xml
      <!-- (REQUIRED) -->
      <dependency>
      <groupId>com.datastax.oss</groupId>
      <artifactId>java-driver-core</artifactId>
      <version>${latest4x}</version>
      </dependency>

      <!-- OPTIONAL -->
      <dependency>
      <groupId>com.datastax.oss</groupId>
      <artifactId>java-driver-query-builder</artifactId>
      <version>${latest4x}</version>
      </dependency>
      <dependency>
      <groupId>com.datastax.oss</groupId>
      <artifactId>java-driver-mapper-runtime</artifactId>
      <version>${latest4x}</version>
      </dependency>
      ```

??? abstract "Sample Code"

      ```java
      import java.nio.file.Paths;
      import org.slf4j.Logger;
      import org.slf4j.LoggerFactory;
      import com.datastax.oss.driver.api.core.CqlSession;

      public class AstraDriver4x {

      static final String ASTRA_ZIP_FILE = "<path_to_secureConnectBundle.zip>";
      static final String ASTRA_USERNAME = "<provide_a_clientId>";
      static final String ASTRA_PASSWORD = "<provide_a_clientSecret>";
      static final String ASTRA_KEYSPACE = "<provide_your_keyspace>";

      public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(AstraDriver4x.class);
        try (CqlSession cqlSession = CqlSession.builder()
          .withCloudSecureConnectBundle(Paths.get(ASTRA_ZIP_FILE))
          .withAuthCredentials(ASTRA_USERNAME, ASTRA_PASSWORD)
          .withKeyspace(ASTRA_KEYSPACE)
          .build()) {
          logger.info("[OK] Welcome to ASTRA. Connected to Keyspace {}", cqlSession.getKeyspace().get());
        }
        logger.info("[OK] Success");
        System.exit(0);
      }

      }
      ```

??? info "Extra Resources"

      - If you want to know more the rational is explained [in this blogpost](https://www.datastax.com/blog/introducing-java-driver-4).
      - If you are still using `3.x` and want to migrate you can have a look the [upgrade guide](https://docs.datastax.com/en/developer/java-driver/4.13/upgrade_guide/#4-0-0) but you can also keep using `3.x` as described [below](#using-java-cassandra-drivers-version-3x)
      - [Multiple Standalone Classes using driver 4.x](https://github.com/DataStax-Examples/java-cassandra-driver-from3x-to4x/tree/master/example-4x/src/main/java/com/datastax/samples)
      - [Spring PetClinic in Reactive](https://github.com/spring-petclinic/spring-petclinic-reactive) and specially the [mapper](https://github.com/spring-petclinic/spring-petclinic-reactive/tree/master/src/main/java/org/springframework/samples/petclinic/vet/db)

<a href="https://github.com/awesome-astra/sample-java-driver3x/archive/refs/heads/main.zip" class="md-button">
  <i class="fa fa-download" ></i>&nbsp;Download Driver 4x Sample
</a>

### 3.2 Drivers 3.x

- Please note that version **3.8+** is required to connect to Astra.

??? abstract "Setup `pom.xml`"

      - Update your `pom.xml` file with the latest version of the 3.x libraries: [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.datastax.cassandra/cassandra-driver-mapping/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.datastax.cassandra/cassandra-driver-mapping/)

      ```xml
      <dependency>
        <groupId>com.datastax.cassandra</groupId>
        <artifactId>cassandra-driver-core</artifactId>
        <version>${latest3x}</version>
      </dependency>
      ```

??? abstract "Sample Code"

      ```java
      import java.io.File;
      import org.slf4j.Logger;
      import org.slf4j.LoggerFactory;
      import com.datastax.driver.core.Cluster;
      import com.datastax.driver.core.Session;

      public class AstraDriver3x {

      // Define inputs
      static final String ASTRA_ZIP_FILE = "<path_to_secureConnectBundle.zip>";
      static final String ASTRA_USERNAME = "<provide_a_clientId>";
      static final String ASTRA_PASSWORD = "<provide_a_clientSecret>";
      static final String ASTRA_KEYSPACE = "<provide_your_keyspace>";

      public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(AstraDriver3x.class);
        try(Cluster cluster = Cluster.builder()
          .withCloudSecureConnectBundle(new File(ASTRA_ZIP_FILE))
          .withCredentials(ASTRA_USERNAME, ASTRA_PASSWORD)
          .build() ) {
            Session session = cluster.connect(ASTRA_KEYSPACE);
            logger.info("[OK] Welcome to ASTRA. Connected to Keyspace {}", session.getLoggedKeyspace());
        }
        logger.info("[OK] Success");
        System.exit(0);
      }

      }
      ```

??? info "Extra Resources for Cassandra Drivers 3.x"

      - [Multiple Standalone Classes using driver 3.x](https://github.com/DataStax-Examples/java-cassandra-driver-from3x-to4x/tree/master/example-3x/src/main/java/com/datastax/samples)

<a href="https://github.com/awesome-astra/sample-java-driver3x/archive/refs/heads/main.zip" class="md-button">
       <i class="fa fa-download" ></i>&nbsp;Download Driver 3x Sample
</a>

### 3.3 Astra SDK

This SDK (Software Development Kit) makes it easy to call Stargate and/or Astra services using idiomatic Java APIs.

![](https://github.com/datastax/astra-sdk-java/raw/main/docs/img/sdk-overview.png?raw/true)

**The Astra SDK** setup the connection to work with AstraDB cloud-based service. You work with the class `AstraClient` (that configure `StargateClient` for you). As you can see on the figure below the `AstraClient` handles not only Stargate Apis but also Astra Devops Api and Apache Pulsar. [Reference documentation](https://github.com/datastax/astra-sdk-java/wiki).

??? abstract annotate "Setup `pom.xml`"

      - Update your `pom.xml` file with the latest version of the SDK

      [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.datastax.astra/astra-sdk/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.datastax.oss/java-driver-core)

      ```xml
      <dependencies>
       <dependency>
           <groupId>com.datastax.astra</groupId>
           <artifactId>astra-sdk</artifactId>
           <version>0.3.0</version>
       </dependency>
      </dependencies>
      ```

??? abstract "Sample Code"

      ```java
      // Init Astra Client
      AstraClient cli = AstraClient.builder()
         .withToken(ASTRA_DB_TOKEN)
         .withDatabaseId(ASTRA_DB_ID)
         .withDatabaseRegion(ASTRA_DB_REGION)
         .withCqlKeyspace(ASTRA_DB_KEYSPACE)
         .enableCql()
         .build();

      // Using Cassandra Interface
      CqlSession cqlSession = astraClient.cqlSession();
      String cqlVersion = cqlSession
                .execute("SELECT cql_version from system.local")
                .one().getString("cql_version"));
      ```

??? info "Extra Resources"

      - To get the full fledge informations regarding SDK check the [github repository](https://github.com/datastax/astra-sdk-java/wiki)

<a href="https://github.com/awesome-astra/sample-java-sdk/archive/refs/heads/main.zip" class="md-button">
  <i class="fa fa-download" ></i>&nbsp;Download SDK Sample
</a>

## 4. Stargate REST Api

**ℹ️ Overview**

Stargate is a data gateway (Proxy) on top of Apache Cassandra exposing new interface to ease the integration. It is a way to create stateless components (1) and ease the integration with 4 different HTTP Apis (rest, doc, graphQL, gRPC). In this chapter we will cover integration with `REST Apis` also called `DATA` in the swagger specifications.

To know more regarding this interface specially you can have a look to [dedicated section of the wiki](https://github.com/datastaxdevs/awesome-astra/wiki/Stargate-Api-Rest) or [reference Stargate Rest Api Quick Start Guide](https://stargate.io/docs/stargate/1.0/quickstart/quick_start-rest.html).

> ⚠️ We recommend to use version `V2` (_with V2 in the URL_) as it covers more features and the V1 would be deprecated sooner.

![v2](https://github.com/datastaxdevs/awesome-astra/blob/main/stargate-api-rest/api-data.png?raw=true)

**📦 Prerequisites [ASTRA]**

- You should have an [Astra account](http://astra.datastax.com/)
- You should [Create and Astra Database](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-AstraDB-Instance)
- You should [Have an Astra Token](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-Astra-Token)

**📦 Prerequisites [Development Environment]**

- You should install **Java Development Kit (JDK) 8**: Use the [reference documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) to install a **Java Development Kit**.

- You should install **Apache Maven**: Use the [reference documentation](https://maven.apache.org/install.html) and validate your installation with

```bash
mvn -version
```

**📦 Setup Project**

- You simple need an `HTTP Client` to use the Rest API. There are a lot in the Java languages like [HttpURLConnection](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/net/HttpURLConnection.html), [HttpClient introduced in Java 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html), [Apache HTTPClient](https://hc.apache.org/httpcomponents-client-5.0.x/index.html), [OkHttpClient](https://square.github.io/okhttp/), [Jetty HttpClient](https://www.eclipse.org/jetty/documentation/current/http-client.html). A comparison is provided is this [blogpost](https://www.mocklab.io/blog/which-java-http-client-should-i-use-in-2020/) to make your choice. In this tutorial we will go with `Apache HttpClient`, this is the one used in the SDK, you should adapt the code depending on the framework you choosed.

- Import relevant dependencies for `Apache Http Client` in your `pom.xml`

```xml
<dependency>
  <groupId>org.apache.httpcomponents.client5</groupId>
  <artifactId>httpclient5</artifactId>
  <version>5.1.3</version>
</dependency>
```

**🖥️ Sample Code (project [astra-httpclient-restapi](https://github.com/DataStax-Examples/astra-samples-java/tree/main/astra-httpclient-restapi))**

```java
public class AstraRestApiHttpClient {

    static final String ASTRA_TOKEN       = "<change_with_your_token>";
    static final String ASTRA_DB_ID       = "<change_with_your_database_identifier>";
    static final String ASTRA_DB_REGION   = "<change_with_your_database_region>";
    static final String ASTRA_DB_KEYSPACE = "<change_with_your_keyspace>";

    static  Logger logger = LoggerFactory.getLogger(AstraRestApiHttpClient.class);

    public static void main(String[] args) throws Exception {

        String apiRestEndpoint = new StringBuilder("https://")
                .append(ASTRA_DB_ID).append("-")
                .append(ASTRA_DB_REGION)
                .append(".apps.astra.datastax.com/api/rest")
                .toString();
        logger.info("Rest Endpoint is {}", apiRestEndpoint);

        try (CloseableHttpClient httpClient = HttpClients.createDefault()) {
            listKeyspaces(httpClient, apiRestEndpoint);
            createTable(httpClient, apiRestEndpoint);
            insertRow(httpClient, apiRestEndpoint);
            retrieveRow(httpClient, apiRestEndpoint);
        }
        logger.info("[OK] Success");
        System.exit(0);
    }
```

- List keyspaces

![listks](https://github.com/datastaxdevs/awesome-astra/blob/main/stargate-api-rest/schemas-keyspace-list.png?raw=true)

```java
private static void listKeyspaces(CloseableHttpClient httpClient, String apiRestEndpoint)
throws Exception {
 // Build Request
 HttpGet listKeyspacesReq = new HttpGet(apiRestEndpoint + "/v2/schemas/keyspaces");
 listKeyspacesReq.addHeader("X-Cassandra-Token", ASTRA_TOKEN);

 // Execute Request
 try(CloseableHttpResponse res = httpClient.execute(listKeyspacesReq)) {
   if (200 == res.getCode()) {
     logger.info("[OK] Keyspaces list retrieved");
     logger.info("Returned message: {}", EntityUtils.toString(res.getEntity()));
   }
 }
}
```

- Create a Table

![table](https://github.com/datastaxdevs/awesome-astra/blob/main/stargate-api-rest/schemas-table-create.png?raw=true)

> Query used is `createTableJson` here:

```json
{
  "name": "users",
  "columnDefinitions": [
    {
      "name": "firstname",
      "typeDefinition": "text"
    },
    {
      "name": "lastname",
      "typeDefinition": "text"
    },
    {
      "name": "email",
      "typeDefinition": "text"
    },
    {
      "name": "color",
      "typeDefinition": "text"
    }
  ],
  "primaryKey": {
    "partitionKey": ["firstname"],
    "clusteringKey": ["lastname"]
  },
  "tableOptions": {
    "defaultTimeToLive": 0,
    "clusteringExpression": [{ "column": "lastname", "order": "ASC" }]
  }
}
```

Create Table code

```java
private static void createTable(CloseableHttpClient httpClient, String apiRestEndpoint)
throws Exception {
  HttpPost createTableReq = new HttpPost(apiRestEndpoint
       + "/v2/schemas/keyspaces/" + ASTRA_DB_KEYSPACE + "/tables");
  createTableReq.addHeader("X-Cassandra-Token", ASTRA_TOKEN);
  String createTableJson = "{...JSON.....}";
  createTableReq.setEntity(new StringEntity(createTableJson, ContentType.APPLICATION_JSON));

  // Execute Request
  try(CloseableHttpResponse res = httpClient.execute(createTableReq)) {
    if (201 == res.getCode()) {
      logger.info("[OK] Table Created (if needed)");
      logger.info("Returned message: {}", EntityUtils.toString(res.getEntity()));
    }
  }
}
```

- Insert a Row

![row](https://github.com/datastaxdevs/awesome-astra/blob/main/stargate-api-rest/data-rows-insert.png?raw=true)

```java
private static void insertRow(CloseableHttpClient httpClient, String apiRestEndpoint)
throws Exception {
  HttpPost insertCedrick = new HttpPost(apiRestEndpoint + "/v2/keyspaces/"
   + ASTRA_DB_KEYSPACE + "/users" );
  insertCedrick.addHeader("X-Cassandra-Token", ASTRA_TOKEN);
  insertCedrick.setEntity(new StringEntity("{"
    + " \"firstname\": \"Cedrick\","
    + " \"lastname\" : \"Lunven\","
    + " \"email\"    : \"c.lunven@gmail.com\","
    + " \"color\"    : \"blue\" }", ContentType.APPLICATION_JSON));

  // Execute Request
  try(CloseableHttpResponse res = httpClient.execute(insertCedrick)) {
    if (201 == res.getCode()) {
      logger.info("[OK] Row inserted");
      logger.info("Returned message: {}", EntityUtils.toString(res.getEntity()));
    }
  }
}
```

- Retrieve a row

![row](https://github.com/datastaxdevs/awesome-astra/blob/main/stargate-api-rest/data-rows-read.png?raw=true)

```java
private static void retrieveRow(CloseableHttpClient httpClient, String apiRestEndpoint)
throws Exception {

  // Build Request
  HttpGet rowReq = new HttpGet(apiRestEndpoint + "/v2/keyspaces/"
    + ASTRA_DB_KEYSPACE + "/users/Cedrick/Lunven" );
  rowReq.addHeader("X-Cassandra-Token", ASTRA_TOKEN);

  // Execute Request
  try(CloseableHttpResponse res = httpClient.execute(rowReq)) {
    if (200 == res.getCode()) {
      String payload =  EntityUtils.toString(res.getEntity());
      logger.info("[OK] Row retrieved");
      logger.info("Row retrieved : {}", payload);
    }
  }
}
```

[![dl](https://dabuttonfactory.com/button.png?t=Download+Project&f=Open+Sans-Bold&ts=14&tc=fff&hp=15&vp=15&w=180&h=50&c=11&bgt=pyramid&bgc=666&ebgc=000&bs=1&bc=444)](https://github.com/DataStax-Examples/astra-samples-java/archive/refs/heads/main.zip)

## 5. Stargate Document Api

**ℹ️ Overview**

api XXX

**📦 Prerequisites [ASTRA]**

- You should have an [Astra account](http://astra.datastax.com/)
- You should [Create and Astra Database](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-AstraDB-Instance)
- You should [Have an Astra Token](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-Astra-Token)

**📦 Prerequisites [Development Environment]**

- You should install **Java Development Kit (JDK) 8**: Use the [reference documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) to install a **Java Development Kit**.

- You should install **Apache Maven**: Use the [reference documentation](https://maven.apache.org/install.html) and validate your installation with

```bash
mvn -version
```

**📦 Setup Project**

- Import relevant dependencies for `Apache Http Client` in your `pom.xml`. Jackon is also helpful to serialize or unserialized Java Objects as JSON.

```xml
<dependency>
  <groupId>org.apache.httpcomponents.client5</groupId>
  <artifactId>httpclient5</artifactId>
  <version>5.1.3</version>
</dependency>
```

**🖥️ Sample Code (project [astra-httpclient-docapi](https://github.com/DataStax-Examples/astra-samples-java/tree/main/astra-httpclient-docapi))**

> [⏫ Back to top](https://github.com/datastaxdevs/awesome-astra/wiki/Coding-Applications-for-Astra-in-JAVA#pick-the-interface-you-need-to-use)

## 6 Stargate GraphQL

### 6.1 CQL First

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

### 6.2 GraphQL First

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

## 7. Stargate gRPC

### 7.1 Stargate Client

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

### 7.2 Astra SDK

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

## 8. Pulsar Client

### 8.1 Pulsar Client

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

### 8.2 Astra SDK

**ℹ️ Overview**

```
TODO
```

**📦 Prerequisites [ASTRA]**

```
TODO
```

**📦 Prerequisites [Development Environment]**

```
TODO
```

**📦 Setup Project**

```
TODO
```

**🖥️ Sample Code**

```
TODO
```

## 9. Pulsar Admin

## 10 Devops API Database

## 11 Devops API Organization

## 12 Devops API Streaming
