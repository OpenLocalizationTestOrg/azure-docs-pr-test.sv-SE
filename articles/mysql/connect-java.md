---
title: "Ansluta tooAzure databas för MySQL använder Java | Microsoft Docs"
description: "Denna Snabbstart innehåller en Java-kodexempel som du kan använda tooconnect och fråga efter data från en Azure-databas för MySQL-databas."
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
ms.openlocfilehash: d584b5491d29700b36fae26800c59d93ceb3e265
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-java-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd Java tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med hjälp av ett Java-program. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Java och att du är ny tooworking med Azure-databas för MySQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Du måste också:
- Hämta hello JDBC-drivrutinen [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)
- Inkludera hello JDBC jar-filen (till exempel mysql-connector-java-5.1.42-bin.jar) i program-klassökvägen. Om du har problem med detta kan du läsa dokumentationen för miljön för information om klassens sökväg, som [Apache Tomcat](https://tomcat.apache.org/tomcat-7.0-doc/class-loader-howto.html) eller [Java SE](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)
- Kontrollera din Azure-databas för MySQL anslutningssäkerhet har konfigurerats med hello brandväggen har öppnats och SSL-inställningar justerad för ditt program tooconnect har.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänster klickar du på **alla resurser**, och sök sedan efter hello-server som du har skapat (till exempel **myserver4demo**).
3. Klicka på hello servernamn.
4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för MySQL-servernamn](./media/connect-java/1_server-properties-name-login.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande kod tooconnect och Läs in hello data med hjälp av hello funktion med en **infoga** SQL-instruktionen. Hej [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metod är att använda tooconnect tooMySQL. Metoder [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) och execute() används toodrop och skapa hello tabell. Hej prepareStatement objektet är används toobuild hello insert-kommandon, med setString() och setInt() toobind hello parametervärden. Metoden executeUpdate() kör hello-kommando för varje uppsättning parametrar tooinsert hello värden. 

Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.

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

        // check that hello driver is installed
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

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooread hello data med en **Välj** SQL-instruktionen. Hej [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metod är att använda tooconnect tooMySQL. Hej metoder [createStatement()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-statements.html) och executeQuery() används tooconnect och kör hello select-instruktion. hello resultat bearbetas med en [ResultSet](https://docs.oracle.com/javase/tutorial/jdbc/basics/retrieving.html) objekt. 

Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.

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

        // check that hello driver is installed
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
            throw new SQLException("Failed toocreate connection toodatabase", e);
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
                throw new SQLException("Encountered an error when executing given sql statement", e);
            }       
        }
        else {
            System.out.println("Failed toocreate connection toodatabase."); 
        }
        System.out.println("Execution finished.");
    }
}
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod toochange hello data med en **uppdatering** SQL-instruktionen. Hej [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metod är att använda tooconnect tooMySQL. Hej metoder [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) och executeUpdate() används tooprepare och kör hello update-instruktion. 

Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.

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

        // check that hello driver is installed
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
            
            // set up hello connection properties
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

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooremove data med en **ta bort** SQL-instruktionen. Hej [getConnection()](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-usagenotes-connect-drivermanager.html) metod är att använda tooconnect tooMySQL.  Hej metoder [prepareStatement()](http://docs.oracle.com/javase/tutorial/jdbc/basics/prepared.html) och executeUpdate() används tooprepare och kör hello update-instruktion. 

Ersätt hello värden, databas, användare och lösenordsparametrar med hello-värden som du angav när du skapade din egen server och databas.

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
        
        // check that hello driver is installed
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
            
            // set up hello connection properties
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
            throw new SQLException("Failed toocreate connection toodatabase", e);
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

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din MySQL-databas tooAzure databas för MySQL med dump och återställning](concepts-migrate-dump-restore.md)
