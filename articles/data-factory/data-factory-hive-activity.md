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
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a><span data-ttu-id="963de-103">Transformera data med hjälp av Hive aktivitet i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="963de-103">Transform data using Hive Activity in Azure Data Factory</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="963de-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="963de-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="963de-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="963de-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="963de-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="963de-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="963de-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="963de-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="963de-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="963de-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="963de-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="963de-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="963de-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="963de-114">Hej HDInsight Hive aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) kör Hive-frågor på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="963de-114">hello HDInsight Hive activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hive queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="963de-115">Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="963de-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="963de-116">Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="963de-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="963de-117">Syntax</span><span class="sxs-lookup"><span data-stu-id="963de-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="963de-118">Information om syntax</span><span class="sxs-lookup"><span data-stu-id="963de-118">Syntax details</span></span>
| <span data-ttu-id="963de-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="963de-119">Property</span></span> | <span data-ttu-id="963de-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="963de-120">Description</span></span> | <span data-ttu-id="963de-121">Krävs</span><span class="sxs-lookup"><span data-stu-id="963de-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="963de-122">namn</span><span class="sxs-lookup"><span data-stu-id="963de-122">name</span></span> |<span data-ttu-id="963de-123">Namnet på hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-123">Name of hello activity</span></span> |<span data-ttu-id="963de-124">Ja</span><span class="sxs-lookup"><span data-stu-id="963de-124">Yes</span></span> |
| <span data-ttu-id="963de-125">description</span><span class="sxs-lookup"><span data-stu-id="963de-125">description</span></span> |<span data-ttu-id="963de-126">Text som beskriver vilka hello-aktivitet används för</span><span class="sxs-lookup"><span data-stu-id="963de-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="963de-127">Nej</span><span class="sxs-lookup"><span data-stu-id="963de-127">No</span></span> |
| <span data-ttu-id="963de-128">typ</span><span class="sxs-lookup"><span data-stu-id="963de-128">type</span></span> |<span data-ttu-id="963de-129">HDinsightHive</span><span class="sxs-lookup"><span data-stu-id="963de-129">HDinsightHive</span></span> |<span data-ttu-id="963de-130">Ja</span><span class="sxs-lookup"><span data-stu-id="963de-130">Yes</span></span> |
| <span data-ttu-id="963de-131">Indata</span><span class="sxs-lookup"><span data-stu-id="963de-131">inputs</span></span> |<span data-ttu-id="963de-132">Indata som används av hello Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-132">Inputs consumed by hello Hive activity</span></span> |<span data-ttu-id="963de-133">Nej</span><span class="sxs-lookup"><span data-stu-id="963de-133">No</span></span> |
| <span data-ttu-id="963de-134">utdata</span><span class="sxs-lookup"><span data-stu-id="963de-134">outputs</span></span> |<span data-ttu-id="963de-135">Utdata som genereras av hello Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-135">Outputs produced by hello Hive activity</span></span> |<span data-ttu-id="963de-136">Ja</span><span class="sxs-lookup"><span data-stu-id="963de-136">Yes</span></span> |
| <span data-ttu-id="963de-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="963de-137">linkedServiceName</span></span> |<span data-ttu-id="963de-138">Referens toohello HDInsight-kluster som är registrerat som en länkad tjänst i Data Factory</span><span class="sxs-lookup"><span data-stu-id="963de-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="963de-139">Ja</span><span class="sxs-lookup"><span data-stu-id="963de-139">Yes</span></span> |
| <span data-ttu-id="963de-140">Skriptet</span><span class="sxs-lookup"><span data-stu-id="963de-140">script</span></span> |<span data-ttu-id="963de-141">Ange hello Hive-skript infogade</span><span class="sxs-lookup"><span data-stu-id="963de-141">Specify hello Hive script inline</span></span> |<span data-ttu-id="963de-142">Nej</span><span class="sxs-lookup"><span data-stu-id="963de-142">No</span></span> |
| <span data-ttu-id="963de-143">sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="963de-143">script path</span></span> |<span data-ttu-id="963de-144">Store hello registreringsdatafilen i ett Azure blob storage och ange hello sökväg toohello fil.</span><span class="sxs-lookup"><span data-stu-id="963de-144">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="963de-145">Använd egenskapen 'script' eller 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="963de-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="963de-146">Båda kan inte användas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="963de-146">Both cannot be used together.</span></span> <span data-ttu-id="963de-147">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="963de-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="963de-148">Nej</span><span class="sxs-lookup"><span data-stu-id="963de-148">No</span></span> |
| <span data-ttu-id="963de-149">definierar</span><span class="sxs-lookup"><span data-stu-id="963de-149">defines</span></span> |<span data-ttu-id="963de-150">Ange parametrar som nyckel/värde-par för refererar till i hello Hive-skript med hjälp av 'hiveconf'</span><span class="sxs-lookup"><span data-stu-id="963de-150">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="963de-151">Nej</span><span class="sxs-lookup"><span data-stu-id="963de-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="963de-152">Exempel</span><span class="sxs-lookup"><span data-stu-id="963de-152">Example</span></span>
<span data-ttu-id="963de-153">Nu ska vi titta ett exempel på spel loggar analytics där du vill att tooidentify hello tid som användes av användare spelar spel startas av ditt företag.</span><span class="sxs-lookup"><span data-stu-id="963de-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by users playing games launched by your company.</span></span> 

<span data-ttu-id="963de-154">hello följande loggen är en spel exempellogg, kommatecken (`,`) avgränsade och innehåller följande fält – profil-ID, SessionStart, varaktighet, SrcIPAddress och GameType hello.</span><span class="sxs-lookup"><span data-stu-id="963de-154">hello following log is a sample game log, which is comma (`,`) separated and contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="963de-155">Hej **registreringsdatafilen** tooprocess informationen:</span><span class="sxs-lookup"><span data-stu-id="963de-155">hello **Hive script** tooprocess this data:</span></span>

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

<span data-ttu-id="963de-156">tooexecute dessa registreringsdata skript i Data Factory-pipelinen, behöver du toodo hello följande</span><span class="sxs-lookup"><span data-stu-id="963de-156">tooexecute this Hive script in a Data Factory pipeline, you need toodo hello following</span></span>

1. <span data-ttu-id="963de-157">Skapa en länkad tjänst tooregister [egna HDInsight-kluster för beräkningar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller konfigurera [på begäran HDInsight beräkningskluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="963de-157">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="963de-158">Vi ska anropa den här länkade tjänsten ”HDInsightLinkedService”.</span><span class="sxs-lookup"><span data-stu-id="963de-158">Let’s call this linked service “HDInsightLinkedService”.</span></span>
2. <span data-ttu-id="963de-159">Skapa en [länkade tjänsten](data-factory-azure-blob-connector.md) tooconfigure hello anslutning tooAzure Blob storage värd hello data.</span><span class="sxs-lookup"><span data-stu-id="963de-159">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="963de-160">Vi ska anropa den här länkade tjänsten ”StorageLinkedService”</span><span class="sxs-lookup"><span data-stu-id="963de-160">Let’s call this linked service “StorageLinkedService”</span></span>
3. <span data-ttu-id="963de-161">Skapa [datauppsättningar](data-factory-create-datasets.md) pekar toohello indata och hello utdata.</span><span class="sxs-lookup"><span data-stu-id="963de-161">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="963de-162">Vi anropet hello indatauppsättning ”HiveSampleIn” och hello utdatauppsättningen ”HiveSampleOut”</span><span class="sxs-lookup"><span data-stu-id="963de-162">Let’s call hello input dataset “HiveSampleIn” and hello output dataset “HiveSampleOut”</span></span>
4. <span data-ttu-id="963de-163">Kopiera hello Hive-fråga som en fil tooAzure Blob Storage konfigurerade i steg #2.</span><span class="sxs-lookup"><span data-stu-id="963de-163">Copy hello Hive query as a file tooAzure Blob Storage configured in step #2.</span></span> <span data-ttu-id="963de-164">Om hello lagringsutrymme som värd för hello data skiljer sig från hello en värd för den här frågefilen, skapa en separat länkad Azure Storage-tjänst och kontrollera tooit i hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="963de-164">if hello storage for hosting hello data is different from hello one hosting this query file, create a separate Azure Storage linked service and refer tooit in hello activity.</span></span> <span data-ttu-id="963de-165">Använd ** scriptPath ** toospecify hello sökvägen toohive frågefilen och **scriptLinkedService** toospecify hello Azure-lagring som innehåller hello skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="963de-165">Use **scriptPath **toospecify hello path toohive query file and **scriptLinkedService** toospecify hello Azure storage that contains hello script file.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="963de-166">Du kan också tillhandahålla hello Hive-skript infogad i aktivitetsdefinitionen hello med hjälp av hello **skriptet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="963de-166">You can also provide hello Hive script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="963de-167">Vi rekommenderar inte den här metoden när alla specialtecken i hello skript i hello JSON-dokument måste föregås av toobe och orsak kan felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="963de-167">We do not recommend this approach as all special characters in hello script within hello JSON document needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="963de-168">hello bästa praxis är toofollow steg #4.</span><span class="sxs-lookup"><span data-stu-id="963de-168">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="963de-169">Skapa en pipeline med hello HDInsightHive aktivitet.</span><span class="sxs-lookup"><span data-stu-id="963de-169">Create a pipeline with hello HDInsightHive activity.</span></span> <span data-ttu-id="963de-170">hello aktivitet processer/transformeringar hello data.</span><span class="sxs-lookup"><span data-stu-id="963de-170">hello activity processes/transforms hello data.</span></span>

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
6. <span data-ttu-id="963de-171">Distribuera hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="963de-171">Deploy hello pipeline.</span></span> <span data-ttu-id="963de-172">Se [skapar pipelines](data-factory-create-pipelines.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="963de-172">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="963de-173">Övervaka hello pipeline använder hello data factory övervakning och vyer för hantering.</span><span class="sxs-lookup"><span data-stu-id="963de-173">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="963de-174">Se [övervakning och hantera Data Factory pipelines](data-factory-monitor-manage-pipelines.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="963de-174">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span> 

## <a name="specifying-parameters-for-a-hive-script"></a><span data-ttu-id="963de-175">Ange parametrar för en Hive-skript</span><span class="sxs-lookup"><span data-stu-id="963de-175">Specifying parameters for a Hive script</span></span>
<span data-ttu-id="963de-176">I det här exemplet spel loggar är inhämtas dagligen i Azure Blob Storage och lagras i en mapp som är partitionerad med datum och tid.</span><span class="sxs-lookup"><span data-stu-id="963de-176">In this example, game logs are ingested daily into Azure Blob Storage and are stored in a folder partitioned with date and time.</span></span> <span data-ttu-id="963de-177">Tooparameterize hello Hive-skript och skicka hello inkommande mapp dynamiskt under körning och även resultat hello partitionerad med datum och tid.</span><span class="sxs-lookup"><span data-stu-id="963de-177">You want tooparameterize hello Hive script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="963de-178">toouse parametriserade Hive-skript, gör följande hello</span><span class="sxs-lookup"><span data-stu-id="963de-178">toouse parameterized Hive script, do hello following</span></span>

* <span data-ttu-id="963de-179">Definiera hello parametrar i **definierar**.</span><span class="sxs-lookup"><span data-stu-id="963de-179">Define hello parameters in **defines**.</span></span>

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
* <span data-ttu-id="963de-180">Hello Hive-skript, se toohello parameter med hjälp av **${hiveconf:parameterName}**.</span><span class="sxs-lookup"><span data-stu-id="963de-180">In hello Hive Script, refer toohello parameter using **${hiveconf:parameterName}**.</span></span> 
  
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
## <a name="see-also"></a><span data-ttu-id="963de-181">Se även</span><span class="sxs-lookup"><span data-stu-id="963de-181">See Also</span></span>
* [<span data-ttu-id="963de-182">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="963de-182">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="963de-183">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="963de-183">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="963de-184">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="963de-184">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="963de-185">Anropa Spark-program</span><span class="sxs-lookup"><span data-stu-id="963de-185">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="963de-186">Anropa R-skript</span><span class="sxs-lookup"><span data-stu-id="963de-186">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

