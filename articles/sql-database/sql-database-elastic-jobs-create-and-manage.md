---
title: aaaManage grupper med Azure SQL-databaser | Microsoft Docs
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
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a><span data-ttu-id="0e74d-103">Skapa och hantera skaländras ut Azure SQL-databaser med elastiska jobb (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0e74d-103">Create and manage scaled out Azure SQL Databases using elastic jobs (preview)</span></span>


<span data-ttu-id="0e74d-104">**Den elastiska databasen jobb** förenkla hanteringen av grupper av databaser genom att utföra administrativa åtgärder, till exempel schemaändringar, hantering av autentiseringsuppgifter, referens uppdateringar, insamling av prestandadata eller klient (kunden) telemetri samling.</span><span class="sxs-lookup"><span data-stu-id="0e74d-104">**Elastic Database jobs** simplify management of groups of databases by executing administrative operations such as schema changes, credentials management, reference data updates, performance data collection or tenant (customer) telemetry collection.</span></span> <span data-ttu-id="0e74d-105">Elastiska jobb i databasen är tillgänglig via hello Azure portal och PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0e74d-105">Elastic Database jobs is currently available through hello Azure portal and PowerShell cmdlets.</span></span> <span data-ttu-id="0e74d-106">Dock hello Azure portal hämtar nedsatt funktionalitet begränsad tooexecution över alla databaser i en [elastisk pool (förhandsgranskning)](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="0e74d-106">However, hello Azure portal surfaces reduced functionality limited tooexecution across all databases in an [elastic pool (preview)](sql-database-elastic-pool.md).</span></span> <span data-ttu-id="0e74d-107">ställa in tooaccess ytterligare funktioner och körning av skript i en grupp databaser inklusive en egendefinierat samling eller en Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-scale-introduction.md)), finns [skapa och hantera jobb med hjälp av PowerShell](sql-database-elastic-jobs-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0e74d-107">tooaccess additional features and execution of scripts across a group of databases including a custom-defined collection or a shard set (created using [Elastic Database client library](sql-database-elastic-scale-introduction.md)), see [Creating and managing jobs using PowerShell](sql-database-elastic-jobs-powershell.md).</span></span> <span data-ttu-id="0e74d-108">Mer information om jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0e74d-108">For more information about jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0e74d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="0e74d-109">Prerequisites</span></span>
* <span data-ttu-id="0e74d-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0e74d-110">An Azure subscription.</span></span> <span data-ttu-id="0e74d-111">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e74d-111">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0e74d-112">En elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="0e74d-112">An elastic pool.</span></span> <span data-ttu-id="0e74d-113">Se [om elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="0e74d-113">See [About elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="0e74d-114">Installation av tjänstkomponenter för elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="0e74d-114">Installation of elastic database job service components.</span></span> <span data-ttu-id="0e74d-115">Se [installerar hello elastiska jobb databastjänsten](sql-database-elastic-jobs-service-installation.md).</span><span class="sxs-lookup"><span data-stu-id="0e74d-115">See [Installing hello elastic database job service](sql-database-elastic-jobs-service-installation.md).</span></span>

## <a name="creating-jobs"></a><span data-ttu-id="0e74d-116">Jobb</span><span class="sxs-lookup"><span data-stu-id="0e74d-116">Creating jobs</span></span>
1. <span data-ttu-id="0e74d-117">Med hjälp av hello [Azure-portalen](https://portal.azure.com), från en befintlig elastisk jobbet databaspool, klicka på **skapa jobbet**.</span><span class="sxs-lookup"><span data-stu-id="0e74d-117">Using hello [Azure portal](https://portal.azure.com), from an existing elastic database job pool, click **Create job**.</span></span>
2. <span data-ttu-id="0e74d-118">Ange hello användarnamnet och lösenordet för hello databasadministratören (skapas vid installationen av jobb) för databasen för hello jobb (metadata lagring för jobb).</span><span class="sxs-lookup"><span data-stu-id="0e74d-118">Type in hello username and password of hello database administrator (created at installation of Jobs) for hello jobs control database (metadata storage for jobs).</span></span>
   
    ![Hello jobb, Skriv eller klistra in koden och klicka på Kör][1]
3. <span data-ttu-id="0e74d-120">I hello **skapa jobbet** bladet, ange ett namn för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0e74d-120">In hello **Create Job** blade, type a name for hello job.</span></span>
4. <span data-ttu-id="0e74d-121">Ange hello användarens namn och lösenord tooconnect toohello måldatabaserna med tillräcklig behörighet för toosucceed för körning av skript.</span><span class="sxs-lookup"><span data-stu-id="0e74d-121">Type hello user name and password tooconnect toohello target databases with sufficient permissions for script execution toosucceed.</span></span>
5. <span data-ttu-id="0e74d-122">Klistra in eller ange hello T-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="0e74d-122">Paste or type in hello T-SQL script.</span></span>
6. <span data-ttu-id="0e74d-123">Klicka på **spara** och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="0e74d-123">Click **Save** and then click **Run**.</span></span>
   
    ![Skapa jobb och köra][5]

## <a name="run-idempotent-jobs"></a><span data-ttu-id="0e74d-125">Kör jobb för idempotent</span><span class="sxs-lookup"><span data-stu-id="0e74d-125">Run idempotent jobs</span></span>
<span data-ttu-id="0e74d-126">När du kör ett skript mot en uppsättning databaser måste du vara säker på att hello skript är idempotent.</span><span class="sxs-lookup"><span data-stu-id="0e74d-126">When you run a script against a set of databases, you must be sure that hello script is idempotent.</span></span> <span data-ttu-id="0e74d-127">Det vill säga måste hello skriptet vara kan toorun flera gånger, även om den har misslyckats innan i ett ofullständigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0e74d-127">That is, hello script must be able toorun multiple times, even if it has failed before in an incomplete state.</span></span> <span data-ttu-id="0e74d-128">Till exempel när ett skript inte hello jobb automatiskt görs tills den lyckas (inom intervallet, som hello försök logik slutligen upphöra hello du försöker).</span><span class="sxs-lookup"><span data-stu-id="0e74d-128">For example, when a script fails, hello job will be automatically retried until it succeeds (within limits, as hello retry logic will eventually cease hello retrying).</span></span> <span data-ttu-id="0e74d-129">hello sätt toodo detta är toouse hello instruktionen ”om det finns” och ta bort alla hittade instansen innan du skapar ett nytt objekt.</span><span class="sxs-lookup"><span data-stu-id="0e74d-129">hello way toodo this is toouse hello an "IF EXISTS" clause and delete any found instance before creating a new object.</span></span> <span data-ttu-id="0e74d-130">Här visas ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0e74d-130">An example is shown here:</span></span>

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

<span data-ttu-id="0e74d-131">Du kan också använda en ”om inte finns”-sats innan du skapar en ny instans:</span><span class="sxs-lookup"><span data-stu-id="0e74d-131">Alternatively, use an "IF NOT EXISTS" clause before creating a new instance:</span></span>

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

<span data-ttu-id="0e74d-132">Det här skriptet uppdaterar hello-tabell som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="0e74d-132">This script then updates hello table created previously.</span></span>

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a><span data-ttu-id="0e74d-133">Kontrollera jobbstatus</span><span class="sxs-lookup"><span data-stu-id="0e74d-133">Checking job status</span></span>
<span data-ttu-id="0e74d-134">När ett jobb har startat, du kan kontrollera förloppet.</span><span class="sxs-lookup"><span data-stu-id="0e74d-134">After a job has begun, you can check on its progress.</span></span>

1. <span data-ttu-id="0e74d-135">Hello elastisk pool sidan klickar du på **hantera jobb**.</span><span class="sxs-lookup"><span data-stu-id="0e74d-135">From hello elastic pool page, click **Manage jobs**.</span></span>
   
    ![Klicka på ”Hantera jobb”][2]
2. <span data-ttu-id="0e74d-137">Klicka på hello namn (a) för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="0e74d-137">Click on hello name (a) of a job.</span></span> <span data-ttu-id="0e74d-138">Hej **STATUS** kan vara ”slutfört” eller ”misslyckades”.</span><span class="sxs-lookup"><span data-stu-id="0e74d-138">hello **STATUS** can be "Completed" or "Failed."</span></span> <span data-ttu-id="0e74d-139">hello jobbinformation visas (b) med dess datum och tid för att skapa och köra.</span><span class="sxs-lookup"><span data-stu-id="0e74d-139">hello job's details appear (b) with its date and time of creation and running.</span></span> <span data-ttu-id="0e74d-140">hello lista (c) under hello som visar hello fortskrider hello-skript mot varje databas i hello pool ger information om datum och tid.</span><span class="sxs-lookup"><span data-stu-id="0e74d-140">hello list (c) below hello that shows hello progress of hello script against each database in hello pool, giving its date and time details.</span></span>
   
    ![Kontrollen en klar][3]

## <a name="checking-failed-jobs"></a><span data-ttu-id="0e74d-142">Det gick inte att kontrollera jobb</span><span class="sxs-lookup"><span data-stu-id="0e74d-142">Checking failed jobs</span></span>
<span data-ttu-id="0e74d-143">Om ett jobb inte kan hitta en logg över exekveringen.</span><span class="sxs-lookup"><span data-stu-id="0e74d-143">If a job fails, a log of its execution can found.</span></span> <span data-ttu-id="0e74d-144">Klicka på hello misslyckades jobb toosee hello namn information.</span><span class="sxs-lookup"><span data-stu-id="0e74d-144">Click hello name of hello failed job toosee its details.</span></span>

![Markera ett jobb som misslyckades][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


