---
title: "aaaTransform data med hjälp av Hive aktivitet – Azure | Microsoft Docs"
description: "Lär dig hur du kan använda hello Hive aktivitet i ett Azure data factory toorun Hive-frågor på en på-begäran/din egen HDInsight-kluster."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 032400cdb8e8f9873f85b811b4ad7380f4410edf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Transformera data med hjälp av Hive aktivitet i Azure Data Factory 
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

Hej HDInsight Hive aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) kör Hive-frågor på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster. Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.

> [!NOTE] 
> Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln. 

## <a name="syntax"></a>Syntax

```JSON
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
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
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```
## <a name="syntax-details"></a>Information om syntax
| Egenskap | Beskrivning | Krävs |
| --- | --- | --- |
| namn |Namnet på hello-aktivitet |Ja |
| description |Text som beskriver vilka hello-aktivitet används för |Nej |
| typ |HDinsightHive |Ja |
| Indata |Indata som används av hello Hive-aktivitet |Nej |
| utdata |Utdata som genereras av hello Hive-aktivitet |Ja |
| linkedServiceName |Referens toohello HDInsight-kluster som är registrerat som en länkad tjänst i Data Factory |Ja |
| Skriptet |Ange hello Hive-skript infogade |Nej |
| sökvägen för skriptet |Store hello registreringsdatafilen i ett Azure blob storage och ange hello sökväg toohello fil. Använd egenskapen 'script' eller 'scriptPath'. Båda kan inte användas tillsammans. hello-filnamnet är skiftlägeskänsliga. |Nej |
| definierar |Ange parametrar som nyckel/värde-par för refererar till i hello Hive-skript med hjälp av 'hiveconf' |Nej |

## <a name="example"></a>Exempel
Nu ska vi titta ett exempel på spel loggar analytics där du vill att tooidentify hello tid som användes av användare spelar spel startas av ditt företag. 

hello följande loggen är en spel exempellogg, kommatecken (`,`) avgränsade och innehåller följande fält – profil-ID, SessionStart, varaktighet, SrcIPAddress och GameType hello.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Hej **registreringsdatafilen** tooprocess informationen:

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

tooexecute dessa registreringsdata skript i Data Factory-pipelinen, behöver du toodo hello följande

1. Skapa en länkad tjänst tooregister [egna HDInsight-kluster för beräkningar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller konfigurera [på begäran HDInsight beräkningskluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Vi ska anropa den här länkade tjänsten ”HDInsightLinkedService”.
2. Skapa en [länkade tjänsten](data-factory-azure-blob-connector.md) tooconfigure hello anslutning tooAzure Blob storage värd hello data. Vi ska anropa den här länkade tjänsten ”StorageLinkedService”
3. Skapa [datauppsättningar](data-factory-create-datasets.md) pekar toohello indata och hello utdata. Vi anropet hello indatauppsättning ”HiveSampleIn” och hello utdatauppsättningen ”HiveSampleOut”
4. Kopiera hello Hive-fråga som en fil tooAzure Blob Storage konfigurerade i steg #2. Om hello lagringsutrymme som värd för hello data skiljer sig från hello en värd för den här frågefilen, skapa en separat länkad Azure Storage-tjänst och kontrollera tooit i hello-aktivitet. Använd ** scriptPath ** toospecify hello sökvägen toohive frågefilen och **scriptLinkedService** toospecify hello Azure-lagring som innehåller hello skriptfilen. 
   
   > [!NOTE]
   > Du kan också tillhandahålla hello Hive-skript infogad i aktivitetsdefinitionen hello med hjälp av hello **skriptet** egenskapen. Vi rekommenderar inte den här metoden när alla specialtecken i hello skript i hello JSON-dokument måste föregås av toobe och orsak kan felsöka problem. hello bästa praxis är toofollow steg #4.
   > 
   > 
5. Skapa en pipeline med hello HDInsightHive aktivitet. hello aktivitet processer/transformeringar hello data.

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. Distribuera hello pipeline. Se [skapar pipelines](data-factory-create-pipelines.md) artikeln för information. 
7. Övervaka hello pipeline använder hello data factory övervakning och vyer för hantering. Se [övervakning och hantera Data Factory pipelines](data-factory-monitor-manage-pipelines.md) artikeln för information. 

## <a name="specifying-parameters-for-a-hive-script"></a>Ange parametrar för en Hive-skript
I det här exemplet spel loggar är inhämtas dagligen i Azure Blob Storage och lagras i en mapp som är partitionerad med datum och tid. Tooparameterize hello Hive-skript och skicka hello inkommande mapp dynamiskt under körning och även resultat hello partitionerad med datum och tid.

toouse parametriserade Hive-skript, gör följande hello

* Definiera hello parametrar i **definierar**.

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* Hello Hive-skript, se toohello parameter med hjälp av **${hiveconf:parameterName}**. 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
## <a name="see-also"></a>Se även
* [Pig-aktivitet](data-factory-pig-activity.md)
* [MapReduce Activity](data-factory-map-reduce.md)
* [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
* [Anropa Spark-program](data-factory-spark.md)
* [Anropa R-skript](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

