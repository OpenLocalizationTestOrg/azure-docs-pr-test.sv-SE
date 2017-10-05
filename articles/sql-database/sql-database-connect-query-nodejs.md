---
title: "Fråga Azure SQL Database med Node.js | Microsoft Docs"
description: "Det här avsnittet visar hur du använder Node.js för att skapa ett program som ansluter till en Azure SQL Database och frågar den med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 1907a95df9132c059d7985b6d5cd913536bf3403
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a><span data-ttu-id="8d71d-103">Använd Node.js för att fråga en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="8d71d-103">Use Node.js to query an Azure SQL database</span></span>

<span data-ttu-id="8d71d-104">Den här snabbstartsguiden visar hur man använder [Node.js](https://nodejs.org/en/) för att skapa ett program som ansluter till en Azure SQL-databas och använder Transact-SQL-uttryck för att fråga data.</span><span class="sxs-lookup"><span data-stu-id="8d71d-104">This quick start tutorial demonstrates how to use [Node.js](https://nodejs.org/en/) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d71d-105">Krav</span><span class="sxs-lookup"><span data-stu-id="8d71d-105">Prerequisites</span></span>

<span data-ttu-id="8d71d-106">Kontrollera att du har följande för att slutföra den här snabbstartskursen:</span><span class="sxs-lookup"><span data-stu-id="8d71d-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="8d71d-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8d71d-107">An Azure SQL database.</span></span> <span data-ttu-id="8d71d-108">Den här snabbstarten använder resurser som har skapats i någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="8d71d-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="8d71d-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="8d71d-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="8d71d-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="8d71d-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="8d71d-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d71d-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="8d71d-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för den offentliga IP-adressen för datorn som du använder för den här snabbstartskursen.</span><span class="sxs-lookup"><span data-stu-id="8d71d-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="8d71d-113">Du har installerat Node.js och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="8d71d-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="8d71d-114">**MacOS**: Installera Homebrew och Node.js och installera därefter ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="8d71d-114">**MacOS**: Install Homebrew and Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="8d71d-115">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="8d71d-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="8d71d-116">**Ubuntu**: Installera Node.js och installera därefter ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="8d71d-116">**Ubuntu**: Install Node.js, and then install the ODBC driver and SQLCMD.</span></span> <span data-ttu-id="8d71d-117">Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span><span class="sxs-lookup"><span data-stu-id="8d71d-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="8d71d-118">**Windows**: Installera Chocolatey och Node.js och installera därefter ODBC-drivrutinen och SQL CMD.</span><span class="sxs-lookup"><span data-stu-id="8d71d-118">**Windows**: Install Chocolatey and Node.js, and then install the ODBC driver and SQL CMD.</span></span> <span data-ttu-id="8d71d-119">Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="8d71d-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="8d71d-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="8d71d-120">SQL server connection information</span></span>

<span data-ttu-id="8d71d-121">Skaffa den anslutningsinformation du behöver för att ansluta till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="8d71d-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="8d71d-122">Du behöver det fullständiga servernamnet, databasnamnet och inloggningsinformationen i nästa procedurer.</span><span class="sxs-lookup"><span data-stu-id="8d71d-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="8d71d-123">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8d71d-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="8d71d-124">Välj **SQL-databaser** på den vänstra menyn och klicka på databasen på sidan **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="8d71d-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="8d71d-125">Granska serverns fullständiga namn på sidan **Översikt** för databasen, se bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="8d71d-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="8d71d-126">Om du hovrar över servernamnet visas alternativet **Kopiera genom att klicka**.</span><span class="sxs-lookup"><span data-stu-id="8d71d-126">You can hover over the server name to bring up the **Click to copy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="8d71d-128">Om du har glömt inloggningsinformationen för Azure SQL Database-server öppnar du serversidan i SQL Database. Där ser du administratörsnamnet för servern och kan återställa lösenordet vid behov.</span><span class="sxs-lookup"><span data-stu-id="8d71d-128">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8d71d-129">Du måste ha en brandväggsregel för den offentliga IP-adressen för datorn som du utför den här självstudien med.</span><span class="sxs-lookup"><span data-stu-id="8d71d-129">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="8d71d-130">Om du använder en annan dator eller har en annan offentlig IP-adress så skapar du en [brandväggsregel på servernivå med hjälp av Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="8d71d-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="8d71d-131">Skapa ett Node.js-projekt</span><span class="sxs-lookup"><span data-stu-id="8d71d-131">Create a Node.js project</span></span>

<span data-ttu-id="8d71d-132">Öppna en kommandotolk och skapa en mapp med namnet *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="8d71d-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="8d71d-133">Navigera till den mapp som du har skapat och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8d71d-133">Navigate to the folder you created and run the following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="8d71d-134">Infoga kod för att fråga SQL-databas</span><span class="sxs-lookup"><span data-stu-id="8d71d-134">Insert code to query SQL database</span></span>

1. <span data-ttu-id="8d71d-135">Skapa en ny fil **sqltest.js** i din utvecklingsmiljö eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="8d71d-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="8d71d-136">Ersätt innehållet med följande kod och lägg till lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="8d71d-136">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-the-code"></a><span data-ttu-id="8d71d-137">Kör koden</span><span class="sxs-lookup"><span data-stu-id="8d71d-137">Run the code</span></span>

1. <span data-ttu-id="8d71d-138">Kör följande kommandon i kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="8d71d-138">At the command prompt, run the following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="8d71d-139">Kontrollera att de 20 översta raderna returneras och stäng sedan programfönstret.</span><span class="sxs-lookup"><span data-stu-id="8d71d-139">Verify that the top 20 rows are returned and then close the application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d71d-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d71d-140">Next steps</span></span>

- <span data-ttu-id="8d71d-141">Läs om [Microsoft Node.js-drivrutinen för SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="8d71d-141">Learn about the [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="8d71d-142">Lär dig hur du [ansluter och frågar en Azure SQL-databas med .NET Core](sql-database-connect-query-dotnet-core.md) på Windows/Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="8d71d-142">Learn how to [connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="8d71d-143">Lär dig mer om att [Komma igång med .NET Core för Windows/Linux/macOS med hjälp av kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="8d71d-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using the command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="8d71d-144">Lär dig hur du [Utformar din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [Utformar din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="8d71d-144">Learn how to [Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="8d71d-145">Läs hur du [ansluter och frågar med SSMS](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="8d71d-145">Learn how to [Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="8d71d-146">Läs hur du [ansluter och frågar med Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="8d71d-146">Learn how to [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


