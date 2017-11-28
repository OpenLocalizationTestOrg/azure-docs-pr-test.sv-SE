---
title: "Fråga Azure SQL Database med Ruby | Microsoft Docs"
description: "Det här avsnittet visar hur du använder Ruby för att skapa ett program som ansluter till en Azure SQL Database och frågar den med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: carlrab
ms.openlocfilehash: 25ff9a9cfaa5494dbb006c84e235099fe51e6545
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-ruby-to-query-an-azure-sql-database"></a><span data-ttu-id="66a04-103">Använd Ruby för att fråga en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="66a04-103">Use Ruby to query an Azure SQL database</span></span>

<span data-ttu-id="66a04-104">Den här snabbstartsguiden visar hur man använder [Ruby](https://www.ruby-lang.org) för att skapa ett program för att ansluta till en Azure SQL-databas och använda Transact-SQL-uttryck för att fråga data.</span><span class="sxs-lookup"><span data-stu-id="66a04-104">This quick start tutorial demonstrates how to use [Ruby](https://www.ruby-lang.org) to create a program to connect to an Azure SQL database and use Transact-SQL statements to query data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66a04-105">Krav</span><span class="sxs-lookup"><span data-stu-id="66a04-105">Prerequisites</span></span>

<span data-ttu-id="66a04-106">För att kunna slutföra den här snabbstartskursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="66a04-106">To complete this quick start tutorial, make sure you have the following prerequisites:</span></span>

- <span data-ttu-id="66a04-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="66a04-107">An Azure SQL database.</span></span> <span data-ttu-id="66a04-108">Den här snabbstarten använder resurser som har skapats i någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="66a04-108">This quick start uses the resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="66a04-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="66a04-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="66a04-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="66a04-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="66a04-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="66a04-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="66a04-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för den offentliga IP-adressen för den dator du använder för den här snabbstartskursen.</span><span class="sxs-lookup"><span data-stu-id="66a04-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for the public IP address of the computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="66a04-113">Du har installerat Ruby och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="66a04-113">You have installed Ruby and related software for your operating system.</span></span>
    - <span data-ttu-id="66a04-114">**MacOS**: Installera Homebrew, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="66a04-114">**MacOS**: Install Homebrew, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="66a04-115">Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span><span class="sxs-lookup"><span data-stu-id="66a04-115">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span></span>
    - <span data-ttu-id="66a04-116">**Ubuntu**: Installera förhandskraven för Ruby, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="66a04-116">**Ubuntu**: Install prerequisites for Ruby, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="66a04-117">Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="66a04-117">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="66a04-118">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="66a04-118">SQL server connection information</span></span>

<span data-ttu-id="66a04-119">Skaffa den anslutningsinformation du behöver för att ansluta till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="66a04-119">Get the connection information needed to connect to the Azure SQL database.</span></span> <span data-ttu-id="66a04-120">Du behöver det fullständiga servernamnet, databasnamnet och inloggningsinformationen i nästa procedurer.</span><span class="sxs-lookup"><span data-stu-id="66a04-120">You will need the fully qualified server name, database name, and login information in the next procedures.</span></span>

1. <span data-ttu-id="66a04-121">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="66a04-121">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="66a04-122">Välj **SQL-databaser** på den vänstra menyn och klicka på databasen på sidan **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="66a04-122">Select **SQL Databases** from the left-hand menu, and click your database on the **SQL databases** page.</span></span> 
3. <span data-ttu-id="66a04-123">Kontrollera det fullständigt kvalificerade servernamnet på sidan **Översikt** för databasen.</span><span class="sxs-lookup"><span data-stu-id="66a04-123">On the **Overview** page for your database, review the fully qualified server name.</span></span> <span data-ttu-id="66a04-124">Om du hovrar över servernamnet visas alternativet **Kopiera genom att klicka**, vilket visas på följande bild:</span><span class="sxs-lookup"><span data-stu-id="66a04-124">You can hover over the server name to bring up the **Click to copy** option, as shown in the following image:</span></span>

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="66a04-126">Om du har glömt inloggningsinformationen för Azure SQL Database-server öppnar du serversidan i SQL Database. Där ser du administratörsnamnet för servern och kan återställa lösenordet vid behov.</span><span class="sxs-lookup"><span data-stu-id="66a04-126">If you have forgotten the login information for your Azure SQL Database server, navigate to the SQL Database server page to view the server admin name and, if necessary, reset the password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="66a04-127">Du måste ha en brandväggsregel för den offentliga IP-adressen för datorn som du utför den här självstudien med.</span><span class="sxs-lookup"><span data-stu-id="66a04-127">You must have a firewall rule in place for the public IP address of the computer on which you perform this tutorial.</span></span> <span data-ttu-id="66a04-128">Om du använder en annan dator eller har en annan offentlig IP-adress så skapar du en [brandväggsregel på servernivå med hjälp av Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="66a04-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using the Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="insert-code-to-query-sql-database"></a><span data-ttu-id="66a04-129">Infoga kod för att fråga SQL Database</span><span class="sxs-lookup"><span data-stu-id="66a04-129">Insert code to query SQL database</span></span>

1. <span data-ttu-id="66a04-130">Skapa en ny fil, **sqltest.py**, i valfri textredigerare</span><span class="sxs-lookup"><span data-stu-id="66a04-130">In your favorite text editor, create a new file, **sqltest.rb**</span></span>

2. <span data-ttu-id="66a04-131">Ersätt innehållet med följande kod och lägg till lämpliga värden för server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="66a04-131">Replace the contents with the following code and add the appropriate values for your server, database, user, and password.</span></span>

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-the-code"></a><span data-ttu-id="66a04-132">Kör koden</span><span class="sxs-lookup"><span data-stu-id="66a04-132">Run the code</span></span>

1. <span data-ttu-id="66a04-133">Kör följande kommandon i kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="66a04-133">At the command prompt, run the following commands:</span></span>

   ```bash
   ruby sqltest.rb
   ```

2. <span data-ttu-id="66a04-134">Kontrollera att de 20 översta raderna returneras och stäng sedan programfönstret.</span><span class="sxs-lookup"><span data-stu-id="66a04-134">Verify that the top 20 rows are returned and then close the application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="66a04-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66a04-135">Next Steps</span></span>
- [<span data-ttu-id="66a04-136">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="66a04-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="66a04-137">GitHub-lagringsplatsen för TinyTDS</span><span class="sxs-lookup"><span data-stu-id="66a04-137">GitHub repository for TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds)
- [<span data-ttu-id="66a04-138">Rapportera problem eller ställ frågor om TinyTDS</span><span class="sxs-lookup"><span data-stu-id="66a04-138">Report issues or ask questions about TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds/issues)
- [<span data-ttu-id="66a04-139">Ruby-drivrutiner för SQLServer</span><span class="sxs-lookup"><span data-stu-id="66a04-139">Ruby Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
