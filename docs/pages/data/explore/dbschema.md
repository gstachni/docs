*Last Update {{ git_revision_date }}* 

<details>
<summary><b> 📖 Reference Documentations and resources</b></summary>
<ol>
<li><a href="https://docs.datastax.com/en/astra/docs/db-integration-dbschema.html"><b>📖 Astra Docs</b> - Reference documentation</a>
<li><a href="https://www.sestevez.com/astra-datagrip/">Instructions described in **Sebastian Estevez Blog post**</a>
</ol>
</details>

<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/download.png?raw=true" />

## A - Overview

DbSchema is a universal database designer for out-of-the-box schema management and documentation, sharing the schema in the team, and deploying on different databases. Visual tools can help developers, database administrators, and decision-makers to query, explore and manage the data.

- ℹ️ [Introduction to DBSchema](https://dbschema.com/features.html)
- 📥 [DBSchema Installation](https://dbschema.com/download.html)

DBSchema uses the [Simba JDBC driver](https://downloads.datastax.com/#odbc-jdbc-drivers) to connect to Cassandra as the storage backend. The Java driver itself supports connections to Astra DB natively.

## B - Prerequisites

- [Create an Astra Database](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-AstraDB-Instance)
- [Create an Astra Token](https://github.com/datastaxdevs/awesome-astra/wiki/Create-an-Astra-Token)
- [Download your secure connect bundle ZIP](https://github.com/datastaxdevs/awesome-astra/wiki/Download-the-secure-connect-bundle)

This article assumes you have installed the latest version of DBSchema on your laptop or PC.

## C - Installation and Setup

**✅ Step 1: JDBC Driver**

Download the JDBC driver from the DataStax website:

1. Go to https://downloads.datastax.com/#odbc-jdbc-drivers.
2. Select **Simba JDBC Driver for Apache Cassandra.**
3. Select **JDBC 4.2.**
4. Read the license terms and accept it (click the checkbox).
5. Hit the blue **Download** button.
6. Once the download completes, unzip the downloaded file.

**✅ Step 2: Establish the Connection**

1. Open [DB Schema](https://dbschema.com/)
2. Select **Connect to the Database**
3. Select **Start**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-start.png?raw=true"/>

4. In the **Choose your database** menu, select Cassandra.

5. Select **Next.**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-cass-sel.png?raw=true" />

6. Select **JDBC Driver** edit option.
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-connection-d.png?raw=true" />

7. In the JDBC Driver Manager, select **New**.
8. In the Add RDBMS window, enter **Astra** and select **OK**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-driver-manager.png?raw=true" />

9. Select **OK** in the confirmation message.
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-connection-d.png?raw=true" />

10. Upload the Simba JDBC Driver.
11. Select **Open**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-simba-driver.png?raw=true" />

12. Once you upload the Simba JDBC Driver, you will see **Astra** in the **Choose your Database **window. Select **Next**.
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-astra.png?raw=true" height="500px" />

13. In the Astra Connection Dialog, add JDBC URL as
    `jdbc:cassandra://;AuthMech=<2>;UID=token;PWD=<ApplicationToken>;SecureConnectionBundlePath=<PATH TO YOUR SECURE CONNECT BUNDLE>;TunableConsistency=<6>` with the following variables:

    - **AuthMech:** Specifies whether the driver connects to a Cassandra or Astra DB database and whether the driver authenticates the connection.
    - **ApplicationToken:** Generated from Astra DB console. See [Manage application tokens.](https://docs.datastax.com/en/astra/docs/manage-application-tokens.html)
    - **SecureConnectionBundlePath:** Path to where your downloaded Secure Connect Bundle is located. See [Get secure connect bundle.](https://docs.datastax.com/en/astra/docs/obtaining-database-credentials.html)
    - **TunableConsistency:** Specifies Cassandra replica or the number of Cassandra replicas that must process a query for the query to be considered successful.

14. Select **Connect**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-url.png?raw=true" height="500px" />

15. In the **Select Schemas/Catalogs**, select the keyspace to which you want to connect.
16. Select **OK.**
<img src="https://github.com/datastaxdevs/awesome-astra/blob/main/dbschema/dbschema-connetion-established.png?raw=true" height="500px" />

**✅ Step 4: Final Test**

Now that your connection is working, you can create tables, introspect your keyspaces, view your data in the DBSchema GUI, and more.

To learn more about DBSchema, see [Quick start with DBSchema](https://dbschema.com/tutorials.html)

[🏠 Back to HOME](https://awesome-astra.github.io/docs/) 