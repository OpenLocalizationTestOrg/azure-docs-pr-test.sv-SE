---
title: "aaaInvoke MapReduce Program från Azure Data Factory"
description: "Lär dig hur tooprocess data genom att köra MapReduce program på en Azure HDInsight-kluster från en Azure data factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 448ef93a10bd97e7ecd4be4f04f88f8a05decc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Anropa MapReduce program från Data Factory
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

Hej HDInsight MapReduce aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) MapReduce-program körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster. Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.

> [!NOTE] 
> Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.  

## <a name="introduction"></a>Introduktion
En pipeline i ett Azure data factory bearbetar data i länkade storage-tjänster med hjälp av länkade beräknings-tjänster. Den innehåller en serie aktiviteter där varje aktivitet utför en specifik bearbetning. Den här artikeln beskriver hur du använder hello HDInsight MapReduce Activity.

Se [Pig](data-factory-pig-activity.md) och [Hive](data-factory-hive-activity.md) mer information om hur du kör Pig/Hive skript på en Windows/Linux-baserade HDInsight-kluster från en rörledning HDInsight Pig och Hive aktiviteter. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON för HDInsight MapReduce Activity
I hello JSON-definitionen för hello HDInsight aktiviteten: 

1. Ange hello **typen** av hello **aktiviteten** för**HDInsight**.
2. Ange hello namn hello-klass för **className** egenskapen.
3. Ange hello sökvägen toohello JAR-filen inklusive hello filnamnet för **jarFilePath** egenskapen.
4. Ange hello länkade tjänst som refererar toohello Azure Blob Storage som innehåller hello JAR-filen för **jarLinkedService** egenskapen.   
5. Ange några argument för hello MapReduce program i hello **argument** avsnitt. Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket. toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden).

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix toodetermine hello similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce toogenerate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
Du kan använda hello HDInsight MapReduce Activity toorun alla MapReduce jar-filen på ett HDInsight-kluster. Följande exempel JSON-definitionen för en pipeline hello HDInsight aktiviteten är konfigurerade hello toorun Mahout JAR-filen.

## <a name="sample-on-github"></a>Exempel på GitHub
Du kan hämta ett exempel för att använda hello HDInsight MapReduce Activity från: [Data Factory exemplen på GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-hello-word-count-program"></a>Kör hello ordräkning program
hello pipeline i det här exemplet körs hello ordräkning kartan/minska program på Azure HDInsight-klustret.   

### <a name="linked-services"></a>Länkade tjänster
Först skapar du en länkad tjänst toolink hello Azure Storage som används av hello Azure HDInsight-kluster toohello Azure data factory. Om du kopiera och klistra in följande kod hello, Glöm inte tooreplace **kontonamn** och **kontonyckel** med hello namn och nyckel för Azure Storage. 

#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst

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
Därefter måste skapa du en länkad tjänst toolink din Azure HDInsight-kluster toohello Azure data factory. Om du kopiera och klistra in följande kod hello, ersätter **HDInsight-klustrets namn** med hello namnet på ditt HDInsight-kluster och ändra användarens namn och lösenord värden.   

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
hello pipeline i det här exemplet tar inte alla indata. Du kan ange en datamängd för utdata för hello HDInsight MapReduce Activity. Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pipeline
hello pipeline i det här exemplet har endast en aktivitet som är av typen: HDInsightMapReduce. Några av hello viktiga egenskaper i hello JSON är: 

| Egenskap | Anteckningar |
|:--- |:--- |
| typ |hello måste anges för**HDInsightMapReduce**. |
| Klassnamn |Namnet på klassen hello är: **wordcount** |
| jarFilePath |Sökvägen toohello jar-filen som innehåller hello-klass. Om du kopiera och klistra in följande kod hello, Glöm inte toochange hello klustrets hello namn. |
| jarLinkedService |Azure Storage länkade tjänst som innehåller hello jar-filen. Den här länkade tjänsten refererar toohello lagring som är associerad med hello HDInsight-kluster. |
| Argument |Hej hämtas wordcount två argument, indata och utdata. hello indatafilen är hello davinci.txt fil. |
| frekvens/intervall |hello värdena för dessa egenskaper matchar hello datamängd för utdata. |
| linkedServiceName |refererar toohello HDInsight länkade-tjänst som du har skapat tidigare. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun hello Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Köra Spark-program
Du kan använda MapReduce aktiviteten toorun Spark-program på HDInsight Spark-klustret. Mer information finns i [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) (Anropa Spark-program från Azure Data Factory).  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a>Se även
* [Hive-aktivitet](data-factory-hive-activity.md)
* [Pig-aktivitet](data-factory-pig-activity.md)
* [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
* [Anropa Spark-program](data-factory-spark.md)
* [Anropa R-skript](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

