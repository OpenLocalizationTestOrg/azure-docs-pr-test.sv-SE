---
title: aaaUse Ruby tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Ruby toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
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
ms.openlocfilehash: 0d4b16b8aacb5e376ab80cbe37569130f2fd52b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ruby-tooquery-an-azure-sql-database"></a><span data-ttu-id="df2a8-103">Använd Ruby tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="df2a8-103">Use Ruby tooquery an Azure SQL database</span></span>

<span data-ttu-id="df2a8-104">Den här snabbstartsguide visar hur toouse [Ruby](https://www.ruby-lang.org) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="df2a8-104">This quick start tutorial demonstrates how toouse [Ruby](https://www.ruby-lang.org) toocreate a program tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df2a8-105">Krav</span><span class="sxs-lookup"><span data-stu-id="df2a8-105">Prerequisites</span></span>

<span data-ttu-id="df2a8-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="df2a8-106">toocomplete this quick start tutorial, make sure you have hello following prerequisites:</span></span>

- <span data-ttu-id="df2a8-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="df2a8-107">An Azure SQL database.</span></span> <span data-ttu-id="df2a8-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="df2a8-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="df2a8-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="df2a8-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="df2a8-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="df2a8-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="df2a8-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="df2a8-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="df2a8-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="df2a8-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>
- <span data-ttu-id="df2a8-113">Du har installerat Ruby och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="df2a8-113">You have installed Ruby and related software for your operating system.</span></span>
    - <span data-ttu-id="df2a8-114">**MacOS**: Installera Homebrew, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="df2a8-114">**MacOS**: Install Homebrew, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="df2a8-115">Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span><span class="sxs-lookup"><span data-stu-id="df2a8-115">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).</span></span>
    - <span data-ttu-id="df2a8-116">**Ubuntu**: Installera förhandskraven för Ruby, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS.</span><span class="sxs-lookup"><span data-stu-id="df2a8-116">**Ubuntu**: Install prerequisites for Ruby, install rbenv and ruby-build, install Ruby, and then install FreeTDS.</span></span> <span data-ttu-id="df2a8-117">Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="df2a8-117">See [Step 1.2, 1.3, 1.4, and 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).</span></span>

## <a name="sql-server-connection-information"></a><span data-ttu-id="df2a8-118">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="df2a8-118">SQL server connection information</span></span>

<span data-ttu-id="df2a8-119">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="df2a8-119">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="df2a8-120">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="df2a8-120">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="df2a8-121">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="df2a8-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="df2a8-122">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="df2a8-122">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="df2a8-123">På hello **översikt** för din databas kan du granska hello fullständigt kvalificerade servernamnet.</span><span class="sxs-lookup"><span data-stu-id="df2a8-123">On hello **Overview** page for your database, review hello fully qualified server name.</span></span> <span data-ttu-id="df2a8-124">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativ, enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="df2a8-124">You can hover over hello server name toobring up hello **Click toocopy** option, as shown in hello following image:</span></span>

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="df2a8-126">Om du har glömt hello inloggningsinformation för din Azure SQL Database-server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="df2a8-126">If you have forgotten hello login information for your Azure SQL Database server, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df2a8-127">Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="df2a8-127">You must have a firewall rule in place for hello public IP address of hello computer on which you perform this tutorial.</span></span> <span data-ttu-id="df2a8-128">Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="df2a8-128">If you are on a different computer or have a different public IP address, create a [server-level firewall rule using hello Azure portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span> 

## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="df2a8-129">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="df2a8-129">Insert code tooquery SQL database</span></span>

1. <span data-ttu-id="df2a8-130">Skapa en ny fil, **sqltest.py**, i valfri textredigerare</span><span class="sxs-lookup"><span data-stu-id="df2a8-130">In your favorite text editor, create a new file, **sqltest.rb**</span></span>

2. <span data-ttu-id="df2a8-131">Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="df2a8-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

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

## <a name="run-hello-code"></a><span data-ttu-id="df2a8-132">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="df2a8-132">Run hello code</span></span>

1. <span data-ttu-id="df2a8-133">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="df2a8-133">At hello command prompt, run hello following commands:</span></span>

   ```bash
   ruby sqltest.rb
   ```

2. <span data-ttu-id="df2a8-134">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="df2a8-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>


## <a name="next-steps"></a><span data-ttu-id="df2a8-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df2a8-135">Next Steps</span></span>
- [<span data-ttu-id="df2a8-136">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="df2a8-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="df2a8-137">GitHub-lagringsplatsen för TinyTDS</span><span class="sxs-lookup"><span data-stu-id="df2a8-137">GitHub repository for TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds)
- [<span data-ttu-id="df2a8-138">Rapportera problem eller ställ frågor om TinyTDS</span><span class="sxs-lookup"><span data-stu-id="df2a8-138">Report issues or ask questions about TinyTDS</span></span>](https://github.com/rails-sqlserver/tiny_tds/issues)
- [<span data-ttu-id="df2a8-139">Ruby-drivrutiner för SQLServer</span><span class="sxs-lookup"><span data-stu-id="df2a8-139">Ruby Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
