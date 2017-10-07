---
title: "aaaManage Azure Data Lake Analytics med hjälp av Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toomanage Data Lake Analytics-konton, datakällor, jobb och katalogobjekt. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/23/2017
ms.author: mahi
ms.openlocfilehash: 5954f0efb7d5a9778727edfccae83aec046343bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Lär dig hur toomanage Azure Data Lake Analytics-konton, datakällor, jobb och katalogobjekt med hjälp av Azure PowerShell. 

## <a name="prerequisites"></a>Krav

När du skapar ett Data Lake Analytics-konto behöver du tooknow:

* **Prenumerations-ID**: hello Azure prenumerations-ID som Data Lake Analytics-kontot finns.
* **Resursgruppen**: hello namnet på hello Azure-resursgrupp som innehåller ditt Data Lake Analytics-konto.
* **Data Lake Analytics-kontonamn**: hello konto namnet får bara innehålla gemena bokstäver och siffror.
* **Data Lake Store-standardkonto**: Varje Data Lake Analytics-konto har ett Data Lake Store-standardkonto. Dessa konton måste finnas i hello samma plats.
* **Plats**: hello platsen för ditt Data Lake Analytics-konto, till exempel ”USA, östra 2” eller andra platser som stöds. Platser som stöds kan visas på vår [sida med priser](https://azure.microsoft.com/pricing/details/data-lake-analytics/).

hello PowerShell kodavsnitt i den här kursen använder dessa variabler toostore informationen

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a>Logga in

Logga in med ett prenumerations-id.

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

Logga in med ett prenumerationsnamn.

```
Login-AzureRmAccount -SubscriptionName $subname 
```

Hej `Login-AzureRmAccount` cmdlet efterfrågar alltid autentiseringsuppgifter. Du kan undvika uppmanas med hjälp av hello följande cmdlets:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a>Hantera konton

### <a name="create-a-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto

Om du inte redan har en [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, skapa en. 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Alla Data Lake Analytics-konton kräver ett Data Lake Store-standardkonto som används för att lagra loggar. Du kan återanvända ett befintligt konto eller skapa ett konto. 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

När en resursgrupp och ett Data Lake Store-konto är tillgängligt skapar du ett Data Lake Analytics-konto.

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a>Hämta information om ett konto

Få information om ett konto.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

Kontrollera hello förekomsten av ett visst Data Lake Analytics-konto. hello cmdlet returnerar antingen `True` eller `False`.

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

Kontrollera hello förekomsten av ett visst Data Lake Store-konto. hello cmdlet returnerar antingen `True` eller `False`.

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a>Visar en lista över konton

Lista över Data Lake Analytics-konton inom hello aktuella prenumeration.

```powershell
Get-AdlAnalyticsAccount
```

Lista över Data Lake Analytics-konton inom en viss resursgrupp.

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a>Hantera brandväggsregler

Visa en lista med brandväggsregler.

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

Lägg till en brandväggsregel.

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Ändra en brandväggsregel.

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

Ta bort en brandväggsregel.

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



Tillåt Azure IP-adresser.

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a>Hantera datakällor
Azure Data Lake Analytics stöder för närvarande hello följande datakällor:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

När du skapar ett Analytics-konto, måste du ange en datakälla för Data Lake Store-konto toobe hello standard. hello Data Lake Store-standardkontot används toostore jobbet metadata och jobbet granskningsloggar. När du har skapat ett Data Lake Analytics-konto kan du lägga till ytterligare Data Lake Store-konton och/eller Storage-konton. 

### <a name="find-hello-default-data-lake-store-account"></a>Hitta hello standardkontot för Data Lake Store

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Du kan hitta Data Lake Store-standardkontot hello genom att filtrera hello lista över datakällor av hello `IsDefault` egenskapen:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Lägga till en datakälla

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Lista över datakällor

```powershell
# List all hello data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>Skicka U-SQL-jobb

### <a name="submit-a-string-as-a-u-sql-script"></a>Skicka en sträng som ett U-SQL-skript

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a>Skicka en fil som ett U-SQL-skript

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a>Lista över jobb i ett konto

### <a name="list-all-hello-jobs-in-hello-account"></a>Lista över alla hello jobb i hello-konto. 

hello utdata innehåller hello pågående jobb och de jobb som nyligen har slutförts.

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a>Visa en lista med ett visst antal jobb

Som standard hello lista över jobb sorteras på Skicka tid. Så hello senast skickade visas jobb först. Hej ADLA konto kommer ihåg jobb i 180 dagar som standard, men hello Ge AdlJob cmdlet som standard returnerar endast hello första 500. Använd - översta parametern toolist ett visst antal jobb.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-hello-value-of-job-property"></a>Lista över jobb baserat på hello värdet för egenskapen för jobb

Med hjälp av hello `-State` parameter. Du kan kombinera dessa värden:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List hello running jobs
Get-AdlJob -Account $adla -State Running

# List hello jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List hello jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

Använd hello `-Result` parametern toodetect om avslutades jobben har slutförts. Det har dessa värden:

* Avbröts
* Det gick inte
* Ingen
* Lyckades

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


Hej `-Submitter` parametern hjälper dig att identifiera vem som har skickat ett jobb.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

Hej `-SubmittedAfter` är användbart i filtrering tooa tidsintervall.


```powershell
# List  jobs submitted in hello last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in hello last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a>Vanliga scenarier för att visa en lista över jobb


```
# List jobs submitted in hello last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within hello past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a>Filtrera en lista över jobb

När du har en lista över jobb i den aktuella PowerShell-sessionen. Du kan använda vanliga PowerShell-cmdlets toofilter hello lista.

Filtrera en lista över jobb toohello jobb skickade i hello senaste 24 timmarna

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

Filtrera en lista över jobb toohello jobb som avslutats i hello senaste 24 timmarna

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

Filtrera en lista över jobb toohello jobb som startats. Ett jobb kan misslyckas vid kompileringen - och därför aldrig startas. Nu ska vi titta på hello misslyckades jobb som startade körs och sedan misslyckades.

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a>Analysera en lista över jobb

Använd hello `Group-Object` cmdlet tooanalyze en lista över jobb.

```
# Count hello number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count hello number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count hello number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count hello number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
När du utför en analys, kan det vara användbart tooadd egenskaper toohello jobbet objekt toomake filtrering och gruppering enklare. hello följande utdrag visar hur tooannotate en Jobbinformations med beräknade egenskaper.

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

## <a name="get-information-about-pipelines-and-recurrences"></a>Hämta information om pipelines och repetitioner

Använd hello `Get-AdlJobPipeline` cmdlet toosee hello pipeline-information skickats tidigare jobb.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Använd hello `Get-AdlJobRecurrence` cmdlet toosee hello återkommande information för tidigare skickade jobb.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a>Hämta information om ett jobb

### <a name="get-job-status"></a>Hämta jobbstatus

Hämta hello status för ett visst jobb.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-hello-job-outputs"></a>Granska utdata för hello jobb

När hello-jobbet har avslutats, kontrollerar du om hello utdatafilen finns genom att ange hello filer i en mapp.

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a>Hantera jobb som körs

### <a name="cancel-a-job"></a>Avbryta ett jobb

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-toofinish"></a>Vänta tills en jobbet toofinish

I stället för att upprepa `Get-AdlAnalyticsJob` tills ett jobb är klar kan du använda hello `Wait-AdlJob` cmdlet toowait för hello jobbet tooend.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a>Hantera principer för beräkning

### <a name="list-existing-compute-policies"></a>Visa en lista med befintliga beräknings-principer

Hej `Get-AdlAnalyticsComputePolicy` cmdlet hämtar information om beräkning principer för ett Data Lake Analytics-konto.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Skapa en princip för beräkning

Hej `New-AdlAnalyticsComputePolicy` cmdlet skapar en ny princip för beräkning för ett Data Lake Analytics-konto. Det här exemplet anger hello maximala Australien tillgängliga toohello angivna användare too50 och hello minsta jobbet prioritet too250.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-hello-existence-of-a-file"></a>Sök efter hello förekomsten av en fil.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a>Överföra och ladda ned

Överför en fil.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Ladda upp en hel mapp rekursivt.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Hämta en fil.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Hämta en hel mapp rekursivt.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Om hello överföra eller hämta processen avbryts, försök tooresume hello processen genom att köra hello cmdleten igen med hello ``-Resume`` flaggan.

## <a name="manage-catalog-items"></a>Hantera katalogobjekt

hello U-SQL-katalogen är används toostructure data och kod så att de kan delas av U-SQL-skript. hello katalog kan hello högsta prestanda möjligt med data i Azure Data Lake. Mer information finns i [Använd U-SQL-katalogen](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-hello-u-sql-catalog"></a>Listobjekt i hello U-SQL-katalogen

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

Lista över alla hello sammansättningar i alla hello databaser i en ADLA.

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Få information om ett katalogobjekt

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a>Skapa autentiseringsuppgifter i en katalog

I en U-SQL-databas, skapar du ett autentiseringsuppgiftobjekt för en databas som finns på Azure. U-SQL-autentiseringsuppgifter för närvarande hello enda typ av katalogobjekt som du kan skapa med hjälp av PowerShell.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a>Hämta grundläggande information om ett ADLA konto

Det angivna namnet på ett konto letar hello följande kod upp grundläggande information om hello-konto

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```

## <a name="working-with-azure"></a>Arbeta med Azure

### <a name="get-details-of-azurerm-errors"></a>Hämta information om AzureRm fel

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a>Kontrollera om du kör som administratör

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Hitta en TenantID

Från ett prenumerationsnamn:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Från ett prenumerations-id:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Från en domänadress, till exempel ”contoso.com”


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>En lista över alla prenumerationer och klient-ID: n

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Skapa ett Data Lake Analytics-konto med hjälp av en mall

Du kan också använda en mall för Azure-resursgrupp med hello följande PowerShell-skript:

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update hello JSON template path 

# Log in tooAzure
Login-AzureRmAccount -SubscriptionId $subId

# Create hello resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create hello Data Lake Analytics account with hello default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

Mer information finns i [distribuera ett program med Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md) och [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

**Exempelmall**

Spara hello följande text som en `.json` fil och sedan använda hello föregående PowerShell-skriptet toouse hello mallen. 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Analytics account toocreate."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Store account toocreate."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a>Nästa steg
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Kom igång med Data Lake Analytics med hjälp av [Azure-portalen](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Hantera Azure Data Lake Analytics med hjälp av [Azure-portalen](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
