---
title: aaaRun Apache Sqoop jobb med Azure HDInsight (Hadoop) | Microsoft Docs
description: "Lär dig hur toouse Azure PowerShell från en arbetsstation toorun Sqoop importera och exportera mellan ett Hadoop-kluster och en Azure SQL database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a>Använda Sqoop med Hadoop i HDInsight
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Lär dig hur toouse Sqoop i HDInsight tooimport och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas.

Även om Hadoop är en fysisk val för bearbetning av Ostrukturerade och halvstrukturerade data, till exempel loggar och filer, kan det också vara en behovet tooprocess strukturerade data som lagras i relationsdatabaser.

[Sqoop] [ sqoop-user-guide-1.4.4] är ett verktyg tootransfer data mellan Hadoop-kluster och relationsdatabaser. Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS) som SQL Server, MySQL eller Oracle till hello Hadoop distributed file system (HDFS), transformera hello data i Hadoop med MapReduce eller Hive och sedan exportera hello data tillbaka till en RDBMS. I kursen får använder du en SQL Server-databas för dina relationsdatabas.

Sqoop-versioner som stöds på HDInsight-kluster finns [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight?][hdinsight-versions]

## <a name="understand-hello-scenario"></a>Förstå hello scenario

HDInsight-kluster levereras med exempeldata. Du kan använda följande två samplingarna hello:

* En loggfil för log4j som finns på */example/data/sample.log*. hello följande loggar extraheras från hello-filen:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* En Hive-tabell med namnet *hivesampletable*, vilket referenser hello datafilen på */hive/warehouse/hivesampletable*. hello tabellen innehåller vissa mobila enheter. 
  
  | Fält | Datatyp |
  | --- | --- |
  | clientid |Sträng |
  | querytime |Sträng |
  | marknaden |Sträng |
  | deviceplatform |Sträng |
  | devicemake |Sträng |
  | devicemodel |Sträng |
  | state |Sträng |
  | Land |Sträng |
  | querydwelltime |dubbla |
  | sessions-ID |bigint |
  | sessionpagevieworder |bigint |

Först måste du exportera *sample.log* och *hivesampletable* toohello Azure SQL database eller tooSQL Server och sedan importera hello tabell som innehåller hello mobil enhetsdata tillbaka tooHDInsight med hjälp av hello följande sökväg:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Skapa kluster och SQL-databas
Det här avsnittet visar hur toocreate ett kluster, en SQL-databas och hello SQL-databas scheman för körs hello självstudiekurs om att använda hello Azure-portalen och en Azure Resource Manager-mall. hello-mallen finns i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). hello Resource Manager-mall anropar en bacpac paketet toodeploy hello tabell scheman tooSQL databas.  Hej bacpac paketet finns i den offentliga blob-behållaren https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Om du vill toouse privata behållare för hello bacpac filer, använder du följande värden i hello mallen hello:
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

Om du föredrar toouse Azure PowerShell toocreate hello klustret och hello SQL-databas finns [bilaga A](#appendix-a---a-powershell-sample).

1. Klicka på hello följande bild tooopen en Resource Manager-mall i hello Azure-portalen.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. Ange hello följande egenskaper:

    - **Prenumerationen**: Ange din Azure-prenumeration.
    - **Resursgruppen**: skapa en ny Azure-resursgrupp eller välj en befintlig resursgrupp.  En resursgrupp är för hantering av ändamål.  Det är en behållare för objekt.
    - **Plats**: Välj en region.
    - **Klusternamn**: Ange ett namn för hello Hadoop-kluster.
    - **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är administratör.
    - **SSH-användarnamn och lösenord**.
    - **Databasen i SQL server-inloggningsnamnet och lösenordet**.
    - **_artifacts plats**: Använd hello standardvärdet om du vill toouse egna backpac-filen på en annan plats.
    - **_artifacts plats Sas-Token**: lämna det tomt.
    - **Bacpac filnamn**: Använd hello standardvärdet om du inte vill toouse backpac filen.
     
     hello följande värden är hårdkodad hello variabler under:
     
     | Standard lagringskontonamn | <CluterName>lagra |
     | --- | --- |
     | Azure SQL server-databasnamn |<ClusterName>dbserver |
     | Azure SQL-databasnamn |<ClusterName>DB |
     
     Skriv ned dessa värden.  Du behöver dem senare i självstudiekursen hello.

3 Klicka på **OK** toosave hello parametrar.

4 från hello **anpassad distribution** bladet, klickar du på **resursgruppen** nedrullningsbara rutan och klicka på **ny** toocreate en ny resursgrupp. hello resursgrupp är en behållare som grupperar hello klustret, hello beroende lagringskontot och andra länkade resurser.

5. Klicka på **Juridiska villkor** och sedan på **Skapa**.

6. Klicka på **Skapa**. Du ser en ny panel med rubriken skicka distribution för malldistribution. Det tar cirka 20 minuter toocreate hello klustret och SQL-databas.

Om du väljer toouse befintliga Azure SQL-databas eller Microsoft SQL Server

* **Azure SQL database**: du måste konfigurera en brandväggsregel för hello Azure SQL database server tooallow åtkomst från din arbetsstation. Instruktioner om hur du skapar en Azure SQL database och konfigurerar hello brandväggen finns [komma igång med Azure SQL database][sqldatabase-get-started]. 
  
  > [!NOTE]
  > Som standard tillåter en Azure SQL database anslutningar från Azure-tjänster, till exempel Azure HDInsight. Om brandväggsinställningen är inaktiverad måste tooenable från hello Azure-portalen. Instruktioner om hur du skapar en Azure SQL database och konfigurera brandväggsregler, se [skapa och konfigurera SQL-databas][sqldatabase-create-configue].
  > 
  > 
* **SQL Server**: om HDInsight-kluster på hello samma virtuella nätverk i Azure som SQL Server, kan du använda hello stegen i den här artikeln tooimport och exportera data tooa SQL Server-databas.
  
  > [!NOTE]
  > HDInsight stöder plats-baserade virtuella nätverk endast och den för närvarande fungerar inte med tillhörighetsgrupp-baserade virtuella nätverk.
  > 
  > 
  
  * toocreate och konfigurera ett virtuellt nätverk, se [skapa ett virtuellt nätverk med hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    
    * När du använder SQL Server i ditt datacenter, måste du konfigurera hello virtuella nätverk som *plats-till-plats* eller *punkt-till-plats*.
      
      > [!NOTE]
      > För **punkt-till-plats** virtuella nätverk, SQL Server måste köra hello VPN client configuration program, som är tillgängliga från hello **instrumentpanelen** av konfigurationen av virtuella Azure-nätverket.
      > 
      > 
    * När du använder SQL Server på en virtuell Azure-dator, konfiguration av virtuellt nätverk kan användas om hello virtuell dator som värd för SQL Server är en medlem av hello samma virtuella nätverk som HDInsight.
  * toocreate ett HDInsight-kluster på ett virtuellt nätverk finns [skapa Hadoop-kluster i HDInsight med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]
    > SQL Server måste även tillåta autentisering. Du måste använda en SQL Server inloggningen toocomplete hello stegen i den här artikeln.
    > 
    > 

## <a name="run-sqoop-jobs"></a>Kör Sqoop jobb
HDInsight kan köra Sqoop jobb med hjälp av olika metoder. Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.

| **Använd den här** om du vill... | .. .an **interaktiva** shell | ... **batch** bearbetning | ... med detta **klustret operativsystem** | .. .from detta **klientoperativsystem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-use-sqoop-mac-linux.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [.NET SDK för Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |✔ |Linux- eller Windows |Windows (för tillfället) |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |✔ |Linux- eller Windows |Windows |

## <a name="limitations"></a>Begränsningar
* Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.
* Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.

## <a name="next-steps"></a>Nästa steg
Nu har du fått veta hur toouse Sqoop. Det finns fler toolearn:

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda Oozie med HDInsight][hdinsight-use-oozie]: Använd Sqoop åtgärd i ett arbetsflöde för Oozie.
* [Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]: använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.
* [Ladda upp data tooHDInsight][hdinsight-upload-data]: hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.

## <a name="appendix-a---a-powershell-sample"></a>Bilaga A – ett PowerShell-exempel
hello PowerShell-exempel utför hello följande steg:

1. Ansluta tooAzure.
2. Skapa en Azure-resursgrupp. Mer information finns i [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)
3. Skapa en Azure SQL Database-server, en Azure SQL database och två tabeller. 
   
    Om du använder SQL Server i stället använda hello följande instruktioner toocreate hello tabeller:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    hello enklaste sättet tooexamine hello-databasen och tabeller är toouse Visual Studio. hello-databasservern och hello databasen kan granskas med hjälp av hello Azure-portalen.
4. Skapa ett HDInsight-kluster.
   
    tooexamine hello klustret som du kan använda hello Azure-portalen eller Azure PowerShell.
5. Förbearbeta hello källdatafilen.
   
    I kursen får exportera du en log4j loggfil (en avgränsad fil) och en Hive tabell tooan Azure SQL-databas. hello avgränsad fil som kallas */example/data/sample.log*. Tidigare i självstudierna hello såg några exempel på log4j loggar. Hello loggfil, det inte finns några tomma rader och vissa liknande toothese rader:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Det här är bra för andra exempel som använder dessa data, men vi måste ta bort de här undantagen innan vi kan importera till hello Azure SQL database eller SQL Server. Sqoop export misslyckas om det finns en tom sträng eller en rad med ett mindre element än hello antalet fält som definierats i hello Azure SQL database-tabellen. Hej log4jlogs tabellen har 7 fält av typen string.
   
    Den här proceduren skapar en ny fil i hello kluster: tutorials/usesqoop/data/sample.log. tooexamine hello ändrade datafil som du kan använda hello Azure-portalen, ett explorer verktyg för Azure Storage eller Azure PowerShell. [Komma igång med HDInsight] [ hdinsight-get-started] har en kod som exempel för att använda Azure PowerShell toodownload en fil och visa hello innehåll.
6. Exportera en data-filen toohello Azure SQL-databas.
   
    hello källfilen är tutorials/usesqoop/data/sample.log. hello tabellen där hello data är exporterade toois kallas log4jlogs.
   
   > [!NOTE]
   > Förutom informationen i anslutningssträngen, bör hello stegen i det här avsnittet fungera för en Azure SQL database eller SQL Server. Dessa steg har testats med hjälp av hello följande konfiguration:
   > 
   > * **Virtuella Azure-nätverket punkt-till-plats configuration**: ett virtuellt nätverk anslutet hello HDInsight-kluster tooa SQL Server i ett privat datacenter. Se [konfigurerar en punkt-till-plats-VPN i hello hanteringsportalen](../vpn-gateway/vpn-gateway-point-to-site-create.md) för mer information.
   > * **Azure HDInsight 3.1**: se [skapa Hadoop-kluster i HDInsight med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md) information om hur du skapar ett kluster på ett virtuellt nätverk.
   > * **SQL Server 2014**: konfigurerad tooallow autentisering och körs hello VPN-klienten configuration paketet tooconnect på ett säkert sätt toohello virtuellt nätverk.
   > 
   > 
7. Exportera en Hive tabell toohello Azure SQL-databas.
8. Importera hello mobiledata tabell toohello HDInsight-kluster.
   
    tooexamine hello ändrade datafil som du kan använda hello Azure-portalen, ett explorer verktyg för Azure Storage eller Azure PowerShell.  [Komma igång med HDInsight] [ hdinsight-get-started] har en kod exempel om hur du använder Azure PowerShell toodownload en fil och visa hello innehåll.

### <a name="hello-powershell-sample"></a>hello PowerShell-exempel
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()

    $conn.close()

    #endregion


    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

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
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
