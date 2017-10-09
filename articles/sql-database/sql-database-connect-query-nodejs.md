---
title: aaaUse Node.js tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Node.js toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
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
ms.openlocfilehash: 3870130a486c218eafeb9cf792a4275de7fd6551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-nodejs-tooquery-an-azure-sql-database"></a><span data-ttu-id="84fe5-103">Använda Node.js tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="84fe5-103">Use Node.js tooquery an Azure SQL database</span></span>

<span data-ttu-id="84fe5-104">Den här snabbstartsguide visar hur toouse [Node.js](https://nodejs.org/en/) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="84fe5-104">This quick start tutorial demonstrates how toouse [Node.js](https://nodejs.org/en/) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84fe5-105">Krav</span><span class="sxs-lookup"><span data-stu-id="84fe5-105">Prerequisites</span></span>

<span data-ttu-id="84fe5-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="84fe5-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="84fe5-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="84fe5-107">An Azure SQL database.</span></span> <span data-ttu-id="84fe5-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="84fe5-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="84fe5-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="84fe5-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="84fe5-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="84fe5-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="84fe5-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="84fe5-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="84fe5-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="84fe5-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="84fe5-113">Du har installerat Node.js och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="84fe5-113">You have installed Node.js and related software for your operating system.</span></span>
    - <span data-ttu-id="84fe5-114">**MacOS**: Installera Homebrew och Node.js och installera sedan hello ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="84fe5-114">**MacOS**: Install Homebrew and Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="84fe5-115">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span><span class="sxs-lookup"><span data-stu-id="84fe5-115">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).</span></span>
    - <span data-ttu-id="84fe5-116">**Ubuntu**: Installera Node.js och installera sedan hello ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="84fe5-116">**Ubuntu**: Install Node.js, and then install hello ODBC driver and SQLCMD.</span></span> <span data-ttu-id="84fe5-117">Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span><span class="sxs-lookup"><span data-stu-id="84fe5-117">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .</span></span>
    - <span data-ttu-id="84fe5-118">**Windows**: Installera Chocolatey och Node.js och installera sedan hello ODBC-drivrutinen och SQL CMD.</span><span class="sxs-lookup"><span data-stu-id="84fe5-118">**Windows**: Install Chocolatey and Node.js, and then install hello ODBC driver and SQL CMD.</span></span> <span data-ttu-id="84fe5-119">Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span><span class="sxs-lookup"><span data-stu-id="84fe5-119">See [Step 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="84fe5-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="84fe5-120">SQL server connection information</span></span>

<span data-ttu-id="84fe5-121">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="84fe5-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="84fe5-122">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="84fe5-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="84fe5-123">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="84fe5-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="84fe5-124">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="84fe5-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="84fe5-125">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="84fe5-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="84fe5-126">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="84fe5-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span> 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="84fe5-128">Om du har glömt hello inloggningsinformation för din Azure SQL Database-server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="84fe5-128">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84fe5-129">Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="84fe5-129">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="84fe5-130">Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="84fe5-130">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="create-a-nodejs-project"></a><span data-ttu-id="84fe5-131">Skapa ett Node.js-projekt</span><span class="sxs-lookup"><span data-stu-id="84fe5-131">Create a Node.js project</span></span>

<span data-ttu-id="84fe5-132">Öppna en kommandotolk och skapa en mapp med namnet *sqltest*.</span><span class="sxs-lookup"><span data-stu-id="84fe5-132">Open a command prompt and create a folder named *sqltest*.</span></span> <span data-ttu-id="84fe5-133">Navigera toohello mappen du skapade och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="84fe5-133">Navigate toohello folder you created and run hello following command:</span></span>

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="84fe5-134">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="84fe5-134">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="84fe5-135">Skapa en ny fil **sqltest.js** i din utvecklingsmiljö eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="84fe5-135">In your development environment or favorite text editor, create a new file, **sqltest.js**.</span></span>

2. <span data-ttu-id="84fe5-136">Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="84fe5-136">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection toodatabase
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

   // Attempt tooconnect and execute queries if connection goes through
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
      { console.log('Reading rows from hello Table...');

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

## <a name="run-hello-code"></a><span data-ttu-id="84fe5-137">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="84fe5-137">Run hello code</span></span>

1. <span data-ttu-id="84fe5-138">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="84fe5-138">At hello command prompt, run hello following commands:</span></span>

   ```js
   node sqltest.js
   ```

2. <span data-ttu-id="84fe5-139">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="84fe5-139">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84fe5-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84fe5-140">Next steps</span></span>

- <span data-ttu-id="84fe5-141">Lär dig mer om hello [Microsoft Node.js Driver för SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span><span class="sxs-lookup"><span data-stu-id="84fe5-141">Learn about hello [Microsoft Node.js Driver for SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)</span></span>
- <span data-ttu-id="84fe5-142">Lär dig hur för[Anslut och fråga en Azure SQL database med .NET core](sql-database-connect-query-dotnet-core.md) på macOS/Windows/Linux.</span><span class="sxs-lookup"><span data-stu-id="84fe5-142">Learn how too[connect and query an Azure SQL database using .NET core](sql-database-connect-query-dotnet-core.md) on Windows/Linux/macOS.</span></span>  
- <span data-ttu-id="84fe5-143">Lär dig mer om [komma igång med .NET Core för Windows/Linux/macOS hello kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="84fe5-143">Learn about [Getting started with .NET Core on Windows/Linux/macOS using hello command line](/dotnet/core/tutorials/using-with-xplat-cli).</span></span>
- <span data-ttu-id="84fe5-144">Lär dig hur för[utforma din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [utforma din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="84fe5-144">Learn how too[Design your first Azure SQL database using SSMS](sql-database-design-first-database.md) or [Design your first Azure SQL database using .NET](sql-database-design-first-database-csharp.md).</span></span>
- <span data-ttu-id="84fe5-145">Lär dig hur för[Anslut och fråga med SSMS](sql-database-connect-query-ssms.md)</span><span class="sxs-lookup"><span data-stu-id="84fe5-145">Learn how too[Connect and query with SSMS](sql-database-connect-query-ssms.md)</span></span>
- <span data-ttu-id="84fe5-146">Lär dig hur för[Anslut och fråga med Visual Studio Code](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="84fe5-146">Learn how too[Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md).</span></span>


