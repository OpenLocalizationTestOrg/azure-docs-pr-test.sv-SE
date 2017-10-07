---
title: "aaaTransform data med hjälp av Pig-aktiviteten i Azure Data Factory | Microsoft Docs"
description: "Lär dig hur du kan använda hello Pig aktivitet i ett Azure data factory toorun Pig-skript på en på-begäran/din egen HDInsight-kluster."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 3ad096c4a9e8603b09f574f6d129b4339a75d381
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Transformera data med hjälp av Pig-aktiviteten i Azure Data Factory
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

Hej HDInsight Pig aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) Pig frågor körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster. Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.

> [!NOTE] 
> Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln. 

## <a name="syntax"></a>Syntax

```JSON
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```
## <a name="syntax-details"></a>Information om syntax
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello-aktivitet |Ja |
| description |Text som beskriver vilka hello-aktivitet används för |Nej |
| typ |HDinsightPig |Ja |
| Indata |En eller flera inmatningar som används av hello Pig-aktivitet |Nej |
| utdata |En eller flera utdata som genereras av hello Pig-aktivitet |Ja |
| linkedServiceName |Referens toohello HDInsight-kluster som är registrerat som en länkad tjänst i Data Factory |Ja |
| Skriptet |Ange hello Pig-skriptet infogade |Nej |
| sökvägen för skriptet |Lagra hello Pig-skriptet i en Azure blob storage och ange hello sökväg toohello fil. Använd egenskapen 'script' eller 'scriptPath'. Båda kan inte användas tillsammans. hello-filnamnet är skiftlägeskänsliga. |Nej |
| definierar |Ange parametrar som nyckel/värde-par för refererar till i hello Pig-skriptet |Nej |

## <a name="example"></a>Exempel
Nu ska vi titta ett exempel på spel loggar analytics där du vill att tooidentify hello tid som användes av spelare spelar spel startas av ditt företag.

hello följande spel exempellogg är en åt med kommatecken (,). Den innehåller följande fält – profil-ID, SessionStart, varaktighet, SrcIPAddress och GameType hello.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hej **svin skriptet** tooprocess informationen:

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

den här Pig-skript i Data Factory-pipelinen tooexecute hello följande steg:

1. Skapa en länkad tjänst tooregister [egna HDInsight-kluster för beräkningar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller konfigurera [på begäran HDInsight beräkningskluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Vi ska anropa den här länkade tjänsten **HDInsightLinkedService**.
2. Skapa en [länkade tjänsten](data-factory-azure-blob-connector.md) tooconfigure hello anslutning tooAzure Blob storage värd hello data. Vi ska anropa den här länkade tjänsten **StorageLinkedService**.
3. Skapa [datauppsättningar](data-factory-create-datasets.md) pekar toohello indata och hello utdata. Vi ringer hello inkommande dataset **PigSampleIn** och hello utdatauppsättningen **PigSampleOut**.
4. Kopiera hello Pig frågan i en fil hello Azure Blob Storage är konfigurerade i steg #2. Om hello Azure-lagring som är värd för hello data skiljer sig från hello något som är värd för hello frågefilen, skapar du en separat länkad Azure Storage-tjänst. Läs toohello länkad tjänst i hello aktivitet konfiguration. Använd ** scriptPath ** toospecify hello sökvägen toopig skriptfilen och **scriptLinkedService**. 
   
   > [!NOTE]
   > Du kan också tillhandahålla hello Pig-skriptet infogad i aktivitetsdefinitionen hello med hjälp av hello **skriptet** egenskapen. Men rekommenderar vi inte den här metoden som alla specialtecken i hello-skriptet måste toobe undantaget och kan orsaka problem för felsökning. hello bästa praxis är toofollow steg #4.
   > 
   > 
5. Skapa hello pipeline med hello HDInsightPig aktivitet. Den här aktiviteten bearbetar hello indata genom att köra Pig-skriptet på HDInsight-kluster.

    ```JSON   
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
        ]
      }
    } 
    ```
6. Distribuera hello pipeline. Se [skapar pipelines](data-factory-create-pipelines.md) artikeln för information. 
7. Övervaka hello pipeline använder hello data factory övervakning och vyer för hantering. Se [övervakning och hantera Data Factory pipelines](data-factory-monitor-manage-pipelines.md) artikeln för information.

## <a name="specifying-parameters-for-a-pig-script"></a>Ange parametrar för Pig-skriptet
Överväg följande exempel hello: spel loggar inhämtas dagligen i Azure Blob Storage och lagras i en mapp partitionerade utifrån datum och tid. Tooparameterize hello Pig-skriptet och skicka hello inkommande mapp dynamiskt under körning och även resultat hello partitionerad med datum och tid.

toouse parametriserade Pig-skriptet, göra hello följande:

* Definiera hello parametrar i **definierar**.

    ```JSON  
    {
        "name": "PigActivitySamplePipeline",
          "properties": {
        "activities": [
            {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                      {
                        "name": "PigSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "PigSampleOut"
                      }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      }
                },
                   "scheduler": {
                      "frequency": "Day",
                      "interval": 1
                }
              }
        ]
      }
    }
    ```  
* I hello Pig-skriptet, referera toohello parametrar med '**$parameterName**' som visas i följande exempel hello:

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a>Se även
* [Hive-aktivitet](data-factory-hive-activity.md)
* [MapReduce Activity](data-factory-map-reduce.md)
* [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
* [Anropa Spark-program](data-factory-spark.md)
* [Anropa R-skript](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

