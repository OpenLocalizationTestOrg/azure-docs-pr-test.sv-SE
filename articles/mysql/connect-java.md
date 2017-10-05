---
title: "Ansluta till Azure Database för MySQL med Java | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i Java som du kan använda för att ansluta till och fråga efter data från en Azure Database för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.devlang: java
ms.date: 06/20/2017
ms.openlocfilehash: 6ffcf3b38a3d868dfa10ea2e2a9d097441387d4f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-java-to-connect-and-query-data"></a><span data-ttu-id="0a9c0-103">Azure-databas för MySQL: Använd Java för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="0a9c0-103">Azure Database for MySQL: Use Java to connect and query data</span></span>
<span data-ttu-id="0a9c0-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett Java-program.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a Java application.</span></span> <span data-ttu-id="0a9c0-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="0a9c0-106">I den här artikeln förutsätter vi att du har kunskaper om Java och att du inte har arbetat med Azure Database för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-106">The steps in this article assume that you are familiar with developing using Java, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a9c0-107">Krav</span><span class="sxs-lookup"><span data-stu-id="0a9c0-107">Prerequisites</span></span>
<span data-ttu-id="0a9c0-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="0a9c0-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="0a9c0-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0a9c0-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="0a9c0-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0a9c0-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="0a9c0-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="0a9c0-111">You also need to:</span></span>
- <span data-ttu-id="0a9c0-112">Hämta JDBC-drivrutinen [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span><span class="sxs-lookup"><span data-stu-id="0a9c0-112">Download the JDBC driver [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)</span></span>
- <span data-ttu-id="0a9c0-113">Ta med JDBC jar-filen (till exempel mysql-connector-java-5.1.42-bin.jar) i klassökvägen i programmet.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-113">Include the JDBC jar file (for example mysql-connector-java-5.1.42-bin.jar) into your application classpath.</span></span> <span data-ttu-id="0a9c0-114">Om du har problem med detta kan du läsa dokumentationen för miljön för information om klassens sökväg, som [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) eller [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)</span><span class="sxs-lookup"><span data-stu-id="0a9c0-114">If you have trouble with this, please consult your environment's documentation for class path specifics, such as [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) or [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)</span></span>
- <span data-ttu-id="0a9c0-115">Se till att säkerhetsinställningarna för Azure Database för MySQL-anslutning konfigureras med brandväggen öppen och att SSL-inställningarna justeras så att programmet kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-115">Ensure your Azure Database for MySQL connection security is configured with the firewall opened and SSL settings adjusted for your application to connect successfully.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="0a9c0-116">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="0a9c0-116">Get connection information</span></span>
<span data-ttu-id="0a9c0-117">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-117">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="0a9c0-118">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-118">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="0a9c0-119">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0a9c0-119">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0a9c0-120">I den vänstra rutan klickar du på **alla resurser** och söker sedan efter servern som du skapade (till exempel **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="0a9c0-120">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="0a9c0-121">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-121">Click the server name.</span></span>
4. <span data-ttu-id="0a9c0-122">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-122">Select the server's **Properties** page.</span></span> <span data-ttu-id="0a9c0-123">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-123">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="0a9c0-124">![Azure Database för MySQL-servernamn](./media/connect-java/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="0a9c0-124">![Azure Database for MySQL server name](./media/connect-java/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="0a9c0-125">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-125">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="0a9c0-126">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="0a9c0-126">Connect, create table, and insert data</span></span>
<span data-ttu-id="0a9c0-127">Använd följande kod för att ansluta och läsa in data med hjälp av en funktion med en **INSERT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-127">Use the following code to connect and load the data using the function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="0a9c0-128">Metoden [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) används för att ansluta till MySQL.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-128">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="0a9c0-129">Metoderna [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) och execute() används för att släppa och skapa tabellen.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-129">Methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and execute() are used to drop and create the table.</span></span> <span data-ttu-id="0a9c0-130">Objektet prepareStatement används för att skapa infogningskommandon och setString() och setInt() används för att binda parametervärdena.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-130">The prepareStatement object is used to build the insert commands, with setString() and setInt() to bind the parameter values.</span></span> <span data-ttu-id="0a9c0-131">Metoden executeUpdate() infogar värdena genom att kommandot körs för varje uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-131">Method executeUpdate() runs the command for each set of parameters to insert the values.</span></span> 

<span data-ttu-id="0a9c0-132">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-132">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Drop previous table of same name if one exists.
                Statement statement = connection.createStatement();
                statement.execute("DROP TABLE IF EXISTS inventory;");
                System.out.println("Finished dropping table (if existed).");
    
                // Create table.
                statement.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
                System.out.println("Created table.");
                
                // Insert some data into table.
                int nRowsInserted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO inventory (name, quantity) VALUES (?, ?);");
                preparedStatement.setString(1, "banana");
                preparedStatement.setInt(2, 150);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "orange");
                preparedStatement.setInt(2, 154);
                nRowsInserted += preparedStatement.executeUpdate();

                preparedStatement.setString(1, "apple");
                preparedStatement.setInt(2, 100);
                nRowsInserted += preparedStatement.executeUpdate();
                System.out.println(String.format("Inserted %d row(s) of data.", nRowsInserted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="read-data"></a><span data-ttu-id="0a9c0-133">Läsa data</span><span class="sxs-lookup"><span data-stu-id="0a9c0-133">Read data</span></span>
<span data-ttu-id="0a9c0-134">Använd följande kod för att läsa in data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-134">Use the following code to read the data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="0a9c0-135">Metoden [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) används för att ansluta till MySQL.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-135">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="0a9c0-136">Metoderna [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) och executeQuery() används för att ansluta och köra select-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-136">The methods [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) and executeQuery() are used to connect and run the select statement.</span></span> <span data-ttu-id="0a9c0-137">Resultaten bearbetas med ett [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html)-objekt.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-137">The results are processed using a [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) object.</span></span> 

<span data-ttu-id="0a9c0-138">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-138">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);

            // Set connection properties.
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
    
                Statement statement = connection.createStatement();
                ResultSet results = statement.executeQuery("SELECT * from inventory;");
                while (results.next())
                {
                    String outputString = 
                        String.format(
                            "Data row = (%s, %s, %s)",
                            results.getString(1),
                            results.getString(2),
                            results.getString(3));
                    System.out.println(outputString);
                }
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a><span data-ttu-id="0a9c0-139">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="0a9c0-139">Update data</span></span>
<span data-ttu-id="0a9c0-140">Använd följande kod för att ändra data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-140">Use the following code to change the data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="0a9c0-141">Metoden [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) används för att ansluta till MySQL.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-141">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span> <span data-ttu-id="0a9c0-142">Metoderna [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) och executeUpdate() används för att förbereda och köra update-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-142">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="0a9c0-143">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-143">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables. 
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }
        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="delete-data"></a><span data-ttu-id="0a9c0-144">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="0a9c0-144">Delete data</span></span>
<span data-ttu-id="0a9c0-145">Använd följande kod för att ta bort data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-145">Use the following code to remove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="0a9c0-146">Metoden [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) används för att ansluta till MySQL.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-146">The [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) method is used to connect to MySQL.</span></span>  <span data-ttu-id="0a9c0-147">Metoderna [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) och executeUpdate() används för att förbereda och köra update-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-147">The methods [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) and executeUpdate() are used to prepare and run the update statement.</span></span> 

<span data-ttu-id="0a9c0-148">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="0a9c0-148">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {
        // Initialize connection variables.
        String host = "myserver4demo.mysql.database.azure.com";
        String database = "quickstartdb";
        String user = "myadmin@myserver4demo";
        String password = "<server_admin_password>";
        
        // check that the driver is installed
        try
        {
            Class.forName("com.mysql.jdbc.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("MySQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("MySQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:mysql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("useSSL", "true");
            properties.setProperty("verifyServerCertificate", "true");
            properties.setProperty("requireSSL", "false");
            
            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed to create connection to database", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection to database.");
        
            // Perform some SQL queries over the connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need to commit all changes to database, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed to create connection to database.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="0a9c0-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a9c0-149">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0a9c0-150">Migrera MySQL-databasen till Azure Database för MySQL med säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="0a9c0-150">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
