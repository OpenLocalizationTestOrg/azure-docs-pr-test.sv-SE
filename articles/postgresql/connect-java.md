---
title: "Ansluta till Azure Database för PostgreSQL med Java | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i Java som du kan använda för att ansluta till och fråga efter data från Azure Database för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: java
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 730a3f464b4437c260d09abc026a186a0e26293c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-java-to-connect-and-query-data"></a><span data-ttu-id="774d0-103">Azure Database för PostgreSQL: Använda Java för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="774d0-103">Azure Database for PostgreSQL: Use Java to connect and query data</span></span>
<span data-ttu-id="774d0-104">Den här snabbstarten visar hur du ansluter till en Azure Database för PostgreSQL med hjälp av ett Java-program.</span><span class="sxs-lookup"><span data-stu-id="774d0-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using a Java application.</span></span> <span data-ttu-id="774d0-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="774d0-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="774d0-106">I den här artikeln förutsätter vi att du har kunskaper om Java och att du inte har arbetat med Azure Database för PostgreSQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="774d0-106">The steps in this article assume that you are familiar with developing using Java, and that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="774d0-107">Krav</span><span class="sxs-lookup"><span data-stu-id="774d0-107">Prerequisites</span></span>
<span data-ttu-id="774d0-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="774d0-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="774d0-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="774d0-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="774d0-110">Skapa DB – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="774d0-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="774d0-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="774d0-111">You also need to:</span></span>
- <span data-ttu-id="774d0-112">Hämta den [PostgreSQL JDBC-drivrutin](https://jdbc.postgresql.org/download.html) som matchar din version av Java och Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="774d0-112">Download the [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matching your version of Java and the Java Development Kit.</span></span>
- <span data-ttu-id="774d0-113">Inkludera PostgreSQL JDBC jar-filen (till exempel postgresql-42.1.1.jar) i program-klassökvägen.</span><span class="sxs-lookup"><span data-stu-id="774d0-113">Include the PostgreSQL JDBC jar file (for example postgresql-42.1.1.jar) in your application classpath.</span></span> <span data-ttu-id="774d0-114">För mer information, se [information om classpath](https://jdbc.postgresql.org/documentation/head/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="774d0-114">For more information, see [classpath details](https://jdbc.postgresql.org/documentation/head/classpath.html).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="774d0-115">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="774d0-115">Get connection information</span></span>
<span data-ttu-id="774d0-116">Hämta den information som du behöver för att ansluta till Azure Database för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="774d0-116">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="774d0-117">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="774d0-117">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="774d0-118">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="774d0-118">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="774d0-119">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="774d0-119">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="774d0-120">Klicka på servernamnet **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="774d0-120">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="774d0-121">Välj serverns **översikt**-sida.</span><span class="sxs-lookup"><span data-stu-id="774d0-121">Select the server's **Overview** page.</span></span> <span data-ttu-id="774d0-122">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="774d0-122">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="774d0-123">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-java/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="774d0-123">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-java/1-connection-string.png)</span></span>
5. <span data-ttu-id="774d0-124">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="774d0-124">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="774d0-125">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="774d0-125">Connect, create table, and insert data</span></span>
<span data-ttu-id="774d0-126">Använd följande kod för att ansluta och läsa in data med hjälp av en funktion med SQL-instruktionen **INFOGA**.</span><span class="sxs-lookup"><span data-stu-id="774d0-126">Use the following code to connect and load the data using the function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="774d0-127">Metoderna [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), och [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) används för att ansluta, ta bort och skapa tabellen.</span><span class="sxs-lookup"><span data-stu-id="774d0-127">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used to connect, drop, and create the table.</span></span> <span data-ttu-id="774d0-128">Objektet [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) används för att skapa infogningskommandon och setString() och setInt() används för att binda parametervärdena.</span><span class="sxs-lookup"><span data-stu-id="774d0-128">The [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) object is used to build the insert commands, with setString() and setInt() to bind the parameter values.</span></span> <span data-ttu-id="774d0-129">Metoden [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) körs för varje uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="774d0-129">Method [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) runs the command for each set of parameters.</span></span> 

<span data-ttu-id="774d0-130">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="774d0-130">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class CreateTableInsertRows {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

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

## <a name="read-data"></a><span data-ttu-id="774d0-131">Läsa data</span><span class="sxs-lookup"><span data-stu-id="774d0-131">Read data</span></span>
<span data-ttu-id="774d0-132">Använd följande kod för att läsa in data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="774d0-132">Use the following code to read the data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="774d0-133">Metoderna [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html) och [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) används för att ansluta och köra select-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="774d0-133">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used to connect, create, and run the select statement.</span></span> <span data-ttu-id="774d0-134">Resultaten bearbetas med ett [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html)-objekt.</span><span class="sxs-lookup"><span data-stu-id="774d0-134">The results are processed using a [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) object.</span></span> 

<span data-ttu-id="774d0-135">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="774d0-135">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class ReadTable {

    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

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

## <a name="update-data"></a><span data-ttu-id="774d0-136">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="774d0-136">Update data</span></span>
<span data-ttu-id="774d0-137">Använd följande kod för att ändra data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="774d0-137">Use the following code to change the data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="774d0-138">Metoderna [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html) och [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html) och [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) används för att ansluta, förbereda och köra update-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="774d0-138">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used to connect, prepare, and run the update statement.</span></span> 

<span data-ttu-id="774d0-139">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="774d0-139">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class UpdateTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

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
## <a name="delete-data"></a><span data-ttu-id="774d0-140">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="774d0-140">Delete data</span></span>
<span data-ttu-id="774d0-141">Använd följande kod för att ta bort data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="774d0-141">Use the following code to remove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="774d0-142">Metoderna [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html) och [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html) och [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) används för att ansluta, förbereda och köra delete-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="774d0-142">The methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used to connect, prepare, and run the delete statement.</span></span> 

<span data-ttu-id="774d0-143">Ersätt parametrarna host, database, user och password med de värden som du angav när du skapade din server och databas.</span><span class="sxs-lookup"><span data-stu-id="774d0-143">Replace the host, database, user, and password parameters with the values that you specified when you created your own server and database.</span></span>

```java
import java.sql.*;
import java.util.Properties;

public class DeleteTable {
    public static void main (String[] args)  throws Exception
    {

        // Initialize connection variables.
        String host = "mypgserver-20170401.postgres.database.azure.com";
        String database = "mypgsqldb";
        String user = "mylogin@mypgserver-20170401";
        String password = "<server_admin_password>";

        // check that the driver is installed
        try
        {
            Class.forName("org.postgresql.Driver");
        }
        catch (ClassNotFoundException e)
        {
            throw new ClassNotFoundException("PostgreSQL JDBC driver NOT detected in library path.", e);
        }

        System.out.println("PostgreSQL JDBC driver detected in library path.");

        Connection connection = null;

        // Initialize connection object
        try
        {
            String url = String.format("jdbc:postgresql://%s/%s", host, database);
            
            // set up the connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

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

## <a name="next-steps"></a><span data-ttu-id="774d0-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="774d0-144">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="774d0-145">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="774d0-145">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
