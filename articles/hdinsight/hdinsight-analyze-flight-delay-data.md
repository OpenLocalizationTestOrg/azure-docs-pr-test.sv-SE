---
title: "aaaAnalyze svarta fördröjning data med Hadoop i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse en Windows PowerShell-skript toocreate ett HDInsight-kluster kör ett Hive-jobb, köra ett jobb för Sqoop och ta bort hello klustret."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6ebaee65d9b270e5dc2141dd1265011d372f497d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analysera svarta fördröjning data med hjälp av Hive i HDInsight
Hive ger dig möjlighet att köra Hadoop MapReduce jobb via en SQL-liknande skriptspråk som kallas * [HiveQL][hadoop-hiveql]*, som kan användas mot sammanfattning, fråga och analys av stora mängder data.

> [!IMPORTANT]
> hello kräver stegen i det här dokumentet ett Windows-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Åtgärder för att arbeta med ett Linux-baserade kluster, se [analysera svarta fördröjning data med hjälp av Hive i HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

En av hello viktiga fördelar med Azure HDInsight är hello uppdelning av datalagring och beräkning. HDInsight använder Azure Blob storage för lagring av data. En typisk jobbet innefattar tre delar:

1. **Lagra data i Azure Blob storage.**  Till exempel väder data, sensordata, webbloggar, och i det här fallet svarta fördröjning data sparas i Azure Blob storage.
2. **Kör jobb.** När det är tid tooprocess hello data kan du köra en Windows PowerShell-skript (eller ett klientprogram) toocreate ett HDInsight-kluster, köra jobb och ta bort hello klustret. hello jobb spara utdata data tooAzure Blob storage. hello utdata bevaras även efter hello kluster har tagits bort. På så sätt kan du betalar bara vad du har förbrukat.
3. **Hämta hello utdata från Azure Blob storage**, eller exportera hello data tooan Azure SQL-databas i den här kursen.

hello följande diagram illustrerar hello scenariot och hello strukturen för den här kursen:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Observera att hello siffrorna i hello diagram visar toohello avsnittsrubriker. **M** står för hello huvudsakliga processen. **En** står för hello innehållet i hello tillägg.

hello största delen av hello kursen visar hur toouse en Windows PowerShell-skript tooperform hello följande uppgifter:

* Skapa ett HDInsight-kluster.
* Köra en Hive-jobb på hello klustret toocalculate genomsnittlig fördröjningar vid flygplatser. hello svarta fördröjning data lagras i ett Azure Blob storage-konto.
* Kör en Sqoop jobbet tooexport hello Hive-jobbet utdata tooan Azure SQL-databas.
* Ta bort hello HDInsight-kluster.

I hello bilagorna, kan du hitta hello anvisningar för överföringen svarta fördröjning data, skapa/ladda upp en Hive-frågesträng och förberedde hello Sqoop jobbet hello Azure SQL-databas.

> [!NOTE]
> hello stegen i det här dokumentet är specifika tooWindows-baserade HDInsight-kluster. Åtgärder för att arbeta med ett Linux-baserade kluster, se [analysera svarta fördröjning data med Hive i HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

### <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **En arbetsstation med Azure PowerShell**.

    > [!IMPORTANT]
    > Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017. hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell. Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.

**Filer som används i den här självstudiekursen**

Den här kursen använder hello-time-prestanda för flygbolag svarta data från [forskning och innovativa tekniken Administration, Bureau transport statistik eller RITA][rita-website].
En kopia av hello data har överfört tooan Azure Blob storage-behållare med hello offentlig Blob-behörighet.
En del av PowerShell-skript kopierar hello data från hello offentlig blob-behållaren toohello standardbehållaren på klustret. Hej HiveQL skript är också kopieras toohello samma Blob-behållare.
Om du vill toolearn hur tooget/överför hello data tooyour äger Storage-konto och hur toocreate/överför hello HiveQL skriptfil, se [bilaga A](#appendix-a) och [bilaga B](#appendix-b).

hello visas följande tabell hello-filer som används i den här självstudiekursen:

<table border="1">
<tr><th>Filer</th><th>Beskrivning</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Hej HiveQL skriptfilen som används av hello Hive-jobb. Det här skriptet har överförda tooan Azure Blob storage-konto med hello offentlig åtkomst. <a href="#appendix-b">Bilaga B</a> har instruktioner för att förbereda och ladda upp den här filen tooyour egna Azure Blob storage-konto.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Indata för hello Hive-jobb. hello data har överförts tooan Azure Blob storage-konto med hello offentlig åtkomst. <a href="#appendix-a">Bilaga A</a> innehåller instruktioner hämtning av hello data och överför hello data tooyour egna Azure Blob storage-konto.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>hello Utdatasökväg för hello Hive-jobb. hello standardbehållaren används för att lagra hello utdata.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>hello Hive-jobbet status mapp på hello standardbehållaren.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Skapa kluster och köra Hive/Sqoop jobb
Hadoop MapReduce är batch-bearbetning. hello de flesta kostnadseffektivt sätt toorun ett Hive-jobb är toocreate ett kluster för hello jobb och tar bort hello jobbet när hello jobbet har slutförts. hello innehåller följande skript hello hela processen.
Mer information om hur du skapar ett HDInsight-kluster och köra Hive-jobb finns [skapa Hadoop-kluster i HDInsight] [ hdinsight-provision] och [använda Hive med HDInsight][hdinsight-use-hive].

**toorun hello Hive-frågor med Azure PowerShell**

1. Skapa en Azure SQL-databasen och hello tabell för hello Sqoop jobbutdata med hello instruktionerna i [bilaga C](#appendix-c).
2. Öppna Windows PowerShell ISE och kör följande skript hello:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure hello follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on hello version, hello folder can be different

    ###########################################
    # (Optional) configure hello following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter hello Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use hello cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare hello HiveQL script and source data
    ###########################################

    # Create hello temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download hello sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data toodefault container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit hello Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit hello Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete hello cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. Ansluta tooyour SQL-databas och se genomsnittlig svarta fördröjningar efter ort i hello AvgDelays tabell:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Bilaga A – överför svarta fördröjning data tooAzure Blob storage
Överför hello-datafilen och hello HiveQL skriptfiler (se [bilaga B](#appendix-b)) kräver lite planering. hello idé är toostore hello-datafiler och hello HiveQL filen innan du skapar ett HDInsight-kluster och köra hello Hive-jobb. Du kan välja mellan två alternativ:

* **Använd hello samma Azure Storage-konto som ska användas av hello HDInsight-kluster som hello standardfilsystem.** Eftersom hello HDInsight-kluster har hello åtkomstnyckeln för Lagringskontot, behöver du inte toomake några ytterligare ändringar.
* **Använd ett annat Azure Storage-konto från hello standardfilsystem för HDInsight-kluster.** Om så är fallet hello måste du ändra hello skapas en del av hello Windows PowerShell-skript finns i [skapa HDInsight-kluster och kör Hive/Sqoop jobb](#runjob) toolink hello Storage-konto som ett ytterligare Storage-konto. Instruktioner finns i [skapa Hadoop-kluster i HDInsight][hdinsight-provision]. Hej HDInsight-kluster känner sedan hello åtkomstnyckeln för hello Storage-konto.

> [!NOTE]
> Hej sökvägen för Blob-lagring för hello datafilen är hårddisken kodad i hello HiveQL skriptfilen. Du måste uppdatera därefter.

**toodownload hello svarta data**

1. Bläddra för[forskning och innovativa tekniken Administration, Bureau transport statistik][rita-website].
2. Hello på sidan Välj hello följande värden:

    <table border="1">
    <tr><th>Namn</th><th>Värde</th></tr>
    <tr><td>Filtrera år</td><td>2013 </td></tr>
    <tr><td>Filtrera Period</td><td>Januari</td></tr>
    <tr><td>Fält</td><td>*År*, *FlightDate*, *UniqueCarrier*, *operatör*, *FlightNum*, *OriginAirportID*, *ursprung*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, * NASDelay*, *SecurityDelay*, *LateAircraftDelay* (rensa alla andra fält)</td></tr>
    </table>
3.Klicka på **hämta**.
4. Packa upp hello filen toohello **C:\Tutorials\FlightDelay\2013Data** mapp. Varje fil är en CSV-fil och är ungefär 60GB i storlek.
5. Byt namn på hello toohello filnamn hello månad som den innehåller data för. Till exempel hello-fil som innehåller hello januari data namnet *January.csv*.
6. Upprepa steg 2 och 5 toodownload en fil för varje hello 12 månader i 2013. Du behöver minst en fil toorun hello kursen.

**tooupload hello svarta fördröjning data tooAzure Blob storage**

1. Förbered hello parametrar:

    <table border="1">
    <tr><th>Variabelnamn</th><th>Anteckningar</th></tr>
    <tr><td>$storageAccountName</td><td>hello Azure Storage-konto där du vill att tooupload hello data.</td></tr>
    <tr><td>$blobContainerName</td><td>hello Blob-behållaren där du vill att tooupload hello data.</td></tr>
    </table>
2. Öppna Azure PowerShell ISE.
3. Klistra in följande skript i hello Skriptfönster hello:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # hello source folder
    $destFolder = "tutorials/flightdelay/2013data"     #hello blob name prefix for hello files toobe uploaded
    #EndRegion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy hello file from local workstation tooAzure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName too$blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "hello source folder on hello workstation doesn't exist" -ForegroundColor Red
    }

    # List hello uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Tryck på **F5** toorun hello skript.

Om du väljer toouse en annan metod för att överföra hello filer, kontrollera hello filsökvägen är självstudier-flightdelay-data. hello-syntaxen för att komma åt hello filer är:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

hello sökväg självstudier/flightdelay/data är hello virtuell mapp du skapade när du har överfört hello-filer. Kontrollera att det finns 12 filer, en för varje månad.

> [!NOTE]
> Du måste uppdatera hello Hive-fråga tooread från hello ny plats.
>
> Du måste konfigurera hello behållaren åtkomst behörighet toobe offentliga eller binda hello Storage-konto toohello HDInsight-kluster. Annars blir inte hello Hive frågesträngen kan tooaccess hello-datafiler.

- - -

## <a id="appendix-b"></a>Bilaga B – skapa och ladda upp en HiveQL-skript
Med Azure PowerShell kan köra du flera HiveQL-instruktioner som vid en tidpunkt eller paketet hello HiveQL-instruktionen i en skriptfil. Det här avsnittet visar hur toocreate HiveQL-skript och överför hello skript tooAzure Blob storage med hjälp av Azure PowerShell. Hive kräver hello HiveQL-skript toobe lagras i Azure Blob storage.

Hej HiveQL-skript utför hello följande:

1. **Släppa hello delays_raw tabellen**, om hello tabellen finns redan.
2. **Skapa hello delays_raw externa Hive-tabell** pekar toohello Blob-lagringsplats med hello svarta fördröjning filer. Den här frågan anger att fälten avgränsas med ””, och att rader avslutas med ”\n”. Det kan medföra problem när fältvärden innehålla kommatecken eftersom Hive kan skilja mellan ett kommatecken som är en fältavgränsaren och en som är en del av ett fältvärde (vilket är det hello i fältvärden för URSPRUNGET\_Stad\_namnet och MÅLET\_ Stad\_namn). tooaddress detta, hello frågan skapar TEMP kolumner toohold data som felaktigt delas upp i kolumner.
3. **Släppa hello fördröjningar tabellen**, om hello tabellen finns redan.
4. **Skapa hello fördröjningar tabell**. Det är bra tooclean hello data innan vidare bearbetning. Den här frågan skapar en ny tabell *fördröjningar*, från hello delays_raw tabell. Observera att inte kopieras hello TEMP kolumner (som tidigare nämnts) och den hello **delsträngen** funktionen är används tooremove citattecken från hello data.
5. **Beräkna hello genomsnittliga väderlek fördröjning och grupper hello resultat av Ortnamn.** Det kommer också utdata hello resultat tooBlob lagring. Observera hello frågan tar bort apostrofer från hello data och utesluta rader där hello värde för **weather_delay** är null. Detta är nödvändigt eftersom Sqoop, används senare i den här kursen kan inte hantera dessa värden avslutas som standard.

En fullständig lista över hello HiveQL kommandon finns [Hive Data Definition Language][hadoop-hiveql]. Varje kommando HiveQL måste sluta med ett semikolon.

**toocreate en HiveQL-skriptfil**

1. Förbered hello parametrar:

    <table border="1">
    <tr><th>Variabelnamn</th><th>Anteckningar</th></tr>
    <tr><td>$storageAccountName</td><td>hello Azure Storage-konto där du vill tooupload hello HiveQL skript.</td></tr>
    <tr><td>$blobContainerName</td><td>hello Blob-behållaren där du vill tooupload hello HiveQL skript.</td></tr>
    </table>
2. Öppna Azure PowerShell ISE.
3. Kopiera och klistra in följande skript i hello Skriptfönster hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # hello HiveQL script file is exported as this file before it's uploaded tooBlob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # hello HiveQL script file will be uploaded tooBlob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by hello HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate hello file and file path

    # Check if a file with hello same file name already exists on hello workstation
    Write-Host "`nvalidating hello folder structure on hello workstation for saving hello HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'hello file, ' $hqlLocalFileName ', exists.  Do you want toooverwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create hello folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write hello Hive script into a local file
    Write-Host "`nWriting hello Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload hello Hive script toohello default Blob container
    Write-Host "`nUploading hello Hive script toohello default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload hello file from local workstation tooBlob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

    Här följer hello variabler i hello skript:

   * **$hqlLocalFileName** -hello skript sparar hello HiveQL skriptfilen lokalt innan den skickas tooBlob lagring. Detta är hello filnamn. hello standardvärdet är <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -hello HiveQL-skript blob filnamn används i hello Azure Blob storage. hello standardvärdet är tutorials/flightdelay/flightdelays.hql. Eftersom hello fil ska skrivas direkt tooAzure Blob storage, finns det inte en ”/” hello början av hello blob-namnet. Om du vill att tooaccess hello filen från Blob storage måste tooadd ”/” hello början av hello filnamn.
   * **$srcDataFolder** och **$dstDataFolder** -= ”data-självstudier/flightdelay” = ”självstudier/flightdelay/utdata”

- - -
## <a id="appendix-c"></a>Bilaga C – förbereda en Azure SQL database för hello Sqoop jobbutdata
**tooprepare hello SQL-databas (sammanfoga det med hello Sqoop skript)**

1. Förbered hello parametrar:

    <table border="1">
    <tr><th>Variabelnamn</th><th>Anteckningar</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>hello namnet på hello Azure SQL database-server. Ange inget toocreate en ny server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>hello inloggningsnamnet för hello Azure SQL database-server. Om $sqlDatabaseServerName är en befintlig server, är hello inloggningsnamn och inloggningslösenord används tooauthenticate med hello-servern. De är annars används toocreate en ny server.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>hello inloggningslösenordet för hello Azure SQL database-servern.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Det här värdet används endast när du skapar en ny Azure databasserver.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>hello SQL-databasen används toocreate hello AvgDelays tabellen för hello Sqoop jobb. Lämna det tomt skapar en databas som heter HDISqoop. hello tabellnamnet för hello Sqoop jobbutdata är AvgDelays. </td></tr>
    </table>
2. Öppna Azure PowerShell ISE.
3. Kopiera och klistra in följande skript i hello Skriptfönster hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello region toocreate hello Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify hello database name if you have one created. Otherwise use "" toohave hello script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command toocreate hello AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > hello-skript som använder en representational tillståndstjänsten för transfer (REST), http://bot.whatismyipaddress.com, tooretrieve extern IP-adress. hello IP-adress används för att skapa en brandväggsregel för SQL-databasservern.

    Här följer några variabler som används i hello skript:

   * **$ipAddressRestService** -hello standardvärdet är http://bot.whatismyipaddress.com. Det är en offentlig IP-adress REST-tjänst för att hämta extern IP-adress. Du kan använda andra tjänster om du vill. hello extern IP-adress som hämtats via hello-tjänsten kommer att använda toocreate en brandväggsregel för din Azure SQL database-server så att du kan komma åt hello databasen från din arbetsstation (med hjälp av Windows PowerShell-skript).
   * **$fireWallRuleName** -är hello namnet på hello brandväggsregel för hello Azure SQL database-servern. hello standardnamnet är <u>FlightDelay</u>. Du kan byta namn på den om du vill.
   * **$sqlDatabaseMaxSizeGB** -det här värdet används endast när du skapar en ny Azure SQL database-server. hello standardvärdet är 10GB. 10GB är tillräcklig för den här kursen.
   * **$sqlDatabaseName** -det här värdet används endast när du skapar en ny Azure SQL-databas. hello standardvärdet är HDISqoop. Om du byter namn måste du uppdatera hello Sqoop Windows PowerShell-skript i enlighet med detta.
4. Tryck på **F5** toorun hello skript.
5. Validera hello utdata för skriptet. Kontrollera att hello skriptet har körts.

## <a id="nextsteps"></a>Nästa steg
Nu när du förstår hur tooupload fil-tooAzure Blob storage, hur toopopulate en Hive-tabell med hello data från Azure Blob storage hur toorun Hive frågor, och hur toouse Sqoop tooexport data från HDFS tooan Azure SQL-databas. toolearn se fler hello följande artiklar:

* [Komma igång med HDInsight][hdinsight-get-started]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Oozie med HDInsight][hdinsight-use-oozie]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-mapreduce]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
