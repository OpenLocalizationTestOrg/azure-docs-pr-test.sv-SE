---
title: "Fråga Azure SQL Database med Java | Microsoft Docs"
description: "Det här avsnittet visar hur du använder Java för att skapa ett program som ansluter till en Azure SQL Database och frågar den med hjälp av Transact-SQL-uttryck."
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
ms.openlocfilehash: 103b0755ab89a13297cfdc9ec72416664da8c1e9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-java-to-query-an-azure-sql-database"></a><span data-ttu-id="b7189-103">Fråga Azure SQL Database med Java</span><span class="sxs-lookup"><span data-stu-id="b7189-103">Use Java to query an Azure SQL database</span></span>

<span data-ttu-id="b7189-104">Den här snabbstarten visar hur du använder [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) för att ansluta till en Azure SQL-databas och sedan använda Transact-SQL-uttryck för att fråga data.</span><span class="sxs-lookup"><span data-stu-id="b7189-104">This quick start demonstrates how to use [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) to connect to an Azure SQL database and then use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7189-105">Krav</span><span class="sxs-lookup"><span data-stu-id="b7189-105">Prerequisites</span></span>

<span data-ttu-id="b7189-106">För att kunna slutföra den här snabbstartskursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b7189-106">To complete this quick start tutorial, make sure you have the following prerequisites:</span></span>

- <span data-ttu-id="b7189-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b7189-107">An Azure SQL database.</span></span> <span data-ttu-id="b7189-108">Den här snabbstarten använder resurser som har skapats i någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="b7189-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="b7189-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="b7189-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="b7189-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="b7189-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="b7189-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7189-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="b7189-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för den offentliga IP-adressen för den dator du använder för den här snabbstartskursen.</span><span class="sxs-lookup"><span data-stu-id="b7189-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="b7189-113">Du har installerat Java och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="b7189-113">You have installed Java and related software for your operating system.</span></span>

    - <span data-ttu-id="b7189-114">**MacOS**: Installera Homebrew och Java och installera sedan Maven.</span><span class="sxs-lookup"><span data-stu-id="b7189-114">**MacOS**: Install Homebrew and Java, and then install Maven.</span></span> <span data-ttu-id="b7189-115">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span><span class="sxs-lookup"><span data-stu-id="b7189-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span></span>
    - <span data-ttu-id="b7189-116">**Ubuntu**: Installera Java Development Kit och Maven.</span><span class="sxs-lookup"><span data-stu-id="b7189-116">**Ubuntu**:  Install the Java Development Kit, and install Maven.</span></span> <span data-ttu-id="b7189-117">Se [Steg 1.2, 1.3 och 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="b7189-117">See [Step 1.2, 1.3, and 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span></span>
    - <span data-ttu-id="b7189-118">**Windows**: Installera Java Development Kit och Maven.</span><span class="sxs-lookup"><span data-stu-id="b7189-118">**Windows**: Install the Java Development Kit, and Maven.</span></span> <span data-ttu-id="b7189-119">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span><span class="sxs-lookup"><span data-stu-id="b7189-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="b7189-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="b7189-120">SQL server connection information</span></span>

<span data-ttu-id="b7189-121">Skaffa den anslutningsinformation du behöver för att ansluta till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b7189-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="b7189-122">Du behöver det fullständiga servernamnet, databasnamnet och inloggningsinformationen i nästa procedurer.</span><span class="sxs-lookup"><span data-stu-id="b7189-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="b7189-123">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b7189-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b7189-124">Välj **SQL-databaser** på den vänstra menyn och klicka på databasen på sidan **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="b7189-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="b7189-125">Granska serverns fullständiga namn på sidan **Översikt** för databasen enligt vad som visas på bilden nedan. Du kan hovra över servernamnet för att se alternativet **Klicka för att kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b7189-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image: You can hover over the server name to bring up the **Click to copy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="b7189-127">Om du glömmer inloggningsinformationen för din server öppnar du serversidan i SQL Database. Där ser du administratörsnamnet för servern.</span><span class="sxs-lookup"><span data-stu-id="b7189-127">If you forget your server login information, navigate to the SQL Database server page to view the server admin name.</span></span>  <span data-ttu-id="b7189-128">Du kan återställa lösenordet om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b7189-128">If necessary, reset the password.</span></span>     

## <a name="create-maven-project-and-dependencies"></a><span data-ttu-id="b7189-129">**Skapa Maven-projekt och beroenden**</span><span class="sxs-lookup"><span data-stu-id="b7189-129">**Create Maven project and dependencies**</span></span>
1. <span data-ttu-id="b7189-130">Skapa ett nytt Maven-projekt som du namnger **sqltest** från terminalen.</span><span class="sxs-lookup"><span data-stu-id="b7189-130">From the terminal, create a new Maven project called **sqltest**.</span></span> 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. <span data-ttu-id="b7189-131">Ange **Y** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="b7189-131">Enter **Y** when prompted.</span></span>
3. <span data-ttu-id="b7189-132">Gå till katalogen för **sqltest** och öppna ***pom.xml*** med valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b7189-132">Change directory to **sqltest** and open ***pom.xml*** with your favorite text editor.</span></span>  <span data-ttu-id="b7189-133">Lägg till **Microsoft JDBC-drivrutinen för SQL Server** i projektets beroenden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b7189-133">Add the **Microsoft JDBC Driver for SQL Server** to your project's dependencies using the following code:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. <span data-ttu-id="b7189-134">Lägg även till följande egenskaper i projektet i filen ***pom.xml***.</span><span class="sxs-lookup"><span data-stu-id="b7189-134">Also in ***pom.xml***, add the following properties to your project.</span></span>  <span data-ttu-id="b7189-135">Om du inte har ett avsnitt för egenskaper kan du lägga till efter beroendena.</span><span class="sxs-lookup"><span data-stu-id="b7189-135">If you don't have a properties section, you can add it after the dependencies.</span></span>

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. <span data-ttu-id="b7189-136">Spara och stäng ***pom.xml***.</span><span class="sxs-lookup"><span data-stu-id="b7189-136">Save and close ***pom.xml***.</span></span>

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="b7189-137">Infoga kod för att fråga SQL Database</span><span class="sxs-lookup"><span data-stu-id="b7189-137">Insert code to query SQL database</span></span>

1. <span data-ttu-id="b7189-138">Du bör redan ha en fil med namnet ***App.java*** i ditt Maven-projekt som finns i: ...\sqltest\src\main\java\com\sqlsamples\App.Java</span><span class="sxs-lookup"><span data-stu-id="b7189-138">You should already have a file called ***App.java*** in your Maven project located at:  ..\sqltest\src\main\java\com\sqlsamples\App.java</span></span>

2. <span data-ttu-id="b7189-139">Öppna filen och ersätt innehållet med följande kod och lägg till lämpliga värden för server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b7189-139">Open the file and replace its contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.DriverManager;

   public class App {

    public static void main(String[] args) {
    
        // Connect to database
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

## <a name="run-the-code"></a><span data-ttu-id="b7189-140">Kör koden</span><span class="sxs-lookup"><span data-stu-id="b7189-140">Run the code</span></span>

1. <span data-ttu-id="b7189-141">Kör följande kommandon i kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="b7189-141">At the command prompt, run the following commands:</span></span>

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. <span data-ttu-id="b7189-142">Kontrollera att de 20 översta raderna returneras och stäng sedan programfönstret.</span><span class="sxs-lookup"><span data-stu-id="b7189-142">Verify that the top 20 rows are returned and then close the application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b7189-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7189-143">Next steps</span></span>
- [<span data-ttu-id="b7189-144">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b7189-144">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="b7189-145">Microsoft JDBC-drivrutin för SQL Server</span><span class="sxs-lookup"><span data-stu-id="b7189-145">Microsoft JDBC Driver for SQL Server</span></span>](https://github.com/microsoft/mssql-jdbc)
- [<span data-ttu-id="b7189-146">Rapportera problem/ställ frågor</span><span class="sxs-lookup"><span data-stu-id="b7189-146">Report issues/ask questions</span></span>](https://github.com/microsoft/mssql-jdbc/issues)

