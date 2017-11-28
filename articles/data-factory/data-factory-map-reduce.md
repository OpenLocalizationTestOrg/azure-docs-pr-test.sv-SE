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
# <a name="invoke-mapreduce-programs-from-data-factory"></a><span data-ttu-id="7be9f-103">Anropa MapReduce program från Data Factory</span><span class="sxs-lookup"><span data-stu-id="7be9f-103">Invoke MapReduce Programs from Data Factory</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="7be9f-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="7be9f-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="7be9f-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="7be9f-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="7be9f-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="7be9f-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="7be9f-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="7be9f-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="7be9f-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="7be9f-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="7be9f-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="7be9f-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="7be9f-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="7be9f-114">Hej HDInsight MapReduce aktivitet i en Datafabrik [pipeline](data-factory-create-pipelines.md) MapReduce-program körs på [egna](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) eller [på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7be9f-114">hello HDInsight MapReduce activity in a Data Factory [pipeline](data-factory-create-pipelines.md) executes MapReduce programs on [your own](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux-based HDInsight cluster.</span></span> <span data-ttu-id="7be9f-115">Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7be9f-115">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities.</span></span>

> [!NOTE] 
> <span data-ttu-id="7be9f-116">Om du är ny tooAzure Data Factory, Läs igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) och hello Självstudier: [skapa din första pipeline data](data-factory-build-your-first-pipeline.md) innan du läser den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7be9f-116">If you are new tooAzure Data Factory, read through [Introduction tooAzure Data Factory](data-factory-introduction.md) and do hello tutorial: [Build your first data pipeline](data-factory-build-your-first-pipeline.md) before reading this article.</span></span>  

## <a name="introduction"></a><span data-ttu-id="7be9f-117">Introduktion</span><span class="sxs-lookup"><span data-stu-id="7be9f-117">Introduction</span></span>
<span data-ttu-id="7be9f-118">En pipeline i ett Azure data factory bearbetar data i länkade storage-tjänster med hjälp av länkade beräknings-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7be9f-118">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="7be9f-119">Den innehåller en serie aktiviteter där varje aktivitet utför en specifik bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7be9f-119">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="7be9f-120">Den här artikeln beskriver hur du använder hello HDInsight MapReduce Activity.</span><span class="sxs-lookup"><span data-stu-id="7be9f-120">This article describes using hello HDInsight MapReduce Activity.</span></span>

<span data-ttu-id="7be9f-121">Se [Pig](data-factory-pig-activity.md) och [Hive](data-factory-hive-activity.md) mer information om hur du kör Pig/Hive skript på en Windows/Linux-baserade HDInsight-kluster från en rörledning HDInsight Pig och Hive aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7be9f-121">See [Pig](data-factory-pig-activity.md) and [Hive](data-factory-hive-activity.md) for details about running Pig/Hive scripts on a Windows/Linux-based HDInsight cluster from a pipeline by using HDInsight Pig and Hive activities.</span></span> 

## <a name="json-for-hdinsight-mapreduce-activity"></a><span data-ttu-id="7be9f-122">JSON för HDInsight MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="7be9f-122">JSON for HDInsight MapReduce Activity</span></span>
<span data-ttu-id="7be9f-123">I hello JSON-definitionen för hello HDInsight aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="7be9f-123">In hello JSON definition for hello HDInsight Activity:</span></span> 

1. <span data-ttu-id="7be9f-124">Ange hello **typen** av hello **aktiviteten** för**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="7be9f-124">Set hello **type** of hello **activity** too**HDInsight**.</span></span>
2. <span data-ttu-id="7be9f-125">Ange hello namn hello-klass för **className** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7be9f-125">Specify hello name of hello class for **className** property.</span></span>
3. <span data-ttu-id="7be9f-126">Ange hello sökvägen toohello JAR-filen inklusive hello filnamnet för **jarFilePath** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7be9f-126">Specify hello path toohello JAR file including hello file name for **jarFilePath** property.</span></span>
4. <span data-ttu-id="7be9f-127">Ange hello länkade tjänst som refererar toohello Azure Blob Storage som innehåller hello JAR-filen för **jarLinkedService** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7be9f-127">Specify hello linked service that refers toohello Azure Blob Storage that contains hello JAR file for **jarLinkedService** property.</span></span>   
5. <span data-ttu-id="7be9f-128">Ange några argument för hello MapReduce program i hello **argument** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7be9f-128">Specify any arguments for hello MapReduce program in hello **arguments** section.</span></span> <span data-ttu-id="7be9f-129">Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket.</span><span class="sxs-lookup"><span data-stu-id="7be9f-129">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="7be9f-130">toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden).</span><span class="sxs-lookup"><span data-stu-id="7be9f-130">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values).</span></span>

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
<span data-ttu-id="7be9f-131">Du kan använda hello HDInsight MapReduce Activity toorun alla MapReduce jar-filen på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7be9f-131">You can use hello HDInsight MapReduce Activity toorun any MapReduce jar file on an HDInsight cluster.</span></span> <span data-ttu-id="7be9f-132">Följande exempel JSON-definitionen för en pipeline hello HDInsight aktiviteten är konfigurerade hello toorun Mahout JAR-filen.</span><span class="sxs-lookup"><span data-stu-id="7be9f-132">In hello following sample JSON definition of a pipeline, hello HDInsight Activity is configured toorun a Mahout JAR file.</span></span>

## <a name="sample-on-github"></a><span data-ttu-id="7be9f-133">Exempel på GitHub</span><span class="sxs-lookup"><span data-stu-id="7be9f-133">Sample on GitHub</span></span>
<span data-ttu-id="7be9f-134">Du kan hämta ett exempel för att använda hello HDInsight MapReduce Activity från: [Data Factory exemplen på GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span><span class="sxs-lookup"><span data-stu-id="7be9f-134">You can download a sample for using hello HDInsight MapReduce Activity from: [Data Factory Samples on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).</span></span>  

## <a name="running-hello-word-count-program"></a><span data-ttu-id="7be9f-135">Kör hello ordräkning program</span><span class="sxs-lookup"><span data-stu-id="7be9f-135">Running hello Word Count program</span></span>
<span data-ttu-id="7be9f-136">hello pipeline i det här exemplet körs hello ordräkning kartan/minska program på Azure HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="7be9f-136">hello pipeline in this example runs hello Word Count Map/Reduce program on your Azure HDInsight cluster.</span></span>   

### <a name="linked-services"></a><span data-ttu-id="7be9f-137">Länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="7be9f-137">Linked Services</span></span>
<span data-ttu-id="7be9f-138">Först skapar du en länkad tjänst toolink hello Azure Storage som används av hello Azure HDInsight-kluster toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7be9f-138">First, you create a linked service toolink hello Azure Storage that is used by hello Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="7be9f-139">Om du kopiera och klistra in följande kod hello, Glöm inte tooreplace **kontonamn** och **kontonyckel** med hello namn och nyckel för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7be9f-139">If you copy/paste hello following code, do not forget tooreplace **account name** and **account key** with hello name and key of your Azure Storage.</span></span> 

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="7be9f-140">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="7be9f-140">Azure Storage linked service</span></span>

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

#### <a name="azure-hdinsight-linked-service"></a><span data-ttu-id="7be9f-141">Azure HDInsight länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="7be9f-141">Azure HDInsight linked service</span></span>
<span data-ttu-id="7be9f-142">Därefter måste skapa du en länkad tjänst toolink din Azure HDInsight-kluster toohello Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7be9f-142">Next, you create a linked service toolink your Azure HDInsight cluster toohello Azure data factory.</span></span> <span data-ttu-id="7be9f-143">Om du kopiera och klistra in följande kod hello, ersätter **HDInsight-klustrets namn** med hello namnet på ditt HDInsight-kluster och ändra användarens namn och lösenord värden.</span><span class="sxs-lookup"><span data-stu-id="7be9f-143">If you copy/paste hello following code, replace **HDInsight cluster name** with hello name of your HDInsight cluster, and change user name and password values.</span></span>   

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

### <a name="datasets"></a><span data-ttu-id="7be9f-144">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="7be9f-144">Datasets</span></span>
#### <a name="output-dataset"></a><span data-ttu-id="7be9f-145">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="7be9f-145">Output dataset</span></span>
<span data-ttu-id="7be9f-146">hello pipeline i det här exemplet tar inte alla indata.</span><span class="sxs-lookup"><span data-stu-id="7be9f-146">hello pipeline in this example does not take any inputs.</span></span> <span data-ttu-id="7be9f-147">Du kan ange en datamängd för utdata för hello HDInsight MapReduce Activity.</span><span class="sxs-lookup"><span data-stu-id="7be9f-147">You specify an output dataset for hello HDInsight MapReduce Activity.</span></span> <span data-ttu-id="7be9f-148">Den här datauppsättningen är bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema.</span><span class="sxs-lookup"><span data-stu-id="7be9f-148">This dataset is just a dummy dataset that is required toodrive hello pipeline schedule.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="7be9f-149">Pipeline</span><span class="sxs-lookup"><span data-stu-id="7be9f-149">Pipeline</span></span>
<span data-ttu-id="7be9f-150">hello pipeline i det här exemplet har endast en aktivitet som är av typen: HDInsightMapReduce.</span><span class="sxs-lookup"><span data-stu-id="7be9f-150">hello pipeline in this example has only one activity that is of type: HDInsightMapReduce.</span></span> <span data-ttu-id="7be9f-151">Några av hello viktiga egenskaper i hello JSON är:</span><span class="sxs-lookup"><span data-stu-id="7be9f-151">Some of hello important properties in hello JSON are:</span></span> 

| <span data-ttu-id="7be9f-152">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7be9f-152">Property</span></span> | <span data-ttu-id="7be9f-153">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="7be9f-153">Notes</span></span> |
|:--- |:--- |
| <span data-ttu-id="7be9f-154">typ</span><span class="sxs-lookup"><span data-stu-id="7be9f-154">type</span></span> |<span data-ttu-id="7be9f-155">hello måste anges för**HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="7be9f-155">hello type must be set too**HDInsightMapReduce**.</span></span> |
| <span data-ttu-id="7be9f-156">Klassnamn</span><span class="sxs-lookup"><span data-stu-id="7be9f-156">className</span></span> |<span data-ttu-id="7be9f-157">Namnet på klassen hello är: **wordcount**</span><span class="sxs-lookup"><span data-stu-id="7be9f-157">Name of hello class is: **wordcount**</span></span> |
| <span data-ttu-id="7be9f-158">jarFilePath</span><span class="sxs-lookup"><span data-stu-id="7be9f-158">jarFilePath</span></span> |<span data-ttu-id="7be9f-159">Sökvägen toohello jar-filen som innehåller hello-klass.</span><span class="sxs-lookup"><span data-stu-id="7be9f-159">Path toohello jar file containing hello class.</span></span> <span data-ttu-id="7be9f-160">Om du kopiera och klistra in följande kod hello, Glöm inte toochange hello klustrets hello namn.</span><span class="sxs-lookup"><span data-stu-id="7be9f-160">If you copy/paste hello following code, don't forget toochange hello name of hello cluster.</span></span> |
| <span data-ttu-id="7be9f-161">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="7be9f-161">jarLinkedService</span></span> |<span data-ttu-id="7be9f-162">Azure Storage länkade tjänst som innehåller hello jar-filen.</span><span class="sxs-lookup"><span data-stu-id="7be9f-162">Azure Storage linked service that contains hello jar file.</span></span> <span data-ttu-id="7be9f-163">Den här länkade tjänsten refererar toohello lagring som är associerad med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7be9f-163">This linked service refers toohello storage that is associated with hello HDInsight cluster.</span></span> |
| <span data-ttu-id="7be9f-164">Argument</span><span class="sxs-lookup"><span data-stu-id="7be9f-164">arguments</span></span> |<span data-ttu-id="7be9f-165">Hej hämtas wordcount två argument, indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="7be9f-165">hello wordcount program takes two arguments, an input and an output.</span></span> <span data-ttu-id="7be9f-166">hello indatafilen är hello davinci.txt fil.</span><span class="sxs-lookup"><span data-stu-id="7be9f-166">hello input file is hello davinci.txt file.</span></span> |
| <span data-ttu-id="7be9f-167">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="7be9f-167">frequency/interval</span></span> |<span data-ttu-id="7be9f-168">hello värdena för dessa egenskaper matchar hello datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="7be9f-168">hello values for these properties match hello output dataset.</span></span> |
| <span data-ttu-id="7be9f-169">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="7be9f-169">linkedServiceName</span></span> |<span data-ttu-id="7be9f-170">refererar toohello HDInsight länkade-tjänst som du har skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="7be9f-170">refers toohello HDInsight linked service you had created earlier.</span></span> |

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

## <a name="run-spark-programs"></a><span data-ttu-id="7be9f-171">Köra Spark-program</span><span class="sxs-lookup"><span data-stu-id="7be9f-171">Run Spark programs</span></span>
<span data-ttu-id="7be9f-172">Du kan använda MapReduce aktiviteten toorun Spark-program på HDInsight Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7be9f-172">You can use MapReduce activity toorun Spark programs on your HDInsight Spark cluster.</span></span> <span data-ttu-id="7be9f-173">Mer information finns i [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) (Anropa Spark-program från Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="7be9f-173">See [Invoke Spark programs from Azure Data Factory](data-factory-spark.md) for details.</span></span>  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com

## <a name="see-also"></a><span data-ttu-id="7be9f-174">Se även</span><span class="sxs-lookup"><span data-stu-id="7be9f-174">See Also</span></span>
* [<span data-ttu-id="7be9f-175">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-175">Hive Activity</span></span>](data-factory-hive-activity.md)
* [<span data-ttu-id="7be9f-176">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="7be9f-176">Pig Activity</span></span>](data-factory-pig-activity.md)
* [<span data-ttu-id="7be9f-177">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="7be9f-177">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
* [<span data-ttu-id="7be9f-178">Anropa Spark-program</span><span class="sxs-lookup"><span data-stu-id="7be9f-178">Invoke Spark programs</span></span>](data-factory-spark.md)
* [<span data-ttu-id="7be9f-179">Anropa R-skript</span><span class="sxs-lookup"><span data-stu-id="7be9f-179">Invoke R scripts</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

