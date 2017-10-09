---
title: aaaUse Java tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Java toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: andrela
ms.openlocfilehash: f014edbe38ca0e7b6e43f4eb4d2e53d3561bf3e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-java-tooquery-an-azure-sql-database"></a>Använd Java tooquery en Azure SQL database

Den här snabbstartsguide visar hur toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL-databas och sedan använda Transact-SQL-instruktioner tooquery data.

## <a name="prerequisites"></a>Krav

toocomplete detta snabb start kursen, kontrollera att du har hello följande krav:

- En Azure SQL-databas. Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter: 

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.

- Du har installerat Java och relaterad programvara för ditt operativsystem.

    - **MacOS**: Installera Homebrew och Java och installera sedan Maven. Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).
    - **Ubuntu**: installerar hello Java Development Kit och Maven. Se [Steg 1.2, 1.3 och 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).
    - **Windows**: Installera hello Java Development Kit och Maven. Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).    

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas. Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. På hello **översikt** för din databas kan du granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello: du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Om du glömmer bort inloggningsinformationen server navigera toohello SQL Database server sidan tooview hello namn på serveradministratör.  Om det behövs, Återställ hello lösenord.     

## <a name="create-maven-project-and-dependencies"></a>**Skapa Maven-projekt och beroenden**
1. Skapa ett nytt Maven-projekt som anropas från hello terminal, **sqltest**. 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. Ange **Y** när du tillfrågas.
3. Ändra katalogen för**sqltest** och öppna ***pom.xml*** med valfri textredigerare.  Lägg till hello **Microsoft JDBC-drivrutinen för SQL Server** tooyour projekt beroenden med hello följande kod:

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. Även i ***pom.xml***, Lägg till följande egenskaper tooyour projekt hello.  Om du inte har ett avsnitt för egenskaper kan du lägga till efter hello beroenden.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. Spara och stäng ***pom.xml***.

## <a name="insert-code-tooquery-sql-database"></a>Infoga kod tooquery SQL-databas

1. Du bör redan ha en fil med namnet ***App.java*** i ditt Maven-projekt som finns i: ...\sqltest\src\main\java\com\sqlsamples\App.Java

2. Öppna filen hello och Ersätt innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect toodatabase
           String hostName = "your_server.database.windows.net";
           String dbName = "your_database";
           String user = "your_username";
           String password = "your_password";
           String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
           Connection connection = null;

           try {
                   connection = DriverManager.getConnection(url);
                   String schema = connection.getSchema();
                   System.out.println("Successful connection - Schema: " + schema);

                   System.out.println("Query data example:");
                   System.out.println("=========================================");

                   // Create and execute a SELECT SQL statement.
                   String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                       + "FROM [SalesLT].[ProductCategory] pc "  
                       + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                   try (Statement statement = connection.createStatement();
                       ResultSet resultSet = statement.executeQuery(selectSql)) {

                           // Print results from select statement
                           System.out.println("Top 20 categories:");
                           while (resultSet.next())
                           {
                               System.out.println(resultSet.getString(1) + " "
                                   + resultSet.getString(2));
                           }
                    connection.close();
                   }                   
           }
           catch (Exception e) {
                   e.printStackTrace();
           }
       }
   }
   ```

## <a name="run-hello-code"></a>Köra hello kod

1. Kör följande kommandon hello Kommandotolken hello:

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.


## <a name="next-steps"></a>Nästa steg
- [Utforma din första Azure SQL Database](sql-database-design-first-database.md)
- [Microsoft JDBC-drivrutin för SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Rapportera problem/ställ frågor](https://github.com/microsoft/mssql-jdbc/issues)

