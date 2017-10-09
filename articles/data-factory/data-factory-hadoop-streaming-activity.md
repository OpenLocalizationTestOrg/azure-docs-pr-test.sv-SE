---
title: aaaTransform data med Hadoop Streaming Activity - Azure | Microsoft Docs
description: "Lär dig hur du kan använda hello Hadoop Streaming Activity i ett Azure data factory tootransform data med Hadoop Streaming program som körs på en på-begäran/din egen HDInsight-kluster."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: a7ddb7268f47162709a9c8136ccd69e0b7d4ad7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformera data med Hadoop Streaming Activity i Azure Data Factory
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

Du kan använda hello HDInsightStreamingActivity aktiviteten anropa en Hadoop Streaming job från ett Azure Data Factory-pipelinen. hello visar följande JSON-utdrag hello syntax för att använda hello HDInsightStreamingActivity i en pipeline-JSON-fil. 

Hej HDInsight Streaming Activity i en Datafabrik [pipeline](data-factory-create-pipelines.md) Hadoop Streaming program körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserat HDInsight kluster. Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.

> [!NOTE] 
> Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln. 

## <a name="json-sample"></a>JSON-exempel
Hej HDInsight-kluster fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt). Som standard är namnet på hello-behållare som används av hello HDInsight-kluster hello namnet på själva hello-klustret. Om klusternamnet är myhdicluster, är namnet på hello blob-behållare som är kopplad till exempel myhdicluster. 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Observera följande punkter hello:

1. Ange hello **linkedServiceName** toohello namnet på hello länkade tjänst som pekar på vilka hello strömning mapreduce-jobbet körs tooyour HDInsight-kluster.
2. Ange hello hello verksamhetens för**HDInsightStreaming**.
3. För hello **mapper** egenskap, ange mapper körbara hello namn. I exemplet hello är cat.exe hello mapper körbara.
4. För hello **reducer** egenskap, ange reducer körbara hello namn. I exemplet hello är wc.exe hello reducer körbara.
5. För hello **inkommande** egenskapen type, ange hello indatafilen (inklusive hello plats) för hello mapper. I exemplet hello ”: wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt”: adfsample är hello blob-behållare, exempel/data/Gutenberg är hello mapp och davinci.txt är hello-blob.
6. För hello **utdata** egenskapen type, ange hello utdatafilen (inklusive hello plats) för hello reducer. hello utdata från hello Hadoop Streaming job skrivs toohello plats som anges för den här egenskapen.
7. I hello **filePaths** ange hello sökvägar för hello mapper och reducer körbara filer. I exemplet hello: ”adfsample/example/apps/wc.exe” adfsample är hello blob-behållare, exempel/appar är hello mapp och wc.exe är hello körbara.
8. För hello **fileLinkedService** egenskap, ange hello Azure Storage länkade tjänst som representerar hello Azure-lagring som innehåller hello-filer som anges i hello filePaths avsnitt.
9. För hello **argument** egenskap, ange hello argument för hello strömning jobb.
10. Hej **getDebugInfo** egenskapen är ett valfritt element. När den är inställd tooFailure laddas hello loggar endast vid fel. När den är inställd tooAlways laddas alltid loggar oavsett hello Körstatus.

> [!NOTE]
> Enligt hello exempel, anger du ett utdatauppsättningen för hello Hadoop Streaming Activity för hello **matar ut** egenskapen. Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema. Du behöver inte toospecify några inkommande dataset för hello aktivitet för hello **indata** egenskapen.  
> 
> 

## <a name="example"></a>Exempel
hello pipeline i den här genomgången körs hello ordräkning strömmande kartan/minska program på Azure HDInsight-klustret. 

### <a name="linked-services"></a>Länkade tjänster
#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
Först skapar du en länkad tjänst toolink hello Azure Storage som används av hello Azure HDInsight-kluster toohello Azure data factory. Om du kopiera och klistra in följande kod hello, Glöm inte tooreplace namn och kontonyckel med hello namnet och nyckeln för Azure Storage. 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight länkad tjänst
Därefter måste skapa du en länkad tjänst toolink din Azure HDInsight-kluster toohello Azure data factory. Om du kopiera och klistra in följande kod hello, Ersätt HDInsight-klustrets namn med hello namnet på ditt HDInsight-kluster och ändra värdena för användarens namn och lösenord. 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Datauppsättningar
#### <a name="output-dataset"></a>Datamängd för utdata
hello pipeline i det här exemplet tar inte alla indata. Du kan ange en datamängd för utdata för hello HDInsight Streaming Activity. Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pipeline
hello pipeline i det här exemplet har endast en aktivitet som är av typen: **HDInsightStreaming**. 

Hej HDInsight-kluster fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt). Som standard är namnet på hello-behållare som används av hello HDInsight-kluster hello namnet på själva hello-klustret. Om klusternamnet är myhdicluster, är namnet på hello blob-behållare som är kopplad till exempel myhdicluster.  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Se även
* [Hive-aktivitet](data-factory-hive-activity.md)
* [Pig-aktivitet](data-factory-pig-activity.md)
* [MapReduce Activity](data-factory-map-reduce.md)
* [Anropa Spark-program](data-factory-spark.md)
* [Anropa R-skript](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

