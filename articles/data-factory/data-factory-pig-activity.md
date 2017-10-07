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
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a><span data-ttu-id="f1020-103">Transformera data med hjälp av Pig-aktiviteten i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f1020-103">Transform data using Pig Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="f1020-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="f1020-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="f1020-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="f1020-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="f1020-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="f1020-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="f1020-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="f1020-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="f1020-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="f1020-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="f1020-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="f1020-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="f1020-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="f1020-114">Hej HDInsight Pig aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) Pig frågor körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f1020-114">hello HDInsight Pig activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Pig queries on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="f1020-115">Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f1020-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="f1020-116">Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f1020-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="syntax"></a><span data-ttu-id="f1020-117">Syntax</span><span class="sxs-lookup"><span data-stu-id="f1020-117">Syntax</span></span>

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
## <a name="syntax-details"></a><span data-ttu-id="f1020-118">Information om syntax</span><span class="sxs-lookup"><span data-stu-id="f1020-118">Syntax details</span></span>
| <span data-ttu-id="f1020-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f1020-119">Property</span></span> | <span data-ttu-id="f1020-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1020-120">Description</span></span> | <span data-ttu-id="f1020-121">Krävs</span><span class="sxs-lookup"><span data-stu-id="f1020-121">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1020-122">namn</span><span class="sxs-lookup"><span data-stu-id="f1020-122">name</span></span> |<span data-ttu-id="f1020-123">Namnet på hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-123">Name of hello activity</span></span> |<span data-ttu-id="f1020-124">Ja</span><span class="sxs-lookup"><span data-stu-id="f1020-124">Yes</span></span> |
| <span data-ttu-id="f1020-125">description</span><span class="sxs-lookup"><span data-stu-id="f1020-125">description</span></span> |<span data-ttu-id="f1020-126">Text som beskriver vilka hello-aktivitet används för</span><span class="sxs-lookup"><span data-stu-id="f1020-126">Text describing what hello activity is used for</span></span> |<span data-ttu-id="f1020-127">Nej</span><span class="sxs-lookup"><span data-stu-id="f1020-127">No</span></span> |
| <span data-ttu-id="f1020-128">typ</span><span class="sxs-lookup"><span data-stu-id="f1020-128">type</span></span> |<span data-ttu-id="f1020-129">HDinsightPig</span><span class="sxs-lookup"><span data-stu-id="f1020-129">HDinsightPig</span></span> |<span data-ttu-id="f1020-130">Ja</span><span class="sxs-lookup"><span data-stu-id="f1020-130">Yes</span></span> |
| <span data-ttu-id="f1020-131">Indata</span><span class="sxs-lookup"><span data-stu-id="f1020-131">inputs</span></span> |<span data-ttu-id="f1020-132">En eller flera inmatningar som används av hello Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-132">One or more inputs consumed by hello Pig activity</span></span> |<span data-ttu-id="f1020-133">Nej</span><span class="sxs-lookup"><span data-stu-id="f1020-133">No</span></span> |
| <span data-ttu-id="f1020-134">utdata</span><span class="sxs-lookup"><span data-stu-id="f1020-134">outputs</span></span> |<span data-ttu-id="f1020-135">En eller flera utdata som genereras av hello Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-135">One or more outputs produced by hello Pig activity</span></span> |<span data-ttu-id="f1020-136">Ja</span><span class="sxs-lookup"><span data-stu-id="f1020-136">Yes</span></span> |
| <span data-ttu-id="f1020-137">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="f1020-137">linkedServiceName</span></span> |<span data-ttu-id="f1020-138">Referens toohello HDInsight-kluster som är registrerat som en länkad tjänst i Data Factory</span><span class="sxs-lookup"><span data-stu-id="f1020-138">Reference toohello HDInsight cluster registered as a linked service in Data Factory</span></span> |<span data-ttu-id="f1020-139">Ja</span><span class="sxs-lookup"><span data-stu-id="f1020-139">Yes</span></span> |
| <span data-ttu-id="f1020-140">Skriptet</span><span class="sxs-lookup"><span data-stu-id="f1020-140">script</span></span> |<span data-ttu-id="f1020-141">Ange hello Pig-skriptet infogade</span><span class="sxs-lookup"><span data-stu-id="f1020-141">Specify hello Pig script inline</span></span> |<span data-ttu-id="f1020-142">Nej</span><span class="sxs-lookup"><span data-stu-id="f1020-142">No</span></span> |
| <span data-ttu-id="f1020-143">sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="f1020-143">script path</span></span> |<span data-ttu-id="f1020-144">Lagra hello Pig-skriptet i en Azure blob storage och ange hello sökväg toohello fil.</span><span class="sxs-lookup"><span data-stu-id="f1020-144">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="f1020-145">Använd egenskapen 'script' eller 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="f1020-145">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="f1020-146">Båda kan inte användas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="f1020-146">Both cannot be used together.</span></span> <span data-ttu-id="f1020-147">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f1020-147">hello file name is case-sensitive.</span></span> |<span data-ttu-id="f1020-148">Nej</span><span class="sxs-lookup"><span data-stu-id="f1020-148">No</span></span> |
| <span data-ttu-id="f1020-149">definierar</span><span class="sxs-lookup"><span data-stu-id="f1020-149">defines</span></span> |<span data-ttu-id="f1020-150">Ange parametrar som nyckel/värde-par för refererar till i hello Pig-skriptet</span><span class="sxs-lookup"><span data-stu-id="f1020-150">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="f1020-151">Nej</span><span class="sxs-lookup"><span data-stu-id="f1020-151">No</span></span> |

## <a name="example"></a><span data-ttu-id="f1020-152">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1020-152">Example</span></span>
<span data-ttu-id="f1020-153">Nu ska vi titta ett exempel på spel loggar analytics där du vill att tooidentify hello tid som användes av spelare spelar spel startas av ditt företag.</span><span class="sxs-lookup"><span data-stu-id="f1020-153">Let’s consider an example of game logs analytics where you want tooidentify hello time spent by players playing games launched by your company.</span></span>

<span data-ttu-id="f1020-154">hello följande spel exempellogg är en åt med kommatecken (,).</span><span class="sxs-lookup"><span data-stu-id="f1020-154">hello following sample game log is a comma (,) separated file.</span></span> <span data-ttu-id="f1020-155">Den innehåller följande fält – profil-ID, SessionStart, varaktighet, SrcIPAddress och GameType hello.</span><span class="sxs-lookup"><span data-stu-id="f1020-155">It contains hello following fields – ProfileID, SessionStart, Duration, SrcIPAddress, and GameType.</span></span>

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

<span data-ttu-id="f1020-156">Hej **svin skriptet** tooprocess informationen:</span><span class="sxs-lookup"><span data-stu-id="f1020-156">hello **Pig script** tooprocess this data:</span></span>

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

<span data-ttu-id="f1020-157">den här Pig-skript i Data Factory-pipelinen tooexecute hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f1020-157">tooexecute this Pig script in a Data Factory pipeline, do hello following steps:</span></span>

1. <span data-ttu-id="f1020-158">Skapa en länkad tjänst tooregister [egna HDInsight-kluster för beräkningar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller konfigurera [på begäran HDInsight beräkningskluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="f1020-158">Create a linked service tooregister [your own HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or configure [on-demand HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span></span> <span data-ttu-id="f1020-159">Vi ska anropa den här länkade tjänsten **HDInsightLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f1020-159">Let’s call this linked service **HDInsightLinkedService**.</span></span>
2. <span data-ttu-id="f1020-160">Skapa en [länkade tjänsten](data-factory-azure-blob-connector.md) tooconfigure hello anslutning tooAzure Blob storage värd hello data.</span><span class="sxs-lookup"><span data-stu-id="f1020-160">Create a [linked service](data-factory-azure-blob-connector.md) tooconfigure hello connection tooAzure Blob storage hosting hello data.</span></span> <span data-ttu-id="f1020-161">Vi ska anropa den här länkade tjänsten **StorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f1020-161">Let’s call this linked service **StorageLinkedService**.</span></span>
3. <span data-ttu-id="f1020-162">Skapa [datauppsättningar](data-factory-create-datasets.md) pekar toohello indata och hello utdata.</span><span class="sxs-lookup"><span data-stu-id="f1020-162">Create [datasets](data-factory-create-datasets.md) pointing toohello input and hello output data.</span></span> <span data-ttu-id="f1020-163">Vi ringer hello inkommande dataset **PigSampleIn** och hello utdatauppsättningen **PigSampleOut**.</span><span class="sxs-lookup"><span data-stu-id="f1020-163">Let’s call hello input dataset **PigSampleIn** and hello output dataset **PigSampleOut**.</span></span>
4. <span data-ttu-id="f1020-164">Kopiera hello Pig frågan i en fil hello Azure Blob Storage är konfigurerade i steg #2.</span><span class="sxs-lookup"><span data-stu-id="f1020-164">Copy hello Pig query in a file hello Azure Blob Storage configured in step #2.</span></span> <span data-ttu-id="f1020-165">Om hello Azure-lagring som är värd för hello data skiljer sig från hello något som är värd för hello frågefilen, skapar du en separat länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f1020-165">If hello Azure storage that hosts hello data is different from hello one that hosts hello query file, create a separate Azure Storage linked service.</span></span> <span data-ttu-id="f1020-166">Läs toohello länkad tjänst i hello aktivitet konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f1020-166">Refer toohello linked service in hello activity configuration.</span></span> <span data-ttu-id="f1020-167">Använd ** scriptPath ** toospecify hello sökvägen toopig skriptfilen och **scriptLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f1020-167">Use **scriptPath **toospecify hello path toopig script file and **scriptLinkedService**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="f1020-168">Du kan också tillhandahålla hello Pig-skriptet infogad i aktivitetsdefinitionen hello med hjälp av hello **skriptet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f1020-168">You can also provide hello Pig script inline in hello activity definition by using hello **script** property.</span></span> <span data-ttu-id="f1020-169">Men rekommenderar vi inte den här metoden som alla specialtecken i hello-skriptet måste toobe undantaget och kan orsaka problem för felsökning.</span><span class="sxs-lookup"><span data-stu-id="f1020-169">However, we do not recommend this approach as all special characters in hello script needs toobe escaped and may cause debugging issues.</span></span> <span data-ttu-id="f1020-170">hello bästa praxis är toofollow steg #4.</span><span class="sxs-lookup"><span data-stu-id="f1020-170">hello best practice is toofollow step #4.</span></span>
   > 
   > 
5. <span data-ttu-id="f1020-171">Skapa hello pipeline med hello HDInsightPig aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f1020-171">Create hello pipeline with hello HDInsightPig activity.</span></span> <span data-ttu-id="f1020-172">Den här aktiviteten bearbetar hello indata genom att köra Pig-skriptet på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f1020-172">This activity processes hello input data by running Pig script on HDInsight cluster.</span></span>

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
6. <span data-ttu-id="f1020-173">Distribuera hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="f1020-173">Deploy hello pipeline.</span></span> <span data-ttu-id="f1020-174">Se [skapar pipelines](data-factory-create-pipelines.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f1020-174">See [Creating pipelines](data-factory-create-pipelines.md) article for details.</span></span> 
7. <span data-ttu-id="f1020-175">Övervaka hello pipeline använder hello data factory övervakning och vyer för hantering.</span><span class="sxs-lookup"><span data-stu-id="f1020-175">Monitor hello pipeline using hello data factory monitoring and management views.</span></span> <span data-ttu-id="f1020-176">Se [övervakning och hantera Data Factory pipelines](data-factory-monitor-manage-pipelines.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f1020-176">See [Monitoring and manage Data Factory pipelines](data-factory-monitor-manage-pipelines.md) article for details.</span></span>

## <a name="specifying-parameters-for-a-pig-script"></a><span data-ttu-id="f1020-177">Ange parametrar för Pig-skriptet</span><span class="sxs-lookup"><span data-stu-id="f1020-177">Specifying parameters for a Pig script</span></span>
<span data-ttu-id="f1020-178">Överväg följande exempel hello: spel loggar inhämtas dagligen i Azure Blob Storage och lagras i en mapp partitionerade utifrån datum och tid.</span><span class="sxs-lookup"><span data-stu-id="f1020-178">Consider hello following example: game logs are ingested daily into Azure Blob Storage and stored in a folder partitioned based on date and time.</span></span> <span data-ttu-id="f1020-179">Tooparameterize hello Pig-skriptet och skicka hello inkommande mapp dynamiskt under körning och även resultat hello partitionerad med datum och tid.</span><span class="sxs-lookup"><span data-stu-id="f1020-179">You want tooparameterize hello Pig script and pass hello input folder location dynamically during runtime and also produce hello output partitioned with date and time.</span></span>

<span data-ttu-id="f1020-180">toouse parametriserade Pig-skriptet, göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="f1020-180">toouse parameterized Pig script, do hello following:</span></span>

* <span data-ttu-id="f1020-181">Definiera hello parametrar i **definierar**.</span><span class="sxs-lookup"><span data-stu-id="f1020-181">Define hello parameters in **defines**.</span></span>

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
* <span data-ttu-id="f1020-182">I hello Pig-skriptet, referera toohello parametrar med '**$parameterName**' som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="f1020-182">In hello Pig Script, refer toohello parameters using '**$parameterName**' as shown in hello following example:</span></span>

    ```  
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);    
    GroupProfile = Group PigSampleIn all;        
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);        
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```
## <a name="see-also"></a><span data-ttu-id="f1020-183">Se även</span><span class="sxs-lookup"><span data-stu-id="f1020-183">See Also</span></span>
* [<span data-ttu-id="f1020-184">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1020-184">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="f1020-185">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="f1020-185">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="f1020-186">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="f1020-186">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="f1020-187">Anropa Spark-program</span><span class="sxs-lookup"><span data-stu-id="f1020-187">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="f1020-188">Anropa R-skript</span><span class="sxs-lookup"><span data-stu-id="f1020-188">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

