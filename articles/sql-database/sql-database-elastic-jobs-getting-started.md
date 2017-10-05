---
title: "Komma igång med elastisk databas jobb | Microsoft Docs"
description: "hur du använder jobb för elastisk databas"
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
ms.openlocfilehash: 05c20e880d4eb1eacdecc0c4c7e7491dfe1e6a89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a><span data-ttu-id="d2a35-103">Komma igång med jobb för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="d2a35-103">Getting started with Elastic Database jobs</span></span>
<span data-ttu-id="d2a35-104">Elastiska databasen jobb (förhandsversion) för Azure SQL Database kan du tillförlitlighet köra T-SQL-skript som sträcker sig över flera databaser när du försöker och ge garantier för eventuell slutförande automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d2a35-104">Elastic Database jobs (preview) for Azure SQL Database allows you to reliability execute T-SQL scripts that span multiple databases while automatically retrying and providing eventual completion guarantees.</span></span> <span data-ttu-id="d2a35-105">Mer information om funktionen för elastisk databas jobb finns i [översikt funktionssidan](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2a35-105">For more information about the Elastic Database job feature, please see the [feature overview page](sql-database-elastic-jobs-overview.md).</span></span>

<span data-ttu-id="d2a35-106">Det här avsnittet utökar exemplet hittades i [komma igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d2a35-106">This topic extends the sample found in [Getting started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span> <span data-ttu-id="d2a35-107">När du är klar, kommer: Lär dig att skapa och hantera jobb som hanterar en grupp av relaterade databaser.</span><span class="sxs-lookup"><span data-stu-id="d2a35-107">When completed, you will: learn how to create and manage jobs that manage a group of related databases.</span></span> <span data-ttu-id="d2a35-108">Du behöver inte använda verktygen elastisk skalbarhet för att kunna dra nytta av fördelarna med elastiska jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-108">It is not required to use the Elastic Scale tools in order to take advantage of the benefits of Elastic jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2a35-109">Krav</span><span class="sxs-lookup"><span data-stu-id="d2a35-109">Prerequisites</span></span>
<span data-ttu-id="d2a35-110">Hämta och kör den [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d2a35-110">Download and run the [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-the-sample-app"></a><span data-ttu-id="d2a35-111">Skapa en Fragmentera kartan manager med sample-appen</span><span class="sxs-lookup"><span data-stu-id="d2a35-111">Create a shard map manager using the sample app</span></span>
<span data-ttu-id="d2a35-112">Här skapar du en karta Fragmentera manager tillsammans med flera delar, följt av infogning av data till shards.</span><span class="sxs-lookup"><span data-stu-id="d2a35-112">Here you will create a shard map manager along with several shards, followed by insertion of data into the shards.</span></span> <span data-ttu-id="d2a35-113">Om du redan har shards med shardade data i dem kan du hoppa över följande steg och flytta till nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d2a35-113">If you already have shards set up with sharded data in them, you can skip the following steps and move to the next section.</span></span>

1. <span data-ttu-id="d2a35-114">Skapa och köra den **komma igång med elastiska Databasverktyg** exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-114">Build and run the **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="d2a35-115">Följ stegen tills steg 7 i avsnittet [ladda ned och kör exempelappen](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="d2a35-115">Follow the steps until step 7 in the section [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="d2a35-116">I slutet av steg 7 visas följande kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="d2a35-116">At the end of Step 7, you will see the following command prompt:</span></span>

   ![kommandotolk](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. <span data-ttu-id="d2a35-118">I Kommandotolken, Skriv ”1” och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-118">In the command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="d2a35-119">Detta skapar Fragmentera kartan manager och lägger till två delar i server.</span><span class="sxs-lookup"><span data-stu-id="d2a35-119">This creates the shard map manager, and adds two shards to the server.</span></span> <span data-ttu-id="d2a35-120">Sedan skriver ”3” och tryck på **RETUR**; Upprepa åtgärden fyra gånger.</span><span class="sxs-lookup"><span data-stu-id="d2a35-120">Then type "3" and press **Enter**; repeat this action four times.</span></span> <span data-ttu-id="d2a35-121">Detta infogar exempel datarader i din shards.</span><span class="sxs-lookup"><span data-stu-id="d2a35-121">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="d2a35-122">Den [Azure Portal](https://portal.azure.com) tre nya databaser som ska visas:</span><span class="sxs-lookup"><span data-stu-id="d2a35-122">The [Azure Portal](https://portal.azure.com) should show three new databases:</span></span>

   ![Visual Studio-bekräftelse](./media/sql-database-elastic-query-getting-started/portal.png)

   <span data-ttu-id="d2a35-124">Nu skapar vi en anpassad databas-samling som visar alla databaser i kartan Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="d2a35-124">At this point, we will create a custom database collection that reflects all the databases in the shard map.</span></span> <span data-ttu-id="d2a35-125">Detta gör att vi kan skapa och köra ett jobb som lägger till en ny tabell över shards.</span><span class="sxs-lookup"><span data-stu-id="d2a35-125">This will allow us to create and execute a job that add a new table across shards.</span></span>

<span data-ttu-id="d2a35-126">Här kan vi skulle vanligtvis skapa en karta Fragmentera mål med hjälp av den **ny AzureSqlJobTarget** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-126">Here we would usually create a shard map target, using the **New-AzureSqlJobTarget** cmdlet.</span></span> <span data-ttu-id="d2a35-127">Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan kartan specifika Fragmentera har angetts som mål.</span><span class="sxs-lookup"><span data-stu-id="d2a35-127">The shard map manager database must be set as a database target and then the specific shard map is specified as a target.</span></span> <span data-ttu-id="d2a35-128">I stället det dags att räkna upp alla databaser på servern och lägga till databaserna till den nya anpassa samlingen med undantag för master-databasen.</span><span class="sxs-lookup"><span data-stu-id="d2a35-128">Instead, we are going to enumerate all the databases in the server and add the databases to the new custom collection with the exception of master database.</span></span>

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a><span data-ttu-id="d2a35-129">Skapar en anpassad samling och lägger till alla databaser på servern till målet med undantag för master anpassad samling.</span><span class="sxs-lookup"><span data-stu-id="d2a35-129">Creates a custom collection and add all databases in the server to the custom collection target with the exception of master.</span></span>
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
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="d2a35-130">Skapa ett T-SQL-skript för körning på databaser</span><span class="sxs-lookup"><span data-stu-id="d2a35-130">Create a T-SQL Script for execution across databases</span></span>
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

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a><span data-ttu-id="d2a35-131">Skapa jobb för att köra ett skript på den anpassade gruppen av databaser</span><span class="sxs-lookup"><span data-stu-id="d2a35-131">Create the job to execute a script across the custom group of databases</span></span>

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a><span data-ttu-id="d2a35-132">Kör jobbet</span><span class="sxs-lookup"><span data-stu-id="d2a35-132">Execute the job</span></span>
<span data-ttu-id="d2a35-133">Följande PowerShell-skript kan användas för att köra ett befintligt jobb:</span><span class="sxs-lookup"><span data-stu-id="d2a35-133">The following PowerShell script can be used to execute an existing job:</span></span>

<span data-ttu-id="d2a35-134">Uppdatera följande variabel för att återspegla önskade Jobbnamnet som har köras:</span><span class="sxs-lookup"><span data-stu-id="d2a35-134">Update the following variable to reflect the desired job name to have executed:</span></span>

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a><span data-ttu-id="d2a35-135">Hämta tillståndet för en enskild jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="d2a35-135">Retrieve the state of a single job execution</span></span>
<span data-ttu-id="d2a35-136">Använda samma **Get-AzureSqlJobExecution** med den **IncludeChildren** parametern för att visa status för underordnade jobbet körningar, nämligen ett visst tillstånd för varje jobbkörningen mot varje databas som mål för jobbet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-136">Use the same **Get-AzureSqlJobExecution** cmdlet with the **IncludeChildren** parameter to view the state of child job executions, namely the specific state for each job execution against each database targeted by the job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a><span data-ttu-id="d2a35-137">Visa status över flera jobb körningar</span><span class="sxs-lookup"><span data-stu-id="d2a35-137">View the state across multiple job executions</span></span>
<span data-ttu-id="d2a35-138">Den **Get-AzureSqlJobExecution** cmdlet har flera valfria parametrar som kan användas för att visa flera jobb körningar, filtreras via de angivna parametrarna.</span><span class="sxs-lookup"><span data-stu-id="d2a35-138">The **Get-AzureSqlJobExecution** cmdlet has multiple optional parameters that can be used to display multiple job executions, filtered through the provided parameters.</span></span> <span data-ttu-id="d2a35-139">Följande visar några av de möjliga sätt att använda Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="d2a35-139">The following demonstrates some of the possible ways to use Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="d2a35-140">Hämta alla aktiva översta nivå jobbet körningar:</span><span class="sxs-lookup"><span data-stu-id="d2a35-140">Retrieve all active top level job executions:</span></span>

   ```
    Get-AzureSqlJobExecution
   ```

<span data-ttu-id="d2a35-141">Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="d2a35-141">Retrieve all top level job executions, including inactive job executions:</span></span>

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

<span data-ttu-id="d2a35-142">Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="d2a35-142">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

<span data-ttu-id="d2a35-143">Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="d2a35-143">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

<span data-ttu-id="d2a35-144">Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="d2a35-144">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="d2a35-145">Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="d2a35-145">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="d2a35-146">Hämta listan över jobb uppgiften körningar inom en specifik jobbkörningen:</span><span class="sxs-lookup"><span data-stu-id="d2a35-146">Retrieve the list of job task executions within a specific job execution:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

<span data-ttu-id="d2a35-147">Hämta jobb aktivitetsinformation körning:</span><span class="sxs-lookup"><span data-stu-id="d2a35-147">Retrieve job task execution details:</span></span>

<span data-ttu-id="d2a35-148">Följande PowerShell-skript kan användas för att visa information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.</span><span class="sxs-lookup"><span data-stu-id="d2a35-148">The following PowerShell script can be used to view the details of a job task execution, which is particularly useful when debugging execution failures.</span></span>
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a><span data-ttu-id="d2a35-149">Hämta fel i jobb uppgiften körningar</span><span class="sxs-lookup"><span data-stu-id="d2a35-149">Retrieve failures within job task executions</span></span>
<span data-ttu-id="d2a35-150">Objektet JobTaskExecution innehåller en egenskap för livscykeln för uppgiften tillsammans med en meddelandeegenskap.</span><span class="sxs-lookup"><span data-stu-id="d2a35-150">The JobTaskExecution object includes a property for the Lifecycle of the task along with a Message property.</span></span> <span data-ttu-id="d2a35-151">Om ett jobb för körning av aktiviteten misslyckades livscykel egenskap anges *misslyckades* och meddelandeegenskapen kommer att anges till det resulterande Undantagsmeddelandet och dess stacken.</span><span class="sxs-lookup"><span data-stu-id="d2a35-151">If a job task execution failed, the Lifecycle property will be set to *Failed* and the Message property will be set to the resulting exception message and its stack.</span></span> <span data-ttu-id="d2a35-152">Om ett jobb misslyckades, är det viktigt att visa information om jobbuppgifter som misslyckades för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-152">If a job did not succeed, it is important to view the details of job tasks that did not succeed for a given job.</span></span>

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

## <a name="waiting-for-a-job-execution-to-complete"></a><span data-ttu-id="d2a35-153">Väntar på att ett jobbkörning ska slutföras</span><span class="sxs-lookup"><span data-stu-id="d2a35-153">Waiting for a job execution to complete</span></span>
<span data-ttu-id="d2a35-154">Följande PowerShell-skript kan användas för att vänta på en projektaktivitet att slutföra:</span><span class="sxs-lookup"><span data-stu-id="d2a35-154">The following PowerShell script can be used to wait for a job task to complete:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="d2a35-155">Skapa en princip för anpassad körning</span><span class="sxs-lookup"><span data-stu-id="d2a35-155">Create a custom execution policy</span></span>
<span data-ttu-id="d2a35-156">Den elastiska databasen jobb kan du skapa anpassade körningsprinciper som kan användas när du startar jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-156">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="d2a35-157">Körningsprinciper tillåter för närvarande för att definiera:</span><span class="sxs-lookup"><span data-stu-id="d2a35-157">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="d2a35-158">Namn: Identifierare för körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="d2a35-158">Name: Identifier for the execution policy.</span></span>
* <span data-ttu-id="d2a35-159">Tidsgräns för jobb: Total tid innan ett jobb kommer att avbrytas av elastiska databasen jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-159">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="d2a35-160">Inledande återförsöksintervall: Intervall ska vänta innan nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="d2a35-160">Initial Retry Interval: Interval to wait before first retry.</span></span>
* <span data-ttu-id="d2a35-161">Maximal återförsöksintervall: Locket återförsöksintervall ska användas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-161">Maximum Retry Interval: Cap of retry intervals to use.</span></span>
* <span data-ttu-id="d2a35-162">Försök intervall Backoff värde: Värde används för att beräkna nästa intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="d2a35-162">Retry Interval Backoff Coefficient: Coefficient used to calculate the next interval between retries.</span></span>  <span data-ttu-id="d2a35-163">Följande formel används: (första försök intervall) * Math.pow ((intervall Backoff värde) (antal nya försök) - 2).</span><span class="sxs-lookup"><span data-stu-id="d2a35-163">The following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span>
* <span data-ttu-id="d2a35-164">Maximalt antal försök: Maximalt antal försök försöker utföra inom ett jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-164">Maximum Attempts: The maximum number of retry attempts to perform within a job.</span></span>

<span data-ttu-id="d2a35-165">Standard-körningsprincipen används följande värden:</span><span class="sxs-lookup"><span data-stu-id="d2a35-165">The default execution policy uses the following values:</span></span>

* <span data-ttu-id="d2a35-166">Namn: Standardprincipen för körning</span><span class="sxs-lookup"><span data-stu-id="d2a35-166">Name: Default execution policy</span></span>
* <span data-ttu-id="d2a35-167">Tidsgräns för jobb: 1 vecka</span><span class="sxs-lookup"><span data-stu-id="d2a35-167">Job Timeout: 1 week</span></span>
* <span data-ttu-id="d2a35-168">Inledande återförsöksintervall: 100 millisekunder</span><span class="sxs-lookup"><span data-stu-id="d2a35-168">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="d2a35-169">Maximal återförsöksintervall: 30 minuter</span><span class="sxs-lookup"><span data-stu-id="d2a35-169">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="d2a35-170">Försök intervall värde: 2</span><span class="sxs-lookup"><span data-stu-id="d2a35-170">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="d2a35-171">Maximalt antal försök: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="d2a35-171">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="d2a35-172">Skapa önskade körningsprincipen:</span><span class="sxs-lookup"><span data-stu-id="d2a35-172">Create the desired execution policy:</span></span>

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

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="d2a35-173">Uppdatera en anpassad körningsprincip</span><span class="sxs-lookup"><span data-stu-id="d2a35-173">Update a custom execution policy</span></span>
<span data-ttu-id="d2a35-174">Uppdatera önskade körningsprincipen att uppdatera:</span><span class="sxs-lookup"><span data-stu-id="d2a35-174">Update the desired execution policy to update:</span></span>

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

## <a name="cancel-a-job"></a><span data-ttu-id="d2a35-175">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="d2a35-175">Cancel a job</span></span>
<span data-ttu-id="d2a35-176">Den elastiska databasen jobb stöder jobb annullering begäranden.</span><span class="sxs-lookup"><span data-stu-id="d2a35-176">Elastic Database Jobs supports jobs cancellation requests.</span></span>  <span data-ttu-id="d2a35-177">Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den att stoppa jobbet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-177">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt to stop the job.</span></span>

<span data-ttu-id="d2a35-178">Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:</span><span class="sxs-lookup"><span data-stu-id="d2a35-178">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="d2a35-179">Avbryta pågående aktiviteter: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom körs aspekt av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="d2a35-179">Canceling Currently Executing Tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within the currently executing aspect of the task.</span></span>  <span data-ttu-id="d2a35-180">Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är ett försök att avbryta frågan.</span><span class="sxs-lookup"><span data-stu-id="d2a35-180">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt to cancel the query.</span></span>
2. <span data-ttu-id="d2a35-181">Annullering återförsök för aktiviteten: Om en annullering identifieras av kontrollen tråd innan en uppgift startas för körning kontrollen tråden ska undvika att starta uppgiften och deklarera begäran som avbruten.</span><span class="sxs-lookup"><span data-stu-id="d2a35-181">Canceling Task Retries: If a cancellation is detected by the control thread before a task is launched for execution, the control thread will avoid launching the task and declare the request as canceled.</span></span>

<span data-ttu-id="d2a35-182">Om det krävs en annullering av jobbet för överordnade jobb respekteras begäran om att avbryta för överordnade jobb och alla dess underordnade jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-182">If a job cancellation is requested for a parent job, the cancellation request will be honored for the parent job and for all of its child jobs.</span></span>

<span data-ttu-id="d2a35-183">För att skicka en begäran om att avbryta, Använd den **stoppa AzureSqlJobExecution** cmdlet och ange den **JobExecutionId** parameter.</span><span class="sxs-lookup"><span data-stu-id="d2a35-183">To submit a cancellation request, use the **Stop-AzureSqlJobExecution** cmdlet and set the **JobExecutionId** parameter.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a><span data-ttu-id="d2a35-184">Ta bort ett jobb efter namn och den jobbhistorik</span><span class="sxs-lookup"><span data-stu-id="d2a35-184">Delete a job by name and the job's history</span></span>
<span data-ttu-id="d2a35-185">Den elastiska databasen jobb stöder asynkrona borttagning av jobb.</span><span class="sxs-lookup"><span data-stu-id="d2a35-185">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="d2a35-186">Ett jobb kan vara markerad för borttagning och tas bort jobbet och alla dess jobbhistorik när alla jobb körningar har slutförts för jobbet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-186">A job can be marked for deletion and the system will delete the job and all its job history after all job executions have completed for the job.</span></span> <span data-ttu-id="d2a35-187">Systemet Avbryt inte automatiskt aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="d2a35-187">The system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="d2a35-188">Stoppa AzureSqlJobExecution måste i stället anropas om du vill avbryta aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="d2a35-188">Instead, Stop-AzureSqlJobExecution must be invoked to cancel active job executions.</span></span>

<span data-ttu-id="d2a35-189">Utlös borttagning av jobbet genom att använda den **ta bort AzureSqlJob** cmdlet och ange den **jobbnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="d2a35-189">To trigger job deletion, use the **Remove-AzureSqlJob** cmdlet and set the **JobName** parameter.</span></span>

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a><span data-ttu-id="d2a35-190">Skapa en anpassad databas mål</span><span class="sxs-lookup"><span data-stu-id="d2a35-190">Create a custom database target</span></span>
<span data-ttu-id="d2a35-191">Anpassad databas mål kan definieras i jobb för elastisk databas som kan användas för att köras direkt eller som ska ingå i en anpassad databas-grupp.</span><span class="sxs-lookup"><span data-stu-id="d2a35-191">Custom database targets can be defined in Elastic Database jobs which can be used either for execution directly or for inclusion within a custom database group.</span></span> <span data-ttu-id="d2a35-192">Eftersom **elastiska pooler** ännu direkt stöds inte via PowerShell-APIs du helt enkelt skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla databaser i poolen.</span><span class="sxs-lookup"><span data-stu-id="d2a35-192">Since **elastic pools** are not yet directly supported via the PowerShell APIs, you simply create a custom database target and custom database collection target which encompasses all the databases in the pool.</span></span>

<span data-ttu-id="d2a35-193">Ange följande variabler återspeglar den önskade databasinformationen:</span><span class="sxs-lookup"><span data-stu-id="d2a35-193">Set the following variables to reflect the desired database information:</span></span>

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a><span data-ttu-id="d2a35-194">Skapa en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="d2a35-194">Create a custom database collection target</span></span>
<span data-ttu-id="d2a35-195">En anpassad databas samling mål kan definieras för att möjliggöra körning över flera definierade databasen mål.</span><span class="sxs-lookup"><span data-stu-id="d2a35-195">A custom database collection target can be defined to enable execution across multiple defined database targets.</span></span> <span data-ttu-id="d2a35-196">När du har skapat en databasgrupp kan databaser vara kopplad till anpassade samlingar målet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-196">After a database group is created, databases can be associated to the custom collection target.</span></span>

<span data-ttu-id="d2a35-197">Ange följande variabler önskade anpassade samlingar mål konfiguration:</span><span class="sxs-lookup"><span data-stu-id="d2a35-197">Set the following variables to reflect the desired custom collection target configuration:</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a><span data-ttu-id="d2a35-198">Lägga till databaser till en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="d2a35-198">Add databases to a custom database collection target</span></span>
<span data-ttu-id="d2a35-199">Databasen mål kan vara associerat med anpassad databas samling mål för att skapa en grupp databaser.</span><span class="sxs-lookup"><span data-stu-id="d2a35-199">Database targets can be associated with custom database collection targets to create a group of databases.</span></span> <span data-ttu-id="d2a35-200">När ett jobb skapas som en anpassad databas samling mål, kommer att expanderas om du vill anpassa databaserna som är associerade med gruppen vid tidpunkten för körning.</span><span class="sxs-lookup"><span data-stu-id="d2a35-200">Whenever a job is created that targets a custom database collection target, it will be expanded to target the databases associated to the group at the time of execution.</span></span>

<span data-ttu-id="d2a35-201">Lägg till rätt databas i en specifik egen samling:</span><span class="sxs-lookup"><span data-stu-id="d2a35-201">Add the desired database to a specific custom collection:</span></span>

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="d2a35-202">Granska databaser i en anpassad databas samling målet</span><span class="sxs-lookup"><span data-stu-id="d2a35-202">Review the databases within a custom database collection target</span></span>
<span data-ttu-id="d2a35-203">Använd den **Get-AzureSqlJobTarget** för att hämta underordnade databaser i en anpassad databas samling målet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-203">Use the **Get-AzureSqlJobTarget** cmdlet to retrieve the child databases within a custom database collection target.</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="d2a35-204">Skapa ett jobb för att köra ett skript i en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="d2a35-204">Create a job to execute a script across a custom database collection target</span></span>
<span data-ttu-id="d2a35-205">Använd den **ny AzureSqlJob** för att skapa ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål.</span><span class="sxs-lookup"><span data-stu-id="d2a35-205">Use the **New-AzureSqlJob** cmdlet to create a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="d2a35-206">Elastiska databasen jobb kommer Expandera jobbet till flera underordnade jobb varje motsvarar en databas som är associerade med samlingen målet anpassad databas och se till att skriptet körs mot varje databas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-206">Elastic Database jobs will expand the job into multiple child jobs each corresponding to a database associated with the custom database collection target and ensure that the script is executed against each database.</span></span> <span data-ttu-id="d2a35-207">Igen, är det viktigt att skripten har idempotent för att hantera att återförsök.</span><span class="sxs-lookup"><span data-stu-id="d2a35-207">Again, it is important that scripts are idempotent to be resilient to retries.</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a><span data-ttu-id="d2a35-208">Insamling av data över databaser</span><span class="sxs-lookup"><span data-stu-id="d2a35-208">Data collection across databases</span></span>
<span data-ttu-id="d2a35-209">**Den elastiska databasen jobb** stöder köra en fråga i en grupp med databaser och skickar resultatet till en angiven databastabell.</span><span class="sxs-lookup"><span data-stu-id="d2a35-209">**Elastic Database jobs** supports executing a query across a group of databases and sends the results to a specified database’s table.</span></span> <span data-ttu-id="d2a35-210">Tabellen kan efterfrågas efter faktumet att se resultatet av frågan från varje databas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-210">The table can be queried after the fact to see the query’s results from each database.</span></span> <span data-ttu-id="d2a35-211">Detta ger en asynkron metod för att köra en fråga över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="d2a35-211">This provides an asynchronous mechanism to execute a query across many databases.</span></span> <span data-ttu-id="d2a35-212">Fel fall, till exempel en av databaserna som för tillfället otillgänglig hanteras automatiskt via återförsök.</span><span class="sxs-lookup"><span data-stu-id="d2a35-212">Failure cases such as one of the databases being temporarily unavailable are handled automatically via retries.</span></span>

<span data-ttu-id="d2a35-213">Den angivna tabellen skapas automatiskt om den inte ännu finns, matchar schemat för den returnerade resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="d2a35-213">The specified destination table will be automatically created if it does not yet exist, matching the schema of the returned result set.</span></span> <span data-ttu-id="d2a35-214">Om en skriptkörningen returnerar flera resultatmängder skickas bara det första till den angivna måltabellen jobb för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-214">If a script execution returns multiple result sets, Elastic Database jobs will only send the first one to the provided destination table.</span></span>

<span data-ttu-id="d2a35-215">Följande PowerShell-skript kan användas för att köra ett skript som samlar in resultaten till en angiven tabell.</span><span class="sxs-lookup"><span data-stu-id="d2a35-215">The following PowerShell script can be used to execute a script collecting its results into a specified table.</span></span> <span data-ttu-id="d2a35-216">Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2a35-216">This script assumes that a T-SQL script has been created which outputs a single result set and a custom database collection target has been created.</span></span>

<span data-ttu-id="d2a35-217">Ange följande för att återspegla önskade skript, autentiseringsuppgifter och mål för körning:</span><span class="sxs-lookup"><span data-stu-id="d2a35-217">Set the following to reflect the desired script, credentials, and execution target:</span></span>

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="d2a35-218">Skapa och starta ett jobb för scenarion för insamling av data</span><span class="sxs-lookup"><span data-stu-id="d2a35-218">Create and start a job for data collection scenarios</span></span>
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a><span data-ttu-id="d2a35-219">Skapa ett schema för jobbkörningen med en utlösare för jobb</span><span class="sxs-lookup"><span data-stu-id="d2a35-219">Create a schedule for job execution using a job trigger</span></span>
<span data-ttu-id="d2a35-220">Följande PowerShell-skript kan användas för att skapa ett reoccurring schema.</span><span class="sxs-lookup"><span data-stu-id="d2a35-220">The following PowerShell script can be used to create a reoccurring schedule.</span></span> <span data-ttu-id="d2a35-221">Det här skriptet använder en minuts intervall, men ny AzureSqlJobSchedule stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar.</span><span class="sxs-lookup"><span data-stu-id="d2a35-221">This script uses a one minute interval, but New-AzureSqlJobSchedule also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="d2a35-222">Scheman som kör en gång kan skapas med skicka - görs.</span><span class="sxs-lookup"><span data-stu-id="d2a35-222">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="d2a35-223">Skapa ett nytt schema:</span><span class="sxs-lookup"><span data-stu-id="d2a35-223">Create a new schedule:</span></span>
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="d2a35-224">Skapa en utlösare för jobbet om du vill att ett jobb som körs på en tidsplan</span><span class="sxs-lookup"><span data-stu-id="d2a35-224">Create a job trigger to have a job executed on a time schedule</span></span>
<span data-ttu-id="d2a35-225">En utlösare för jobbet kan definieras om du vill att ett jobb som körs enligt ett schema med tiden.</span><span class="sxs-lookup"><span data-stu-id="d2a35-225">A job trigger can be defined to have a job executed according to a time schedule.</span></span> <span data-ttu-id="d2a35-226">Följande PowerShell-skript kan användas för att skapa en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-226">The following PowerShell script can be used to create a job trigger.</span></span>

<span data-ttu-id="d2a35-227">Ange följande variabler som motsvarar den önskade jobbet och schema:</span><span class="sxs-lookup"><span data-stu-id="d2a35-227">Set the following variables to correspond to the desired job and schedule:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a><span data-ttu-id="d2a35-228">Ta bort en schemalagd association att avbryta jobbet körs enligt schema</span><span class="sxs-lookup"><span data-stu-id="d2a35-228">Remove a scheduled association to stop job from executing on schedule</span></span>
<span data-ttu-id="d2a35-229">Jobbet utlösaren kan tas bort för att avbryta igen jobbkörningen via en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-229">To discontinue reoccurring job execution through a job trigger, the job trigger can be removed.</span></span>
<span data-ttu-id="d2a35-230">Ta bort utlösaren jobbet om du vill stoppa ett jobb från utförs enligt ett schema som använder den **ta bort AzureSqlJobTrigger** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-230">Remove a job trigger to stop a job from being executed according to a schedule using the **Remove-AzureSqlJobTrigger** cmdlet.</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a><span data-ttu-id="d2a35-231">Importera elastisk databas frågeresultaten till Excel</span><span class="sxs-lookup"><span data-stu-id="d2a35-231">Import elastic database query results to Excel</span></span>
 <span data-ttu-id="d2a35-232">Du kan importera resultatet av en fråga till en Excel-fil.</span><span class="sxs-lookup"><span data-stu-id="d2a35-232">You can import the results from of a query to an Excel file.</span></span>

1. <span data-ttu-id="d2a35-233">Starta Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="d2a35-233">Launch Excel 2013.</span></span>
2. <span data-ttu-id="d2a35-234">Navigera till den **Data** menyfliksområdet.</span><span class="sxs-lookup"><span data-stu-id="d2a35-234">Navigate to the **Data** ribbon.</span></span>
3. <span data-ttu-id="d2a35-235">Klicka på **från andra källor** och på **från SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-235">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel-import från andra källor](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. <span data-ttu-id="d2a35-237">I den **Dataanslutningsguiden** skriver server servernamn och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d2a35-237">In the **Data Connection Wizard** type the server name and login credentials.</span></span> <span data-ttu-id="d2a35-238">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-238">Then click **Next**.</span></span>
5. <span data-ttu-id="d2a35-239">I dialogrutan **Markera databasen som innehåller de data som du vill**, Välj den **ElasticDBQuery** databas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-239">In the dialog box **Select the database that contains the data you want**, select the **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="d2a35-240">Välj den **kunder** tabell i listan och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-240">Select the **Customers** table in the list view and click **Next**.</span></span> <span data-ttu-id="d2a35-241">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-241">Then click **Finish**.</span></span>
7. <span data-ttu-id="d2a35-242">I den **importera Data** formuläret under **Välj hur du vill visa data i arbetsboken**väljer **tabell** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2a35-242">In the **Import Data** form, under **Select how you want to view this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="d2a35-243">Alla rader från **kunder** tabell som sparas i olika delar fylla i Excel-blad.</span><span class="sxs-lookup"><span data-stu-id="d2a35-243">All the rows from **Customers** table, stored in different shards populate the Excel sheet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2a35-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2a35-244">Next steps</span></span>
<span data-ttu-id="d2a35-245">Du kan nu använda Excel-data.</span><span class="sxs-lookup"><span data-stu-id="d2a35-245">You can now use Excel’s data functions.</span></span> <span data-ttu-id="d2a35-246">Använda anslutningssträngen med servernamnet, databasnamnet och autentiseringsuppgifter för att ansluta din verktyg för BI och integrering till elastisk fråga databas.</span><span class="sxs-lookup"><span data-stu-id="d2a35-246">Use the connection string with your server name, database name and credentials to connect your BI and data integration tools to the elastic query database.</span></span> <span data-ttu-id="d2a35-247">Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver.</span><span class="sxs-lookup"><span data-stu-id="d2a35-247">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="d2a35-248">Referera till elastisk fråga databas och externa tabeller precis som andra SQL Server-databas och SQL Server-tabeller som du vill ansluta till med din-verktyget.</span><span class="sxs-lookup"><span data-stu-id="d2a35-248">Refer to the elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect to with your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="d2a35-249">Kostnad</span><span class="sxs-lookup"><span data-stu-id="d2a35-249">Cost</span></span>
<span data-ttu-id="d2a35-250">Det finns utan extra kostnad för att använda funktionen för elastisk databas frågan.</span><span class="sxs-lookup"><span data-stu-id="d2a35-250">There is no additional charge for using the Elastic Database query feature.</span></span> <span data-ttu-id="d2a35-251">Dock just nu den här funktionen är bara tillgängliga på premiumdatabaser som en slutpunkt, men delar som kan vara av en tjänstnivå.</span><span class="sxs-lookup"><span data-stu-id="d2a35-251">However, at this time this feature is available only on premium databases as an end point, but the shards can be of any service tier.</span></span>

<span data-ttu-id="d2a35-252">Mer information om priser finns [prisinformation för SQL-databasen](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="d2a35-252">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
