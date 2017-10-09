---
title: "aaaGetting igång med elastisk databas jobb | Microsoft Docs"
description: "hur toouse jobb för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 2540de0e-2235-4cdd-9b6a-b841adba00e5
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: bc5894d2df4235738ab961db4f69c11cdf786cc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a><span data-ttu-id="539fb-103">Komma igång med jobb för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="539fb-103">Getting started with Elastic Database jobs</span></span>
<span data-ttu-id="539fb-104">Den elastiska databasen jobb (förhandsversion) för Azure SQL Database kan du tooreliability köra T-SQL-skript som sträcker sig över flera databaser när du försöker automatiskt och garanterar att tillhandahålla eventuell slutförande.</span><span class="sxs-lookup"><span data-stu-id="539fb-104">Elastic Database jobs (preview) for Azure SQL Database allows you tooreliability execute T-SQL scripts that span multiple databases while automatically retrying and providing eventual completion guarantees.</span></span> <span data-ttu-id="539fb-105">Mer information om hello elastiska jobb databasfunktion finns hello [översikt funktionssidan](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="539fb-105">For more information about hello Elastic Database job feature, please see hello [feature overview page](sql-database-elastic-jobs-overview.md).</span></span>

<span data-ttu-id="539fb-106">Det här avsnittet utökar hello-exempel finns i [komma igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="539fb-106">This topic extends hello sample found in [Getting started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span> <span data-ttu-id="539fb-107">När du är klar, kommer: Lär dig hur toocreate och hantera jobb som hanterar en grupp av relaterade databaser.</span><span class="sxs-lookup"><span data-stu-id="539fb-107">When completed, you will: learn how toocreate and manage jobs that manage a group of related databases.</span></span> <span data-ttu-id="539fb-108">Det är inte obligatoriska toouse hello elastisk skalbarhet verktyg i ordning tootake nytta av hello fördelarna med elastiska jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-108">It is not required toouse hello Elastic Scale tools in order tootake advantage of hello benefits of Elastic jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="539fb-109">Krav</span><span class="sxs-lookup"><span data-stu-id="539fb-109">Prerequisites</span></span>
<span data-ttu-id="539fb-110">Hämta och köra hello [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="539fb-110">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="539fb-111">Skapa en Fragmentera karta manager med hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="539fb-111">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="539fb-112">Här skapar du en karta Fragmentera manager tillsammans med flera delar, följt av infogning av data i hello delar.</span><span class="sxs-lookup"><span data-stu-id="539fb-112">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="539fb-113">Om du redan har shards med shardade data i dem kan du hoppa över hello följande steg och flytta toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="539fb-113">If you already have shards set up with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="539fb-114">Skapa och köra hello **komma igång med elastiska Databasverktyg** exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="539fb-114">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="539fb-115">Gör hello förrän steg 7 i hello avsnittet [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="539fb-115">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="539fb-116">Hello slutet av steg 7 visas hello följande kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="539fb-116">At hello end of Step 7, you will see hello following command prompt:</span></span>

   ![kommandotolk](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. <span data-ttu-id="539fb-118">Skriv ”1” i hello-kommandotolk och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="539fb-118">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="539fb-119">Detta skapar hello Fragmentera kartan manager och lägger till två delar toohello server.</span><span class="sxs-lookup"><span data-stu-id="539fb-119">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="539fb-120">Sedan skriver ”3” och tryck på **RETUR**; Upprepa åtgärden fyra gånger.</span><span class="sxs-lookup"><span data-stu-id="539fb-120">Then type "3" and press **Enter**; repeat this action four times.</span></span> <span data-ttu-id="539fb-121">Detta infogar exempel datarader i din shards.</span><span class="sxs-lookup"><span data-stu-id="539fb-121">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="539fb-122">Hej [Azure Portal](https://portal.azure.com) tre nya databaser som ska visas:</span><span class="sxs-lookup"><span data-stu-id="539fb-122">hello [Azure Portal](https://portal.azure.com) should show three new databases:</span></span>

   ![Visual Studio-bekräftelse](./media/sql-database-elastic-query-getting-started/portal.png)

   <span data-ttu-id="539fb-124">Nu skapar vi en anpassad databas-samling som visar alla hello databaser i hello Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="539fb-124">At this point, we will create a custom database collection that reflects all hello databases in hello shard map.</span></span> <span data-ttu-id="539fb-125">Detta innebär att vi toocreate och köra ett jobb som lägger till en ny tabell över shards.</span><span class="sxs-lookup"><span data-stu-id="539fb-125">This will allow us toocreate and execute a job that add a new table across shards.</span></span>

<span data-ttu-id="539fb-126">Här vi skulle vanligtvis skapa en karta mål Fragmentera med hello **ny AzureSqlJobTarget** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="539fb-126">Here we would usually create a shard map target, using hello **New-AzureSqlJobTarget** cmdlet.</span></span> <span data-ttu-id="539fb-127">hello Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan hello specifika Fragmentera mappning har angetts som mål.</span><span class="sxs-lookup"><span data-stu-id="539fb-127">hello shard map manager database must be set as a database target and then hello specific shard map is specified as a target.</span></span> <span data-ttu-id="539fb-128">I stället vi kommer tooenumerate alla hello databaser i hello server och Lägg till hello databaser toohello nya anpassade samlingar med hello undantag av master-databasen.</span><span class="sxs-lookup"><span data-stu-id="539fb-128">Instead, we are going tooenumerate all hello databases in hello server and add hello databases toohello new custom collection with hello exception of master database.</span></span>

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a><span data-ttu-id="539fb-129">Skapar en anpassad samling och lägger till alla databaser i hello server toohello anpassad samling mål med hello undantag av master.</span><span class="sxs-lookup"><span data-stu-id="539fb-129">Creates a custom collection and add all databases in hello server toohello custom collection target with hello exception of master.</span></span>
   ```
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in hello custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="539fb-130">Skapa ett T-SQL-skript för körning på databaser</span><span class="sxs-lookup"><span data-stu-id="539fb-130">Create a T-SQL Script for execution across databases</span></span>
   ```
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a><span data-ttu-id="539fb-131">Skapa ett skript för hello jobbet tooexecute över hello anpassad grupp av databaser</span><span class="sxs-lookup"><span data-stu-id="539fb-131">Create hello job tooexecute a script across hello custom group of databases</span></span>

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a><span data-ttu-id="539fb-132">Köra hello jobb</span><span class="sxs-lookup"><span data-stu-id="539fb-132">Execute hello job</span></span>
<span data-ttu-id="539fb-133">hello följande PowerShell-skript kan vara används tooexecute ett befintligt jobb:</span><span class="sxs-lookup"><span data-stu-id="539fb-133">hello following PowerShell script can be used tooexecute an existing job:</span></span>

<span data-ttu-id="539fb-134">Uppdatera hello variabeln tooreflect hello önskad jobbet namn toohave utförs följande:</span><span class="sxs-lookup"><span data-stu-id="539fb-134">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="539fb-135">Hämta hello tillståndet för en enskild jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="539fb-135">Retrieve hello state of a single job execution</span></span>
<span data-ttu-id="539fb-136">Använd hello samma **Get-AzureSqlJobExecution** med hello **IncludeChildren** parametern tooview hello tillstånd för underordnad jobbet körningar, nämligen hello specifikt tillstånd för varje jobbkörningen mot varje databasen som mål för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-136">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a><span data-ttu-id="539fb-137">Visa status för hello över flera jobb körningar</span><span class="sxs-lookup"><span data-stu-id="539fb-137">View hello state across multiple job executions</span></span>
<span data-ttu-id="539fb-138">Hej **Get-AzureSqlJobExecution** cmdlet har flera valfria parametrar som kan använda toodisplay flera jobb körningar, filtreras via hello angivna parametrar.</span><span class="sxs-lookup"><span data-stu-id="539fb-138">hello **Get-AzureSqlJobExecution** cmdlet has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="539fb-139">hello nedan visar några av hello möjliga sätt toouse Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="539fb-139">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="539fb-140">Hämta alla aktiva översta nivå jobbet körningar:</span><span class="sxs-lookup"><span data-stu-id="539fb-140">Retrieve all active top level job executions:</span></span>

   ```
    Get-AzureSqlJobExecution
   ```

<span data-ttu-id="539fb-141">Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="539fb-141">Retrieve all top level job executions, including inactive job executions:</span></span>

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

<span data-ttu-id="539fb-142">Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="539fb-142">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

<span data-ttu-id="539fb-143">Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="539fb-143">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

<span data-ttu-id="539fb-144">Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="539fb-144">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="539fb-145">Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="539fb-145">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="539fb-146">Hämta hello lista över jobb uppgiften körningar inom en specifik jobbkörningen:</span><span class="sxs-lookup"><span data-stu-id="539fb-146">Retrieve hello list of job task executions within a specific job execution:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

<span data-ttu-id="539fb-147">Hämta jobb aktivitetsinformation körning:</span><span class="sxs-lookup"><span data-stu-id="539fb-147">Retrieve job task execution details:</span></span>

<span data-ttu-id="539fb-148">hello följande PowerShell-skript kan vara används tooview hello information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.</span><span class="sxs-lookup"><span data-stu-id="539fb-148">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a><span data-ttu-id="539fb-149">Hämta fel i jobb uppgiften körningar</span><span class="sxs-lookup"><span data-stu-id="539fb-149">Retrieve failures within job task executions</span></span>
<span data-ttu-id="539fb-150">Hej JobTaskExecution objektet innehåller en egenskap för hello livscykeln för hello uppgiften tillsammans med en meddelandeegenskap.</span><span class="sxs-lookup"><span data-stu-id="539fb-150">hello JobTaskExecution object includes a property for hello Lifecycle of hello task along with a Message property.</span></span> <span data-ttu-id="539fb-151">Om ett jobb för körning av aktiviteten misslyckades hello livscykel egenskapen ställs in för*misslyckades* och kommer att anges hello meddelandeegenskap toohello resulterande Undantagsmeddelande och dess stacken.</span><span class="sxs-lookup"><span data-stu-id="539fb-151">If a job task execution failed, hello Lifecycle property will be set too*Failed* and hello Message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="539fb-152">Om ett jobb misslyckades, är det viktigt tooview hello information om jobbuppgifter som misslyckades för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-152">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-toocomplete"></a><span data-ttu-id="539fb-153">Väntar på att en toocomplete för körning av jobbet</span><span class="sxs-lookup"><span data-stu-id="539fb-153">Waiting for a job execution toocomplete</span></span>
<span data-ttu-id="539fb-154">hello följande PowerShell-skript kan vara används toowait för ett jobb uppgiften toocomplete:</span><span class="sxs-lookup"><span data-stu-id="539fb-154">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="539fb-155">Skapa en princip för anpassad körning</span><span class="sxs-lookup"><span data-stu-id="539fb-155">Create a custom execution policy</span></span>
<span data-ttu-id="539fb-156">Den elastiska databasen jobb kan du skapa anpassade körningsprinciper som kan användas när du startar jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-156">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="539fb-157">Körningsprinciper tillåter för närvarande för att definiera:</span><span class="sxs-lookup"><span data-stu-id="539fb-157">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="539fb-158">Namn: Identifierare för hello körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="539fb-158">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="539fb-159">Tidsgräns för jobb: Total tid innan ett jobb kommer att avbrytas av elastiska databasen jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-159">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="539fb-160">Inledande återförsöksintervall: Intervall toowait innan nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="539fb-160">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="539fb-161">Maximal återförsöksintervall: Locket försök intervall toouse.</span><span class="sxs-lookup"><span data-stu-id="539fb-161">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="539fb-162">Försök intervall Backoff värde: Värde används toocalculate hello nästa intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="539fb-162">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="539fb-163">hello används följande formel: (första försök intervall) * Math.pow ((intervall Backoff värde) (antal nya försök) - 2).</span><span class="sxs-lookup"><span data-stu-id="539fb-163">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span>
* <span data-ttu-id="539fb-164">Maximalt antal försök: hello maximalt antal försök försök tooperform inom ett jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-164">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="539fb-165">hello standard körningsprincipen använder hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="539fb-165">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="539fb-166">Namn: Standardprincipen för körning</span><span class="sxs-lookup"><span data-stu-id="539fb-166">Name: Default execution policy</span></span>
* <span data-ttu-id="539fb-167">Tidsgräns för jobb: 1 vecka</span><span class="sxs-lookup"><span data-stu-id="539fb-167">Job Timeout: 1 week</span></span>
* <span data-ttu-id="539fb-168">Inledande återförsöksintervall: 100 millisekunder</span><span class="sxs-lookup"><span data-stu-id="539fb-168">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="539fb-169">Maximal återförsöksintervall: 30 minuter</span><span class="sxs-lookup"><span data-stu-id="539fb-169">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="539fb-170">Försök intervall värde: 2</span><span class="sxs-lookup"><span data-stu-id="539fb-170">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="539fb-171">Maximalt antal försök: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="539fb-171">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="539fb-172">Skapa hello önskad körningsprincipen:</span><span class="sxs-lookup"><span data-stu-id="539fb-172">Create hello desired execution policy:</span></span>

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="539fb-173">Uppdatera en anpassad körningsprincip</span><span class="sxs-lookup"><span data-stu-id="539fb-173">Update a custom execution policy</span></span>
<span data-ttu-id="539fb-174">Uppdatera hello önskad körning princip tooupdate:</span><span class="sxs-lookup"><span data-stu-id="539fb-174">Update hello desired execution policy tooupdate:</span></span>

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a><span data-ttu-id="539fb-175">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="539fb-175">Cancel a job</span></span>
<span data-ttu-id="539fb-176">Den elastiska databasen jobb stöder jobb annullering begäranden.</span><span class="sxs-lookup"><span data-stu-id="539fb-176">Elastic Database Jobs supports jobs cancellation requests.</span></span>  <span data-ttu-id="539fb-177">Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den toostop hello jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-177">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="539fb-178">Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:</span><span class="sxs-lookup"><span data-stu-id="539fb-178">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="539fb-179">Avbryta pågående aktiviteter: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom hello aspekt av hello aktivitet som körs.</span><span class="sxs-lookup"><span data-stu-id="539fb-179">Canceling Currently Executing Tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="539fb-180">Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är en försök toocancel hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="539fb-180">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="539fb-181">Annullering återförsök för aktiviteten: Om en annullering identifieras av hello kontrollen tråd innan en uppgift startas för körning hello kontrollen tråd undvika starta hello aktivitet och deklarera hello begäran som avbruten.</span><span class="sxs-lookup"><span data-stu-id="539fb-181">Canceling Task Retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="539fb-182">Om det krävs en annullering av jobbet för överordnade jobb respekteras hello avbrottsbegäran för hello överordnade jobb och alla dess underordnade jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-182">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="539fb-183">toosubmit en begäran om att avbryta, använda hello **stoppa AzureSqlJobExecution** cmdlet och ange hello **JobExecutionId** parameter.</span><span class="sxs-lookup"><span data-stu-id="539fb-183">toosubmit a cancellation request, use hello **Stop-AzureSqlJobExecution** cmdlet and set hello **JobExecutionId** parameter.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a><span data-ttu-id="539fb-184">Ta bort ett jobb efter namn och hello jobbhistorik</span><span class="sxs-lookup"><span data-stu-id="539fb-184">Delete a job by name and hello job's history</span></span>
<span data-ttu-id="539fb-185">Den elastiska databasen jobb stöder asynkrona borttagning av jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-185">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="539fb-186">Ett jobb kan vara markerad för borttagning och hello tas bort hello jobb och alla dess jobbhistorik när alla jobb körningar har slutförts för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="539fb-186">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="539fb-187">hello system Avbryt inte automatiskt aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="539fb-187">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="539fb-188">Stoppa AzureSqlJobExecution måste i stället vara anropade toocancel aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="539fb-188">Instead, Stop-AzureSqlJobExecution must be invoked toocancel active job executions.</span></span>

<span data-ttu-id="539fb-189">tootrigger jobbet borttagning, Använd hello **ta bort AzureSqlJob** cmdlet och ange hello **jobbnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="539fb-189">tootrigger job deletion, use hello **Remove-AzureSqlJob** cmdlet and set hello **JobName** parameter.</span></span>

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a><span data-ttu-id="539fb-190">Skapa en anpassad databas mål</span><span class="sxs-lookup"><span data-stu-id="539fb-190">Create a custom database target</span></span>
<span data-ttu-id="539fb-191">Anpassad databas mål kan definieras i jobb för elastisk databas som kan användas för att köras direkt eller som ska ingå i en anpassad databas-grupp.</span><span class="sxs-lookup"><span data-stu-id="539fb-191">Custom database targets can be defined in Elastic Database jobs which can be used either for execution directly or for inclusion within a custom database group.</span></span> <span data-ttu-id="539fb-192">Eftersom **elastiska pooler** ännu direkt stöds inte via hello PowerShell APIs du helt enkelt skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla hello-databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="539fb-192">Since **elastic pools** are not yet directly supported via hello PowerShell APIs, you simply create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="539fb-193">Ange följande variabler tooreflect hello önskad databasinformation hello:</span><span class="sxs-lookup"><span data-stu-id="539fb-193">Set hello following variables tooreflect hello desired database information:</span></span>

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a><span data-ttu-id="539fb-194">Skapa en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="539fb-194">Create a custom database collection target</span></span>
<span data-ttu-id="539fb-195">En anpassad databas samling mål kan vara definierade tooenable körning över flera definierade databasen mål.</span><span class="sxs-lookup"><span data-stu-id="539fb-195">A custom database collection target can be defined tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="539fb-196">När du har skapat en databasgrupp kan databaser vara associerade toohello anpassad samling mål.</span><span class="sxs-lookup"><span data-stu-id="539fb-196">After a database group is created, databases can be associated toohello custom collection target.</span></span>

<span data-ttu-id="539fb-197">Ange hello följande variabler tooreflect hello önskade anpassade samlingar mål konfiguration:</span><span class="sxs-lookup"><span data-stu-id="539fb-197">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="539fb-198">Lägg till databaser tooa anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="539fb-198">Add databases tooa custom database collection target</span></span>
<span data-ttu-id="539fb-199">Databasen mål kan vara associerat med anpassad databas samling mål toocreate en grupp databaser.</span><span class="sxs-lookup"><span data-stu-id="539fb-199">Database targets can be associated with custom database collection targets toocreate a group of databases.</span></span> <span data-ttu-id="539fb-200">När ett jobb skapas som en anpassad databas samling mål, blir det utökade tootarget hello databaserna associerade toohello grupp på hello tiden för körningen.</span><span class="sxs-lookup"><span data-stu-id="539fb-200">Whenever a job is created that targets a custom database collection target, it will be expanded tootarget hello databases associated toohello group at hello time of execution.</span></span>

<span data-ttu-id="539fb-201">Lägg till hello önskad databasen tooa specifika anpassade samlingar:</span><span class="sxs-lookup"><span data-stu-id="539fb-201">Add hello desired database tooa specific custom collection:</span></span>

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="539fb-202">Granska hello databaser i en anpassad databas samling målet</span><span class="sxs-lookup"><span data-stu-id="539fb-202">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="539fb-203">Använd hello **Get-AzureSqlJobTarget** cmdlet tooretrieve hello underordnade databaser i en anpassad databas samling målet.</span><span class="sxs-lookup"><span data-stu-id="539fb-203">Use hello **Get-AzureSqlJobTarget** cmdlet tooretrieve hello child databases within a custom database collection target.</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="539fb-204">Skapa ett skript i ett jobb tooexecute över en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="539fb-204">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="539fb-205">Använd hello **ny AzureSqlJob** cmdlet toocreate ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål.</span><span class="sxs-lookup"><span data-stu-id="539fb-205">Use hello **New-AzureSqlJob** cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="539fb-206">Den elastiska databasen jobb kommer att expandera hello jobbet till flera underordnade jobb varje motsvarande tooa-databas som är associerade med hello anpassad databas samling mål och se till att hello skriptet körs mot varje databas.</span><span class="sxs-lookup"><span data-stu-id="539fb-206">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="539fb-207">Igen, är det viktigt att skripten är idempotent toobe flexibel tooretries.</span><span class="sxs-lookup"><span data-stu-id="539fb-207">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a><span data-ttu-id="539fb-208">Insamling av data över databaser</span><span class="sxs-lookup"><span data-stu-id="539fb-208">Data collection across databases</span></span>
<span data-ttu-id="539fb-209">**Den elastiska databasen jobb** stöder köra en fråga i en grupp med databaser och skickar hello-resultat-tooa angivna database-tabellen.</span><span class="sxs-lookup"><span data-stu-id="539fb-209">**Elastic Database jobs** supports executing a query across a group of databases and sends hello results tooa specified database’s table.</span></span> <span data-ttu-id="539fb-210">hello tabellen kan efterfrågas efter hello fakta toosee hello frågans resultat från varje databas.</span><span class="sxs-lookup"><span data-stu-id="539fb-210">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="539fb-211">Detta ger en mekanism för asynkron tooexecute en fråga över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="539fb-211">This provides an asynchronous mechanism tooexecute a query across many databases.</span></span> <span data-ttu-id="539fb-212">Fel fall, till exempel hello databaserna tillfälligt otillgänglig hanteras automatiskt via återförsök.</span><span class="sxs-lookup"><span data-stu-id="539fb-212">Failure cases such as one of hello databases being temporarily unavailable are handled automatically via retries.</span></span>

<span data-ttu-id="539fb-213">hello angivna måltabellen skapas automatiskt om det inte finns ännu, matchande hello schemat för hello returnerade en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="539fb-213">hello specified destination table will be automatically created if it does not yet exist, matching hello schema of hello returned result set.</span></span> <span data-ttu-id="539fb-214">Om en skriptkörningen returnerar flera resultatmängder, skickas endast elastisk databas jobb hello första måltabellen för en toohello som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="539fb-214">If a script execution returns multiple result sets, Elastic Database jobs will only send hello first one toohello provided destination table.</span></span>

<span data-ttu-id="539fb-215">hello följande PowerShell-skript kan vara används tooexecute ett skript som samlar in resultaten till en angiven tabell.</span><span class="sxs-lookup"><span data-stu-id="539fb-215">hello following PowerShell script can be used tooexecute a script collecting its results into a specified table.</span></span> <span data-ttu-id="539fb-216">Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.</span><span class="sxs-lookup"><span data-stu-id="539fb-216">This script assumes that a T-SQL script has been created which outputs a single result set and a custom database collection target has been created.</span></span>

<span data-ttu-id="539fb-217">Ange hello följande tooreflect hello önskad skript, autentiseringsuppgifter och körning av mål:</span><span class="sxs-lookup"><span data-stu-id="539fb-217">Set hello following tooreflect hello desired script, credentials, and execution target:</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="539fb-218">Skapa och starta ett jobb för scenarion för insamling av data</span><span class="sxs-lookup"><span data-stu-id="539fb-218">Create and start a job for data collection scenarios</span></span>
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a><span data-ttu-id="539fb-219">Skapa ett schema för jobbkörningen med en utlösare för jobb</span><span class="sxs-lookup"><span data-stu-id="539fb-219">Create a schedule for job execution using a job trigger</span></span>
<span data-ttu-id="539fb-220">hello följande PowerShell-skript kan vara används toocreate reoccurring schema.</span><span class="sxs-lookup"><span data-stu-id="539fb-220">hello following PowerShell script can be used toocreate a reoccurring schedule.</span></span> <span data-ttu-id="539fb-221">Det här skriptet använder en minuts intervall, men ny AzureSqlJobSchedule stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar.</span><span class="sxs-lookup"><span data-stu-id="539fb-221">This script uses a one minute interval, but New-AzureSqlJobSchedule also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="539fb-222">Scheman som kör en gång kan skapas med skicka - görs.</span><span class="sxs-lookup"><span data-stu-id="539fb-222">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="539fb-223">Skapa ett nytt schema:</span><span class="sxs-lookup"><span data-stu-id="539fb-223">Create a new schedule:</span></span>
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="539fb-224">Skapa ett jobb utlösaren toohave ett jobb som körs på en tidsplan</span><span class="sxs-lookup"><span data-stu-id="539fb-224">Create a job trigger toohave a job executed on a time schedule</span></span>
<span data-ttu-id="539fb-225">En utlösare för jobb kan vara definierade toohave ett jobb körs bl.a tooa tid schema.</span><span class="sxs-lookup"><span data-stu-id="539fb-225">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="539fb-226">hello följande PowerShell-skript kan vara används toocreate en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="539fb-226">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="539fb-227">Ange hello följande variabler toocorrespond toohello önskade jobb och schemalägga:</span><span class="sxs-lookup"><span data-stu-id="539fb-227">Set hello following variables toocorrespond toohello desired job and schedule:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="539fb-228">Ta bort en schemalagd association toostop jobbet körs enligt schema</span><span class="sxs-lookup"><span data-stu-id="539fb-228">Remove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="539fb-229">toodiscontinue igen jobbkörningen via jobb utlösare hello jobbet utlösare kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="539fb-229">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span>
<span data-ttu-id="539fb-230">Ta bort ett jobb utlösaren toostop ett jobb från utförs bl.a tooa schema med hello **ta bort AzureSqlJobTrigger** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="539fb-230">Remove a job trigger toostop a job from being executed according tooa schedule using hello **Remove-AzureSqlJobTrigger** cmdlet.</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="539fb-231">Importera elastisk databas frågan resultat tooExcel</span><span class="sxs-lookup"><span data-stu-id="539fb-231">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="539fb-232">Du kan importera hello resultaten från av en fråga tooan Excel-fil.</span><span class="sxs-lookup"><span data-stu-id="539fb-232">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="539fb-233">Starta Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="539fb-233">Launch Excel 2013.</span></span>
2. <span data-ttu-id="539fb-234">Navigera toohello **Data** menyfliksområdet.</span><span class="sxs-lookup"><span data-stu-id="539fb-234">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="539fb-235">Klicka på **från andra källor** och på **från SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="539fb-235">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel-import från andra källor](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. <span data-ttu-id="539fb-237">I hello **Dataanslutningsguiden** skriver hello server namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="539fb-237">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="539fb-238">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="539fb-238">Then click **Next**.</span></span>
5. <span data-ttu-id="539fb-239">I dialogrutan för hello **väljer hello-databas som innehåller hello data som du vill**väljer hello **ElasticDBQuery** databas.</span><span class="sxs-lookup"><span data-stu-id="539fb-239">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="539fb-240">Välj hello **kunder** tabell i hello listvyn och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="539fb-240">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="539fb-241">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="539fb-241">Then click **Finish**.</span></span>
7. <span data-ttu-id="539fb-242">I hello **importera Data** formuläret under **väljer du hur tooview dessa data i arbetsboken**väljer **tabell** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="539fb-242">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="539fb-243">Alla hello rader från **kunder** tabell som sparas i olika delar fylla hello Excel-blad.</span><span class="sxs-lookup"><span data-stu-id="539fb-243">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="539fb-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="539fb-244">Next steps</span></span>
<span data-ttu-id="539fb-245">Du kan nu använda Excel-data.</span><span class="sxs-lookup"><span data-stu-id="539fb-245">You can now use Excel’s data functions.</span></span> <span data-ttu-id="539fb-246">Använd hello anslutningssträngen med servernamnet, databasnamnet och autentiseringsuppgifter tooconnect BI och data integration verktyg toohello elastisk fråga databasen.</span><span class="sxs-lookup"><span data-stu-id="539fb-246">Use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="539fb-247">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="539fb-247">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="539fb-248">Läs toohello elastisk fråga databas och externa tabeller precis som andra SQL Server-databas och att du vill ansluta toowith verktyget du SQL Server-tabeller.</span><span class="sxs-lookup"><span data-stu-id="539fb-248">Refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="539fb-249">Kostnad</span><span class="sxs-lookup"><span data-stu-id="539fb-249">Cost</span></span>
<span data-ttu-id="539fb-250">Det finns utan extra kostnad för funktionen hello elastisk databas frågan.</span><span class="sxs-lookup"><span data-stu-id="539fb-250">There is no additional charge for using hello Elastic Database query feature.</span></span> <span data-ttu-id="539fb-251">Dock just nu den här funktionen är bara tillgängliga på premiumdatabaser som en slutpunkt men hello shards kan vara av en tjänstnivå.</span><span class="sxs-lookup"><span data-stu-id="539fb-251">However, at this time this feature is available only on premium databases as an end point, but hello shards can be of any service tier.</span></span>

<span data-ttu-id="539fb-252">Mer information om priser finns [prisinformation för SQL-databasen](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="539fb-252">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
