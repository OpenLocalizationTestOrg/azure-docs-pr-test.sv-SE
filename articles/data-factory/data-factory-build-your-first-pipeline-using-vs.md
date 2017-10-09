---
title: "aaaBuild din första data factory (Visual Studio) | Microsoft Docs"
description: "I den här självstudien skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a>Självstudiekurs: Skapa en datafabrik med hjälp av Visual Studio
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

De här självstudierna visar hur toocreate ett Azure data factory med hjälp av Visual Studio. Du skapar ett Visual Studio-projekt med hello Data Factory projektmall, definiera Data Factory-enheter (länkade tjänster, datauppsättningar och pipeline) i JSON-format och sedan publicera/distribuera dessa enheter toohello moln. 

hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata. hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider. 

> [!NOTE]
> Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).


## <a name="walkthrough-create-and-publish-data-factory-entities"></a>Genomgång: Skapa och publicera datafabriksentiteter
Här är hello steg du utför som en del av den här genomgången:

1. Skapa två länkade tjänster: **AzureStorageLinkedService1** och **HDInsightOnDemandLinkedService1**. 
   
    I den här självstudiekursen hello både indata och utdata för hello hive aktiviteten är i du samma Azure Blob Storage. Du använder en på-begäran HDInsight-kluster tooprocess befintlig indata tooproduce utdata. hello på begäran HDInsight-kluster skapas automatiskt för dig av Azure Data Factory vid körning när hello indata är klar toobe bearbetas. Du måste toolink dina data lagras eller beräknar tooyour data factory så att hello Data Factory-tjänsten kan ansluta toothem vid körning. Därför du länka din Azure Storage-konto toohello data factory med hello AzureStorageLinkedService1 och länka ett HDInsight-kluster på begäran med hjälp av hello HDInsightOnDemandLinkedService1. När du publicerar kan ange du hello namn för hello data factory toobe skapas eller en befintlig datafabrik.  
2. Skapa två datamängder: **InputDataset** och **OutputDataset**, som representerar hello i/o-data som lagras i hello Azure blob storage. 
   
    Dessa dataset-definitioner finns toohello länkad Azure Storage-tjänst som du skapade i föregående steg i hello. För hello InputDataset, ange hello blob-behållaren (adfgetstarted) och hello mapp (inptutdata) som innehåller en blob med hello indata. För hello OutputDataset, anger du hello blob-behållaren (adfgetstarted) och hello mapp (partitioneddata) som innehåller hello utdata. Du kan också ange andra egenskaper som struktur, tillgänglighet och princip.
3. Skapa en pipeline med namnet **MyFirstPipeline**. 
  
    I den här genomgången hello pipeline har endast en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten transformeringen indata tooproduce utdata genom att köra en hive-skript på ett HDInsight-kluster på begäran. toolearn mer om hive-aktivitet, se [Hive-aktivitet](data-factory-hive-activity.md) 
4. Skapa en datafabrik med namnet **DataFactoryUsingVS**. Distribuera hello data factory och alla Data Factory-enheter (länkade tjänster, tabeller och hello pipeline).
5. När du publicerar använder du Azure portal blad och övervakning & Management-appen toomonitor hello pipeline. 
  
### <a name="prerequisites"></a>Krav
1. Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg. Du kan också välja hello **översikt och förutsättningar** alternativ i listrutan för hello på hello översta tooswitch toohello artikel. När du har slutfört hello krav växla tillbaka toothis artikel genom att välja **Visual Studio** alternativ i listrutan hello.
2. toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.  
3. Du måste ha hello följande installerat på datorn:
   * Visual Studio 2013 eller Visual Studio 2015
   * Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015. Navigera för[Azure-hämtningssida](https://azure.microsoft.com/downloads/) och på **VS 2013** eller **VS 2015** i hello **.NET** avsnitt.
   * Hämta hello senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Du kan också uppdatera hello plugin-programmet genom att göra följande hello: på menyn hello **verktyg** -> **tillägg och uppdateringar** -> **Online**  ->  **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **uppdatering**.

Nu ska vi använda Visual Studio toocreate ett Azure data factory.

### <a name="create-visual-studio-project"></a>Skapa Visual Studio-projekt
1. Starta **Visual Studio 2013** eller **Visual Studio 2015**. Klicka på **filen**, peka för**ny**, och klicka på **projekt**. Du bör se hello **nytt projekt** dialogrutan.  
2. I hello **nytt projekt** dialogrutan, Välj hello **DataFactory** mall och klicka på **tomt Data Factory projekt**.   

    ![Dialogrutan Nytt projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. Ange en **namn** för hello projekt **plats**, och ett namn för hello **lösning**, och klicka på **OK**.

    ![Solution Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget kan du skapa två länkade tjänster: **Azure Storage** och **HDInsight på begäran**. 

hello Azure Storage länkade tjänsten länkar din toohello data factory för Azure Storage-konto genom att tillhandahålla hello anslutningsinformationen. Data Factory-tjänsten använder hello anslutningssträng från hello länkade tjänstinställning tooconnect toohello Azure storage vid körning. Lagringen innehåller indata och utdata för hello pipeline och hello hive skriptfilen som används av hello hive-aktivitet. 

Med på begäran HDInsight länkad tjänst skapas hello HDInsight-kluster automatiskt vid körning när hello indata är klar tooprocessed. hello klustret tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod. 

> [!NOTE]
> Du kan skapa en datafabrik genom att ange dess namn och inställningar för närvarande hello publicera din Data Factory-lösning.

#### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
1. Högerklicka på **länkade tjänster** i hello solution explorer peka för**Lägg till**, och klicka på **nytt objekt**.      
2. I hello **Lägg till nytt objekt** dialogrutan **länkade Azure-lagringstjänsten** hello listan och klickar på **Lägg till**.
    ![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)
3. Ersätt `<accountname>` och `<accountkey>` med hello namnet på ditt Azure storage-konto och dess nyckel. toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
    ![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)
4. Spara hello **AzureStorageLinkedService1.json** fil.

#### <a name="create-azure-hdinsight-linked-service"></a>Skapa en Azure HDInsight-länkad tjänst
1. I hello **Solution Explorer**, högerklicka på **länkade tjänster**, peka för**Lägg till**, och klicka på **nytt objekt**.
2. Välj **Länkad HDInsight-tjänst på begäran** och klicka på **Lägg till**.
3. Ersätt hello **JSON** med hello följande JSON:

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
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

    Egenskap | Beskrivning
    -------- | ----------- 
    ClusterSize | Anger hello storleken på hello HDInsight Hadoop-kluster.
    TimeToLive | Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort.
    linkedServiceName | Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight Hadoop-kluster. 

    > [!IMPORTANT]
    > Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (linkedServiceName). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive). hello klustret tas bort automatiskt när hello bearbetningen är klar.
    > 
    > Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

    Mer information om JSON-egenskaper finns i artikeln [Länkade tjänster för Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
4. Spara hello **HDInsightOnDemandLinkedService1.json** fil.

### <a name="create-datasets"></a>Skapa datauppsättningar
I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning. De här datauppsättningarna finns toohello **AzureStorageLinkedService1** du har skapat tidigare i den här kursen. Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.   

#### <a name="create-input-dataset"></a>Skapa indatauppsättning
1. I hello **Solution Explorer**, högerklicka på **tabeller**, peka för**Lägg till**, och klicka på **nytt objekt**.
2. Välj **Azure Blob** hello listan Ändra hello namnet på hello-filen för**InputDataSet.json**, och klicka på **Lägg till**.
3. Ersätt hello **JSON** i hello-redigeraren med hello följande JSON-fragment:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    Den här JSON-fragment definierar en datamängd som kallas **AzureBlobInput** som representerar indata för hello hive aktivitet i hello pipeline. Du anger att hello indata finns i hello blob-behållaren som kallas `adfgetstarted` och hello mapp som heter `inputdata`.

    hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

    Egenskap | Beskrivning |
    -------- | ----------- |
    typ |hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i Azure Blob Storage.
    linkedServiceName | Refererar toohello AzureStorageLinkedService1 som du skapade tidigare.
    fileName |Den här egenskapen är valfri. Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats. I det här fallet bearbetas endast hello input.log.
    typ | hello-loggfiler finns i textformat, så vi använder TextFormat. |
    columnDelimiter | kolumner i hello loggfiler är avgränsade med kommatecken hello (`,`)
    frekvens/intervall | frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad.
    extern | Den här egenskapen anges tootrue om hello indata för aktivitet hello inte genereras av hello pipeline. Den här egenskapen anges endast för indatauppsättningar. För hello inkommande dataset hello första aktivitet alltid ange tootrue.
4. Spara hello **InputDataset.json** fil.

#### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
Nu kan skapa du hello utdata dataset toorepresent utdata data som lagras i hello Azure Blob storage.

1. I hello **Solution Explorer**, högerklicka på **tabeller**, peka för**Lägg till**, och klicka på **nytt objekt**.
2. Välj **Azure Blob** hello listan Ändra hello namnet på hello-filen för**OutputDataset.json**, och klicka på **Lägg till**.
3. Ersätt hello **JSON** i hello-redigeraren med hello följande JSON:
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    hello JSON fragment definierar en datamängd som kallas **AzureBlobOutput** som representerar utdata produceras av hello hive aktivitet i hello pipeline. Du anger att hello utgående data skapas av hello hive aktiviteten placeras i hello blob-behållaren som kallas `adfgetstarted` och hello mapp som heter `partitioneddata`. 
    
    Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad. hello utdata dataset enheter hello schema för hello pipeline. hello pipeline körs varje månad mellan dess start- och sluttider. 

    Se **skapa hello inkommande dataset** avsnitt beskrivs dessa egenskaper. Du anger inte externa hello-egenskapen på en datamängd för utdata som hello dataset produceras av hello pipeline.
4. Spara hello **OutputDataset.json** fil.

### <a name="create-pipeline"></a>Skapa pipeline
Du har skapat hello länkad Azure Storage-tjänst och inkommande och utgående datauppsättningar hittills. Nu ska du skapa en pipeline med en **HDInsightHive**-aktivitet. Hej **inkommande** för hello hive aktiviteten är inställd för**AzureBlobInput** och **utdata** har angetts för**AzureBlobOutput**. En sektor i ett inkommande dataset finns varje månad (frekvens: månad, intervall: 1), och hello utdata segment skapas varje månad för. 

1. I hello **Solution Explorer**, högerklicka på **Pipelines**, peka för**Lägg till**, och klicka på **nytt objekt.**
2. Välj **Hive omvandling Pipeline** hello listan och klickar på **Lägg till**.
3. Ersätt hello **JSON** med följande kodavsnitt hello:

    > [!IMPORTANT]
    > Ersätt `<storageaccountname>` med hello namnet på ditt lagringskonto.

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
                        "scriptLinkedService": "AzureStorageLinkedService1",
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
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > Ersätt `<storageaccountname>` med hello namnet på ditt lagringskonto.

    hello JSON fragment definierar en pipeline som består av en enskild aktivitet (Hive aktivitet). Den här aktiviteten körs en Hive-skript tooprocess inkommande data på en på-begäran HDInsight-kluster tooproduce utdata. I hello aktiviteter i pipeline-hello JSON kan du se endast en aktivitet i hello matris med typen som angetts för**HDInsightHive**. 

    I hello egenskaper som är specifika tooHDInsight Hive aktivitet, anger du vilken länkad Azure Storage-tjänst har hello hive-skriptfil, hello sökvägen toohello skriptfilen och parametrar toohello skriptfilen. 

    hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService) och i hello `script` mapp i hello behållaren `adfgetstarted`.

    Hej `defines` avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.

    Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline. Du har konfigurerat hello dataset toobe producerade därför månadsvis, bara en gång sektorn produceras av hello pipeline (eftersom hello månad är samma i start- och slutdatum).

    I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.
4. Spara hello **HiveActivity1.json** fil.

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Lägg till partitionweblogs.hql och input.log som ett beroende
1. Högerklicka på **beroenden** i hello **Solution Explorer** fönstret peka för**Lägg till**, och klicka på **befintlig artikel**.  
2. Navigera toohello **C:\ADFGettingStarted** och välj **partitionweblogs.hql**, **input.log** filer och klicka på **Lägg till**. Du har skapat dessa två filer som en del av krav från hello [kursen översikt](data-factory-build-your-first-pipeline.md).

När du publicerar hello lösning i nästa steg i hello hello **partitionweblogs.hql** filen är överförda toohello **skriptet** mapp i hello `adfgetstarted` blob-behållare.   

### <a name="publishdeploy-data-factory-entities"></a>Publicera/distribuera Data Factory-entiteter
I det här steget kan publicera du hello Data Factory entiteter (länkade tjänster, datauppsättningar och pipeline) i ditt projekt toohello Azure Data Factory-tjänsten. Pågående hello för publicering, kan du ange hello namn för din data factory. 

1. Högerklicka på projektet i hello Solution Explorer och klicka på **publicera**.
2. Om du ser **logga in tooyour Microsoft-konto** dialogrutan, ange dina autentiseringsuppgifter för hello-konto med Azure-prenumeration och på **logga in**.
3. Du bör se följande dialogrutan hello:

   ![Dialogrutan Publicera](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. I hello **konfigurera datafabriken** sidan, hello följande steg:

    ![Publicera – nya datafabriksinställningar](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. välj alternativet **Skapa ny Data Factory**.
   2. Ange ett unikt **namn** för hello data factory. Exempel: **DataFactoryUsingVS09152016**. hello namn måste vara globalt unika.
   3. Välj rätt hello-prenumeration för hello **prenumeration** fältet. 
        > [!IMPORTANT]
        > Om du inte ser någon prenumeration, kontrollera att du har loggat in med ett konto som är en administratör eller medadministratör för hello prenumeration.
   4. Välj hello **resursgruppen** för hello data factory toobe skapas.
   5. Välj hello **region** för hello data factory.
   6. Klicka på **nästa** tooswitch toohello **publicera objekt** sidan. (Tryck på **FLIKEN** toomove utanför hello namnet fältet tooif hello **nästa** är inaktiverat.)

    > [!IMPORTANT]
    > Om felmeddelandet hello **datafabriksnamnet ”DataFactoryUsingVS” är inte tillgänglig** vid publicering, ändra hello namn (till exempel yournameDataFactoryUsingVS). Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.   
1. I hello **publicera objekt** , se till att alla hello Datafabriker entiteter är markerade och klickar på **nästa** tooswitch toohello **sammanfattning** sidan.

    ![Sidan Publish items (Publicera objekt)](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. Granska hello sammanfattning och klicka på **nästa** toostart hello distribution processen och visa hello **Distributionsstatus**.

    ![Sammanfattningssida](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. I hello **Distributionsstatus** sida, bör du se hello status hello distributionsprocessen. Klicka på Slutför när hello distributionen är klar.

Viktiga punkter toonote:

- Om felmeddelandet hello: **den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**, gör du något av följande hello och försök att publicera igen:
    - Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell.
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad.

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - Logga in med hjälp av hello Azure-prenumeration i toohello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen. Den här åtgärden registrerar automatiskt hello-providern för dig.
- hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.
- toocreate Data Factory instanser måste toobe administratör eller medadministratör av hello Azure-prenumeration

### <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget kan övervaka du hello pipeline med diagramvyn i hello data factory. 

#### <a name="monitor-pipeline-using-diagram-view"></a>Övervaka pipeline med diagramvyn
1. Logga in toohello [Azure-portalen](https://portal.azure.com/), hello följande steg:
   1. Klicka på **Fler tjänster** och på **Datafabriker**.
       
        ![Bläddra igenom datafabrikerna](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. Välj hello namnet på din data factory (till exempel: **DataFactoryUsingVS09152016**) hello listan över datafabriker.
   
       ![Välj din datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. I hello startsidan för din data factory, klickar du på **Diagram**.

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. I hello diagramvyn visas en översikt över hello pipelines och datauppsättningar som används i den här självstudiekursen.

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. tooview alla aktiviteter i hello pipeline, högerklicka på pipeline i hello diagram och klicka på Öppna Pipeline.

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. Bekräfta att du ser hello HDInsightHive aktivitet i hello pipeline.

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    toonavigate bakifrån toohello tidigare, klicka på **datafabriken** i hello dynamiska menyn hello överst.
6. I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobInput**. Kontrollera att hello-segment i **klar** tillstånd. Det kan ta några minuter för hello sektorn tooshow i tillståndet Ready. Om det inte sker när du vänta ett tag, se om du har hello indatafilen (input.log) placeras i rätt hello-behållaren (`adfgetstarted`) och mappar (`inputdata`). Och se till att hello **externa** för hello inkommande datauppsättningen egenskapen för**SANT**. 

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. Klicka på **X** tooclose **AzureBlobInput** bladet.
8. I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobOutput**. Du ser att hello-segment som håller på att behandlas.

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. När bearbetningen är klar visas hello sektor i **klar** tillstånd.

   > [!IMPORTANT]
   > Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter). Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.  
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. När hello sektorn är i **klar** tillstånd, kontrollera hello `partitioneddata` mapp i hello `adfgetstarted` behållare i blobblagring för hello utdata.  

    ![utdata](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Klicka på hello sektorn toosee information om den i en **datasektorn** bladet.

    ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Klicka på en aktivitet som körs i hello **aktiviteten körs listan** toosee information om en aktivitet kör (Hive aktivitet i vårt scenario) i en **aktivitet köras information** fönster. 
  
    ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    Du kan se hello Hive-fråga som har utförts och statusinformation från hello loggfiler. Dessa loggar är användbara vid felsökning av eventuella problem.  

Se [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) anvisningar för hur toouse hello Azure portal toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.

#### <a name="monitor-pipeline-using-monitor--manage-app"></a>Övervaka pipeline med övervaknings- och hanteringsappen
Du kan också använda Övervakare och hantera program toomonitor din pipelines. Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.

1. Klicka på ikonen Övervaka och hantera.

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. Du bör se programmet Övervaka och hantera. Ändra hello **starttid** och **sluttiden** toomatch start (2016-04-01 12:00 AM)- och sluttider (2016-02-04 12:00 AM) i din pipeline och klickar på **tillämpa**.

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. toosee information om en aktivitetsfönstret markerar du den i hello **aktivitet Windows lista** toosee information om den.
    ![Aktivitetsfönsterinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)

> [!IMPORTANT]
> hello indatafilen hämtar bort när hello segment har bearbetats. Därför, om du vill toorerun hello segment eller hello kursen igen överför hello indatafilen (input.log) toohello `inputdata` för hello `adfgetstarted` behållare.

### <a name="additional-notes"></a>Ytterligare information
- En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel indata en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform. Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten. Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory.
- Länkade tjänster länka datalager eller compute services tooan Azure data factory. Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten. Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory och [omvandling aktiviteter](data-factory-data-transformation-activities.md) som körs på dem..
- Se [flytta data från / tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om JSON-egenskaper som används i hello länkad Azure Storage service definition.
- Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran. Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.
-  hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello föregående JSON. Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.
- Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (linkedServiceName). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive). hello klustret tas bort automatiskt när hello bearbetningen är klar.
    
    Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.
- Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset. 
- Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-server-explorer-tooview-data-factories"></a>Använd Server Explorer tooview datafabriker
1. I **Visual Studio**, klickar du på **visa** på hello menyn och klickar på **Server Explorer**.
2. I hello Server Explorer, expandera **Azure** och expandera **Data Factory**. Om du ser **logga in tooVisual Studio**, ange hello **konto** som är associerade med din Azure-prenumeration och klicka på **Fortsätt**. Ange **lösenordet** och klicka på **Logga in**. Visual Studio försöker tooget information om alla Azure datafabriker i din prenumeration. Du ser hello status för den här åtgärden i hello **Data Factory uppgiftslista** fönster.

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Du kan högerklicka på en datafabrik och välj **exportera Data Factory tooNew projekt** toocreate Visual Studio-projekt utifrån en befintlig datafabrik.

    ![Exportera datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Uppdatera Data Factory-verktyg för Visual Studio
tooupdate Azure Data Factory tools för Visual Studio hello följande steg:

1. Klicka på **verktyg** på hello-menyn och välj **tillägg och uppdateringar**.
2. Välj **uppdateringar** i hello till vänster och välj sedan **Visual Studio-galleriet**.
3. Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**. Om du inte ser den här posten har du redan hello senaste versionen av hello verktyg.

## <a name="use-configuration-files"></a>Använda konfigurationsfiler
Du kan använda konfigurationsfiler i Visual Studio tooconfigure egenskaper för länkade tjänster/tabeller/pipelines på olika sätt för varje miljö.

Överväg att hello JSON-definitionen för en länkad Azure Storage-tjänst. toospecify **connectionString** med olika värden för accountname eller accountkey baserat på hello miljö (Dev/Test/produktion) toowhich du distribuerar Data Factory entiteter. Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.

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

### <a name="add-a-configuration-file"></a>Lägga till en konfigurationsfil
Lägg till en konfigurationsfil för varje miljö genom att utföra hello följande steg:   

1. Högerklicka på hello Data Factory-projekt i Visual Studio-lösning, peka för**Lägg till**, och klicka på **nytt objekt**.
2. Välj **Config** hello listan över installerade mallar hello vänster och välj **konfigurationsfilen**, ange en **namn** för konfiguration av hello fil och klicka på **Lägga till**.

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Lägg till konfigurationsparametrar och deras värden i hello följande format:

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst. Observera att hello syntax för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).   

    Om JSON har en egenskap som innehåller en matris med värden som visas i följande kod hello:  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    Konfigurera egenskaperna som visas i hello följande konfigurationsfilen (Använd Nollbaserad indexering):

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a>Egenskapsnamn med blanksteg
Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser som visas i följande exempel (Databasservernamnet) hello:

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a>Distribuera lösningen med en konfiguration
När du publicerar Azure Data Factory-entiteter i VS anger du hello-konfiguration som du vill toouse för publishing operationen.

toopublish entiteter i ett Azure Data Factory-projekt med hjälp av konfigurationsfil:   

1. Högerklicka på Data Factory-projektet och klicka på **publicera** toosee hello **publicera objekt** dialogrutan.
2. Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på hello **konfigurera datafabriken** och klicka på **nästa**.   
3. På hello **publicera objekt** sida: du kan se en lista med tillgängliga konfigurationer för hello **Välj distributionskonfiguration** fältet.

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. Välj hello **konfigurationsfilen** som du skulle t.ex. toouse och på **nästa**.
5. Bekräfta att du ser hello namnet på JSON-fil i hello **sammanfattning** och klickar på **nästa**.
6. Klicka på **Slutför** när hello Distributionsåtgärden har slutförts.

När du distribuerar är hello värden från konfigurationsfilen hello används tooset värden för egenskaper i hello JSON-filer innan hello entiteter som är distribuerade tooAzure Data Factory-tjänsten.   

## <a name="use-azure-key-vault"></a>Använda Azure Key Vault
Det är inte tillrådligt och ofta mot säkerhet princip toocommit känsliga data, till exempel anslutning strängar toohello databasen. Se [ADF Secure publicera](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub toolearn om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory entiteter. hello Secure publicera tillägget för Visual Studio kan hello hemligheter toobe lagras i Nyckelvalvet och endast referenser toothem har angetts i länkade tjänster / distributionskonfigurationer. När du publicerar Data Factory entiteter tooAzure matchas dessa referenser. De här filerna kan sedan vara allokerat toosource databasen utan att exponera alla hemligheter.

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

Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory). 


## <a name="see-also"></a>Se även
| Avsnitt | Beskrivning |
|:--- |:--- |
| [Pipelines](data-factory-create-pipelines.md) |Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct datadrivna arbetsflöden för din scenario eller ditt företag. |
| [Datauppsättningar](data-factory-create-datasets.md) |I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory. |
| [Datatransformeringsaktiviteter](data-factory-data-transformation-activities.md) |Den här artikeln innehåller en lista med de datatransformeringsaktiviteter (till exempel HDInsight Hive-transformeringen som du använde i självstudien) som stöds av Azure Data Factory. |
| [Schemaläggning och körning](data-factory-scheduling-and-execution.md) |Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell. |
| [Övervaka och hantera pipelines med övervakningsappen](data-factory-monitor-manage-app.md) |Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen. |
