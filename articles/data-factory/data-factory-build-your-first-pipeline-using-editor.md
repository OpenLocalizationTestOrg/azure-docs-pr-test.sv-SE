---
title: "aaaBuild din första data factory (Azure portal) | Microsoft Docs"
description: "I den här självstudiekursen skapar du en pipeline för Azure Data Factory av exemplet med Data Factory-redigeraren i hello Azure-portalen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Självstudier: Skapa din första Azure-datafabrik med Azure-portalen
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)


I den här artikeln får du lära dig hur toouse [Azure-portalen](https://portal.azure.com/) toocreate din första Azure data factory. toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan. 

hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata. hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider. 

> [!NOTE]
> hello data pipeline i den här handledningen omvandlar indata tooproduce utdata. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).

## <a name="prerequisites"></a>Krav
1. Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.
2. Den här artikeln innehåller inte en översikt över hello Azure Data Factory-tjänsten. Vi rekommenderar att du går igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel en detaljerad översikt av hello-tjänsten.  

## <a name="create-data-factory"></a>Skapa en datafabrik
En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata. Låt oss börja med att skapa hello data factory i det här steget.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**.

   ![Bladet Skapa](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. I hello **nya datafabriken** bladet ange **GetStartedDF** för hello namn.

   ![Bladet Ny datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > hello hello Azure data factory måste vara **globalt unika**. Om felmeddelandet hello: **datafabriksnamnet ”GetStartedDF” är inte tillgänglig**. Ändra hello namn i hello data factory (till exempel yournameGetStartedDF) och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
   >
   > hello namn i hello data factory får registreras som en **DNS** namn i hello framtiden och därför bli synligt offentligt.
   >
   >
4. Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.
5. Välj befintlig **resursgrupp** eller skapa en resursgrupp. Hello självstudiekurs skapar du en resursgrupp med namnet: **ADFGetStartedRG**.
6. Välj hello **plats** för hello data factory. Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.
7. Välj **PIN-kod toodashboard**. 
8. Klicka på **skapa** på hello **nya data factory** bladet.

   > [!IMPORTANT]
   > toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.
   >
   >
7. Hello instrumentpanelen visas hello följande panelen med status: distribuera data factory.    

   ![Skapar datafabrikstatus](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Grattis! Du har skapat din första datafabrik. När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.     

    ![Bladet Datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Innan du skapar en pipeline i hello data factory, måste toocreate några Data Factory-entiteter först. Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent in-/ utdata i länkade datalager och sedan skapa hello pipeline med en aktivitet som använder dessa data.

## <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory. hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet. hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet. Identifiera vad [datalagret](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) används i din situation och länka dessa tjänster toohello data factory genom att skapa länkade tjänster.  

### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure Storage-konto tooyour data factory. I den här kursen använder du hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.

1. Klicka på **författare och distribuera** på hello **DATAFABRIKEN** bladet för **GetStartedDF**. Du bör se hello Data Factory-redigeraren.

   ![Ikonen Författare och distribution](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Klicka på **Nytt datalager** och välj **Azure-lagring**.

   ![Ny datalagring - Azure Storage - meny](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.

   ![Länkad Azure-lagringstjänst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto. toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

    ![Knappen Distribuera](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   När hello länkade tjänsten har distribuerats korrekt hello **utkast 1** fönstret bör försvinner och du ser **AzureStorageLinkedService** i hello trädvyn hello vänster.

    ![Länkad lagringstjänst i menyn](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Skapa en Azure HDInsight-länkad tjänst
I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory. Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt.

1. I hello **Data Factory-redigeraren**, klickar du på **... Fler**, klicka på **Ny beräkning**, och välj **På begäran HDInsight-kluster**.

    ![Ny beräkning](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopiera och klistra in följande kodutdrag toohello hello **utkast 1** fönster. hello JSON fragment beskriver hello egenskaper används toocreate hello HDInsight-kluster på begäran.

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
   | ClusterSize |Anger hello storleken på hello HDInsight-kluster. |
   | TimeToLive | Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort. |
   | linkedServiceName | Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight. |

    Observera följande punkter hello:

   * hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello JSON. Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
   * Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran. Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.
   * Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (**timeToLive**). hello klustret tas bort automatiskt när hello bearbetningen är klar.

       Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

     Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.

    ![Distribuera på begäran länkad HDInsight-tjänst](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Bekräfta att du ser både **AzureStorageLinkedService** och **HDInsightOnDemandLinkedService** i hello trädvyn hello vänster.

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning. De här datauppsättningarna finns toohello **AzureStorageLinkedService** du har skapat tidigare i den här kursen. Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.   

### <a name="create-input-dataset"></a>Skapa indatauppsättning
1. I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.

    ![Ny datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret. Hello JSON fragment du skapar en datauppsättning som kallas **AzureBlobInput** som representerar indata för en aktivitet i hello pipeline. Dessutom kan du ange att hello indata finns i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **inputdata**.

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
    hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

   | Egenskap | Beskrivning |
   |:--- |:--- |
   | typ |hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage. |
   | linkedServiceName |Refererar toohello **AzureStorageLinkedService** du skapade tidigare. |
   | folderPath | Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar. | 
   | fileName |Den här egenskapen är valfri. Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats. I den här självstudiekursen bara hello **input.log** bearbetas. |
   | typ |hello loggfilerna i textformat, så vi använder **TextFormat**. |
   | columnDelimiter |kolumner i hello loggfiler avgränsas med **kommatecken tecken (`,`)** |
   | frekvens/intervall |Ange frekvensen för**månad** och är **1**, vilket innebär att hello indata segment som är tillgängliga varje månad. |
   | extern | Den här egenskapen anges för**SANT** om hello indata inte genereras av denna pipeline. I den här självstudiekursen genereras hello input.log inte av denna pipeline, så vi ställa in hello egenskapen tootrue. |

    Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello nyskapad dataset. Du bör se hello datauppsättning i hello trädvyn hello vänster.

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
Nu kan skapa du hello utdata dataset toorepresent hello utgående data som lagras i hello Azure Blob storage.

1. I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.  
2. Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret. Hello JSON fragment du skapar en datauppsättning som kallas **AzureBlobOutput**, och ange hello strukturen för hello data som produceras av hello Hive-skript. Dessutom kan du ange att hello resultat lagras i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **partitioneddata**. Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.

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
    Se **skapa hello inkommande dataset** avsnitt beskrivs dessa egenskaper. Du anger inte externa hello-egenskapen på en datamängd för utdata som hello dataset produceras av hello Data Factory-tjänsten.
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello nyskapad dataset.
4. Kontrollera att hello datauppsättningen har skapats.

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet. Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly. hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha. Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset. hello-egenskaper som används i följande JSON hello beskrivs hello slutet av det här avsnittet.

1. I hello **Data Factory-redigeraren**, klickar du på **ellips (...) Fler kommandon** och klicka sedan på **ny pipeline**.

    ![knappen ny pipeline](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret.

   > [!IMPORTANT]
   > Ersätt **storageaccountname** med hello namnet på ditt lagringskonto i hello JSON.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
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

    hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **AzureStorageLinkedService**), och i  **skriptet** mapp i hello behållaren **adfgetstarted**.

    Hej **definierar** avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

    Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.

    I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i hello exempel.
   >
   >
3. Bekräfta hello följande:

   1. **Input.log** filen finns på hello **inputdata** för hello **adfgetstarted** behållare i hello Azure blob-lagring
   2. **partitionweblogs.hql** filen finns på hello **skriptet** för hello **adfgetstarted** behållare i hello Azure blob storage. Fullständig hello nödvändiga steg i hello [kursen översikt](data-factory-build-your-first-pipeline.md) om du inte ser de här filerna.
   3. Bekräfta att du har ersatt **storageaccountname** med hello namnet på ditt lagringskonto i hello pipeline-JSON.
4. Klicka på **distribuera** på hello i kommandofältet toodeploy hello pipeline. Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.
5. Bekräfta att du ser hello pipeline i hello trädvyn.

    ![Trädvy med pipeline](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Grattis, du har skapat din första pipeline!

## <a name="monitor-pipeline"></a>Övervaka pipeline
### <a name="monitor-pipeline-using-diagram-view"></a>Övervaka pipeline med diagramvyn
1. Klicka på **X** tooclose Data Factory-redigeraren blad toonavigate tillbaka toohello Data Factory-bladet och på **Diagram**.

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. I hello diagramvyn visas en översikt över hello pipelines och datauppsättningar som används i den här självstudiekursen.

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. tooview alla aktiviteter i hello pipeline, högerklicka på pipeline i hello diagram och klicka på Öppna Pipeline.

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Bekräfta att du ser hello HDInsightHive aktivitet i hello pipeline.

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    toonavigate bakifrån toohello tidigare, klicka på **datafabriken** i hello dynamiska menyn hello överst.
5. I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobInput**. Kontrollera att hello-segment i **klar** tillstånd. Det kan ta några minuter för hello sektorn tooshow i tillståndet Ready. Om det inte sker när du vänta ett tag, kan du se om du har hello indatafilen (input.log) placerade i rätt hello-behållaren (adfgetstarted) och mappen (inputdata).

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Klicka på **X** tooclose **AzureBlobInput** bladet.
7. I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobOutput**. Du ser att hello-segment som håller på att behandlas.

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. När bearbetningen är klar visas hello sektor i **klar** tillstånd.

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter). Därför förvänta sig hello pipeline för ta **cirka 30 minuter** tooprocess hello sektorn.
   >
   >

9. När hello sektorn är i **klar** tillstånd, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.  

   ![utdata](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Klicka på hello sektorn toosee information om den i en **datasektorn** bladet.

   ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Klicka på en aktivitet som körs i hello **aktiviteten körs listan** toosee information om en aktivitet kör (Hive aktivitet i vårt scenario) i en **aktivitet köras information** fönster.   

   ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   Du kan se hello Hive-fråga som har utförts och statusinformation från hello loggfiler. Dessa loggar är användbara vid felsökning av eventuella problem.
   Se artikeln [Övervaka och hantera pipelines med Azure-portalblad](data-factory-monitor-manage-pipelines.md) för mer information.

> [!IMPORTANT]
> hello indatafilen hämtar bort när hello segment har bearbetats. Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Övervaka pipeline med övervaknings- och hanteringsappen
Du kan också använda Övervakare och hantera program toomonitor din pipelines. Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.

1. Klicka på **övervaka och hantera** panelen på hello startsidan för din data factory.

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Du bör se **Övervakaren och hantera program**. Ändra hello **starttid** och **sluttiden** toomatch starta sluttider för din pipeline och på **tillämpa**.

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Välj en aktivitetsfönstret i hello **aktivitet Windows** listan toosee information om den.

    ![Information om aktivitetsfönstret](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Sammanfattning
I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster. Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade två **länkade tjänster**:
   1. **Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.
   2. **Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory. Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.
3. Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.
4. Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett HDInsight-kluster på begäran. hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Se även
| Avsnitt | Beskrivning |
|:--- |:--- |
| [Pipelines](data-factory-create-pipelines.md) |Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag. |
| [Datauppsättningar](data-factory-create-datasets.md) |I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory. |
| [Schemaläggning och körning](data-factory-scheduling-and-execution.md) |Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell. |
| [Övervaka och hantera pipelines med övervakningsappen](data-factory-monitor-manage-app.md) |Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen. |
