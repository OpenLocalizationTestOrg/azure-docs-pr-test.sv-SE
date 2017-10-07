---
title: "aaaBuild din första data factory (PowerShell) | Microsoft Docs"
description: "I den här självstudien skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av Azure PowerShell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a>Självstudier: Skapa din första Azure-datafabrik med Azure PowerShell
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

I den här artikeln använder du Azure PowerShell toocreate din första Azure data factory. toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.

hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata. hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider. 

> [!NOTE]
> hello data pipeline i den här handledningen omvandlar indata tooproduce utdata. Den kopierar inte data från en källa data store tooa målarkiv data. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).

## <a name="prerequisites"></a>Krav
* Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.
* Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn.
* (valfritt) Den här artikeln täcker inte alla hello Data Factory-cmdletar. Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om Data Factory-cmdletar.

## <a name="create-data-factory"></a>Skapa en datafabrik
I det här steget kan du använda Azure PowerShell toocreate ett Azure Data Factory med namnet **FirstDataFactoryPSH**. En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel indata en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform. Låt oss börja med att skapa hello data factory i det här steget.

1. Starta Azure PowerShell och kör följande kommando hello. Behåll Azure PowerShell öppen tills hello slutet av den här kursen. Om du stänga och öppna måste toorun kommandona igen.
   * Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * Hello kör följande kommando tooselect hello prenumeration som du vill toowork med. Den här prenumerationen bör hello samtidigt som hello något du använde i hello Azure-portalen.
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando hello:
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup. Om du använder en annan resursgrupp måste toouse den i stället för ADFTutorialResourceGroup i den här självstudiekursen.
3. Kör hello **ny AzureRmDataFactory** cmdlet som skapar en datafabrik som heter **FirstDataFactoryPSH**.

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
Observera följande punkter hello:

* hello namnet på hello Azure Data Factory måste vara globalt unika. Om felmeddelandet hello **datafabriksnamnet ”FirstDataFactoryPSH” är inte tillgänglig**, ändra hello namn (till exempel yournameFirstDataFactoryPSH). Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
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

Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först. Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent in-/ utdata i länkade datalager och sedan skapa hello pipeline med en aktivitet som använder dessa data.

## <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory. hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet. hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet. Identifiera vilka data store/beräkning tjänster används i din situation och länka dessa tjänster toohello data factory genom att skapa länkade tjänster.

### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure Storage-konto tooyour data factory. Du använder hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.

1. Skapa en JSON-fil som heter StorageLinkedService.json i hello C:\ADFGetStarted mappen med hello följande innehåll. Skapa hello mapp ADFGetStarted om den inte redan finns.

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }
    ```
    Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto. toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
2. Växla toohello ADFGetStarted mapp i Azure PowerShell.
3. Du kan använda hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar en länkad tjänst. Denna cmdlet och andra Data Factory-cmdletar som du använder i den här kursen kräver att du toopass värden för hello *ResourceGroupName* och *DataFactoryName* parametrar. Du kan också använda **Get-AzureRmDataFactory** tooget en **DataFactory** objekt och skickar hello objekt utan att ange *ResourceGroupName* och  *DataFactoryName* varje gång du kör en cmdlet. Hello kör följande kommando tooassign hello utdata från hello **Get-AzureRmDataFactory** cmdlet tooa **$df** variabeln.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. Kör nu hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar hello länkade **StorageLinkedService** service.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    Om du inte kör hello **Get-AzureRmDataFactory** cmdlet och tilldelade hello utdata toohello **$df** variabel, skulle det ha toospecify värden för hello *ResourceGroupName*och *DataFactoryName* parametrar på följande sätt.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    Om du stänger Azure PowerShell hello mitten av hello kursen har du toorun hello **Get-AzureRmDataFactory** cmdlet nästa gång du startar Azure PowerShell toocomplete hello kursen.

### <a name="create-azure-hdinsight-linked-service"></a>Skapa en Azure HDInsight-länkad tjänst
I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory. Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt. Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran. Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.

1. Skapa en JSON-fil med namnet **HDInsightOnDemandLinkedService**JSON i hello **C:\ADFGetStarted** mapp med hello följande innehåll.

    ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

   | Egenskap | Beskrivning |
   |:--- |:--- |
   | ClusterSize |Anger hello storleken på hello HDInsight-kluster. |
   | TimeToLive |Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort. |
   | linkedServiceName |Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight |

    Observera följande punkter hello:

   * hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello JSON. Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
   * Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran. Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.
   * Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (**timeToLive**). hello klustret tas bort automatiskt när hello bearbetningen är klar.

       Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

     Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
2. Kör hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar hello länkade tjänsten som kallas HDInsightOnDemandLinkedService.
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a>Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning. De här datauppsättningarna finns toohello **StorageLinkedService** du har skapat tidigare i den här kursen. Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.

### <a name="create-input-dataset"></a>Skapa indatauppsättning
1. Skapa en JSON-fil med namnet **InputTable.json** i hello **C:\ADFGetStarted** mapp med hello följande innehåll:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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
   | columnDelimiter |kolumner i hello loggfiler är avgränsade med hello kommatecken (,). |
   | frekvens/intervall |frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad. |
   | extern |den här egenskapen anges tootrue om hello indata inte genereras av hello Data Factory-tjänsten. |
2. Kör följande kommando i Azure PowerShell toocreate hello Data Factory dataset hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
Nu kan skapa du hello utdata dataset toorepresent hello utgående data som lagras i hello Azure Blob storage.

1. Skapa en JSON-fil med namnet **OutputTable.json** i hello **C:\ADFGetStarted** mapp med hello följande innehåll:

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
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
2. Kör följande kommando i Azure PowerShell toocreate hello Data Factory dataset hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet. Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly. hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha. Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset. hello-egenskaper som används i följande JSON hello beskrivs hello slutet av det här avsnittet.

1. Skapa en JSON-fil som heter MyFirstPipelinePSH.json i hello C:\ADFGetStarted mappen med hello följande innehåll:

   > [!IMPORTANT]
   > Ersätt **storageaccountname** med hello namnet på ditt lagringskonto i hello JSON.
   >
   >

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
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
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```
    Hello JSON kodutdrag skapar du en pipeline som består av en enskild aktivitet som använder Hive tooprocess Data på ett HDInsight-kluster.

    hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **StorageLinkedService**), och i **skript**  mapp i hello behållaren **adfgetstarted**.

    Hej **definierar** avsnittet är används toospecify hello runtime inställningar som ska skickas toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

    Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.

    I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i hello exempel.

2. Bekräfta att du ser hello **input.log** filen i hello **adfgetstarted/inputdata** mapp i hello Azure blob storage och kör hello efter kommandot toodeploy hello pipeline. Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. Grattis, du har skapat din första pipeline med Azure PowerShell!

## <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget använder du Azure PowerShell toomonitor vad som händer i ett Azure data factory.

1. Kör **Get-AzureRmDataFactory** och tilldela hello utdata tooa **$df** variabeln.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. Kör **Get-AzureRmDataFactorySlice** tooget information om alla segment av hello **EmpSQLTable**, vilket är hello utdatatabellen för hello pipeline.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    Observera att hello StartDateTime som du anger här hello samma starttid har angetts i hello pipeline-JSON. Här är exempel på utdata från hello:

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Kör **Get-AzureRmDataFactoryRun** tooget hello information om aktivitet körs för ett visst segment.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    Här är exempel på utdata från hello: 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    Du kan hålla köra denna cmdlet tills du ser hello sektor i **klar** tillstånd eller **misslyckades** tillstånd. När hello sektorn är i tillståndet Ready, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.  Det kan ta lite längre tid att skapa ett HDInsight-kluster på begäran.

    ![utdata](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter). Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.
>
> hello indatafilen hämtar bort när hello segment har bearbetats. Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.
>
>

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
| [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) |Se den omfattande dokumentationen för Data Factory-cmdletar |
| [Pipelines](data-factory-create-pipelines.md) |Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag. |
| [Datauppsättningar](data-factory-create-datasets.md) |I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory. |
| [Schemaläggning och körning](data-factory-scheduling-and-execution.md) |Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell. |
| [Övervaka och hantera pipelines med övervakningsappen](data-factory-monitor-manage-app.md) |Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen. |
