---
title: "aaaBuild din första data factory (REST) | Microsoft Docs"
description: "I den här självstudiekursen ska du skapa en Azure Data Factory-exempelpipeline med hjälp av REST-API:et för Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Självstudier: Skapa din första Azure-datafabrik med hjälp av REST-API:et för Data Factory
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


I den här artikeln använder du Data Factory REST API: et toocreate din första Azure data factory. toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.

hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata. hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.

> [!NOTE]
> Den här artikeln täcker inte alla hello REST API. Omfattande dokumentation om REST API finns i [referensen för REST-API för Data Factory](/rest/api/datafactory/).
> 
> En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).


## <a name="prerequisites"></a>Krav
* Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.
* Installera [Curl](https://curl.haxx.se/dlwiz/) på din dator. Verktyget hello CURL med övriga kommandon toocreate en datafabrik.
* Gör följande genom att följa anvisningarna i [den här artikeln](../azure-resource-manager/resource-group-create-service-principal-portal.md):
  1. Skapa en webbapp med namnet **ADFGetStartedApp** i Azure Active Directory.
  2. Hämta ett **klient-ID** och en **hemlig nyckel**.
  3. Hämta ett **klientorganisations-ID**.
  4. Tilldela hello **ADFGetStartedApp** programmet toohello **Data Factory deltagare** roll.
* [Installera Azure PowerShell](/powershell/azure/overview).
* Starta **PowerShell** och hello kör följande kommando. Behåll Azure PowerShell öppen tills hello slutet av den här kursen. Om du stänga och öppna måste toorun hello kommandon igen.
  1. Kör **Login-AzureRmAccount** och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.
  2. Kör **Get-AzureRmSubscription** tooview alla hello prenumerationer för det här kontot.
  3. Kör **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** tooselect hello prenumeration som du vill toowork med. Ersätt **NameOfAzureSubscription** med hello namnet på din Azure-prenumeration.
* Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup. Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.

## <a name="create-json-definitions"></a>Skapa JSON-definitioner
Skapa följande JSON-filer i hello mapp där curl.exe finns.

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Namnet måste vara globalt unika, så du kanske vill tooprefix-suffix ADFCopyTutorialDF toomake den ett unikt namn.
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Ersätt **accountname** och **accountkey** med namnet och nyckeln för ditt Azure-lagringskonto. toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
>
>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.json

```JSON
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

| Egenskap | Beskrivning |
|:--- |:--- |
| ClusterSize |Storleken på hello HDInsight-kluster. |
| TimeToLive |Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort. |
| linkedServiceName |Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight |

Observera följande punkter hello:

* hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello ovan JSON. Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
* Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran. Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.
* Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång en sektor bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.

    Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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

hello JSON definierar en datauppsättning med namnet **AzureBlobInput**, som representerar indata för en aktivitet i hello pipeline. Dessutom det anger att hello indata finns i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **inputdata**.

hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

| Egenskap | Beskrivning |
|:--- |:--- |
| typ |hello anges typ egenskapen tooAzureBlob eftersom data finns i Azure blob storage. |
| linkedServiceName |refererar toohello StorageLinkedService som du skapade tidigare. |
| fileName |Den här egenskapen är valfri. Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats. I det här fallet bearbetas endast hello input.log. |
| typ |hello-loggfiler finns i textformat, så vi använder TextFormat. |
| columnDelimiter |kolumner i hello loggfiler är avgränsade med semikolon kommatecken () |
| frekvens/intervall |frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad. |
| extern |den här egenskapen anges tootrue om hello indata inte genereras av hello Data Factory-tjänsten. |

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        }
    }
}
```

hello JSON definierar en datauppsättning med namnet **AzureBlobOutput**, som representerar utdata för en aktivitet i hello pipeline. Dessutom kan det anger att hello resultat lagras i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **partitioneddata**. Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.

### <a name="pipelinejson"></a>pipeline.json
> [!IMPORTANT]
> Ersätt **storageaccountname** med namnet på ditt Azure-lagringskonto.
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "policy": {
                "concurrency": 1,
                "retry": 3
            },
            "scheduler": {
                "frequency": "Month",
                "interval": 1
            },
            "name": "RunSampleHiveActivity",
            "linkedServiceName": "HDInsightOnDemandLinkedService"
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

Hello JSON kodutdrag skapar du en pipeline som består av en enskild aktivitet som använder Hive tooprocess data på ett HDInsight-kluster.

hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **StorageLinkedService**), och i **skript**  mapp i hello behållaren **adfgetstarted**.

Hej **definierar** avsnittet anger körningsinställningar som skickas toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.

I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [!NOTE]
> Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i föregående exempel hello.
>
>

## <a name="set-global-variables"></a>Ange globala variabler
Kör följande kommandon när du ersätter hello värdena med dina egna hello i Azure PowerShell:

> [!IMPORTANT]
> Anvisningar för hur du hämtar klient-ID, klienthemlighet, klientorganisations-ID och prenumerations-ID finns i [kravavsnittet](#prerequisites).
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a>Autentisera med AAD

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a>Skapa en datafabrik
I det här steget ska du skapa en Azure Data Factory-fabrik med namnet **FirstDataFactoryREST**. En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform data. Kör följande kommandon toocreate hello data factory hello:

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    Bekräfta det hello-namnet i hello data factory som du anger här (ADFCopyTutorialDF) matchar hello namnet som angetts i hello **datafactory.json**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datafabriken har skapats visas hello JSON för hello data factory i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```

Observera följande punkter hello:

* hello namnet på hello Azure Data Factory måste vara globalt unika. Om du ser hello fel i resultat: **datafabriksnamnet ”FirstDataFactoryREST” är inte tillgänglig**, hello följande steg:
  1. Ändra hello namn (till exempel yournameFirstDataFactoryREST) i hello **datafactory.json** fil. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
  2. I hello första kommandot där hello **$cmd** variabeln har tilldelats ett värde, Ersätt FirstDataFactoryREST med hello nytt namn och kör hello-kommando.
  3. Kör hello följande två kommandon tooinvoke hello REST API toocreate hello data factory och Skriv ut hello resultaten av hello igen.
* toocreate Data Factory instanser måste toobe deltagare/administratören av hello Azure-prenumeration
* hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.
* Om felmeddelandet hello ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**”, gör du något av följande hello och försök att publicera igen:

  * Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

      Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad:
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen. Den här åtgärden registrerar automatiskt hello-providern för dig.

Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först. Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent i länkade datalager.

## <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory. hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet. hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet.

### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure Storage-konto tooyour data factory. Med den här självstudiekursen kommer du använda hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a>Skapa en Azure HDInsight-länkad tjänst
I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory. Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt. Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran. Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning. De här datauppsättningarna finns toohello **StorageLinkedService** du har skapat tidigare i den här kursen. Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.

### <a name="create-input-dataset"></a>Skapa indatauppsättning
I det här steget skapar du hello inkommande dataset toorepresent inkommande data som lagras i hello Azure Blob storage.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
I det här steget skapar du hello utdata dataset toorepresent utdata data som lagras i hello Azure Blob storage.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet. Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly. hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha. Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.

Bekräfta att du ser hello **input.log** filen i hello **adfgetstarted/inputdata** mapp i hello Azure blob storage och kör hello efter kommandot toodeploy hello pipeline. Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell
    Write-Host $results
    ```
4. Grattis, du har skapat din första pipeline med Azure PowerShell!

## <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget kan använda du Data Factory REST API: et toomonitor segment som produceras av hello pipeline.

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter). Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.
>
>

Kör hello Invoke-Command och hello nästa tills du ser hello sektor i **klar** tillstånd eller **misslyckades** tillstånd. När hello sektorn är i tillståndet Ready, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.  hello skapandet av ett HDInsight-kluster på begäran tar vanligtvis en stund.

![utdata](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> hello indatafilen hämtar bort när hello segment har bearbetats. Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.
>
>

Du kan också använda Azure portal toomonitor segment och felsöka eventuella problem. Mer information finns i [Övervaka pipelines med hjälp av Azure-portalen](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline).

## <a name="summary"></a>Sammanfattning
I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster. Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:

1. Du skapade en Azure **Data Factory**.
2. Du skapade två **länkade tjänster**:
   1. **Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.
   2. **Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory. Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.
3. Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.
4. Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett Azure HDInsight-kluster på begäran. hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure Blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Se även
| Avsnitt | Beskrivning |
|:--- |:--- |
| [Referens för REST-API:et för Data Factory](/rest/api/datafactory/) |Se den omfattande dokumentationen för Data Factory-cmdletar |
| [Pipelines](data-factory-create-pipelines.md) |Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag. |
| [Datauppsättningar](data-factory-create-datasets.md) |I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory. |
| [Schemaläggning och körning](data-factory-scheduling-and-execution.md) |Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell. |
| [Övervaka och hantera pipelines med övervakningsappen](data-factory-monitor-manage-app.md) |Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen. |
