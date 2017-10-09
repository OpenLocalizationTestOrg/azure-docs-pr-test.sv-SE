---
title: "Ansluta tooAzure databas för PostgreSQL använder Java | Microsoft Docs"
description: "Denna Snabbstart innehåller en Java-kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
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
ms.openlocfilehash: 8f6e0a47a0d6dfebf29eb56c31ccccabd7c2b670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-java-tooconnect-and-query-data"></a><span data-ttu-id="c4034-103">Azure-databas för PostgreSQL: Använd Java tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="c4034-103">Azure Database for PostgreSQL: Use Java tooconnect and query data</span></span>
<span data-ttu-id="c4034-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med hjälp av ett Java-program.</span><span class="sxs-lookup"><span data-stu-id="c4034-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a Java application.</span></span> <span data-ttu-id="c4034-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="c4034-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="c4034-106">hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Java och att du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c4034-106">hello steps in this article assume that you are familiar with developing using Java, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4034-107">Krav</span><span class="sxs-lookup"><span data-stu-id="c4034-107">Prerequisites</span></span>
<span data-ttu-id="c4034-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="c4034-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c4034-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="c4034-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="c4034-110">Skapa DB – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c4034-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="c4034-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="c4034-111">You also need to:</span></span>
- <span data-ttu-id="c4034-112">Hämta hello [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matchar din version av Java och hello Java Development Kit.</span><span class="sxs-lookup"><span data-stu-id="c4034-112">Download hello [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html) matching your version of Java and hello Java Development Kit.</span></span>
- <span data-ttu-id="c4034-113">Inkludera hello PostgreSQL JDBC jar-filen (till exempel postgresql-42.1.1.jar) i program-klassökvägen.</span><span class="sxs-lookup"><span data-stu-id="c4034-113">Include hello PostgreSQL JDBC jar file (for example postgresql-42.1.1.jar) in your application classpath.</span></span> <span data-ttu-id="c4034-114">För mer information, se [information om classpath](https://jdbc.postgresql.org/documentation/head/classpath.html).</span><span class="sxs-lookup"><span data-stu-id="c4034-114">For more information, see [classpath details](https://jdbc.postgresql.org/documentation/head/classpath.html).</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c4034-115">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="c4034-115">Get connection information</span></span>
<span data-ttu-id="c4034-116">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c4034-116">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="c4034-117">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4034-117">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c4034-118">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c4034-118">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c4034-119">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="c4034-119">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="c4034-120">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="c4034-120">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="c4034-121">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="c4034-121">Select hello server's **Overview** page.</span></span> <span data-ttu-id="c4034-122">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="c4034-122">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c4034-123">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-java/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="c4034-123">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-java/1-connection-string.png)</span></span>
5. <span data-ttu-id="c4034-124">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c4034-124">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="c4034-125">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="c4034-125">Connect, create table, and insert data</span></span>
<span data-ttu-id="c4034-126">Använd hello följande kod tooconnect och Läs in hello data med hjälp av hello funktion med en **infoga** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c4034-126">Use hello following code tooconnect and load hello data using hello function with an **INSERT** SQL statement.</span></span> <span data-ttu-id="c4034-127">Hej metoder [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), och [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) är används tooconnect släpp och skapa hello tabell.</span><span class="sxs-lookup"><span data-stu-id="c4034-127">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, drop, and create hello table.</span></span> <span data-ttu-id="c4034-128">Hej [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) objektet är används toobuild hello insert-kommandon, med setString() och setInt() toobind hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="c4034-128">hello [prepareStatement](https://jdbc.postgresql.org/documentation/head/query.html) object is used toobuild hello insert commands, with setString() and setInt() toobind hello parameter values.</span></span> <span data-ttu-id="c4034-129">Metoden [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) körs hello-kommando för varje uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="c4034-129">Method [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) runs hello command for each set of parameters.</span></span> 

<span data-ttu-id="c4034-130">Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.</span><span class="sxs-lookup"><span data-stu-id="c4034-130">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
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
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
    
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="read-data"></a><span data-ttu-id="c4034-131">Läsa data</span><span class="sxs-lookup"><span data-stu-id="c4034-131">Read data</span></span>
<span data-ttu-id="c4034-132">Använd hello följande kod tooread hello data med en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c4034-132">Use hello following code tooread hello data with a **SELECT** SQL statement.</span></span> <span data-ttu-id="c4034-133">Hej metoder [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), och [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) är används tooconnect skapa och köra hello select-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c4034-133">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [createStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeQuery()](https://jdbc.postgresql.org/documentation/head/query.html) are used tooconnect, create, and run hello select statement.</span></span> <span data-ttu-id="c4034-134">hello resultat bearbetas med en [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) objekt.</span><span class="sxs-lookup"><span data-stu-id="c4034-134">hello results are processed using a [ResultSet](https://www.postgresql.org/docs/7.4/static/jdbc-query.html) object.</span></span> 

<span data-ttu-id="c4034-135">Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.</span><span class="sxs-lookup"><span data-stu-id="c4034-135">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
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
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}

```

## <a name="update-data"></a><span data-ttu-id="c4034-136">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="c4034-136">Update data</span></span>
<span data-ttu-id="c4034-137">Använd hello följande kod toochange hello data med en **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c4034-137">Use hello following code toochange hello data with an **UPDATE** SQL statement.</span></span> <span data-ttu-id="c4034-138">Hej metoder [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), och [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) är används tooconnect förbereda och kör hello update-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c4034-138">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello update statement.</span></span> 

<span data-ttu-id="c4034-139">Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.</span><span class="sxs-lookup"><span data-stu-id="c4034-139">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Modify some data in table.
                int nRowsUpdated = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?;");
                preparedStatement.setInt(1, 200);
                preparedStatement.setString(2, "banana");
                nRowsUpdated += preparedStatement.executeUpdate();
                System.out.println(String.format("Updated %d row(s) of data.", nRowsUpdated));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```
## <a name="delete-data"></a><span data-ttu-id="c4034-140">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="c4034-140">Delete data</span></span>
<span data-ttu-id="c4034-141">Använd hello följande kod tooremove data med en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="c4034-141">Use hello following code tooremove data with a **DELETE** SQL statement.</span></span> <span data-ttu-id="c4034-142">Hej metoder [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), och [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) är används tooconnect förbereda och kör hello delete-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c4034-142">hello methods [getConnection()](https://www.postgresql.org/docs/7.4/static/jdbc-use.html), [prepareStatement()](https://jdbc.postgresql.org/documentation/head/query.html), and [executeUpdate()](https://jdbc.postgresql.org/documentation/head/update.html) are used tooconnect, prepare, and run hello delete statement.</span></span> 

<span data-ttu-id="c4034-143">Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.</span><span class="sxs-lookup"><span data-stu-id="c4034-143">Replace hello host, database, user, and password parameters with hello values that you specified when you created your own server and database.</span></span>

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
            Properties properties = new Properties();
            properties.setProperty("user", user);
            properties.setProperty("password", password);
            properties.setProperty("ssl", "true");

            // get connection
            connection = DriverManager.getConnection(url, properties);
        }
        catch (SQLException e)
        {
            throw new SQLException("Failed toocreate connection toodatabase.", e);
        }
        if (connection != null) 
        { 
            System.out.println("Successfully created connection toodatabase.");
        
            // Perform some SQL queries over hello connection.
            try
            {
                // Delete some data from table.
                int nRowsDeleted = 0;
                PreparedStatement preparedStatement = connection.prepareStatement("DELETE FROM inventory WHERE name = ?;");
                preparedStatement.setString(1, "orange");
                nRowsDeleted += preparedStatement.executeUpdate();
                System.out.println(String.format("Deleted %d row(s) of data.", nRowsDeleted));
    
                // NOTE No need toocommit all changes toodatabase, as auto-commit is enabled by default.
            }
            catch (SQLException e)
            {
                throw new SQLException("Encountered an error when executing given sql statement.", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase.");
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="c4034-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4034-144">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c4034-145">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="c4034-145">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
