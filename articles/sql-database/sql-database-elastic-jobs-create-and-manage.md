---
title: Hantera grupper med Azure SQL-databaser | Microsoft Docs
description: "Gå igenom skapande och hantering av ett elastiska jobb."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d30cc74778e0b36dd632c2f040ce80d1ca8af5a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a><span data-ttu-id="7a44b-103">Skapa och hantera skaländras ut Azure SQL-databaser med elastiska jobb (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="7a44b-103">Create and manage scaled out Azure SQL Databases using elastic jobs (preview)</span></span>


<span data-ttu-id="7a44b-104">**Den elastiska databasen jobb** förenkla hanteringen av grupper av databaser genom att utföra administrativa åtgärder, till exempel schemaändringar, hantering av autentiseringsuppgifter, referens uppdateringar, insamling av prestandadata eller klient (kunden) telemetri samling.</span><span class="sxs-lookup"><span data-stu-id="7a44b-104">**Elastic Database jobs** simplify management of groups of databases by executing administrative operations such as schema changes, credentials management, reference data updates, performance data collection or tenant (customer) telemetry collection.</span></span> <span data-ttu-id="7a44b-105">Elastiska jobb i databasen är tillgänglig via Azure portal och PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7a44b-105">Elastic Database jobs is currently available through the Azure portal and PowerShell cmdlets.</span></span> <span data-ttu-id="7a44b-106">Dock Azure portal hämtar nedsatt funktionalitet begränsad till körning över alla databaser i en [elastisk pool (förhandsgranskning)](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="7a44b-106">However, the Azure portal surfaces reduced functionality limited to execution across all databases in an [elastic pool (preview)](sql-database-elastic-pool.md).</span></span> <span data-ttu-id="7a44b-107">Få tillgång till ytterligare funktioner och körning av skript via en grupp databaser inklusive en egendefinierat samling eller en Fragmentera ange (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-scale-introduction.md)), se [skapa och hantera jobb med hjälp av PowerShell](sql-database-elastic-jobs-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7a44b-107">To access additional features and execution of scripts across a group of databases including a custom-defined collection or a shard set (created using [Elastic Database client library](sql-database-elastic-scale-introduction.md)), see [Creating and managing jobs using PowerShell](sql-database-elastic-jobs-powershell.md).</span></span> <span data-ttu-id="7a44b-108">Mer information om jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7a44b-108">For more information about jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7a44b-109">Krav</span><span class="sxs-lookup"><span data-stu-id="7a44b-109">Prerequisites</span></span>
* <span data-ttu-id="7a44b-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a44b-110">An Azure subscription.</span></span> <span data-ttu-id="7a44b-111">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a44b-111">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7a44b-112">En elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="7a44b-112">An elastic pool.</span></span> <span data-ttu-id="7a44b-113">Se [om elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="7a44b-113">See [About elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="7a44b-114">Installation av tjänstkomponenter för elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="7a44b-114">Installation of elastic database job service components.</span></span> <span data-ttu-id="7a44b-115">Se [installera tjänsten elastisk databas jobbet](sql-database-elastic-jobs-service-installation.md).</span><span class="sxs-lookup"><span data-stu-id="7a44b-115">See [Installing the elastic database job service](sql-database-elastic-jobs-service-installation.md).</span></span>

## <a name="creating-jobs"></a><span data-ttu-id="7a44b-116">Jobb</span><span class="sxs-lookup"><span data-stu-id="7a44b-116">Creating jobs</span></span>
1. <span data-ttu-id="7a44b-117">Med hjälp av den [Azure-portalen](https://portal.azure.com), från en befintlig elastisk jobbet databaspool, klicka på **skapa jobbet**.</span><span class="sxs-lookup"><span data-stu-id="7a44b-117">Using the [Azure portal](https://portal.azure.com), from an existing elastic database job pool, click **Create job**.</span></span>
2. <span data-ttu-id="7a44b-118">Ange användarnamnet och lösenordet för databasadministratören (skapas vid installationen av jobb) för jobb kontrollen databasen (metadata lagring för jobb).</span><span class="sxs-lookup"><span data-stu-id="7a44b-118">Type in the username and password of the database administrator (created at installation of Jobs) for the jobs control database (metadata storage for jobs).</span></span>
   
    ![Namnge jobbet, Skriv eller klistra in koden och klicka på Kör][1]
3. <span data-ttu-id="7a44b-120">I den **skapa jobbet** bladet, ange ett namn för jobbet.</span><span class="sxs-lookup"><span data-stu-id="7a44b-120">In the **Create Job** blade, type a name for the job.</span></span>
4. <span data-ttu-id="7a44b-121">Ange användarnamn och lösenord för att ansluta till måldatabaserna med tillräcklig behörighet för att kunna köra skript.</span><span class="sxs-lookup"><span data-stu-id="7a44b-121">Type the user name and password to connect to the target databases with sufficient permissions for script execution to succeed.</span></span>
5. <span data-ttu-id="7a44b-122">Klistra in eller ange T-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="7a44b-122">Paste or type in the T-SQL script.</span></span>
6. <span data-ttu-id="7a44b-123">Klicka på **spara** och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="7a44b-123">Click **Save** and then click **Run**.</span></span>
   
    ![Skapa jobb och köra][5]

## <a name="run-idempotent-jobs"></a><span data-ttu-id="7a44b-125">Kör jobb för idempotent</span><span class="sxs-lookup"><span data-stu-id="7a44b-125">Run idempotent jobs</span></span>
<span data-ttu-id="7a44b-126">När du kör ett skript mot en uppsättning databaser, måste du vara säker på att skriptet finns idempotent.</span><span class="sxs-lookup"><span data-stu-id="7a44b-126">When you run a script against a set of databases, you must be sure that the script is idempotent.</span></span> <span data-ttu-id="7a44b-127">Det vill säga måste skriptet kunna köras flera gånger, även om den har misslyckats innan i ett ofullständigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7a44b-127">That is, the script must be able to run multiple times, even if it has failed before in an incomplete state.</span></span> <span data-ttu-id="7a44b-128">Till exempel när ett skript inte jobbet automatiskt görs tills den lyckas (inom intervallet, som det nya försöket logik slutligen upphör den försöker igen).</span><span class="sxs-lookup"><span data-stu-id="7a44b-128">For example, when a script fails, the job will be automatically retried until it succeeds (within limits, as the retry logic will eventually cease the retrying).</span></span> <span data-ttu-id="7a44b-129">Sätt att göra detta är att använda den en ”om det finns”-sats och ta bort alla hittade instansen innan du skapar ett nytt objekt.</span><span class="sxs-lookup"><span data-stu-id="7a44b-129">The way to do this is to use the an "IF EXISTS" clause and delete any found instance before creating a new object.</span></span> <span data-ttu-id="7a44b-130">Här visas ett exempel:</span><span class="sxs-lookup"><span data-stu-id="7a44b-130">An example is shown here:</span></span>

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

<span data-ttu-id="7a44b-131">Du kan också använda en ”om inte finns”-sats innan du skapar en ny instans:</span><span class="sxs-lookup"><span data-stu-id="7a44b-131">Alternatively, use an "IF NOT EXISTS" clause before creating a new instance:</span></span>

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

<span data-ttu-id="7a44b-132">Det här skriptet uppdaterar tabellen som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="7a44b-132">This script then updates the table created previously.</span></span>

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a><span data-ttu-id="7a44b-133">Kontrollera jobbstatus</span><span class="sxs-lookup"><span data-stu-id="7a44b-133">Checking job status</span></span>
<span data-ttu-id="7a44b-134">När ett jobb har startat, du kan kontrollera förloppet.</span><span class="sxs-lookup"><span data-stu-id="7a44b-134">After a job has begun, you can check on its progress.</span></span>

1. <span data-ttu-id="7a44b-135">Elastisk pool-sidan klickar du på **hantera jobb**.</span><span class="sxs-lookup"><span data-stu-id="7a44b-135">From the elastic pool page, click **Manage jobs**.</span></span>
   
    ![Klicka på ”Hantera jobb”][2]
2. <span data-ttu-id="7a44b-137">Klicka på namnet (a) för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="7a44b-137">Click on the name (a) of a job.</span></span> <span data-ttu-id="7a44b-138">Den **STATUS** kan vara ”slutfört” eller ”misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="7a44b-138">The **STATUS** can be "Completed" or "Failed."</span></span> <span data-ttu-id="7a44b-139">Jobbets information visas (b) med dess datum och tid för att skapa och köra.</span><span class="sxs-lookup"><span data-stu-id="7a44b-139">The job's details appear (b) with its date and time of creation and running.</span></span> <span data-ttu-id="7a44b-140">I listan (c) som visar status för skript mot varje databas i poolen, vilket ger information om datum och tid.</span><span class="sxs-lookup"><span data-stu-id="7a44b-140">The list (c) below the that shows the progress of the script against each database in the pool, giving its date and time details.</span></span>
   
    ![Kontrollen en klar][3]

## <a name="checking-failed-jobs"></a><span data-ttu-id="7a44b-142">Det gick inte att kontrollera jobb</span><span class="sxs-lookup"><span data-stu-id="7a44b-142">Checking failed jobs</span></span>
<span data-ttu-id="7a44b-143">Om ett jobb inte kan hitta en logg över exekveringen.</span><span class="sxs-lookup"><span data-stu-id="7a44b-143">If a job fails, a log of its execution can found.</span></span> <span data-ttu-id="7a44b-144">Klicka på namnet på det misslyckade jobbet om du vill visa information.</span><span class="sxs-lookup"><span data-stu-id="7a44b-144">Click the name of the failed job to see its details.</span></span>

![Markera ett jobb som misslyckades][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


