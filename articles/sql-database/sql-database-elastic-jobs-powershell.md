---
title: "aaaCreate och hantera elastiska jobb med hjälp av PowerShell | Microsoft Docs"
description: "PowerShell används toomanage Azure SQL Database-pooler"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a>Skapa och hantera SQL Database: elastiska jobb med hjälp av PowerShell (förhandsgranskning)

hello PowerShell APIs för **elastisk databas jobb** (under förhandsgranskning) gör att du kan definiera en grupp databaser mot vilken skript körs. Den här artikeln visar hur toocreate och hantera **elastisk databas jobb** med PowerShell-cmdlets. Se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* En uppsättning databaser som skapats med hello elastisk Databasverktyg. Se [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
* **Den elastiska databasen jobb** PowerShell paket: finns [jobb installerar elastisk databas](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Välj din Azure-prenumeration
tooselect hello prenumeration som du behöver ditt prenumerations-Id (**- SubscriptionId**) eller prenumerationsnamn (**- SubscriptionName**). Om du har flera prenumerationer kan du köra hello **Get-AzureRmSubscription** cmdlet och kopiera hello önskad prenumerationsinformation hello resultatmängden. När du har din prenumerationsinformation kör följande cmdlet tooset hello prenumerationen som hello standard, nämligen hello mål för att skapa och hantera jobb:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Hej [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) rekommenderas för användning toodevelop och köra PowerShell-skript mot hello elastisk databas jobb.

## <a name="elastic-database-jobs-objects"></a>Elastiska jobb databasobjekt
Hej följande tabell visar ut alla hello objekttyper för **elastisk databas jobb** tillsammans med dess beskrivning och relevanta PowerShell APIs.

<table style="width:100%">
  <tr>
    <th>Objekttyp</th>
    <th>Beskrivning</th>
    <th>Relaterade PowerShell API: er</th>
  </tr>
  <tr>
    <td>Autentiseringsuppgift</td>
    <td>Användarnamn och lösenord toouse när du ansluter toodatabases för körning av skript eller ett program av DACPACs. <p>hello lösenord krypteras innan det skickas tooand lagra i hello elastiska databasen jobb databas.  hello lösenord dekrypteras av hello elastiska databasen jobb tjänsten via hello autentiseringsuppgifter skapas och som överförts från hello installationsskript.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Ny AzureSqlJobCredential</p><p>Ange AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skript</td>
    <td>Transact-SQL-skript toobe används för att köras över databaser.  hello skript ska vara idempotent skapade toobe eftersom hello tjänsten kommer att försöka köra hello skriptet vid fel.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Ny AzureSqlJobContent</p>
    <p>Ange AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Programmet på datanivå </a> paketet toobe används över flera databaser.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Ny AzureSqlJobContent</p>
    <p>Ange AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Databasen mål</td>
    <td>Databas- och name peka tooan Azure SQL Database.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Ny AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Fragmentera kartan mål</td>
    <td>Kombinationen av target databas och en autentiseringsuppgift toobe används toodetermine information som lagras i en elastisk databas Fragmentera mappning.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Ny AzureSqlJobTarget</p>
    <p>Ange AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Mål för anpassad insamling</td>
    <td>Angiven grupp databaser toocollectively använda för körning.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Ny AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Anpassad samling underordnade mål</td>
    <td>Mål för databasen som refereras från en anpassad samling.</td>
    <td>
    <p>Lägg till AzureSqlJobChildTarget</p>
    <p>Ta bort AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Jobb</td>
    <td>
    <p>Definition av parametrar för ett jobb som kan använda tootrigger körning eller toofulfill ett schema.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Ny AzureSqlJob</p>
    <p>Ange AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Jobbkörningen</td>
    <td>
    <p>Behållare för aktiviteter nödvändiga toofulfill antingen köra ett skript eller använda DACPAC tooa mål med autentiseringsuppgifter för databasanslutningar med fel hanteras i enlighet tooan körningsprincipen.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stoppa AzureSqlJobExecution</p>
    <p>Vänta AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Jobb för körning av aktiviteten</td>
    <td>
    <p>Enhet för arbete toofulfill ett jobb.</p>
    <p>Om en projektaktivitet inte kan toosuccessfully köra, hello resulterande Undantagsmeddelande loggas och en ny matchande projektaktivitet skapas och körs i enlighet toohello angetts körningsprincipen.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Start-AzureSqlJobExecution</p>
    <p>Stoppa AzureSqlJobExecution</p>
    <p>Vänta AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Jobbet körningsprincipen</td>
    <td>
    <p>Kontroller jobbet körning timeout, försök gränser och intervall mellan försök.</p>
    <p>Den elastiska databasen jobb innehåller en körningsprincip för standard-jobb som orsakar i princip obegränsat återförsök uppgiften jobbfel med exponentiell backoff intervall mellan varje försök.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Ny AzureSqlJobExecutionPolicy</p>
    <p>Ange AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Schema</td>
    <td>
    <p>Tid baserat specifikationen för körning av tootake plats på ett reoccurring intervall eller på en gång.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Ny AzureSqlJobSchedule</p>
    <p>Ange AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Jobbet utlöser</td>
    <td>
    <p>En mappning mellan ett jobb och ett schema tootrigger jobbkörningen enligt toohello schema.</p>
    </td>
    <td>
    <p>Ny AzureSqlJobTrigger</p>
    <p>Ta bort AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Stöds för elastisk databas jobb gruppera typer
hello jobbet kör Transact-SQL (T-SQL)-skript eller ett program av DACPACs över flera databaser. När ett jobb är skickade toobe utförs över begärde en grupp av databaser, hello jobbet ”expanderas” Hej till underordnade jobb där varje utför hello körning mot en enskild databas i hello grupp. 

Det finns två typer av grupper som du kan skapa: 

* [Fragmentera kartan](sql-database-elastic-scale-shard-map-management.md) grupp: när ett jobb är skickade tootarget en Fragmentera karta, hello jobbet frågar hello Fragmentera kartan toodetermine aktuell uppsättning shards och skapar sedan underordnade jobb för varje Fragmentera hello Fragmentera kartan.
* Samlingsgruppen för anpassade: en anpassad definierad uppsättning databaser. När ett jobb riktar sig till en anpassad samling, skapar den underordnade jobb för varje databas för närvarande i hello anpassad samling.

## <a name="tooset-hello-elastic-database-jobs-connection"></a>tooset hello elastiska jobb databasanslutning
En anslutning måste toobe set toohello jobb *databasen* tidigare toousing hello jobb API: er. Kör denna cmdlet utlöser en autentiseringsuppgift fönstret toopop in begär hello användarnamn och lösenord som skapas när du installerar elastisk databas jobb. Alla exemplen i det här avsnittet förutsätter att det här första steget redan har utförts.

Öppna en anslutning toohello elastisk databas jobb:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a>Krypterade autentiseringsuppgifter i jobb för hello elastisk databas
Databasautentiseringsuppgifter kan infogas i hello jobb *databasen* med dess lösenord krypteras. Det är nödvändigt toostore autentiseringsuppgifter tooenable jobb toobe körs vid ett senare tillfälle (med jobbscheman).

Kryptering fungerar via ett certifikat som skapats som en del av hello installationsskript. hello installationsskriptet skapar och överföringar hello certifikatet till hello Azure Cloud Service för dekryptering av hello lagras krypterade lösenord. hello Azure Cloud Service senare lagrar hello offentlig nyckel i hello jobb *databasen* som aktiverar hello PowerShell API eller klassiska Azure-portalen gränssnittet tooencrypt angivna lösenord utan hello certifikat toobe har installerats lokalt.

hello autentiseringsuppgifter lösenord är krypterad och säker från användare med läsåtkomst tooElastic jobb databasobjekt. Men det är möjligt för en obehörig användare med läs-/ skrivåtkomst tooElastic databasen jobb objekt tooextract ett lösenord. Autentiseringsuppgifterna är utformad toobe återanvänds över jobb körningar. Autentiseringsuppgifter skickas tootarget databaser när anslutningar upprättas. Det finns för närvarande inga begränsningar för hello måldatabaserna används för varje autentiseringsuppgifter, obehörig användare kan lägga till ett mål för databasen för en databas under hello angripare kontroll. hello användare kan sedan starta ett jobb som målobjekt för den här databasen toogain hello-referensens lösenord.

Rekommenderade säkerhetsmetoder för elastisk databas jobb är:

* Begränsa användning av hello-API: er tootrusted enskilda användare.
* Autentiseringsuppgifter ska ha hello minst privilegier krävs tooperform hello projektaktivitet.  Mer information kan ses i detta [auktoriserings- och behörigheter](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-artikel.

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a>toocreate en krypterade autentiseringsuppgifter för jobbkörningen över databaser
toocreate en ny krypterade autentiseringsuppgifter, hello [ **cmdlet Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) uppmanas att ange ett användarnamn och lösenord som kan skickas toohello [ **ny AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a>tooupdate autentiseringsuppgifter
När du ändrar lösenord, använda hello [ **cmdlet Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) och ange hello **CredentialName** parameter.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a>toodefine ett mål för elastisk databas Fragmentera karta
tooexecute ett jobb för alla databaser i en Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)), Använd en Fragmentera som mål för hello-databasen. Det här exemplet kräver ett delat program som skapats med hjälp av hello klientbibliotek för elastisk databas. Se [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).

hello Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan hello specifika Fragmentera mappning måste anges som mål.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Skapa ett T-SQL-skript för körning på databaser
När du skapar T-SQL-skript för körning, rekommenderas toobuild dem toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) och motståndskraftiga mot fel. Den elastiska databasen jobb kommer att försöka körningen av ett skript när körningen påträffar ett fel, oavsett hello klassificering av hello-fel.

Använd hello [ **cmdlet New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate och sparat ett skript för körning och ange hello **- ContentName** och **- CommandText**parametrar.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Skapa ett nytt skript från en fil
Om hello T-SQL-skript har definierats i en fil, använder du skriptet tooimport hello:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a>tooupdate T-SQL-skript för körning över databaser
Den här PowerShell-skript uppdateringar hello T-SQL kommandotexten för ett befintligt skript.

Ange hello följande variabler tooreflect hello önskad skript definition toobe set:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="tooupdate-hello-definition-tooan-existing-script"></a>tooupdate hello definition tooan befintligt skript
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a>toocreate jobbet-tooexecute ett skript i en Fragmentera karta
Detta PowerShell-skript startar ett jobb för körning av ett skript på varje Fragmentera i en elastisk skalbarhet Fragmentera mappning.

Ange hello följande variabler tooreflect hello önskad skript och mål:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a>tooexecute ett jobb
Det här PowerShell-skriptet körs ett befintligt jobb:

Uppdatera hello variabeln tooreflect hello önskad jobbet namn toohave utförs följande:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a>tooretrieve hello tillståndet för en enskild jobbkörningen
Använd hello [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) och ange hello **JobExecutionId** parametern tooview hello tillståndet för jobbkörningen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Använd hello samma **Get-AzureSqlJobExecution** med hello **IncludeChildren** parametern tooview hello tillstånd för underordnad jobbet körningar, nämligen hello specifikt tillstånd för varje jobbkörningen mot varje databasen som mål för hello jobb.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a>tooview hello tillstånd över flera jobb körningar
Hej [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) har flera valfria parametrar som kan använda toodisplay flera jobb körningar, filtreras via hello angivna parametrar. hello nedan visar några av hello möjliga sätt toouse Get-AzureSqlJobExecution:

Hämta alla aktiva översta nivå jobbet körningar:

    Get-AzureSqlJobExecution

Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:

    Get-AzureSqlJobExecution -IncludeInactive

Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

Hämta hello lista över jobb uppgiften körningar inom en specifik jobbkörningen:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Hämta jobb aktivitetsinformation körning:

hello följande PowerShell-skript kan vara används tooview hello information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a>tooretrieve fel i jobb uppgiften körningar
Hej **JobTaskExecution objekt** innehåller en egenskap för hello livscykeln för hello uppgiften tillsammans med en meddelandeegenskap. Om ett jobb för körning av aktiviteten misslyckades hello livscykel kommer att ange egenskapen för*misslyckades* och kommer att anges hello meddelandeegenskap toohello resulterande Undantagsmeddelande och dess stacken. Om ett jobb misslyckades, är det viktigt tooview hello information om jobbuppgifter som misslyckades för ett visst jobb.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a>toowait för en toocomplete för körning av jobbet
hello följande PowerShell-skript kan vara används toowait för ett jobb uppgiften toocomplete:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

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

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Uppdatera en anpassad körningsprincip
Uppdatera hello önskad körning princip tooupdate:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a>Avbryta ett jobb
Den elastiska databasen jobb stöder förfrågningar om annullering av jobb.  Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den toostop hello jobb.

Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:

1. Avbryt aktiviteter som körs: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom hello aspekt av hello aktivitet som körs.  Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är en försök toocancel hello-fråga.
2. Om du avbryter återförsök för aktiviteten: om en annullering identifieras av hello kontrollen tråd innan en uppgift startas för körning hello kontrollen tråd ska undvika starta hello aktivitet och deklarera hello begäran som avbruten.

Om det krävs en annullering av jobbet för överordnade jobb respekteras hello avbrottsbegäran för hello överordnade jobb och alla dess underordnade jobb.

toosubmit en begäran om att avbryta, använda hello [ **cmdlet Stop AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) och ange hello **JobExecutionId** parameter.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a>toodelete ett jobb och Jobbhistorik asynkront
Den elastiska databasen jobb stöder asynkrona borttagning av jobb. Ett jobb kan vara markerad för borttagning och hello tas bort hello jobb och alla dess jobbhistorik när alla jobb körningar har slutförts för hello jobb. hello system Avbryt inte automatiskt aktiva jobb körningar.  

Anropa [ **stoppa AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel aktiva jobb körningar.

tootrigger jobbet borttagning, Använd hello [ **cmdlet Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) och ange hello **jobbnamn** parameter.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a>toocreate mål för en anpassad databas
Du kan definiera anpassade databasen mål för att köra en direkt eller som ska ingå i en anpassad databas-grupp. Till exempel eftersom **elastiska pooler** är stöds inte än direkt med hjälp av PowerShell APIs, kan du skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla hello-databaser i hello pool.

Ange följande variabler tooreflect hello önskad databasinformation hello:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a>toocreate en anpassad databas samling mål
Använd hello [ **ny AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet toodefine en anpassad samling mål tooenable databaskörning över flera definierade databasen mål. När du har skapat en databasgrupp kan databaser vara associerat med hello anpassad samling mål.

Ange hello följande variabler tooreflect hello önskade anpassade samlingar mål konfiguration:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a>tooadd databaser tooa anpassad databas samling mål
tooadd en databas tooa specifika anpassad samling använda hello [ **Lägg till AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Granska hello databaser i en anpassad databas samling målet
Använd hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet tooretrieve hello underordnade databaser i en anpassad databas samling målet. 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Skapa ett skript i ett jobb tooexecute över en anpassad databas samling mål
Använd hello [ **ny AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet toocreate ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål. Den elastiska databasen jobb kommer att expandera hello jobbet till flera underordnade jobb varje motsvarande tooa-databas som är associerade med hello anpassad databas samling mål och se till att hello skriptet körs mot varje databas. Igen, är det viktigt att skripten är idempotent toobe flexibel tooretries.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Insamling av data över databaser
Du kan använda en jobbet tooexecute en fråga i en grupp med databaser och skicka hello tooa specifika resultattabellen. hello tabellen kan efterfrågas efter hello fakta toosee hello frågans resultat från varje databas. Detta ger en asynkron metod tooexecute en fråga över flera databaser. Misslyckade försök hanteras automatiskt via återförsök.

hello angivna måltabellen skapas automatiskt om det inte finns ännu. hello ny tabell matchar hello schemat för hello returnerade en resultatuppsättning. Om ett skript som returnerar flera resultatmängder, skickas endast elastisk databas jobb hello första toohello måltabellen.

hello följande PowerShell-skript körs ett skript och samlar in resultaten i en angiven tabell. Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.

Det här skriptet använder hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet. Ange hello parametrar för skriptet, autentiseringsuppgifter och körning av mål:

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

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a>toocreate och starta ett jobb för scenarion för insamling av data
Det här skriptet använder hello [ **Start AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="tooschedule-a-job-execution-trigger"></a>tooschedule en utlösare för körning av jobbet
hello följande PowerShell-skript kan vara används toocreate ett återkommande schema. Det här skriptet använder ett minuters intervall, men [ **ny AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar. Scheman som kör en gång kan skapas med skicka - görs.

Skapa ett nytt schema:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a>tootrigger ett jobb som körs på en tidsplan
En utlösare för jobb kan vara definierade toohave ett jobb körs bl.a tooa tid schema. hello följande PowerShell-skript kan vara används toocreate en utlösare för jobbet.

Använd [ny AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) och ange hello följande variabler toocorrespond toohello önskade jobbet och schema:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>tooremove ett schemalagda association toostop jobb körs enligt schema
toodiscontinue igen jobbkörningen via jobb utlösare hello jobbet utlösare kan tas bort. Ta bort ett jobb utlösaren toostop ett jobb från utförs bl.a tooa schema med hello [ **cmdlet Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a>Hämta jobbschemat utlösare bundna tooa tid
hello följande PowerShell-skript kan använda tooobtain och visa hello utlösare registrerade tooa viss tid jobbschemat.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a>tooretrieve jobb utlösare bundna tooa jobb
Använd [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) tooobtain och visa scheman som innehåller ett registrerade jobb.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a>toocreate datanivå-program (DACPAC) för körning över databaser
toocreate en DACPAC finns [-datanivåprogram](https://msdn.microsoft.com/library/ee210546.aspx). toodeploy DACPAC, använda hello [cmdlet New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent). Hej DACPAC måste vara tillgänglig toohello tjänst. Det är rekommenderat tooupload en skapade DACPAC tooAzure lagring och skapa en [signatur för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för hello DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a>tooupdate datanivå-program (DACPAC) för körning över databaser
Befintliga DACPACs som registrerats i den elastiska databasen jobb kan vara uppdaterade toopoint toonew URI: er. Använd hello [ **cmdlet Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI på en befintlig registrerad DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a>toocreate jobbet-tooapply datanivå-program (DACPAC) över databaser
När en DACPAC har skapats i den elastiska databasen jobb, kan ett jobb skapas tooapply hello DACPAC över flera databaser. hello följande PowerShell-skript kan vara används toocreate ett DACPAC-jobb över en anpassad samling med databaser:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
