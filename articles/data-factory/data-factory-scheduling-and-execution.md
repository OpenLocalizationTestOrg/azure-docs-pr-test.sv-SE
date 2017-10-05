---
title: "Schemaläggning och körning med Data Factory | Microsoft Docs"
description: "Lär dig schemaläggning och körning av aspekter av Azure Data Factory programmodell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: e6fd92cde91ae5f171c855c07fa8974a19703b41
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="data-factory-scheduling-and-execution"></a><span data-ttu-id="48019-103">Data Factory schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="48019-103">Data Factory scheduling and execution</span></span>
<span data-ttu-id="48019-104">I den här artikeln beskrivs aspekter för schemaläggning och körning av Azure Data Factory-programmodellen.</span><span class="sxs-lookup"><span data-stu-id="48019-104">This article explains the scheduling and execution aspects of the Azure Data Factory application model.</span></span> <span data-ttu-id="48019-105">Den här artikeln förutsätter att du förstår grunderna för Data Factory programmet modellen begrepp, inklusive aktivitet, rörledningar, länkade tjänster och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="48019-105">This article assumes that you understand basics of Data Factory application model concepts, including activity, pipelines, linked services, and datasets.</span></span> <span data-ttu-id="48019-106">Grundläggande begrepp för Azure Data Factory finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="48019-106">For basic concepts of Azure Data Factory, see the following articles:</span></span>

* [<span data-ttu-id="48019-107">Introduktion till Data Factory</span><span class="sxs-lookup"><span data-stu-id="48019-107">Introduction to Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="48019-108">Pipelines</span><span class="sxs-lookup"><span data-stu-id="48019-108">Pipelines</span></span>](data-factory-create-pipelines.md)
* [<span data-ttu-id="48019-109">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="48019-109">Datasets</span></span>](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a><span data-ttu-id="48019-110">Start- och sluttider för pipeline</span><span class="sxs-lookup"><span data-stu-id="48019-110">Start and end times of pipeline</span></span>
<span data-ttu-id="48019-111">En pipeline är aktiv endast mellan dess **starta** tid och **end** tid.</span><span class="sxs-lookup"><span data-stu-id="48019-111">A pipeline is active only between its **start** time and **end** time.</span></span> <span data-ttu-id="48019-112">Det utförs inte före starttiden eller efter sluttid.</span><span class="sxs-lookup"><span data-stu-id="48019-112">It is not executed before the start time or after the end time.</span></span> <span data-ttu-id="48019-113">Om pipeline är pausad körs den inte oavsett dess start- och tid.</span><span class="sxs-lookup"><span data-stu-id="48019-113">If the pipeline is paused, it is not executed irrespective of its start and end time.</span></span> <span data-ttu-id="48019-114">För en rörledning för att köra, bör det inte pausas.</span><span class="sxs-lookup"><span data-stu-id="48019-114">For a pipeline to run, it should not be paused.</span></span> <span data-ttu-id="48019-115">Du hittar dessa inställningar (start, slut, pausats) i pipeline-definition:</span><span class="sxs-lookup"><span data-stu-id="48019-115">You find these settings (start, end, paused) in the pipeline definition:</span></span> 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

<span data-ttu-id="48019-116">Mer information finns i de här egenskaperna [skapa pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="48019-116">For more information these properties, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> 


## <a name="specify-schedule-for-an-activity"></a><span data-ttu-id="48019-117">Ange schemat för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="48019-117">Specify schedule for an activity</span></span>
<span data-ttu-id="48019-118">Det är inte den pipeline som körs.</span><span class="sxs-lookup"><span data-stu-id="48019-118">It is not the pipeline that is executed.</span></span> <span data-ttu-id="48019-119">Det är aktiviteterna i pipeline som körs i kontexten övergripande av pipeline.</span><span class="sxs-lookup"><span data-stu-id="48019-119">It is the activities in the pipeline that are executed in the overall context of the pipeline.</span></span> <span data-ttu-id="48019-120">Du kan ange ett återkommande schema för en aktivitet med hjälp av den **scheduler** delen av aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="48019-120">You can specify a recurring schedule for an activity by using the **scheduler** section of activity JSON.</span></span> <span data-ttu-id="48019-121">Exempelvis kan du schemalägga en aktivitet för att köra varje timme på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="48019-121">For example, you can schedule an activity to run hourly as follows:</span></span>  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

<span data-ttu-id="48019-122">I följande diagram visas att ange ett schema för en aktivitet skapas en serie rullande fönster med i pipeline-start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="48019-122">As shown in the following diagram, specifying a schedule for an activity creates a series of tumbling windows with in the pipeline start and end times.</span></span> <span data-ttu-id="48019-123">Rullande fönster är en serie med fast storlek inte överlappar, sammanhängande intervall.</span><span class="sxs-lookup"><span data-stu-id="48019-123">Tumbling windows are a series of fixed-size non-overlapping, contiguous time intervals.</span></span> <span data-ttu-id="48019-124">Dessa logiska rullande fönster för en aktivitet kallas **aktivitet windows**.</span><span class="sxs-lookup"><span data-stu-id="48019-124">These logical tumbling windows for an activity are called **activity windows**.</span></span>

![Aktiviteten scheduler-exempel](media/data-factory-scheduling-and-execution/scheduler-example.png)

<span data-ttu-id="48019-126">Den **scheduler** -egenskapen för en aktivitet är valfria.</span><span class="sxs-lookup"><span data-stu-id="48019-126">The **scheduler** property for an activity is optional.</span></span> <span data-ttu-id="48019-127">Om du anger den här egenskapen, måste den matcha det intervall som du anger i definitionen av datamängd för utdata för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-127">If you do specify this property, it must match the cadence you specify in the definition of output dataset for the activity.</span></span> <span data-ttu-id="48019-128">Schemat styrs för närvarande av utdatamängd.</span><span class="sxs-lookup"><span data-stu-id="48019-128">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="48019-129">Därför måste du skapa en datamängd för utdata, även om aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="48019-129">Therefore, you must create an output dataset even if the activity does not produce any output.</span></span> 

## <a name="specify-schedule-for-a-dataset"></a><span data-ttu-id="48019-130">Ange schemat för en datamängd</span><span class="sxs-lookup"><span data-stu-id="48019-130">Specify schedule for a dataset</span></span>
<span data-ttu-id="48019-131">En aktivitet i en Data Factory-pipelinen har noll eller fler indata **datauppsättningar** och skapa en eller flera utdata-datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="48019-131">An activity in a Data Factory pipeline can take zero or more input **datasets** and produce one or more output datasets.</span></span> <span data-ttu-id="48019-132">För en aktivitet kan du ange det intervall då finns indata eller utdata skapas med hjälp av den **tillgänglighet** avsnitt i dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="48019-132">For an activity, you can specify the cadence at which the input data is available or the output data is produced by using the **availability** section in the dataset definitions.</span></span> 

<span data-ttu-id="48019-133">**Frekvensen** i den **tillgänglighet** avsnittet anger tidsenheten.</span><span class="sxs-lookup"><span data-stu-id="48019-133">**Frequency** in the **availability** section specifies the time unit.</span></span> <span data-ttu-id="48019-134">Tillåtna värden för frekvens är: minut, timma, dag, vecka och månad.</span><span class="sxs-lookup"><span data-stu-id="48019-134">The allowed values for frequency are: Minute, Hour, Day, Week, and Month.</span></span> <span data-ttu-id="48019-135">Den **intervall** egenskap i avsnittet tillgänglighet anger en multiplikator för frekvens.</span><span class="sxs-lookup"><span data-stu-id="48019-135">The **interval** property in the availability section specifies a multiplier for frequency.</span></span> <span data-ttu-id="48019-136">Exempel: om frekvensen är inställd på dagen och intervallet anges till 1 för en datamängd för utdata, utdata skapas varje dag.</span><span class="sxs-lookup"><span data-stu-id="48019-136">For example: if the frequency is set to Day and interval is set to 1 for an output dataset, the output data is produced daily.</span></span> <span data-ttu-id="48019-137">Om du anger frekvensen som minut, rekommenderar vi att du anger intervallet till mindre än 15.</span><span class="sxs-lookup"><span data-stu-id="48019-137">If you specify the frequency as minute, we recommend that you set the interval to no less than 15.</span></span> 

<span data-ttu-id="48019-138">I följande exempel indata finns varje timme och utdata skapas varje timme (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="48019-138">In the following example, the input data is available hourly and the output data is produced hourly (`"frequency": "Hour", "interval": 1`).</span></span> 

<span data-ttu-id="48019-139">**Indatauppsättning:**</span><span class="sxs-lookup"><span data-stu-id="48019-139">**Input dataset:**</span></span> 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


<span data-ttu-id="48019-140">**Datamängd för utdata**</span><span class="sxs-lookup"><span data-stu-id="48019-140">**Output dataset**</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="48019-141">För närvarande **utdatauppsättningen enheter schemat**.</span><span class="sxs-lookup"><span data-stu-id="48019-141">Currently, **output dataset drives the schedule**.</span></span> <span data-ttu-id="48019-142">Det schema som angetts för datamängd för utdata är med andra ord används för att köra en aktivitet vid körning.</span><span class="sxs-lookup"><span data-stu-id="48019-142">In other words, the schedule specified for the output dataset is used to run an activity at runtime.</span></span> <span data-ttu-id="48019-143">Därför måste du skapa en datamängd för utdata, även om aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="48019-143">Therefore, you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="48019-144">Om aktiviteten inte får några indata, kan du hoppa över att skapa indatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="48019-144">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> 

<span data-ttu-id="48019-145">I följande pipeline-definition i **scheduler** egenskapen används för att ange schemat för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-145">In the following pipeline definition, the **scheduler** property is used to specify schedule for the activity.</span></span> <span data-ttu-id="48019-146">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="48019-146">This property is optional.</span></span> <span data-ttu-id="48019-147">För närvarande måste schemat för aktiviteten matcha det schema som angetts för datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="48019-147">Currently, the schedule for the activity must match the schedule specified for the output dataset.</span></span>
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

<span data-ttu-id="48019-148">I det här exemplet körs aktiviteten varje timme mellan start- och sluttider för pipeline.</span><span class="sxs-lookup"><span data-stu-id="48019-148">In this example, the activity runs hourly between the start and end times of the pipeline.</span></span> <span data-ttu-id="48019-149">Utdata skapas varje timme för tre timmars windows (8: 00 – 9 AM, 9: 00 – 10: 00, och 10 AM - 11: 00).</span><span class="sxs-lookup"><span data-stu-id="48019-149">The output data is produced hourly for three-hour windows (8 AM - 9 AM, 9 AM - 10 AM, and 10 AM - 11 AM).</span></span> 

<span data-ttu-id="48019-150">Varje dataenhet förbrukas eller produceras av en aktivitet som körs kallas en **datasektorn**.</span><span class="sxs-lookup"><span data-stu-id="48019-150">Each unit of data consumed or produced by an activity run is called a **data slice**.</span></span> <span data-ttu-id="48019-151">Följande diagram visar ett exempel på en aktivitet med en inkommande datauppsättning och en utdatauppsättning:</span><span class="sxs-lookup"><span data-stu-id="48019-151">The following diagram shows an example of an activity with one input dataset and one output dataset:</span></span> 

![Tillgänglighet Schemaläggaren](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

<span data-ttu-id="48019-153">Diagrammet visar timvis datasektorer för inkommande och utgående dataset.</span><span class="sxs-lookup"><span data-stu-id="48019-153">The diagram shows the hourly data slices for the input and output dataset.</span></span> <span data-ttu-id="48019-154">Diagrammet visar tre inkommande segment som är klara för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="48019-154">The diagram shows three input slices that are ready for processing.</span></span> <span data-ttu-id="48019-155">Aktiviteten 10 11 AM pågår, producerar 10 11 AM utdata sektorn.</span><span class="sxs-lookup"><span data-stu-id="48019-155">The 10-11 AM activity is in progress, producing the 10-11 AM output slice.</span></span> 

<span data-ttu-id="48019-156">Du kan komma åt det tidsintervall som är associerade med det aktuella segmentet i datauppsättningen JSON med hjälp av variabler: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) och [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="48019-156">You can access the time interval associated with the current slice in the dataset JSON by using variables: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) and [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="48019-157">På liknande sätt kan du komma åt det tidsintervall som är associerad med ett fönster med en aktivitet med hjälp av WindowStart och WindowEnd.</span><span class="sxs-lookup"><span data-stu-id="48019-157">Similarly, you can access the time interval associated with an activity window by using the WindowStart and WindowEnd.</span></span> <span data-ttu-id="48019-158">Schemat för en aktivitet måste stämma överens med schemat för datamängd för utdata för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-158">The schedule of an activity must match the schedule of the output dataset for the activity.</span></span> <span data-ttu-id="48019-159">Därför är värdena SliceStart och SliceEnd samma som WindowStart och WindowEnd värden respektive.</span><span class="sxs-lookup"><span data-stu-id="48019-159">Therefore, the SliceStart and SliceEnd values are the same as WindowStart and WindowEnd values respectively.</span></span> <span data-ttu-id="48019-160">Mer information om dessa variabler finns [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md#data-factory-system-variables) artiklar.</span><span class="sxs-lookup"><span data-stu-id="48019-160">For more information on these variables, see [Data Factory functions and system variables](data-factory-functions-variables.md#data-factory-system-variables) articles.</span></span>  

<span data-ttu-id="48019-161">Du kan använda dessa variabler för olika ändamål i din aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="48019-161">You can use these variables for different purposes in your activity JSON.</span></span> <span data-ttu-id="48019-162">Du kan till exempel använda dem för att hämta data från inkommande och utgående datauppsättningar som representerar tid series-data (till exempel: 8: 00 till 9: 00).</span><span class="sxs-lookup"><span data-stu-id="48019-162">For example, you can use them to select data from input and output datasets representing time series data (for example: 8 AM to 9 AM).</span></span> <span data-ttu-id="48019-163">Det här exemplet använder också **WindowStart** och **WindowEnd** för att välja relevanta data för en aktivitet körs och kopierar den till en blob med lämplig **folderPath**.</span><span class="sxs-lookup"><span data-stu-id="48019-163">This example also uses **WindowStart** and **WindowEnd** to select relevant data for an activity run and copy it to a blob with the appropriate **folderPath**.</span></span> <span data-ttu-id="48019-164">Den **folderPath** parametriserade om du vill ha en separat mapp för varje timme.</span><span class="sxs-lookup"><span data-stu-id="48019-164">The **folderPath** is parameterized to have a separate folder for every hour.</span></span>  

<span data-ttu-id="48019-165">I föregående exempel schemat som angetts för indata och utdata-datauppsättningar är den samma (varje timme).</span><span class="sxs-lookup"><span data-stu-id="48019-165">In the preceding example, the schedule specified for input and output datasets is the same (hourly).</span></span> <span data-ttu-id="48019-166">Om den inkommande datamängden för aktiviteten är tillgängligt på en annan frekvens Säg var 15: e minut körs aktiviteten som producerar datauppsättningen utdata fortfarande en gång i timmen som datamängd för utdata är styr aktiviteten schemat.</span><span class="sxs-lookup"><span data-stu-id="48019-166">If the input dataset for the activity is available at a different frequency, say every 15 minutes, the activity that produces this output dataset still runs once an hour as the output dataset is what drives the activity schedule.</span></span> <span data-ttu-id="48019-167">Mer information finns i [modell datauppsättningar med olika frekvenser](#model-datasets-with-different-frequencies).</span><span class="sxs-lookup"><span data-stu-id="48019-167">For more information, see [Model datasets with different frequencies](#model-datasets-with-different-frequencies).</span></span>

## <a name="dataset-availability-and-policies"></a><span data-ttu-id="48019-168">DataSet tillgänglighet och principer</span><span class="sxs-lookup"><span data-stu-id="48019-168">Dataset availability and policies</span></span>
<span data-ttu-id="48019-169">Du har sett användning av frekvensen och intervall egenskaper i avsnittet tillgänglighet i datauppsättningsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="48019-169">You have seen the usage of frequency and interval properties in the availability section of dataset definition.</span></span> <span data-ttu-id="48019-170">Det finns några andra egenskaper som påverkar schemaläggningen och körning av en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48019-170">There are a few other properties that affect the scheduling and execution of an activity.</span></span> 

### <a name="dataset-availability"></a><span data-ttu-id="48019-171">DataSet-tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="48019-171">Dataset availability</span></span> 
<span data-ttu-id="48019-172">Följande tabell beskriver egenskaper som kan användas i den **tillgänglighet** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="48019-172">The following table describes properties you can use in the **availability** section:</span></span>

| <span data-ttu-id="48019-173">Egenskap</span><span class="sxs-lookup"><span data-stu-id="48019-173">Property</span></span> | <span data-ttu-id="48019-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="48019-174">Description</span></span> | <span data-ttu-id="48019-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="48019-175">Required</span></span> | <span data-ttu-id="48019-176">Standard</span><span class="sxs-lookup"><span data-stu-id="48019-176">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="48019-177">frekvens</span><span class="sxs-lookup"><span data-stu-id="48019-177">frequency</span></span> |<span data-ttu-id="48019-178">Anger tidsenheten för dataset sektorn produktion.</span><span class="sxs-lookup"><span data-stu-id="48019-178">Specifies the time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="48019-179"><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad</span><span class="sxs-lookup"><span data-stu-id="48019-179"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="48019-180">Ja</span><span class="sxs-lookup"><span data-stu-id="48019-180">Yes</span></span> |<span data-ttu-id="48019-181">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="48019-181">NA</span></span> |
| <span data-ttu-id="48019-182">intervall</span><span class="sxs-lookup"><span data-stu-id="48019-182">interval</span></span> |<span data-ttu-id="48019-183">Anger en multiplikator för frekvens</span><span class="sxs-lookup"><span data-stu-id="48019-183">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="48019-184">”X frekvensintervall” avgör hur ofta sektorn skapas.</span><span class="sxs-lookup"><span data-stu-id="48019-184">”Frequency x interval” determines how often the slice is produced.</span></span><br/><br/><span data-ttu-id="48019-185">Om du behöver datamängden som segmenterat timme kan du ange <b>frekvens</b> till <b>timme</b>, och <b>intervall</b> till <b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="48019-185">If you need the dataset to be sliced on an hourly basis, you set <b>Frequency</b> to <b>Hour</b>, and <b>interval</b> to <b>1</b>.</span></span><br/><br/><span data-ttu-id="48019-186"><b>Obs</b>: Om du anger frekvens som minut, rekommenderar vi att du anger intervallet till mindre än 15</span><span class="sxs-lookup"><span data-stu-id="48019-186"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set the interval to no less than 15</span></span> |<span data-ttu-id="48019-187">Ja</span><span class="sxs-lookup"><span data-stu-id="48019-187">Yes</span></span> |<span data-ttu-id="48019-188">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="48019-188">NA</span></span> |
| <span data-ttu-id="48019-189">format</span><span class="sxs-lookup"><span data-stu-id="48019-189">style</span></span> |<span data-ttu-id="48019-190">Anger om sektorn ska produceras start/slutet av intervallet.</span><span class="sxs-lookup"><span data-stu-id="48019-190">Specifies whether the slice should be produced at the start/end of the interval.</span></span><ul><li><span data-ttu-id="48019-191">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="48019-191">StartOfInterval</span></span></li><li><span data-ttu-id="48019-192">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="48019-192">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="48019-193">Om frekvensen är inställd på månad och stil anges till EndOfInterval producerade sektorn på den sista dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="48019-193">If Frequency is set to Month and style is set to EndOfInterval, the slice is produced on the last day of month.</span></span> <span data-ttu-id="48019-194">Om formatet som har angetts till StartOfInterval producerade sektorn på den första dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="48019-194">If the style is set to StartOfInterval, the slice is produced on the first day of month.</span></span><br/><br/><span data-ttu-id="48019-195">Om frekvensen är inställd på dagen och stil anges till EndOfInterval producerade sektorn under den senaste timmen på dagen.</span><span class="sxs-lookup"><span data-stu-id="48019-195">If Frequency is set to Day and style is set to EndOfInterval, the slice is produced in the last hour of the day.</span></span><br/><br/><span data-ttu-id="48019-196">Om frekvensen är inställd på timme och stil anges till EndOfInterval producerade sektorn i slutet av en timme.</span><span class="sxs-lookup"><span data-stu-id="48019-196">If Frequency is set to Hour and style is set to EndOfInterval, the slice is produced at the end of the hour.</span></span> <span data-ttu-id="48019-197">För ett segment för PM 1 – 2 PM period, till exempel produceras sektorn klockan 2.</span><span class="sxs-lookup"><span data-stu-id="48019-197">For example, for a slice for 1 PM – 2 PM period, the slice is produced at 2 PM.</span></span> |<span data-ttu-id="48019-198">Nej</span><span class="sxs-lookup"><span data-stu-id="48019-198">No</span></span> |<span data-ttu-id="48019-199">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="48019-199">EndOfInterval</span></span> |
| <span data-ttu-id="48019-200">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="48019-200">anchorDateTime</span></span> |<span data-ttu-id="48019-201">Definierar absolut placering i tid som används av Schemaläggaren för att beräkna dataset sektorn gränser.</span><span class="sxs-lookup"><span data-stu-id="48019-201">Defines the absolute position in time used by scheduler to compute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="48019-202"><b>Obs</b>: om AnchorDateTime har datumdelar som är mer detaljerad än frekvensen sedan mer detaljerade delar ignoreras.</span><span class="sxs-lookup"><span data-stu-id="48019-202"><b>Note</b>: If the AnchorDateTime has date parts that are more granular than the frequency then the more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="48019-203">Till exempel om den <b>intervall</b> är <b>varje timme</b> (frekvens: timme och intervall: 1) och <b>AnchorDateTime</b> innehåller <b>minuter och sekunder</b>, sedan <b>minuter och sekunder</b> delar av AnchorDateTime ignoreras.</span><span class="sxs-lookup"><span data-stu-id="48019-203">For example, if the <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and the <b>AnchorDateTime</b> contains <b>minutes and seconds</b>, then the <b>minutes and seconds</b> parts of the AnchorDateTime are ignored.</span></span> |<span data-ttu-id="48019-204">Nej</span><span class="sxs-lookup"><span data-stu-id="48019-204">No</span></span> |<span data-ttu-id="48019-205">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="48019-205">01/01/0001</span></span> |
| <span data-ttu-id="48019-206">förskjutning</span><span class="sxs-lookup"><span data-stu-id="48019-206">offset</span></span> |<span data-ttu-id="48019-207">TimeSpan som start- och slutdatum för alla dataset segment flyttat.</span><span class="sxs-lookup"><span data-stu-id="48019-207">Timespan by which the start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="48019-208"><b>Obs</b>: om både anchorDateTime och offset anges, resultatet är kombinerade SKIFT.</span><span class="sxs-lookup"><span data-stu-id="48019-208"><b>Note</b>: If both anchorDateTime and offset are specified, the result is the combined shift.</span></span> |<span data-ttu-id="48019-209">Nej</span><span class="sxs-lookup"><span data-stu-id="48019-209">No</span></span> |<span data-ttu-id="48019-210">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="48019-210">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="48019-211">Offset exempel</span><span class="sxs-lookup"><span data-stu-id="48019-211">offset example</span></span>
<span data-ttu-id="48019-212">Som standard varje dag (`"frequency": "Day", "interval": 1`) segment som börjar vid 12: 00 UTC-tid (midnatt).</span><span class="sxs-lookup"><span data-stu-id="48019-212">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM UTC time (midnight).</span></span> <span data-ttu-id="48019-213">Om du vill att starttiden ska 06: 00 UTC-tid i stället ange förskjutningen som visas i följande fragment:</span><span class="sxs-lookup"><span data-stu-id="48019-213">If you want the start time to be 6 AM UTC time instead, set the offset as shown in the following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="48019-214">anchorDateTime-exempel</span><span class="sxs-lookup"><span data-stu-id="48019-214">anchorDateTime example</span></span>
<span data-ttu-id="48019-215">I följande exempel skapas datauppsättningen var 23: e timme.</span><span class="sxs-lookup"><span data-stu-id="48019-215">In the following example, the dataset is produced once every 23 hours.</span></span> <span data-ttu-id="48019-216">Det första segmentet börjar vid den tid som anges av anchorDateTime som har angetts till `2017-04-19T08:00:00` (UTC-tid).</span><span class="sxs-lookup"><span data-stu-id="48019-216">The first slice starts at the time specified by the anchorDateTime, which is set to `2017-04-19T08:00:00` (UTC time).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="48019-217">förskjutning/stil exempel</span><span class="sxs-lookup"><span data-stu-id="48019-217">offset/style Example</span></span>
<span data-ttu-id="48019-218">Följande datauppsättningen månatliga datauppsättning och skapas på 3: e varje månad kl 8:00 (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="48019-218">The following dataset is a monthly dataset and is produced on 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a><span data-ttu-id="48019-219">DataSet-princip</span><span class="sxs-lookup"><span data-stu-id="48019-219">Dataset policy</span></span>
<span data-ttu-id="48019-220">En dataset kan ha en verifieringsprincip har definierats som anger hur data som genereras av en sektor körning kan valideras innan den är klar för användning.</span><span class="sxs-lookup"><span data-stu-id="48019-220">A dataset can have a validation policy defined that specifies how the data generated by a slice execution can be validated before it is ready for consumption.</span></span> <span data-ttu-id="48019-221">I sådana fall när sektorn är klar körning och utdata sektorn statusen ändras till **väntar på** med en substatus av **validering**.</span><span class="sxs-lookup"><span data-stu-id="48019-221">In such cases, after the slice has finished execution, the output slice status is changed to **Waiting** with a substatus of **Validation**.</span></span> <span data-ttu-id="48019-222">När segmenten verifieras sektorn status ändras till **klar**.</span><span class="sxs-lookup"><span data-stu-id="48019-222">After the slices are validated, the slice status changes to **Ready**.</span></span> <span data-ttu-id="48019-223">Om en datasektorn har producerats men klarade inte valideringen, bearbetas inte aktiviteten körs för underordnade segment som är beroende av det här segmentet.</span><span class="sxs-lookup"><span data-stu-id="48019-223">If a data slice has been produced but did not pass the validation, activity runs for downstream slices that depend on this slice are not processed.</span></span> <span data-ttu-id="48019-224">[Övervaka och hantera pipelines](data-factory-monitor-manage-pipelines.md) beskriver de olika lägena för datasektorer i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="48019-224">[Monitor and manage pipelines](data-factory-monitor-manage-pipelines.md) covers the various states of data slices in Data Factory.</span></span>

<span data-ttu-id="48019-225">Den **princip** avsnitt i datauppsättningsdefinitionen definierar villkoren eller det villkor som dataset-segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="48019-225">The **policy** section in dataset definition defines the criteria or the condition that the dataset slices must fulfill.</span></span> <span data-ttu-id="48019-226">Följande tabell beskriver egenskaper som kan användas i den **princip** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="48019-226">The following table describes properties you can use in the **policy** section:</span></span>

| <span data-ttu-id="48019-227">Principnamn</span><span class="sxs-lookup"><span data-stu-id="48019-227">Policy Name</span></span> | <span data-ttu-id="48019-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="48019-228">Description</span></span> | <span data-ttu-id="48019-229">Tillämpas på</span><span class="sxs-lookup"><span data-stu-id="48019-229">Applied To</span></span> | <span data-ttu-id="48019-230">Krävs</span><span class="sxs-lookup"><span data-stu-id="48019-230">Required</span></span> | <span data-ttu-id="48019-231">Standard</span><span class="sxs-lookup"><span data-stu-id="48019-231">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="48019-232">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="48019-232">minimumSizeMB</span></span> | <span data-ttu-id="48019-233">Validerar att data i en **Azure blob** uppfyller minsta storlek (i megabyte).</span><span class="sxs-lookup"><span data-stu-id="48019-233">Validates that the data in an **Azure blob** meets the minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="48019-234">Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="48019-234">Azure Blob</span></span> |<span data-ttu-id="48019-235">Nej</span><span class="sxs-lookup"><span data-stu-id="48019-235">No</span></span> |<span data-ttu-id="48019-236">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="48019-236">NA</span></span> |
| <span data-ttu-id="48019-237">minimumRows</span><span class="sxs-lookup"><span data-stu-id="48019-237">minimumRows</span></span> | <span data-ttu-id="48019-238">Validerar att data i en **Azure SQL database** eller en **Azure-tabellen** innehåller det minsta antalet rader.</span><span class="sxs-lookup"><span data-stu-id="48019-238">Validates that the data in an **Azure SQL database** or an **Azure table** contains the minimum number of rows.</span></span> |<ul><li><span data-ttu-id="48019-239">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="48019-239">Azure SQL Database</span></span></li><li><span data-ttu-id="48019-240">Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="48019-240">Azure Table</span></span></li></ul> |<span data-ttu-id="48019-241">Nej</span><span class="sxs-lookup"><span data-stu-id="48019-241">No</span></span> |<span data-ttu-id="48019-242">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="48019-242">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="48019-243">Exempel</span><span class="sxs-lookup"><span data-stu-id="48019-243">Examples</span></span>
<span data-ttu-id="48019-244">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="48019-244">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="48019-245">**minimumRows**</span><span class="sxs-lookup"><span data-stu-id="48019-245">**minimumRows**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

<span data-ttu-id="48019-246">Mer information om dessa egenskaper och exempel finns [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="48019-246">For more information about these properties and examples, see [Create datasets](data-factory-create-datasets.md) article.</span></span> 

## <a name="activity-policies"></a><span data-ttu-id="48019-247">Principer för användaraktivitet</span><span class="sxs-lookup"><span data-stu-id="48019-247">Activity policies</span></span>
<span data-ttu-id="48019-248">Principer påverkar beteendet körning av en aktivitet, särskilt när segment i en tabell som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="48019-248">Policies affect the run-time behavior of an activity, specifically when the slice of a table is processed.</span></span> <span data-ttu-id="48019-249">Följande tabell innehåller information.</span><span class="sxs-lookup"><span data-stu-id="48019-249">The following table provides the details.</span></span>

| <span data-ttu-id="48019-250">Egenskap</span><span class="sxs-lookup"><span data-stu-id="48019-250">Property</span></span> | <span data-ttu-id="48019-251">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="48019-251">Permitted values</span></span> | <span data-ttu-id="48019-252">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="48019-252">Default Value</span></span> | <span data-ttu-id="48019-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="48019-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="48019-254">Concurrency</span><span class="sxs-lookup"><span data-stu-id="48019-254">concurrency</span></span> |<span data-ttu-id="48019-255">Integer</span><span class="sxs-lookup"><span data-stu-id="48019-255">Integer</span></span> <br/><br/><span data-ttu-id="48019-256">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="48019-256">Max value: 10</span></span> |<span data-ttu-id="48019-257">1</span><span class="sxs-lookup"><span data-stu-id="48019-257">1</span></span> |<span data-ttu-id="48019-258">Antal samtidiga körningar av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-258">Number of concurrent executions of the activity.</span></span><br/><br/><span data-ttu-id="48019-259">Den avgör antalet ParallellAktivitet körningar som kan inträffa på olika segment.</span><span class="sxs-lookup"><span data-stu-id="48019-259">It determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="48019-260">Till exempel om en aktivitet behöver gå igenom snabbare en stor mängd tillgänglig data, med ett större värde för samtidighet behandling.</span><span class="sxs-lookup"><span data-stu-id="48019-260">For example, if an activity needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> |
| <span data-ttu-id="48019-261">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="48019-261">executionPriorityOrder</span></span> |<span data-ttu-id="48019-262">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="48019-262">NewestFirst</span></span><br/><br/><span data-ttu-id="48019-263">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="48019-263">OldestFirst</span></span> |<span data-ttu-id="48019-264">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="48019-264">OldestFirst</span></span> |<span data-ttu-id="48019-265">Anger den sorteringen av datasektorer som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="48019-265">Determines the ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="48019-266">Till exempel om du har 2 sektorer (en inträffar klockan 4, och en annan kl) och båda finns väntande körning.</span><span class="sxs-lookup"><span data-stu-id="48019-266">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="48019-267">Om du ställer in executionPriorityOrder ska NewestFirst bearbetas sektorn kl först.</span><span class="sxs-lookup"><span data-stu-id="48019-267">If you set the executionPriorityOrder to be NewestFirst, the slice at 5 PM is processed first.</span></span> <span data-ttu-id="48019-268">På liknande sätt om du ställer in executionPriorityORder ska OldestFIrst bearbetas sedan sektorn klockan 4.</span><span class="sxs-lookup"><span data-stu-id="48019-268">Similarly if you set the executionPriorityORder to be OldestFIrst, then the slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="48019-269">Försök igen</span><span class="sxs-lookup"><span data-stu-id="48019-269">retry</span></span> |<span data-ttu-id="48019-270">Integer</span><span class="sxs-lookup"><span data-stu-id="48019-270">Integer</span></span><br/><br/><span data-ttu-id="48019-271">Värdet för maximalt antal kan vara 10</span><span class="sxs-lookup"><span data-stu-id="48019-271">Max value can be 10</span></span> |<span data-ttu-id="48019-272">0</span><span class="sxs-lookup"><span data-stu-id="48019-272">0</span></span> |<span data-ttu-id="48019-273">Antal försök innan databearbetning för sektorn som har markerats som ett fel.</span><span class="sxs-lookup"><span data-stu-id="48019-273">Number of retries before the data processing for the slice is marked as Failure.</span></span> <span data-ttu-id="48019-274">Aktivitetskörningen för en datasektorn försöks upp till antalet angivna försök igen.</span><span class="sxs-lookup"><span data-stu-id="48019-274">Activity execution for a data slice is retried up to the specified retry count.</span></span> <span data-ttu-id="48019-275">Det nya försöket görs så snart som möjligt efter fel.</span><span class="sxs-lookup"><span data-stu-id="48019-275">The retry is done as soon as possible after the failure.</span></span> |
| <span data-ttu-id="48019-276">Timeout</span><span class="sxs-lookup"><span data-stu-id="48019-276">timeout</span></span> |<span data-ttu-id="48019-277">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="48019-277">TimeSpan</span></span> |<span data-ttu-id="48019-278">00:00:00</span><span class="sxs-lookup"><span data-stu-id="48019-278">00:00:00</span></span> |<span data-ttu-id="48019-279">Tidsgräns för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-279">Timeout for the activity.</span></span> <span data-ttu-id="48019-280">Exempel: 00:10:00 (inbegriper timeout 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="48019-280">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="48019-281">Om ett värde har angetts eller är 0, är tidsgränsen obegränsad.</span><span class="sxs-lookup"><span data-stu-id="48019-281">If a value is not specified or is 0, the timeout is infinite.</span></span><br/><br/><span data-ttu-id="48019-282">Om databearbetningstiden på ett segment överskrider timeout-värdet, det avbryts och försöker systemet att försök bearbetning.</span><span class="sxs-lookup"><span data-stu-id="48019-282">If the data processing time on a slice exceeds the timeout value, it is canceled, and the system attempts to retry the processing.</span></span> <span data-ttu-id="48019-283">Antalet försök beror på egenskapen försök igen.</span><span class="sxs-lookup"><span data-stu-id="48019-283">The number of retries depends on the retry property.</span></span> <span data-ttu-id="48019-284">När timeout uppstår är status för lång tid.</span><span class="sxs-lookup"><span data-stu-id="48019-284">When timeout occurs, the status is set to TimedOut.</span></span> |
| <span data-ttu-id="48019-285">Fördröjning</span><span class="sxs-lookup"><span data-stu-id="48019-285">delay</span></span> |<span data-ttu-id="48019-286">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="48019-286">TimeSpan</span></span> |<span data-ttu-id="48019-287">00:00:00</span><span class="sxs-lookup"><span data-stu-id="48019-287">00:00:00</span></span> |<span data-ttu-id="48019-288">Ange fördröjning innan databearbetningen av sektorn startar.</span><span class="sxs-lookup"><span data-stu-id="48019-288">Specify the delay before data processing of the slice starts.</span></span><br/><br/><span data-ttu-id="48019-289">Körningen av aktiviteten för en datasektorn startas efter fördröjningen har passerat den förväntade tiden för körningen.</span><span class="sxs-lookup"><span data-stu-id="48019-289">The execution of activity for a data slice is started after the Delay is past the expected execution time.</span></span><br/><br/><span data-ttu-id="48019-290">Exempel: 00:10:00 (inbegriper fördröjning på 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="48019-290">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="48019-291">longRetry</span><span class="sxs-lookup"><span data-stu-id="48019-291">longRetry</span></span> |<span data-ttu-id="48019-292">Integer</span><span class="sxs-lookup"><span data-stu-id="48019-292">Integer</span></span><br/><br/><span data-ttu-id="48019-293">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="48019-293">Max value: 10</span></span> |<span data-ttu-id="48019-294">1</span><span class="sxs-lookup"><span data-stu-id="48019-294">1</span></span> |<span data-ttu-id="48019-295">Antal lång nya försök innan körningen sektorn misslyckades.</span><span class="sxs-lookup"><span data-stu-id="48019-295">The number of long retry attempts before the slice execution is failed.</span></span><br/><br/><span data-ttu-id="48019-296">longRetry försök fördelade av longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="48019-296">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="48019-297">Så om du behöver ange en tid mellan nya försök använda longRetry.</span><span class="sxs-lookup"><span data-stu-id="48019-297">So if you need to specify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="48019-298">Om både försök och longRetry anges varje longRetry försök omförsök max antal försök används och försök igen * longRetry.</span><span class="sxs-lookup"><span data-stu-id="48019-298">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and the max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="48019-299">Till exempel om vi har följande inställningar i principen för aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="48019-299">For example, if we have the following settings in the activity policy:</span></span><br/><span data-ttu-id="48019-300">Försök: 3</span><span class="sxs-lookup"><span data-stu-id="48019-300">Retry: 3</span></span><br/><span data-ttu-id="48019-301">longRetry: 2</span><span class="sxs-lookup"><span data-stu-id="48019-301">longRetry: 2</span></span><br/><span data-ttu-id="48019-302">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="48019-302">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="48019-303">Anta att det finns endast en sektor att köra (status väntar) och aktivitetskörningen misslyckas varje gång.</span><span class="sxs-lookup"><span data-stu-id="48019-303">Assume there is only one slice to execute (status is Waiting) and the activity execution fails every time.</span></span> <span data-ttu-id="48019-304">Det skulle inledningsvis 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="48019-304">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="48019-305">Efter varje försök är sektorn status försök igen.</span><span class="sxs-lookup"><span data-stu-id="48019-305">After each attempt, the slice status would be Retry.</span></span> <span data-ttu-id="48019-306">Efter första 3 försök över är statusen för sektorn LongRetry.</span><span class="sxs-lookup"><span data-stu-id="48019-306">After first 3 attempts are over, the slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="48019-307">Efter en timme (det vill säga Longretryinteval's värde), skulle det finnas en annan uppsättning 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="48019-307">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="48019-308">Efter detta segment status skulle misslyckades och inga fler försök kan inte genomföras.</span><span class="sxs-lookup"><span data-stu-id="48019-308">After that, the slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="48019-309">Därför har övergripande 6 försök gjorts.</span><span class="sxs-lookup"><span data-stu-id="48019-309">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="48019-310">Om alla körning lyckas, statusen för sektorn är klar och inga fler försök görs.</span><span class="sxs-lookup"><span data-stu-id="48019-310">If any execution succeeds, the slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="48019-311">longRetry får användas i situationer där beroende data kommer till icke-deterministiska gånger eller hela miljön är flaky under vilken databearbetningen sker.</span><span class="sxs-lookup"><span data-stu-id="48019-311">longRetry may be used in situations where dependent data arrives at non-deterministic times or the overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="48019-312">I sådana fall kan detta återförsök efter varandra inte kan hjälpa och gör att när ett intervall på tid som resulterar i önskad utdata.</span><span class="sxs-lookup"><span data-stu-id="48019-312">In such cases, doing retries one after another may not help and doing so after an interval of time results in the desired output.</span></span><br/><br/><span data-ttu-id="48019-313">Liten varning: Ange inte höga värden för longRetry eller longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="48019-313">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="48019-314">Normalt en högre värden andra systemfel problem.</span><span class="sxs-lookup"><span data-stu-id="48019-314">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="48019-315">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="48019-315">longRetryInterval</span></span> |<span data-ttu-id="48019-316">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="48019-316">TimeSpan</span></span> |<span data-ttu-id="48019-317">00:00:00</span><span class="sxs-lookup"><span data-stu-id="48019-317">00:00:00</span></span> |<span data-ttu-id="48019-318">Fördröjning mellan försök har lång</span><span class="sxs-lookup"><span data-stu-id="48019-318">The delay between long retry attempts</span></span> |

<span data-ttu-id="48019-319">Mer information finns i [Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="48019-319">For more information, see [Pipelines](data-factory-create-pipelines.md) article.</span></span> 

## <a name="parallel-processing-of-data-slices"></a><span data-ttu-id="48019-320">Parallell bearbetning av datasektorer</span><span class="sxs-lookup"><span data-stu-id="48019-320">Parallel processing of data slices</span></span>
<span data-ttu-id="48019-321">Du kan ange startdatum för pipeline tidigare.</span><span class="sxs-lookup"><span data-stu-id="48019-321">You can set the start date for the pipeline in the past.</span></span> <span data-ttu-id="48019-322">När du gör det beräknas (tillbaka fyllning) alla datasektorer tidigare Data Factory automatiskt och börjar bearbeta dem.</span><span class="sxs-lookup"><span data-stu-id="48019-322">When you do so, Data Factory automatically calculates (back fills) all data slices in the past and begins processing them.</span></span> <span data-ttu-id="48019-323">Exempel: Om du skapar en pipeline med startdatum 2017-04-01 och det aktuella datumet infaller 2017-04-10.</span><span class="sxs-lookup"><span data-stu-id="48019-323">For example: if you create a pipeline with start date 2017-04-01 and the current date is 2017-04-10.</span></span> <span data-ttu-id="48019-324">Om intervall som datamängd för utdata är varje dag och sedan Data Factory startar bearbetning av alla segment från 2017-04-01 till 2017-04-09 omedelbart eftersom startdatum har passerats.</span><span class="sxs-lookup"><span data-stu-id="48019-324">If the cadence of the output dataset is daily, then Data Factory starts processing all the slices from 2017-04-01 to 2017-04-09 immediately because the start date is in the past.</span></span> <span data-ttu-id="48019-325">Sektorn från 2017-04-10 bearbetas inte ännu eftersom värdet för style-egenskapen i avsnittet tillgänglighet är EndOfInterval som standard.</span><span class="sxs-lookup"><span data-stu-id="48019-325">The slice from 2017-04-10 is not processed yet because the value of style property in the availability section is EndOfInterval by default.</span></span> <span data-ttu-id="48019-326">Äldsta sektorn bearbetas först som standard är värdet för executionPriorityOrder OldestFirst.</span><span class="sxs-lookup"><span data-stu-id="48019-326">The oldest slice is processed first as the default value of executionPriorityOrder is OldestFirst.</span></span> <span data-ttu-id="48019-327">En beskrivning av egenskapen style finns [dataset tillgänglighet](#dataset-availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="48019-327">For a description of the style property, see [dataset availability](#dataset-availability) section.</span></span> <span data-ttu-id="48019-328">En beskrivning av executionPriorityOrder-avsnittet, finns det [aktivitetsprinciper](#activity-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="48019-328">For a description of the executionPriorityOrder section, see the [activity policies](#activity-policies) section.</span></span> 

<span data-ttu-id="48019-329">Du kan konfigurera tillbaka ifylld datasektorer bearbetas parallellt genom att ange den **samtidighet** egenskap i den **princip** delen av aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="48019-329">You can configure back-filled data slices to be processed in parallel by setting the **concurrency** property in the **policy** section of the activity JSON.</span></span> <span data-ttu-id="48019-330">Den här egenskapen anger antalet ParallellAktivitet körningar som kan inträffa på olika segment.</span><span class="sxs-lookup"><span data-stu-id="48019-330">This property determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="48019-331">Standardvärdet för egenskapen samtidighet är 1.</span><span class="sxs-lookup"><span data-stu-id="48019-331">The default value for the concurrency property is 1.</span></span> <span data-ttu-id="48019-332">Därför bearbetas en sektor samtidigt som standard.</span><span class="sxs-lookup"><span data-stu-id="48019-332">Therefore, one slice is processed at a time by default.</span></span> <span data-ttu-id="48019-333">Det maximala värdet är 10.</span><span class="sxs-lookup"><span data-stu-id="48019-333">The maximum value is 10.</span></span> <span data-ttu-id="48019-334">När en pipeline behöver gå igenom en stor mängd tillgänglig data, med ett större värde för samtidighet snabbare behandling.</span><span class="sxs-lookup"><span data-stu-id="48019-334">When a pipeline needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> 

## <a name="rerun-a-failed-data-slice"></a><span data-ttu-id="48019-335">Kör en misslyckad datasektorn</span><span class="sxs-lookup"><span data-stu-id="48019-335">Rerun a failed data slice</span></span>
<span data-ttu-id="48019-336">När ett fel uppstår under bearbetning av en datasektorn du reda på varför bearbetning av ett segment misslyckats med hjälp av Azure portal blad eller övervaka och hantera appen.</span><span class="sxs-lookup"><span data-stu-id="48019-336">When an error occurs while processing a data slice, you can find out why the processing of a slice failed by using Azure portal blades or Monitor and Manage App.</span></span> <span data-ttu-id="48019-337">Se [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="48019-337">See [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md) for details.</span></span>

<span data-ttu-id="48019-338">Överväg följande exempel som visar två aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="48019-338">Consider the following example, which shows two activities.</span></span> <span data-ttu-id="48019-339">Activity1 och aktivitet 2.</span><span class="sxs-lookup"><span data-stu-id="48019-339">Activity1 and Activity 2.</span></span> <span data-ttu-id="48019-340">Activity1 använder ett segment av Dataset1 och genererar ett segment av Dataset2 som används som indata av Activity2 att skapa en sektor i den slutliga Dataset.</span><span class="sxs-lookup"><span data-stu-id="48019-340">Activity1 consumes a slice of Dataset1 and produces a slice of Dataset2, which is consumed as an input by Activity2 to produce a slice of the Final Dataset.</span></span>

![Misslyckade sektorn](./media/data-factory-scheduling-and-execution/failed-slice.png)

<span data-ttu-id="48019-342">Diagrammet visar att utanför tre senaste segment det uppstod ett fel som producerar 9 10 AM sektorn för Dataset2.</span><span class="sxs-lookup"><span data-stu-id="48019-342">The diagram shows that out of three recent slices, there was a failure producing the 9-10 AM slice for Dataset2.</span></span> <span data-ttu-id="48019-343">Data Factory spårar automatiskt beroende för tid serien dataset.</span><span class="sxs-lookup"><span data-stu-id="48019-343">Data Factory automatically tracks dependency for the time series dataset.</span></span> <span data-ttu-id="48019-344">Därför kan startar den inte aktiviteten körs för den underordnade sektorn 9-10: 00.</span><span class="sxs-lookup"><span data-stu-id="48019-344">As a result, it does not start the activity run for the 9-10 AM downstream slice.</span></span>

<span data-ttu-id="48019-345">Data Factory övervakning och hantering av verktygen kan du detaljerat diagnostikloggar att enkelt hitta den bakomliggande orsaken till problemet och åtgärda den misslyckade sektorn.</span><span class="sxs-lookup"><span data-stu-id="48019-345">Data Factory monitoring and management tools allow you to drill into the diagnostic logs for the failed slice to easily find the root cause for the issue and fix it.</span></span> <span data-ttu-id="48019-346">När problemet har åtgärdats kan du enkelt starta aktiviteten kör för att skapa den misslyckade sektorn.</span><span class="sxs-lookup"><span data-stu-id="48019-346">After you have fixed the issue, you can easily start the activity run to produce the failed slice.</span></span> <span data-ttu-id="48019-347">Läs mer om hur du kör och förstå tillståndsövergångar för datasektorer [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="48019-347">For more information on how to rerun and understand state transitions for data slices, see [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span>

<span data-ttu-id="48019-348">När du kör 9 10 AM sektorn för **Dataset2**, Data Factory startar kör för sektorn 9 10 AM beroende på den sista datamängden.</span><span class="sxs-lookup"><span data-stu-id="48019-348">After you rerun the 9-10 AM slice for **Dataset2**, Data Factory starts the run for the 9-10 AM dependent slice on the final dataset.</span></span>

![Kör misslyckade sektorn](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a><span data-ttu-id="48019-350">Flera aktiviteter i en pipeline</span><span class="sxs-lookup"><span data-stu-id="48019-350">Multiple activities in a pipeline</span></span>
<span data-ttu-id="48019-351">Du kan ha mer än en aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="48019-351">You can have more than one activity in a pipeline.</span></span> <span data-ttu-id="48019-352">Om du har flera aktiviteter i en pipeline och utdata för en aktivitet inte är indata för en annan aktivitet, kan aktiviteter köras parallellt om indata segment för aktiviteter är klar.</span><span class="sxs-lookup"><span data-stu-id="48019-352">If you have multiple activities in a pipeline and the output of an activity is not an input of another activity, the activities may run in parallel if input data slices for the activities are ready.</span></span>

<span data-ttu-id="48019-353">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-353">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="48019-354">Aktiviteter kan vara i samma rörledning eller i olika pipelines.</span><span class="sxs-lookup"><span data-stu-id="48019-354">The activities can be in the same pipeline or in different pipelines.</span></span> <span data-ttu-id="48019-355">Den andra aktiviteten körs bara när den första som har slutförts.</span><span class="sxs-lookup"><span data-stu-id="48019-355">The second activity executes only when the first one finishes successfully.</span></span>

<span data-ttu-id="48019-356">Tänk dig följande om en pipeline har två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="48019-356">For example, consider the following case where a pipeline has two activities:</span></span>

1. <span data-ttu-id="48019-357">Aktiviteten A1 som kräver indata externa datauppsättningen D1 och producerar utdatauppsättningen D2.</span><span class="sxs-lookup"><span data-stu-id="48019-357">Activity A1 that requires external input dataset D1, and produces output dataset D2.</span></span>
2. <span data-ttu-id="48019-358">Aktiviteten A2 som kräver indata från dataset D2 och producerar utdatauppsättningen D3.</span><span class="sxs-lookup"><span data-stu-id="48019-358">Activity A2 that requires input from dataset D2, and produces output dataset D3.</span></span>

<span data-ttu-id="48019-359">I det här scenariot finns aktiviteter A1 och A2 i samma rörledning.</span><span class="sxs-lookup"><span data-stu-id="48019-359">In this scenario, activities A1 and A2 are in the same pipeline.</span></span> <span data-ttu-id="48019-360">Aktiviteten A1 körs när externa data är tillgängliga och hur ofta schemalagd tillgänglighet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="48019-360">The activity A1 runs when the external data is available and the scheduled availability frequency is reached.</span></span> <span data-ttu-id="48019-361">Aktiviteten A2 körs när från D2 schemalagda segment bli tillgänglig och frekvens för schemalagd tillgänglighet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="48019-361">The activity A2 runs when the scheduled slices from D2 become available and the scheduled availability frequency is reached.</span></span> <span data-ttu-id="48019-362">Om det finns ett fel i en sektorer i dataset D2, körs inte A2 för den sektorn förrän den blir tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="48019-362">If there is an error in one of the slices in dataset D2, A2 does not run for that slice until it becomes available.</span></span>

<span data-ttu-id="48019-363">Diagramvy med både aktiviteter i samma rörledning ser ut som i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="48019-363">The Diagram view with both activities in the same pipeline would look like the following diagram:</span></span>

![Aktiviteter i samma pipeline-länkning](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

<span data-ttu-id="48019-365">Som nämnts tidigare kan aktiviteter vara i olika pipelines.</span><span class="sxs-lookup"><span data-stu-id="48019-365">As mentioned earlier, the activities could be in different pipelines.</span></span> <span data-ttu-id="48019-366">I ett sådant scenario ser diagramvyn ut som i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="48019-366">In such a scenario, the diagram view would look like the following diagram:</span></span>

![Länkning aktiviteter i två rörledningar](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

<span data-ttu-id="48019-368">Finns det [kopiera sekventiellt](#copy-sequentially) avsnitt i tillägget för ett exempel.</span><span class="sxs-lookup"><span data-stu-id="48019-368">See the [copy sequentially](#copy-sequentially) section in the appendix for an example.</span></span>

## <a name="model-datasets-with-different-frequencies"></a><span data-ttu-id="48019-369">Modellen datauppsättningar med olika frekvenser</span><span class="sxs-lookup"><span data-stu-id="48019-369">Model datasets with different frequencies</span></span>
<span data-ttu-id="48019-370">I prover har frekvenser för inkommande och utgående datauppsättningar och schemafönstret aktivitet samma.</span><span class="sxs-lookup"><span data-stu-id="48019-370">In the samples, the frequencies for input and output datasets and the activity schedule window were the same.</span></span> <span data-ttu-id="48019-371">Vissa scenarier kräver möjlighet att resultat med en frekvens som skiljer sig frekvenser på en eller flera inmatningar.</span><span class="sxs-lookup"><span data-stu-id="48019-371">Some scenarios require the ability to produce output at a frequency different than the frequencies of one or more inputs.</span></span> <span data-ttu-id="48019-372">Data Factory stöder modeling dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="48019-372">Data Factory supports modeling these scenarios.</span></span>

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a><span data-ttu-id="48019-373">Exempel 1: Skapa en daglig utdata-rapport för inkommande data som är tillgängliga i timmen</span><span class="sxs-lookup"><span data-stu-id="48019-373">Sample 1: Produce a daily output report for input data that is available every hour</span></span>
<span data-ttu-id="48019-374">Föreställ dig ett scenario där du har definierat mätdata från sensorer som är tillgängliga i timmen i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="48019-374">Consider a scenario in which you have input measurement data from sensors available every hour in Azure Blob storage.</span></span> <span data-ttu-id="48019-375">Du vill skapa en daglig sammanställda rapport med statistik som medelvärde, högsta och lägsta för dag med [Data Factory hive aktiviteten](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="48019-375">You want to produce a daily aggregate report with statistics such as mean, maximum, and minimum for the day with [Data Factory hive activity](data-factory-hive-activity.md).</span></span>

<span data-ttu-id="48019-376">Här visas hur du kan utforma det här scenariot med Data Factory:</span><span class="sxs-lookup"><span data-stu-id="48019-376">Here is how you can model this scenario with Data Factory:</span></span>

<span data-ttu-id="48019-377">**Indatauppsättning**</span><span class="sxs-lookup"><span data-stu-id="48019-377">**Input dataset**</span></span>

<span data-ttu-id="48019-378">Varje timme indatafilerna bort i mappen för den angivna dagen.</span><span class="sxs-lookup"><span data-stu-id="48019-378">The hourly input files are dropped in the folder for the given day.</span></span> <span data-ttu-id="48019-379">Tillgänglighet för indata är inställt på **timme** (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="48019-379">Availability for input is set at **Hour** (frequency: Hour, interval: 1).</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="48019-380">**Datamängd för utdata**</span><span class="sxs-lookup"><span data-stu-id="48019-380">**Output dataset**</span></span>

<span data-ttu-id="48019-381">En utdatafil skapas varje dag i dagens mapp.</span><span class="sxs-lookup"><span data-stu-id="48019-381">One output file is created every day in the day's folder.</span></span> <span data-ttu-id="48019-382">Tillgängligheten för utdata är inställt på **dag** (frekvens: dag och intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="48019-382">Availability of output is set at **Day** (frequency: Day and interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="48019-383">**Aktiviteten: hive aktivitet i en pipeline**</span><span class="sxs-lookup"><span data-stu-id="48019-383">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="48019-384">Hive-skript tar emot rätt *DateTime* information som parametrar som använder den **WindowStart** variabeln enligt följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="48019-384">The hive script receives the appropriate *DateTime* information as parameters that use the **WindowStart** variable as shown in the following snippet.</span></span> <span data-ttu-id="48019-385">Hive-skriptet använder den här variabeln för att läsa in data från rätt mapp för dag och kör sammanställning för att generera utdata.</span><span class="sxs-lookup"><span data-stu-id="48019-385">The hive script uses this variable to load the data from the correct folder for the day and run the aggregation to generate the output.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

<span data-ttu-id="48019-386">Följande diagram illustrerar scenario ur en data-beroendet.</span><span class="sxs-lookup"><span data-stu-id="48019-386">The following diagram shows the scenario from a data-dependency point of view.</span></span>

![Beroendet av data](./media/data-factory-scheduling-and-execution/data-dependency.png)

<span data-ttu-id="48019-388">Sektorn utdata för varje dag beror på 24 timvis segment från en datamängd som indata.</span><span class="sxs-lookup"><span data-stu-id="48019-388">The output slice for every day depends on 24 hourly slices from an input dataset.</span></span> <span data-ttu-id="48019-389">Data Factory beräknar automatiskt dessa beroenden genom att ta reda på indata segment som faller inom samma tidsperiod som utdata sektorn ska produceras.</span><span class="sxs-lookup"><span data-stu-id="48019-389">Data Factory computes these dependencies automatically by figuring out the input data slices that fall in the same time period as the output slice to be produced.</span></span> <span data-ttu-id="48019-390">Om någon av de 24 inkommande segment inte är tillgänglig väntar Data Factory på indata sektorn ska bli klar innan du startar daglig aktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="48019-390">If any of the 24 input slices is not available, Data Factory waits for the input slice to be ready before starting the daily activity run.</span></span>

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a><span data-ttu-id="48019-391">Exempel 2: Ange samband med uttryck och Data Factory-funktioner</span><span class="sxs-lookup"><span data-stu-id="48019-391">Sample 2: Specify dependency with expressions and Data Factory functions</span></span>
<span data-ttu-id="48019-392">Nu ska vi titta ett annat scenario.</span><span class="sxs-lookup"><span data-stu-id="48019-392">Let’s consider another scenario.</span></span> <span data-ttu-id="48019-393">Anta att du har en hive-aktivitet som bearbetar två inkommande datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="48019-393">Suppose you have a hive activity that processes two input datasets.</span></span> <span data-ttu-id="48019-394">En av dem har nya data varje dag, men en av dem hämtar nya data varje vecka.</span><span class="sxs-lookup"><span data-stu-id="48019-394">One of them has new data daily, but one of them gets new data every week.</span></span> <span data-ttu-id="48019-395">Anta att du vill göra en koppling mellan de två indatavärdena och skapar ett utgående varje dag.</span><span class="sxs-lookup"><span data-stu-id="48019-395">Suppose you wanted to do a join across the two inputs and produce an output every day.</span></span>

<span data-ttu-id="48019-396">Enkel metod i vilken Data Factory automatiskt siffrorna ut höger mata in segment att bearbeta genom att justera till utgående data tidsintervallet tidsperiod inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="48019-396">The simple approach in which Data Factory automatically figures out the right input slices to process by aligning to the output data slice’s time period does not work.</span></span>

<span data-ttu-id="48019-397">Du måste ange att för varje aktivitet som kör Data Factory ska använda föregående vecka datasektorn för varje vecka inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="48019-397">You must specify that for every activity run, the Data Factory should use last week’s data slice for the weekly input dataset.</span></span> <span data-ttu-id="48019-398">Du kan använda Azure Data Factory-funktioner som visas i följande fragment för att genomföra det här beteendet.</span><span class="sxs-lookup"><span data-stu-id="48019-398">You use Azure Data Factory functions as shown in the following snippet to implement this behavior.</span></span>

<span data-ttu-id="48019-399">**Input1: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="48019-399">**Input1: Azure blob**</span></span>

<span data-ttu-id="48019-400">Första indata är Azure blob som uppdateras dagligen.</span><span class="sxs-lookup"><span data-stu-id="48019-400">The first input is the Azure blob being updated daily.</span></span>

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="48019-401">**Input2: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="48019-401">**Input2: Azure blob**</span></span>

<span data-ttu-id="48019-402">Input2 är Azure-blobben uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="48019-402">Input2 is the Azure blob being updated weekly.</span></span>

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

<span data-ttu-id="48019-403">**Utdata: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="48019-403">**Output: Azure blob**</span></span>

<span data-ttu-id="48019-404">En utdatafil skapas varje dag i mappen för dagen.</span><span class="sxs-lookup"><span data-stu-id="48019-404">One output file is created every day in the folder for the day.</span></span> <span data-ttu-id="48019-405">Tillgängligheten för utdata är inställd på **dag** (frekvens: Day, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="48019-405">Availability of output is set to **day** (frequency: Day, interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="48019-406">**Aktiviteten: hive aktivitet i en pipeline**</span><span class="sxs-lookup"><span data-stu-id="48019-406">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="48019-407">Aktiviteten hive tar de två indatavärdena och genererar ett utdata segment varje dag.</span><span class="sxs-lookup"><span data-stu-id="48019-407">The hive activity takes the two inputs and produces an output slice every day.</span></span> <span data-ttu-id="48019-408">Du kan ange varje dag utdata sektion så att den är beroende av föregående vecka inkommande segment för varje vecka indata på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="48019-408">You can specify every day’s output slice to depend on the previous week’s input slice for weekly input as follows.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

<span data-ttu-id="48019-409">Se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md) en lista över funktioner och systemvariabler som har stöd för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="48019-409">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of functions and system variables that Data Factory supports.</span></span>

## <a name="appendix"></a><span data-ttu-id="48019-410">Bilaga</span><span class="sxs-lookup"><span data-stu-id="48019-410">Appendix</span></span>

### <a name="example-copy-sequentially"></a><span data-ttu-id="48019-411">Exempel: kopiera sekventiellt</span><span class="sxs-lookup"><span data-stu-id="48019-411">Example: copy sequentially</span></span>
<span data-ttu-id="48019-412">Det är möjligt att köra flera kopieringsåtgärder efter varandra på ett sätt som sekventiella/sorterade.</span><span class="sxs-lookup"><span data-stu-id="48019-412">It is possible to run multiple copy operations one after another in a sequential/ordered manner.</span></span> <span data-ttu-id="48019-413">Du kan till exempel ha två kopiera aktiviteter i en pipeline (CopyActivity1 och CopyActivity2) med följande indata utdata datauppsättningar:</span><span class="sxs-lookup"><span data-stu-id="48019-413">For example, you might have two copy activities in a pipeline (CopyActivity1 and CopyActivity2) with the following input data output datasets:</span></span>   

<span data-ttu-id="48019-414">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="48019-414">CopyActivity1</span></span>

<span data-ttu-id="48019-415">Indata: Dataset.</span><span class="sxs-lookup"><span data-stu-id="48019-415">Input: Dataset.</span></span> <span data-ttu-id="48019-416">Utdata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="48019-416">Output: Dataset2.</span></span>

<span data-ttu-id="48019-417">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="48019-417">CopyActivity2</span></span>

<span data-ttu-id="48019-418">Indata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="48019-418">Input: Dataset2.</span></span>  <span data-ttu-id="48019-419">Utdata: Dataset3.</span><span class="sxs-lookup"><span data-stu-id="48019-419">Output: Dataset3.</span></span>

<span data-ttu-id="48019-420">CopyActivity2 körs bara om CopyActivity1 har körts och Dataset2 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="48019-420">CopyActivity2 would run only if the CopyActivity1 has run successfully and Dataset2 is available.</span></span>

<span data-ttu-id="48019-421">Här är exempel pipeline-JSON:</span><span class="sxs-lookup"><span data-stu-id="48019-421">Here is the sample pipeline JSON:</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="48019-422">Observera att datamängd för utdata för den första kopia aktiviteten (Dataset2) har angetts i det här exemplet som indata för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-422">Notice that in the example, the output dataset of the first copy activity (Dataset2) is specified as input for the second activity.</span></span> <span data-ttu-id="48019-423">Därför kan körs den andra aktiviteten bara datamängd för utdata från den första aktiviteten är klar.</span><span class="sxs-lookup"><span data-stu-id="48019-423">Therefore, the second activity runs only when the output dataset from the first activity is ready.</span></span>  

<span data-ttu-id="48019-424">I det här exemplet CopyActivity2 kan ha en annan inmatning, till exempel Dataset3, men du anger Dataset2 som indata till CopyActivity2, så att aktiviteten inte körs förrän CopyActivity1 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="48019-424">In the example, CopyActivity2 can have a different input, such as Dataset3, but you specify Dataset2 as an input to CopyActivity2, so the activity does not run until CopyActivity1 finishes.</span></span> <span data-ttu-id="48019-425">Exempel:</span><span class="sxs-lookup"><span data-stu-id="48019-425">For example:</span></span>

<span data-ttu-id="48019-426">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="48019-426">CopyActivity1</span></span>

<span data-ttu-id="48019-427">Indata: Dataset1.</span><span class="sxs-lookup"><span data-stu-id="48019-427">Input: Dataset1.</span></span> <span data-ttu-id="48019-428">Utdata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="48019-428">Output: Dataset2.</span></span>

<span data-ttu-id="48019-429">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="48019-429">CopyActivity2</span></span>

<span data-ttu-id="48019-430">Indata: Dataset3, Dataset2.</span><span class="sxs-lookup"><span data-stu-id="48019-430">Inputs: Dataset3, Dataset2.</span></span> <span data-ttu-id="48019-431">Utdata: Dataset4.</span><span class="sxs-lookup"><span data-stu-id="48019-431">Output: Dataset4.</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="48019-432">Observera att två indatauppsättningar har angetts i det här exemplet för andra kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48019-432">Notice that in the example, two input datasets are specified for the second copy activity.</span></span> <span data-ttu-id="48019-433">När flera inmatningar anges endast den första inkommande datauppsättningen används för att kopiera data, men andra datauppsättningar som används som beroenden.</span><span class="sxs-lookup"><span data-stu-id="48019-433">When multiple inputs are specified, only the first input dataset is used for copying data, but other datasets are used as dependencies.</span></span> <span data-ttu-id="48019-434">CopyActivity2 skulle startas endast efter att följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="48019-434">CopyActivity2 would start only after the following conditions are met:</span></span>

* <span data-ttu-id="48019-435">CopyActivity1 har slutförts och Dataset2 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="48019-435">CopyActivity1 has successfully completed and Dataset2 is available.</span></span> <span data-ttu-id="48019-436">Den här datauppsättningen används inte när du kopierar data till Dataset4.</span><span class="sxs-lookup"><span data-stu-id="48019-436">This dataset is not used when copying data to Dataset4.</span></span> <span data-ttu-id="48019-437">Det fungerar bara som ett schemaläggning beroende för CopyActivity2.</span><span class="sxs-lookup"><span data-stu-id="48019-437">It only acts as a scheduling dependency for CopyActivity2.</span></span>   
* <span data-ttu-id="48019-438">Dataset3 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="48019-438">Dataset3 is available.</span></span> <span data-ttu-id="48019-439">Den här datauppsättningen representerar de data som kopieras till målet.</span><span class="sxs-lookup"><span data-stu-id="48019-439">This dataset represents the data that is copied to the destination.</span></span> 