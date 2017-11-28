---
title: "Fråga Azure SQL Database med PHP | Microsoft Docs"
description: "Det här avsnittet visar hur du använder PHP för att skapa ett program som ansluter till en Azure SQL Database och frågar den med hjälp av Transact-SQL-uttryck."
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
ms.openlocfilehash: 3a43472ad2be4a0fd6f7126f72433acd8b5f25fd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-php-to-query-an-azure-sql-database"></a><span data-ttu-id="d7a1e-103">Använd PHP för att fråga en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="d7a1e-103">Use PHP to query an Azure SQL database</span></span>

<span data-ttu-id="d7a1e-104">Den här snabbstartsguiden visar hur man använder [PHP](http://php.net/manual/en/intro-whatis.php) för att skapa ett program för att ansluta till en Azure SQL-databas och använda Transact-SQL-uttryck för att fråga data.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-104">This quick start tutorial demonstrates how to use [PHP](http://php.net/manual/en/intro-whatis.php) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7a1e-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d7a1e-105">Prerequisites</span></span>

<span data-ttu-id="d7a1e-106">Kontrollera att du har följande för att slutföra den här snabbstartskursen:</span><span class="sxs-lookup"><span data-stu-id="d7a1e-106">To complete this quick start tutorial, make sure you have the following:</span></span>

- <span data-ttu-id="d7a1e-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-107">An Azure SQL database.</span></span> <span data-ttu-id="d7a1e-108">Den här snabbstarten använder resurser som har skapats i någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="d7a1e-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="d7a1e-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="d7a1e-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="d7a1e-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="d7a1e-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="d7a1e-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="d7a1e-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="d7a1e-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för den offentliga IP-adressen för datorn som du använder för den här snabbstartskursen.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="d7a1e-113">Du har installerat PHP och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-113">You have installed PHP and related software for your operating system.</span></span>

    - <span data-ttu-id="d7a1e-114">**MacOS**: Först installerar du Homebrew och PHP, sedan ODBC-drivrutinen och SQLCMD och sedan installerar du PHP-drivrutinen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-114">**MacOS**: Install Homebrew and PHP, install the ODBC driver and SQLCMD, and then install the PHP Driver for SQL Server.</span></span> <span data-ttu-id="d7a1e-115">Se [steg 1.2, 1.3 och 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span><span class="sxs-lookup"><span data-stu-id="d7a1e-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).</span></span>
    - <span data-ttu-id="d7a1e-116">**Ubuntu**: Först installerar du PHP och andra paket som krävs, sedan installerar du PHP-drivrutinen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-116">**Ubuntu**:  Install PHP and other required packages, and then install the PHP Driver for SQL Server.</span></span> <span data-ttu-id="d7a1e-117">Se [steg 1.2 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="d7a1e-117">See [Steps 1.2 and 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).</span></span>
    - <span data-ttu-id="d7a1e-118">**Windows**: installera den senaste versionen av PHP för IIS Express, den senaste versionen av Microsoft-drivrutiner för SQL Server i IIS Express, Chocolatey, ODBC-drivrutinen och SQLCMD.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-118">**Windows**: Install the newest version of PHP for IIS Express, the newest version of Microsoft Drivers for SQL Server in IIS Express, Chocolatey, the ODBC driver, and SQLCMD.</span></span> <span data-ttu-id="d7a1e-119">Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span><span class="sxs-lookup"><span data-stu-id="d7a1e-119">See [Steps 1.2 and 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).</span></span>    

## <a name="sql-server-connection-information"></a><span data-ttu-id="d7a1e-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="d7a1e-120">SQL server connection information</span></span>

<span data-ttu-id="d7a1e-121">Skaffa den anslutningsinformation du behöver för att ansluta till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-121">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="d7a1e-122">Du behöver det fullständiga servernamnet, databasnamnet och inloggningsinformationen i nästa procedurer.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-122">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="d7a1e-123">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d7a1e-123">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d7a1e-124">Välj **SQL-databaser** på den vänstra menyn och klicka på databasen på sidan **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-124">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="d7a1e-125">Granska serverns fullständiga namn på sidan **Översikt** för databasen, se bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-125">On the **Overview** page for your database, review the fully qualified server name as shown in the following image.</span></span> <span data-ttu-id="d7a1e-126">Om du hovrar över servernamnet visas alternativet **Kopiera genom att klicka**.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-126">You can hover over the server name to bring up the **Click to copy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="d7a1e-128">Om du glömmer inloggningsinformationen för din server öppnar du serversidan i SQL Database. Där ser du administratörsnamnet för servern och kan återställa lösenordet vid behov.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-128">If you forget your server login information, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>     
    
## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="d7a1e-129">Infoga kod för att fråga SQL-databas</span><span class="sxs-lookup"><span data-stu-id="d7a1e-129">Insert code to query SQL database</span></span>

1. <span data-ttu-id="d7a1e-130">Skapa en ny fil, **sqltest.php**, i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-130">In your favorite text editor, create a new file, **sqltest.php**.</span></span>  

2. <span data-ttu-id="d7a1e-131">Ersätt innehållet med följande kod och lägg till lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-131">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
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

## <a name="run-the-code"></a><span data-ttu-id="d7a1e-132">Kör koden</span><span class="sxs-lookup"><span data-stu-id="d7a1e-132">Run the code</span></span>

1. <span data-ttu-id="d7a1e-133">Kör följande kommandon i kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="d7a1e-133">At the command prompt, run the following commands:</span></span>

   ```php
   php sqltest.php
   ```

2. <span data-ttu-id="d7a1e-134">Kontrollera att de 20 översta raderna returneras och stäng sedan programfönstret.</span><span class="sxs-lookup"><span data-stu-id="d7a1e-134">Verify that the top 20 rows are returned and then close the application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7a1e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7a1e-135">Next steps</span></span>
- [<span data-ttu-id="d7a1e-136">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d7a1e-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="d7a1e-137">Microsoft PHP-drivrutiner för SQL Server</span><span class="sxs-lookup"><span data-stu-id="d7a1e-137">Microsoft PHP Drivers for SQL Server</span></span>](https://github.com/Microsoft/msphpsql/)
- [<span data-ttu-id="d7a1e-138">Rapportera problem eller ställ frågor</span><span class="sxs-lookup"><span data-stu-id="d7a1e-138">Report issues or ask questions</span></span>](https://github.com/Microsoft/msphpsql/issues)
