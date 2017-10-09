---
title: aaaUse PHP tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse PHP toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 5fc49dcc42ab07cc1bec554be39bdf08dbd6f75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-php-tooquery-an-azure-sql-database"></a><span data-ttu-id="c747d-103">Använd PHP tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="c747d-103">Use PHP tooquery an Azure SQL database</span></span>

<span data-ttu-id="c747d-104">Den här snabbstartsguide visar hur toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="c747d-104">This quick start tutorial demonstrates how toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c747d-105">Krav</span><span class="sxs-lookup"><span data-stu-id="c747d-105">Prerequisites</span></span>

<span data-ttu-id="c747d-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="c747d-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="c747d-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c747d-107">An Azure SQL database.</span></span> <span data-ttu-id="c747d-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="c747d-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="c747d-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="c747d-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="c747d-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="c747d-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="c747d-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="c747d-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="c747d-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="c747d-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="c747d-113">Du har installerat PHP och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c747d-113">You have installed PHP and related software for your operating system.</span></span>

    - <span data-ttu-id="c747d-114">**MacOS**: Installera Homebrew och PHP, installera hello ODBC-drivrutinen och SQLCMD och installera sedan hello PHP-drivrutin för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c747d-114">**MacOS**: Install Homebrew and PHP, install hello ODBC driver and SQLCMD, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="c747d-115">Se [steg 1.2, 1.3 och 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span><span class="sxs-lookup"><span data-stu-id="c747d-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span></span>
    - <span data-ttu-id="c747d-116">**Ubuntu**: Installera PHP och andra nödvändiga paket och installera hello PHP-drivrutin för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c747d-116">**Ubuntu**:  Install PHP and other required packages, and then install hello PHP Driver for SQL Server.</span></span> <span data-ttu-id="c747d-117">Se [steg 1.2 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="c747d-117">See [Steps 1.2 and 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span></span>
    - <span data-ttu-id="c747d-118">**Windows**: Installera hello senaste versionen av PHP för IIS Express, hello senaste versionen av Drivers Microsoft för SQL Server i IIS Express, Chocolatey, hello ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="c747d-118">**Windows**: Install hello newest version of PHP for IIS Express, hello newest version of Microsoft Drivers for SQL Server in IIS Express, Chocolatey, hello ODBC driver, and SQLCMD.</span></span> <span data-ttu-id="c747d-119">Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span><span class="sxs-lookup"><span data-stu-id="c747d-119">See [Steps 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="c747d-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="c747d-120">SQL server connection information</span></span>

<span data-ttu-id="c747d-121">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c747d-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="c747d-122">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="c747d-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="c747d-123">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c747d-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c747d-124">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="c747d-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="c747d-125">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="c747d-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="c747d-126">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="c747d-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="c747d-128">Om du glömmer inloggningsinformationen server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="c747d-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="c747d-129">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c747d-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="c747d-130">Skapa en ny fil, **sqltest.php**, i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="c747d-130">In your favorite text editor, create a new file, **sqltest.php**.</span></span>  

2. <span data-ttu-id="c747d-131">Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c747d-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes hello connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-hello-code"></a><span data-ttu-id="c747d-132">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="c747d-132">Run hello code</span></span>

1. <span data-ttu-id="c747d-133">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="c747d-133">At hello command prompt, run hello following commands:</span></span>

   ```php
   php sqltest.php
   ```

2. <span data-ttu-id="c747d-134">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="c747d-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c747d-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c747d-135">Next steps</span></span>
- [<span data-ttu-id="c747d-136">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c747d-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="c747d-137">Microsoft PHP-drivrutiner för SQL Server</span><span class="sxs-lookup"><span data-stu-id="c747d-137">Microsoft PHP Drivers for SQL Server</span></span>](https://github.com/Microsoft/msphpsql/)
- [<span data-ttu-id="c747d-138">Rapportera problem eller ställ frågor</span><span class="sxs-lookup"><span data-stu-id="c747d-138">Report issues or ask questions</span></span>](https://github.com/Microsoft/msphpsql/issues)
