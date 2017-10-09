---
title: "aaaCreate på begäran Hadoop-kluster med hjälp av Data Factory - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Skapa på begäran Hadoop-kluster i HDInsight med hjälp av Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Azure Data Factory](../data-factory/data-factory-introduction.md) är en molnbaserad integration datatjänst som samordnar och automatiserar hello flytt och transformering av data. Det kan skapa en HDInsight Hadoop-kluster just-in-time tooprocess ett indata-segment och ta bort hello klustret när hello bearbetningen har slutförts. Några av hello fördelarna med att använda ett HDInsight Hadoop-kluster på begäran är:

- Du endast betala för hello jobbet körs på hello HDInsight Hadoop-kluster (plus en kort konfigurerbara inaktivitetstid). hello fakturering för HDInsight-kluster sker proportionerligt per minut, oavsett om du använder dem eller inte. När du använder en på-begäran länkad HDInsight-tjänst i Data Factory skapas hello kluster på begäran. Och hello kluster tas bort automatiskt när hello jobben har slutförts. Därför kan betalar du bara för hello jobbet körs och hello kort inaktiv tid (time to live-inställning).
- Du kan skapa ett arbetsflöde med Data Factory-pipelinen. Du kan till exempel ha hello pipeline toocopy data från en lokal SQL Server-tooan Azure blob storage, processen hello data genom att köra en Hive-skript och Pig-skriptet på en HDInsight Hadoop-kluster på begäran. Kopiera sedan hello resultatet data tooan Azure SQL Data Warehouse för BI program tooconsume.
- Du kan schemalägga hello arbetsflöde toorun regelbundet (varje timme, varje dag, vecka, månad, etc.).

En datafabrik kan ha en eller flera data pipelines i Azure Data Factory. En data-pipeline har en eller flera aktiviteter. Det finns två typer av aktiviteter: [Data Movement aktiviteter](../data-factory/data-factory-data-movement-activities.md) och [Data Transformation aktiviteter](../data-factory/data-factory-data-transformation-activities.md). Du kan använda dataflytt aktiviteter (för närvarande endast Kopieringsaktivitet) toomove data från en källa data store tooa målarkiv data. Du kan använda data transformation aktiviteter tootransform/bearbetning av data. HDInsight Hive aktivitet är en av hello omvandling av aktiviteter som stöds av Data Factory. Du kan använda hello Hive omvandling aktiviteten i den här självstudiekursen.

Du kan konfigurera en hive aktiviteten toouse egna HDInsight Hadoop-kluster eller en HDInsight Hadoop-kluster på begäran. I den här kursen är hello Hive aktivitet i hello data factory-pipelinen konfigurerade toouse ett HDInsight-kluster på begäran. Därför när hello aktiviteten körs tooprocess en datasektorn är här vad som händer:

1. Ett HDInsight Hadoop-kluster skapas automatiskt för du just-in-time tooprocess hello sektorn.  
2. hello indata bearbetas genom att köra en HiveQL-skript på hello klustret.
3. Hej HDInsight Hadoop-kluster tas bort när hello bearbetningen är klar och hello klustret är inaktiv under hello konfigurerad tidsperiod (timeToLive-inställning). Om hello nästa datasektorn är tillgängliga för bearbetning med i den här timeToLive väntetid är hello samma kluster används tooprocess hello sektorn.  

I den här självstudiekursen utför hello HiveQL-skript som är associerade med hello hive aktiviteten hello följande åtgärder:

1. Skapar en extern tabell refererar hello rådata web loggdata som lagras i ett Azure Blob storage.
2. Partitioner hello rådata per år och månad.
3. Lagrar hello partitionerade data i hello Azure blob storage.

I den här självstudiekursen skapar hello HiveQL-skript som är associerade med hello hive aktiviteten en extern tabell refererar hello rådata web loggdata som lagras i hello Azure Blob Storage. Här följer hello exempel rader för varje månad i hello indatafilen.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Hej HiveQL-skript partitioner hello rådata per år och månad. Den skapar tre utdatamappar baserat på hello tidigare indata. Varje mapp innehåller en fil med poster från varje månad.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

En lista över Data Factory data transformation aktiviteter i tillägg tooHive aktivitet finns [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> För närvarande kan du bara skapa HDInsight-kluster av version 3.2 från Azure Data Factory.

## <a name="prerequisites"></a>Krav
Innan du börjar hello anvisningarna i den här artikeln, måste du ha hello följande objekt:

* [Azure-prenumeration](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Förbereda storage-konto
Du kan använda upp toothree storage-konton i det här scenariot:

- standardkontot för lagring för hello HDInsight-kluster
- lagringskontot för hello indata
- Storage-konto för hello-utdata

toosimplify hello självstudier, använda en storage-konto tooserve hello tre funktioner. hello Azure PowerShell-exempelskript finns i det här avsnittet utför hello följande uppgifter:

1. Logga in tooAzure.
2. Skapa en Azure-resursgrupp.
3. Skapa ett Azure Storage-konto.
4. Skapa en blobbbehållare i hello storage-konto
5. Kopiera hello följande två filer toohello Blob-behållare:

   * Indatafilen: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * HiveQL-skript: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Filer som lagras i en offentlig Blob-behållare.


**tooprepare hello lagring och kopiera hello filer med hjälp av Azure PowerShell:**
> [!IMPORTANT]
> Ange namn för hello Azure-resursgrupp och hello Azure storage-konto som skapas av hello-skriptet.
> Skriv ned **resursgruppnamn**, **lagringskontonamnet**, och **lagringskontonyckel** utdata av hello skript. Du behöver dem i hello nästa avsnitt.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

Om du behöver hjälp med hello PowerShell-skript finns [Using hello Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md). Om du toouse Azure CLI Läs hello [bilaga](#appendix) avsnittet hello Azure CLI-skript.

**tooexamine hello storage-konto och hello innehållet**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **resursgrupper** hello vänster.
3. Dubbelklicka på hello resursgruppens namn du skapade i PowerShell-skript. Använda hello filtret om du har för många resursgrupper i listan.
4. På hello **resurser** sida vid sida, har du en resurs i listan om du delar hello resursgrupp med andra projekt. Den här resursen är hello lagringskonto med hello namn du angav tidigare. Klicka på hello lagringskontonamn.
5. Klicka på hello **Blobbar** paneler.
6. Klicka på hello **adfgetstarted** behållare. Du ser två mappar: **inputdata** och **skriptet**.
7. Öppna mappen hello och kontrollera hello filer i hello mappar. Hej inputdata innehåller hello input.log filen med indata hello skript mappen innehåller hello HiveQL skriptfilen.

## <a name="create-a-data-factory-using-resource-manager-template"></a>Skapa en datafabrik med hjälp av Resource Manager-mall
Med hello lagringskonto, hello indata och hello förberedd HiveQL-skript, är du redo toocreate ett Azure data factory. Det finns flera metoder för att skapa datafabriken. I kursen får skapa du en datafabrik genom att distribuera en Azure Resource Manager-mallen med hjälp av hello Azure-portalen. Du kan också distribuera en Resource Manager-mall med hjälp av [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) och [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template). Andra data factory-metoder, se [Självstudier: skapa din första data factory](../data-factory/data-factory-build-your-first-pipeline.md).

1. Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen. hello-mallen finns på https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json. Se hello [Data Factory-entiteter i hello mallen](#data-factory-entities-in-the-template) finns detaljerad information om enheter som har definierats i mallen för hello. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Välj **Använd befintliga** alternativ för hello **resursgruppen** inställningen och välj hello namnet på hello resursgrupp som du skapade i föregående steg i hello (med PowerShell-skript).
3. Ange ett namn för hello data factory (**Datafabriksnamnet**). Det här namnet måste vara globalt unika.
4. Ange hello **lagringskontonamnet** och **lagringskontonyckel** du noterade i hello föregående steg.
5. Välj **jag accepterar villkoren toohello** anges ovan när du har läst **villkor**.
6. Välj **PIN-kod toodashboard** alternativet.
6. Klicka på **inköp/skapa**. Du ser en panel på instrumentpanelen kallas hello **skicka malldistribution**. Vänta tills hello **resursgruppen** öppnas bladet för resursgruppen. Du kan också klicka på hello sida vid sida med titeln som din grupp namnet tooopen hello resurs blad för resursgrupp.
6. Klicka på hello panelen tooopen hello resursgrupp om hello blad för resursgrupp inte redan är öppen. Nu visas en mer data factory-resurs visas dessutom toohello lagringsresurs för kontot.
7. Klicka på hello namnet på din data factory (värdet du angav för hello **Datafabriksnamnet** parametern).
8. Hello Data Factory-bladet, klickar du på hello **Diagram** panelen. hello diagram visar en aktivitet med en inkommande datauppsättning och en datamängd för utdata:

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline diagram](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    hello namn har definierats i hello Resource Manager-mall.
9. Dubbelklicka på **AzureBlobOutput**.
10. På hello **senaste uppdaterade segment**, visas ett segment. Om hello status är **pågår**, vänta tills den har ändrats för**klar**. Det tar vanligtvis om **20 minuter** toocreate ett HDInsight-kluster.

### <a name="check-hello-data-factory-output"></a>Kontrollera hello data factory-utdata

1. Använd hello samma procedur i hello senaste sessionen toocheck hello behållare för hello adfgetstarted behållare. Det finns två nya behållare dessutom för**adfgetsarted**:

   * En behållare med namn som följer hello mönster: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Den här behållaren är hello standardbehållaren för hello HDInsight-kluster.
   * adfjobs: den här behållaren är hello behållare för hello ADF jobbloggar.

     hello data factory utdata lagras i **afgetstarted** som du konfigurerade i hello Resource Manager-mall.
2. Klicka på **adfgetstarted**.
3. Dubbelklicka på **partitioneddata**. Du ser en **år 2014 =** mappen eftersom alla hello webbloggar funktionsdokumentationen år 2014.

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Om du detaljnivån hello lista visas tre mappar för januari, februari och mars. Och det finns en logg för varje månad.

    ![Azure Data Factory HDInsight på begäran Hive aktivitet pipeline utdata](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a>Data Factory-entiteter i hello mall
Här är att hello översta Resource Manager-mall för en datafabrik som ser ut som:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a>Definiera en datafabrik
Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
Hej dataFactoryName är hello datafabriken som du anger när du distribuerar mallen hello hello namn. Datafabriken är för närvarande stöds endast i hello östra USA, västra USA och Norra Europa regioner.

### <a name="defining-entities-within-hello-data-factory"></a>Definiera enheter inom hello data factory
hello har följande Data Factory-enheter definierats i hello JSON-mall:

* [Länkad Azure Storage-tjänst](#azure-storage-linked-service)
* [Länkad tjänst för HDInsight på begäran](#hdinsight-on-demand-linked-service)
* [Indatauppsättning för Azure-blobb](#azure-blob-input-dataset)
* [Utdatauppsättning för Azure-blobb](#azure-blob-output-dataset)
* [Datapipeline med en kopieringsaktivitet](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
hello Azure Storage länkade tjänsten länkar till din Azure storage-konto toohello data factory. I den här självstudiekursen används hello samma lagringskonto som hello HDInsight standardkontot för lagring, lagring av indata och utdata datalagring. Därför kan definiera du endast en Azure Storage länkade tjänsten. I hello länkade tjänstdefinitionen ange hello namn och nyckel för Azure storage-konto. Se [Azure länkade lagringstjänsten](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
Hej **connectionString** använder hello storageAccountName och storageAccountKey parametrar. Du anger värden för dessa parametrar när du distribuerar hello mall.  

#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)
Hello på begäran HDInsight länkade tjänstdefinitionen kan du ange värden för konfigurationsparametrar som används av hello Data Factory-tjänsten toocreate en HDInsight Hadoop-kluster vid körning. Se [Compute länkade tjänster](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikeln för information om toodefine för JSON-egenskaper som används för en länkad tjänst för HDInsight-på begäran.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Observera följande punkter hello:

* hello Data Factory skapar en **Linux-baserade** HDInsight-kluster.
* Hej HDInsight Hadoop-kluster skapas i hello samma region som hello storage-konto.
* Meddelande hello *timeToLive* inställningen. Hej datafabriken tar bort hello klustret automatiskt när hello klustret är att ha varit inaktiv i 30 minuter.
* Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.

Se [HDInsight-länkad tjänst på begäran](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.

> [!IMPORTANT]
> Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

#### <a name="azure-blob-input-dataset"></a>Indatauppsättning för Azure-blobb
Ange hello namnen på blob-behållare, mapp och fil som innehåller hello indata i hello inkommande datauppsättningsdefinitionen. Se [Azure Blob-egenskaper för datamängd](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

Observera hello efter specifika inställningar i hello JSON-definitionen:

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb
Ange hello namnen på blob-behållaren och mappen som innehåller hello utdata i datauppsättningsdefinitionen för hello utdata. Se [Azure Blob-egenskaper för datamängd](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

hello folderPath anger hello toohello sökväg som innehåller hello utdata:

```json
"folderPath": "adfgetstarted/partitioneddata",
```

Hej [dataset tillgänglighet](../data-factory/data-factory-create-datasets.md#dataset-availability) inställningen är följande:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

Utdata dataset tillgänglighet enheter hello pipeline i Azure Data Factory. I det här exemplet skapas hello sektorn månad på hello sista dagen i månaden (EndOfInterval). Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).

#### <a name="data-pipeline"></a>Datapipeline
Du kan definiera en pipeline som omvandlar data genom att köra Hive-skript på en Azure HDInsight-kluster på begäran. Se [Pipeline-JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet.

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

hello pipelinen innehåller en aktivitet, HDInsightHive aktivitet. Som både start- och slutdatum är i januari 2016 data för endast en månad (ett segment) har bearbetats. Båda *starta* och *end* hello aktivitet har en tidigare tidpunkt, så hello Data Factory bearbetar data i hello månad omedelbart. Om hello end är ett datum i framtiden, skapar hello data factory ett annat segment när hello tid kommer. Mer information finns i [Data Factory schemaläggning och körning av](../data-factory/data-factory-scheduling-and-execution.md).

## <a name="clean-up-hello-tutorial"></a>Rensa hello självstudiekursen

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a>Ta bort hello blob-behållare som skapats av HDInsight-kluster på begäran
Med länkad HDInsight-tjänsten på begäran skapas ett HDInsight-kluster varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (timeToLive) och hello klustret tas bort när hello bearbetningen är klar. För varje kluster skapar Azure Data Factory en blob-behållare i hello Azure blob-lagring som används som hello stroage standardkontot för hello klustret. Även om HDInsight-kluster tas bort hello standard blob storage-behållare och hello associerade lagringskontot tas inte bort. Det här beteendet är avsiktligt. Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Ta bort hello **adfjobs** och **adfyourdatafactoryname-linkedservicename-datetimestamp** mappar. Hej adfjobs behållaren innehåller jobbloggar från Azure Data Factory.

### <a name="delete-hello-resource-group"></a>Ta bort hello resursgruppen
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) är används toodeploy, hantera och övervaka din lösning som en grupp.  Tar bort en resursgrupp alla hello komponenter inuti hello grupp.  

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **resursgrupper** hello vänster.
3. Klicka på hello resursgruppens namn du skapade i PowerShell-skript. Använda hello filtret om du har för många resursgrupper i listan. Hello resursgrupp öppnas i ett nytt blad.
4. På hello **resurser** sida vid sida, har du hello standardkontot för lagring och hello data factory om du delar hello resursgrupp med andra projekt i listan.
5. Klicka på **ta bort** hello längst upp på bladet hello. Detta tar bort hello storage-konto och hello data som lagras i hello storage-konto.
6. Ange hello resurs grupp namnet tooconfirm togs bort och klicka sedan på **ta bort**.

Om du inte vill toodelete hello storage-konto när du tar bort hello resursgrupp, Överväg hello följande arkitektur genom att avgränsa hello affärsdata från hello standardkontot för lagring. I så fall måste du har en resursgrupp för hello storage-konto med hello affärsdata och hello andra resursgruppen för hello standardkontot för lagring för HDInsight länkade tjänsten och hello data factory. När du tar bort hello andra resursgruppen påverkar inte hello business data storage-konto. toodo så:

* Lägg till hello följande toohello översta resursgruppen tillsammans med hello Microsoft.DataFactory/datafactories resurs i Resource Manager-mall. Den skapar ett lagringskonto:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Lägg till ett nytt länkade tjänsten punkt toohello nya lagringskonto:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* Konfigurera hello HDInsight ondemand länkad tjänst med en ytterligare dependsOn och en additionalLinkedServiceNames:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse Azure Data Factory toocreate på begäran HDInsight-kluster tooprocess Hive-jobb. tooread mer:

* [Hadoop-vägledning: komma igång med Linux-baserade Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight-dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Data factory-dokumentation](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Bilaga

### <a name="azure-cli-script"></a>Azure CLI-skript
Du kan använda Azure CLI istället för att använda Azure PowerShell toodo hello kursen. först installera toouse Azure CLI, Azure CLI enligt instruktionerna hello:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a>Använda Azure CLI tooprepare hello lagring och kopiera hello-filer

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

hello behållarens namn är *adfgetstarted*. Se till att den som den är. Annars måste tooupdate hello Resource Manager-mall. Om du behöver hjälp med skriptet CLI, se [Using hello Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).
