---
title: "aaaQuery data från HDFS-kompatibla Azure storage - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooquery data från Azure storage och Azure Data Lake Store toostore resultat av dina analyser."
keywords: blob storage,hdfs,structured data,unstructured data,data lake store,Hadoop input,Hadoop output, hadoop storage, hdfs input,hdfs output,hdfs storage,wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Använda Azure-lagring med Azure HDInsight-kluster

tooanalyze data i HDInsight-kluster, du kan lagra hello data i Azure Storage, Azure Data Lake Store eller båda. Båda lagringsalternativ aktivera toosafely ta bort HDInsight-kluster som används för beräkning utan att förlora användardata.

Hadoop stöder begreppet standardfilsystem hello. Hej standardfilsystemet kräver ett standardschema och utfärdare. Det kan också vara används tooresolve relativa sökvägar. Du kan ange en blob-behållare i Azure Storage som hello standardfilsystem under hello processen för HDInsight-kluster, eller med HDInsight 3.5, kan du välja Azure Storage eller Azure Data Lake Store som hello standard filer system med några få undantag. Hello när du kan använda Data Lake Store som både hello standard och länkad lagring, se [tillgången för HDInsight-kluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).

I den här artikeln får du lära dig hur Azure Storage fungerar med HDInsight-kluster. toolearn hur Data Lake Store fungerar med HDInsight-kluster finns i [Använd Azure Data Lake Store med Azure HDInsight-kluster](hdinsight-hadoop-use-data-lake-store.md). Mer information om hur du skapar ett HDInsight-kluster finns i [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Skapa Hadoop-kluster i HDInsight).

Azure Storage är en robust lagringslösning för allmänna ändamål som smidigt kan integreras med HDInsight. HDInsight kan du använda en blob-behållare i Azure Storage som hello standardfilsystem för hello-kluster. Via Hadoop distributed file system (HDFS) gränssnittet tillämpas hello fullständig uppsättning komponenter i HDInsight direkt på strukturerade eller Ostrukturerade data som lagras som blobar.

> [!WARNING]
> Det finns flera tillgängliga alternativ när du skapar ett Azure Storage-konto. hello följande tabell innehåller information om vilka alternativ som stöds med HDInsight:
> 
> | Storage Account-typ | Lagringsnivå | Stöds av HDInsight |
> | ------- | ------- | ------- |
> | Allmänna lagringskonton | Standard | __Ja__ |
> | &nbsp; | Premium | Nej |
> | Blob Storage-konto | Frekvent | Nej |
> | &nbsp; | Lågfrekvent | Nej |

Vi rekommenderar inte att du använder hello standardbehållaren för att lagra företagets data. Ta bort hello standardbehållaren efter varje Använd tooreduce är lagringskostnaden bra. Observera att hello standardbehållaren innehåller program- och loggar. Se till att tooretrieve hello loggar innan du tar bort hello behållare.

Det går inte att dela en blobbehållare över flera kluster.

## <a name="hdinsight-storage-architecture"></a>Lagringsarkitekturen i HDInsight
hello följande diagram ger en abstrakt vy av hello lagringsarkitekturen i HDInsight för att använda Azure Storage:

![Hadoop-kluster använder hello HDFS API tooaccess och lagra strukturerade och Ostrukturerade data i Blob storage. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Lagringsarkitekturen i HDInsight")

HDInsight ger åtkomst toohello distribuerade filsystemet som är lokalt anslutna toohello compute-noder. Detta filsystem kan nås med hjälp av hello fullständigt kvalificerade URI, till exempel:

    hdfs://<namenodehost>/<path>

HDInsight kan dessutom tooaccess data som lagras i Azure Storage. hello-syntaxen är:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Här är några saker att tänka på när du använder Azure Storage-konton med HDInsight-kluster.

* **Behållare på hello lagringskonton som är anslutna tooa kluster:** eftersom hello kontonamnet och nyckeln associeras med hello kluster under skapande, har du fullständig åtkomst toohello blobarna i dessa behållare.

* **Offentliga behållare eller offentliga blobar på lagringskonton som inte är ansluten tooa kluster:** du har läsbehörighet toohello blobarna i hello behållare.
  
  > [!NOTE]
  > Offentliga behållare låter dig tooget en lista över alla blobar som är tillgängliga i behållaren samt hämta metadata för behållaren. Offentliga blobar tooaccess hello blobbar endast om du vet hello exakta URL. Mer information finns i <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">begränsa åtkomst toocontainers och blobbar</a>.
  > 
  > 
* **Privata behållare på lagringskonton som inte är ansluten tooa kluster:** hello blobarna i hello behållare kan inte komma åt såvida du inte definierar hello storage-konto när du skickar hello WebHCat-jobb. Detta beskrivs senare i artikeln.

Hej lagringskonton som definieras i hello skapandeprocessen och deras nycklar lagras i %HADOOP_HOME%/conf/core-site.xml i klusternoderna hello. hello standardbeteendet för HDInsight är toouse hello lagringskonton som definieras i hello core-site.xml-filen. Du kan ändra den här inställningen med [Ambari](./hdinsight-hadoop-manage-ambari.md)

Flera WebHCat-jobb, inklusive Hive, MapReduce, Hadoop-strömning och Pig, kan ha med sig en beskrivning av lagringskonton och metadata. (För närvarande fungerar för Pig med lagringskonton, men inte för metadata.) Mer information finns i [Använda ett HDInsight-kluster med alternativa lagringskonton och metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blobar kan användas för strukturerade och ostrukturerade data. I blob-behållare förvaras data som nyckel/värde-par och det finns ingen kataloghierarki. Men hello snedstreck (/) kan användas i hello nyckelnamn toomake visas den som om en fil lagras i en katalogstruktur. Nyckeln för en blob kan till exempel vara *input/log1.txt*. Ingen faktisk *inkommande* katalog finns, men på grund av toohello förekomst av hello snedstreck i nyckelnamnet hello, har hello utseendet på en sökväg till filen.

## <a id="benefits"></a>Fördelar med Azure Storage
hello underförstådd hello sätt hello beräkningsklustren skapas nära toohello lagringskontoresurserna i hello Azure-regionen där hello höghastighetsnätverket gör att hindra kostnaden för inte samordnar beräkningskluster och lagringsresurser effektivt hello datornoderna tooaccess hello data i Azure storage.

Det finns flera fördelar med att lagra hello data i Azure storage i stället för HDFS:

* **Data återanvändning och delning:** hello data i HDFS finns inuti hello beräkningskluster. Endast hello-program som har åtkomst toohello beräkna klustret kan använda hello data med hjälp av HDFS-API: er. hello data i Azure storage kan nås via hello HDFS-API: er eller hello [Blob Storage REST API: er][blob-storage-restAPI]. Därför kan en större grupp program (inklusive andra HDInsight-kluster) och verktyg att använda tooproduce och använda hello data.
* **Dataarkivering:** lagra data i Azure-lagring kan hello HDInsight-kluster som används för beräkning toobe tas bort utan att förlora användardata.
* **Kostnad för datalagring:** lagra data i DFS för hello långsiktiga är dyrare än att lagra hello data i Azure-lagring eftersom hello kostnaden för ett beräkningskluster är högre än hello kostnaden för Azure storage. Dessutom hello data har inte lästs in på nytt för varje generation av beräkningskluster toobe, sparar du kostnader för datainläsning.
* **Elastisk utskalning:** även om HDFS ger dig ett utskalat filsystem, hello skala bestäms av hello antalet noder som du skapar för klustret. Ändra hello skala kan bli en mer komplicerad process än att använda hello elastiska skalningsfunktionerna som du automatiskt i Azure-lagring.
* **Geo-replikering:** Din Azure Storage kan geo-replikeras. Även om detta ger dig geografisk återställning och dataredundans, en växling vid fel toohello geo-replikerade platsen en avsevärd försämring av prestanda och det kan innebära ytterligare kostnader. Vår rekommendation är klokt toochoose hello geo-replikering och bara om hello-värde för hello är värda hello extra kostnad.

Vissa MapReduce-jobb och paket kan skapa mellanresultat som du inte verkligen vill toostore i Azure-lagring. I den fallet kan du välja toostore hello data i hello lokala HDFS. Faktum är att HDInsight använder DFS för flera av dessa mellanresultat i Hive-jobb och andra processer.

> [!NOTE]
> De flesta HDFS-kommandon (till exempel <b>ls</b>, <b>copyFromLocal</b> och <b>mkdir</b>) fungerar fortfarande som förväntat. Endast hello kommandon som är specifika toohello interna HDFS-implementeringen (vilket är refererad tooas DFS), till exempel <b>fschk</b> och <b>dfsadmin</b>, visa olika beteenden i Azure-lagring.
> 
> 

## <a name="create-blob-containers"></a>Skapa blob-behållare
toouse blobbar du först skapa en [Azure Storage-konto][azure-storage-create]. Som en del av det här kan ange du en Azure-region där hello storage-konto har skapats. hello-kluster och hello storage-kontot måste finnas i hello samma region. hello Hive metastore SQL Server-databas och Oozie metastore SQL Server-databas måste även finnas i hello samma region.

Oavsett var den finns tillhör varje blob som du skapar tooa behållare i Azure Storage-konto. Den här behållaren kan vara en befintlig blob som skapats utanför HDInsight eller en behållare som skapats för ett HDInsight-kluster.

hello standardbehållaren lagrar klusterspecifika information, till exempel jobbhistorik och loggar. Låt inte flera HDInsight-kluster dela en standardblob-behållare. Detta kan skada jobbets historik. Det rekommenderas toouse en annan behållare för varje kluster och publicera delade data på ett konto för länkad lagring som anges i distribution av alla relevanta kluster snarare än hello standardkontot för lagring. Mer information om hur du konfigurerar länkade lagringskonton finns i [Skapa HDInsight-kluster][hdinsight-creation]. Du kan emellertid återanvända en standardbehållare för lagring när hello ursprungliga HDInsight-klustret har tagits bort. För HBase-kluster kan du faktiskt behålla hello HBase-tabellschemat och data genom att skapa en ny HBase-kluster med hjälp av hello standardbehållaren som används av ett HBase-kluster som har tagits bort.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen
När du skapar ett HDInsight-kluster från hello Portal kan ha hello alternativ (som visas nedan) tooprovide hello lagringskontouppgifter. Du kan också ange om du vill att ett konto för ytterligare lagringsutrymme som associerats med hello kluster och i så fall, Välj Data Lake Store eller en annan Azure Storage blob som hello ytterligare lagringsutrymme.

![HDInsight hadoop, skapa datakälla](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.


### <a name="use-azure-powershell"></a>Använda Azure PowerShell
Om du [installerat och konfigurerat Azure PowerShell][powershell-install], du kan använda hello följande från hello Azure PowerShell-prompt toocreate ett lagringskonto och en behållare:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a>Använda Azure CLI

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Om du har [installerat och konfigurerat hello Azure CLI](../cli-install-nodejs.md), hello följande kommando kan vara används tooa storage-konto och en behållare.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> Hej `--type` parameter anger hur hello storage-konto replikeras. Mer information finns i [Azure Storage-replikering](../storage/storage-redundancy.md). Använd inte ZRS eftersom ZRS inte stöder sidblobbar, filer, tabeller eller köer.
> 
> 

Du kan ange toospecify hello geografiska region som hello lagringskonto skapas i. Du bör skapa hello storage-konto i hello samma region som du planerar att skapa ditt HDInsight-kluster.

När hello storage-konto har skapats använder du följande kommando tooretrieve hello lagringskontonycklar hello:

    azure storage account keys list <storageaccountname>

toocreate en behållare använder hello följande kommando:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a>Adressera filer i Azure Storage
hello URI-schemat för att komma åt filer i Azure storage från HDInsight är:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

hello URI-schemat ger okrypterad åtkomst (med hello *wasb:* prefix) och SSL-krypterad åtkomst (med *wasbs*). Vi rekommenderar att du använder *wasbs* där det är möjligt, även om det inte att komma åt data som finns i hello samma region i Azure.

Hej &lt;BlobStorageContainerName&gt; identifierar hello namnet på hello blob-behållaren i Azure-lagring.
Hej &lt;StorageAccountName&gt; identifierar hello Azure Storage-kontonamnet. Ett fullständigt kvalificerat domännamn (FQDN) krävs.

Om varken &lt;BlobStorageContainerName&gt; eller &lt;StorageAccountName&gt; har angetts används hello standardfilsystem. För hello filer i filsystemet för hello standard kan använda du en relativ sökväg eller en absolut sökväg. Till exempel hello *hadoop-mapreduce-examples.jar* fil som medföljer HDInsight-kluster kan vara hänvisade tooby med hjälp av en av följande hello:

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> hello-filnamnet är <i>hadoop examples.jar</i> i HDInsight-kluster av version 2.1 och 1.6.
> 
> 

Hej &lt;sökväg&gt; är hello filen eller katalogen HDFS-sökvägsnamn. Eftersom behållare i Azure Storage helt enkelt är lagringsplatser för nyckel-/värdepar finns det inte något faktiskt hierarkiskt filsystem. Ett snedstreck (/) i en blob-nyckel tolkas som en katalogavgränsare. Till exempel hello blob-namnet för *hadoop-mapreduce-examples.jar* är:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> När du arbetar med blobar utanför HDInsight kan de flesta verktyg inte identifiera WASB-formatet hello och förväntar sig i stället ett grundläggande sökvägsformat som `example/jars/hadoop-mapreduce-examples.jar`.
> 
> 

## <a name="access-blobs"></a>Få åtkomst till blobbar 


### <a name="access-blobs-using-azure-powershell"></a>Använda Azure PowerShell
> [!NOTE]
> hello kommandona i det här avsnittet ger grundläggande exempel på hur du använder PowerShell tooaccess data som lagras i BLOB. En mer komplett exempel som är anpassad för att arbeta med HDInsight finns hello [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).
> 
> 

Använd följande kommando toolist hello blob-relaterade cmdlets hello:

    Get-Command *blob*

![Lista över blob-relaterade PowerShell-cmdlets.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Överföra filer
Se [överför data tooHDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Hämta filer
hello hämtar följande skript en block-blob toohello aktuella mappen. Ändra hello directory tooa mapp där du har behörighet att skriva innan köra hello skriptet.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Att erbjuda hello resursgruppens namn och hello klusternamnet, kan du använda hello följande kod:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a>Ta bort filer
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Lista filer
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Köra Hive-frågor med ett odefinierat lagringskonto
Det här exemplet illustrerar hur hello toolist en mapp från lagringskontot som inte har definierats under skapandeprocessen.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a>Använda Azure CLI
Använd följande kommando toolist hello blob-relaterade kommandon hello:

    azure storage blob

**Exempel på användning av Azure CLI tooupload en fil**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Exempel på användning av Azure CLI toodownload en fil**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Exempel på användning av Azure CLI toodelete en fil**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Exempel på användning av Azure CLI toolist filer**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a>Använda ytterligare lagringskonton

När du skapar ett HDInsight-kluster måste ange du hello Azure Storage-konto som du vill tooassociate med den. Dessutom toothis storage-konto, du kan lägga till ytterligare lagringskonton från hello samma Azure-prenumeration eller andra Azure-prenumerationer under skapandeprocessen hello eller ett kluster har skapats. Mer information om hur du lägger till ytterligare lagringskonton finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse HDFS-kompatibla Azure storage med HDInsight. Detta gör att du toobuild skalbara, långsiktiga, arkivering lösningar för datainsamling och använda HDInsight toounlock hello information i hello lagrade strukturerade och Ostrukturerade data.

Mer information finns i:

* [Kom igång med Azure HDInsight][hdinsight-get-started]
* [Kom igång med Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Använda Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata med HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
