---
title: 'PowerShell: Skapa och hantera en elastisk pool med Azure SQL | Microsoft Docs'
description: "Lär dig hur toouse PowerShell toomanage en elastisk pool."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 61289770-69b9-4ae3-9252-d0e94d709331
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/06/2017
ms.author: srinia
ms.openlocfilehash: 92de2a4b243dcc74502064e9d2c31682691753d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a>Skapa och hantera en elastisk pool med PowerShell
Det här avsnittet beskrivs hur du toocreate och hantera skalbara [elastiska pooler](sql-database-elastic-pool.md) med PowerShell.  Du kan också skapa och hantera en Azure-elastisk pool med hello [Azure-portalen](https://portal.azure.com/), REST-API eller [C#](sql-database-elastic-pool-manage-csharp.md). Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a>Skapa en elastisk pool
Hej [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet skapar en elastisk pool. hello värdena för eDTU per pool, min och max dtu: er är begränsad av hello tjänstnivåvärdet (Basic, Standard, Premium eller Premium RS). Se [eDTU och lagringsgränser för elastiska pooler och grupperade databaser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Skapa en delad databas i en elastisk pool
Använd hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet och ange hello **ElasticPoolName** målpool för parametern toohello. toomove en befintlig databas i en elastisk pool, se [flytta en databas till en elastisk pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a>Fullständigt skript
Det här skriptet skapar en Azure-resursgrupp och en server. När du uppmanas ange ett administratörsanvändarnamn och lösenord för hello ny server (inte dina Azure-autentiseringsuppgifter).

```PowerShell
$subscriptionId = '<your Azure subscription id>'
$resourceGroupName = '<resource group name>'
$location = '<datacenter location>'
$serverName = '<server name>'
$poolName = '<pool name>'
$databaseName = '<database name>'

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB
```

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a>Skapa en elastisk pool och lägga till flera grupperade databaser
Skapandet av många databaser i en elastisk pool kan ta tid när det görs med hello portal eller PowerShell-cmdletar som skapar bara en enskild databas åt gången. tooautomate skapande i en elastisk pool, se [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="move-a-database-into-an-elastic-pool"></a>Flytta en databas till en elastisk pool
Du kan flytta en databas till eller från en elastisk pool med hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a>Ändra inställningar för prestanda för en elastisk pool
När sjunker prestanda, kan du ändra inställningarna för hello hello poolen tooaccommodate tillväxt. Använd hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet. Ange hello - Dtu parametern toohello edtu: er per pool. Se [eDTU och Lagringsgränser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för möjliga värden.

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a>Ändra hello lagringsgränsen för en elastisk pool

Använd hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet tooset hello _- StorageMB_ parameter. Ange hello lagringsgränsen i MB (till exempel 2097152 anger hello lagring gränsen too2 TB). Se [eDTU och Lagringsgränser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för möjliga värden.

> [!IMPORTANT]
> Hej max data standardlagring per pool för Premium-pooler med 1500 edtu: er eller flera är 750 GB. tooobtain hello högre _max data lagringsstorleken per pool_, hello lagringsgräns måste anges explicit. Premium-pooler med mer än 750 GB lagring är för närvarande i förhandsversion i hello följande områden: östra USA 2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, Sydostasien, östra, Östra Australien, Kanada Central och Kanada Öst.

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a>Hämta hello status för pool-åtgärder
Skapa en elastisk pool kan ta tid. tootrack hello status för poolen operations inklusive skapandet och uppdateringar, Använd hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a>Hämta hello status för att flytta en databas till och från en elastisk pool
Flytta en databas kan ta tid. Spåra en move-status med hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Hämta resursanvändningsdata för en elastisk pool
Mått som kan hämtas som en procentandel av hello resursgräns pool:

| Måttnamnet | Beskrivning |
|:--- |:--- |
| CPU\_procent |Genomsnittlig beräkning användning i procent av hello gränsen för hello pool. |
| fysiska\_data\_läsa\_procent |Genomsnittlig i/o-användning i procent baserat på hello gränsen för hello pool. |
| loggen\_skriva\_procent |Genomsnittlig skriva resursutnyttjande i procent av hello gränsen för hello pool. |
| DTU\_förbrukning\_procent |Genomsnittlig eDTU-användning i procent av eDTU gränsen för hello poolen |
| lagring\_procent |Genomsnittlig lagringsanvändningen i procent av hello lagringsgränsen för hello pool. |
| anställda\_procent |Maximal samtidiga arbetare (antal begäranden) i procent baserat på hello gränsen för hello pool. |
| sessioner\_procent |Maximalt antal samtidiga sessioner i procent baserat på hello gränsen för hello pool. |
| eDTU_limit |Aktuell elastiska poolens DTU inställning för den här elastiska poolen under intervallet. |
| lagring\_gräns |Aktuella elastiska poolens lagringsgräns inställningen för den här elastiska poolen i megabyte under intervallet. |
| eDTU\_används |Genomsnittlig edtu: er som används av hello-pool i det här intervallet. |
| lagring\_används |Genomsnittlig lagringsutrymme som används av hello-pool i det här intervallet i byte |

**Mått granularitet/kvarhållningsperioder:**

* Data returneras på 5 minuter granularitet.  
* Datakvarhållning är 35 dagar.  

Denna cmdlet och API begränsar hello antalet rader som kan hämtas i ett anrop too1000 rader (cirka 3 dagar data på 5 minuter granularitet). Men det här kommandot kan anropas flera gånger med olika börja/sluta tid intervall tooretrieve mer data

tooretrieve hello mått:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a>Hämta resursanvändningsdata för en databas i en elastisk pool
Dessa API: er är hello samma som hello API: er som används för att övervaka hello resursanvändningen för en enskild databas, förutom följande semantiska skillnaden hello: mått som hämtats är uttryckt i procent av hello per databas max edtu: er (eller motsvarande fjärrskrivbordsanslutning för hello Ange underliggande måttet som processor eller I/O) för poolen. Till exempel anger 50% användning av någon av de här måtten att hello resursanvändningen för specifika är vid 50% av hello per databas cap-gränsen för den här resursen i hello överordnade poolen.

tooretrieve hello mått:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Lägg till en resurs för avisering tooan elastisk pool
Du kan lägga till Varningsregler tooan elastisk pool toosend e-postmeddelanden eller meddela strängar för[URL slutpunkter](https://msdn.microsoft.com/library/mt718036.aspx) när hello elastisk pool träffar ett tröskelvärde för användning som du skapat. Använd hello Lägg till AzureRmMetricAlertRule cmdlet.

> [!IMPORTANT]
> Resursutnyttjande övervakning för elastiska pooler har en fördröjning på minst 5 minuter. Konfigurera aviseringar för mindre än 10 minuter för elastiska pooler stöds inte för närvarande. Alla aviseringar som angetts för elastiska pooler med en punkt (parameter med namnet ”-fönsterstorlek” i PowerShell API) på mindre än 10 minuter kan inte utlösas. Kontrollera att alla aviseringar som du definierar för elastiska pooler använder en period (WindowSize) på 10 minuter eller mer.
>
>

Det här exemplet lägger till en avisering för att få en avisering när en elastisk pool-eDTU förbrukning går över tröskelvärde.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location =  '<location'                         # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

#$Target Resource ID
$ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# create a unique rule name
$alertName = $poolName + "- DTU consumption rule"

# Create an alert rule for DTU_consumption_percent
Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail
```

Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a>Lägga till aviseringar tooall databaser i en elastisk pool
Du kan lägga till Varningsregler tooall databas i en elastisk pool toosend e-postmeddelanden eller meddela strängar för[URL slutpunkter](https://msdn.microsoft.com/library/mt718036.aspx) när en resurs träffar ett tröskelvärdet för användning som konfigurerats av hello avisering.

> [!IMPORTANT]
> Resursutnyttjande övervakning för elastiska pooler har en fördröjning på minst 5 minuter. Konfigurera aviseringar för mindre än 10 minuter för elastiska pooler stöds inte för närvarande. Alla aviseringar som angetts för elastiska pooler med en punkt (parameter med namnet ”-fönsterstorlek” i PowerShell API) på mindre än 10 minuter kan inte utlösas. Kontrollera att alla aviseringar som du definierar för elastiska pooler använder en period (WindowSize) på 10 minuter eller mer.
>

Det här exemplet lägger till en avisering tooeach hello databaser i en elastisk pool för att få ett meddelande när att databasen DTU-förbrukning går över tröskelvärde.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop hello alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Samla in och övervaka resursanvändningsdata över flera pooler i en prenumeration
När du har många databaser i en prenumeration är besvärlig toomonitor varje elastisk pool separat. I stället kan PowerShell-cmdlets för SQL-databasen och T-SQL-frågor vara kombinerade toocollect resursanvändningsdata från flera pooler och deras databaser för övervakning och analys av Resursanvändning. En [exempel på implementering](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) av sådana en uppsättning powershell-skript finns i hello GitHub SQL Server-exempel databasen tillsammans med dokumentation om vad det gör och hur toouse den.

toouse detta exempel på implementering, Följ dessa steg.

1. Hämta hello [skript och dokumentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Ändra hello skript för din miljö. Ange en eller flera servrar som elastiska pooler finns.
3. Ange en telemetri databas där hello insamlade är toobe lagras.
4. Anpassa hello skriptet toospecify hello varaktighet för körning av hello skript.

På en hög nivå hello skript hello följande:

* Räknar upp alla servrar i en viss Azure-prenumeration (eller en angiven lista över servrar).
* Kör ett bakgrundsjobb för varje server. hello jobbet körs i en slinga med jämna mellanrum och samlar in telemetridata för alla hello pooler i hello-server. Hello insamlade data hämtas sedan till hello angivna telemetri databasen.
* Räknar upp en lista över databaser i varje pool toocollect hello databasen resursanvändningsdata. Hello insamlade data hämtas sedan till hello telemetri databasen.

hello kan insamlade mått i hello telemetri databasen vara analyserade toomonitor hello hälsotillståndet för elastiska pooler och hello databaser i den. hello skriptet installerar också en fördefinierad tabell Value-funktion (Tabellvärdesfunktion) i hello telemetri databasen toohelp sammanställd hello mått för ett angivet tidsintervall. Resultaten av hello Tabellvärdesfunktion kan till exempel vara används tooshow ”främsta elastiska pooler med hello maximala eDTU-användning i ett angivet tidsintervall”. Du kan också använda analytiska verktyg som Excel eller Power BI tooquery och analysera hello insamlade data.

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a>Exempel: hämta resurs förbrukning mått för en elastisk pool och dess databaser
Det här exemplet hämtar hello förbrukning mått för en given elastiska poolen och alla dess databaser. Insamlade data är formaterad och skrivs tooa CSV-formaterad fil. hello filen går att bläddra i Excel.

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login tooAzure account and select hello subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for hello specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct hello pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format hello metrics and output as .csv file using hello following script block.
$command = {
param($metricList, $outputFile)

# Format metrics into a table.
$table = @()
foreach($metric in $metricList) {
   foreach($metricValue in $metric.MetricValues) {
      $sx = New-Object PSObject -Property @{
      Timestamp = $metricValue.Timestamp.ToString()
      MetricName = $metric.Name;
      Average = $metricValue.Average;
      ResourceID = $metric.ResourceId
   }$table = $table += $sx
   }
}

# Output hello metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a>Svarstiden för åtgärder elastisk pool
* Ändra hello min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.
* Ändra hello edtu: er per pool beror på hello totala mängden diskutrymme som används av alla databaser i hello pool. Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB. Om hello totalt utrymme som används av alla databaser i poolen hello är exempelvis 200 GB, och sedan hello förväntade svarstid för att ändra hello pool-eDTU per pool är tre timmar eller mindre.



## <a name="next-steps"></a>Nästa steg
* [Skapa elastiska jobb](sql-database-elastic-jobs-overview.md) elastiska jobb låter dig köra T-SQL-skript mot valfritt antal databaser i hello pool.
* Se [skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md): Använd elastisk verktyg tooscale ut, flytta data, fråga eller skapa transaktioner.
