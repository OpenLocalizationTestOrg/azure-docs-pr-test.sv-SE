---
title: Transformera data med Hadoop Streaming Activity - Azure | Microsoft Docs
description: "Lär dig hur du kan använda Hadoop Streaming Activity i ett Azure data factory för att omvandla data med Hadoop Streaming program som körs på en på-begäran/din egen HDInsight-kluster."
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
ms.openlocfilehash: bfe62aa60f5a0ff339e1d495d22a5fdfac10d5dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a><span data-ttu-id="23c2c-103">Transformera data med Hadoop Streaming Activity i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="23c2c-103">Transform data using Hadoop Streaming Activity in Azure Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="23c2c-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="23c2c-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="23c2c-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="23c2c-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="23c2c-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="23c2c-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="23c2c-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="23c2c-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="23c2c-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="23c2c-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="23c2c-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="23c2c-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="23c2c-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="23c2c-114">Du kan använda HDInsightStreamingActivity aktiviteten anropa en Hadoop Streaming job från ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-114">You can use the HDInsightStreamingActivity Activity invoke a Hadoop Streaming job from an Azure Data Factory pipeline.</span></span> <span data-ttu-id="23c2c-115">Följande JSON-utdrag visar syntaxen för HDInsightStreamingActivity i en pipeline-JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="23c2c-115">The following JSON snippet shows the syntax for using the HDInsightStreamingActivity in a pipeline JSON file.</span></span> 

<span data-ttu-id="23c2c-116">HDInsight Streaming Activity i en Datafabrik [pipeline](data-factory-create-pipelines.md) Hadoop Streaming program körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="23c2c-116">The HDInsight Streaming Activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes Hadoop Streaming programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="23c2c-117">Den här artikeln bygger på den [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och stöds omvandling aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="23c2c-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="23c2c-118">Om du har använt Azure Data Factory, Läs igenom [introduktion till Azure Data Factory](data-factory-introduction.md) och gör kursen: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="23c2c-118">If you are new to Azure Data Factory, read through [Introduction to Azure Data Factory](data-factory-introduction.md) and do the tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span> 

## <a name="json-sample"></a><span data-ttu-id="23c2c-119">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="23c2c-119">JSON sample</span></span>
<span data-ttu-id="23c2c-120">HDInsight-klustret fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="23c2c-120">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="23c2c-121">Som standard är namnet på behållaren som används av HDInsight-klustret namn för själva klustret.</span><span class="sxs-lookup"><span data-stu-id="23c2c-121">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="23c2c-122">Om klusternamnet är myhdicluster, är namnet på blobbehållare som är kopplad till exempel myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="23c2c-122">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span> 

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

<span data-ttu-id="23c2c-123">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="23c2c-123">Note the following points:</span></span>

1. <span data-ttu-id="23c2c-124">Ange den **linkedServiceName** till namnet på den länkade tjänst som pekar på ditt HDInsight-kluster som strömmande mapreduce-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="23c2c-124">Set the **linkedServiceName** to the name of the linked service that points to your HDInsight cluster on which the streaming mapreduce job is run.</span></span>
2. <span data-ttu-id="23c2c-125">Ange vilken typ av aktivitet till **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="23c2c-125">Set the type of the activity to **HDInsightStreaming**.</span></span>
3. <span data-ttu-id="23c2c-126">För den **mapper** egenskap, ange namnet på mapper körbara.</span><span class="sxs-lookup"><span data-stu-id="23c2c-126">For the **mapper** property, specify the name of mapper executable.</span></span> <span data-ttu-id="23c2c-127">I det här exemplet är cat.exe mapper körbara.</span><span class="sxs-lookup"><span data-stu-id="23c2c-127">In the example, cat.exe is the mapper executable.</span></span>
4. <span data-ttu-id="23c2c-128">För den **reducer** egenskap, ange namnet på reducer körbara.</span><span class="sxs-lookup"><span data-stu-id="23c2c-128">For the **reducer** property, specify the name of reducer executable.</span></span> <span data-ttu-id="23c2c-129">I det här exemplet är wc.exe reducer körbara.</span><span class="sxs-lookup"><span data-stu-id="23c2c-129">In the example, wc.exe is the reducer executable.</span></span>
5. <span data-ttu-id="23c2c-130">För den **inkommande** egenskapen type, ange indatafilen (inklusive platsen) för mapparen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-130">For the **input** type property, specify the input file (including the location) for the mapper.</span></span> <span data-ttu-id="23c2c-131">Exempel ”: wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt”: adfsample är blobbehållare, exempel/data/Gutenberg är mappen och davinci.txt är blob.</span><span class="sxs-lookup"><span data-stu-id="23c2c-131">In the example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is the blob container, example/data/Gutenberg is the folder, and davinci.txt is the blob.</span></span>
6. <span data-ttu-id="23c2c-132">För den **utdata** egenskapen type, ange utdatafilen (inklusive platsen) för reducer.</span><span class="sxs-lookup"><span data-stu-id="23c2c-132">For the **output** type property, specify the output file (including the location) for the reducer.</span></span> <span data-ttu-id="23c2c-133">Utdata för direktuppspelning av Hadoop-jobb skrivs till den plats som anges för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-133">The output of the Hadoop Streaming job is written to the location specified for this property.</span></span>
7. <span data-ttu-id="23c2c-134">I den **filePaths** ange sökvägar för mappning och reducer körbara filer.</span><span class="sxs-lookup"><span data-stu-id="23c2c-134">In the **filePaths** section, specify the paths for the mapper and reducer executables.</span></span> <span data-ttu-id="23c2c-135">Exempel: ”adfsample/example/apps/wc.exe” adfsample är blobbehållare, exempel/appar är mappen och wc.exe är den körbara filen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-135">In the example: "adfsample/example/apps/wc.exe", adfsample is the blob container, example/apps is the folder, and wc.exe is the executable.</span></span>
8. <span data-ttu-id="23c2c-136">För den **fileLinkedService** egenskap, ange länkad Azure Storage-tjänst som representerar den Azure-lagring som innehåller de filer som anges i avsnittet filePaths.</span><span class="sxs-lookup"><span data-stu-id="23c2c-136">For the **fileLinkedService** property, specify the Azure Storage linked service that represents the Azure storage that contains the files specified in the filePaths section.</span></span>
9. <span data-ttu-id="23c2c-137">För den **argument** egenskap, ange argument för direktuppspelningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="23c2c-137">For the **arguments** property, specify the arguments for the streaming job.</span></span>
10. <span data-ttu-id="23c2c-138">Den **getDebugInfo** egenskapen är ett valfritt element.</span><span class="sxs-lookup"><span data-stu-id="23c2c-138">The **getDebugInfo** property is an optional element.</span></span> <span data-ttu-id="23c2c-139">När den är inställd på fel laddas i loggarna ned endast vid fel.</span><span class="sxs-lookup"><span data-stu-id="23c2c-139">When it is set to Failure, the logs are downloaded only on failure.</span></span> <span data-ttu-id="23c2c-140">När den är inställd på alltid laddas alltid loggar oavsett körningsstatusen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-140">When it is set to Always, logs are always downloaded irrespective of the execution status.</span></span>

> [!NOTE]
> <span data-ttu-id="23c2c-141">Som du ser i exemplet kan du ange en datamängd för utdata för Hadoop Streaming Activity för den **matar ut** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-141">As shown in the example, you specify an output dataset for the Hadoop Streaming Activity for the **outputs** property.</span></span> <span data-ttu-id="23c2c-142">Den här datauppsättningen är bara en dummy datamängd som krävs för att driva pipeline-schemat.</span><span class="sxs-lookup"><span data-stu-id="23c2c-142">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> <span data-ttu-id="23c2c-143">Du behöver inte ange några inkommande dataset för aktiviteten för den **indata** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="23c2c-143">You do not need to specify any input dataset for the activity for the **inputs** property.</span></span>  
> 
> 

## <a name="example"></a><span data-ttu-id="23c2c-144">Exempel</span><span class="sxs-lookup"><span data-stu-id="23c2c-144">Example</span></span>
<span data-ttu-id="23c2c-145">Ordräkning strömmande kartan/minska programmet körs i pipeline i den här genomgången på Azure HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="23c2c-145">The pipeline in this walkthrough runs the Word Count streaming Map/Reduce program on your Azure HDInsight cluster.</span></span> 

### <a name="linked-services"></a><span data-ttu-id="23c2c-146">Länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="23c2c-146">Linked services</span></span>
#### <a name="azure-storage-linked-service"></a><span data-ttu-id="23c2c-147">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="23c2c-147">Azure Storage linked service</span></span>
<span data-ttu-id="23c2c-148">Först skapar du en länkad tjänst om du vill länka Azure Storage som används av Azure HDInsight-kluster till Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="23c2c-148">First, you create a linked service to link the Azure Storage that is used by the Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="23c2c-149">Om du kopiera och klistra in följande kod, Glöm inte att ersätta kontonamn och kontonyckel med namn och nyckel för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="23c2c-149">If you copy/paste the following code, do not forget to replace account name and account key with the name and key of your Azure Storage.</span></span> 

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="23c2c-150">Azure HDInsight länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="23c2c-150">Azure HDInsight linked service</span></span>
<span data-ttu-id="23c2c-151">Därefter skapar du en länkad tjänst om du vill länka ditt Azure HDInsight-kluster till Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="23c2c-151">Next, you create a linked service to link your Azure HDInsight cluster to the Azure data factory.</span></span> <span data-ttu-id="23c2c-152">Om du kopiera och klistra in följande kod, Ersätt HDInsight-klustrets namn med namnet på ditt HDInsight-kluster och ändra värdena för användarens namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="23c2c-152">If you copy/paste the following code, replace HDInsight cluster name with the name of your HDInsight cluster, and change user name and password values.</span></span> 

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

### <a name="datasets"></a><span data-ttu-id="23c2c-153">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="23c2c-153">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="23c2c-154">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="23c2c-154">Output dataset</span></span>
<span data-ttu-id="23c2c-155">Pipeline i det här exemplet tar inte alla indata.</span><span class="sxs-lookup"><span data-stu-id="23c2c-155">The pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="23c2c-156">Du kan ange en datamängd för utdata för aktiviteten strömning HDInsight.</span><span class="sxs-lookup"><span data-stu-id="23c2c-156">You specify an output dataset for the HDInsight Streaming Activity.</span></span> <span data-ttu-id="23c2c-157">Den här datauppsättningen är bara en dummy datamängd som krävs för att driva pipeline-schemat.</span><span class="sxs-lookup"><span data-stu-id="23c2c-157">This dataset is just a dummy dataset that is required to drive the pipeline schedule.</span></span> 

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

### <a name="pipeline"></a><span data-ttu-id="23c2c-158">Pipeline</span><span class="sxs-lookup"><span data-stu-id="23c2c-158">Pipeline</span></span>
<span data-ttu-id="23c2c-159">Pipeline i det här exemplet har endast en aktivitet som är av typen: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="23c2c-159">The pipeline in this example has only one activity that is of type: **HDInsightStreaming**.</span></span> 

<span data-ttu-id="23c2c-160">HDInsight-klustret fylls automatiskt med exempel program (wc.exe och cat.exe) och (davinci.txt).</span><span class="sxs-lookup"><span data-stu-id="23c2c-160">The HDInsight cluster is automatically populated with example programs (wc.exe and cat.exe) and data (davinci.txt).</span></span> <span data-ttu-id="23c2c-161">Som standard är namnet på behållaren som används av HDInsight-klustret namn för själva klustret.</span><span class="sxs-lookup"><span data-stu-id="23c2c-161">By default, name of the container that is used by the HDInsight cluster is the name of the cluster itself.</span></span> <span data-ttu-id="23c2c-162">Om klusternamnet är myhdicluster, är namnet på blobbehållare som är kopplad till exempel myhdicluster.</span><span class="sxs-lookup"><span data-stu-id="23c2c-162">For example, if your cluster name is myhdicluster, name of the blob container associated would be myhdicluster.</span></span>  

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
## <a name="see-also"></a><span data-ttu-id="23c2c-163">Se även</span><span class="sxs-lookup"><span data-stu-id="23c2c-163">See Also</span></span>
* [<span data-ttu-id="23c2c-164">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-164">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="23c2c-165">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="23c2c-165">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="23c2c-166">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="23c2c-166">MapReduce Activity</span></span>](data-factory-map-reduce.md)
* [<span data-ttu-id="23c2c-167">Anropa Spark-program</span><span class="sxs-lookup"><span data-stu-id="23c2c-167">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="23c2c-168">Anropa R-skript</span><span class="sxs-lookup"><span data-stu-id="23c2c-168">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

