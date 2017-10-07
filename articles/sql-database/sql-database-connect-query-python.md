---
title: aaaUse Python tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Python toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 6c786e7a61f5ed3e7d2395da59acfeae008e90e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-tooquery-an-azure-sql-database"></a><span data-ttu-id="07b8b-103">Använda Python tooquery en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="07b8b-103">Use Python tooquery an Azure SQL database</span></span>

 <span data-ttu-id="07b8b-104">Den här snabbstartsguide visar hur toouse [Python](https://python.org) tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.</span><span class="sxs-lookup"><span data-stu-id="07b8b-104">This quick start demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure SQL database and use Transact-SQL statements tooquery data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07b8b-105">Krav</span><span class="sxs-lookup"><span data-stu-id="07b8b-105">Prerequisites</span></span>

<span data-ttu-id="07b8b-106">toocomplete detta snabb start kursen, kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="07b8b-106">toocomplete this quick start tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="07b8b-107">En Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="07b8b-107">An Azure SQL database.</span></span> <span data-ttu-id="07b8b-108">Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="07b8b-108">This quick start uses hello resources created in one of these quick starts:</span></span> 

   - [<span data-ttu-id="07b8b-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="07b8b-109">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="07b8b-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="07b8b-110">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="07b8b-111">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="07b8b-111">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="07b8b-112">En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="07b8b-112">A [server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) for hello public IP address of hello computer you use for this quick start tutorial.</span></span>

- <span data-ttu-id="07b8b-113">Du har installerat Python och relaterad programvara för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="07b8b-113">You have installed Python and related software for your operating system.</span></span>

    - <span data-ttu-id="07b8b-114">**MacOS**: Installera Homebrew och Python, installera hello ODBC-drivrutinen och SQLCMD och installera sedan hello Python-drivrutin för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07b8b-114">**MacOS**: Install Homebrew and Python, install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="07b8b-115">Se [steg 1.2, 1.3 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span><span class="sxs-lookup"><span data-stu-id="07b8b-115">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).</span></span>
    - <span data-ttu-id="07b8b-116">**Ubuntu**: Installera Python och andra krävs paket och installera hello Python-drivrutin för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07b8b-116">**Ubuntu**:  Install Python and other required packages, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="07b8b-117">Se [steg 1.2, 1.3 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="07b8b-117">See [Steps 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).</span></span>
    - <span data-ttu-id="07b8b-118">**Windows**: Installera hello senaste versionen av Python (miljövariabeln har nu konfigurerats för du), installera hello ODBC-drivrutinen och SQLCMD och installera sedan hello Python-drivrutin för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07b8b-118">**Windows**: Install hello newest version of Python (environment variable is now configured for you), install hello ODBC driver and SQLCMD, and then install hello Python Driver for SQL Server.</span></span> <span data-ttu-id="07b8b-119">Se [Steg 1.2, 1.3 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span><span class="sxs-lookup"><span data-stu-id="07b8b-119">See [Step 1.2, 1.3, and 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/).</span></span> 

## <a name="sql-server-connection-information"></a><span data-ttu-id="07b8b-120">Anslutningsinformation för en SQL-server</span><span class="sxs-lookup"><span data-stu-id="07b8b-120">SQL server connection information</span></span>

<span data-ttu-id="07b8b-121">Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="07b8b-121">Get hello connection information needed tooconnect toohello Azure SQL database.</span></span> <span data-ttu-id="07b8b-122">Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.</span><span class="sxs-lookup"><span data-stu-id="07b8b-122">You will need hello fully qualified server name, database name, and login information in hello next procedures.</span></span>

1. <span data-ttu-id="07b8b-123">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="07b8b-123">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="07b8b-124">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="07b8b-124">Select **SQL Databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 
3. <span data-ttu-id="07b8b-125">På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="07b8b-125">On hello **Overview** page for your database, review hello fully qualified server name as shown in hello following image.</span></span> <span data-ttu-id="07b8b-126">Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.</span><span class="sxs-lookup"><span data-stu-id="07b8b-126">You can hover over hello server name toobring up hello **Click toocopy** option.</span></span>  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. <span data-ttu-id="07b8b-128">Om du glömmer inloggningsinformationen server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="07b8b-128">If you forget your server login information, navigate toohello SQL Database server page tooview hello server admin name and, if necessary, reset hello password.</span></span>     
    
## <a name="insert-code-tooquery-sql-database"></a><span data-ttu-id="07b8b-129">Infoga kod tooquery SQL-databas</span><span class="sxs-lookup"><span data-stu-id="07b8b-129">Insert code tooquery SQL database</span></span> 

1. <span data-ttu-id="07b8b-130">Skapa en ny fil, **sqltest.py**, i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="07b8b-130">In your favorite text editor, create a new file, **sqltest.py**.</span></span>  

2. <span data-ttu-id="07b8b-131">Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.</span><span class="sxs-lookup"><span data-stu-id="07b8b-131">Replace hello contents with hello following code and add hello appropriate values for your server, database, user, and password.</span></span>

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-hello-code"></a><span data-ttu-id="07b8b-132">Köra hello kod</span><span class="sxs-lookup"><span data-stu-id="07b8b-132">Run hello code</span></span>

1. <span data-ttu-id="07b8b-133">Kör följande kommandon hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="07b8b-133">At hello command prompt, run hello following commands:</span></span>

   ```Python
   python sqltest.py
   ```

2. <span data-ttu-id="07b8b-134">Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="07b8b-134">Verify that hello top 20 rows are returned and then close hello application window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07b8b-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07b8b-135">Next steps</span></span>

- [<span data-ttu-id="07b8b-136">Utforma din första Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="07b8b-136">Design your first Azure SQL database</span></span>](sql-database-design-first-database.md)
- [<span data-ttu-id="07b8b-137">Microsoft Python-drivrutiner för SQLServer</span><span class="sxs-lookup"><span data-stu-id="07b8b-137">Microsoft Python Drivers for SQL Server</span></span>](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [<span data-ttu-id="07b8b-138">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="07b8b-138">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/?v=17.23h)

