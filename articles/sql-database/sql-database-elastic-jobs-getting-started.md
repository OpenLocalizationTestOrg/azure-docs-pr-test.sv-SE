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
# <a name="getting-started-with-elastic-database-jobs"></a>Komma igång med jobb för elastisk databas
Den elastiska databasen jobb (förhandsversion) för Azure SQL Database kan du tooreliability köra T-SQL-skript som sträcker sig över flera databaser när du försöker automatiskt och garanterar att tillhandahålla eventuell slutförande. Mer information om hello elastiska jobb databasfunktion finns hello [översikt funktionssidan](sql-database-elastic-jobs-overview.md).

Det här avsnittet utökar hello-exempel finns i [komma igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md). När du är klar, kommer: Lär dig hur toocreate och hantera jobb som hanterar en grupp av relaterade databaser. Det är inte obligatoriska toouse hello elastisk skalbarhet verktyg i ordning tootake nytta av hello fördelarna med elastiska jobb.

## <a name="prerequisites"></a>Krav
Hämta och köra hello [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>Skapa en Fragmentera karta manager med hello sample-appen
Här skapar du en karta Fragmentera manager tillsammans med flera delar, följt av infogning av data i hello delar. Om du redan har shards med shardade data i dem kan du hoppa över hello följande steg och flytta toohello nästa avsnitt.

1. Skapa och köra hello **komma igång med elastiska Databasverktyg** exempelprogrammet. Gör hello förrän steg 7 i hello avsnittet [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Hello slutet av steg 7 visas hello följande kommandotolk:

   ![kommandotolk](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. Skriv ”1” i hello-kommandotolk och tryck på **RETUR**. Detta skapar hello Fragmentera kartan manager och lägger till två delar toohello server. Sedan skriver ”3” och tryck på **RETUR**; Upprepa åtgärden fyra gånger. Detta infogar exempel datarader i din shards.
3. Hej [Azure Portal](https://portal.azure.com) tre nya databaser som ska visas:

   ![Visual Studio-bekräftelse](./media/sql-database-elastic-query-getting-started/portal.png)

   Nu skapar vi en anpassad databas-samling som visar alla hello databaser i hello Fragmentera kartan. Detta innebär att vi toocreate och köra ett jobb som lägger till en ny tabell över shards.

Här vi skulle vanligtvis skapa en karta mål Fragmentera med hello **ny AzureSqlJobTarget** cmdlet. hello Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan hello specifika Fragmentera mappning har angetts som mål. I stället vi kommer tooenumerate alla hello databaser i hello server och Lägg till hello databaser toohello nya anpassade samlingar med hello undantag av master-databasen.

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a>Skapar en anpassad samling och lägger till alla databaser i hello server toohello anpassad samling mål med hello undantag av master.
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
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Skapa ett T-SQL-skript för körning på databaser
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

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a>Skapa ett skript för hello jobbet tooexecute över hello anpassad grupp av databaser

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a>Köra hello jobb
hello följande PowerShell-skript kan vara används tooexecute ett befintligt jobb:

Uppdatera hello variabeln tooreflect hello önskad jobbet namn toohave utförs följande:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a>Hämta hello tillståndet för en enskild jobbkörningen
Använd hello samma **Get-AzureSqlJobExecution** med hello **IncludeChildren** parametern tooview hello tillstånd för underordnad jobbet körningar, nämligen hello specifikt tillstånd för varje jobbkörningen mot varje databasen som mål för hello jobb.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a>Visa status för hello över flera jobb körningar
Hej **Get-AzureSqlJobExecution** cmdlet har flera valfria parametrar som kan använda toodisplay flera jobb körningar, filtreras via hello angivna parametrar. hello nedan visar några av hello möjliga sätt toouse Get-AzureSqlJobExecution:

Hämta alla aktiva översta nivå jobbet körningar:

   ```
    Get-AzureSqlJobExecution
   ```

Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Hämta hello lista över jobb uppgiften körningar inom en specifik jobbkörningen:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Hämta jobb aktivitetsinformation körning:

hello följande PowerShell-skript kan vara används tooview hello information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Hämta fel i jobb uppgiften körningar
Hej JobTaskExecution objektet innehåller en egenskap för hello livscykeln för hello uppgiften tillsammans med en meddelandeegenskap. Om ett jobb för körning av aktiviteten misslyckades hello livscykel egenskapen ställs in för*misslyckades* och kommer att anges hello meddelandeegenskap toohello resulterande Undantagsmeddelande och dess stacken. Om ett jobb misslyckades, är det viktigt tooview hello information om jobbuppgifter som misslyckades för ett visst jobb.

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

## <a name="waiting-for-a-job-execution-toocomplete"></a>Väntar på att en toocomplete för körning av jobbet
hello följande PowerShell-skript kan vara används toowait för ett jobb uppgiften toocomplete:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Skapa en princip för anpassad körning
Den elastiska databasen jobb kan du skapa anpassade körningsprinciper som kan användas när du startar jobb.

Körningsprinciper tillåter för närvarande för att definiera:

* Namn: Identifierare för hello körningsprincipen.
* Tidsgräns för jobb: Total tid innan ett jobb kommer att avbrytas av elastiska databasen jobb.
* Inledande återförsöksintervall: Intervall toowait innan nytt försök görs.
* Maximal återförsöksintervall: Locket försök intervall toouse.
* Försök intervall Backoff värde: Värde används toocalculate hello nästa intervall mellan försök.  hello används följande formel: (första försök intervall) * Math.pow ((intervall Backoff värde) (antal nya försök) - 2).
* Maximalt antal försök: hello maximalt antal försök försök tooperform inom ett jobb.

hello standard körningsprincipen använder hello följande värden:

* Namn: Standardprincipen för körning
* Tidsgräns för jobb: 1 vecka
* Inledande återförsöksintervall: 100 millisekunder
* Maximal återförsöksintervall: 30 minuter
* Försök intervall värde: 2
* Maximalt antal försök: 2 147 483 647

Skapa hello önskad körningsprincipen:

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

### <a name="update-a-custom-execution-policy"></a>Uppdatera en anpassad körningsprincip
Uppdatera hello önskad körning princip tooupdate:

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

## <a name="cancel-a-job"></a>Avbryta ett jobb
Den elastiska databasen jobb stöder jobb annullering begäranden.  Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den toostop hello jobb.

Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:

1. Avbryta pågående aktiviteter: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom hello aspekt av hello aktivitet som körs.  Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är en försök toocancel hello-fråga.
2. Annullering återförsök för aktiviteten: Om en annullering identifieras av hello kontrollen tråd innan en uppgift startas för körning hello kontrollen tråd undvika starta hello aktivitet och deklarera hello begäran som avbruten.

Om det krävs en annullering av jobbet för överordnade jobb respekteras hello avbrottsbegäran för hello överordnade jobb och alla dess underordnade jobb.

toosubmit en begäran om att avbryta, använda hello **stoppa AzureSqlJobExecution** cmdlet och ange hello **JobExecutionId** parameter.

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a>Ta bort ett jobb efter namn och hello jobbhistorik
Den elastiska databasen jobb stöder asynkrona borttagning av jobb. Ett jobb kan vara markerad för borttagning och hello tas bort hello jobb och alla dess jobbhistorik när alla jobb körningar har slutförts för hello jobb. hello system Avbryt inte automatiskt aktiva jobb körningar.  

Stoppa AzureSqlJobExecution måste i stället vara anropade toocancel aktiva jobb körningar.

tootrigger jobbet borttagning, Använd hello **ta bort AzureSqlJob** cmdlet och ange hello **jobbnamn** parameter.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Skapa en anpassad databas mål
Anpassad databas mål kan definieras i jobb för elastisk databas som kan användas för att köras direkt eller som ska ingå i en anpassad databas-grupp. Eftersom **elastiska pooler** ännu direkt stöds inte via hello PowerShell APIs du helt enkelt skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla hello-databaser i hello pool.

Ange följande variabler tooreflect hello önskad databasinformation hello:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Skapa en anpassad databas samling mål
En anpassad databas samling mål kan vara definierade tooenable körning över flera definierade databasen mål. När du har skapat en databasgrupp kan databaser vara associerade toohello anpassad samling mål.

Ange hello följande variabler tooreflect hello önskade anpassade samlingar mål konfiguration:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a>Lägg till databaser tooa anpassad databas samling mål
Databasen mål kan vara associerat med anpassad databas samling mål toocreate en grupp databaser. När ett jobb skapas som en anpassad databas samling mål, blir det utökade tootarget hello databaserna associerade toohello grupp på hello tiden för körningen.

Lägg till hello önskad databasen tooa specifika anpassade samlingar:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Granska hello databaser i en anpassad databas samling målet
Använd hello **Get-AzureSqlJobTarget** cmdlet tooretrieve hello underordnade databaser i en anpassad databas samling målet.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Skapa ett skript i ett jobb tooexecute över en anpassad databas samling mål
Använd hello **ny AzureSqlJob** cmdlet toocreate ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål. Den elastiska databasen jobb kommer att expandera hello jobbet till flera underordnade jobb varje motsvarande tooa-databas som är associerade med hello anpassad databas samling mål och se till att hello skriptet körs mot varje databas. Igen, är det viktigt att skripten är idempotent toobe flexibel tooretries.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Insamling av data över databaser
**Den elastiska databasen jobb** stöder köra en fråga i en grupp med databaser och skickar hello-resultat-tooa angivna database-tabellen. hello tabellen kan efterfrågas efter hello fakta toosee hello frågans resultat från varje databas. Detta ger en mekanism för asynkron tooexecute en fråga över flera databaser. Fel fall, till exempel hello databaserna tillfälligt otillgänglig hanteras automatiskt via återförsök.

hello angivna måltabellen skapas automatiskt om det inte finns ännu, matchande hello schemat för hello returnerade en resultatuppsättning. Om en skriptkörningen returnerar flera resultatmängder, skickas endast elastisk databas jobb hello första måltabellen för en toohello som tillhandahålls.

hello följande PowerShell-skript kan vara används tooexecute ett skript som samlar in resultaten till en angiven tabell. Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.

Ange hello följande tooreflect hello önskad skript, autentiseringsuppgifter och körning av mål:

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Skapa och starta ett jobb för scenarion för insamling av data
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Skapa ett schema för jobbkörningen med en utlösare för jobb
hello följande PowerShell-skript kan vara används toocreate reoccurring schema. Det här skriptet använder en minuts intervall, men ny AzureSqlJobSchedule stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar. Scheman som kör en gång kan skapas med skicka - görs.

Skapa ett nytt schema:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a>Skapa ett jobb utlösaren toohave ett jobb som körs på en tidsplan
En utlösare för jobb kan vara definierade toohave ett jobb körs bl.a tooa tid schema. hello följande PowerShell-skript kan vara används toocreate en utlösare för jobbet.

Ange hello följande variabler toocorrespond toohello önskade jobb och schemalägga:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>Ta bort en schemalagd association toostop jobbet körs enligt schema
toodiscontinue igen jobbkörningen via jobb utlösare hello jobbet utlösare kan tas bort.
Ta bort ett jobb utlösaren toostop ett jobb från utförs bl.a tooa schema med hello **ta bort AzureSqlJobTrigger** cmdlet.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a>Importera elastisk databas frågan resultat tooExcel
 Du kan importera hello resultaten från av en fråga tooan Excel-fil.

1. Starta Excel 2013.
2. Navigera toohello **Data** menyfliksområdet.
3. Klicka på **från andra källor** och på **från SQL Server**.

   ![Excel-import från andra källor](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. I hello **Dataanslutningsguiden** skriver hello server namnet och autentiseringsuppgifterna för inloggning. Klicka sedan på **Nästa**.
5. I dialogrutan för hello **väljer hello-databas som innehåller hello data som du vill**väljer hello **ElasticDBQuery** databas.
6. Välj hello **kunder** tabell i hello listvyn och klicka på **nästa**. Klicka på **Slutför**.
7. I hello **importera Data** formuläret under **väljer du hur tooview dessa data i arbetsboken**väljer **tabell** och på **OK**.

Alla hello rader från **kunder** tabell som sparas i olika delar fylla hello Excel-blad.

## <a name="next-steps"></a>Nästa steg
Du kan nu använda Excel-data. Använd hello anslutningssträngen med servernamnet, databasnamnet och autentiseringsuppgifter tooconnect BI och data integration verktyg toohello elastisk fråga databasen. Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver. Läs toohello elastisk fråga databas och externa tabeller precis som andra SQL Server-databas och att du vill ansluta toowith verktyget du SQL Server-tabeller.

### <a name="cost"></a>Kostnad
Det finns utan extra kostnad för funktionen hello elastisk databas frågan. Dock just nu den här funktionen är bara tillgängliga på premiumdatabaser som en slutpunkt men hello shards kan vara av en tjänstnivå.

Mer information om priser finns [prisinformation för SQL-databasen](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
