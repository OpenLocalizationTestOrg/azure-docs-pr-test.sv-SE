---
title: "aaaInvoke Spark-program från Azure Data Factory | Microsoft Docs"
description: "Lär dig hur tooinvoke Spark-program från ett Azure data factory med hello MapReduce Activity."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Anropa Spark-program från Azure Data Factory pipelines

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md)
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Introduktion
Spark aktivitet är en av hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) stöds av Azure Data Factory. Den här aktiviteten körs hello angivna Spark-program på din Apache Spark-kluster i Azure HDInsight.    

> [!IMPORTANT]
> - Spark aktiviteten stöder inte HDInsight Spark-kluster som använder ett Azure Data Lake Store som primär lagring.
> - Spark aktiviteten stöder endast befintliga (egna) HDInsight Spark-kluster. Det stöder inte en länkad HDInsight på begäran-tjänst.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Genomgång: skapa en pipeline med Spark-aktivitet
Här följer hello typiska steg toocreate Data Factory-pipelinen med ett Spark-aktivitet.  

1. Skapa en datafabrik.
2. Skapa en länkad Azure Storage service toolink ditt Azure storage som är associerad med din HDInsight Spark-kluster toohello data factory.     
2. Skapa ett Azure HDInsight länkade tjänsten toolink Apache Spark-kluster i Azure HDInsight toohello data factory.
3. Skapa en datamängd som refererar toohello länkad Azure Storage-tjänst. För närvarande måste du ange en datamängd för utdata för en aktivitet även om det finns inga utdata som skapas.  
4. Skapa en pipeline med Spark-aktivitet som refererar toohello HDInsight länkad tjänst skapas i #2. hello-aktiviteten är konfigurerad med hello dataset som du skapade i föregående steg för hello som en datamängd för utdata. hello datamängd för utdata är vilka enheter hello schema (varje timme, varje dag, osv.). Därför måste du ange hello utdatauppsättningen även om hello aktiviteten inte verkligen ger utdata.

### <a name="prerequisites"></a>Krav
1. Skapa en **allmänna Azure Storage-konto** genom att följa anvisningarna i hello genomgång: [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Skapa en **Apache Spark-kluster i Azure HDInsight** genom att följa anvisningarna i kursen hello: [skapa Apache Spark-kluster i Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Associera hello Azure storage-konto som du skapade i steg #1 med det här klustret.  
3. Hämta och granska hello python skriptfilen **test.py** finns på: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Överför **test.py** toohello **pyFiles** mapp i hello **adfspark** behållare i Azure Blob storage. Skapa hello-behållaren och hello mappen om de inte finns.

### <a name="create-data-factory"></a>Skapa en datafabrik
Låt oss börja med att skapa hello data factory i det här steget.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**.
3. I hello **nya datafabriken** bladet ange **SparkDF** för hello namn.

   > [!IMPORTANT]
   > hello hello Azure data factory måste vara **globalt unika**. Om du ser hello-fel: **datafabriksnamnet ”SparkDF” är inte tillgänglig**. Ändra hello namn i hello data factory (till exempel yournameSparkDFdate och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.   
4. Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.
5. Välj en befintlig **resursgruppen** eller skapa ett Azure-resursgrupp.
6. Välj **PIN-kod toodashboard** alternativet.  
6. Klicka på **skapa** på hello **nya data factory** bladet.

   > [!IMPORTANT]
   > toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.
7. Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen på följande sätt:   
8. När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory. Om hello data factory-sidan inte visas klickar du på hello panelen för din data factory på hello instrumentpanel.

    ![Bladet Datafabrik](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget och skapa två länkade tjänster, en toolink din Spark-kluster tooyour data factory och hello andra toolink din Azure storage tooyour data factory.  

#### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure Storage-konto tooyour data factory. En datauppsättning som du skapar i ett steg senare i den här genomgången refererar toothis länkade tjänsten. Hej HDInsight länkade tjänst som du definierar i hello nästa steg refererar toothis länkad tjänst för.  

1. Klicka på **författare och distribuera** på hello **Data Factory** bladet för din data factory. Du bör se hello Data Factory-redigeraren.
2. Klicka på **Nytt datalager** och välj **Azure-lagring**.

   ![Ny datalagring - Azure Storage - meny](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Du bör se hello **JSON-skript** länkade tjänsten i hello redigerare för att skapa ett Azure Storage.

   ![Länkad Azure-lagringstjänst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Ersätt **kontonamn** och **kontonyckel** med hello namn och åtkomstnyckel för Azure storage-konto. toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält. När hello länkade tjänsten har distribuerats korrekt hello **utkast 1** fönstret bör försvinner och du ser **AzureStorageLinkedService** i hello trädvyn hello vänster.

#### <a name="create-hdinsight-linked-service"></a>Skapa länkad HDInsight-tjänst
I det här steget skapar du Azure HDInsight länkade tjänsten toolink din HDInsight Spark-kluster toohello data factory. Hej HDInsight-kluster är används toorun hello Spark-program som angetts i hello Spark-aktivitet för hello pipeline i det här exemplet.  

1. Klicka på **... Flera** på hello verktygsfältet **nya beräkning**, och klicka sedan på **HDInsight-kluster**.

    ![Skapa länkad HDInsight-tjänst](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Kopiera och klistra in följande kodutdrag toohello hello **utkast 1** fönster. I Redigeraren för hello JSON hello följande steg:
    1. Ange hello **URI** för hello HDInsight Spark-kluster. Till exempel: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Ange hello namn hello **användaren** vem som har åtkomst toohello Spark-kluster.
    3. Ange hello **lösenord** för användaren.
    4. Ange hello **Azure länkade lagringstjänsten** som är associerad med hello HDInsight Spark-kluster. I det här exemplet är: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Spark aktiviteten stöder inte HDInsight Spark-kluster som använder ett Azure Data Lake Store som primär lagring.
    > - Spark aktiviteten stöder endast befintliga (egna) HDInsight Spark-kluster. Det stöder inte en länkad HDInsight på begäran-tjänst.

    Se [länkad HDInsight-tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) mer information om hello HDInsight länkade tjänsten.
3.  toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält.  

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
hello datamängd för utdata är vilka enheter hello schema (varje timme, varje dag, osv.). Därför måste du ange en datamängd för utdata för hello spark aktiviteten i hello pipeline även om hello aktivitet verkligen inte producerar några utdata. Ange en inkommande datauppsättning för hello aktiviteten är valfritt.

1. I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.  
2. Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret. hello JSON fragment definierar en datamängd som kallas **OutputDataset**. Dessutom kan du ange att hello resultat lagras i hello blob-behållaren som kallas **adfspark** och hello mapp som heter **pyFiles-/ utdata**. Som tidigare nämnts är den här datauppsättningen dummy dataset. hello Spark-program i det här exemplet skapar inte några utdata. Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje dag.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. toodeploy Hej dataset, klickar du på **distribuera** i hello kommandofält.


### <a name="create-pipeline"></a>Skapa pipeline
I det här steget skapar du en pipeline med en **HDInsightSpark** aktivitet. Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata. Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset. Därför har inga indata datamängden angetts i det här exemplet.

1. I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet och klicka sedan på **ny pipeline**.
2. Ersätt hello skript i hello utkast-1-fönster med hello följande skript:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Observera följande punkter hello:
    - Hej **typen** egenskapen för**HDInsightSpark**.
    - Hej **rootPath** har angetts för**adfspark\\pyFiles** där adfspark är hello Azure Blob-behållare och pyFiles är bra mapp i behållaren. I det här exemplet är hello Azure Blob Storage hello en som är associerad med hello Spark-kluster. Du kan ladda upp hello filen tooa olika Azure Storage. Om du gör det, skapa en länkad Azure Storage service toolink storage-konto toohello data factory. Ange hello namnet på hello länkade tjänst som ett värde för hello **sparkJobLinkedService** egenskapen. Se [Spark Aktivitetsegenskaper](#spark-activity-properties) för ytterligare information om den här egenskapen och andra egenskaper som stöds av hello Spark-aktivitet.  
    - Hej **entryFilePath** anges toohello **test.py**, vilket är hello python-fil.
    - Hej **getDebugInfo** egenskapen för**alltid**, vilket innebär att hello loggfiler är alltid genereras (lyckade eller misslyckade).

        > [!IMPORTANT]
        > Vi rekommenderar att du inte anger den här egenskapen för`Always` i en produktionsmiljö om du felsöker ett problem.
    - Hej **matar ut** avsnittet innehåller en datamängd för utdata. Du måste ange en datamängd för utdata, även om hello spark-program inte producerar några utdata. hello utdata dataset enheter hello schema för hello pipelinen (varje timme, varje dag, osv.).  

        Mer information om hello egenskaper som stöds av Spark-aktiviteten finns [Väck Aktivitetsegenskaper](#spark-activity-properties) avsnitt.
3. toodeploy hello pipeline, klickar du på **distribuera** i hello kommandofält.

### <a name="monitor-pipeline"></a>Övervaka pipeline
1. Klicka på **X** tooclose Data Factory-redigeraren blad och toonavigate tillbaka toohello Data Factory-startsidan. Klicka på **övervaka och hantera** toolaunch hello övervakning av program på en annan flik.

    ![Övervaka och hantera sida vid sida](media/data-factory-spark/monitor-and-manage-tile.png)
2. Ändra hello **starttid** filtrera hello överst för**2/1/2017**, och klicka på **tillämpa**.
3. Du bör se endast en aktivitetsfönstret eftersom det finns endast en dag mellan hello start (2017-02-01)- och sluttider (2017-02-02) för hello pipeline. Kontrollera att hello datasektorn i **klar** tillstånd.

    ![Övervakaren hello pipeline](media/data-factory-spark/monitor-and-manage-app.png)    
4. Välj hello **aktivitetsfönstret** toosee information om hello aktiviteter körs. Om det finns ett fel, se information om det i hello till höger.

### <a name="verify-hello-results"></a>Verifiera hello resultaten

1. Starta **Jupyter-anteckningsbok** för HDInsight Spark-kluster genom att gå till: https://CLUSTERNAME.azurehdinsight.net/jupyter. Du kan också starta instrumentpanelen klustret för HDInsight Spark-klustret och sedan starta **Jupyter-anteckningsbok**.
2. Klicka på **ny** -> **PySpark** toostart en ny anteckningsbok.

    ![Ny Jupyter-anteckningsbok](media/data-factory-spark/jupyter-new-book.png)
3. Kör hello följande kommando genom att kopiera/klistra in hello text och trycka på **SKIFT + RETUR** hello slutet av andra hello-satsen.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Bekräfta att du ser hello data från hello hvac tabell:  

    ![Jupyter frågeresultat](media/data-factory-spark/jupyter-notebook-results.png)

Se [köra Spark SQL-fråga](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) avsnittet detaljerade anvisningar. 

### <a name="troubleshooting"></a>Felsökning
Eftersom du ställer in **getDebugInfo** för**alltid**, visas en **loggen** undermapp i hello **pyFiles** mapp i Azure Blob-behållare. hello-loggfilen i hello loggmappen innehåller ytterligare information. Den här loggfilen är användbart när det uppstår ett fel. I en produktionsmiljö kan du tooset den för**fel**.

Ytterligare felsökningsinformation, hello följande steg:


1. Navigera för`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![YARN-Användargränssnittet program](media/data-factory-spark/yarnui-application.png)  
2. Klicka på **loggar** för en hello kör försök.

    ![Sidan program](media/data-factory-spark/yarn-applications.png)
3. Du bör se ytterligare information om fel hello loggen på sidan.

    ![Logga fel](media/data-factory-spark/yarnui-application-error.png)

hello följande avsnitt innehåller information om Data Factory entiteter toouse Apache Spark-kluster och Spark aktivitet i din data factory.

## <a name="spark-activity-properties"></a>Spark Aktivitetsegenskaper
Här är hello exempel JSON-definitionen för en pipeline med Spark aktivitet:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

hello beskrivs följande tabell hello JSON egenskaper som används i hello JSON-definitionen:

| Egenskap | Beskrivning | Krävs |
| -------- | ----------- | -------- |
| namn | Namnet på hello aktivitet i hello pipeline. | Ja |
| description | Text som beskriver vilka hello-aktiviteten har. | Nej |
| typ | Den här egenskapen måste anges tooHDInsightSpark. | Ja |
| linkedServiceName | Namnet på hello HDInsight länkade tjänsten på vilka hello Spark programmet körs. | Ja |
| rootPath | hello Azure Blob-behållaren och mappen som innehåller hello Spark-fil. hello-filnamnet är skiftlägeskänsliga. | Ja |
| entryFilePath | Relativ sökväg toohello rotmapp hello Spark kodpaketet. | Ja |
| Klassnamn | Programmets Java/Spark huvudsakliga klass | Nej |
| Argument | En lista med kommandoradsargument toohello Spark-program. | Nej |
| proxyUser | hello konto tooimpersonate tooexecute hello Spark användarprogram | Nej |
| sparkConfig | Ange värden för egenskaper för Spark-konfiguration som beskrivs i avsnittet hello: [Spark-konfiguration – programegenskaper](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nej |
| getDebugInfo | Anger när hello Spark loggfilerna kopierade toohello Azure storage som används av HDInsight-kluster (eller) anges av sparkJobLinkedService. Tillåtna värden: None, alltid eller fel. Standardvärde: Ingen. | Nej |
| sparkJobLinkedService | hello länkad Azure Storage-tjänst som äger hello Spark fil, beroenden och loggar.  Om du inte anger ett värde för den här egenskapen används hello lagring som är associerade med HDInsight-kluster. | Nej |

## <a name="folder-structure"></a>Mappstruktur
hello Spark aktiviteten stöder inte en infogad skriptet som Pig och gör Hive-aktiviteter. Spark jobb är också mer utökningsbar än Pig/Hive-jobb. För Spark jobb, du kan ange flera beroenden som jar-paket (placeras i hello java KLASSÖKVÄGEN), python-filer (placerad hello PYTHONPATH) och andra filer.

Skapa hello följande mappstrukturen i hello Azure Blob storage som refereras av hello länkad HDInsight-tjänst. Därefter kan du överföra beroende filer toohello lämpliga undermappar i hello rotmapp som representeras av **entryFilePath**. Till exempel överföra python filer toohello pyFiles undermapp och jar-filer toohello burkar undermapp hello rotmappen. Vid körning förväntar Data Factory-tjänsten hello följande mappstrukturen i hello Azure Blob storage:     

| Sökväg | Beskrivning | Krävs | Typ |
| ---- | ----------- | -------- | ---- |
| . | hello rotsökvägen för hello Spark jobb i hello länkade lagringstjänsten    | Ja | Mapp |
| &lt;användardefinierade&gt; | Hej sökväg som pekar toohello post-filen för hello Spark jobb | Ja | Fil |
| . / JAR: er | Alla filer under den här mappen överförs och placeras på hello java klassökväg hello kluster | Nej | Mapp |
| . / pyFiles | Alla filer under den här mappen överförs och placeras på hello PYTHONPATH hello-klustret | Nej | Mapp |
| . / filer | Alla filer under den här mappen överförs och placeras på utföraren arbetskatalogen | Nej | Mapp |
| . / arkiveras | Alla filer under den här mappen dekomprimeras | Nej | Mapp |
| . / loggar | hello mappen där loggar från hello Spark-kluster lagras.| Nej | Mapp |

Här är ett exempel på en lagringsplats som innehåller två Spark jobbfiler i hello Azure Blob Storage som refereras av hello länkad HDInsight-tjänst.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
