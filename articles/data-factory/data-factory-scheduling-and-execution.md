---
title: "aaaScheduling och körning med Data Factory | Microsoft Docs"
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
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a><span data-ttu-id="c8968-103">Data Factory schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="c8968-103">Data Factory scheduling and execution</span></span>
<span data-ttu-id="c8968-104">Den här artikeln förklarar hello schemaläggning och körning av aspekter av hello Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="c8968-104">This article explains hello scheduling and execution aspects of hello Azure Data Factory application model.</span></span> <span data-ttu-id="c8968-105">Den här artikeln förutsätter att du förstår grunderna för Data Factory programmet modellen begrepp, inklusive aktivitet, rörledningar, länkade tjänster och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c8968-105">This article assumes that you understand basics of Data Factory application model concepts, including activity, pipelines, linked services, and datasets.</span></span> <span data-ttu-id="c8968-106">Grundläggande begrepp för Azure Data Factory finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="c8968-106">For basic concepts of Azure Data Factory, see hello following articles:</span></span>

* [<span data-ttu-id="c8968-107">Introduktion tooData fabriken</span><span class="sxs-lookup"><span data-stu-id="c8968-107">Introduction tooData Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="c8968-108">Pipelines</span><span class="sxs-lookup"><span data-stu-id="c8968-108">Pipelines</span></span>](data-factory-create-pipelines.md)
* [<span data-ttu-id="c8968-109">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="c8968-109">Datasets</span></span>](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a><span data-ttu-id="c8968-110">Start- och sluttider för pipeline</span><span class="sxs-lookup"><span data-stu-id="c8968-110">Start and end times of pipeline</span></span>
<span data-ttu-id="c8968-111">En pipeline är aktiv endast mellan dess **starta** tid och **end** tid.</span><span class="sxs-lookup"><span data-stu-id="c8968-111">A pipeline is active only between its **start** time and **end** time.</span></span> <span data-ttu-id="c8968-112">Det utförs inte före starttiden för hello eller efter hello sluttid.</span><span class="sxs-lookup"><span data-stu-id="c8968-112">It is not executed before hello start time or after hello end time.</span></span> <span data-ttu-id="c8968-113">Om hello pipeline är pausad körs den inte oavsett dess start- och tid.</span><span class="sxs-lookup"><span data-stu-id="c8968-113">If hello pipeline is paused, it is not executed irrespective of its start and end time.</span></span> <span data-ttu-id="c8968-114">För en pipeline-toorun bör det inte pausas.</span><span class="sxs-lookup"><span data-stu-id="c8968-114">For a pipeline toorun, it should not be paused.</span></span> <span data-ttu-id="c8968-115">Du hittar dessa inställningar (start, slut, pausats) i hello pipeline definition:</span><span class="sxs-lookup"><span data-stu-id="c8968-115">You find these settings (start, end, paused) in hello pipeline definition:</span></span> 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

<span data-ttu-id="c8968-116">Mer information finns i de här egenskaperna [skapa pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c8968-116">For more information these properties, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> 


## <a name="specify-schedule-for-an-activity"></a><span data-ttu-id="c8968-117">Ange schemat för en aktivitet</span><span class="sxs-lookup"><span data-stu-id="c8968-117">Specify schedule for an activity</span></span>
<span data-ttu-id="c8968-118">Det är inte hello pipeline som körs.</span><span class="sxs-lookup"><span data-stu-id="c8968-118">It is not hello pipeline that is executed.</span></span> <span data-ttu-id="c8968-119">Det är hello aktiviteter i hello pipeline som utförs i hello övergripande kontext för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="c8968-119">It is hello activities in hello pipeline that are executed in hello overall context of hello pipeline.</span></span> <span data-ttu-id="c8968-120">Du kan ange ett återkommande schema för en aktivitet med hello **scheduler** delen av aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="c8968-120">You can specify a recurring schedule for an activity by using hello **scheduler** section of activity JSON.</span></span> <span data-ttu-id="c8968-121">T.ex, kan du schemalägga en aktivitet toorun varje timme på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c8968-121">For example, you can schedule an activity toorun hourly as follows:</span></span>  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

<span data-ttu-id="c8968-122">Som visas i följande diagram hello, ange ett schema för en aktivitet skapas en serie rullande fönster med i hello pipeline start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="c8968-122">As shown in hello following diagram, specifying a schedule for an activity creates a series of tumbling windows with in hello pipeline start and end times.</span></span> <span data-ttu-id="c8968-123">Rullande fönster är en serie med fast storlek inte överlappar, sammanhängande intervall.</span><span class="sxs-lookup"><span data-stu-id="c8968-123">Tumbling windows are a series of fixed-size non-overlapping, contiguous time intervals.</span></span> <span data-ttu-id="c8968-124">Dessa logiska rullande fönster för en aktivitet kallas **aktivitet windows**.</span><span class="sxs-lookup"><span data-stu-id="c8968-124">These logical tumbling windows for an activity are called **activity windows**.</span></span>

![Aktiviteten scheduler-exempel](media/data-factory-scheduling-and-execution/scheduler-example.png)

<span data-ttu-id="c8968-126">Hej **scheduler** -egenskapen för en aktivitet är valfria.</span><span class="sxs-lookup"><span data-stu-id="c8968-126">hello **scheduler** property for an activity is optional.</span></span> <span data-ttu-id="c8968-127">Om du anger den här egenskapen, måste den matcha hello takt som du anger i hello definitionen för utdatauppsättningen för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8968-127">If you do specify this property, it must match hello cadence you specify in hello definition of output dataset for hello activity.</span></span> <span data-ttu-id="c8968-128">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="c8968-128">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="c8968-129">Därför måste du skapa en datamängd för utdata, även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="c8968-129">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> 

## <a name="specify-schedule-for-a-dataset"></a><span data-ttu-id="c8968-130">Ange schemat för en datamängd</span><span class="sxs-lookup"><span data-stu-id="c8968-130">Specify schedule for a dataset</span></span>
<span data-ttu-id="c8968-131">En aktivitet i en Data Factory-pipelinen har noll eller fler indata **datauppsättningar** och skapa en eller flera utdata-datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c8968-131">An activity in a Data Factory pipeline can take zero or more input **datasets** and produce one or more output datasets.</span></span> <span data-ttu-id="c8968-132">Du kan ange hello takt i vilken hello indata är tillgänglig för en aktivitet eller hello-utdata skapas med hjälp av hello **tillgänglighet** avsnitt i hello dataset definitioner.</span><span class="sxs-lookup"><span data-stu-id="c8968-132">For an activity, you can specify hello cadence at which hello input data is available or hello output data is produced by using hello **availability** section in hello dataset definitions.</span></span> 

<span data-ttu-id="c8968-133">**Frekvensen** i hello **tillgänglighet** avsnittet anger hello tidsenhet.</span><span class="sxs-lookup"><span data-stu-id="c8968-133">**Frequency** in hello **availability** section specifies hello time unit.</span></span> <span data-ttu-id="c8968-134">hello tillåtna värden för frekvens är: minut, timma, dag, vecka och månad.</span><span class="sxs-lookup"><span data-stu-id="c8968-134">hello allowed values for frequency are: Minute, Hour, Day, Week, and Month.</span></span> <span data-ttu-id="c8968-135">Hej **intervall** egenskap hello tillgänglighet under anger en multiplikator för frekvens.</span><span class="sxs-lookup"><span data-stu-id="c8968-135">hello **interval** property in hello availability section specifies a multiplier for frequency.</span></span> <span data-ttu-id="c8968-136">Exempel: om hello frekvens anges tooDay och intervallet anges too1 för en datamängd för utdata, hello-utdata skapas varje dag.</span><span class="sxs-lookup"><span data-stu-id="c8968-136">For example: if hello frequency is set tooDay and interval is set too1 for an output dataset, hello output data is produced daily.</span></span> <span data-ttu-id="c8968-137">Om du anger hello frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15.</span><span class="sxs-lookup"><span data-stu-id="c8968-137">If you specify hello frequency as minute, we recommend that you set hello interval toono less than 15.</span></span> 

<span data-ttu-id="c8968-138">I följande exempel hello, hello indata finns varje timme och hello-utdata skapas varje timme (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="c8968-138">In hello following example, hello input data is available hourly and hello output data is produced hourly (`"frequency": "Hour", "interval": 1`).</span></span> 

<span data-ttu-id="c8968-139">**Indatauppsättning:**</span><span class="sxs-lookup"><span data-stu-id="c8968-139">**Input dataset:**</span></span> 

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


<span data-ttu-id="c8968-140">**Datamängd för utdata**</span><span class="sxs-lookup"><span data-stu-id="c8968-140">**Output dataset**</span></span>

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

<span data-ttu-id="c8968-141">För närvarande **utdata dataset enheter hello schema**.</span><span class="sxs-lookup"><span data-stu-id="c8968-141">Currently, **output dataset drives hello schedule**.</span></span> <span data-ttu-id="c8968-142">Med andra ord är hello schemat för utdatauppsättningen hello används toorun en aktivitet vid körning.</span><span class="sxs-lookup"><span data-stu-id="c8968-142">In other words, hello schedule specified for hello output dataset is used toorun an activity at runtime.</span></span> <span data-ttu-id="c8968-143">Därför måste du skapa en datamängd för utdata, även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="c8968-143">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="c8968-144">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-144">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 

<span data-ttu-id="c8968-145">I följande hello pipeline-definition, hello **scheduler** egenskapen är används toospecify schema för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c8968-145">In hello following pipeline definition, hello **scheduler** property is used toospecify schedule for hello activity.</span></span> <span data-ttu-id="c8968-146">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="c8968-146">This property is optional.</span></span> <span data-ttu-id="c8968-147">För närvarande måste hello schema för hello aktiviteten matcha hello schemat för hello datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="c8968-147">Currently, hello schedule for hello activity must match hello schedule specified for hello output dataset.</span></span>
 
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

<span data-ttu-id="c8968-148">I det här exemplet hello aktiviteten körs varje timme mellan hello start och sluttider för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="c8968-148">In this example, hello activity runs hourly between hello start and end times of hello pipeline.</span></span> <span data-ttu-id="c8968-149">hello-utdata skapas varje timme för tre timmars windows (8: 00 – 9 AM, 9: 00 – 10: 00, och 10 AM - 11: 00).</span><span class="sxs-lookup"><span data-stu-id="c8968-149">hello output data is produced hourly for three-hour windows (8 AM - 9 AM, 9 AM - 10 AM, and 10 AM - 11 AM).</span></span> 

<span data-ttu-id="c8968-150">Varje dataenhet förbrukas eller produceras av en aktivitet som körs kallas en **datasektorn**.</span><span class="sxs-lookup"><span data-stu-id="c8968-150">Each unit of data consumed or produced by an activity run is called a **data slice**.</span></span> <span data-ttu-id="c8968-151">hello följande diagram visar ett exempel på en aktivitet med en inkommande datauppsättning och en datamängd för utdata:</span><span class="sxs-lookup"><span data-stu-id="c8968-151">hello following diagram shows an example of an activity with one input dataset and one output dataset:</span></span> 

![Tillgänglighet Schemaläggaren](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

<span data-ttu-id="c8968-153">hello diagram visar hello varje timme datasektorer för hello indata och utdata dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-153">hello diagram shows hello hourly data slices for hello input and output dataset.</span></span> <span data-ttu-id="c8968-154">hello diagram visar tre inkommande segment som är klara för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="c8968-154">hello diagram shows three input slices that are ready for processing.</span></span> <span data-ttu-id="c8968-155">hello 10 11 AM aktivitet pågår, producerar hello 10 11 AM utdata sektorn.</span><span class="sxs-lookup"><span data-stu-id="c8968-155">hello 10-11 AM activity is in progress, producing hello 10-11 AM output slice.</span></span> 

<span data-ttu-id="c8968-156">Du kan komma åt hello tidsintervall som är kopplat till hello aktuellt segment i hello dataset JSON genom att använda variabler: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) och [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="c8968-156">You can access hello time interval associated with hello current slice in hello dataset JSON by using variables: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) and [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="c8968-157">På liknande sätt kan du komma åt hello tidsintervall som är associerad med ett fönster med en aktivitet med hjälp av hello WindowStart och WindowEnd.</span><span class="sxs-lookup"><span data-stu-id="c8968-157">Similarly, you can access hello time interval associated with an activity window by using hello WindowStart and WindowEnd.</span></span> <span data-ttu-id="c8968-158">hello schemat för en aktivitet måste matcha hello schemat för hello utdatauppsättningen för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8968-158">hello schedule of an activity must match hello schedule of hello output dataset for hello activity.</span></span> <span data-ttu-id="c8968-159">Därför hello SliceStart och SliceEnd värden är hello samma som WindowStart och WindowEnd värden respektive.</span><span class="sxs-lookup"><span data-stu-id="c8968-159">Therefore, hello SliceStart and SliceEnd values are hello same as WindowStart and WindowEnd values respectively.</span></span> <span data-ttu-id="c8968-160">Mer information om dessa variabler finns [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md#data-factory-system-variables) artiklar.</span><span class="sxs-lookup"><span data-stu-id="c8968-160">For more information on these variables, see [Data Factory functions and system variables](data-factory-functions-variables.md#data-factory-system-variables) articles.</span></span>  

<span data-ttu-id="c8968-161">Du kan använda dessa variabler för olika ändamål i din aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="c8968-161">You can use these variables for different purposes in your activity JSON.</span></span> <span data-ttu-id="c8968-162">T.ex, du kan använda dem tooselect data från inkommande och utgående datauppsättningar som representerar tid series-data (till exempel: 8 AM too9 är).</span><span class="sxs-lookup"><span data-stu-id="c8968-162">For example, you can use them tooselect data from input and output datasets representing time series data (for example: 8 AM too9 AM).</span></span> <span data-ttu-id="c8968-163">Det här exemplet använder också **WindowStart** och **WindowEnd** tooselect relevanta data för en aktivitet körs och kopiera den tooa blob med lämpliga hello **folderPath**.</span><span class="sxs-lookup"><span data-stu-id="c8968-163">This example also uses **WindowStart** and **WindowEnd** tooselect relevant data for an activity run and copy it tooa blob with hello appropriate **folderPath**.</span></span> <span data-ttu-id="c8968-164">Hej **folderPath** är parametriserade toohave en separat mapp för varje timme.</span><span class="sxs-lookup"><span data-stu-id="c8968-164">hello **folderPath** is parameterized toohave a separate folder for every hour.</span></span>  

<span data-ttu-id="c8968-165">I föregående exempel hello, hello schemat för inkommande och utgående datauppsättningar för hello samma (varje timme).</span><span class="sxs-lookup"><span data-stu-id="c8968-165">In hello preceding example, hello schedule specified for input and output datasets is hello same (hourly).</span></span> <span data-ttu-id="c8968-166">Om hello inkommande datamängden för hello aktivitet är tillgänglig på en annan frekvens Säg var 15: e minut hello aktiviteten som producerar datauppsättningen utdata fortfarande körs en gång i timmen som hello utdatauppsättningen vilka enheter hello aktivitetsschema.</span><span class="sxs-lookup"><span data-stu-id="c8968-166">If hello input dataset for hello activity is available at a different frequency, say every 15 minutes, hello activity that produces this output dataset still runs once an hour as hello output dataset is what drives hello activity schedule.</span></span> <span data-ttu-id="c8968-167">Mer information finns i [modell datauppsättningar med olika frekvenser](#model-datasets-with-different-frequencies).</span><span class="sxs-lookup"><span data-stu-id="c8968-167">For more information, see [Model datasets with different frequencies](#model-datasets-with-different-frequencies).</span></span>

## <a name="dataset-availability-and-policies"></a><span data-ttu-id="c8968-168">DataSet tillgänglighet och principer</span><span class="sxs-lookup"><span data-stu-id="c8968-168">Dataset availability and policies</span></span>
<span data-ttu-id="c8968-169">Du har sett hello användning av frekvensen och intervall egenskaper under hello tillgänglighet i datauppsättningsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c8968-169">You have seen hello usage of frequency and interval properties in hello availability section of dataset definition.</span></span> <span data-ttu-id="c8968-170">Det finns några andra egenskaper som påverkar hello schemaläggning och körning av en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8968-170">There are a few other properties that affect hello scheduling and execution of an activity.</span></span> 

### <a name="dataset-availability"></a><span data-ttu-id="c8968-171">DataSet-tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="c8968-171">Dataset availability</span></span> 
<span data-ttu-id="c8968-172">hello följande tabell beskriver egenskaper som kan användas i hello **tillgänglighet** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="c8968-172">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="c8968-173">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c8968-173">Property</span></span> | <span data-ttu-id="c8968-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c8968-174">Description</span></span> | <span data-ttu-id="c8968-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="c8968-175">Required</span></span> | <span data-ttu-id="c8968-176">Standard</span><span class="sxs-lookup"><span data-stu-id="c8968-176">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c8968-177">frequency</span><span class="sxs-lookup"><span data-stu-id="c8968-177">frequency</span></span> |<span data-ttu-id="c8968-178">Anger hello tidsenhet för dataset sektorn produktion.</span><span class="sxs-lookup"><span data-stu-id="c8968-178">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="c8968-179"><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad</span><span class="sxs-lookup"><span data-stu-id="c8968-179"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="c8968-180">Ja</span><span class="sxs-lookup"><span data-stu-id="c8968-180">Yes</span></span> |<span data-ttu-id="c8968-181">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="c8968-181">NA</span></span> |
| <span data-ttu-id="c8968-182">interval</span><span class="sxs-lookup"><span data-stu-id="c8968-182">interval</span></span> |<span data-ttu-id="c8968-183">Anger en multiplikator för frekvens</span><span class="sxs-lookup"><span data-stu-id="c8968-183">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="c8968-184">”X frekvensintervall” avgör hur ofta hello segment skapas.</span><span class="sxs-lookup"><span data-stu-id="c8968-184">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="c8968-185">Om du behöver hello dataset toobe segmenterat timme, ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="c8968-185">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="c8968-186"><b>Obs</b>: Om du anger frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15</span><span class="sxs-lookup"><span data-stu-id="c8968-186"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="c8968-187">Ja</span><span class="sxs-lookup"><span data-stu-id="c8968-187">Yes</span></span> |<span data-ttu-id="c8968-188">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="c8968-188">NA</span></span> |
| <span data-ttu-id="c8968-189">format</span><span class="sxs-lookup"><span data-stu-id="c8968-189">style</span></span> |<span data-ttu-id="c8968-190">Anger om hello segment ska produceras hello start/slutet av hello intervall.</span><span class="sxs-lookup"><span data-stu-id="c8968-190">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="c8968-191">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="c8968-191">StartOfInterval</span></span></li><li><span data-ttu-id="c8968-192">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="c8968-192">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="c8968-193">Om frekvensen anges tooMonth och format anges tooEndOfInterval, tillverkas hello segment på hello sista dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="c8968-193">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="c8968-194">Om hello format anges tooStartOfInterval, produceras hello segment på hello första dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="c8968-194">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="c8968-195">Om frekvensen anges tooDay och format anges tooEndOfInterval, tillverkas hello segment i hello senaste timmen på dagen hello.</span><span class="sxs-lookup"><span data-stu-id="c8968-195">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="c8968-196">Om frekvensen anges tooHour och format anges tooEndOfInterval, tillverkas hello sektorn hello slutet av hello timme.</span><span class="sxs-lookup"><span data-stu-id="c8968-196">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="c8968-197">För ett segment för PM 1 – 2 PM period, till exempel produceras hello sektorn klockan 2.</span><span class="sxs-lookup"><span data-stu-id="c8968-197">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="c8968-198">Nej</span><span class="sxs-lookup"><span data-stu-id="c8968-198">No</span></span> |<span data-ttu-id="c8968-199">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="c8968-199">EndOfInterval</span></span> |
| <span data-ttu-id="c8968-200">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="c8968-200">anchorDateTime</span></span> |<span data-ttu-id="c8968-201">Definierar hello absolut placering i tid som används av Schemaläggaren toocompute dataset sektorn gränser.</span><span class="sxs-lookup"><span data-stu-id="c8968-201">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="c8968-202"><b>Obs</b>: om hello AnchorDateTime har datumdelar som är mer detaljerad än hello frekvens sedan hello mer detaljerade delar ignoreras.</span><span class="sxs-lookup"><span data-stu-id="c8968-202"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="c8968-203">Till exempel, om hello <b>intervall</b> är <b>varje timme</b> (frekvens: timme och intervall: 1) och hello <b>AnchorDateTime</b> innehåller <b>minuter och sekunder</b>, sedan hello <b>minuter och sekunder</b> delar av hello AnchorDateTime ignoreras.</span><span class="sxs-lookup"><span data-stu-id="c8968-203">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b>, then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="c8968-204">Nej</span><span class="sxs-lookup"><span data-stu-id="c8968-204">No</span></span> |<span data-ttu-id="c8968-205">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="c8968-205">01/01/0001</span></span> |
| <span data-ttu-id="c8968-206">förskjutning</span><span class="sxs-lookup"><span data-stu-id="c8968-206">offset</span></span> |<span data-ttu-id="c8968-207">TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat.</span><span class="sxs-lookup"><span data-stu-id="c8968-207">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="c8968-208"><b>Obs</b>: om både anchorDateTime och offset anges hello resultatet är hello kombineras SKIFT.</span><span class="sxs-lookup"><span data-stu-id="c8968-208"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="c8968-209">Nej</span><span class="sxs-lookup"><span data-stu-id="c8968-209">No</span></span> |<span data-ttu-id="c8968-210">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="c8968-210">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="c8968-211">Offset exempel</span><span class="sxs-lookup"><span data-stu-id="c8968-211">offset example</span></span>
<span data-ttu-id="c8968-212">Som standard varje dag (`"frequency": "Day", "interval": 1`) segment som börjar vid 12: 00 UTC-tid (midnatt).</span><span class="sxs-lookup"><span data-stu-id="c8968-212">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM UTC time (midnight).</span></span> <span data-ttu-id="c8968-213">Om du vill hello start tid toobe 06: 00 UTC-tid i stället anger du hello förskjutning som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="c8968-213">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="c8968-214">anchorDateTime-exempel</span><span class="sxs-lookup"><span data-stu-id="c8968-214">anchorDateTime example</span></span>
<span data-ttu-id="c8968-215">I följande exempel hello, produceras hello dataset var 23: e timme.</span><span class="sxs-lookup"><span data-stu-id="c8968-215">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="c8968-216">hello första sektorn startar vid hello tid som anges av hello anchorDateTime som har angetts för`2017-04-19T08:00:00` (UTC-tid).</span><span class="sxs-lookup"><span data-stu-id="c8968-216">hello first slice starts at hello time specified by hello anchorDateTime, which is set too`2017-04-19T08:00:00` (UTC time).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="c8968-217">förskjutning/stil exempel</span><span class="sxs-lookup"><span data-stu-id="c8968-217">offset/style Example</span></span>
<span data-ttu-id="c8968-218">hello följande dataset månatliga datauppsättning och skapas på 3: e varje månad kl 8:00 (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="c8968-218">hello following dataset is a monthly dataset and is produced on 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a><span data-ttu-id="c8968-219">DataSet-princip</span><span class="sxs-lookup"><span data-stu-id="c8968-219">Dataset policy</span></span>
<span data-ttu-id="c8968-220">En dataset kan ha en verifieringsprincip har definierats som anger hur hello data som genereras av en sektor körning kan valideras innan den är klar för användning.</span><span class="sxs-lookup"><span data-stu-id="c8968-220">A dataset can have a validation policy defined that specifies how hello data generated by a slice execution can be validated before it is ready for consumption.</span></span> <span data-ttu-id="c8968-221">I sådana fall när hello sektorn är klar körning och hello utdata sektorn status ändras för**väntar på** med en substatus av **validering**.</span><span class="sxs-lookup"><span data-stu-id="c8968-221">In such cases, after hello slice has finished execution, hello output slice status is changed too**Waiting** with a substatus of **Validation**.</span></span> <span data-ttu-id="c8968-222">Efter hello segment validerats, hello sektorn status ändras för**klar**.</span><span class="sxs-lookup"><span data-stu-id="c8968-222">After hello slices are validated, hello slice status changes too**Ready**.</span></span> <span data-ttu-id="c8968-223">Om en datasektorn har producerats men validerades inte hello, bearbetas inte aktiviteten körs för underordnade segment som är beroende av det här segmentet.</span><span class="sxs-lookup"><span data-stu-id="c8968-223">If a data slice has been produced but did not pass hello validation, activity runs for downstream slices that depend on this slice are not processed.</span></span> <span data-ttu-id="c8968-224">[Övervaka och hantera pipelines](data-factory-monitor-manage-pipelines.md) omfattar hello olika lägen för datasektorer i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c8968-224">[Monitor and manage pipelines](data-factory-monitor-manage-pipelines.md) covers hello various states of data slices in Data Factory.</span></span>

<span data-ttu-id="c8968-225">Hej **princip** avsnitt i datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="c8968-225">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <span data-ttu-id="c8968-226">hello följande tabell beskriver egenskaper som kan användas i hello **princip** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="c8968-226">hello following table describes properties you can use in hello **policy** section:</span></span>

| <span data-ttu-id="c8968-227">Principnamn</span><span class="sxs-lookup"><span data-stu-id="c8968-227">Policy Name</span></span> | <span data-ttu-id="c8968-228">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c8968-228">Description</span></span> | <span data-ttu-id="c8968-229">Används för</span><span class="sxs-lookup"><span data-stu-id="c8968-229">Applied too</span></span>| <span data-ttu-id="c8968-230">Krävs</span><span class="sxs-lookup"><span data-stu-id="c8968-230">Required</span></span> | <span data-ttu-id="c8968-231">Standard</span><span class="sxs-lookup"><span data-stu-id="c8968-231">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="c8968-232">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="c8968-232">minimumSizeMB</span></span> | <span data-ttu-id="c8968-233">Verifierar att hello-data i en **Azure blob** uppfyller hello krav på minsta storlek (i megabyte).</span><span class="sxs-lookup"><span data-stu-id="c8968-233">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="c8968-234">Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="c8968-234">Azure Blob</span></span> |<span data-ttu-id="c8968-235">Nej</span><span class="sxs-lookup"><span data-stu-id="c8968-235">No</span></span> |<span data-ttu-id="c8968-236">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="c8968-236">NA</span></span> |
| <span data-ttu-id="c8968-237">minimumRows</span><span class="sxs-lookup"><span data-stu-id="c8968-237">minimumRows</span></span> | <span data-ttu-id="c8968-238">Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader.</span><span class="sxs-lookup"><span data-stu-id="c8968-238">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="c8968-239">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c8968-239">Azure SQL Database</span></span></li><li><span data-ttu-id="c8968-240">Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="c8968-240">Azure Table</span></span></li></ul> |<span data-ttu-id="c8968-241">Nej</span><span class="sxs-lookup"><span data-stu-id="c8968-241">No</span></span> |<span data-ttu-id="c8968-242">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="c8968-242">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="c8968-243">Exempel</span><span class="sxs-lookup"><span data-stu-id="c8968-243">Examples</span></span>
<span data-ttu-id="c8968-244">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="c8968-244">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="c8968-245">**minimumRows**</span><span class="sxs-lookup"><span data-stu-id="c8968-245">**minimumRows**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

<span data-ttu-id="c8968-246">Mer information om dessa egenskaper och exempel finns [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c8968-246">For more information about these properties and examples, see [Create datasets](data-factory-create-datasets.md) article.</span></span> 

## <a name="activity-policies"></a><span data-ttu-id="c8968-247">Principer för användaraktivitet</span><span class="sxs-lookup"><span data-stu-id="c8968-247">Activity policies</span></span>
<span data-ttu-id="c8968-248">Principer påverkar hello körning beteendet för en aktivitet, särskilt när hello segment i en tabell som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="c8968-248">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="c8968-249">hello följande tabell innehåller hello information.</span><span class="sxs-lookup"><span data-stu-id="c8968-249">hello following table provides hello details.</span></span>

| <span data-ttu-id="c8968-250">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c8968-250">Property</span></span> | <span data-ttu-id="c8968-251">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="c8968-251">Permitted values</span></span> | <span data-ttu-id="c8968-252">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="c8968-252">Default Value</span></span> | <span data-ttu-id="c8968-253">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c8968-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c8968-254">Concurrency</span><span class="sxs-lookup"><span data-stu-id="c8968-254">concurrency</span></span> |<span data-ttu-id="c8968-255">Integer</span><span class="sxs-lookup"><span data-stu-id="c8968-255">Integer</span></span> <br/><br/><span data-ttu-id="c8968-256">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="c8968-256">Max value: 10</span></span> |<span data-ttu-id="c8968-257">1</span><span class="sxs-lookup"><span data-stu-id="c8968-257">1</span></span> |<span data-ttu-id="c8968-258">Antal samtidiga körningar av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8968-258">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="c8968-259">Den avgör hello antalet ParallellAktivitet körningar som kan inträffa på olika segment.</span><span class="sxs-lookup"><span data-stu-id="c8968-259">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="c8968-260">Till exempel om en aktivitet måste toogo via snabbare en stor mängd tillgänglig data, med ett större värde för samtidighet hello databearbetning.</span><span class="sxs-lookup"><span data-stu-id="c8968-260">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="c8968-261">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="c8968-261">executionPriorityOrder</span></span> |<span data-ttu-id="c8968-262">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="c8968-262">NewestFirst</span></span><br/><br/><span data-ttu-id="c8968-263">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="c8968-263">OldestFirst</span></span> |<span data-ttu-id="c8968-264">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="c8968-264">OldestFirst</span></span> |<span data-ttu-id="c8968-265">Anger hello sorteringen av datasektorer som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="c8968-265">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="c8968-266">Till exempel om du har 2 sektorer (en inträffar klockan 4, och en annan kl) och båda finns väntande körning.</span><span class="sxs-lookup"><span data-stu-id="c8968-266">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="c8968-267">Om du ställer in hello executionPriorityOrder toobe NewestFirst bearbetas hello sektorn kl först.</span><span class="sxs-lookup"><span data-stu-id="c8968-267">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="c8968-268">På liknande sätt om du ställer in hello executionPriorityORder toobe OldestFIrst bearbetas sedan hello sektorn klockan 4.</span><span class="sxs-lookup"><span data-stu-id="c8968-268">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="c8968-269">retry</span><span class="sxs-lookup"><span data-stu-id="c8968-269">retry</span></span> |<span data-ttu-id="c8968-270">Integer</span><span class="sxs-lookup"><span data-stu-id="c8968-270">Integer</span></span><br/><br/><span data-ttu-id="c8968-271">Värdet för maximalt antal kan vara 10</span><span class="sxs-lookup"><span data-stu-id="c8968-271">Max value can be 10</span></span> |<span data-ttu-id="c8968-272">0</span><span class="sxs-lookup"><span data-stu-id="c8968-272">0</span></span> |<span data-ttu-id="c8968-273">Antalet försök innan hello databearbetning för hello segment har markerats som ett fel.</span><span class="sxs-lookup"><span data-stu-id="c8968-273">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="c8968-274">Aktivitetskörningen för en datasektorn försöks in toohello angetts antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="c8968-274">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="c8968-275">hello försök görs så snart som möjligt efter hello-fel.</span><span class="sxs-lookup"><span data-stu-id="c8968-275">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="c8968-276">timeout</span><span class="sxs-lookup"><span data-stu-id="c8968-276">timeout</span></span> |<span data-ttu-id="c8968-277">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c8968-277">TimeSpan</span></span> |<span data-ttu-id="c8968-278">00:00:00</span><span class="sxs-lookup"><span data-stu-id="c8968-278">00:00:00</span></span> |<span data-ttu-id="c8968-279">Tidsgräns för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8968-279">Timeout for hello activity.</span></span> <span data-ttu-id="c8968-280">Exempel: 00:10:00 (inbegriper timeout 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="c8968-280">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="c8968-281">Om ett värde har angetts eller är 0, är hello timeout oändligt.</span><span class="sxs-lookup"><span data-stu-id="c8968-281">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="c8968-282">Om hello bearbetningstid på ett segment överskrider hello timeout-värde, det avbryts och hello system försöker tooretry hello bearbetning.</span><span class="sxs-lookup"><span data-stu-id="c8968-282">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="c8968-283">hello antal återförsök beror på hello försök egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c8968-283">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="c8968-284">När timeout uppstår status hello tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="c8968-284">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="c8968-285">Fördröjning</span><span class="sxs-lookup"><span data-stu-id="c8968-285">delay</span></span> |<span data-ttu-id="c8968-286">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c8968-286">TimeSpan</span></span> |<span data-ttu-id="c8968-287">00:00:00</span><span class="sxs-lookup"><span data-stu-id="c8968-287">00:00:00</span></span> |<span data-ttu-id="c8968-288">Ange hello fördröjning innan data bearbetning av hello segment startas.</span><span class="sxs-lookup"><span data-stu-id="c8968-288">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="c8968-289">hello körningen av aktiviteten för en datasektorn startas efter att hello fördröjning har förfallit hello förväntades körningstid.</span><span class="sxs-lookup"><span data-stu-id="c8968-289">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="c8968-290">Exempel: 00:10:00 (inbegriper fördröjning på 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="c8968-290">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="c8968-291">longRetry</span><span class="sxs-lookup"><span data-stu-id="c8968-291">longRetry</span></span> |<span data-ttu-id="c8968-292">Integer</span><span class="sxs-lookup"><span data-stu-id="c8968-292">Integer</span></span><br/><br/><span data-ttu-id="c8968-293">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="c8968-293">Max value: 10</span></span> |<span data-ttu-id="c8968-294">1</span><span class="sxs-lookup"><span data-stu-id="c8968-294">1</span></span> |<span data-ttu-id="c8968-295">hello antal lång försök försök innan hello sektorn körningen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="c8968-295">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="c8968-296">longRetry försök fördelade av longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="c8968-296">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="c8968-297">Så om du behöver toospecify taget mellan nya försök använda longRetry.</span><span class="sxs-lookup"><span data-stu-id="c8968-297">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="c8968-298">Om både försök och longRetry anges varje longRetry försök omförsök Hej max antal försök används och försök igen * longRetry.</span><span class="sxs-lookup"><span data-stu-id="c8968-298">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="c8968-299">Om till exempel har vi hello följande inställningar i hello aktivitetsprincip:</span><span class="sxs-lookup"><span data-stu-id="c8968-299">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="c8968-300">Försök: 3</span><span class="sxs-lookup"><span data-stu-id="c8968-300">Retry: 3</span></span><br/><span data-ttu-id="c8968-301">longRetry: 2</span><span class="sxs-lookup"><span data-stu-id="c8968-301">longRetry: 2</span></span><br/><span data-ttu-id="c8968-302">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="c8968-302">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="c8968-303">Anta att det finns bara ett segment tooexecute (status väntar) och hello aktivitetskörningen misslyckas varje gång.</span><span class="sxs-lookup"><span data-stu-id="c8968-303">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="c8968-304">Det skulle inledningsvis 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="c8968-304">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="c8968-305">Efter varje försök skulle hello sektorn status vara försök igen.</span><span class="sxs-lookup"><span data-stu-id="c8968-305">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="c8968-306">Efter första 3 försök är över, är hello sektorn status LongRetry.</span><span class="sxs-lookup"><span data-stu-id="c8968-306">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="c8968-307">Efter en timme (det vill säga Longretryinteval's värde), skulle det finnas en annan uppsättning 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="c8968-307">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="c8968-308">Efter detta hello sektorn status skulle misslyckades och inga fler försök kan inte genomföras.</span><span class="sxs-lookup"><span data-stu-id="c8968-308">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="c8968-309">Därför har övergripande 6 försök gjorts.</span><span class="sxs-lookup"><span data-stu-id="c8968-309">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="c8968-310">Om alla körning lyckas hello sektorn status är klar och inga fler försök görs.</span><span class="sxs-lookup"><span data-stu-id="c8968-310">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="c8968-311">longRetry får användas i situationer där beroende data kommer till icke-deterministiska gånger eller hello hela miljön är flaky under vilken databearbetningen sker.</span><span class="sxs-lookup"><span data-stu-id="c8968-311">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="c8968-312">I sådana fall kan inte göra återförsök efter varandra kan hjälpa och gör det med tiden resulterar i hello tidsintervall önskad utdata.</span><span class="sxs-lookup"><span data-stu-id="c8968-312">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="c8968-313">Liten varning: Ange inte höga värden för longRetry eller longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="c8968-313">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="c8968-314">Normalt en högre värden andra systemfel problem.</span><span class="sxs-lookup"><span data-stu-id="c8968-314">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="c8968-315">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="c8968-315">longRetryInterval</span></span> |<span data-ttu-id="c8968-316">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="c8968-316">TimeSpan</span></span> |<span data-ttu-id="c8968-317">00:00:00</span><span class="sxs-lookup"><span data-stu-id="c8968-317">00:00:00</span></span> |<span data-ttu-id="c8968-318">hello fördröjning mellan försök har lång</span><span class="sxs-lookup"><span data-stu-id="c8968-318">hello delay between long retry attempts</span></span> |

<span data-ttu-id="c8968-319">Mer information finns i [Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c8968-319">For more information, see [Pipelines](data-factory-create-pipelines.md) article.</span></span> 

## <a name="parallel-processing-of-data-slices"></a><span data-ttu-id="c8968-320">Parallell bearbetning av datasektorer</span><span class="sxs-lookup"><span data-stu-id="c8968-320">Parallel processing of data slices</span></span>
<span data-ttu-id="c8968-321">Du kan ange hello startdatum för hello pipeline i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="c8968-321">You can set hello start date for hello pipeline in hello past.</span></span> <span data-ttu-id="c8968-322">När du gör det Data Factory beräknas (tillbaka fyllning) alla datasektorer i hello senaste och automatiskt börjar bearbeta dem.</span><span class="sxs-lookup"><span data-stu-id="c8968-322">When you do so, Data Factory automatically calculates (back fills) all data slices in hello past and begins processing them.</span></span> <span data-ttu-id="c8968-323">Exempel: Om du skapar en pipeline med startdatum 2017-04-01 och hello aktuellt datum är 2017-04-10.</span><span class="sxs-lookup"><span data-stu-id="c8968-323">For example: if you create a pipeline with start date 2017-04-01 and hello current date is 2017-04-10.</span></span> <span data-ttu-id="c8968-324">Om hello takt av hello utflöde dataset är varje dag och sedan Data Factory startar bearbetning av alla hello segment från 2017-04-01 är too2017-04-09 omedelbart eftersom hello startdatum hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="c8968-324">If hello cadence of hello output dataset is daily, then Data Factory starts processing all hello slices from 2017-04-01 too2017-04-09 immediately because hello start date is in hello past.</span></span> <span data-ttu-id="c8968-325">hello segment från 2017-04-10 bearbetas inte ännu eftersom hello värdet style-egenskapen i hello tillgänglighetssektion är EndOfInterval som standard.</span><span class="sxs-lookup"><span data-stu-id="c8968-325">hello slice from 2017-04-10 is not processed yet because hello value of style property in hello availability section is EndOfInterval by default.</span></span> <span data-ttu-id="c8968-326">hello äldsta sektorn bearbetas först som hello standard är värdet för executionPriorityOrder OldestFirst.</span><span class="sxs-lookup"><span data-stu-id="c8968-326">hello oldest slice is processed first as hello default value of executionPriorityOrder is OldestFirst.</span></span> <span data-ttu-id="c8968-327">En beskrivning av hello style-egenskapen, se [dataset tillgänglighet](#dataset-availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c8968-327">For a description of hello style property, see [dataset availability](#dataset-availability) section.</span></span> <span data-ttu-id="c8968-328">En beskrivning av hello executionPriorityOrder avsnittet finns hello [aktivitetsprinciper](#activity-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c8968-328">For a description of hello executionPriorityOrder section, see hello [activity policies](#activity-policies) section.</span></span> 

<span data-ttu-id="c8968-329">Du kan konfigurera fylls i med tillbaka data segment toobe bearbetas parallellt genom att ange hello **samtidighet** egenskap i hello **princip** avsnitt i hello aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="c8968-329">You can configure back-filled data slices toobe processed in parallel by setting hello **concurrency** property in hello **policy** section of hello activity JSON.</span></span> <span data-ttu-id="c8968-330">Den här egenskapen anger hello antalet ParallellAktivitet körningar som kan inträffa på olika segment.</span><span class="sxs-lookup"><span data-stu-id="c8968-330">This property determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="c8968-331">hello standardvärdet för hello concurrency-egenskapen är 1.</span><span class="sxs-lookup"><span data-stu-id="c8968-331">hello default value for hello concurrency property is 1.</span></span> <span data-ttu-id="c8968-332">Därför bearbetas en sektor samtidigt som standard.</span><span class="sxs-lookup"><span data-stu-id="c8968-332">Therefore, one slice is processed at a time by default.</span></span> <span data-ttu-id="c8968-333">hello maxvärdet är 10.</span><span class="sxs-lookup"><span data-stu-id="c8968-333">hello maximum value is 10.</span></span> <span data-ttu-id="c8968-334">När en pipeline måste toogo via en stor mängd tillgänglig data, med ett större värde för samtidighet snabbare hello databearbetning.</span><span class="sxs-lookup"><span data-stu-id="c8968-334">When a pipeline needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> 

## <a name="rerun-a-failed-data-slice"></a><span data-ttu-id="c8968-335">Kör en misslyckad datasektorn</span><span class="sxs-lookup"><span data-stu-id="c8968-335">Rerun a failed data slice</span></span>
<span data-ttu-id="c8968-336">När ett fel uppstår under bearbetning av en datasektorn, hittar reda på varför hello bearbetning av ett segment misslyckats med hjälp av Azure portal blad eller övervaka och hantera appen.</span><span class="sxs-lookup"><span data-stu-id="c8968-336">When an error occurs while processing a data slice, you can find out why hello processing of a slice failed by using Azure portal blades or Monitor and Manage App.</span></span> <span data-ttu-id="c8968-337">Se [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="c8968-337">See [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md) for details.</span></span>

<span data-ttu-id="c8968-338">Överväg att hello följande exempel, som innehåller två aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c8968-338">Consider hello following example, which shows two activities.</span></span> <span data-ttu-id="c8968-339">Activity1 och aktivitet 2.</span><span class="sxs-lookup"><span data-stu-id="c8968-339">Activity1 and Activity 2.</span></span> <span data-ttu-id="c8968-340">Activity1 använder ett segment av Dataset1 och genererar ett segment av Dataset2 som används som indata av Activity2 tooproduce ett segment av hello slutliga Dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-340">Activity1 consumes a slice of Dataset1 and produces a slice of Dataset2, which is consumed as an input by Activity2 tooproduce a slice of hello Final Dataset.</span></span>

![Misslyckade sektorn](./media/data-factory-scheduling-and-execution/failed-slice.png)

<span data-ttu-id="c8968-342">hello diagram visar att utanför tre senaste segment det uppstod ett fel berörda hello 9 10 AM segment för Dataset2.</span><span class="sxs-lookup"><span data-stu-id="c8968-342">hello diagram shows that out of three recent slices, there was a failure producing hello 9-10 AM slice for Dataset2.</span></span> <span data-ttu-id="c8968-343">Data Factory spårar automatiskt beroende för hello tid serien dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-343">Data Factory automatically tracks dependency for hello time series dataset.</span></span> <span data-ttu-id="c8968-344">Därför kan startar den inte hello aktiviteten kör för hello 9 10 AM underordnade sektorn.</span><span class="sxs-lookup"><span data-stu-id="c8968-344">As a result, it does not start hello activity run for hello 9-10 AM downstream slice.</span></span>

<span data-ttu-id="c8968-345">Data Factory övervakning och hantering av verktygen kan du toodrill till hello diagnostikloggar för hello misslyckade sektorn tooeasily hitta hello rot orsaka för hello problemet och åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="c8968-345">Data Factory monitoring and management tools allow you toodrill into hello diagnostic logs for hello failed slice tooeasily find hello root cause for hello issue and fix it.</span></span> <span data-ttu-id="c8968-346">När du har åtgärdat hello problemet, kan du enkelt starta hello aktiviteten kör tooproduce hello misslyckade sektorn.</span><span class="sxs-lookup"><span data-stu-id="c8968-346">After you have fixed hello issue, you can easily start hello activity run tooproduce hello failed slice.</span></span> <span data-ttu-id="c8968-347">Mer information om hur toorerun och förstå tillståndsövergångar för datasektorer, se [övervakning och hantering av pipelines med hjälp av Azure portal blad](data-factory-monitor-manage-pipelines.md) eller [övervakning och hantering av](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="c8968-347">For more information on how toorerun and understand state transitions for data slices, see [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span>

<span data-ttu-id="c8968-348">När du kör hello 9 10 AM segmentera för **Dataset2**, Data Factory startar hello körs för hello 9 10 AM beroende segment på hello slutliga dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-348">After you rerun hello 9-10 AM slice for **Dataset2**, Data Factory starts hello run for hello 9-10 AM dependent slice on hello final dataset.</span></span>

![Kör misslyckade sektorn](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a><span data-ttu-id="c8968-350">Flera aktiviteter i en pipeline</span><span class="sxs-lookup"><span data-stu-id="c8968-350">Multiple activities in a pipeline</span></span>
<span data-ttu-id="c8968-351">Du kan fler än en aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="c8968-351">You can have more than one activity in a pipeline.</span></span> <span data-ttu-id="c8968-352">Om du har flera aktiviteter i en pipeline och hello utdata för en aktivitet inte är indata för en annan aktivitet, kan hello aktiviteter köras parallellt om indata segment för hello aktiviteter är klar.</span><span class="sxs-lookup"><span data-stu-id="c8968-352">If you have multiple activities in a pipeline and hello output of an activity is not an input of another activity, hello activities may run in parallel if input data slices for hello activities are ready.</span></span>

<span data-ttu-id="c8968-353">Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c8968-353">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="c8968-354">hello aktiviteter kan vara i hello samma pipeline eller i olika pipelines.</span><span class="sxs-lookup"><span data-stu-id="c8968-354">hello activities can be in hello same pipeline or in different pipelines.</span></span> <span data-ttu-id="c8968-355">hello andra aktiviteten körs bara när hello först en slutförs.</span><span class="sxs-lookup"><span data-stu-id="c8968-355">hello second activity executes only when hello first one finishes successfully.</span></span>

<span data-ttu-id="c8968-356">Anta till exempel att hello följande om en pipeline har två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="c8968-356">For example, consider hello following case where a pipeline has two activities:</span></span>

1. <span data-ttu-id="c8968-357">Aktiviteten A1 som kräver indata externa datauppsättningen D1 och producerar utdatauppsättningen D2.</span><span class="sxs-lookup"><span data-stu-id="c8968-357">Activity A1 that requires external input dataset D1, and produces output dataset D2.</span></span>
2. <span data-ttu-id="c8968-358">Aktiviteten A2 som kräver indata från dataset D2 och producerar utdatauppsättningen D3.</span><span class="sxs-lookup"><span data-stu-id="c8968-358">Activity A2 that requires input from dataset D2, and produces output dataset D3.</span></span>

<span data-ttu-id="c8968-359">I det här scenariot, aktiviteter A1 och A2 är i hello samma pipeline.</span><span class="sxs-lookup"><span data-stu-id="c8968-359">In this scenario, activities A1 and A2 are in hello same pipeline.</span></span> <span data-ttu-id="c8968-360">hello aktivitet A1 körs när hello externa data är tillgängliga och frekvens för hello schemalagd tillgänglighet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="c8968-360">hello activity A1 runs when hello external data is available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="c8968-361">hello aktivitet A2 körs när hello schemalagda segment från D2 bli tillgänglig och hello schemalagd tillgänglighet frekvens har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="c8968-361">hello activity A2 runs when hello scheduled slices from D2 become available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="c8968-362">Om det finns ett fel i en hello segment i dataset D2, körs inte A2 för den sektorn förrän den blir tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="c8968-362">If there is an error in one of hello slices in dataset D2, A2 does not run for that slice until it becomes available.</span></span>

<span data-ttu-id="c8968-363">Hej diagramvyn med både aktiviteter i hello samma pipeline som ser ut som följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="c8968-363">hello Diagram view with both activities in hello same pipeline would look like hello following diagram:</span></span>

![Länkning aktiviteter i hello pipeline samma](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

<span data-ttu-id="c8968-365">Som tidigare nämnts kan hello aktiviteter vara i olika pipelines.</span><span class="sxs-lookup"><span data-stu-id="c8968-365">As mentioned earlier, hello activities could be in different pipelines.</span></span> <span data-ttu-id="c8968-366">I ett sådant scenario ser hello diagramvyn ut som följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="c8968-366">In such a scenario, hello diagram view would look like hello following diagram:</span></span>

![Länkning aktiviteter i två rörledningar](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

<span data-ttu-id="c8968-368">Se hello [kopiera sekventiellt](#copy-sequentially) avsnitt i hello bilagan ett exempel.</span><span class="sxs-lookup"><span data-stu-id="c8968-368">See hello [copy sequentially](#copy-sequentially) section in hello appendix for an example.</span></span>

## <a name="model-datasets-with-different-frequencies"></a><span data-ttu-id="c8968-369">Modellen datauppsättningar med olika frekvenser</span><span class="sxs-lookup"><span data-stu-id="c8968-369">Model datasets with different frequencies</span></span>
<span data-ttu-id="c8968-370">I prover hello, har hello frekvenser för inkommande och utgående datauppsättningar och hello schema aktivitetsfönstret hello samma.</span><span class="sxs-lookup"><span data-stu-id="c8968-370">In hello samples, hello frequencies for input and output datasets and hello activity schedule window were hello same.</span></span> <span data-ttu-id="c8968-371">Vissa scenarier kräver hello möjlighet tooproduce utdata med en frekvens som skiljer sig hello frekvenser på en eller flera inmatningar.</span><span class="sxs-lookup"><span data-stu-id="c8968-371">Some scenarios require hello ability tooproduce output at a frequency different than hello frequencies of one or more inputs.</span></span> <span data-ttu-id="c8968-372">Data Factory stöder modeling dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="c8968-372">Data Factory supports modeling these scenarios.</span></span>

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a><span data-ttu-id="c8968-373">Exempel 1: Skapa en daglig utdata-rapport för inkommande data som är tillgängliga i timmen</span><span class="sxs-lookup"><span data-stu-id="c8968-373">Sample 1: Produce a daily output report for input data that is available every hour</span></span>
<span data-ttu-id="c8968-374">Föreställ dig ett scenario där du har definierat mätdata från sensorer som är tillgängliga i timmen i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c8968-374">Consider a scenario in which you have input measurement data from sensors available every hour in Azure Blob storage.</span></span> <span data-ttu-id="c8968-375">Du vill tooproduce en daglig sammanställda rapport med statistik som medelvärde, högsta och lägsta för hello dag med [Data Factory hive aktiviteten](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="c8968-375">You want tooproduce a daily aggregate report with statistics such as mean, maximum, and minimum for hello day with [Data Factory hive activity](data-factory-hive-activity.md).</span></span>

<span data-ttu-id="c8968-376">Här visas hur du kan utforma det här scenariot med Data Factory:</span><span class="sxs-lookup"><span data-stu-id="c8968-376">Here is how you can model this scenario with Data Factory:</span></span>

<span data-ttu-id="c8968-377">**Indatauppsättning**</span><span class="sxs-lookup"><span data-stu-id="c8968-377">**Input dataset**</span></span>

<span data-ttu-id="c8968-378">hello ange varje timme filer tas bort i hello mapp för hello angivna dag.</span><span class="sxs-lookup"><span data-stu-id="c8968-378">hello hourly input files are dropped in hello folder for hello given day.</span></span> <span data-ttu-id="c8968-379">Tillgänglighet för indata är inställt på **timme** (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="c8968-379">Availability for input is set at **Hour** (frequency: Hour, interval: 1).</span></span>

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
<span data-ttu-id="c8968-380">**Datamängd för utdata**</span><span class="sxs-lookup"><span data-stu-id="c8968-380">**Output dataset**</span></span>

<span data-ttu-id="c8968-381">En utdatafil skapas varje dag i hello dag mapp.</span><span class="sxs-lookup"><span data-stu-id="c8968-381">One output file is created every day in hello day's folder.</span></span> <span data-ttu-id="c8968-382">Tillgängligheten för utdata är inställt på **dag** (frekvens: dag och intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="c8968-382">Availability of output is set at **Day** (frequency: Day and interval: 1).</span></span>

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

<span data-ttu-id="c8968-383">**Aktiviteten: hive aktivitet i en pipeline**</span><span class="sxs-lookup"><span data-stu-id="c8968-383">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="c8968-384">hello hive-skript tar emot hello lämpliga *DateTime* information som parametrar som använder hello **WindowStart** variabeln som visas i följande fragment hello.</span><span class="sxs-lookup"><span data-stu-id="c8968-384">hello hive script receives hello appropriate *DateTime* information as parameters that use hello **WindowStart** variable as shown in hello following snippet.</span></span> <span data-ttu-id="c8968-385">hello hive-skript använder den här variabeln tooload hello data från hello rätt mapp för hello dag och kör hello aggregering toogenerate hello utdata.</span><span class="sxs-lookup"><span data-stu-id="c8968-385">hello hive script uses this variable tooload hello data from hello correct folder for hello day and run hello aggregation toogenerate hello output.</span></span>

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

<span data-ttu-id="c8968-386">hello visar följande diagram hello scenariot ur en data-beroendet.</span><span class="sxs-lookup"><span data-stu-id="c8968-386">hello following diagram shows hello scenario from a data-dependency point of view.</span></span>

![Beroendet av data](./media/data-factory-scheduling-and-execution/data-dependency.png)

<span data-ttu-id="c8968-388">hello utdata sektorn för varje dag beror på 24 timvis segment från en datamängd som indata.</span><span class="sxs-lookup"><span data-stu-id="c8968-388">hello output slice for every day depends on 24 hourly slices from an input dataset.</span></span> <span data-ttu-id="c8968-389">Data Factory beräknar beroendena automatiskt genom att räkna ut hello indata-segment som faller inom hello samma tidsperioden som hello utdata sektorn toobe genereras.</span><span class="sxs-lookup"><span data-stu-id="c8968-389">Data Factory computes these dependencies automatically by figuring out hello input data slices that fall in hello same time period as hello output slice toobe produced.</span></span> <span data-ttu-id="c8968-390">Om någon av hello 24 inkommande segment inte är tillgänglig väntar Data Factory hello inkommande segmentet toobe redo innan du startar hello daglig aktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="c8968-390">If any of hello 24 input slices is not available, Data Factory waits for hello input slice toobe ready before starting hello daily activity run.</span></span>

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a><span data-ttu-id="c8968-391">Exempel 2: Ange samband med uttryck och Data Factory-funktioner</span><span class="sxs-lookup"><span data-stu-id="c8968-391">Sample 2: Specify dependency with expressions and Data Factory functions</span></span>
<span data-ttu-id="c8968-392">Nu ska vi titta ett annat scenario.</span><span class="sxs-lookup"><span data-stu-id="c8968-392">Let’s consider another scenario.</span></span> <span data-ttu-id="c8968-393">Anta att du har en hive-aktivitet som bearbetar två inkommande datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c8968-393">Suppose you have a hive activity that processes two input datasets.</span></span> <span data-ttu-id="c8968-394">En av dem har nya data varje dag, men en av dem hämtar nya data varje vecka.</span><span class="sxs-lookup"><span data-stu-id="c8968-394">One of them has new data daily, but one of them gets new data every week.</span></span> <span data-ttu-id="c8968-395">Anta att du vill ha toodo en koppling mellan två hello-indata och skapar ett utgående varje dag.</span><span class="sxs-lookup"><span data-stu-id="c8968-395">Suppose you wanted toodo a join across hello two inputs and produce an output every day.</span></span>

<span data-ttu-id="c8968-396">hello enkel metod som Data Factory automatiskt räknat ut hello rätt indata sektorer tooprocess genom att justera toohello utdata data tidsintervallet tidsperiod inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="c8968-396">hello simple approach in which Data Factory automatically figures out hello right input slices tooprocess by aligning toohello output data slice’s time period does not work.</span></span>

<span data-ttu-id="c8968-397">Du måste ange att för varje aktivitet som kör hello Data Factory ska använda föregående vecka datasektorn för hello veckovisa inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-397">You must specify that for every activity run, hello Data Factory should use last week’s data slice for hello weekly input dataset.</span></span> <span data-ttu-id="c8968-398">Du kan använda Azure Data Factory-funktioner som visas i följande fragment tooimplement hello problemet.</span><span class="sxs-lookup"><span data-stu-id="c8968-398">You use Azure Data Factory functions as shown in hello following snippet tooimplement this behavior.</span></span>

<span data-ttu-id="c8968-399">**Input1: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="c8968-399">**Input1: Azure blob**</span></span>

<span data-ttu-id="c8968-400">hello första indata hello Azure blob uppdateras dagligen.</span><span class="sxs-lookup"><span data-stu-id="c8968-400">hello first input is hello Azure blob being updated daily.</span></span>

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

<span data-ttu-id="c8968-401">**Input2: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="c8968-401">**Input2: Azure blob**</span></span>

<span data-ttu-id="c8968-402">Input2 är hello Azure blob uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="c8968-402">Input2 is hello Azure blob being updated weekly.</span></span>

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

<span data-ttu-id="c8968-403">**Utdata: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="c8968-403">**Output: Azure blob**</span></span>

<span data-ttu-id="c8968-404">En utdatafil skapas varje dag i hello mapp för hello dag.</span><span class="sxs-lookup"><span data-stu-id="c8968-404">One output file is created every day in hello folder for hello day.</span></span> <span data-ttu-id="c8968-405">Tillgängligheten för utdata har angetts för**dag** (frekvens: Day, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="c8968-405">Availability of output is set too**day** (frequency: Day, interval: 1).</span></span>

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

<span data-ttu-id="c8968-406">**Aktiviteten: hive aktivitet i en pipeline**</span><span class="sxs-lookup"><span data-stu-id="c8968-406">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="c8968-407">hello hive aktiviteten tar hello två indata och skapar ett utgående segment varje dag.</span><span class="sxs-lookup"><span data-stu-id="c8968-407">hello hive activity takes hello two inputs and produces an output slice every day.</span></span> <span data-ttu-id="c8968-408">Du kan ange varje dag utdata sektorn toodepend på hello föregående vecka inkommande segment för varje vecka indata på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="c8968-408">You can specify every day’s output slice toodepend on hello previous week’s input slice for weekly input as follows.</span></span>

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

<span data-ttu-id="c8968-409">Se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md) en lista över funktioner och systemvariabler som har stöd för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c8968-409">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of functions and system variables that Data Factory supports.</span></span>

## <a name="appendix"></a><span data-ttu-id="c8968-410">Bilaga</span><span class="sxs-lookup"><span data-stu-id="c8968-410">Appendix</span></span>

### <a name="example-copy-sequentially"></a><span data-ttu-id="c8968-411">Exempel: kopiera sekventiellt</span><span class="sxs-lookup"><span data-stu-id="c8968-411">Example: copy sequentially</span></span>
<span data-ttu-id="c8968-412">Det är möjligt toorun flera kopieringsåtgärder efter varandra på ett sätt som sekventiella/sorterade.</span><span class="sxs-lookup"><span data-stu-id="c8968-412">It is possible toorun multiple copy operations one after another in a sequential/ordered manner.</span></span> <span data-ttu-id="c8968-413">Du kan till exempel ha två utdatauppsättningar som kopiera aktiviteter i en pipeline (CopyActivity1 och CopyActivity2) med hello efter inkommande data:</span><span class="sxs-lookup"><span data-stu-id="c8968-413">For example, you might have two copy activities in a pipeline (CopyActivity1 and CopyActivity2) with hello following input data output datasets:</span></span>   

<span data-ttu-id="c8968-414">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="c8968-414">CopyActivity1</span></span>

<span data-ttu-id="c8968-415">Indata: Dataset.</span><span class="sxs-lookup"><span data-stu-id="c8968-415">Input: Dataset.</span></span> <span data-ttu-id="c8968-416">Utdata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="c8968-416">Output: Dataset2.</span></span>

<span data-ttu-id="c8968-417">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="c8968-417">CopyActivity2</span></span>

<span data-ttu-id="c8968-418">Indata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="c8968-418">Input: Dataset2.</span></span>  <span data-ttu-id="c8968-419">Utdata: Dataset3.</span><span class="sxs-lookup"><span data-stu-id="c8968-419">Output: Dataset3.</span></span>

<span data-ttu-id="c8968-420">CopyActivity2 körs bara om hello CopyActivity1 har körts och Dataset2 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="c8968-420">CopyActivity2 would run only if hello CopyActivity1 has run successfully and Dataset2 is available.</span></span>

<span data-ttu-id="c8968-421">Här är hello exempel pipeline-JSON:</span><span class="sxs-lookup"><span data-stu-id="c8968-421">Here is hello sample pipeline JSON:</span></span>

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
                "description": "Copy data from a blob tooanother"
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
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="c8968-422">Observera att hello datamängd för utdata för hello första kopieringsaktiviteten (Dataset2) har angetts i hello exempel som indata för andra hello-aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c8968-422">Notice that in hello example, hello output dataset of hello first copy activity (Dataset2) is specified as input for hello second activity.</span></span> <span data-ttu-id="c8968-423">Därför körs hello andra aktiviteten bara hello datamängd för utdata från hello första aktiviteten är klar.</span><span class="sxs-lookup"><span data-stu-id="c8968-423">Therefore, hello second activity runs only when hello output dataset from hello first activity is ready.</span></span>  

<span data-ttu-id="c8968-424">I exemplet hello CopyActivity2 kan ha en annan inmatning, till exempel Dataset3, men du ange Dataset2 som ett inkommande tooCopyActivity2 så hello aktiviteten inte körs förrän CopyActivity1 har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c8968-424">In hello example, CopyActivity2 can have a different input, such as Dataset3, but you specify Dataset2 as an input tooCopyActivity2, so hello activity does not run until CopyActivity1 finishes.</span></span> <span data-ttu-id="c8968-425">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c8968-425">For example:</span></span>

<span data-ttu-id="c8968-426">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="c8968-426">CopyActivity1</span></span>

<span data-ttu-id="c8968-427">Indata: Dataset1.</span><span class="sxs-lookup"><span data-stu-id="c8968-427">Input: Dataset1.</span></span> <span data-ttu-id="c8968-428">Utdata: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="c8968-428">Output: Dataset2.</span></span>

<span data-ttu-id="c8968-429">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="c8968-429">CopyActivity2</span></span>

<span data-ttu-id="c8968-430">Indata: Dataset3, Dataset2.</span><span class="sxs-lookup"><span data-stu-id="c8968-430">Inputs: Dataset3, Dataset2.</span></span> <span data-ttu-id="c8968-431">Utdata: Dataset4.</span><span class="sxs-lookup"><span data-stu-id="c8968-431">Output: Dataset4.</span></span>

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
                "description": "Copy data from a blob tooanother"
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
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="c8968-432">Observera att två indatauppsättningar har angetts i hello exempelvis för hello andra kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c8968-432">Notice that in hello example, two input datasets are specified for hello second copy activity.</span></span> <span data-ttu-id="c8968-433">När flera inmatningar anges endast hello första inkommande dataset används för att kopiera data, men andra datauppsättningar som används som beroenden.</span><span class="sxs-lookup"><span data-stu-id="c8968-433">When multiple inputs are specified, only hello first input dataset is used for copying data, but other datasets are used as dependencies.</span></span> <span data-ttu-id="c8968-434">CopyActivity2 skulle startas endast efter hello följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="c8968-434">CopyActivity2 would start only after hello following conditions are met:</span></span>

* <span data-ttu-id="c8968-435">CopyActivity1 har slutförts och Dataset2 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="c8968-435">CopyActivity1 has successfully completed and Dataset2 is available.</span></span> <span data-ttu-id="c8968-436">Den här datauppsättningen används inte när du kopierar data tooDataset4.</span><span class="sxs-lookup"><span data-stu-id="c8968-436">This dataset is not used when copying data tooDataset4.</span></span> <span data-ttu-id="c8968-437">Det fungerar bara som ett schemaläggning beroende för CopyActivity2.</span><span class="sxs-lookup"><span data-stu-id="c8968-437">It only acts as a scheduling dependency for CopyActivity2.</span></span>   
* <span data-ttu-id="c8968-438">Dataset3 är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="c8968-438">Dataset3 is available.</span></span> <span data-ttu-id="c8968-439">Den här datauppsättningen representerar hello data som är mål för kopierade toohello.</span><span class="sxs-lookup"><span data-stu-id="c8968-439">This dataset represents hello data that is copied toohello destination.</span></span> 
