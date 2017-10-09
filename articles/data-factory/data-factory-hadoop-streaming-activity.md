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
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="ea237-103">Transformera data med Hadoop Streaming Activity i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ea237-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="ea237-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="ea237-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="ea237-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="ea237-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="ea237-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="ea237-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="ea237-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="ea237-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="ea237-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="ea237-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="ea237-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="ea237-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="ea237-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="ea237-114">Du kan använda hello HDInsightStreamingActivity aktiviteten anropa en Hadoop Streaming job från ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="ea237-114">You can use hello HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="ea237-115">hello visar följande JSON-utdrag hello syntax för att använda hello HDInsightStreamingActivity i en pipeline-JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="ea237-115">hello following JSON snippet shows hello syntax for using hello HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="ea237-116">Hej HDInsight Streaming Activity i en Datafabrik [pipeline](data-factory-create-pipelines.md) Hadoop Streaming program körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserat HDInsight kluster.</span><span class="sxs-lookup"><span data-stu-id="ea237-116">hello HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ea237-117">Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="ea237-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="ea237-118">Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ea237-118">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="ea237-119">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="ea237-119">JSON sample</span></span>
<span data-ttu-id="ea237-120">Hej HDInsight-kluster fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="ea237-120">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="ea237-121">Som standard är namnet på hello-behållare som används av hello HDInsight-kluster hello namnet på själva hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="ea237-121">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="ea237-122">Om klusternamnet är myhdicluster, är namnet på hello blob-behållare som är kopplad till exempel myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="ea237-122">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span> 

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

<span data-ttu-id="ea237-123">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="ea237-123">Note hello following points:</span></span>

1. <span data-ttu-id="ea237-124">Ange hello **linkedServiceName** toohello namnet på hello länkade tjänst som pekar på vilka hello strömning mapreduce-jobbet körs tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ea237-124">Set hello **linkedServiceName** toohello name of hello linked service that points tooyour HDInsight cluster on which hello streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="ea237-125">Ange hello hello verksamhetens för**HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="ea237-125">Set hello type of hello activity too**HDInsightStreaming**.</span></span>
3. <span data-ttu-id="ea237-126">För hello **mapper** egenskap, ange mapper körbara hello namn.</span><span class="sxs-lookup"><span data-stu-id="ea237-126">For hello **mapper** property, specify hello name of mapper executable.</span></span> <span data-ttu-id="ea237-127">I exemplet hello är cat.exe hello mapper körbara.</span><span class="sxs-lookup"><span data-stu-id="ea237-127">In hello example, cat.exe is hello mapper executable.</span></span>
4. <span data-ttu-id="ea237-128">För hello **reducer** egenskap, ange reducer körbara hello namn.</span><span class="sxs-lookup"><span data-stu-id="ea237-128">For hello **reducer** property, specify hello name of reducer executable.</span></span> <span data-ttu-id="ea237-129">I exemplet hello är wc.exe hello reducer körbara.</span><span class="sxs-lookup"><span data-stu-id="ea237-129">In hello example, wc.exe is hello reducer executable.</span></span>
5. <span data-ttu-id="ea237-130">För hello **inkommande** egenskapen type, ange hello indatafilen (inklusive hello plats) för hello mapper.</span><span class="sxs-lookup"><span data-stu-id="ea237-130">For hello **input** type property, specify hello input file (including hello location) for hello mapper.</span></span> <span data-ttu-id="ea237-131">I exemplet hello ”: wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt”: adfsample är hello blob-behållare, exempel/data/Gutenberg är hello mapp och davinci.txt är hello-blob.</span><span class="sxs-lookup"><span data-stu-id="ea237-131">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span>
6. <span data-ttu-id="ea237-132">För hello **utdata** egenskapen type, ange hello utdatafilen (inklusive hello plats) för hello reducer.</span><span class="sxs-lookup"><span data-stu-id="ea237-132">For hello **output** type property, specify hello output file (including hello location) for hello reducer.</span></span> <span data-ttu-id="ea237-133">hello utdata från hello Hadoop Streaming job skrivs toohello plats som anges för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ea237-133">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span>
7. <span data-ttu-id="ea237-134">I hello **filePaths** ange hello sökvägar för hello mapper och reducer körbara filer.</span><span class="sxs-lookup"><span data-stu-id="ea237-134">In hello **filePaths** section, specify hello paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="ea237-135">I exemplet hello: ”adfsample/example/apps/wc.exe” adfsample är hello blob-behållare, exempel/appar är hello mapp och wc.exe är hello körbara.</span><span class="sxs-lookup"><span data-stu-id="ea237-135">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span>
8. <span data-ttu-id="ea237-136">För hello **fileLinkedService** egenskap, ange hello Azure Storage länkade tjänst som representerar hello Azure-lagring som innehåller hello-filer som anges i hello filePaths avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ea237-136">For hello **fileLinkedService** property, specify hello Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span>
9. <span data-ttu-id="ea237-137">För hello **argument** egenskap, ange hello argument för hello strömning jobb.</span><span class="sxs-lookup"><span data-stu-id="ea237-137">For hello **arguments** property, specify hello arguments for hello streaming job.</span></span>
10. <span data-ttu-id="ea237-138">Hej **getDebugInfo** egenskapen är ett valfritt element.</span><span class="sxs-lookup"><span data-stu-id="ea237-138">hello **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="ea237-139">När den är inställd tooFailure laddas hello loggar endast vid fel.</span><span class="sxs-lookup"><span data-stu-id="ea237-139">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="ea237-140">När den är inställd tooAlways laddas alltid loggar oavsett hello Körstatus.</span><span class="sxs-lookup"><span data-stu-id="ea237-140">When it is set tooAlways, logs are always downloaded irrespective of hello execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="ea237-141">Enligt hello exempel, anger du ett utdatauppsättningen för hello Hadoop Streaming Activity för hello **matar ut** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ea237-141">As shown in hello example, you specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="ea237-142">Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema.</span><span class="sxs-lookup"><span data-stu-id="ea237-142">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> <span data-ttu-id="ea237-143">Du behöver inte toospecify några inkommande dataset för hello aktivitet för hello **indata** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ea237-143">You do not need toospecify any input dataset for hello activity for hello **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="ea237-144">Exempel</span><span class="sxs-lookup"><span data-stu-id="ea237-144">Example</span></span>
<span data-ttu-id="ea237-145">hello pipeline i den här genomgången körs hello ordräkning strömmande kartan/minska program på Azure HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="ea237-145">hello pipeline in this walkthrough runs hello Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="ea237-146">Länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="ea237-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="ea237-147">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="ea237-147">Azure Storage linked service</span></span>
<span data-ttu-id="ea237-148">Först skapar du en länkad tjänst toolink hello Azure Storage som används av hello Azure HDInsight-kluster toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="ea237-148">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="ea237-149">Om du kopiera och klistra in följande kod hello, Glöm inte tooreplace namn och kontonyckel med hello namnet och nyckeln för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ea237-149">If you copy/paste hello following code, do not forget tooreplace account name and account key with hello name and key of your Azure Storage.</span></span> 

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="ea237-150">Azure HDInsight länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="ea237-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="ea237-151">Därefter måste skapa du en länkad tjänst toolink din Azure HDInsight-kluster toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="ea237-151">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="ea237-152">Om du kopiera och klistra in följande kod hello, Ersätt HDInsight-klustrets namn med hello namnet på ditt HDInsight-kluster och ändra värdena för användarens namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ea237-152">If you copy/paste hello following code, replace HDInsight cluster name with hello name of your HDInsight cluster, and change user name and password values.</span></span> 

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

### <a name="datasets"></a><span data-ttu-id="ea237-153">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="ea237-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="ea237-154">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="ea237-154">Output dataset</span></span>
<span data-ttu-id="ea237-155">hello pipeline i det här exemplet tar inte alla indata.</span><span class="sxs-lookup"><span data-stu-id="ea237-155">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="ea237-156">Du kan ange en datamängd för utdata för hello HDInsight Streaming Activity.</span><span class="sxs-lookup"><span data-stu-id="ea237-156">You specify an output dataset for hello HDInsight Streaming Activity.</span></span> <span data-ttu-id="ea237-157">Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema.</span><span class="sxs-lookup"><span data-stu-id="ea237-157">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span> 

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

### <a name="pipeline"></a><span data-ttu-id="ea237-158">Pipeline</span><span class="sxs-lookup"><span data-stu-id="ea237-158">Pipeline</span></span>
<span data-ttu-id="ea237-159">hello pipeline i det här exemplet har endast en aktivitet som är av typen: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="ea237-159">hello pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="ea237-160">Hej HDInsight-kluster fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="ea237-160">hello HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="ea237-161">Som standard är namnet på hello-behållare som används av hello HDInsight-kluster hello namnet på själva hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="ea237-161">By default, name of hello container that is used by hello HDInsight cluster is hello name of hello cluster itself.</span></span> <span data-ttu-id="ea237-162">Om klusternamnet är myhdicluster, är namnet på hello blob-behållare som är kopplad till exempel myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="ea237-162">For example, if your cluster name is myhdicluster, name of hello blob container associated would be myhdicluster.</span></span>  

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
## <a name="see-also"></a><span data-ttu-id="ea237-163">Se även</span><span class="sxs-lookup"><span data-stu-id="ea237-163">See Also</span></span>
* [<span data-ttu-id="ea237-164">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="ea237-165">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ea237-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="ea237-166">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="ea237-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="ea237-167">Anropa Spark-program</span><span class="sxs-lookup"><span data-stu-id="ea237-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="ea237-168">Anropa R-skript</span><span class="sxs-lookup"><span data-stu-id="ea237-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

