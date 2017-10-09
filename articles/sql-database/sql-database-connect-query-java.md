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
# <a name="use-java-tooquery-an-azure-sql-database"></a><span data-ttu-id="7fb59-103">Använd Java tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="7fb59-103">Use Java tooquery an Azure SQL database</span></span>

<span data-ttu-id="7fb59-104">Den här snabbstartsguide visar hur toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL-databas och sedan använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="7fb59-104">This quick start demonstrates how toouse [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) tooconnect tooan Azure SQL database and then use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7fb59-105">Krav</span><span class="sxs-lookup"><span data-stu-id="7fb59-105">Prerequisites</span></span>

<span data-ttu-id="7fb59-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="7fb59-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="7fb59-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7fb59-107">An Azure SQL database.</span></span> <span data-ttu-id="7fb59-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="7fb59-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="7fb59-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="7fb59-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="7fb59-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="7fb59-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="7fb59-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="7fb59-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="7fb59-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="7fb59-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="7fb59-113">Du har installerat Java och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="7fb59-113">You have installed Java and related software for your operating system.</span></span>

    - <span data-ttu-id="7fb59-114">**MacOS**: Installera Homebrew och Java och installera sedan Maven.</span><span class="sxs-lookup"><span data-stu-id="7fb59-114">**MacOS**: Install Homebrew and Java, and then install Maven.</span></span> <span data-ttu-id="7fb59-115">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span><span class="sxs-lookup"><span data-stu-id="7fb59-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).</span></span>
    - <span data-ttu-id="7fb59-116">**Ubuntu**: installerar hello Java Development Kit och Maven.</span><span class="sxs-lookup"><span data-stu-id="7fb59-116">**Ubuntu**:  Install hello Java Development Kit, and install Maven.</span></span> <span data-ttu-id="7fb59-117">Se [Steg 1.2, 1.3 och 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="7fb59-117">See [Step 1.2, 1.3, and 1.4](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).</span></span>
    - <span data-ttu-id="7fb59-118">**Windows**: Installera hello Java Development Kit och Maven.</span><span class="sxs-lookup"><span data-stu-id="7fb59-118">**Windows**: Install hello Java Development Kit, and Maven.</span></span> <span data-ttu-id="7fb59-119">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span><span class="sxs-lookup"><span data-stu-id="7fb59-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="7fb59-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="7fb59-120">SQL server connection information</span></span>

<span data-ttu-id="7fb59-121">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7fb59-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="7fb59-122">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="7fb59-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="7fb59-123">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7fb59-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="7fb59-124">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="7fb59-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="7fb59-125">På hello **översikt** för din databas kan du granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello: du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="7fb59-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image: You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="7fb59-127">Om du glömmer bort inloggningsinformationen server navigera toohello SQL Database server sidan tooview hello namn på serveradministratör.</span><span class="sxs-lookup"><span data-stu-id="7fb59-127">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name.</span></span>  <span data-ttu-id="7fb59-128">Om det behövs, Återställ hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="7fb59-128">If necessary, reset hello password.</span></span>     

## <a name="create-maven-project-and-dependencies"></a><span data-ttu-id="7fb59-129">**Skapa Maven-projekt och beroenden**</span><span class="sxs-lookup"><span data-stu-id="7fb59-129">**Create Maven project and dependencies**</span></span>
1. <span data-ttu-id="7fb59-130">Skapa ett nytt Maven-projekt som anropas från hello terminal, **sqltest**.</span><span class="sxs-lookup"><span data-stu-id="7fb59-130">From hello terminal, create a new Maven project called **sqltest**.</span></span> 

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

2. <span data-ttu-id="7fb59-131">Ange **Y** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="7fb59-131">Enter **Y** when prompted.</span></span>
3. <span data-ttu-id="7fb59-132">Ändra katalogen för**sqltest** och öppna ***pom.xml*** med valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7fb59-132">Change directory too**sqltest** and open ***pom.xml*** with your favorite text editor.</span></span>  <span data-ttu-id="7fb59-133">Lägg till hello **Microsoft JDBC-drivrutinen för SQL Server** tooyour projekt beroenden med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="7fb59-133">Add hello **Microsoft JDBC Driver for SQL Server** tooyour project's dependencies using hello following code:</span></span>

   ```xml
   <dependency>
       <groupId>com.microsoft.sqlserver</groupId>
       <artifactId>mssql-jdbc</artifactId>
       <version>6.2.1.jre8</version>
   </dependency>
   ```

4. <span data-ttu-id="7fb59-134">Även i ***pom.xml***, Lägg till följande egenskaper tooyour projekt hello.</span><span class="sxs-lookup"><span data-stu-id="7fb59-134">Also in ***pom.xml***, add hello following properties tooyour project.</span></span>  <span data-ttu-id="7fb59-135">Om du inte har ett avsnitt för egenskaper kan du lägga till efter hello beroenden.</span><span class="sxs-lookup"><span data-stu-id="7fb59-135">If you don't have a properties section, you can add it after hello dependencies.</span></span>

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

5. <span data-ttu-id="7fb59-136">Spara och stäng ***pom.xml***.</span><span class="sxs-lookup"><span data-stu-id="7fb59-136">Save and close ***pom.xml***.</span></span>

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="7fb59-137">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="7fb59-137">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="7fb59-138">Du bör redan ha en fil med namnet ***App.java*** i ditt Maven-projekt som finns i: ...\sqltest\src\main\java\com\sqlsamples\App.Java</span><span class="sxs-lookup"><span data-stu-id="7fb59-138">You should already have a file called ***App.java*** in your Maven project located at:  ..\sqltest\src\main\java\com\sqlsamples\App.java</span></span>

2. <span data-ttu-id="7fb59-139">Öppna filen hello och Ersätt innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7fb59-139">Open hello file and replace its contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="7fb59-140">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="7fb59-140">Run hello code</span></span>

1. <span data-ttu-id="7fb59-141">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="7fb59-141">At hello command prompt, run hello following commands:</span></span>

   ```bash
   mvn package
   mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

2. <span data-ttu-id="7fb59-142">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="7fb59-142">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7fb59-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fb59-143">Next steps</span></span>
- [<span data-ttu-id="7fb59-144">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="7fb59-144">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="7fb59-145">Microsoft JDBC-drivrutin för SQL Server</span><span class="sxs-lookup"><span data-stu-id="7fb59-145">Microsoft JDBC Driver for SQL Server</span></span>](https://github.com/microsoft/mssql-jdbc)
- [<span data-ttu-id="7fb59-146">Rapportera problem/ställ frågor</span><span class="sxs-lookup"><span data-stu-id="7fb59-146">Report issues/ask questions</span></span>](https://github.com/microsoft/mssql-jdbc/issues)

