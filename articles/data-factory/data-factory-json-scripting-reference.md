---
title: "aaaAzure Data Factory - referens för JSON-skript | Microsoft Docs"
description: "Ger JSON-scheman för Data Factory entiteter."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a><span data-ttu-id="3b691-103">Azure Data Factory - JSON-skript referens</span><span class="sxs-lookup"><span data-stu-id="3b691-103">Azure Data Factory - JSON Scripting Reference</span></span>
<span data-ttu-id="3b691-104">Den här artikeln innehåller exempel och JSON-scheman för att definiera Azure Data Factory-enheter (pipeline, aktivitet, datamängd och länkade tjänsten).</span><span class="sxs-lookup"><span data-stu-id="3b691-104">This article provides JSON schemas and examples for defining Azure Data Factory entities (pipeline, activity, dataset, and linked service).</span></span>  

## <a name="pipeline"></a><span data-ttu-id="3b691-105">Pipeline</span><span class="sxs-lookup"><span data-stu-id="3b691-105">Pipeline</span></span> 
<span data-ttu-id="3b691-106">hello övergripande struktur för en pipeline-definition är följande:</span><span class="sxs-lookup"><span data-stu-id="3b691-106">hello high-level structure for a pipeline definition is as follows:</span></span> 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="3b691-107">Följande tabell beskriver hello egenskaper i hello pipeline JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="3b691-107">Following table describes hello properties within hello pipeline JSON definition:</span></span>

| <span data-ttu-id="3b691-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-108">Property</span></span> | <span data-ttu-id="3b691-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-109">Description</span></span> | <span data-ttu-id="3b691-110">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-110">Required</span></span>
-------- | ----------- | --------
| <span data-ttu-id="3b691-111">namn</span><span class="sxs-lookup"><span data-stu-id="3b691-111">name</span></span> | <span data-ttu-id="3b691-112">Namnet på hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-112">Name of hello pipeline.</span></span> <span data-ttu-id="3b691-113">Ange ett namn som representerar hello-åtgärd som hello aktivitet eller pipeline är konfigurerade toodo</span><span class="sxs-lookup"><span data-stu-id="3b691-113">Specify a name that represents hello action that hello activity or pipeline is configured toodo</span></span><br/><ul><li><span data-ttu-id="3b691-114">Max. antal tecken: 260</span><span class="sxs-lookup"><span data-stu-id="3b691-114">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="3b691-115">Måste börja med en bokstav, en siffra eller ett understreck (_)</span><span class="sxs-lookup"><span data-stu-id="3b691-115">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="3b691-116">Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</span><span class="sxs-lookup"><span data-stu-id="3b691-116">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="3b691-117">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-117">Yes</span></span> |
| <span data-ttu-id="3b691-118">description</span><span class="sxs-lookup"><span data-stu-id="3b691-118">description</span></span> |<span data-ttu-id="3b691-119">Text som beskriver vilka hello aktivitet eller den pipeline som används för</span><span class="sxs-lookup"><span data-stu-id="3b691-119">Text describing what hello activity or pipeline is used for</span></span> | <span data-ttu-id="3b691-120">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-120">No</span></span> |
| <span data-ttu-id="3b691-121">activities</span><span class="sxs-lookup"><span data-stu-id="3b691-121">activities</span></span> | <span data-ttu-id="3b691-122">Innehåller en lista med aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3b691-122">Contains a list of activities.</span></span> | <span data-ttu-id="3b691-123">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-123">Yes</span></span> |
| <span data-ttu-id="3b691-124">start</span><span class="sxs-lookup"><span data-stu-id="3b691-124">start</span></span> |<span data-ttu-id="3b691-125">Starttid för hello pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-125">Start date-time for hello pipeline.</span></span> <span data-ttu-id="3b691-126">Måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="3b691-126">Must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="3b691-127">Till exempel: 2014-10-14T16:32:41.</span><span class="sxs-lookup"><span data-stu-id="3b691-127">For example: 2014-10-14T16:32:41.</span></span> <br/><br/><span data-ttu-id="3b691-128">Det är möjligt toospecify lokal tid, till exempel en EST tid.</span><span class="sxs-lookup"><span data-stu-id="3b691-128">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="3b691-129">Här är ett exempel: `2016-02-27T06:00:00**-05:00`, vilket är 6 AM EST.</span><span class="sxs-lookup"><span data-stu-id="3b691-129">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="3b691-130">hello start- och slutdatum egenskaper anger tillsammans aktiva perioden för hello pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-130">hello start and end properties together specify active period for hello pipeline.</span></span> <span data-ttu-id="3b691-131">Utdata segment som endast produceras med i den här aktiva period.</span><span class="sxs-lookup"><span data-stu-id="3b691-131">Output slices are only produced with in this active period.</span></span> |<span data-ttu-id="3b691-132">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-132">No</span></span><br/><br/><span data-ttu-id="3b691-133">Om du anger ett värde för hello end-egenskapen måste du ange värdet för egenskapen för hello start.</span><span class="sxs-lookup"><span data-stu-id="3b691-133">If you specify a value for hello end property, you must specify value for hello start property.</span></span><br/><br/><span data-ttu-id="3b691-134">hello kan start- och sluttider vara tom toocreate en pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-134">hello start and end times can both be empty toocreate a pipeline.</span></span> <span data-ttu-id="3b691-135">Du måste ange båda värdena tooset en aktiva perioden för hello pipeline toorun.</span><span class="sxs-lookup"><span data-stu-id="3b691-135">You must specify both values tooset an active period for hello pipeline toorun.</span></span> <span data-ttu-id="3b691-136">Om du inte anger start- och sluttider när du skapar en pipeline, kan du ange dem med hjälp av cmdlet hello Set AzureRmDataFactoryPipelineActivePeriod senare.</span><span class="sxs-lookup"><span data-stu-id="3b691-136">If you do not specify start and end times when creating a pipeline, you can set them using hello Set-AzureRmDataFactoryPipelineActivePeriod cmdlet later.</span></span> |
| <span data-ttu-id="3b691-137">End</span><span class="sxs-lookup"><span data-stu-id="3b691-137">end</span></span> |<span data-ttu-id="3b691-138">Datum sluttiden för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-138">End date-time for hello pipeline.</span></span> <span data-ttu-id="3b691-139">Om du måste vara i ISO-format.</span><span class="sxs-lookup"><span data-stu-id="3b691-139">If specified must be in ISO format.</span></span> <span data-ttu-id="3b691-140">Till exempel: 2014-10-14T17:32:41</span><span class="sxs-lookup"><span data-stu-id="3b691-140">For example: 2014-10-14T17:32:41</span></span> <br/><br/><span data-ttu-id="3b691-141">Det är möjligt toospecify lokal tid, till exempel en EST tid.</span><span class="sxs-lookup"><span data-stu-id="3b691-141">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="3b691-142">Här är ett exempel: `2016-02-27T06:00:00**-05:00`, vilket är 6 AM EST.</span><span class="sxs-lookup"><span data-stu-id="3b691-142">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="3b691-143">toorun hello pipeline ange på obestämd tid 09-09-9999 som hello värde för hello end-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-143">toorun hello pipeline indefinitely, specify 9999-09-09 as hello value for hello end property.</span></span> |<span data-ttu-id="3b691-144">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-144">No</span></span> <br/><br/><span data-ttu-id="3b691-145">Om du anger ett värde för egenskapen för hello start, måste du ange värde för hello end-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-145">If you specify a value for hello start property, you must specify value for hello end property.</span></span><br/><br/><span data-ttu-id="3b691-146">Du hittar information för hello **starta** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-146">See notes for hello **start** property.</span></span> |
| <span data-ttu-id="3b691-147">isPaused</span><span class="sxs-lookup"><span data-stu-id="3b691-147">isPaused</span></span> |<span data-ttu-id="3b691-148">Om set tootrue hello pipelinen inte körs.</span><span class="sxs-lookup"><span data-stu-id="3b691-148">If set tootrue hello pipeline does not run.</span></span> <span data-ttu-id="3b691-149">Standardvärde = false.</span><span class="sxs-lookup"><span data-stu-id="3b691-149">Default value = false.</span></span> <span data-ttu-id="3b691-150">Du kan använda den här egenskapen tooenable eller inaktivera.</span><span class="sxs-lookup"><span data-stu-id="3b691-150">You can use this property tooenable or disable.</span></span> |<span data-ttu-id="3b691-151">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-151">No</span></span> |
| <span data-ttu-id="3b691-152">pipelineMode</span><span class="sxs-lookup"><span data-stu-id="3b691-152">pipelineMode</span></span> |<span data-ttu-id="3b691-153">hello-metoden för schemaläggning körs för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-153">hello method for scheduling runs for hello pipeline.</span></span> <span data-ttu-id="3b691-154">Tillåtna värden är: schemalagda (standard), görs.</span><span class="sxs-lookup"><span data-stu-id="3b691-154">Allowed values are: scheduled (default), onetime.</span></span><br/><br/><span data-ttu-id="3b691-155">”Schemalagd' anger att hello pipeline körs vid ett angivet tidsintervall enligt tooits aktiva period (start- och -tid).</span><span class="sxs-lookup"><span data-stu-id="3b691-155">‘Scheduled’ indicates that hello pipeline runs at a specified time interval according tooits active period (start and end time).</span></span> <span data-ttu-id="3b691-156">'Görs ”anger att hello pipelinen körs bara en gång.</span><span class="sxs-lookup"><span data-stu-id="3b691-156">‘Onetime’ indicates that hello pipeline runs only once.</span></span> <span data-ttu-id="3b691-157">Görs pipelines när skapat går inte att ändra/uppdatera för närvarande.</span><span class="sxs-lookup"><span data-stu-id="3b691-157">Onetime pipelines once created cannot be modified/updated currently.</span></span> <span data-ttu-id="3b691-158">Se [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) mer information om hur du görs.</span><span class="sxs-lookup"><span data-stu-id="3b691-158">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details about onetime setting.</span></span> |<span data-ttu-id="3b691-159">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-159">No</span></span> |
| <span data-ttu-id="3b691-160">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="3b691-160">expirationTime</span></span> |<span data-ttu-id="3b691-161">Tidsperiod när den har skapats för vilka hello pipeline är giltig och vara etablerade.</span><span class="sxs-lookup"><span data-stu-id="3b691-161">Duration of time after creation for which hello pipeline is valid and should remain provisioned.</span></span> <span data-ttu-id="3b691-162">Om den inte har någon aktiv, misslyckades, eller väntande körs hello pipeline tas bort automatiskt när når den hello förfallotid.</span><span class="sxs-lookup"><span data-stu-id="3b691-162">If it does not have any active, failed, or pending runs, hello pipeline is deleted automatically once it reaches hello expiration time.</span></span> |<span data-ttu-id="3b691-163">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-163">No</span></span> |


## <a name="activity"></a><span data-ttu-id="3b691-164">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-164">Activity</span></span> 
<span data-ttu-id="3b691-165">hello övergripande struktur för en aktivitet i en pipeline-definition (aktiviteter element) är följande:</span><span class="sxs-lookup"><span data-stu-id="3b691-165">hello high-level structure for an activity within a pipeline definition (activities element) is as follows:</span></span>

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

<span data-ttu-id="3b691-166">Följande tabell Beskriv hello egenskaper i hello aktivitet JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="3b691-166">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="3b691-167">Tagga</span><span class="sxs-lookup"><span data-stu-id="3b691-167">Tag</span></span> | <span data-ttu-id="3b691-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-168">Description</span></span> | <span data-ttu-id="3b691-169">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-170">namn</span><span class="sxs-lookup"><span data-stu-id="3b691-170">name</span></span> |<span data-ttu-id="3b691-171">Namn på hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-171">Name of hello activity.</span></span> <span data-ttu-id="3b691-172">Ange ett namn som representerar hello-åtgärd som hello aktivitet är konfigurerade toodo</span><span class="sxs-lookup"><span data-stu-id="3b691-172">Specify a name that represents hello action that hello activity is configured toodo</span></span><br/><ul><li><span data-ttu-id="3b691-173">Max. antal tecken: 260</span><span class="sxs-lookup"><span data-stu-id="3b691-173">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="3b691-174">Måste börja med en bokstav, en siffra eller ett understreck (_)</span><span class="sxs-lookup"><span data-stu-id="3b691-174">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="3b691-175">Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/”, ”<” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</span><span class="sxs-lookup"><span data-stu-id="3b691-175">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="3b691-176">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-176">Yes</span></span> |
| <span data-ttu-id="3b691-177">description</span><span class="sxs-lookup"><span data-stu-id="3b691-177">description</span></span> |<span data-ttu-id="3b691-178">Text som beskriver vilka hello-aktivitet används för.</span><span class="sxs-lookup"><span data-stu-id="3b691-178">Text describing what hello activity is used for.</span></span> |<span data-ttu-id="3b691-179">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-179">Yes</span></span> |
| <span data-ttu-id="3b691-180">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-180">type</span></span> |<span data-ttu-id="3b691-181">Anger hello hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-181">Specifies hello type of hello activity.</span></span> <span data-ttu-id="3b691-182">Se hello [DATALAGER](#data-stores) och [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) avsnitt för olika typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3b691-182">See hello [DATA STORES](#data-stores) and [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) sections for different types of activities.</span></span> |<span data-ttu-id="3b691-183">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-183">Yes</span></span> |
| <span data-ttu-id="3b691-184">Indata</span><span class="sxs-lookup"><span data-stu-id="3b691-184">inputs</span></span> |<span data-ttu-id="3b691-185">Indatatabeller som används av hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-185">Input tables used by hello activity</span></span><br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |<span data-ttu-id="3b691-186">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-186">Yes</span></span> |
| <span data-ttu-id="3b691-187">utdata</span><span class="sxs-lookup"><span data-stu-id="3b691-187">outputs</span></span> |<span data-ttu-id="3b691-188">Utdata tabeller som används av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-188">Output tables used by hello activity.</span></span><br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |<span data-ttu-id="3b691-189">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-189">Yes</span></span> |
| <span data-ttu-id="3b691-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3b691-190">linkedServiceName</span></span> |<span data-ttu-id="3b691-191">Namnet på hello länkade tjänst som används av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-191">Name of hello linked service used by hello activity.</span></span> <br/><br/><span data-ttu-id="3b691-192">En aktivitet kan kräva att du anger hello länkade tjänst som länkar toohello krävs beräknings-miljö.</span><span class="sxs-lookup"><span data-stu-id="3b691-192">An activity may require that you specify hello linked service that links toohello required compute environment.</span></span> |<span data-ttu-id="3b691-193">Ja för HDInsight aktiviteter, Azure Machine Learning aktiviteter och lagrade Proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-193">Yes for HDInsight activities, Azure Machine Learning activities, and Stored Procedure Activity.</span></span> <br/><br/><span data-ttu-id="3b691-194">Nej för alla andra</span><span class="sxs-lookup"><span data-stu-id="3b691-194">No for all others</span></span> |
| <span data-ttu-id="3b691-195">typeProperties</span><span class="sxs-lookup"><span data-stu-id="3b691-195">typeProperties</span></span> |<span data-ttu-id="3b691-196">Egenskaper i hello typeProperties avsnittet beror på typ av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-196">Properties in hello typeProperties section depend on type of hello activity.</span></span> |<span data-ttu-id="3b691-197">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-197">No</span></span> |
| <span data-ttu-id="3b691-198">policy</span><span class="sxs-lookup"><span data-stu-id="3b691-198">policy</span></span> |<span data-ttu-id="3b691-199">Principer som påverkar hello körning funktionssätt hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-199">Policies that affect hello run-time behavior of hello activity.</span></span> <span data-ttu-id="3b691-200">Om det inte anges används standardprinciper.</span><span class="sxs-lookup"><span data-stu-id="3b691-200">If it is not specified, default policies are used.</span></span> |<span data-ttu-id="3b691-201">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-201">No</span></span> |
| <span data-ttu-id="3b691-202">Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="3b691-202">scheduler</span></span> |<span data-ttu-id="3b691-203">Egenskapen ”scheduler” är används toodefine önskad schemaläggning för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-203">“scheduler” property is used toodefine desired scheduling for hello activity.</span></span> <span data-ttu-id="3b691-204">Dess subegenskaper är hello samma som hello i hello [tillgänglighet egenskap i en dataset](data-factory-create-datasets.md#dataset-availability).</span><span class="sxs-lookup"><span data-stu-id="3b691-204">Its subproperties are hello same as hello ones in hello [availability property in a dataset](data-factory-create-datasets.md#dataset-availability).</span></span> |<span data-ttu-id="3b691-205">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-205">No</span></span> |

### <a name="policies"></a><span data-ttu-id="3b691-206">Principer</span><span class="sxs-lookup"><span data-stu-id="3b691-206">Policies</span></span>
<span data-ttu-id="3b691-207">Principer påverkar hello körning beteendet för en aktivitet, särskilt när hello segment i en tabell som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="3b691-207">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="3b691-208">hello följande tabell innehåller hello information.</span><span class="sxs-lookup"><span data-stu-id="3b691-208">hello following table provides hello details.</span></span>

| <span data-ttu-id="3b691-209">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-209">Property</span></span> | <span data-ttu-id="3b691-210">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-210">Permitted values</span></span> | <span data-ttu-id="3b691-211">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="3b691-211">Default Value</span></span> | <span data-ttu-id="3b691-212">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-212">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-213">Concurrency</span><span class="sxs-lookup"><span data-stu-id="3b691-213">concurrency</span></span> |<span data-ttu-id="3b691-214">Integer</span><span class="sxs-lookup"><span data-stu-id="3b691-214">Integer</span></span> <br/><br/><span data-ttu-id="3b691-215">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="3b691-215">Max value: 10</span></span> |<span data-ttu-id="3b691-216">1</span><span class="sxs-lookup"><span data-stu-id="3b691-216">1</span></span> |<span data-ttu-id="3b691-217">Antal samtidiga körningar av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-217">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="3b691-218">Den avgör hello antalet ParallellAktivitet körningar som kan inträffa på olika segment.</span><span class="sxs-lookup"><span data-stu-id="3b691-218">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="3b691-219">Till exempel om en aktivitet måste toogo via snabbare en stor mängd tillgänglig data, med ett större värde för samtidighet hello databearbetning.</span><span class="sxs-lookup"><span data-stu-id="3b691-219">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="3b691-220">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="3b691-220">executionPriorityOrder</span></span> |<span data-ttu-id="3b691-221">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="3b691-221">NewestFirst</span></span><br/><br/><span data-ttu-id="3b691-222">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="3b691-222">OldestFirst</span></span> |<span data-ttu-id="3b691-223">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="3b691-223">OldestFirst</span></span> |<span data-ttu-id="3b691-224">Anger hello sorteringen av datasektorer som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="3b691-224">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="3b691-225">Till exempel om du har 2 sektorer (en inträffar klockan 4, och en annan kl) och båda finns väntande körning.</span><span class="sxs-lookup"><span data-stu-id="3b691-225">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="3b691-226">Om du ställer in hello executionPriorityOrder toobe NewestFirst bearbetas hello sektorn kl först.</span><span class="sxs-lookup"><span data-stu-id="3b691-226">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="3b691-227">På liknande sätt om du ställer in hello executionPriorityORder toobe OldestFIrst bearbetas sedan hello sektorn klockan 4.</span><span class="sxs-lookup"><span data-stu-id="3b691-227">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="3b691-228">retry</span><span class="sxs-lookup"><span data-stu-id="3b691-228">retry</span></span> |<span data-ttu-id="3b691-229">Integer</span><span class="sxs-lookup"><span data-stu-id="3b691-229">Integer</span></span><br/><br/><span data-ttu-id="3b691-230">Värdet för maximalt antal kan vara 10</span><span class="sxs-lookup"><span data-stu-id="3b691-230">Max value can be 10</span></span> |<span data-ttu-id="3b691-231">0</span><span class="sxs-lookup"><span data-stu-id="3b691-231">0</span></span> |<span data-ttu-id="3b691-232">Antalet försök innan hello databearbetning för hello segment har markerats som ett fel.</span><span class="sxs-lookup"><span data-stu-id="3b691-232">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="3b691-233">Aktivitetskörningen för en datasektorn försöks in toohello angetts antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="3b691-233">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="3b691-234">hello försök görs så snart som möjligt efter hello-fel.</span><span class="sxs-lookup"><span data-stu-id="3b691-234">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="3b691-235">timeout</span><span class="sxs-lookup"><span data-stu-id="3b691-235">timeout</span></span> |<span data-ttu-id="3b691-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-236">TimeSpan</span></span> |<span data-ttu-id="3b691-237">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3b691-237">00:00:00</span></span> |<span data-ttu-id="3b691-238">Tidsgräns för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-238">Timeout for hello activity.</span></span> <span data-ttu-id="3b691-239">Exempel: 00:10:00 (inbegriper timeout 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="3b691-239">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="3b691-240">Om ett värde har angetts eller är 0, är hello timeout oändligt.</span><span class="sxs-lookup"><span data-stu-id="3b691-240">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="3b691-241">Om hello bearbetningstid på ett segment överskrider hello timeout-värde, det avbryts och hello system försöker tooretry hello bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3b691-241">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="3b691-242">hello antal återförsök beror på hello försök egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-242">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="3b691-243">När timeout uppstår status hello tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="3b691-243">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="3b691-244">Fördröjning</span><span class="sxs-lookup"><span data-stu-id="3b691-244">delay</span></span> |<span data-ttu-id="3b691-245">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-245">TimeSpan</span></span> |<span data-ttu-id="3b691-246">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3b691-246">00:00:00</span></span> |<span data-ttu-id="3b691-247">Ange hello fördröjning innan data bearbetning av hello segment startas.</span><span class="sxs-lookup"><span data-stu-id="3b691-247">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="3b691-248">hello körningen av aktiviteten för en datasektorn startas efter att hello fördröjning har förfallit hello förväntades körningstid.</span><span class="sxs-lookup"><span data-stu-id="3b691-248">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="3b691-249">Exempel: 00:10:00 (inbegriper fördröjning på 10 minuter)</span><span class="sxs-lookup"><span data-stu-id="3b691-249">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="3b691-250">longRetry</span><span class="sxs-lookup"><span data-stu-id="3b691-250">longRetry</span></span> |<span data-ttu-id="3b691-251">Integer</span><span class="sxs-lookup"><span data-stu-id="3b691-251">Integer</span></span><br/><br/><span data-ttu-id="3b691-252">Värdet för maximalt antal: 10</span><span class="sxs-lookup"><span data-stu-id="3b691-252">Max value: 10</span></span> |<span data-ttu-id="3b691-253">1</span><span class="sxs-lookup"><span data-stu-id="3b691-253">1</span></span> |<span data-ttu-id="3b691-254">hello antal lång försök försök innan hello sektorn körningen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="3b691-254">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="3b691-255">longRetry försök fördelade av longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="3b691-255">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="3b691-256">Så om du behöver toospecify taget mellan nya försök använda longRetry.</span><span class="sxs-lookup"><span data-stu-id="3b691-256">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="3b691-257">Om både försök och longRetry anges varje longRetry försök omförsök Hej max antal försök används och försök igen * longRetry.</span><span class="sxs-lookup"><span data-stu-id="3b691-257">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="3b691-258">Om till exempel har vi hello följande inställningar i hello aktivitetsprincip:</span><span class="sxs-lookup"><span data-stu-id="3b691-258">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="3b691-259">Försök: 3</span><span class="sxs-lookup"><span data-stu-id="3b691-259">Retry: 3</span></span><br/><span data-ttu-id="3b691-260">longRetry: 2</span><span class="sxs-lookup"><span data-stu-id="3b691-260">longRetry: 2</span></span><br/><span data-ttu-id="3b691-261">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="3b691-261">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="3b691-262">Anta att det finns bara ett segment tooexecute (status väntar) och hello aktivitetskörningen misslyckas varje gång.</span><span class="sxs-lookup"><span data-stu-id="3b691-262">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="3b691-263">Det skulle inledningsvis 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="3b691-263">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="3b691-264">Efter varje försök skulle hello sektorn status vara försök igen.</span><span class="sxs-lookup"><span data-stu-id="3b691-264">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="3b691-265">Efter första 3 försök är över, är hello sektorn status LongRetry.</span><span class="sxs-lookup"><span data-stu-id="3b691-265">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="3b691-266">Efter en timme (det vill säga Longretryinteval's värde), skulle det finnas en annan uppsättning 3 körning på varandra följande försök.</span><span class="sxs-lookup"><span data-stu-id="3b691-266">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="3b691-267">Efter detta hello sektorn status skulle misslyckades och inga fler försök kan inte genomföras.</span><span class="sxs-lookup"><span data-stu-id="3b691-267">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="3b691-268">Därför har övergripande 6 försök gjorts.</span><span class="sxs-lookup"><span data-stu-id="3b691-268">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="3b691-269">Om alla körning lyckas hello sektorn status är klar och inga fler försök görs.</span><span class="sxs-lookup"><span data-stu-id="3b691-269">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="3b691-270">longRetry får användas i situationer där beroende data kommer till icke-deterministiska gånger eller hello hela miljön är flaky under vilken databearbetningen sker.</span><span class="sxs-lookup"><span data-stu-id="3b691-270">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="3b691-271">I sådana fall kan inte göra återförsök efter varandra kan hjälpa och gör det med tiden resulterar i hello tidsintervall önskad utdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-271">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="3b691-272">Liten varning: Ange inte höga värden för longRetry eller longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="3b691-272">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="3b691-273">Normalt en högre värden andra systemfel problem.</span><span class="sxs-lookup"><span data-stu-id="3b691-273">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="3b691-274">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="3b691-274">longRetryInterval</span></span> |<span data-ttu-id="3b691-275">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-275">TimeSpan</span></span> |<span data-ttu-id="3b691-276">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3b691-276">00:00:00</span></span> |<span data-ttu-id="3b691-277">hello fördröjning mellan försök har lång</span><span class="sxs-lookup"><span data-stu-id="3b691-277">hello delay between long retry attempts</span></span> |

### <a name="typeproperties-section"></a><span data-ttu-id="3b691-278">typeProperties avsnitt</span><span class="sxs-lookup"><span data-stu-id="3b691-278">typeProperties section</span></span>
<span data-ttu-id="3b691-279">Hej typeProperties avsnitt är olika för varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-279">hello typeProperties section is different for each activity.</span></span> <span data-ttu-id="3b691-280">Omvandling aktiviteter har precis hello Typegenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b691-280">Transformation activities have just hello type properties.</span></span> <span data-ttu-id="3b691-281">Se [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) i den här artikeln för JSON-exempel som definierar omvandling av aktiviteter i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-281">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span> 

<span data-ttu-id="3b691-282">**Kopieringsaktiviteten** har två avsnitt i hello typeProperties avsnittet: **källa** och **sink**.</span><span class="sxs-lookup"><span data-stu-id="3b691-282">**Copy activity** has two subsections in hello typeProperties section: **source** and **sink**.</span></span> <span data-ttu-id="3b691-283">Se [DATALAGER](#data-stores) avsnitt i den här artikeln för JSON-exempel som visar hur toouse data lagras som en källa och/eller mottagare.</span><span class="sxs-lookup"><span data-stu-id="3b691-283">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span> 

### <a name="sample-copy-pipeline"></a><span data-ttu-id="3b691-284">Exempel på kopieringspipeline</span><span class="sxs-lookup"><span data-stu-id="3b691-284">Sample copy pipeline</span></span>
<span data-ttu-id="3b691-285">I följande exempel pipeline hello, är en aktivitet av typen **kopiera** i hello **aktiviteter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-285">In hello following sample pipeline, there is one activity of type **Copy** in hello **activities** section.</span></span> <span data-ttu-id="3b691-286">I det här exemplet hello [Kopieringsaktiviteten](data-factory-data-movement-activities.md) kopierar data från Azure Blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-286">In this sample, hello [Copy activity](data-factory-data-movement-activities.md) copies data from an Azure Blob storage tooan Azure SQL database.</span></span> 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="3b691-287">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-287">Note hello following points:</span></span>

* <span data-ttu-id="3b691-288">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="3b691-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span>
* <span data-ttu-id="3b691-289">Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="3b691-289">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span>
* <span data-ttu-id="3b691-290">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="3b691-290">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span>

<span data-ttu-id="3b691-291">Se [DATALAGER](#data-stores) avsnitt i den här artikeln för JSON-exempel som visar hur toouse data lagras som en källa och/eller mottagare.</span><span class="sxs-lookup"><span data-stu-id="3b691-291">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span>    

<span data-ttu-id="3b691-292">En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3b691-292">For a complete walkthrough of creating this pipeline, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="sample-transformation-pipeline"></a><span data-ttu-id="3b691-293">Exempel på transfomeringspipeline</span><span class="sxs-lookup"><span data-stu-id="3b691-293">Sample transformation pipeline</span></span>
<span data-ttu-id="3b691-294">I följande exempel pipeline hello, är en aktivitet av typen **HDInsightHive** i hello **aktiviteter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-294">In hello following sample pipeline, there is one activity of type **HDInsightHive** in hello **activities** section.</span></span> <span data-ttu-id="3b691-295">I det här exemplet hello [HDInsight Hive aktiviteten](data-factory-hive-activity.md) omvandlar data från Azure Blob-lagring genom att köra en Hive-skriptfil på ett Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-295">In this sample, hello [HDInsight Hive activity](data-factory-hive-activity.md) transforms data from an Azure Blob storage by running a Hive script file on an Azure HDInsight Hadoop cluster.</span></span> 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
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
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3b691-296">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-296">Note hello following points:</span></span> 

* <span data-ttu-id="3b691-297">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="3b691-297">In hello activities section, there is only one activity whose **type** is set too**HDInsightHive**.</span></span>
* <span data-ttu-id="3b691-298">hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **AzureStorageLinkedService**), och i  **skriptet** mapp i hello behållaren **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="3b691-298">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>
* <span data-ttu-id="3b691-299">Hej **definierar** avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span><span class="sxs-lookup"><span data-stu-id="3b691-299">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span></span>

<span data-ttu-id="3b691-300">Se [DATA TRANSFORMATION aktiviteter](#data-transformation-activities) i den här artikeln för JSON-exempel som definierar omvandling av aktiviteter i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-300">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span>

<span data-ttu-id="3b691-301">En fullständig genomgång av hur du skapar den här pipelinen finns [Självstudier: skapa din första pipeline tooprocess data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="3b691-301">For a complete walkthrough of creating this pipeline, see [Tutorial: Build your first pipeline tooprocess data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="linked-service"></a><span data-ttu-id="3b691-302">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-302">Linked service</span></span>
<span data-ttu-id="3b691-303">hello övergripande struktur för en länkad tjänst-definition är följande:</span><span class="sxs-lookup"><span data-stu-id="3b691-303">hello high-level structure for a linked service definition is as follows:</span></span>

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

<span data-ttu-id="3b691-304">Följande tabell Beskriv hello egenskaper i hello aktivitet JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="3b691-304">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="3b691-305">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-305">Property</span></span> | <span data-ttu-id="3b691-306">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-306">Description</span></span> | <span data-ttu-id="3b691-307">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-307">Required</span></span> |
| -------- | ----------- | -------- | 
| <span data-ttu-id="3b691-308">namn</span><span class="sxs-lookup"><span data-stu-id="3b691-308">name</span></span> | <span data-ttu-id="3b691-309">Namnet på hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-309">Name of hello linked service.</span></span> | <span data-ttu-id="3b691-310">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-310">Yes</span></span> | 
| <span data-ttu-id="3b691-311">Egenskaper - typ</span><span class="sxs-lookup"><span data-stu-id="3b691-311">properties - type</span></span> | <span data-ttu-id="3b691-312">Typ av hello länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-312">Type of hello linked service.</span></span> <span data-ttu-id="3b691-313">Till exempel: Azure Storage, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3b691-313">For example: Azure Storage, Azure SQL Database.</span></span> |
| <span data-ttu-id="3b691-314">typeProperties</span><span class="sxs-lookup"><span data-stu-id="3b691-314">typeProperties</span></span> | <span data-ttu-id="3b691-315">Hej typeProperties avsnittet innehåller element som är olika för varje dataarkiv eller compute-miljö.</span><span class="sxs-lookup"><span data-stu-id="3b691-315">hello typeProperties section has elements that are different for each data store or compute environment.</span></span> <span data-ttu-id="3b691-316">Se [datalager](#datastores) avsnittet för alla hello datalager länkade tjänster och [compute miljöer](#compute-environments) för alla hello compute länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="3b691-316">See [data stores](#datastores) section for all hello data store linked services and [compute environments](#compute-environments) for all hello compute linked services</span></span> |   

## <a name="dataset"></a><span data-ttu-id="3b691-317">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-317">Dataset</span></span> 
<span data-ttu-id="3b691-318">En datamängd i Azure Data Factory definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="3b691-318">A dataset in Azure Data Factory is defined as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="3b691-319">hello följande tabell beskriver egenskaper i hello ovan JSON:</span><span class="sxs-lookup"><span data-stu-id="3b691-319">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="3b691-320">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-320">Property</span></span> | <span data-ttu-id="3b691-321">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-321">Description</span></span> | <span data-ttu-id="3b691-322">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-322">Required</span></span> | <span data-ttu-id="3b691-323">Standard</span><span class="sxs-lookup"><span data-stu-id="3b691-323">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-324">namn</span><span class="sxs-lookup"><span data-stu-id="3b691-324">name</span></span> | <span data-ttu-id="3b691-325">Namnet på hello dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-325">Name of hello dataset.</span></span> <span data-ttu-id="3b691-326">Se [Azure Data Factory - namngivningsregler](data-factory-naming-rules.md) för namngivningsregler.</span><span class="sxs-lookup"><span data-stu-id="3b691-326">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="3b691-327">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-327">Yes</span></span> |<span data-ttu-id="3b691-328">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-328">NA</span></span> |
| <span data-ttu-id="3b691-329">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-329">type</span></span> | <span data-ttu-id="3b691-330">typ av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-330">Type of hello dataset.</span></span> <span data-ttu-id="3b691-331">Ange en hello-typer som stöds av Azure Data Factory (till exempel: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="3b691-331">Specify one of hello types supported by Azure Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <span data-ttu-id="3b691-332">Se [DATALAGER](#data-stores) avsnittet för alla hello datalager och dataset-typer som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-332">See [DATA STORES](#data-stores) section for all hello data stores and dataset types supported by Data Factory.</span></span> | 
| <span data-ttu-id="3b691-333">struktur</span><span class="sxs-lookup"><span data-stu-id="3b691-333">structure</span></span> | <span data-ttu-id="3b691-334">Schemat för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-334">Schema of hello dataset.</span></span> <span data-ttu-id="3b691-335">Den innehåller kolumner, deras typer och så vidare.</span><span class="sxs-lookup"><span data-stu-id="3b691-335">It contains columns, their types, etc.</span></span> | <span data-ttu-id="3b691-336">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-336">No</span></span> |<span data-ttu-id="3b691-337">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-337">NA</span></span> |
| <span data-ttu-id="3b691-338">typeProperties</span><span class="sxs-lookup"><span data-stu-id="3b691-338">typeProperties</span></span> | <span data-ttu-id="3b691-339">Egenskaper för motsvarande toohello valda typen.</span><span class="sxs-lookup"><span data-stu-id="3b691-339">Properties corresponding toohello selected type.</span></span> <span data-ttu-id="3b691-340">Se [DATALAGER](#data-stores) för typer som stöds och deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b691-340">See [DATA STORES](#data-stores) section for supported types and their properties.</span></span> |<span data-ttu-id="3b691-341">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-341">Yes</span></span> |<span data-ttu-id="3b691-342">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-342">NA</span></span> |
| <span data-ttu-id="3b691-343">extern</span><span class="sxs-lookup"><span data-stu-id="3b691-343">external</span></span> | <span data-ttu-id="3b691-344">Boolesk flagga toospecify om en datamängd uttryckligen produceras av en data factory-pipelinen eller inte.</span><span class="sxs-lookup"><span data-stu-id="3b691-344">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> |<span data-ttu-id="3b691-345">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-345">No</span></span> |<span data-ttu-id="3b691-346">FALSKT</span><span class="sxs-lookup"><span data-stu-id="3b691-346">false</span></span> |
| <span data-ttu-id="3b691-347">availability</span><span class="sxs-lookup"><span data-stu-id="3b691-347">availability</span></span> | <span data-ttu-id="3b691-348">Definierar hello bearbetning fönster eller hello segmentering modellen för hello dataset produktion.</span><span class="sxs-lookup"><span data-stu-id="3b691-348">Defines hello processing window or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="3b691-349">Mer information om hello dataset segmentering modellen finns [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-349">For details on hello dataset slicing model, see [Scheduling and Execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="3b691-350">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-350">Yes</span></span> |<span data-ttu-id="3b691-351">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-351">NA</span></span> |
| <span data-ttu-id="3b691-352">policy</span><span class="sxs-lookup"><span data-stu-id="3b691-352">policy</span></span> |<span data-ttu-id="3b691-353">Definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="3b691-353">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="3b691-354">Mer information finns i [Dataset princip](#Policy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-354">For details, see [Dataset Policy](#Policy) section.</span></span> |<span data-ttu-id="3b691-355">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-355">No</span></span> |<span data-ttu-id="3b691-356">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-356">NA</span></span> |

<span data-ttu-id="3b691-357">Varje kolumn i hello **struktur** avsnitt innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3b691-357">Each column in hello **structure** section contains hello following properties:</span></span>

| <span data-ttu-id="3b691-358">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-358">Property</span></span> | <span data-ttu-id="3b691-359">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-359">Description</span></span> | <span data-ttu-id="3b691-360">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-360">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-361">namn</span><span class="sxs-lookup"><span data-stu-id="3b691-361">name</span></span> |<span data-ttu-id="3b691-362">Namnet på hello-kolumn.</span><span class="sxs-lookup"><span data-stu-id="3b691-362">Name of hello column.</span></span> |<span data-ttu-id="3b691-363">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-363">Yes</span></span> |
| <span data-ttu-id="3b691-364">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-364">type</span></span> |<span data-ttu-id="3b691-365">Datatypen för kolumnen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-365">Data type of hello column.</span></span>  |<span data-ttu-id="3b691-366">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-366">No</span></span> |
| <span data-ttu-id="3b691-367">Kultur</span><span class="sxs-lookup"><span data-stu-id="3b691-367">culture</span></span> |<span data-ttu-id="3b691-368">.NET baserat kultur toobe används när typ har angetts och .NET-typ `Datetime` eller `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="3b691-368">.NET based culture toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="3b691-369">Standardvärdet är `en-us`.</span><span class="sxs-lookup"><span data-stu-id="3b691-369">Default is `en-us`.</span></span> |<span data-ttu-id="3b691-370">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-370">No</span></span> |
| <span data-ttu-id="3b691-371">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-371">format</span></span> |<span data-ttu-id="3b691-372">Formatera strängen toobe används när typ har angetts och .NET-typ `Datetime` eller `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="3b691-372">Format string toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="3b691-373">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-373">No</span></span> |

<span data-ttu-id="3b691-374">I följande exempel hello, hello datamängden har tre kolumner `slicetimestamp`, `projectname`, och `pageviews` och de är av typen: String, String och Decimal respektive.</span><span class="sxs-lookup"><span data-stu-id="3b691-374">In hello following example, hello dataset has three columns `slicetimestamp`, `projectname`, and `pageviews` and they are of type: String, String, and Decimal respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="3b691-375">hello följande tabell beskriver egenskaper som kan användas i hello **tillgänglighet** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-375">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="3b691-376">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-376">Property</span></span> | <span data-ttu-id="3b691-377">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-377">Description</span></span> | <span data-ttu-id="3b691-378">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-378">Required</span></span> | <span data-ttu-id="3b691-379">Standard</span><span class="sxs-lookup"><span data-stu-id="3b691-379">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-380">frequency</span><span class="sxs-lookup"><span data-stu-id="3b691-380">frequency</span></span> |<span data-ttu-id="3b691-381">Anger hello tidsenhet för dataset sektorn produktion.</span><span class="sxs-lookup"><span data-stu-id="3b691-381">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="3b691-382"><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad</span><span class="sxs-lookup"><span data-stu-id="3b691-382"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="3b691-383">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-383">Yes</span></span> |<span data-ttu-id="3b691-384">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-384">NA</span></span> |
| <span data-ttu-id="3b691-385">interval</span><span class="sxs-lookup"><span data-stu-id="3b691-385">interval</span></span> |<span data-ttu-id="3b691-386">Anger en multiplikator för frekvens</span><span class="sxs-lookup"><span data-stu-id="3b691-386">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="3b691-387">”X frekvensintervall” avgör hur ofta hello segment skapas.</span><span class="sxs-lookup"><span data-stu-id="3b691-387">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="3b691-388">Om du behöver hello dataset toobe segmenterat timme, ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="3b691-388">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="3b691-389"><b>Obs</b>: Om du anger frekvens som minut, rekommenderar vi att du ställer in hello intervall toono som är mindre än 15</span><span class="sxs-lookup"><span data-stu-id="3b691-389"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="3b691-390">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-390">Yes</span></span> |<span data-ttu-id="3b691-391">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-391">NA</span></span> |
| <span data-ttu-id="3b691-392">format</span><span class="sxs-lookup"><span data-stu-id="3b691-392">style</span></span> |<span data-ttu-id="3b691-393">Anger om hello segment ska produceras hello start/slutet av hello intervall.</span><span class="sxs-lookup"><span data-stu-id="3b691-393">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="3b691-394">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="3b691-394">StartOfInterval</span></span></li><li><span data-ttu-id="3b691-395">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="3b691-395">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="3b691-396">Om frekvensen anges tooMonth och format anges tooEndOfInterval, tillverkas hello segment på hello sista dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="3b691-396">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="3b691-397">Om hello format anges tooStartOfInterval, produceras hello segment på hello första dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="3b691-397">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="3b691-398">Om frekvensen anges tooDay och format anges tooEndOfInterval, tillverkas hello segment i hello senaste timmen på dagen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-398">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="3b691-399">Om frekvensen anges tooHour och format anges tooEndOfInterval, tillverkas hello sektorn hello slutet av hello timme.</span><span class="sxs-lookup"><span data-stu-id="3b691-399">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="3b691-400">För ett segment för PM 1 – 2 PM period, till exempel produceras hello sektorn klockan 2.</span><span class="sxs-lookup"><span data-stu-id="3b691-400">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="3b691-401">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-401">No</span></span> |<span data-ttu-id="3b691-402">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="3b691-402">EndOfInterval</span></span> |
| <span data-ttu-id="3b691-403">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="3b691-403">anchorDateTime</span></span> |<span data-ttu-id="3b691-404">Definierar hello absolut placering i tid som används av Schemaläggaren toocompute dataset sektorn gränser.</span><span class="sxs-lookup"><span data-stu-id="3b691-404">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="3b691-405"><b>Obs</b>: om hello AnchorDateTime har datumdelar som är mer detaljerad än hello frekvens sedan hello mer detaljerade delar ignoreras.</span><span class="sxs-lookup"><span data-stu-id="3b691-405"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="3b691-406">Till exempel, om hello <b>intervall</b> är <b>varje timme</b> (frekvens: timme och intervall: 1) och hello <b>AnchorDateTime</b> innehåller <b>minuter och sekunder</b>sedan hello <b>minuter och sekunder</b> delar av hello AnchorDateTime ignoreras.</span><span class="sxs-lookup"><span data-stu-id="3b691-406">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b> then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="3b691-407">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-407">No</span></span> |<span data-ttu-id="3b691-408">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="3b691-408">01/01/0001</span></span> |
| <span data-ttu-id="3b691-409">förskjutning</span><span class="sxs-lookup"><span data-stu-id="3b691-409">offset</span></span> |<span data-ttu-id="3b691-410">TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat.</span><span class="sxs-lookup"><span data-stu-id="3b691-410">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="3b691-411"><b>Obs</b>: om både anchorDateTime och offset anges hello resultatet är hello kombineras SKIFT.</span><span class="sxs-lookup"><span data-stu-id="3b691-411"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="3b691-412">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-412">No</span></span> |<span data-ttu-id="3b691-413">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-413">NA</span></span> |

<span data-ttu-id="3b691-414">hello efter tillgänglighet avsnittet anger att hello utdatauppsättningen är producerade varje timme (eller) indata dataset finns varje timme:</span><span class="sxs-lookup"><span data-stu-id="3b691-414">hello following availability section specifies that hello output dataset is either produced hourly (or) input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="3b691-415">Hej **princip** avsnitt i datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="3b691-415">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

| <span data-ttu-id="3b691-416">Principnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-416">Policy Name</span></span> | <span data-ttu-id="3b691-417">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-417">Description</span></span> | <span data-ttu-id="3b691-418">Används för</span><span class="sxs-lookup"><span data-stu-id="3b691-418">Applied too</span></span>| <span data-ttu-id="3b691-419">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-419">Required</span></span> | <span data-ttu-id="3b691-420">Standard</span><span class="sxs-lookup"><span data-stu-id="3b691-420">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="3b691-421">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="3b691-421">minimumSizeMB</span></span> |<span data-ttu-id="3b691-422">Verifierar att hello-data i en **Azure blob** uppfyller hello krav på minsta storlek (i megabyte).</span><span class="sxs-lookup"><span data-stu-id="3b691-422">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="3b691-423">Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="3b691-423">Azure Blob</span></span> |<span data-ttu-id="3b691-424">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-424">No</span></span> |<span data-ttu-id="3b691-425">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-425">NA</span></span> |
| <span data-ttu-id="3b691-426">minimumRows</span><span class="sxs-lookup"><span data-stu-id="3b691-426">minimumRows</span></span> |<span data-ttu-id="3b691-427">Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader.</span><span class="sxs-lookup"><span data-stu-id="3b691-427">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="3b691-428">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3b691-428">Azure SQL Database</span></span></li><li><span data-ttu-id="3b691-429">Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="3b691-429">Azure Table</span></span></li></ul> |<span data-ttu-id="3b691-430">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-430">No</span></span> |<span data-ttu-id="3b691-431">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="3b691-431">NA</span></span> |

<span data-ttu-id="3b691-432">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3b691-432">**Example:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="3b691-433">Om ett DataSet-objekt produceras av Azure Data Factory, bör det markeras som **externa**.</span><span class="sxs-lookup"><span data-stu-id="3b691-433">Unless a dataset is being produced by Azure Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="3b691-434">Den här inställningen gäller vanligtvis toohello indata för den första aktiviteten i en pipeline om aktiviteten eller pipeline länkning används.</span><span class="sxs-lookup"><span data-stu-id="3b691-434">This setting generally applies toohello inputs of first activity in a pipeline unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="3b691-435">Namn</span><span class="sxs-lookup"><span data-stu-id="3b691-435">Name</span></span> | <span data-ttu-id="3b691-436">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-436">Description</span></span> | <span data-ttu-id="3b691-437">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-437">Required</span></span> | <span data-ttu-id="3b691-438">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="3b691-438">Default Value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-439">dataDelay</span><span class="sxs-lookup"><span data-stu-id="3b691-439">dataDelay</span></span> |<span data-ttu-id="3b691-440">Tid toodelay hello kontroll av hello tillgängligheten för hello externa data för hello angivna sektorn.</span><span class="sxs-lookup"><span data-stu-id="3b691-440">Time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="3b691-441">Om hello data finns varje timme, hello Kontrollera toosee hello externa data är tillgängliga och hello motsvarande segment kan klar försenas med hjälp av dataDelay.</span><span class="sxs-lookup"><span data-stu-id="3b691-441">For example, if hello data is available hourly, hello check toosee hello external data is available and hello corresponding slice is Ready can be delayed by using dataDelay.</span></span><br/><br/><span data-ttu-id="3b691-442">Gäller endast toohello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="3b691-442">Only applies toohello present time.</span></span>  <span data-ttu-id="3b691-443">Om det är 1:00 PM just nu och det här värdet är 10 minuter, till exempel startas hello validering klockan 13:10.</span><span class="sxs-lookup"><span data-stu-id="3b691-443">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="3b691-444">Den här inställningen påverkar inte segment i hello tidigare (segment med segment sluttid + dataDelay < nu) bearbetas utan fördröjning.</span><span class="sxs-lookup"><span data-stu-id="3b691-444">This setting does not affect slices in hello past (slices with Slice End Time + dataDelay < Now) are processed without any delay.</span></span><br/><br/><span data-ttu-id="3b691-445">Tid som är större än 23:59 timmar måste toospecified med hello `day.hours:minutes:seconds` format.</span><span class="sxs-lookup"><span data-stu-id="3b691-445">Time greater than 23:59 hours need toospecified using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="3b691-446">Till exempel toospecify 24 timmar ska inte använda 24:00:00. Använd i stället 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="3b691-446">For example, toospecify 24 hours, don't use 24:00:00; instead, use 1.00:00:00.</span></span> <span data-ttu-id="3b691-447">Om du använder 24:00:00, behandlas den som 24 dagar (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="3b691-447">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="3b691-448">Ange 1:04:00:00 för 1 dag och 4 timmar.</span><span class="sxs-lookup"><span data-stu-id="3b691-448">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="3b691-449">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-449">No</span></span> |<span data-ttu-id="3b691-450">0</span><span class="sxs-lookup"><span data-stu-id="3b691-450">0</span></span> |
| <span data-ttu-id="3b691-451">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="3b691-451">retryInterval</span></span> |<span data-ttu-id="3b691-452">hello väntetiden mellan en felet och hello nästa försök.</span><span class="sxs-lookup"><span data-stu-id="3b691-452">hello wait time between a failure and hello next retry attempt.</span></span> <span data-ttu-id="3b691-453">Om ett försök misslyckas är hello nästa försök efter retryInterval.</span><span class="sxs-lookup"><span data-stu-id="3b691-453">If a try fails, hello next try is after retryInterval.</span></span> <br/><br/><span data-ttu-id="3b691-454">Om den är 1:00 PM just nu kan börja vi hello första försöket.</span><span class="sxs-lookup"><span data-stu-id="3b691-454">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="3b691-455">Om hello varaktighet toocomplete hello första verifieringen är 1 minut och hello-åtgärden misslyckades, hello nästa försök görs 1:00 + 1 min. (varaktighet) + 1 min. (Återförsöksintervall) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="3b691-455">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1 min (duration) + 1 min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="3b691-456">För segment i hello senaste finns ingen fördröjning.</span><span class="sxs-lookup"><span data-stu-id="3b691-456">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="3b691-457">hello försök sker omedelbart.</span><span class="sxs-lookup"><span data-stu-id="3b691-457">hello retry happens immediately.</span></span> |<span data-ttu-id="3b691-458">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-458">No</span></span> |<span data-ttu-id="3b691-459">00:01:00 (1 minut)</span><span class="sxs-lookup"><span data-stu-id="3b691-459">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="3b691-460">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-460">retryTimeout</span></span> |<span data-ttu-id="3b691-461">hello tidsgräns för varje nytt försök.</span><span class="sxs-lookup"><span data-stu-id="3b691-461">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="3b691-462">Om den här egenskapen anges too10 minuter hello verifiering måste toobe slutföras inom 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="3b691-462">If this property is set too10 minutes, hello validation needs toobe completed within 10 minutes.</span></span> <span data-ttu-id="3b691-463">Om det tar längre tid än 10 minuter tooperform hello validering försök hello gånger ut.</span><span class="sxs-lookup"><span data-stu-id="3b691-463">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="3b691-464">Om alla försök för hello verifiering timeout har hello segment markerats som för lång tid.</span><span class="sxs-lookup"><span data-stu-id="3b691-464">If all attempts for hello validation times out, hello slice is marked as TimedOut.</span></span> |<span data-ttu-id="3b691-465">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-465">No</span></span> |<span data-ttu-id="3b691-466">00:10:00 (10 minuter)</span><span class="sxs-lookup"><span data-stu-id="3b691-466">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="3b691-467">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="3b691-467">maximumRetry</span></span> |<span data-ttu-id="3b691-468">Antal gånger toocheck hello tillgänglighet för hello externa data.</span><span class="sxs-lookup"><span data-stu-id="3b691-468">Number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="3b691-469">hello tillåtna maxvärdet är 10.</span><span class="sxs-lookup"><span data-stu-id="3b691-469">hello allowed maximum value is 10.</span></span> |<span data-ttu-id="3b691-470">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-470">No</span></span> |<span data-ttu-id="3b691-471">3</span><span class="sxs-lookup"><span data-stu-id="3b691-471">3</span></span> |


## <a name="data-stores"></a><span data-ttu-id="3b691-472">DATALAGER</span><span class="sxs-lookup"><span data-stu-id="3b691-472">DATA STORES</span></span>
<span data-ttu-id="3b691-473">Hej [länkade tjänsten](#linked-service) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="3b691-473">hello [Linked service](#linked-service) section provided descriptions for JSON elements that are common tooall types of linked services.</span></span> <span data-ttu-id="3b691-474">Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-474">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="3b691-475">Hej [Dataset](#dataset) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="3b691-475">hello [Dataset](#dataset) section provided descriptions for JSON elements that are common tooall types of datasets.</span></span> <span data-ttu-id="3b691-476">Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-476">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="3b691-477">Hej [aktiviteten](#activity) avsnitt som finns beskrivningar för JSON-element som är vanliga tooall typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3b691-477">hello [Activity](#activity) section provided descriptions for JSON elements that are common tooall types of activities.</span></span> <span data-ttu-id="3b691-478">Det här avsnittet innehåller information om JSON-element som är specifika tooeach datalagret när den används som en källor/mottagare i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-478">This section provides details about JSON elements that are specific tooeach data store when it is used as a source/sink in a copy activity.</span></span>  

<span data-ttu-id="3b691-479">Klicka på länken hello för hello store du är intresserad av toosee hello JSON-scheman för den länkade tjänsten, dataset, och hello källor/mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-479">Click hello link for hello store you are interested in toosee hello JSON schemas for linked service, dataset, and hello source/sink for hello copy activity.</span></span>

| <span data-ttu-id="3b691-480">Kategori</span><span class="sxs-lookup"><span data-stu-id="3b691-480">Category</span></span> | <span data-ttu-id="3b691-481">Datalager</span><span class="sxs-lookup"><span data-stu-id="3b691-481">Data store</span></span> 
|:--- |:--- |
| <span data-ttu-id="3b691-482">**Azure**</span><span class="sxs-lookup"><span data-stu-id="3b691-482">**Azure**</span></span> |[<span data-ttu-id="3b691-483">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3b691-483">Azure Blob storage</span></span>](#azure-blob-storage) |
| &nbsp; |[<span data-ttu-id="3b691-484">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b691-484">Azure Data Lake Store</span></span>](#azure-datalake-store) |
| &nbsp; |[<span data-ttu-id="3b691-485">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3b691-485">Azure Cosmos DB</span></span>](#azure-cosmos-db) |
| &nbsp; |[<span data-ttu-id="3b691-486">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3b691-486">Azure SQL Database</span></span>](#azure-sql-database) |
| &nbsp; |[<span data-ttu-id="3b691-487">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-487">Azure SQL Data Warehouse</span></span>](#azure-sql-data-warehouse) |
| &nbsp; |[<span data-ttu-id="3b691-488">Azure Search</span><span class="sxs-lookup"><span data-stu-id="3b691-488">Azure Search</span></span>](#azure-search) |
| &nbsp; |[<span data-ttu-id="3b691-489">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="3b691-489">Azure Table storage</span></span>](#azure-table-storage) |
| <span data-ttu-id="3b691-490">**Databaser**</span><span class="sxs-lookup"><span data-stu-id="3b691-490">**Databases**</span></span> |[<span data-ttu-id="3b691-491">Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="3b691-491">Amazon Redshift</span></span>](#amazon-redshift) |
| &nbsp; |[<span data-ttu-id="3b691-492">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="3b691-492">IBM DB2</span></span>](#ibm-db2) |
| &nbsp; |[<span data-ttu-id="3b691-493">MySQL</span><span class="sxs-lookup"><span data-stu-id="3b691-493">MySQL</span></span>](#mysql) |
| &nbsp; |[<span data-ttu-id="3b691-494">Oracle</span><span class="sxs-lookup"><span data-stu-id="3b691-494">Oracle</span></span>](#oracle) |
| &nbsp; |[<span data-ttu-id="3b691-495">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3b691-495">PostgreSQL</span></span>](#postgresql) |
| &nbsp; |[<span data-ttu-id="3b691-496">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-496">SAP Business Warehouse</span></span>](#sap-business-warehouse) |
| &nbsp; |[<span data-ttu-id="3b691-497">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3b691-497">SAP HANA</span></span>](#sap-hana) |
| &nbsp; |[<span data-ttu-id="3b691-498">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3b691-498">SQL Server</span></span>](#sql-server) |
| &nbsp; |[<span data-ttu-id="3b691-499">Sybase</span><span class="sxs-lookup"><span data-stu-id="3b691-499">Sybase</span></span>](#sybase) |
| &nbsp; |[<span data-ttu-id="3b691-500">Teradata</span><span class="sxs-lookup"><span data-stu-id="3b691-500">Teradata</span></span>](#teradata) |
| <span data-ttu-id="3b691-501">**NoSQL**</span><span class="sxs-lookup"><span data-stu-id="3b691-501">**NoSQL**</span></span> |[<span data-ttu-id="3b691-502">Cassandra</span><span class="sxs-lookup"><span data-stu-id="3b691-502">Cassandra</span></span>](#cassandra) |
| &nbsp; |[<span data-ttu-id="3b691-503">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3b691-503">MongoDB</span></span>](#mongodb) |
| <span data-ttu-id="3b691-504">**Fil**</span><span class="sxs-lookup"><span data-stu-id="3b691-504">**File**</span></span> |[<span data-ttu-id="3b691-505">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="3b691-505">Amazon S3</span></span>](#amazon-s3) |
| &nbsp; |[<span data-ttu-id="3b691-506">Filsystem</span><span class="sxs-lookup"><span data-stu-id="3b691-506">File System</span></span>](#file-system) |
| &nbsp; |[<span data-ttu-id="3b691-507">FTP</span><span class="sxs-lookup"><span data-stu-id="3b691-507">FTP</span></span>](#ftp) |
| &nbsp; |[<span data-ttu-id="3b691-508">HDFS</span><span class="sxs-lookup"><span data-stu-id="3b691-508">HDFS</span></span>](#hdfs) |
| &nbsp; |[<span data-ttu-id="3b691-509">SFTP</span><span class="sxs-lookup"><span data-stu-id="3b691-509">SFTP</span></span>](#sftp) |
| <span data-ttu-id="3b691-510">**Andra**</span><span class="sxs-lookup"><span data-stu-id="3b691-510">**Others**</span></span> |[<span data-ttu-id="3b691-511">HTTP</span><span class="sxs-lookup"><span data-stu-id="3b691-511">HTTP</span></span>](#http) |
| &nbsp; |[<span data-ttu-id="3b691-512">OData</span><span class="sxs-lookup"><span data-stu-id="3b691-512">OData</span></span>](#odata) |
| &nbsp; |[<span data-ttu-id="3b691-513">ODBC</span><span class="sxs-lookup"><span data-stu-id="3b691-513">ODBC</span></span>](#odbc) |
| &nbsp; |[<span data-ttu-id="3b691-514">Salesforce</span><span class="sxs-lookup"><span data-stu-id="3b691-514">Salesforce</span></span>](#salesforce) |
| &nbsp; |[<span data-ttu-id="3b691-515">Webbtabell</span><span class="sxs-lookup"><span data-stu-id="3b691-515">Web Table</span></span>](#web-table) |

## <a name="azure-blob-storage"></a><span data-ttu-id="3b691-516">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3b691-516">Azure Blob Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-517">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-517">Linked service</span></span>
<span data-ttu-id="3b691-518">Det finns två typer av länkade tjänster: Azure länkade lagringstjänsten och Azure Storage SAS länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-518">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="3b691-519">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-519">Azure Storage Linked Service</span></span>
<span data-ttu-id="3b691-520">toolink din Azure storage-konto tooa data factory med hjälp av hello **kontonyckel**, skapa en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-520">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="3b691-521">toodefine ett Azure Storage länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="3b691-521">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="3b691-522">Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-522">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-523">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-523">Property</span></span> | <span data-ttu-id="3b691-524">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-524">Description</span></span> | <span data-ttu-id="3b691-525">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-525">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-526">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-526">connectionString</span></span> |<span data-ttu-id="3b691-527">Ange nödvändig information tooconnect tooAzure lagring för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-527">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="3b691-528">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-528">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="3b691-529">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-529">Example</span></span>  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="3b691-530">Azure Storage SAS länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="3b691-530">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="3b691-531">hello Azure Storage SAS länkad tjänst kan du toolink ett Azure Storage-konto tooan Azure data factory med hjälp av en delad signatur åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="3b691-531">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="3b691-532">Det ger hello data factory med begränsad/Tidsbundna åtkomst tooall utvalda resurser (blobbehållare) i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="3b691-532">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="3b691-533">toolink din Azure storage-konto tooa data factory med signatur för delad åtkomst, skapa en Azure Storage SAS länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-533">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="3b691-534">toodefine ett Azure Storage SAS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="3b691-534">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="3b691-535">Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-535">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="3b691-536">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-536">Property</span></span> | <span data-ttu-id="3b691-537">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-537">Description</span></span> | <span data-ttu-id="3b691-538">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-538">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-539">sasUri</span><span class="sxs-lookup"><span data-stu-id="3b691-539">sasUri</span></span> |<span data-ttu-id="3b691-540">Ange URI för delad åtkomst-signatur toohello Azure Storage-resurser, till exempel blob, behållare eller tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-540">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="3b691-541">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-541">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="3b691-542">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-542">Example</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="3b691-543">Mer information om dessa länkade tjänster finns [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-543">For more information about these linked services, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-544">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-544">Dataset</span></span>
<span data-ttu-id="3b691-545">toodefine en Azure-blobbdatauppsättning set hello **typen** på hello datamängd för**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="3b691-545">toodefine an Azure Blob dataset, set hello **type** of hello dataset too**AzureBlob**.</span></span> <span data-ttu-id="3b691-546">Ange sedan följande specifika Azure Blob-egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-546">Then, specify hello following Azure Blob specific properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-547">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-547">Property</span></span> | <span data-ttu-id="3b691-548">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-548">Description</span></span> | <span data-ttu-id="3b691-549">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-549">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-550">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-550">folderPath</span></span> |<span data-ttu-id="3b691-551">Sökvägen toohello behållaren och mappen i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="3b691-551">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="3b691-552">Exempel: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="3b691-552">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="3b691-553">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-553">Yes</span></span> |
| <span data-ttu-id="3b691-554">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-554">fileName</span></span> |<span data-ttu-id="3b691-555">Namnet på hello-blob.</span><span class="sxs-lookup"><span data-stu-id="3b691-555">Name of hello blob.</span></span> <span data-ttu-id="3b691-556">Filnamnet är valfria och skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="3b691-556">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="3b691-557">Om du anger ett filnamn, hello aktivitet (inklusive kopia) fungerar på hello specifika Blob.</span><span class="sxs-lookup"><span data-stu-id="3b691-557">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="3b691-558">Om filnamnet har angetts innehåller kopiera alla BLOB i hello folderPath för inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-558">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="3b691-559">Om filnamnet inte anges för en utdatauppsättningen hello namnet på hello genereras skulle vara i hello efter det här formatet: Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3b691-559">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3b691-560">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-560">No</span></span> |
| <span data-ttu-id="3b691-561">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-561">partitionedBy</span></span> |<span data-ttu-id="3b691-562">partitionedBy är en valfri egenskap.</span><span class="sxs-lookup"><span data-stu-id="3b691-562">partitionedBy is an optional property.</span></span> <span data-ttu-id="3b691-563">Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-563">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="3b691-564">Exempelvis kan folderPath parameteriseras för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-564">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-565">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-565">No</span></span> |
| <span data-ttu-id="3b691-566">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-566">format</span></span> | <span data-ttu-id="3b691-567">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-567">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-568">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-568">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-569">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-569">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-570">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-570">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-571">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-571">No</span></span> |
| <span data-ttu-id="3b691-572">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-572">compression</span></span> | <span data-ttu-id="3b691-573">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-573">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-574">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-574">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-575">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-575">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-576">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-576">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-577">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-577">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-578">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-578">Example</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


<span data-ttu-id="3b691-579">Mer information finns i [Azure Blob-koppling](data-factory-azure-blob-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-579">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#dataset-properties) article.</span></span>

### <a name="blobsource-in-copy-activity"></a><span data-ttu-id="3b691-580">BlobSource i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-580">BlobSource in Copy Activity</span></span>
<span data-ttu-id="3b691-581">Om du vill kopiera data från ett Azure Blob Storage, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**BlobSource**, och ange följande egenskaper i hello ** källa ** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-581">If you are copying data from an Azure Blob Storage, set hello **source type** of hello copy activity too**BlobSource**, and specify following properties in hello **source **section:</span></span>

| <span data-ttu-id="3b691-582">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-582">Property</span></span> | <span data-ttu-id="3b691-583">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-583">Description</span></span> | <span data-ttu-id="3b691-584">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-584">Allowed values</span></span> | <span data-ttu-id="3b691-585">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-585">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-586">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-586">recursive</span></span> |<span data-ttu-id="3b691-587">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-587">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-588">SANT (standardvärdet), FALSKT</span><span class="sxs-lookup"><span data-stu-id="3b691-588">True (default value), False</span></span> |<span data-ttu-id="3b691-589">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-589">No</span></span> |

#### <a name="example-blobsource"></a><span data-ttu-id="3b691-590">Exempel: BlobSource **</span><span class="sxs-lookup"><span data-stu-id="3b691-590">Example: BlobSource**</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a><span data-ttu-id="3b691-591">BlobSink i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-591">BlobSink in Copy Activity</span></span>
<span data-ttu-id="3b691-592">Om du kopierar data tooan Azure Blob Storage, ange hello **sink typen** av hello kopieringsaktiviteten för**BlobSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-592">If you are copying data tooan Azure Blob Storage, set hello **sink type** of hello copy activity too**BlobSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-593">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-593">Property</span></span> | <span data-ttu-id="3b691-594">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-594">Description</span></span> | <span data-ttu-id="3b691-595">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-595">Allowed values</span></span> | <span data-ttu-id="3b691-596">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-596">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-597">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3b691-597">copyBehavior</span></span> |<span data-ttu-id="3b691-598">Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem.</span><span class="sxs-lookup"><span data-stu-id="3b691-598">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="3b691-599"><b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-599"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="3b691-600">hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-600">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="3b691-601"><b>FlattenHierarchy</b>: alla filer från källmappen hello finns i hello först för målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-601"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="3b691-602">hello målfilerna har genereras automatiskt namn.</span><span class="sxs-lookup"><span data-stu-id="3b691-602">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="3b691-603"><b>MergeFiles (standard):</b> sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="3b691-603"><b>MergeFiles (default):</b> merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="3b691-604">Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-604">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="3b691-605">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-605">No</span></span> |

#### <a name="example-blobsink"></a><span data-ttu-id="3b691-606">Exempel: BlobSink</span><span class="sxs-lookup"><span data-stu-id="3b691-606">Example: BlobSink</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-607">Mer information finns i [Azure Blob-koppling](data-factory-azure-blob-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-607">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-data-lake-store"></a><span data-ttu-id="3b691-608">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3b691-608">Azure Data Lake Store</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-609">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-609">Linked service</span></span>
<span data-ttu-id="3b691-610">toodefine en länkad Azure Data Lake Store-tjänsten, ange hello typ av hello länkade tjänsten för**AzureDataLakeStore**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-610">toodefine an Azure Data Lake Store linked service, set hello type of hello linked service too**AzureDataLakeStore**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-611">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-611">Property</span></span> | <span data-ttu-id="3b691-612">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-612">Description</span></span> | <span data-ttu-id="3b691-613">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-613">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-614">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-614">type</span></span> | <span data-ttu-id="3b691-615">hello Typegenskapen måste anges till: **AzureDataLakeStore**</span><span class="sxs-lookup"><span data-stu-id="3b691-615">hello type property must be set to: **AzureDataLakeStore**</span></span> | <span data-ttu-id="3b691-616">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-616">Yes</span></span> |
| <span data-ttu-id="3b691-617">dataLakeStoreUri</span><span class="sxs-lookup"><span data-stu-id="3b691-617">dataLakeStoreUri</span></span> | <span data-ttu-id="3b691-618">Ange information om hello Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="3b691-618">Specify information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="3b691-619">Det är i hello följande format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` eller `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="3b691-619">It is in hello following format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="3b691-620">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-620">Yes</span></span> |
| <span data-ttu-id="3b691-621">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="3b691-621">subscriptionId</span></span> | <span data-ttu-id="3b691-622">Azure-prenumeration Id toowhich Data Lake Store tillhör.</span><span class="sxs-lookup"><span data-stu-id="3b691-622">Azure subscription Id toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="3b691-623">Krävs för sink</span><span class="sxs-lookup"><span data-stu-id="3b691-623">Required for sink</span></span> |
| <span data-ttu-id="3b691-624">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="3b691-624">resourceGroupName</span></span> | <span data-ttu-id="3b691-625">Azure-resurs grupp namnet toowhich Data Lake Store tillhör.</span><span class="sxs-lookup"><span data-stu-id="3b691-625">Azure resource group name toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="3b691-626">Krävs för sink</span><span class="sxs-lookup"><span data-stu-id="3b691-626">Required for sink</span></span> |
| <span data-ttu-id="3b691-627">servicePrincipalId</span><span class="sxs-lookup"><span data-stu-id="3b691-627">servicePrincipalId</span></span> | <span data-ttu-id="3b691-628">Ange hello programmets klient-ID.</span><span class="sxs-lookup"><span data-stu-id="3b691-628">Specify hello application's client ID.</span></span> | <span data-ttu-id="3b691-629">Ja (för tjänstens huvudnamn autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-629">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3b691-630">servicePrincipalKey</span><span class="sxs-lookup"><span data-stu-id="3b691-630">servicePrincipalKey</span></span> | <span data-ttu-id="3b691-631">Ange hello programnyckel.</span><span class="sxs-lookup"><span data-stu-id="3b691-631">Specify hello application's key.</span></span> | <span data-ttu-id="3b691-632">Ja (för tjänstens huvudnamn autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-632">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3b691-633">Klient</span><span class="sxs-lookup"><span data-stu-id="3b691-633">tenant</span></span> | <span data-ttu-id="3b691-634">Ange hello klient information (domain name eller klient ID) under där programmet finns.</span><span class="sxs-lookup"><span data-stu-id="3b691-634">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="3b691-635">Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b691-635">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure portal.</span></span> | <span data-ttu-id="3b691-636">Ja (för tjänstens huvudnamn autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-636">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3b691-637">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="3b691-637">authorization</span></span> | <span data-ttu-id="3b691-638">Klicka på **auktorisera** knapp i hello **Data Factory-redigeraren** och ange dina autentiseringsuppgifter som tilldelar hello autogenererade auktorisering URL toothis-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-638">Click **Authorize** button in hello **Data Factory Editor** and enter your credential that assigns hello auto-generated authorization URL toothis property.</span></span> | <span data-ttu-id="3b691-639">Ja (för autentisering av autentiseringsuppgifter för användare)</span><span class="sxs-lookup"><span data-stu-id="3b691-639">Yes (for user credential authentication)</span></span>|
| <span data-ttu-id="3b691-640">Sessions-ID</span><span class="sxs-lookup"><span data-stu-id="3b691-640">sessionId</span></span> | <span data-ttu-id="3b691-641">OAuth sessions-id från hello OAuth-auktorisering session.</span><span class="sxs-lookup"><span data-stu-id="3b691-641">OAuth session id from hello OAuth authorization session.</span></span> <span data-ttu-id="3b691-642">Varje sessions-id är unikt och får endast användas en gång.</span><span class="sxs-lookup"><span data-stu-id="3b691-642">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="3b691-643">Den här inställningen genereras automatiskt när du använder Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3b691-643">This setting is automatically generated when you use Data Factory Editor.</span></span> | <span data-ttu-id="3b691-644">Ja (för autentisering av autentiseringsuppgifter för användare)</span><span class="sxs-lookup"><span data-stu-id="3b691-644">Yes (for user credential authentication)</span></span> |

#### <a name="example-using-service-principal-authentication"></a><span data-ttu-id="3b691-645">Exempel: med service principal autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-645">Example: using service principal authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a><span data-ttu-id="3b691-646">Exempel: med hjälp av användarautentisering för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="3b691-646">Example: using user credential authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

<span data-ttu-id="3b691-647">Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-647">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-648">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-648">Dataset</span></span>
<span data-ttu-id="3b691-649">toodefine ett Azure Data Lake Store-dataset set hello **typen** på hello datamängd för**AzureDataLakeStore**, och ange följande egenskaper i hello hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-649">toodefine an Azure Data Lake Store dataset, set hello **type** of hello dataset too**AzureDataLakeStore**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-650">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-650">Property</span></span> | <span data-ttu-id="3b691-651">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-651">Description</span></span> | <span data-ttu-id="3b691-652">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-652">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-653">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-653">folderPath</span></span> |<span data-ttu-id="3b691-654">Lagra sökvägen toohello behållaren och mappen i hello Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="3b691-654">Path toohello container and folder in hello Azure Data Lake store.</span></span> |<span data-ttu-id="3b691-655">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-655">Yes</span></span> |
| <span data-ttu-id="3b691-656">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-656">fileName</span></span> |<span data-ttu-id="3b691-657">Namnet på hello fil hello Azure Data Lake store.</span><span class="sxs-lookup"><span data-stu-id="3b691-657">Name of hello file in hello Azure Data Lake store.</span></span> <span data-ttu-id="3b691-658">Filnamnet är valfria och skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="3b691-658">fileName is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="3b691-659">Om du anger ett filnamn, fungerar hello aktivitet (inklusive kopia) för specifik hello-fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-659">If you specify a filename, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="3b691-660">Om filnamnet har angetts innehåller kopiera alla filer i hello folderPath för inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-660">When fileName is not specified, Copy includes all files in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="3b691-661">Om filnamnet inte anges för en utdatauppsättningen hello namnet på hello genereras skulle vara i hello efter det här formatet: Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3b691-661">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3b691-662">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-662">No</span></span> |
| <span data-ttu-id="3b691-663">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-663">partitionedBy</span></span> |<span data-ttu-id="3b691-664">partitionedBy är en valfri egenskap.</span><span class="sxs-lookup"><span data-stu-id="3b691-664">partitionedBy is an optional property.</span></span> <span data-ttu-id="3b691-665">Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-665">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="3b691-666">Exempelvis kan folderPath parameteriseras för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-666">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-667">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-667">No</span></span> |
| <span data-ttu-id="3b691-668">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-668">format</span></span> | <span data-ttu-id="3b691-669">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-669">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-670">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-670">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-671">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-671">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-672">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-672">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-673">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-673">No</span></span> |
| <span data-ttu-id="3b691-674">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-674">compression</span></span> | <span data-ttu-id="3b691-675">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-675">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-676">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-676">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-677">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-677">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-678">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-678">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-679">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-679">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-680">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-680">Example</span></span>
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-681">Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-681">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-data-lake-store-source-in-copy-activity"></a><span data-ttu-id="3b691-682">Azure Data Lake Store-källan i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-682">Azure Data Lake Store Source in Copy Activity</span></span>
<span data-ttu-id="3b691-683">Om du vill kopiera data från ett Azure Data Lake Store, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**AzureDataLakeStoreSource**, och ange följande egenskaper i hello **källa**  avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-683">If you are copying data from an Azure Data Lake Store, set hello **source type** of hello copy activity too**AzureDataLakeStoreSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="3b691-684">**AzureDataLakeStoreSource** stöder följande egenskaper hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-684">**AzureDataLakeStoreSource** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="3b691-685">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-685">Property</span></span> | <span data-ttu-id="3b691-686">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-686">Description</span></span> | <span data-ttu-id="3b691-687">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-687">Allowed values</span></span> | <span data-ttu-id="3b691-688">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-688">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-689">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-689">recursive</span></span> |<span data-ttu-id="3b691-690">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-690">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-691">SANT (standardvärdet), FALSKT</span><span class="sxs-lookup"><span data-stu-id="3b691-691">True (default value), False</span></span> |<span data-ttu-id="3b691-692">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-692">No</span></span> |

#### <a name="example-azuredatalakestoresource"></a><span data-ttu-id="3b691-693">Exempel: AzureDataLakeStoreSource</span><span class="sxs-lookup"><span data-stu-id="3b691-693">Example: AzureDataLakeStoreSource</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-694">Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-694">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span>

### <a name="azure-data-lake-store-sink-in-copy-activity"></a><span data-ttu-id="3b691-695">Azure Data Lake Store mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-695">Azure Data Lake Store Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-696">Om du kopierar data tooan Azure Data Lake Store, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureDataLakeStoreSink**, och ange följande egenskaper i hello **sink**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-696">If you are copying data tooan Azure Data Lake Store, set hello **sink type** of hello copy activity too**AzureDataLakeStoreSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-697">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-697">Property</span></span> | <span data-ttu-id="3b691-698">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-698">Description</span></span> | <span data-ttu-id="3b691-699">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-699">Allowed values</span></span> | <span data-ttu-id="3b691-700">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-700">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-701">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3b691-701">copyBehavior</span></span> |<span data-ttu-id="3b691-702">Anger hello kopiera beteende.</span><span class="sxs-lookup"><span data-stu-id="3b691-702">Specifies hello copy behavior.</span></span> |<span data-ttu-id="3b691-703"><b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-703"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="3b691-704">hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-704">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="3b691-705"><b>FlattenHierarchy</b>: alla filer från källmappen hello skapas i hello första nivån i målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-705"><b>FlattenHierarchy</b>: all files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="3b691-706">hello mål filer skapas med namnet genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3b691-706">hello target files are created with auto generated name.</span></span><br/><br/><span data-ttu-id="3b691-707"><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="3b691-707"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="3b691-708">Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-708">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="3b691-709">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-709">No</span></span> |

#### <a name="example-azuredatalakestoresink"></a><span data-ttu-id="3b691-710">Exempel: AzureDataLakeStoreSink</span><span class="sxs-lookup"><span data-stu-id="3b691-710">Example: AzureDataLakeStoreSink</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-711">Mer information finns i [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-711">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-cosmos-db"></a><span data-ttu-id="3b691-712">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3b691-712">Azure Cosmos DB</span></span>  

### <a name="linked-service"></a><span data-ttu-id="3b691-713">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-713">Linked service</span></span>
<span data-ttu-id="3b691-714">toodefine en Azure-Cosmos-DB länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**DocumentDb**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-714">toodefine an Azure Cosmos DB linked service, set hello **type** of hello linked service too**DocumentDb**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-715">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="3b691-715">**Property**</span></span> | <span data-ttu-id="3b691-716">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="3b691-716">**Description**</span></span> | <span data-ttu-id="3b691-717">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="3b691-717">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-718">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-718">connectionString</span></span> |<span data-ttu-id="3b691-719">Ange information som behövs för tooconnect tooAzure Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-719">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="3b691-720">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-720">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-721">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-721">Example</span></span>

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
<span data-ttu-id="3b691-722">Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-722">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-723">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-723">Dataset</span></span>
<span data-ttu-id="3b691-724">toodefine en Azure Cosmos DB datauppsättning set hello **typen** på hello datamängd för**DocumentDbCollection**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-724">toodefine an Azure Cosmos DB dataset, set hello **type** of hello dataset too**DocumentDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-725">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="3b691-725">**Property**</span></span> | <span data-ttu-id="3b691-726">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="3b691-726">**Description**</span></span> | <span data-ttu-id="3b691-727">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="3b691-727">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-728">Samlingsnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-728">collectionName</span></span> |<span data-ttu-id="3b691-729">Namnet på hello Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="3b691-729">Name of hello Azure Cosmos DB collection.</span></span> |<span data-ttu-id="3b691-730">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-730">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-731">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-731">Example</span></span>

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="3b691-732">Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-732">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) article.</span></span>

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a><span data-ttu-id="3b691-733">Azure Cosmos DB samling källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-733">Azure Cosmos DB Collection Source in Copy Activity</span></span>
<span data-ttu-id="3b691-734">Om du vill kopiera data från en Azure-Cosmos-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**DocumentDbCollectionSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-734">If you are copying data from an Azure Cosmos DB, set hello **source type** of hello copy activity too**DocumentDbCollectionSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-735">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="3b691-735">**Property**</span></span> | <span data-ttu-id="3b691-736">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="3b691-736">**Description**</span></span> | <span data-ttu-id="3b691-737">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="3b691-737">**Allowed values**</span></span> | <span data-ttu-id="3b691-738">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="3b691-738">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-739">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-739">query</span></span> |<span data-ttu-id="3b691-740">Ange hello frågan tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-740">Specify hello query tooread data.</span></span> |<span data-ttu-id="3b691-741">Frågesträng stöds av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3b691-741">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="3b691-742">Exempel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="3b691-742">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="3b691-743">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-743">No</span></span> <br/><br/><span data-ttu-id="3b691-744">Om inget anges hello SQL-instruktionen som körs:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="3b691-744">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="3b691-745">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="3b691-745">nestingSeparator</span></span> |<span data-ttu-id="3b691-746">Specialtecken tooindicate som hello dokumentet är kapslad</span><span class="sxs-lookup"><span data-stu-id="3b691-746">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="3b691-747">Valfritt tecken.</span><span class="sxs-lookup"><span data-stu-id="3b691-747">Any character.</span></span> <br/><br/><span data-ttu-id="3b691-748">Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="3b691-748">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="3b691-749">Azure Data Factory aktiverar toodenote användarhierarkin via nestingSeparator, vilket är ””.</span><span class="sxs-lookup"><span data-stu-id="3b691-749">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="3b691-750">i hello exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="3b691-750">in hello above examples.</span></span> <span data-ttu-id="3b691-751">Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition.</span><span class="sxs-lookup"><span data-stu-id="3b691-751">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="3b691-752">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-752">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-753">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-753">Example</span></span>

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a><span data-ttu-id="3b691-754">Azure Cosmos DB samling mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-754">Azure Cosmos DB Collection Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-755">Om du kopierar data tooAzure Cosmos DB ange hello **sink typen** av hello kopieringsaktiviteten för**DocumentDbCollectionSink**, och ange följande egenskaper i hello **sink**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-755">If you are copying data tooAzure Cosmos DB, set hello **sink type** of hello copy activity too**DocumentDbCollectionSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-756">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="3b691-756">**Property**</span></span> | <span data-ttu-id="3b691-757">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="3b691-757">**Description**</span></span> | <span data-ttu-id="3b691-758">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="3b691-758">**Allowed values**</span></span> | <span data-ttu-id="3b691-759">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="3b691-759">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-760">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="3b691-760">nestingSeparator</span></span> |<span data-ttu-id="3b691-761">Det krävs ett specialtecken i hello källa kolumnen namn tooindicate som kapslade dokument.</span><span class="sxs-lookup"><span data-stu-id="3b691-761">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="3b691-762">Till exempel ovan: `Name.First` i hello utdata producerar tabellen hello följande JSON-strukturen i hello Cosmos DB dokument:</span><span class="sxs-lookup"><span data-stu-id="3b691-762">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="3b691-763">”Name”: {</span><span class="sxs-lookup"><span data-stu-id="3b691-763">"Name": {</span></span><br/>    <span data-ttu-id="3b691-764">”Första”: ”John”</span><span class="sxs-lookup"><span data-stu-id="3b691-764">"First": "John"</span></span><br/><span data-ttu-id="3b691-765">},</span><span class="sxs-lookup"><span data-stu-id="3b691-765">},</span></span> |<span data-ttu-id="3b691-766">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="3b691-766">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="3b691-767">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="3b691-767">Default value is `.` (dot).</span></span> |<span data-ttu-id="3b691-768">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="3b691-768">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="3b691-769">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="3b691-769">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="3b691-770">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-770">writeBatchSize</span></span> |<span data-ttu-id="3b691-771">Antalet parallella begäranden tooAzure Cosmos DB toocreate servicedokument.</span><span class="sxs-lookup"><span data-stu-id="3b691-771">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="3b691-772">Du kan finjustera hello prestanda vid kopiering av data till och från Azure Cosmos DB genom att använda den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-772">You can fine-tune hello performance when copying data to/from Azure Cosmos DB by using this property.</span></span> <span data-ttu-id="3b691-773">Du kan förvänta dig bättre prestanda om du ökar writeBatchSize eftersom flera parallella begäranden tooAzure Cosmos DB skickas.</span><span class="sxs-lookup"><span data-stu-id="3b691-773">You can expect a better performance when you increase writeBatchSize because more parallel requests tooAzure Cosmos DB are sent.</span></span> <span data-ttu-id="3b691-774">Men du behöver tooavoid begränsning som kan utlösa hello felmeddelande: ”begär frekvensen är stor”.</span><span class="sxs-lookup"><span data-stu-id="3b691-774">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="3b691-775">Begränsning bestäms av ett antal faktorer, bland annat storlek dokument, antalet villkoren i dokument, indexering princip målsamling osv. Kopieringen, kan du använda en bättre samling (till exempel S3) toohave hello de flesta genomströmning tillgänglig (2 500 begärande enheter per sekund).</span><span class="sxs-lookup"><span data-stu-id="3b691-775">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (for example, S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="3b691-776">Integer</span><span class="sxs-lookup"><span data-stu-id="3b691-776">Integer</span></span> |<span data-ttu-id="3b691-777">Nej (standard: 5)</span><span class="sxs-lookup"><span data-stu-id="3b691-777">No (default: 5)</span></span> |
| <span data-ttu-id="3b691-778">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-778">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-779">Vänta tills hello åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3b691-779">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="3b691-780">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-780">timespan</span></span><br/><br/> <span data-ttu-id="3b691-781">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3b691-781">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3b691-782">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-782">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-783">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-783">Example</span></span>

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

<span data-ttu-id="3b691-784">Mer information finns i [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-784">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="3b691-785">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3b691-785">Azure SQL Database</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-786">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-786">Linked service</span></span>
<span data-ttu-id="3b691-787">toodefine en Azure SQL Database länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDatabase**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-787">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-788">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-788">Property</span></span> | <span data-ttu-id="3b691-789">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-789">Description</span></span> | <span data-ttu-id="3b691-790">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-790">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-791">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-791">connectionString</span></span> |<span data-ttu-id="3b691-792">Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-792">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="3b691-793">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-793">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-794">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-794">Example</span></span>
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3b691-795">Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-795">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-796">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-796">Dataset</span></span>
<span data-ttu-id="3b691-797">toodefine en Azure SQL Database-datauppsättning set hello **typen** på hello datamängd för**AzureSqlTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-797">toodefine an Azure SQL Database dataset, set hello **type** of hello dataset too**AzureSqlTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-798">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-798">Property</span></span> | <span data-ttu-id="3b691-799">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-799">Description</span></span> | <span data-ttu-id="3b691-800">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-800">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-801">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-801">tableName</span></span> |<span data-ttu-id="3b691-802">Namnet på hello tabellen eller vyn i hello Azure SQL Database-instans som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-802">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="3b691-803">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-803">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-804">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-804">Example</span></span>

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3b691-805">Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-805">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="3b691-806">SQL-datakälla i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-806">SQL Source in Copy Activity</span></span>
<span data-ttu-id="3b691-807">Om du vill kopiera data från en Azure SQL Database, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-807">If you are copying data from an Azure SQL Database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-808">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-808">Property</span></span> | <span data-ttu-id="3b691-809">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-809">Description</span></span> | <span data-ttu-id="3b691-810">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-810">Allowed values</span></span> | <span data-ttu-id="3b691-811">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-811">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-812">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3b691-812">sqlReaderQuery</span></span> |<span data-ttu-id="3b691-813">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-813">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-814">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-814">SQL query string.</span></span> <span data-ttu-id="3b691-815">Exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-815">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-816">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-816">No</span></span> |
| <span data-ttu-id="3b691-817">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-817">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3b691-818">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-818">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="3b691-819">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-819">Name of hello stored procedure.</span></span> |<span data-ttu-id="3b691-820">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-820">No</span></span> |
| <span data-ttu-id="3b691-821">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-821">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-822">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-822">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3b691-823">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3b691-823">Name/value pairs.</span></span> <span data-ttu-id="3b691-824">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-824">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3b691-825">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-825">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-826">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-826">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3b691-827">Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-827">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="3b691-828">SQL-mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-828">SQL Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-829">Om du kopierar data tooAzure SQL-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-829">If you are copying data tooAzure SQL Database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-830">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-830">Property</span></span> | <span data-ttu-id="3b691-831">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-831">Description</span></span> | <span data-ttu-id="3b691-832">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-832">Allowed values</span></span> | <span data-ttu-id="3b691-833">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-833">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-834">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-834">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-835">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3b691-835">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="3b691-836">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-836">timespan</span></span><br/><br/> <span data-ttu-id="3b691-837">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3b691-837">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3b691-838">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-838">No</span></span> |
| <span data-ttu-id="3b691-839">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-839">writeBatchSize</span></span> |<span data-ttu-id="3b691-840">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3b691-840">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3b691-841">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3b691-841">Integer (number of rows)</span></span> |<span data-ttu-id="3b691-842">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="3b691-842">No (default: 10000)</span></span> |
| <span data-ttu-id="3b691-843">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3b691-843">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3b691-844">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="3b691-844">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3b691-845">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="3b691-845">A query statement.</span></span> |<span data-ttu-id="3b691-846">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-846">No</span></span> |
| <span data-ttu-id="3b691-847">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3b691-847">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3b691-848">Ange ett kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="3b691-848">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="3b691-849">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="3b691-849">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3b691-850">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-850">No</span></span> |
| <span data-ttu-id="3b691-851">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-851">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="3b691-852">Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-852">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="3b691-853">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-853">Name of hello stored procedure.</span></span> |<span data-ttu-id="3b691-854">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-854">No</span></span> |
| <span data-ttu-id="3b691-855">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-855">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-856">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-856">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3b691-857">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3b691-857">Name/value pairs.</span></span> <span data-ttu-id="3b691-858">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-858">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3b691-859">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-859">No</span></span> |
| <span data-ttu-id="3b691-860">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="3b691-860">sqlWriterTableType</span></span> |<span data-ttu-id="3b691-861">Ange tabellen typen namnet toobe används i hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="3b691-861">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="3b691-862">Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-862">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="3b691-863">Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data.</span><span class="sxs-lookup"><span data-stu-id="3b691-863">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="3b691-864">Ett namn för tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-864">A table type name.</span></span> |<span data-ttu-id="3b691-865">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-865">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-866">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-866">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-867">Mer information finns i [Azure SQL-anslutningen](data-factory-azure-sql-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-867">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="3b691-868">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-868">Azure SQL Data Warehouse</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-869">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-869">Linked service</span></span>
<span data-ttu-id="3b691-870">toodefine ett Azure SQL Data Warehouse länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDW**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-870">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-871">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-871">Property</span></span> | <span data-ttu-id="3b691-872">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-872">Description</span></span> | <span data-ttu-id="3b691-873">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-873">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-874">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-874">connectionString</span></span> |<span data-ttu-id="3b691-875">Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-875">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="3b691-876">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-876">Yes</span></span> |



#### <a name="example"></a><span data-ttu-id="3b691-877">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-877">Example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3b691-878">Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-878">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-879">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-879">Dataset</span></span>
<span data-ttu-id="3b691-880">toodefine en Azure SQL Data Warehouse-datauppsättning set hello **typen** på hello datamängd för**AzureSqlDWTable**, och ange följande egenskaper i hello hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-880">toodefine an Azure SQL Data Warehouse dataset, set hello **type** of hello dataset too**AzureSqlDWTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-881">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-881">Property</span></span> | <span data-ttu-id="3b691-882">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-882">Description</span></span> | <span data-ttu-id="3b691-883">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-883">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-884">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-884">tableName</span></span> |<span data-ttu-id="3b691-885">Namnet på hello tabellen eller vyn i hello Azure SQL Data Warehouse-databas som hello länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-885">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="3b691-886">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-886">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-887">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-887">Example</span></span>

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-888">Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-888">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-dw-source-in-copy-activity"></a><span data-ttu-id="3b691-889">SQL DW-datakälla i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-889">SQL DW Source in Copy Activity</span></span>
<span data-ttu-id="3b691-890">Om du vill kopiera data från Azure SQL Data Warehouse, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlDWSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-890">If you are copying data from Azure SQL Data Warehouse, set hello **source type** of hello copy activity too**SqlDWSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-891">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-891">Property</span></span> | <span data-ttu-id="3b691-892">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-892">Description</span></span> | <span data-ttu-id="3b691-893">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-893">Allowed values</span></span> | <span data-ttu-id="3b691-894">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-894">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-895">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3b691-895">sqlReaderQuery</span></span> |<span data-ttu-id="3b691-896">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-896">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-897">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-897">SQL query string.</span></span> <span data-ttu-id="3b691-898">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-898">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-899">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-899">No</span></span> |
| <span data-ttu-id="3b691-900">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-900">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3b691-901">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-901">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="3b691-902">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-902">Name of hello stored procedure.</span></span> |<span data-ttu-id="3b691-903">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-903">No</span></span> |
| <span data-ttu-id="3b691-904">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-904">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-905">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-905">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3b691-906">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3b691-906">Name/value pairs.</span></span> <span data-ttu-id="3b691-907">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-907">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3b691-908">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-908">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-909">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-909">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-910">Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-910">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-dw-sink-in-copy-activity"></a><span data-ttu-id="3b691-911">SQL DW mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-911">SQL DW Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-912">Om du kopierar data tooAzure SQL Data Warehouse, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlDWSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-912">If you are copying data tooAzure SQL Data Warehouse, set hello **sink type** of hello copy activity too**SqlDWSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-913">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-913">Property</span></span> | <span data-ttu-id="3b691-914">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-914">Description</span></span> | <span data-ttu-id="3b691-915">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-915">Allowed values</span></span> | <span data-ttu-id="3b691-916">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-916">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-917">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3b691-917">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3b691-918">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="3b691-918">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3b691-919">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="3b691-919">A query statement.</span></span> |<span data-ttu-id="3b691-920">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-920">No</span></span> |
| <span data-ttu-id="3b691-921">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="3b691-921">allowPolyBase</span></span> |<span data-ttu-id="3b691-922">Anger om toouse PolyBase (i förekommande fall) i stället för BULKINSERT mekanism.</span><span class="sxs-lookup"><span data-stu-id="3b691-922">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="3b691-923">**Hello rekommenderat sätt tooload data till SQL Data Warehouse är med PolyBase.**</span><span class="sxs-lookup"><span data-stu-id="3b691-923">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> |<span data-ttu-id="3b691-924">True</span><span class="sxs-lookup"><span data-stu-id="3b691-924">True</span></span> <br/><span data-ttu-id="3b691-925">FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-925">False (default)</span></span> |<span data-ttu-id="3b691-926">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-926">No</span></span> |
| <span data-ttu-id="3b691-927">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="3b691-927">polyBaseSettings</span></span> |<span data-ttu-id="3b691-928">En grupp egenskaper som kan anges när hello **allowPolybase** egenskapen för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="3b691-928">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="3b691-929">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-929">No</span></span> |
| <span data-ttu-id="3b691-930">rejectValue</span><span class="sxs-lookup"><span data-stu-id="3b691-930">rejectValue</span></span> |<span data-ttu-id="3b691-931">Anger antalet hello eller procentandelen rader som kan avvisas innan hello frågan inte kunde köras.</span><span class="sxs-lookup"><span data-stu-id="3b691-931">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="3b691-932">Lär dig mer om hello PolyBase avvisa alternativ i hello **argument** avsnitt i [Skapa extern tabell (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3b691-932">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="3b691-933">0 (standard), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="3b691-933">0 (default), 1, 2, …</span></span> |<span data-ttu-id="3b691-934">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-934">No</span></span> |
| <span data-ttu-id="3b691-935">rejectType</span><span class="sxs-lookup"><span data-stu-id="3b691-935">rejectType</span></span> |<span data-ttu-id="3b691-936">Anger om hello rejectValue alternativet har angetts som ett litteralvärde eller ett procentvärde.</span><span class="sxs-lookup"><span data-stu-id="3b691-936">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="3b691-937">Värdet (standard), procent</span><span class="sxs-lookup"><span data-stu-id="3b691-937">Value (default), Percentage</span></span> |<span data-ttu-id="3b691-938">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-938">No</span></span> |
| <span data-ttu-id="3b691-939">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="3b691-939">rejectSampleValue</span></span> |<span data-ttu-id="3b691-940">Anger hello antalet rader tooretrieve innan hello PolyBase beräknar hello procentandelen Avvisade rader.</span><span class="sxs-lookup"><span data-stu-id="3b691-940">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="3b691-941">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="3b691-941">1, 2, …</span></span> |<span data-ttu-id="3b691-942">Ja, om **rejectType** är **procent**</span><span class="sxs-lookup"><span data-stu-id="3b691-942">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="3b691-943">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="3b691-943">useTypeDefault</span></span> |<span data-ttu-id="3b691-944">Anger hur toohandle saknar värden i avgränsade textfiler när PolyBase hämtar data från hello textfil.</span><span class="sxs-lookup"><span data-stu-id="3b691-944">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="3b691-945">Mer information om den här egenskapen från hello argument-avsnittet i [skapa externt FILFORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b691-945">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="3b691-946">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-946">True, False (default)</span></span> |<span data-ttu-id="3b691-947">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-947">No</span></span> |
| <span data-ttu-id="3b691-948">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-948">writeBatchSize</span></span> |<span data-ttu-id="3b691-949">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-949">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="3b691-950">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3b691-950">Integer (number of rows)</span></span> |<span data-ttu-id="3b691-951">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="3b691-951">No (default: 10000)</span></span> |
| <span data-ttu-id="3b691-952">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-952">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-953">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3b691-953">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="3b691-954">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-954">timespan</span></span><br/><br/> <span data-ttu-id="3b691-955">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3b691-955">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3b691-956">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-956">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-957">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-957">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-958">Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-958">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-search"></a><span data-ttu-id="3b691-959">Azure Search</span><span class="sxs-lookup"><span data-stu-id="3b691-959">Azure Search</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-960">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-960">Linked service</span></span>
<span data-ttu-id="3b691-961">toodefine ett Azure Search länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSearch**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-961">toodefine an Azure Search linked service, set hello **type** of hello linked service too**AzureSearch**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-962">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-962">Property</span></span> | <span data-ttu-id="3b691-963">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-963">Description</span></span> | <span data-ttu-id="3b691-964">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-964">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3b691-965">URL: en</span><span class="sxs-lookup"><span data-stu-id="3b691-965">url</span></span> | <span data-ttu-id="3b691-966">URL för hello Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-966">URL for hello Azure Search service.</span></span> | <span data-ttu-id="3b691-967">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-967">Yes</span></span> |
| <span data-ttu-id="3b691-968">key</span><span class="sxs-lookup"><span data-stu-id="3b691-968">key</span></span> | <span data-ttu-id="3b691-969">Admin-nyckel för hello Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-969">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="3b691-970">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-970">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-971">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-971">Example</span></span>

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="3b691-972">Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-972">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-973">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-973">Dataset</span></span>
<span data-ttu-id="3b691-974">toodefine en Azure Search-datauppsättning set hello **typen** på hello datamängd för**AzureSearchIndex**, och ange följande egenskaper i hello hello **typeProperties** avsnitt :</span><span class="sxs-lookup"><span data-stu-id="3b691-974">toodefine an Azure Search dataset, set hello **type** of hello dataset too**AzureSearchIndex**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-975">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-975">Property</span></span> | <span data-ttu-id="3b691-976">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-976">Description</span></span> | <span data-ttu-id="3b691-977">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-977">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3b691-978">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-978">type</span></span> | <span data-ttu-id="3b691-979">hello Typegenskapen måste anges för**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="3b691-979">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="3b691-980">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-980">Yes</span></span> |
| <span data-ttu-id="3b691-981">Indexnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-981">indexName</span></span> | <span data-ttu-id="3b691-982">Namnet på hello Azure Search index.</span><span class="sxs-lookup"><span data-stu-id="3b691-982">Name of hello Azure Search index.</span></span> <span data-ttu-id="3b691-983">Data Factory skapar inte hello index.</span><span class="sxs-lookup"><span data-stu-id="3b691-983">Data Factory does not create hello index.</span></span> <span data-ttu-id="3b691-984">hello index måste finnas i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3b691-984">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="3b691-985">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-985">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-986">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-986">Example</span></span>

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

<span data-ttu-id="3b691-987">Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-987">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) article.</span></span>

### <a name="azure-search-index-sink-in-copy-activity"></a><span data-ttu-id="3b691-988">Azure Search Index mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-988">Azure Search Index Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-989">Om du kopierar data tooan Azure Search index, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureSearchIndexSink**, och ange följande egenskaper i hello **sink**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-989">If you are copying data tooan Azure Search index, set hello **sink type** of hello copy activity too**AzureSearchIndexSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-990">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-990">Property</span></span> | <span data-ttu-id="3b691-991">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-991">Description</span></span> | <span data-ttu-id="3b691-992">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-992">Allowed values</span></span> | <span data-ttu-id="3b691-993">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-993">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="3b691-994">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="3b691-994">WriteBehavior</span></span> | <span data-ttu-id="3b691-995">Anger om toomerge eller ersätta när ett dokument det redan finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="3b691-995">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> | <span data-ttu-id="3b691-996">sammanfoga (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-996">Merge (default)</span></span><br/><span data-ttu-id="3b691-997">Ladda upp</span><span class="sxs-lookup"><span data-stu-id="3b691-997">Upload</span></span>| <span data-ttu-id="3b691-998">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-998">No</span></span> |
| <span data-ttu-id="3b691-999">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-999">WriteBatchSize</span></span> | <span data-ttu-id="3b691-1000">Överför data till hello Azure Search index när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3b691-1000">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> | <span data-ttu-id="3b691-1001">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="3b691-1001">1 too1,000.</span></span> <span data-ttu-id="3b691-1002">Standardvärdet är 1000.</span><span class="sxs-lookup"><span data-stu-id="3b691-1002">Default value is 1000.</span></span> | <span data-ttu-id="3b691-1003">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1003">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1004">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1004">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1005">Mer information finns i [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1005">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-table-storage"></a><span data-ttu-id="3b691-1006">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="3b691-1006">Azure Table Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1007">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1007">Linked service</span></span>
<span data-ttu-id="3b691-1008">Det finns två typer av länkade tjänster: Azure länkade lagringstjänsten och Azure Storage SAS länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-1008">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="3b691-1009">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1009">Azure Storage Linked Service</span></span>
<span data-ttu-id="3b691-1010">toolink din Azure storage-konto tooa data factory med hjälp av hello **kontonyckel**, skapa en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-1010">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="3b691-1011">toodefine ett Azure Storage länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1011">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="3b691-1012">Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1012">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1013">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1013">Property</span></span> | <span data-ttu-id="3b691-1014">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1014">Description</span></span> | <span data-ttu-id="3b691-1015">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1015">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-1016">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-1016">type</span></span> |<span data-ttu-id="3b691-1017">hello Typegenskapen måste anges till: **AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="3b691-1017">hello type property must be set to: **AzureStorage**</span></span> |<span data-ttu-id="3b691-1018">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1018">Yes</span></span> |
| <span data-ttu-id="3b691-1019">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-1019">connectionString</span></span> |<span data-ttu-id="3b691-1020">Ange nödvändig information tooconnect tooAzure lagring för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-1020">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="3b691-1021">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1021">Yes</span></span> |

<span data-ttu-id="3b691-1022">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3b691-1022">**Example:**</span></span>  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="3b691-1023">Azure Storage SAS länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="3b691-1023">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="3b691-1024">hello Azure Storage SAS länkad tjänst kan du toolink ett Azure Storage-konto tooan Azure data factory med hjälp av en delad signatur åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="3b691-1024">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="3b691-1025">Det ger hello data factory med begränsad/Tidsbundna åtkomst tooall utvalda resurser (blobbehållare) i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="3b691-1025">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="3b691-1026">toolink din Azure storage-konto tooa data factory med signatur för delad åtkomst, skapa en Azure Storage SAS länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-1026">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="3b691-1027">toodefine ett Azure Storage SAS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1027">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="3b691-1028">Du kan sedan ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1028">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="3b691-1029">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1029">Property</span></span> | <span data-ttu-id="3b691-1030">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1030">Description</span></span> | <span data-ttu-id="3b691-1031">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1031">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-1032">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-1032">type</span></span> |<span data-ttu-id="3b691-1033">hello Typegenskapen måste anges till: **AzureStorageSas**</span><span class="sxs-lookup"><span data-stu-id="3b691-1033">hello type property must be set to: **AzureStorageSas**</span></span> |<span data-ttu-id="3b691-1034">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1034">Yes</span></span> |
| <span data-ttu-id="3b691-1035">sasUri</span><span class="sxs-lookup"><span data-stu-id="3b691-1035">sasUri</span></span> |<span data-ttu-id="3b691-1036">Ange URI för delad åtkomst-signatur toohello Azure Storage-resurser, till exempel blob, behållare eller tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1036">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="3b691-1037">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1037">Yes</span></span> |

<span data-ttu-id="3b691-1038">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="3b691-1038">**Example:**</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="3b691-1039">Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1039">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1040">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1040">Dataset</span></span>
<span data-ttu-id="3b691-1041">toodefine en Azure Table-datauppsättning set hello **typen** på hello datamängd för**AzureTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1041">toodefine an Azure Table dataset, set hello **type** of hello dataset too**AzureTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1042">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1042">Property</span></span> | <span data-ttu-id="3b691-1043">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1043">Description</span></span> | <span data-ttu-id="3b691-1044">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1044">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1045">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1045">tableName</span></span> |<span data-ttu-id="3b691-1046">Namnet på hello tabell i hello Azure Table-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1046">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="3b691-1047">Ja.</span><span class="sxs-lookup"><span data-stu-id="3b691-1047">Yes.</span></span> <span data-ttu-id="3b691-1048">När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="3b691-1048">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="3b691-1049">Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="3b691-1049">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1050">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1050">Example</span></span>

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1051">Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1051">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-table-source-in-copy-activity"></a><span data-ttu-id="3b691-1052">Azure Tabellkälla i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1052">Azure Table Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1053">Om du vill kopiera data från Azure Table Storage, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**AzureTableSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1053">If you are copying data from Azure Table Storage, set hello **source type** of hello copy activity too**AzureTableSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1054">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1054">Property</span></span> | <span data-ttu-id="3b691-1055">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1055">Description</span></span> | <span data-ttu-id="3b691-1056">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1056">Allowed values</span></span> | <span data-ttu-id="3b691-1057">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1057">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1058">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="3b691-1058">azureTableSourceQuery</span></span> |<span data-ttu-id="3b691-1059">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1059">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1060">Azure-tabellen frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1060">Azure table query string.</span></span> <span data-ttu-id="3b691-1061">Se exemplen i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1061">See examples in hello next section.</span></span> |<span data-ttu-id="3b691-1062">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-1062">No.</span></span> <span data-ttu-id="3b691-1063">När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="3b691-1063">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="3b691-1064">Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="3b691-1064">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="3b691-1065">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="3b691-1065">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="3b691-1066">Ange om swallow hello undantag av tabellen inte finns.</span><span class="sxs-lookup"><span data-stu-id="3b691-1066">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="3b691-1067">SANT</span><span class="sxs-lookup"><span data-stu-id="3b691-1067">TRUE</span></span><br/><span data-ttu-id="3b691-1068">FALSKT</span><span class="sxs-lookup"><span data-stu-id="3b691-1068">FALSE</span></span> |<span data-ttu-id="3b691-1069">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1069">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1070">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1070">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1071">Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1071">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

### <a name="azure-table-sink-in-copy-activity"></a><span data-ttu-id="3b691-1072">Azure-tabellen mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1072">Azure Table Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-1073">Om du kopierar data tooAzure Table Storage, ange hello **sink typen** av hello kopieringsaktiviteten för**AzureTableSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1073">If you are copying data tooAzure Table Storage, set hello **sink type** of hello copy activity too**AzureTableSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-1074">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1074">Property</span></span> | <span data-ttu-id="3b691-1075">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1075">Description</span></span> | <span data-ttu-id="3b691-1076">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1076">Allowed values</span></span> | <span data-ttu-id="3b691-1077">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1077">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1078">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="3b691-1078">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="3b691-1079">Standard partitionsnyckelvärde som kan användas av hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="3b691-1079">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="3b691-1080">Ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="3b691-1080">A string value.</span></span> |<span data-ttu-id="3b691-1081">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1081">No</span></span> |
| <span data-ttu-id="3b691-1082">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="3b691-1082">azureTablePartitionKeyName</span></span> |<span data-ttu-id="3b691-1083">Ange namnet på hello kolumn vars värden används som partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1083">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="3b691-1084">Om inget anges används AzureTableDefaultPartitionKeyValue som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1084">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="3b691-1085">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1085">A column name.</span></span> |<span data-ttu-id="3b691-1086">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1086">No</span></span> |
| <span data-ttu-id="3b691-1087">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="3b691-1087">azureTableRowKeyName</span></span> |<span data-ttu-id="3b691-1088">Ange namnet på hello kolumn vars kolumnvärdena används som radnyckel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1088">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="3b691-1089">Om inget annat anges, kan du använda ett GUID för varje rad.</span><span class="sxs-lookup"><span data-stu-id="3b691-1089">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="3b691-1090">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1090">A column name.</span></span> |<span data-ttu-id="3b691-1091">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1091">No</span></span> |
| <span data-ttu-id="3b691-1092">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="3b691-1092">azureTableInsertType</span></span> |<span data-ttu-id="3b691-1093">hello läge tooinsert data till Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1093">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="3b691-1094">Den här egenskapen anger om befintliga rader i hello utdatatabell med matchande partition och radnycklar få sina värden bytas ut eller samman.</span><span class="sxs-lookup"><span data-stu-id="3b691-1094">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="3b691-1095">toolearn om hur dessa inställningar (dokument och Ersätt) fungerar, se [Insert- eller Merge-entiteten](https://msdn.microsoft.com/library/azure/hh452241.aspx) och [infoga eller ersätta entiteten](https://msdn.microsoft.com/library/azure/hh452242.aspx) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1095">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="3b691-1096">Den här inställningen gäller vid hello raden nivån, inte hello tabell nivån, varken alternativet tar bort rader i hello utdatatabell som inte finns i hello indata.</span><span class="sxs-lookup"><span data-stu-id="3b691-1096">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="3b691-1097">sammanfoga (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-1097">merge (default)</span></span><br/><span data-ttu-id="3b691-1098">Ersätt</span><span class="sxs-lookup"><span data-stu-id="3b691-1098">replace</span></span> |<span data-ttu-id="3b691-1099">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1099">No</span></span> |
| <span data-ttu-id="3b691-1100">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-1100">writeBatchSize</span></span> |<span data-ttu-id="3b691-1101">Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1101">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="3b691-1102">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3b691-1102">Integer (number of rows)</span></span> |<span data-ttu-id="3b691-1103">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="3b691-1103">No (default: 10000)</span></span> |
| <span data-ttu-id="3b691-1104">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-1104">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-1105">Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn</span><span class="sxs-lookup"><span data-stu-id="3b691-1105">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="3b691-1106">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-1106">timespan</span></span><br/><br/><span data-ttu-id="3b691-1107">Exempel ”: 00: 20:00” (20 minuter)</span><span class="sxs-lookup"><span data-stu-id="3b691-1107">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="3b691-1108">Nej (standard toostorage klienten standardtimeout-värdet 90 sek)</span><span class="sxs-lookup"><span data-stu-id="3b691-1108">No (Default toostorage client default timeout value 90 sec)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1109">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1109">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3b691-1110">Mer information om dessa länkade tjänster finns [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1110">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="amazon-redshift"></a><span data-ttu-id="3b691-1111">Amazon RedShift</span><span class="sxs-lookup"><span data-stu-id="3b691-1111">Amazon RedShift</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1112">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1112">Linked service</span></span>
<span data-ttu-id="3b691-1113">toodefine en Amazon Redshift länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AmazonRedshift**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1113">toodefine an Amazon Redshift linked service, set hello **type** of hello linked service too**AmazonRedshift**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1114">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1114">Property</span></span> | <span data-ttu-id="3b691-1115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1115">Description</span></span> | <span data-ttu-id="3b691-1116">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1116">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1117">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1117">server</span></span> |<span data-ttu-id="3b691-1118">IP-adressen eller värdnamnet namnet på hello Amazon Redshift server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1118">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="3b691-1119">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1119">Yes</span></span> |
| <span data-ttu-id="3b691-1120">port</span><span class="sxs-lookup"><span data-stu-id="3b691-1120">port</span></span> |<span data-ttu-id="3b691-1121">hello antal hello TCP-port som hello Amazon Redshift server använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1121">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="3b691-1122">Nej, standardvärde: 5439</span><span class="sxs-lookup"><span data-stu-id="3b691-1122">No, default value: 5439</span></span> |
| <span data-ttu-id="3b691-1123">Databasen</span><span class="sxs-lookup"><span data-stu-id="3b691-1123">database</span></span> |<span data-ttu-id="3b691-1124">Namnet på hello Amazon Redshift databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1124">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="3b691-1125">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1125">Yes</span></span> |
| <span data-ttu-id="3b691-1126">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1126">username</span></span> |<span data-ttu-id="3b691-1127">Namnet på användaren som har åtkomst till toohello databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1127">Name of user who has access toohello database.</span></span> |<span data-ttu-id="3b691-1128">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1128">Yes</span></span> |
| <span data-ttu-id="3b691-1129">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1129">password</span></span> |<span data-ttu-id="3b691-1130">Lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1130">Password for hello user account.</span></span> |<span data-ttu-id="3b691-1131">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1131">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1132">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1132">Example</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

<span data-ttu-id="3b691-1133">Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1133">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1134">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1134">Dataset</span></span>
<span data-ttu-id="3b691-1135">toodefine en Amazon Redshift datauppsättning set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1135">toodefine an Amazon Redshift dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1136">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1136">Property</span></span> | <span data-ttu-id="3b691-1137">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1137">Description</span></span> | <span data-ttu-id="3b691-1138">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1138">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1139">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1139">tableName</span></span> |<span data-ttu-id="3b691-1140">Namnet på hello tabell i hello Amazon Redshift databas som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1140">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="3b691-1141">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1141">No (if **query** of **RelationalSource** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-1142">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1142">Example</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3b691-1143">Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1143">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1144">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1144">Relational Source in Copy Activity</span></span> 
<span data-ttu-id="3b691-1145">Om du vill kopiera data från Amazon Redshift, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1145">If you are copying data from Amazon Redshift, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1146">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1146">Property</span></span> | <span data-ttu-id="3b691-1147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1147">Description</span></span> | <span data-ttu-id="3b691-1148">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1148">Allowed values</span></span> | <span data-ttu-id="3b691-1149">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1149">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1150">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1150">query</span></span> |<span data-ttu-id="3b691-1151">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1151">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1152">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1152">SQL query string.</span></span> <span data-ttu-id="3b691-1153">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1153">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-1154">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1154">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1155">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1155">Example</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="3b691-1156">Mer information finns i [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1156">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) article.</span></span>

## <a name="ibm-db2"></a><span data-ttu-id="3b691-1157">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="3b691-1157">IBM DB2</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1158">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1158">Linked service</span></span>
<span data-ttu-id="3b691-1159">toodefine en IBM DB2 länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesDB2**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1159">toodefine an IBM DB2 linked service, set hello **type** of hello linked service too**OnPremisesDB2**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1160">Property</span></span> | <span data-ttu-id="3b691-1161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1161">Description</span></span> | <span data-ttu-id="3b691-1162">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1163">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1163">server</span></span> |<span data-ttu-id="3b691-1164">Namnet på hello DB2-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1164">Name of hello DB2 server.</span></span> |<span data-ttu-id="3b691-1165">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1165">Yes</span></span> |
| <span data-ttu-id="3b691-1166">Databasen</span><span class="sxs-lookup"><span data-stu-id="3b691-1166">database</span></span> |<span data-ttu-id="3b691-1167">Namnet på hello DB2-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1167">Name of hello DB2 database.</span></span> |<span data-ttu-id="3b691-1168">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1168">Yes</span></span> |
| <span data-ttu-id="3b691-1169">Schemat</span><span class="sxs-lookup"><span data-stu-id="3b691-1169">schema</span></span> |<span data-ttu-id="3b691-1170">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1170">Name of hello schema in hello database.</span></span> <span data-ttu-id="3b691-1171">hello schemanamnet är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="3b691-1171">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="3b691-1172">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1172">No</span></span> |
| <span data-ttu-id="3b691-1173">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1173">authenticationType</span></span> |<span data-ttu-id="3b691-1174">Typ av autentisering används tooconnect toohello DB2-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1174">Type of authentication used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="3b691-1175">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-1175">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3b691-1176">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1176">Yes</span></span> |
| <span data-ttu-id="3b691-1177">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1177">username</span></span> |<span data-ttu-id="3b691-1178">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1178">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3b691-1179">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1179">No</span></span> |
| <span data-ttu-id="3b691-1180">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1180">password</span></span> |<span data-ttu-id="3b691-1181">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1181">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-1182">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1182">No</span></span> |
| <span data-ttu-id="3b691-1183">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1183">gatewayName</span></span> |<span data-ttu-id="3b691-1184">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala DB2-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1184">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="3b691-1185">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1185">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1186">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1186">Example</span></span>
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="3b691-1187">Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1187">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1188">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1188">Dataset</span></span>
<span data-ttu-id="3b691-1189">toodefine en DB2 dataset, ange hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1189">toodefine a DB2 dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="3b691-1190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1190">Property</span></span> | <span data-ttu-id="3b691-1191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1191">Description</span></span> | <span data-ttu-id="3b691-1192">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1192">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1193">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1193">tableName</span></span> |<span data-ttu-id="3b691-1194">Namnet på hello tabell i hello DB2-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1194">Name of hello table in hello DB2 Database instance that linked service refers to.</span></span> <span data-ttu-id="3b691-1195">hello tableName är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="3b691-1195">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="3b691-1196">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1196">No (if **query** of **RelationalSource** is specified)</span></span> 

#### <a name="example"></a><span data-ttu-id="3b691-1197">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1197">Example</span></span>
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1198">Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1198">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1199">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1199">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1200">Om du vill kopiera data från IBM DB2, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1200">If you are copying data from IBM DB2, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1201">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1201">Property</span></span> | <span data-ttu-id="3b691-1202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1202">Description</span></span> | <span data-ttu-id="3b691-1203">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1203">Allowed values</span></span> | <span data-ttu-id="3b691-1204">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1204">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1205">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1205">query</span></span> |<span data-ttu-id="3b691-1206">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1206">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1207">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1207">SQL query string.</span></span> <span data-ttu-id="3b691-1208">Till exempel: `"query": "select * from "MySchema"."MyTable""`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1208">For example: `"query": "select * from "MySchema"."MyTable""`.</span></span> |<span data-ttu-id="3b691-1209">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1209">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1210">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1210">Example</span></span>
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="3b691-1211">Mer information finns i [IBM DB2-koppling](#data-factory-onprem-db2-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1211">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) article.</span></span>

## <a name="mysql"></a><span data-ttu-id="3b691-1212">MySQL</span><span class="sxs-lookup"><span data-stu-id="3b691-1212">MySQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1213">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1213">Linked service</span></span>
<span data-ttu-id="3b691-1214">toodefine en MySQL länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesMySql**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1214">toodefine a MySQL linked service, set hello **type** of hello linked service too**OnPremisesMySql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1215">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1215">Property</span></span> | <span data-ttu-id="3b691-1216">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1216">Description</span></span> | <span data-ttu-id="3b691-1217">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1217">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1218">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1218">server</span></span> |<span data-ttu-id="3b691-1219">Namnet på hello MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1219">Name of hello MySQL server.</span></span> |<span data-ttu-id="3b691-1220">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1220">Yes</span></span> |
| <span data-ttu-id="3b691-1221">Databasen</span><span class="sxs-lookup"><span data-stu-id="3b691-1221">database</span></span> |<span data-ttu-id="3b691-1222">Namnet på hello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1222">Name of hello MySQL database.</span></span> |<span data-ttu-id="3b691-1223">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1223">Yes</span></span> |
| <span data-ttu-id="3b691-1224">Schemat</span><span class="sxs-lookup"><span data-stu-id="3b691-1224">schema</span></span> |<span data-ttu-id="3b691-1225">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1225">Name of hello schema in hello database.</span></span> |<span data-ttu-id="3b691-1226">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1226">No</span></span> |
| <span data-ttu-id="3b691-1227">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1227">authenticationType</span></span> |<span data-ttu-id="3b691-1228">Typ av autentisering används tooconnect toohello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1228">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="3b691-1229">Möjliga värden är: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1229">Possible values are: `Basic`.</span></span> |<span data-ttu-id="3b691-1230">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1230">Yes</span></span> |
| <span data-ttu-id="3b691-1231">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1231">username</span></span> |<span data-ttu-id="3b691-1232">Ange användarens namn tooconnect toohello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1232">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="3b691-1233">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1233">Yes</span></span> |
| <span data-ttu-id="3b691-1234">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1234">password</span></span> |<span data-ttu-id="3b691-1235">Ange lösenord för hello-användarkonto som du angav.</span><span class="sxs-lookup"><span data-stu-id="3b691-1235">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="3b691-1236">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1236">Yes</span></span> |
| <span data-ttu-id="3b691-1237">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1237">gatewayName</span></span> |<span data-ttu-id="3b691-1238">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1238">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="3b691-1239">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1239">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1240">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1240">Example</span></span>

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

<span data-ttu-id="3b691-1241">Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1241">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1242">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1242">Dataset</span></span>
<span data-ttu-id="3b691-1243">toodefine en MySQL-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1243">toodefine a MySQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1244">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1244">Property</span></span> | <span data-ttu-id="3b691-1245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1245">Description</span></span> | <span data-ttu-id="3b691-1246">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1247">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1247">tableName</span></span> |<span data-ttu-id="3b691-1248">Namnet på hello tabell i hello MySQL-databasinstans som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1248">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="3b691-1249">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1249">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1250">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1250">Example</span></span>

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3b691-1251">Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1251">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1252">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1252">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1253">Om du vill kopiera data från en MySQL-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1253">If you are copying data from a MySQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1254">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1254">Property</span></span> | <span data-ttu-id="3b691-1255">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1255">Description</span></span> | <span data-ttu-id="3b691-1256">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1256">Allowed values</span></span> | <span data-ttu-id="3b691-1257">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1257">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1258">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1258">query</span></span> |<span data-ttu-id="3b691-1259">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1259">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1260">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1260">SQL query string.</span></span> <span data-ttu-id="3b691-1261">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1261">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-1262">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1262">No (if **tableName** of **dataset** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-1263">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1263">Example</span></span>
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1264">Mer information finns i [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1264">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="oracle"></a><span data-ttu-id="3b691-1265">Oracle</span><span class="sxs-lookup"><span data-stu-id="3b691-1265">Oracle</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-1266">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1266">Linked service</span></span>
<span data-ttu-id="3b691-1267">toodefine en Oracle länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesOracle**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1267">toodefine an Oracle linked service, set hello **type** of hello linked service too**OnPremisesOracle**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1268">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1268">Property</span></span> | <span data-ttu-id="3b691-1269">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1269">Description</span></span> | <span data-ttu-id="3b691-1270">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1270">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1271">driverType</span><span class="sxs-lookup"><span data-stu-id="3b691-1271">driverType</span></span> | <span data-ttu-id="3b691-1272">Ange vilka drivrutinen toouse toocopy data från / tooOracle databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1272">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="3b691-1273">Tillåtna värden är **Microsoft** eller **ODP** (standard).</span><span class="sxs-lookup"><span data-stu-id="3b691-1273">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="3b691-1274">Se [stöds version och vilka installationsalternativ](#supported-versions-and-installation) avsnitt för mer information.</span><span class="sxs-lookup"><span data-stu-id="3b691-1274">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="3b691-1275">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1275">No</span></span> |
| <span data-ttu-id="3b691-1276">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-1276">connectionString</span></span> | <span data-ttu-id="3b691-1277">Ange nödvändig information tooconnect toohello Oracle-databasinstans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-1277">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="3b691-1278">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1278">Yes</span></span> |
| <span data-ttu-id="3b691-1279">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1279">gatewayName</span></span> | <span data-ttu-id="3b691-1280">Namnet på hello gateway som som används tooconnect toohello lokal Oracle-server</span><span class="sxs-lookup"><span data-stu-id="3b691-1280">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="3b691-1281">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1281">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1282">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1282">Example</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3b691-1283">Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1283">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1284">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1284">Dataset</span></span>
<span data-ttu-id="3b691-1285">toodefine en Oracle-datauppsättning set hello **typen** på hello datamängd för**OracleTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1285">toodefine an Oracle dataset, set hello **type** of hello dataset too**OracleTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1286">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1286">Property</span></span> | <span data-ttu-id="3b691-1287">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1287">Description</span></span> | <span data-ttu-id="3b691-1288">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1288">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1289">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1289">tableName</span></span> |<span data-ttu-id="3b691-1290">Namnet på hello tabell i hello Oracle-databas som hello länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1290">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="3b691-1291">Nej (om **oracleReaderQuery** av **OracleSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1291">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1292">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1292">Example</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3b691-1293">Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1293">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) article.</span></span>

### <a name="oracle-source-in-copy-activity"></a><span data-ttu-id="3b691-1294">Oracle-källan i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1294">Oracle Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1295">Om du vill kopiera data från en Oracle-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**OracleSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1295">If you are copying data from an Oracle database, set hello **source type** of hello copy activity too**OracleSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1296">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1296">Property</span></span> | <span data-ttu-id="3b691-1297">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1297">Description</span></span> | <span data-ttu-id="3b691-1298">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1298">Allowed values</span></span> | <span data-ttu-id="3b691-1299">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1299">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1300">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3b691-1300">oracleReaderQuery</span></span> |<span data-ttu-id="3b691-1301">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1301">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1302">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1302">SQL query string.</span></span> <span data-ttu-id="3b691-1303">Exempel: `select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="3b691-1303">For example: `select * from MyTable`</span></span> <br/><br/><span data-ttu-id="3b691-1304">Om inget anges hello SQL-instruktionen som körs:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="3b691-1304">If not specified, hello SQL statement that is executed: `select * from MyTable`</span></span> |<span data-ttu-id="3b691-1305">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1305">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1306">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1306">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1307">Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1307">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

### <a name="oracle-sink-in-copy-activity"></a><span data-ttu-id="3b691-1308">Oracle mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1308">Oracle Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-1309">Om du kopierar data tooam Oracle-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**OracleSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1309">If you are copying data tooam Oracle database, set hello **sink type** of hello copy activity too**OracleSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-1310">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1310">Property</span></span> | <span data-ttu-id="3b691-1311">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1311">Description</span></span> | <span data-ttu-id="3b691-1312">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1312">Allowed values</span></span> | <span data-ttu-id="3b691-1313">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1313">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1314">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-1314">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-1315">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3b691-1315">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="3b691-1316">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-1316">timespan</span></span><br/><br/> <span data-ttu-id="3b691-1317">Exempel: 00:30:00 (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3b691-1317">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="3b691-1318">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1318">No</span></span> |
| <span data-ttu-id="3b691-1319">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-1319">writeBatchSize</span></span> |<span data-ttu-id="3b691-1320">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3b691-1320">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3b691-1321">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3b691-1321">Integer (number of rows)</span></span> |<span data-ttu-id="3b691-1322">Nej (standard: 100)</span><span class="sxs-lookup"><span data-stu-id="3b691-1322">No (default: 100)</span></span> |
| <span data-ttu-id="3b691-1323">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3b691-1323">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3b691-1324">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="3b691-1324">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3b691-1325">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="3b691-1325">A query statement.</span></span> |<span data-ttu-id="3b691-1326">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1326">No</span></span> |
| <span data-ttu-id="3b691-1327">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3b691-1327">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3b691-1328">Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1328">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="3b691-1329">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="3b691-1329">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3b691-1330">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1330">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1331">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1331">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3b691-1332">Mer information finns i [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1332">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

## <a name="postgresql"></a><span data-ttu-id="3b691-1333">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3b691-1333">PostgreSQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1334">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1334">Linked service</span></span>
<span data-ttu-id="3b691-1335">toodefine en PostgreSQL länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesPostgreSql**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1335">toodefine a PostgreSQL linked service, set hello **type** of hello linked service too**OnPremisesPostgreSql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1336">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1336">Property</span></span> | <span data-ttu-id="3b691-1337">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1337">Description</span></span> | <span data-ttu-id="3b691-1338">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1338">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1339">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1339">server</span></span> |<span data-ttu-id="3b691-1340">Namnet på hello PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1340">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="3b691-1341">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1341">Yes</span></span> |
| <span data-ttu-id="3b691-1342">Databasen</span><span class="sxs-lookup"><span data-stu-id="3b691-1342">database</span></span> |<span data-ttu-id="3b691-1343">Namnet på hello PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1343">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="3b691-1344">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1344">Yes</span></span> |
| <span data-ttu-id="3b691-1345">Schemat</span><span class="sxs-lookup"><span data-stu-id="3b691-1345">schema</span></span> |<span data-ttu-id="3b691-1346">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1346">Name of hello schema in hello database.</span></span> <span data-ttu-id="3b691-1347">hello schemanamnet är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="3b691-1347">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="3b691-1348">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1348">No</span></span> |
| <span data-ttu-id="3b691-1349">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1349">authenticationType</span></span> |<span data-ttu-id="3b691-1350">Typ av autentisering används tooconnect toohello PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1350">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="3b691-1351">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-1351">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3b691-1352">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1352">Yes</span></span> |
| <span data-ttu-id="3b691-1353">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1353">username</span></span> |<span data-ttu-id="3b691-1354">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1354">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3b691-1355">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1355">No</span></span> |
| <span data-ttu-id="3b691-1356">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1356">password</span></span> |<span data-ttu-id="3b691-1357">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1357">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-1358">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1358">No</span></span> |
| <span data-ttu-id="3b691-1359">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1359">gatewayName</span></span> |<span data-ttu-id="3b691-1360">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1360">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="3b691-1361">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1361">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1362">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1362">Example</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="3b691-1363">Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1363">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1364">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1364">Dataset</span></span>
<span data-ttu-id="3b691-1365">toodefine en PostgreSQL-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1365">toodefine a PostgreSQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1366">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1366">Property</span></span> | <span data-ttu-id="3b691-1367">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1367">Description</span></span> | <span data-ttu-id="3b691-1368">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1368">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1369">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1369">tableName</span></span> |<span data-ttu-id="3b691-1370">Namnet på hello tabell i hello PostgreSQL-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1370">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="3b691-1371">hello tableName är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="3b691-1371">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="3b691-1372">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1372">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1373">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1373">Example</span></span>
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3b691-1374">Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1374">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1375">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1375">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1376">Om du vill kopiera data från en PostgreSQL-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1376">If you are copying data from a PostgreSQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1377">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1377">Property</span></span> | <span data-ttu-id="3b691-1378">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1378">Description</span></span> | <span data-ttu-id="3b691-1379">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1379">Allowed values</span></span> | <span data-ttu-id="3b691-1380">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1380">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1381">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1381">query</span></span> |<span data-ttu-id="3b691-1382">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1382">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1383">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1383">SQL query string.</span></span> <span data-ttu-id="3b691-1384">Till exempel: ”frågan” ”: Välj * från \"MySchema\".\" Mytable prefix\"”.</span><span class="sxs-lookup"><span data-stu-id="3b691-1384">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="3b691-1385">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1385">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1386">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1386">Example</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1387">Mer information finns i [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1387">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) article.</span></span>

## <a name="sap-business-warehouse"></a><span data-ttu-id="3b691-1388">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-1388">SAP Business Warehouse</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-1389">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1389">Linked service</span></span>
<span data-ttu-id="3b691-1390">toodefine en SAP Business Warehouse (BW) länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**SapBw**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1390">toodefine a SAP Business Warehouse (BW) linked service, set hello **type** of hello linked service too**SapBw**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="3b691-1391">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1391">Property</span></span> | <span data-ttu-id="3b691-1392">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1392">Description</span></span> | <span data-ttu-id="3b691-1393">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1393">Allowed values</span></span> | <span data-ttu-id="3b691-1394">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1394">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="3b691-1395">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1395">server</span></span> | <span data-ttu-id="3b691-1396">Namnet på hello-server på vilken hello SAP BW instansen finns.</span><span class="sxs-lookup"><span data-stu-id="3b691-1396">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="3b691-1397">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1397">string</span></span> | <span data-ttu-id="3b691-1398">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1398">Yes</span></span>
<span data-ttu-id="3b691-1399">systemNumber</span><span class="sxs-lookup"><span data-stu-id="3b691-1399">systemNumber</span></span> | <span data-ttu-id="3b691-1400">System antal hello SAP BW system.</span><span class="sxs-lookup"><span data-stu-id="3b691-1400">System number of hello SAP BW system.</span></span> | <span data-ttu-id="3b691-1401">Två siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1401">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="3b691-1402">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1402">Yes</span></span>
<span data-ttu-id="3b691-1403">clientId</span><span class="sxs-lookup"><span data-stu-id="3b691-1403">clientId</span></span> | <span data-ttu-id="3b691-1404">Klient-ID för hello klienten i hello SAP W system.</span><span class="sxs-lookup"><span data-stu-id="3b691-1404">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="3b691-1405">Tre siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1405">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="3b691-1406">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1406">Yes</span></span>
<span data-ttu-id="3b691-1407">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1407">username</span></span> | <span data-ttu-id="3b691-1408">Namnet på hello-användare som har åtkomst toohello SAP-server</span><span class="sxs-lookup"><span data-stu-id="3b691-1408">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="3b691-1409">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1409">string</span></span> | <span data-ttu-id="3b691-1410">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1410">Yes</span></span>
<span data-ttu-id="3b691-1411">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1411">password</span></span> | <span data-ttu-id="3b691-1412">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1412">Password for hello user.</span></span> | <span data-ttu-id="3b691-1413">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1413">string</span></span> | <span data-ttu-id="3b691-1414">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1414">Yes</span></span>
<span data-ttu-id="3b691-1415">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1415">gatewayName</span></span> | <span data-ttu-id="3b691-1416">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP BW instans.</span><span class="sxs-lookup"><span data-stu-id="3b691-1416">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="3b691-1417">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1417">string</span></span> | <span data-ttu-id="3b691-1418">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1418">Yes</span></span>
<span data-ttu-id="3b691-1419">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1419">encryptedCredential</span></span> | <span data-ttu-id="3b691-1420">hello krypterade autentiseringsuppgifter strängen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1420">hello encrypted credential string.</span></span> | <span data-ttu-id="3b691-1421">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1421">string</span></span> | <span data-ttu-id="3b691-1422">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1422">No</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-1423">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1423">Example</span></span>

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3b691-1424">Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1424">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1425">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1425">Dataset</span></span>
<span data-ttu-id="3b691-1426">toodefine en SAP BW dataset set hello **typen** på hello datamängd för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1426">toodefine a SAP BW dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="3b691-1427">Det finns inga typspecifika egenskaper som stöds för hello SAP BW dataset av typen **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1427">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span>  

#### <a name="example"></a><span data-ttu-id="3b691-1428">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1428">Example</span></span>

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3b691-1429">Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1429">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1430">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1430">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1431">Om du vill kopiera data från SAP Business Warehouse, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1431">If you are copying data from SAP Business Warehouse, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1432">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1432">Property</span></span> | <span data-ttu-id="3b691-1433">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1433">Description</span></span> | <span data-ttu-id="3b691-1434">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1434">Allowed values</span></span> | <span data-ttu-id="3b691-1435">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1435">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1436">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1436">query</span></span> | <span data-ttu-id="3b691-1437">Anger hello MDX-fråga tooread data från hello SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="3b691-1437">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="3b691-1438">MDX-fråga.</span><span class="sxs-lookup"><span data-stu-id="3b691-1438">MDX query.</span></span> | <span data-ttu-id="3b691-1439">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1439">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1440">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1440">Example</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1441">Mer information finns i [SAP Business Warehouse-anslutningsapp](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1441">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sap-hana"></a><span data-ttu-id="3b691-1442">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3b691-1442">SAP HANA</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1443">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1443">Linked service</span></span>
<span data-ttu-id="3b691-1444">toodefine en SAP HANA länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**SapHana**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1444">toodefine a SAP HANA linked service, set hello **type** of hello linked service too**SapHana**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="3b691-1445">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1445">Property</span></span> | <span data-ttu-id="3b691-1446">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1446">Description</span></span> | <span data-ttu-id="3b691-1447">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1447">Allowed values</span></span> | <span data-ttu-id="3b691-1448">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1448">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="3b691-1449">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1449">server</span></span> | <span data-ttu-id="3b691-1450">Namnet på hello-server på vilken hello SAP HANA instansen finns.</span><span class="sxs-lookup"><span data-stu-id="3b691-1450">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="3b691-1451">Om servern använder en anpassad port, ange `server:port`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1451">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="3b691-1452">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1452">string</span></span> | <span data-ttu-id="3b691-1453">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1453">Yes</span></span>
<span data-ttu-id="3b691-1454">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1454">authenticationType</span></span> | <span data-ttu-id="3b691-1455">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1455">Type of authentication.</span></span> | <span data-ttu-id="3b691-1456">Sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1456">string.</span></span> <span data-ttu-id="3b691-1457">”Basic” eller ”Windows”</span><span class="sxs-lookup"><span data-stu-id="3b691-1457">"Basic" or "Windows"</span></span> | <span data-ttu-id="3b691-1458">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1458">Yes</span></span> 
<span data-ttu-id="3b691-1459">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1459">username</span></span> | <span data-ttu-id="3b691-1460">Namnet på hello-användare som har åtkomst toohello SAP-server</span><span class="sxs-lookup"><span data-stu-id="3b691-1460">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="3b691-1461">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1461">string</span></span> | <span data-ttu-id="3b691-1462">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1462">Yes</span></span>
<span data-ttu-id="3b691-1463">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1463">password</span></span> | <span data-ttu-id="3b691-1464">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1464">Password for hello user.</span></span> | <span data-ttu-id="3b691-1465">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1465">string</span></span> | <span data-ttu-id="3b691-1466">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1466">Yes</span></span>
<span data-ttu-id="3b691-1467">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1467">gatewayName</span></span> | <span data-ttu-id="3b691-1468">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="3b691-1468">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="3b691-1469">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1469">string</span></span> | <span data-ttu-id="3b691-1470">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1470">Yes</span></span>
<span data-ttu-id="3b691-1471">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1471">encryptedCredential</span></span> | <span data-ttu-id="3b691-1472">hello krypterade autentiseringsuppgifter strängen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1472">hello encrypted credential string.</span></span> | <span data-ttu-id="3b691-1473">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1473">string</span></span> | <span data-ttu-id="3b691-1474">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1474">No</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-1475">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1475">Example</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
<span data-ttu-id="3b691-1476">Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1476">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) article.</span></span>
 
### <a name="dataset"></a><span data-ttu-id="3b691-1477">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1477">Dataset</span></span>
<span data-ttu-id="3b691-1478">toodefine en SAP HANA-dataset set hello **typen** på hello datamängd för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1478">toodefine a SAP HANA dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="3b691-1479">Det finns inga typspecifika egenskaper som stöds för hello SAP HANA dataset av typen **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1479">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 

#### <a name="example"></a><span data-ttu-id="3b691-1480">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1480">Example</span></span>

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3b691-1481">Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1481">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1482">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1482">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1483">Om du vill kopiera data från en SAP HANA-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1483">If you are copying data from a SAP HANA data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1484">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1484">Property</span></span> | <span data-ttu-id="3b691-1485">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1485">Description</span></span> | <span data-ttu-id="3b691-1486">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1486">Allowed values</span></span> | <span data-ttu-id="3b691-1487">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1487">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1488">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1488">query</span></span> | <span data-ttu-id="3b691-1489">Anger hello SQL-frågan tooread data från hello SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="3b691-1489">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="3b691-1490">SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="3b691-1490">SQL query.</span></span> | <span data-ttu-id="3b691-1491">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1491">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-1492">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1492">Example</span></span>


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1493">Mer information finns i [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1493">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) article.</span></span>


## <a name="sql-server"></a><span data-ttu-id="3b691-1494">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3b691-1494">SQL Server</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1495">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1495">Linked service</span></span>
<span data-ttu-id="3b691-1496">Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1496">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="3b691-1497">hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-1497">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="3b691-1498">hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-1498">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="3b691-1499">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1499">Property</span></span> | <span data-ttu-id="3b691-1500">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1500">Description</span></span> | <span data-ttu-id="3b691-1501">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1501">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1502">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-1502">type</span></span> |<span data-ttu-id="3b691-1503">hello typegenskapen ska anges till: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1503">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="3b691-1504">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1504">Yes</span></span> |
| <span data-ttu-id="3b691-1505">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-1505">connectionString</span></span> |<span data-ttu-id="3b691-1506">Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1506">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="3b691-1507">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1507">Yes</span></span> |
| <span data-ttu-id="3b691-1508">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1508">gatewayName</span></span> |<span data-ttu-id="3b691-1509">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1509">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="3b691-1510">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1510">Yes</span></span> |
| <span data-ttu-id="3b691-1511">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1511">username</span></span> |<span data-ttu-id="3b691-1512">Ange användarnamnet om du använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1512">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="3b691-1513">Exempel: **domainname\\användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1513">Example: **domainname\\username**.</span></span> |<span data-ttu-id="3b691-1514">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1514">No</span></span> |
| <span data-ttu-id="3b691-1515">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1515">password</span></span> |<span data-ttu-id="3b691-1516">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1516">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-1517">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1517">No</span></span> |

<span data-ttu-id="3b691-1518">Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):</span><span class="sxs-lookup"><span data-stu-id="3b691-1518">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="3b691-1519">Exempel: JSON för att använda SQL-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-1519">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="3b691-1520">Exempel: JSON för att använda Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-1520">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="3b691-1521">Om användarnamn och lösenord anges gateway använder dem tooimpersonate hello användaren konto tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1521">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="3b691-1522">Gatewayen ansluter annars toohello SQL Server direkt med hello säkerhetskontexten för Gateway (dess start-konto).</span><span class="sxs-lookup"><span data-stu-id="3b691-1522">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3b691-1523">Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1523">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1524">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1524">Dataset</span></span>
<span data-ttu-id="3b691-1525">toodefine en SQL Server-dataset set hello **typen** på hello datamängd för**SqlServerTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1525">toodefine a SQL Server dataset, set hello **type** of hello dataset too**SqlServerTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1526">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1526">Property</span></span> | <span data-ttu-id="3b691-1527">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1527">Description</span></span> | <span data-ttu-id="3b691-1528">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1528">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1529">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1529">tableName</span></span> |<span data-ttu-id="3b691-1530">Namnet på hello tabellen eller vyn i hello SQL Server-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1530">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="3b691-1531">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1531">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1532">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1532">Example</span></span>
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1533">Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1533">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="3b691-1534">SQL-datakälla i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1534">Sql Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1535">Om du vill kopiera data från en SQL Server-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**SqlSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1535">If you are copying data from a SQL Server database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1536">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1536">Property</span></span> | <span data-ttu-id="3b691-1537">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1537">Description</span></span> | <span data-ttu-id="3b691-1538">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1538">Allowed values</span></span> | <span data-ttu-id="3b691-1539">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1539">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1540">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3b691-1540">sqlReaderQuery</span></span> |<span data-ttu-id="3b691-1541">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1541">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1542">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1542">SQL query string.</span></span> <span data-ttu-id="3b691-1543">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1543">For example: `select * from MyTable`.</span></span> <span data-ttu-id="3b691-1544">Kan referera till flera tabeller från hello-databas som refereras av hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-1544">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="3b691-1545">Om inget anges hello SQL-instruktionen som körs: Välj från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="3b691-1545">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="3b691-1546">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1546">No</span></span> |
| <span data-ttu-id="3b691-1547">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-1547">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3b691-1548">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1548">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="3b691-1549">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-1549">Name of hello stored procedure.</span></span> |<span data-ttu-id="3b691-1550">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1550">No</span></span> |
| <span data-ttu-id="3b691-1551">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-1551">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-1552">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-1552">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3b691-1553">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3b691-1553">Name/value pairs.</span></span> <span data-ttu-id="3b691-1554">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1554">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3b691-1555">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1555">No</span></span> |

<span data-ttu-id="3b691-1556">Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källa tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1556">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="3b691-1557">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="3b691-1557">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="3b691-1558">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1558">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="3b691-1559">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1559">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="3b691-1560">När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON.</span><span class="sxs-lookup"><span data-stu-id="3b691-1560">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="3b691-1561">Det finns inga verifieringar utföras om mot den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1561">There are no validations performed against this table though.</span></span>


#### <a name="example"></a><span data-ttu-id="3b691-1562">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1562">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1563">I det här exemplet **sqlReaderQuery** har angetts för hello SqlSource.</span><span class="sxs-lookup"><span data-stu-id="3b691-1563">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="3b691-1564">Hej Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källdata tooget hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1564">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="3b691-1565">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="3b691-1565">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="3b691-1566">Hej sqlReaderQuery kan referera till flera tabeller i hello-databas som refereras av hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-1566">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="3b691-1567">Det är inte begränsad tooonly hello tabellen som hello datauppsättnings tableName typeProperty.</span><span class="sxs-lookup"><span data-stu-id="3b691-1567">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="3b691-1568">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1568">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="3b691-1569">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1569">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="3b691-1570">Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1570">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="3b691-1571">SQL-mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1571">Sql Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-1572">Om du kopierar data tooa SQL Server-databas, ange hello **sink typen** av hello kopieringsaktiviteten för**SqlSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1572">If you are copying data tooa SQL Server database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-1573">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1573">Property</span></span> | <span data-ttu-id="3b691-1574">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1574">Description</span></span> | <span data-ttu-id="3b691-1575">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1575">Allowed values</span></span> | <span data-ttu-id="3b691-1576">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1576">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1577">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-1577">writeBatchTimeout</span></span> |<span data-ttu-id="3b691-1578">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3b691-1578">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="3b691-1579">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3b691-1579">timespan</span></span><br/><br/> <span data-ttu-id="3b691-1580">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3b691-1580">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3b691-1581">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1581">No</span></span> |
| <span data-ttu-id="3b691-1582">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3b691-1582">writeBatchSize</span></span> |<span data-ttu-id="3b691-1583">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3b691-1583">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3b691-1584">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3b691-1584">Integer (number of rows)</span></span> |<span data-ttu-id="3b691-1585">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="3b691-1585">No (default: 10000)</span></span> |
| <span data-ttu-id="3b691-1586">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3b691-1586">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3b691-1587">Ange frågan för Kopieringsaktiviteten tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="3b691-1587">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="3b691-1588">Mer information finns i [repeterbarhet](#repeatability-during-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1588">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="3b691-1589">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="3b691-1589">A query statement.</span></span> |<span data-ttu-id="3b691-1590">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1590">No</span></span> |
| <span data-ttu-id="3b691-1591">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3b691-1591">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3b691-1592">Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1592">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="3b691-1593">Mer information finns i [repeterbarhet](#repeatability-during-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1593">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="3b691-1594">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="3b691-1594">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3b691-1595">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1595">No</span></span> |
| <span data-ttu-id="3b691-1596">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-1596">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="3b691-1597">Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1597">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="3b691-1598">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-1598">Name of hello stored procedure.</span></span> |<span data-ttu-id="3b691-1599">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1599">No</span></span> |
| <span data-ttu-id="3b691-1600">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-1600">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-1601">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3b691-1601">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3b691-1602">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3b691-1602">Name/value pairs.</span></span> <span data-ttu-id="3b691-1603">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1603">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3b691-1604">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1604">No</span></span> |
| <span data-ttu-id="3b691-1605">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="3b691-1605">sqlWriterTableType</span></span> |<span data-ttu-id="3b691-1606">Ange tabellen typen namnet toobe används i hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="3b691-1606">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="3b691-1607">Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1607">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="3b691-1608">Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1608">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="3b691-1609">Ett namn för tabellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1609">A table type name.</span></span> |<span data-ttu-id="3b691-1610">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1610">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1611">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1611">Example</span></span>
<span data-ttu-id="3b691-1612">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="3b691-1612">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="3b691-1613">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1613">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1614">Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1614">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sybase"></a><span data-ttu-id="3b691-1615">Sybase</span><span class="sxs-lookup"><span data-stu-id="3b691-1615">Sybase</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1616">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1616">Linked service</span></span>
<span data-ttu-id="3b691-1617">toodefine en Sybase länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesSybase**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1617">toodefine a Sybase linked service, set hello **type** of hello linked service too**OnPremisesSybase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1618">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1618">Property</span></span> | <span data-ttu-id="3b691-1619">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1619">Description</span></span> | <span data-ttu-id="3b691-1620">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1620">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1621">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1621">server</span></span> |<span data-ttu-id="3b691-1622">Namnet på hello Sybase-servern.</span><span class="sxs-lookup"><span data-stu-id="3b691-1622">Name of hello Sybase server.</span></span> |<span data-ttu-id="3b691-1623">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1623">Yes</span></span> |
| <span data-ttu-id="3b691-1624">Databasen</span><span class="sxs-lookup"><span data-stu-id="3b691-1624">database</span></span> |<span data-ttu-id="3b691-1625">Namnet på hello Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1625">Name of hello Sybase database.</span></span> |<span data-ttu-id="3b691-1626">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1626">Yes</span></span> |
| <span data-ttu-id="3b691-1627">Schemat</span><span class="sxs-lookup"><span data-stu-id="3b691-1627">schema</span></span> |<span data-ttu-id="3b691-1628">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1628">Name of hello schema in hello database.</span></span> |<span data-ttu-id="3b691-1629">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1629">No</span></span> |
| <span data-ttu-id="3b691-1630">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1630">authenticationType</span></span> |<span data-ttu-id="3b691-1631">Typ av autentisering används tooconnect toohello Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1631">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="3b691-1632">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-1632">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3b691-1633">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1633">Yes</span></span> |
| <span data-ttu-id="3b691-1634">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1634">username</span></span> |<span data-ttu-id="3b691-1635">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1635">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3b691-1636">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1636">No</span></span> |
| <span data-ttu-id="3b691-1637">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1637">password</span></span> |<span data-ttu-id="3b691-1638">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1638">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-1639">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1639">No</span></span> |
| <span data-ttu-id="3b691-1640">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1640">gatewayName</span></span> |<span data-ttu-id="3b691-1641">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1641">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="3b691-1642">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1642">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1643">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1643">Example</span></span>
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="3b691-1644">Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1644">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1645">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1645">Dataset</span></span>
<span data-ttu-id="3b691-1646">toodefine en Sybase-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1646">toodefine a Sybase dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1647">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1647">Property</span></span> | <span data-ttu-id="3b691-1648">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1648">Description</span></span> | <span data-ttu-id="3b691-1649">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1649">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1650">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1650">tableName</span></span> |<span data-ttu-id="3b691-1651">Namnet på hello tabell i hello Sybase-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3b691-1651">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="3b691-1652">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1652">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1653">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1653">Example</span></span>

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1654">Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1654">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1655">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1655">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1656">Om du vill kopiera data från en Sybase-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1656">If you are copying data from a Sybase database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1657">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1657">Property</span></span> | <span data-ttu-id="3b691-1658">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1658">Description</span></span> | <span data-ttu-id="3b691-1659">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1659">Allowed values</span></span> | <span data-ttu-id="3b691-1660">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1660">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1661">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1661">query</span></span> |<span data-ttu-id="3b691-1662">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1662">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1663">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1663">SQL query string.</span></span> <span data-ttu-id="3b691-1664">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1664">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-1665">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1665">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1666">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1666">Example</span></span>

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1667">Mer information finns i [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1667">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) article.</span></span>

## <a name="teradata"></a><span data-ttu-id="3b691-1668">Teradata</span><span class="sxs-lookup"><span data-stu-id="3b691-1668">Teradata</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1669">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1669">Linked service</span></span>
<span data-ttu-id="3b691-1670">toodefine en Teradata länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesTeradata**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1670">toodefine a Teradata linked service, set hello **type** of hello linked service too**OnPremisesTeradata**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1671">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1671">Property</span></span> | <span data-ttu-id="3b691-1672">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1672">Description</span></span> | <span data-ttu-id="3b691-1673">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1673">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1674">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1674">server</span></span> |<span data-ttu-id="3b691-1675">Namnet på hello Teradata-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1675">Name of hello Teradata server.</span></span> |<span data-ttu-id="3b691-1676">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1676">Yes</span></span> |
| <span data-ttu-id="3b691-1677">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1677">authenticationType</span></span> |<span data-ttu-id="3b691-1678">Typ av autentisering används tooconnect toohello Teradata-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1678">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="3b691-1679">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-1679">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3b691-1680">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1680">Yes</span></span> |
| <span data-ttu-id="3b691-1681">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1681">username</span></span> |<span data-ttu-id="3b691-1682">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1682">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3b691-1683">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1683">No</span></span> |
| <span data-ttu-id="3b691-1684">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1684">password</span></span> |<span data-ttu-id="3b691-1685">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1685">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-1686">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1686">No</span></span> |
| <span data-ttu-id="3b691-1687">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1687">gatewayName</span></span> |<span data-ttu-id="3b691-1688">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Teradata-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1688">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="3b691-1689">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1689">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1690">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1690">Example</span></span>
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="3b691-1691">Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1691">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1692">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1692">Dataset</span></span>
<span data-ttu-id="3b691-1693">toodefine en Teradata-blobbdatauppsättning set hello **typen** på hello datamängd för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1693">toodefine a Teradata Blob dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="3b691-1694">Det finns för närvarande inga egenskaper som stöds för hello Teradata dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-1694">Currently, there are no type properties supported for hello Teradata dataset.</span></span> 

#### <a name="example"></a><span data-ttu-id="3b691-1695">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1695">Example</span></span>
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1696">Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1696">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-1697">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1697">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1698">Om du vill kopiera data från en Teradata-databas, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1698">If you are copying data from a Teradata database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1699">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1699">Property</span></span> | <span data-ttu-id="3b691-1700">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1700">Description</span></span> | <span data-ttu-id="3b691-1701">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1701">Allowed values</span></span> | <span data-ttu-id="3b691-1702">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1702">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1703">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1703">query</span></span> |<span data-ttu-id="3b691-1704">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1704">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1705">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1705">SQL query string.</span></span> <span data-ttu-id="3b691-1706">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1706">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-1707">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1707">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1708">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1708">Example</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3b691-1709">Mer information finns i [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1709">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) article.</span></span>

## <a name="cassandra"></a><span data-ttu-id="3b691-1710">Cassandra</span><span class="sxs-lookup"><span data-stu-id="3b691-1710">Cassandra</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-1711">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1711">Linked service</span></span>
<span data-ttu-id="3b691-1712">toodefine en Cassandra länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesCassandra**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1712">toodefine a Cassandra linked service, set hello **type** of hello linked service too**OnPremisesCassandra**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1713">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1713">Property</span></span> | <span data-ttu-id="3b691-1714">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1714">Description</span></span> | <span data-ttu-id="3b691-1715">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1715">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1716">värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1716">host</span></span> |<span data-ttu-id="3b691-1717">En eller flera IP-adresser eller värdnamn Cassandra servrar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1717">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="3b691-1718">Ange en kommaavgränsad lista med IP-adresser eller tooall värdservrar namn tooconnect samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1718">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="3b691-1719">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1719">Yes</span></span> |
| <span data-ttu-id="3b691-1720">port</span><span class="sxs-lookup"><span data-stu-id="3b691-1720">port</span></span> |<span data-ttu-id="3b691-1721">hello TCP-port som hello Cassandra server använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1721">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="3b691-1722">Nej, standardvärde: 9042</span><span class="sxs-lookup"><span data-stu-id="3b691-1722">No, default value: 9042</span></span> |
| <span data-ttu-id="3b691-1723">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1723">authenticationType</span></span> |<span data-ttu-id="3b691-1724">Grundläggande eller anonym</span><span class="sxs-lookup"><span data-stu-id="3b691-1724">Basic, or Anonymous</span></span> |<span data-ttu-id="3b691-1725">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1725">Yes</span></span> |
| <span data-ttu-id="3b691-1726">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1726">username</span></span> |<span data-ttu-id="3b691-1727">Ange användarnamn för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3b691-1727">Specify user name for hello user account.</span></span> |<span data-ttu-id="3b691-1728">Ja, om authenticationType anges tooBasic.</span><span class="sxs-lookup"><span data-stu-id="3b691-1728">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="3b691-1729">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1729">password</span></span> |<span data-ttu-id="3b691-1730">Ange lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1730">Specify password for hello user account.</span></span> |<span data-ttu-id="3b691-1731">Ja, om authenticationType anges tooBasic.</span><span class="sxs-lookup"><span data-stu-id="3b691-1731">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="3b691-1732">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1732">gatewayName</span></span> |<span data-ttu-id="3b691-1733">hello namnet på hello-gateway som har använt tooconnect toohello lokalt Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1733">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="3b691-1734">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1734">Yes</span></span> |
| <span data-ttu-id="3b691-1735">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1735">encryptedCredential</span></span> |<span data-ttu-id="3b691-1736">Autentiseringsuppgifter har krypterats av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="3b691-1736">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="3b691-1737">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1737">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1738">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1738">Example</span></span>

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3b691-1739">Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1739">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-1740">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1740">Dataset</span></span>
<span data-ttu-id="3b691-1741">toodefine datauppsättningars Cassandra set hello **typen** på hello datamängd för**CassandraTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1741">toodefine a Cassandra dataset, set hello **type** of hello dataset too**CassandraTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1742">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1742">Property</span></span> | <span data-ttu-id="3b691-1743">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1743">Description</span></span> | <span data-ttu-id="3b691-1744">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1744">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1745">keyspace</span><span class="sxs-lookup"><span data-stu-id="3b691-1745">keyspace</span></span> |<span data-ttu-id="3b691-1746">Namnet på hello keyspace eller schema i Cassandra databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1746">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="3b691-1747">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="3b691-1747">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="3b691-1748">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-1748">tableName</span></span> |<span data-ttu-id="3b691-1749">Namnet på hello tabell i Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1749">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="3b691-1750">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="3b691-1750">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1751">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1751">Example</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1752">Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1752">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) article.</span></span> 

### <a name="cassandra-source-in-copy-activity"></a><span data-ttu-id="3b691-1753">Cassandra källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1753">Cassandra Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1754">Om du vill kopiera data från Cassandra, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**CassandraSource**, och ange följande egenskaper i hello **källa** avsnitt :</span><span class="sxs-lookup"><span data-stu-id="3b691-1754">If you are copying data from Cassandra, set hello **source type** of hello copy activity too**CassandraSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1755">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1755">Property</span></span> | <span data-ttu-id="3b691-1756">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1756">Description</span></span> | <span data-ttu-id="3b691-1757">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1757">Allowed values</span></span> | <span data-ttu-id="3b691-1758">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1758">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1759">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1759">query</span></span> |<span data-ttu-id="3b691-1760">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1760">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1761">SQL-92 frågan eller CQL frågan.</span><span class="sxs-lookup"><span data-stu-id="3b691-1761">SQL-92 query or CQL query.</span></span> <span data-ttu-id="3b691-1762">Se [CQL referens](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="3b691-1762">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="3b691-1763">När du använder SQL-frågan anger **keyspace name.table namn** toorepresent hello tabell tooquery.</span><span class="sxs-lookup"><span data-stu-id="3b691-1763">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="3b691-1764">Nej (om tabellnamn och keyspace för datauppsättningen har definierats).</span><span class="sxs-lookup"><span data-stu-id="3b691-1764">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="3b691-1765">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="3b691-1765">consistencyLevel</span></span> |<span data-ttu-id="3b691-1766">hello konsekvensnivå anger hur många repliker måste svara tooa läsbegäran innan det returneras data toohello klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3b691-1766">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="3b691-1767">Cassandra kontrollerar hello angivet antal repliker för data toosatisfy hello läsa begäran.</span><span class="sxs-lookup"><span data-stu-id="3b691-1767">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="3b691-1768">EN, TVÅ, TRE, KVORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="3b691-1768">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="3b691-1769">Se [konfigurera datakonsekvens](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) mer information.</span><span class="sxs-lookup"><span data-stu-id="3b691-1769">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="3b691-1770">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-1770">No.</span></span> <span data-ttu-id="3b691-1771">Standardvärdet är en.</span><span class="sxs-lookup"><span data-stu-id="3b691-1771">Default value is ONE.</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1772">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1772">Example</span></span>
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-1773">Mer information finns i [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1773">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) article.</span></span>

## <a name="mongodb"></a><span data-ttu-id="3b691-1774">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1774">MongoDB</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-1775">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1775">Linked service</span></span>
<span data-ttu-id="3b691-1776">toodefine en MongoDB länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesMongoDB**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1776">toodefine a MongoDB linked service, set hello **type** of hello linked service too**OnPremisesMongoDB**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1777">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1777">Property</span></span> | <span data-ttu-id="3b691-1778">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1778">Description</span></span> | <span data-ttu-id="3b691-1779">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1779">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1780">server</span><span class="sxs-lookup"><span data-stu-id="3b691-1780">server</span></span> |<span data-ttu-id="3b691-1781">IP-adressen eller värdnamnet namnet på hello MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="3b691-1781">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="3b691-1782">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1782">Yes</span></span> |
| <span data-ttu-id="3b691-1783">port</span><span class="sxs-lookup"><span data-stu-id="3b691-1783">port</span></span> |<span data-ttu-id="3b691-1784">TCP-port som hello MongoDB-servern använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="3b691-1784">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="3b691-1785">Valfritt, standardvärdet: 27017</span><span class="sxs-lookup"><span data-stu-id="3b691-1785">Optional, default value: 27017</span></span> |
| <span data-ttu-id="3b691-1786">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-1786">authenticationType</span></span> |<span data-ttu-id="3b691-1787">Grundläggande eller anonym.</span><span class="sxs-lookup"><span data-stu-id="3b691-1787">Basic, or Anonymous.</span></span> |<span data-ttu-id="3b691-1788">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1788">Yes</span></span> |
| <span data-ttu-id="3b691-1789">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1789">username</span></span> |<span data-ttu-id="3b691-1790">Användarens konto tooaccess MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3b691-1790">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="3b691-1791">Ja (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="3b691-1791">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="3b691-1792">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1792">password</span></span> |<span data-ttu-id="3b691-1793">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1793">Password for hello user.</span></span> |<span data-ttu-id="3b691-1794">Ja (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="3b691-1794">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="3b691-1795">authSource</span><span class="sxs-lookup"><span data-stu-id="3b691-1795">authSource</span></span> |<span data-ttu-id="3b691-1796">Namnet på hello MongoDB-databas som du vill toouse toocheck dina autentiseringsuppgifter för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-1796">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="3b691-1797">Valfritt (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="3b691-1797">Optional (if basic authentication is used).</span></span> <span data-ttu-id="3b691-1798">standard: använder hello administratörskonto och hello databas som har angetts med egenskapen databaseName.</span><span class="sxs-lookup"><span data-stu-id="3b691-1798">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="3b691-1799">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="3b691-1799">databaseName</span></span> |<span data-ttu-id="3b691-1800">Namnet på hello MongoDB-databas som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="3b691-1800">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="3b691-1801">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1801">Yes</span></span> |
| <span data-ttu-id="3b691-1802">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1802">gatewayName</span></span> |<span data-ttu-id="3b691-1803">Namnet på hello-gateway som har åtkomst till hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-1803">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="3b691-1804">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1804">Yes</span></span> |
| <span data-ttu-id="3b691-1805">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1805">encryptedCredential</span></span> |<span data-ttu-id="3b691-1806">Autentiseringsuppgifter har krypterats av gateway.</span><span class="sxs-lookup"><span data-stu-id="3b691-1806">Credential encrypted by gateway.</span></span> |<span data-ttu-id="3b691-1807">Valfri</span><span class="sxs-lookup"><span data-stu-id="3b691-1807">Optional</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1808">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1808">Example</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3b691-1809">Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="3b691-1809">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1810">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1810">Dataset</span></span>
<span data-ttu-id="3b691-1811">toodefine en MongoDB-dataset set hello **typen** på hello datamängd för**MongoDbCollection**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1811">toodefine a MongoDB dataset, set hello **type** of hello dataset too**MongoDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1812">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1812">Property</span></span> | <span data-ttu-id="3b691-1813">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1813">Description</span></span> | <span data-ttu-id="3b691-1814">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1814">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1815">Samlingsnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-1815">collectionName</span></span> |<span data-ttu-id="3b691-1816">Namnet på hello-samlingen i MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-1816">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="3b691-1817">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1817">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1818">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1818">Example</span></span>

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="3b691-1819">Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="3b691-1819">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span></span>

#### <a name="mongodb-source-in-copy-activity"></a><span data-ttu-id="3b691-1820">MongoDB-källan i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1820">MongoDB Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1821">Om du vill kopiera data från MongoDB, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**MongoDbSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1821">If you are copying data from MongoDB, set hello **source type** of hello copy activity too**MongoDbSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1822">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1822">Property</span></span> | <span data-ttu-id="3b691-1823">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1823">Description</span></span> | <span data-ttu-id="3b691-1824">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1824">Allowed values</span></span> | <span data-ttu-id="3b691-1825">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1825">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1826">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-1826">query</span></span> |<span data-ttu-id="3b691-1827">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1827">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-1828">SQL-92 frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1828">SQL-92 query string.</span></span> <span data-ttu-id="3b691-1829">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-1829">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-1830">Nej (om **samlingsnamn** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-1830">No (if **collectionName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1831">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1831">Example</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1832">Mer information finns i [MongoDB connector artikel](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="3b691-1832">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span></span>

## <a name="amazon-s3"></a><span data-ttu-id="3b691-1833">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="3b691-1833">Amazon S3</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-1834">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1834">Linked service</span></span>
<span data-ttu-id="3b691-1835">toodefine en Amazon S3 länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AwsAccessKey**, och ange följande egenskaper i hello **typeProperties** avsnitt :</span><span class="sxs-lookup"><span data-stu-id="3b691-1835">toodefine an Amazon S3 linked service, set hello **type** of hello linked service too**AwsAccessKey**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-1836">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1836">Property</span></span> | <span data-ttu-id="3b691-1837">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1837">Description</span></span> | <span data-ttu-id="3b691-1838">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1838">Allowed values</span></span> | <span data-ttu-id="3b691-1839">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1839">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1840">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="3b691-1840">accessKeyID</span></span> |<span data-ttu-id="3b691-1841">ID för hello hemliga snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="3b691-1841">ID of hello secret access key.</span></span> |<span data-ttu-id="3b691-1842">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1842">string</span></span> |<span data-ttu-id="3b691-1843">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1843">Yes</span></span> |
| <span data-ttu-id="3b691-1844">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="3b691-1844">secretAccessKey</span></span> |<span data-ttu-id="3b691-1845">hello hemliga åtkomst själva nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3b691-1845">hello secret access key itself.</span></span> |<span data-ttu-id="3b691-1846">Krypterad hemliga sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1846">Encrypted secret string</span></span> |<span data-ttu-id="3b691-1847">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1847">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-1848">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1848">Example</span></span>
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

<span data-ttu-id="3b691-1849">Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-1849">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1850">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1850">Dataset</span></span>
<span data-ttu-id="3b691-1851">toodefine en Amazon S3 dataset, ange hello **typen** på hello datamängd för**AmazonS3**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1851">toodefine an Amazon S3 dataset, set hello **type** of hello dataset too**AmazonS3**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1852">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1852">Property</span></span> | <span data-ttu-id="3b691-1853">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1853">Description</span></span> | <span data-ttu-id="3b691-1854">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1854">Allowed values</span></span> | <span data-ttu-id="3b691-1855">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1855">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1856">bucketName</span><span class="sxs-lookup"><span data-stu-id="3b691-1856">bucketName</span></span> |<span data-ttu-id="3b691-1857">hello S3 bucket-namn.</span><span class="sxs-lookup"><span data-stu-id="3b691-1857">hello S3 bucket name.</span></span> |<span data-ttu-id="3b691-1858">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1858">String</span></span> |<span data-ttu-id="3b691-1859">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1859">Yes</span></span> |
| <span data-ttu-id="3b691-1860">key</span><span class="sxs-lookup"><span data-stu-id="3b691-1860">key</span></span> |<span data-ttu-id="3b691-1861">hello S3 Objektnyckel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1861">hello S3 object key.</span></span> |<span data-ttu-id="3b691-1862">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1862">String</span></span> |<span data-ttu-id="3b691-1863">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1863">No</span></span> |
| <span data-ttu-id="3b691-1864">prefix</span><span class="sxs-lookup"><span data-stu-id="3b691-1864">prefix</span></span> |<span data-ttu-id="3b691-1865">Prefix för hello S3 objekt nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3b691-1865">Prefix for hello S3 object key.</span></span> <span data-ttu-id="3b691-1866">Objekt vars nycklar som börjar med prefixet är markerade.</span><span class="sxs-lookup"><span data-stu-id="3b691-1866">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="3b691-1867">Gäller endast när nyckeln är tom.</span><span class="sxs-lookup"><span data-stu-id="3b691-1867">Applies only when key is empty.</span></span> |<span data-ttu-id="3b691-1868">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1868">String</span></span> |<span data-ttu-id="3b691-1869">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1869">No</span></span> |
| <span data-ttu-id="3b691-1870">Version</span><span class="sxs-lookup"><span data-stu-id="3b691-1870">version</span></span> |<span data-ttu-id="3b691-1871">hello version av S3 objekt om S3 versionshantering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="3b691-1871">hello version of S3 object if S3 versioning is enabled.</span></span> |<span data-ttu-id="3b691-1872">Sträng</span><span class="sxs-lookup"><span data-stu-id="3b691-1872">String</span></span> |<span data-ttu-id="3b691-1873">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1873">No</span></span> |
| <span data-ttu-id="3b691-1874">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-1874">format</span></span> | <span data-ttu-id="3b691-1875">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1875">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-1876">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-1876">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-1877">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1877">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-1878">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-1878">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-1879">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1879">No</span></span> | |
| <span data-ttu-id="3b691-1880">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-1880">compression</span></span> | <span data-ttu-id="3b691-1881">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1881">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-1882">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1882">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-1883">hello som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1883">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-1884">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-1884">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-1885">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1885">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="3b691-1886">bucketName + nyckeln anger hello platsen för hello S3 objekt där bucket är hello Rotbehållare för S3 objekt och nyckeln är hello fullständig sökväg tooS3 objekt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1886">bucketName + key specifies hello location of hello S3 object where bucket is hello root container for S3 objects and key is hello full path tooS3 object.</span></span>

#### <a name="example-sample-dataset-with-prefix"></a><span data-ttu-id="3b691-1887">Exempel: Exempeldatamängd med prefix</span><span class="sxs-lookup"><span data-stu-id="3b691-1887">Example: Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a><span data-ttu-id="3b691-1888">Exempel: Exempel datauppsättning (med version)</span><span class="sxs-lookup"><span data-stu-id="3b691-1888">Example: Sample data set (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a><span data-ttu-id="3b691-1889">Exempel: Dynamiska sökvägar för S3</span><span class="sxs-lookup"><span data-stu-id="3b691-1889">Example: Dynamic paths for S3</span></span>
<span data-ttu-id="3b691-1890">I exemplet hello använder vi fasta värden för nyckeln och bucketName egenskaper i hello Amazon S3 dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-1890">In hello sample, we use fixed values for key and bucketName properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

<span data-ttu-id="3b691-1891">Du kan ha Data Factory beräkna hello nyckel och bucketName dynamiskt vid körning med systemvariabler som SliceStart.</span><span class="sxs-lookup"><span data-stu-id="3b691-1891">You can have Data Factory calculate hello key and bucketName dynamically at runtime by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="3b691-1892">Du kan göra hello samma för hello prefix-egenskapen för en Amazon S3 datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b691-1892">You can do hello same for hello prefix property of an Amazon S3 dataset.</span></span> <span data-ttu-id="3b691-1893">Se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md) en lista över funktioner som stöds och variabler.</span><span class="sxs-lookup"><span data-stu-id="3b691-1893">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of supported functions and variables.</span></span>

<span data-ttu-id="3b691-1894">Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-1894">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3b691-1895">Källa för System i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1895">File System Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1896">Om du vill kopiera data från Amazon S3, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt :</span><span class="sxs-lookup"><span data-stu-id="3b691-1896">If you are copying data from Amazon S3, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="3b691-1897">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1897">Property</span></span> | <span data-ttu-id="3b691-1898">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1898">Description</span></span> | <span data-ttu-id="3b691-1899">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1899">Allowed values</span></span> | <span data-ttu-id="3b691-1900">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1900">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1901">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-1901">recursive</span></span> |<span data-ttu-id="3b691-1902">Anger om toorecursively lista S3 objekt under hello katalog.</span><span class="sxs-lookup"><span data-stu-id="3b691-1902">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="3b691-1903">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="3b691-1903">true/false</span></span> |<span data-ttu-id="3b691-1904">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1904">No</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-1905">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1905">Example</span></span>


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

<span data-ttu-id="3b691-1906">Mer information finns i [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-1906">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span></span>

## <a name="file-system"></a><span data-ttu-id="3b691-1907">Filsystem</span><span class="sxs-lookup"><span data-stu-id="3b691-1907">File System</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-1908">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-1908">Linked service</span></span>
<span data-ttu-id="3b691-1909">Du kan länka en lokal fil system tooan Azure data factory med hello **lokala filserver** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-1909">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="3b691-1910">hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello lokala filserver länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-1910">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="3b691-1911">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1911">Property</span></span> | <span data-ttu-id="3b691-1912">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1912">Description</span></span> | <span data-ttu-id="3b691-1913">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1913">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1914">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-1914">type</span></span> |<span data-ttu-id="3b691-1915">Kontrollera att hello type-egenskap är inställd för**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1915">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="3b691-1916">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1916">Yes</span></span> |
| <span data-ttu-id="3b691-1917">värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1917">host</span></span> |<span data-ttu-id="3b691-1918">Anger hello rotsökvägen för hello mappen som du vill toocopy.</span><span class="sxs-lookup"><span data-stu-id="3b691-1918">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="3b691-1919">Använd hello escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1919">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="3b691-1920">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1920">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="3b691-1921">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1921">Yes</span></span> |
| <span data-ttu-id="3b691-1922">användar-ID</span><span class="sxs-lookup"><span data-stu-id="3b691-1922">userid</span></span> |<span data-ttu-id="3b691-1923">Ange hello-ID för hello-användare som har åtkomst till toohello servern.</span><span class="sxs-lookup"><span data-stu-id="3b691-1923">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="3b691-1924">Nej (om du väljer encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="3b691-1924">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="3b691-1925">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-1925">password</span></span> |<span data-ttu-id="3b691-1926">Ange hello användarlösenord hello (användar-ID).</span><span class="sxs-lookup"><span data-stu-id="3b691-1926">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="3b691-1927">Nej (om du väljer encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1927">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="3b691-1928">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1928">encryptedCredential</span></span> |<span data-ttu-id="3b691-1929">Ange hello krypterade autentiseringsuppgifter som du kan få genom att köra hello ny AzureRmDataFactoryEncryptValue cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3b691-1929">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="3b691-1930">Nej (om du väljer toospecify användar-ID och lösenord i klartext)</span><span class="sxs-lookup"><span data-stu-id="3b691-1930">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="3b691-1931">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-1931">gatewayName</span></span> |<span data-ttu-id="3b691-1932">Anger hello namnet på hello gateway som Data Factory ska använda tooconnect toohello lokal server.</span><span class="sxs-lookup"><span data-stu-id="3b691-1932">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="3b691-1933">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1933">Yes</span></span> |

#### <a name="sample-folder-path-definitions"></a><span data-ttu-id="3b691-1934">Exempel mappen sökväg definitioner</span><span class="sxs-lookup"><span data-stu-id="3b691-1934">Sample folder path definitions</span></span> 
| <span data-ttu-id="3b691-1935">Scenario</span><span class="sxs-lookup"><span data-stu-id="3b691-1935">Scenario</span></span> | <span data-ttu-id="3b691-1936">Värden i länkade tjänstdefinitionen</span><span class="sxs-lookup"><span data-stu-id="3b691-1936">Host in linked service definition</span></span> | <span data-ttu-id="3b691-1937">folderPath i datauppsättningsdefinitionen</span><span class="sxs-lookup"><span data-stu-id="3b691-1937">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1938">Lokal mapp på Data Management Gateway-datorn:</span><span class="sxs-lookup"><span data-stu-id="3b691-1938">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="3b691-1939">Exempel: D:\\ \* eller D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="3b691-1939">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="3b691-1940">D:\\ \\ (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="3b691-1940">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="3b691-1941">localhost (för tidigare versioner än Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="3b691-1941">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="3b691-1942">. \\ \\ eller mappen\\\\undermapp (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="3b691-1942">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="3b691-1943">D:\\ \\ eller D:\\\\mappen\\\\undermapp (för gateway-versionen nedan 2.0)</span><span class="sxs-lookup"><span data-stu-id="3b691-1943">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="3b691-1944">Delad fjärrmapp:</span><span class="sxs-lookup"><span data-stu-id="3b691-1944">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="3b691-1945">Exempel: \\ \\minserver\\dela\\ \* eller \\ \\minserver\\dela\\mappen\\undermapp\\*</span><span class="sxs-lookup"><span data-stu-id="3b691-1945">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="3b691-1946">\\\\\\\\minserver\\\\dela</span><span class="sxs-lookup"><span data-stu-id="3b691-1946">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="3b691-1947">. \\ \\ eller mappen\\\\undermapp</span><span class="sxs-lookup"><span data-stu-id="3b691-1947">.\\\\ or folder\\\\subfolder</span></span> |


#### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="3b691-1948">Exempel: Med användarnamn och lösenord i klartext</span><span class="sxs-lookup"><span data-stu-id="3b691-1948">Example: Using username and password in plain text</span></span>

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a><span data-ttu-id="3b691-1949">Exempel: Använda encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="3b691-1949">Example: Using encryptedcredential</span></span>

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3b691-1950">Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-1950">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-1951">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-1951">Dataset</span></span>
<span data-ttu-id="3b691-1952">toodefine datauppsättningars filsystemet set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1952">toodefine a File System dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-1953">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1953">Property</span></span> | <span data-ttu-id="3b691-1954">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1954">Description</span></span> | <span data-ttu-id="3b691-1955">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1955">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-1956">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-1956">folderPath</span></span> |<span data-ttu-id="3b691-1957">Anger hello undersökvägen toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-1957">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="3b691-1958">Använd hello escape-tecknet ' \' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-1958">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="3b691-1959">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="3b691-1959">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3b691-1960">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="3b691-1960">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3b691-1961">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-1961">Yes</span></span> |
| <span data-ttu-id="3b691-1962">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-1962">fileName</span></span> |<span data-ttu-id="3b691-1963">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-1963">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="3b691-1964">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-1964">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="3b691-1965">Om filnamnet inte anges för en datamängd för utdata heter hello hello skapas filen i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="3b691-1965">When fileName is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="3b691-1966">`Data.<Guid>.txt`(Exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="3b691-1966">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="3b691-1967">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1967">No</span></span> |
| <span data-ttu-id="3b691-1968">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3b691-1968">fileFilter</span></span> |<span data-ttu-id="3b691-1969">Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="3b691-1969">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="3b691-1970">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="3b691-1970">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3b691-1971">Exempel 1: ”fileFilter” ”: * .log”</span><span class="sxs-lookup"><span data-stu-id="3b691-1971">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="3b691-1972">Exempel 2: ”fileFilter”: 2016 - 1-?. txt ”</span><span class="sxs-lookup"><span data-stu-id="3b691-1972">Example 2: "fileFilter": 2016-1-?.txt"</span></span><br/><br/><span data-ttu-id="3b691-1973">Observera att fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b691-1973">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="3b691-1974">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1974">No</span></span> |
| <span data-ttu-id="3b691-1975">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-1975">partitionedBy</span></span> |<span data-ttu-id="3b691-1976">Du kan använda partitionedBy toospecify dynamiska folderPath/filnamn för time series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1976">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="3b691-1977">Ett exempel är folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1977">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-1978">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1978">No</span></span> |
| <span data-ttu-id="3b691-1979">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-1979">format</span></span> | <span data-ttu-id="3b691-1980">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1980">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-1981">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-1981">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-1982">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1982">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-1983">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-1983">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-1984">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1984">No</span></span> |
| <span data-ttu-id="3b691-1985">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-1985">compression</span></span> | <span data-ttu-id="3b691-1986">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-1986">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-1987">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**; och nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-1987">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-1988">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-1988">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-1989">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-1989">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3b691-1990">Du kan inte använda filnamn och fileFilter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3b691-1990">You cannot use fileName and fileFilter simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-1991">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-1991">Example</span></span>

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-1992">Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-1992">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3b691-1993">Källa för System i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-1993">File System Source in Copy Activity</span></span>
<span data-ttu-id="3b691-1994">Om du kopierar data från filsystemet, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-1994">If you are copying data from File System, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-1995">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-1995">Property</span></span> | <span data-ttu-id="3b691-1996">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-1996">Description</span></span> | <span data-ttu-id="3b691-1997">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-1997">Allowed values</span></span> | <span data-ttu-id="3b691-1998">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-1998">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-1999">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-1999">recursive</span></span> |<span data-ttu-id="3b691-2000">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2000">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-2001">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-2001">True, False (default)</span></span> |<span data-ttu-id="3b691-2002">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2002">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2003">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2003">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3b691-2004">Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-2004">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

### <a name="file-system-sink-in-copy-activity"></a><span data-ttu-id="3b691-2005">Filsystem mottagare i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2005">File System Sink in Copy Activity</span></span>
<span data-ttu-id="3b691-2006">Om du kopierar data tooFile System, ange hello **sink typen** av hello kopieringsaktiviteten för**FileSystemSink**, och ange följande egenskaper i hello **sink** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2006">If you are copying data tooFile System, set hello **sink type** of hello copy activity too**FileSystemSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="3b691-2007">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2007">Property</span></span> | <span data-ttu-id="3b691-2008">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2008">Description</span></span> | <span data-ttu-id="3b691-2009">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2009">Allowed values</span></span> | <span data-ttu-id="3b691-2010">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2010">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2011">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3b691-2011">copyBehavior</span></span> |<span data-ttu-id="3b691-2012">Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem.</span><span class="sxs-lookup"><span data-stu-id="3b691-2012">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="3b691-2013">**PreserveHierarchy:** bevarar hello filen hierarki i hello målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2013">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="3b691-2014">Hello relativ sökväg hello filen toohello källa källmappen är det vill säga hello samma som hello relativa sökväg hello filen toohello mål målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2014">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="3b691-2015">**FlattenHierarchy:** alla filer från källmappen hello skapas i hello första nivån i målmappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2015">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="3b691-2016">hello mål filer skapas med ett namn som genererats automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2016">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="3b691-2017">**MergeFiles:** sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="3b691-2017">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="3b691-2018">Om hello namn/blob filnamn har angetts är hello kopplade filnamnet hello angivet namn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2018">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="3b691-2019">Annars är ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2019">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="3b691-2020">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2020">No</span></span> |
<span data-ttu-id="3b691-2021">Auto-</span><span class="sxs-lookup"><span data-stu-id="3b691-2021">auto-</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-2022">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2022">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-2023">Mer information finns i [filsystemet connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3b691-2023">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

## <a name="ftp"></a><span data-ttu-id="3b691-2024">FTP</span><span class="sxs-lookup"><span data-stu-id="3b691-2024">FTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-2025">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2025">Linked service</span></span>
<span data-ttu-id="3b691-2026">toodefine en FTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**FtpServer**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2026">toodefine an FTP linked service, set hello **type** of hello linked service too**FtpServer**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2027">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2027">Property</span></span> | <span data-ttu-id="3b691-2028">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2028">Description</span></span> | <span data-ttu-id="3b691-2029">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2029">Required</span></span> | <span data-ttu-id="3b691-2030">Standard</span><span class="sxs-lookup"><span data-stu-id="3b691-2030">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2031">värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2031">host</span></span> |<span data-ttu-id="3b691-2032">Namn eller IP-adress för hello FTP-servern</span><span class="sxs-lookup"><span data-stu-id="3b691-2032">Name or IP address of hello FTP Server</span></span> |<span data-ttu-id="3b691-2033">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2033">Yes</span></span> |&nbsp; |
| <span data-ttu-id="3b691-2034">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2034">authenticationType</span></span> |<span data-ttu-id="3b691-2035">Ange autentiseringstyp</span><span class="sxs-lookup"><span data-stu-id="3b691-2035">Specify authentication type</span></span> |<span data-ttu-id="3b691-2036">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2036">Yes</span></span> |<span data-ttu-id="3b691-2037">Grundläggande, anonyma</span><span class="sxs-lookup"><span data-stu-id="3b691-2037">Basic, Anonymous</span></span> |
| <span data-ttu-id="3b691-2038">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2038">username</span></span> |<span data-ttu-id="3b691-2039">Användare som har åtkomst till toohello FTP-servern</span><span class="sxs-lookup"><span data-stu-id="3b691-2039">User who has access toohello FTP server</span></span> |<span data-ttu-id="3b691-2040">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2040">No</span></span> |&nbsp; |
| <span data-ttu-id="3b691-2041">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2041">password</span></span> |<span data-ttu-id="3b691-2042">Lösenordet för hello (användarnamn)</span><span class="sxs-lookup"><span data-stu-id="3b691-2042">Password for hello user (username)</span></span> |<span data-ttu-id="3b691-2043">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2043">No</span></span> |&nbsp; |
| <span data-ttu-id="3b691-2044">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-2044">encryptedCredential</span></span> |<span data-ttu-id="3b691-2045">Krypterade autentiseringsuppgifter tooaccess hello FTP-server</span><span class="sxs-lookup"><span data-stu-id="3b691-2045">Encrypted credential tooaccess hello FTP server</span></span> |<span data-ttu-id="3b691-2046">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2046">No</span></span> |&nbsp; |
| <span data-ttu-id="3b691-2047">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2047">gatewayName</span></span> |<span data-ttu-id="3b691-2048">Namnet på hello Data Management Gateway gateway tooconnect tooan lokalt FTP-servern</span><span class="sxs-lookup"><span data-stu-id="3b691-2048">Name of hello Data Management Gateway gateway tooconnect tooan on-premises FTP server</span></span> |<span data-ttu-id="3b691-2049">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2049">No</span></span> |&nbsp; |
| <span data-ttu-id="3b691-2050">port</span><span class="sxs-lookup"><span data-stu-id="3b691-2050">port</span></span> |<span data-ttu-id="3b691-2051">Port servern lyssnar på vilka hello FTP</span><span class="sxs-lookup"><span data-stu-id="3b691-2051">Port on which hello FTP server is listening</span></span> |<span data-ttu-id="3b691-2052">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2052">No</span></span> |<span data-ttu-id="3b691-2053">21</span><span class="sxs-lookup"><span data-stu-id="3b691-2053">21</span></span> |
| <span data-ttu-id="3b691-2054">enableSsl</span><span class="sxs-lookup"><span data-stu-id="3b691-2054">enableSsl</span></span> |<span data-ttu-id="3b691-2055">Ange om toouse FTP över SSL/TLS-kanalen</span><span class="sxs-lookup"><span data-stu-id="3b691-2055">Specify whether toouse FTP over SSL/TLS channel</span></span> |<span data-ttu-id="3b691-2056">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2056">No</span></span> |<span data-ttu-id="3b691-2057">SANT</span><span class="sxs-lookup"><span data-stu-id="3b691-2057">true</span></span> |
| <span data-ttu-id="3b691-2058">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3b691-2058">enableServerCertificateValidation</span></span> |<span data-ttu-id="3b691-2059">Ange om tooenable server SSL-certifikatet verifieringen när du använder FTP över SSL/TLS-kanalen</span><span class="sxs-lookup"><span data-stu-id="3b691-2059">Specify whether tooenable server SSL certificate validation when using FTP over SSL/TLS channel</span></span> |<span data-ttu-id="3b691-2060">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2060">No</span></span> |<span data-ttu-id="3b691-2061">SANT</span><span class="sxs-lookup"><span data-stu-id="3b691-2061">true</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3b691-2062">Exempel: Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2062">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="3b691-2063">Exempel: Med användarnamn och lösenord i klartext för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2063">Example: Using username and password in plain text for basic authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="3b691-2064">Exempel: Med port, enableSsl enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3b691-2064">Example: Using port, enableSsl, enableServerCertificateValidation</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="3b691-2065">Exempel: Använder encryptedCredential för autentisering och gateway</span><span class="sxs-lookup"><span data-stu-id="3b691-2065">Example: Using encryptedCredential for authentication and gateway</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

<span data-ttu-id="3b691-2066">Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2066">For more information, see [FTP connector](data-factory-ftp-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-2067">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2067">Dataset</span></span>
<span data-ttu-id="3b691-2068">toodefine en FTP-dataset, ange hello **typ** på hello datamängd för**FileShare**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2068">toodefine an FTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2069">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2069">Property</span></span> | <span data-ttu-id="3b691-2070">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2070">Description</span></span> | <span data-ttu-id="3b691-2071">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2071">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2072">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-2072">folderPath</span></span> |<span data-ttu-id="3b691-2073">Sökvägen toohello undermapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-2073">Sub path toohello folder.</span></span> <span data-ttu-id="3b691-2074">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-2074">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="3b691-2075">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2075">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3b691-2076">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="3b691-2076">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3b691-2077">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2077">Yes</span></span> 
| <span data-ttu-id="3b691-2078">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-2078">fileName</span></span> |<span data-ttu-id="3b691-2079">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2079">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="3b691-2080">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2080">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="3b691-2081">Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="3b691-2081">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="3b691-2082">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3b691-2082">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3b691-2083">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2083">No</span></span> |
| <span data-ttu-id="3b691-2084">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3b691-2084">fileFilter</span></span> |<span data-ttu-id="3b691-2085">Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2085">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="3b691-2086">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="3b691-2086">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3b691-2087">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="3b691-2087">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="3b691-2088">Exempel 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="3b691-2088">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="3b691-2089">fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b691-2089">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="3b691-2090">Den här egenskapen stöds inte med HDFS.</span><span class="sxs-lookup"><span data-stu-id="3b691-2090">This property is not supported with HDFS.</span></span> |<span data-ttu-id="3b691-2091">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2091">No</span></span> |
| <span data-ttu-id="3b691-2092">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-2092">partitionedBy</span></span> |<span data-ttu-id="3b691-2093">partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2093">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3b691-2094">Till exempel folderPath som innehåller parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2094">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-2095">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2095">No</span></span> |
| <span data-ttu-id="3b691-2096">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-2096">format</span></span> | <span data-ttu-id="3b691-2097">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2097">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-2098">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-2098">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-2099">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2099">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-2100">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-2100">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-2101">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2101">No</span></span> |
| <span data-ttu-id="3b691-2102">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-2102">compression</span></span> | <span data-ttu-id="3b691-2103">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2103">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-2104">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**; och nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2104">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-2105">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-2105">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-2106">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2106">No</span></span> |
| <span data-ttu-id="3b691-2107">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="3b691-2107">useBinaryTransfer</span></span> |<span data-ttu-id="3b691-2108">Ange om använder binära överföringsläge.</span><span class="sxs-lookup"><span data-stu-id="3b691-2108">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="3b691-2109">True för en binär och FALSKT ASCII.</span><span class="sxs-lookup"><span data-stu-id="3b691-2109">True for binary mode and false ASCII.</span></span> <span data-ttu-id="3b691-2110">Standardvärde: True.</span><span class="sxs-lookup"><span data-stu-id="3b691-2110">Default value: True.</span></span> <span data-ttu-id="3b691-2111">Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2111">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="3b691-2112">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2112">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3b691-2113">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2113">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-2114">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2114">Example</span></span>

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3b691-2115">Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2115">For more information, see [FTP connector](data-factory-ftp-connector.md#dataset-properties) article.</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3b691-2116">Källa för System i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2116">File System Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2117">Om du vill kopiera data från en FTP-server, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2117">If you are copying data from an FTP server, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2118">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2118">Property</span></span> | <span data-ttu-id="3b691-2119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2119">Description</span></span> | <span data-ttu-id="3b691-2120">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2120">Allowed values</span></span> | <span data-ttu-id="3b691-2121">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2121">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2122">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-2122">recursive</span></span> |<span data-ttu-id="3b691-2123">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2123">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-2124">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-2124">True, False (default)</span></span> |<span data-ttu-id="3b691-2125">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2125">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2126">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2126">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

<span data-ttu-id="3b691-2127">Mer information finns i [FTP-anslutningen](data-factory-ftp-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2127">For more information, see [FTP connector](data-factory-ftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="hdfs"></a><span data-ttu-id="3b691-2128">HDFS</span><span class="sxs-lookup"><span data-stu-id="3b691-2128">HDFS</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-2129">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2129">Linked service</span></span>
<span data-ttu-id="3b691-2130">toodefine en HDFS länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Hdfs**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2130">toodefine a HDFS linked service, set hello **type** of hello linked service too**Hdfs**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2131">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2131">Property</span></span> | <span data-ttu-id="3b691-2132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2132">Description</span></span> | <span data-ttu-id="3b691-2133">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2133">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2134">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2134">type</span></span> |<span data-ttu-id="3b691-2135">hello Typegenskapen måste anges till: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="3b691-2135">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="3b691-2136">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2136">Yes</span></span> |
| <span data-ttu-id="3b691-2137">URL</span><span class="sxs-lookup"><span data-stu-id="3b691-2137">Url</span></span> |<span data-ttu-id="3b691-2138">URL: en toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="3b691-2138">URL toohello HDFS</span></span> |<span data-ttu-id="3b691-2139">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2139">Yes</span></span> |
| <span data-ttu-id="3b691-2140">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2140">authenticationType</span></span> |<span data-ttu-id="3b691-2141">Anonym, eller Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-2141">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="3b691-2142">toouse **Kerberos-autentisering** HDFS-anslutningen finns för[i det här avsnittet](#use-kerberos-authentication-for-hdfs-connector) tooset upp din lokala miljö därefter.</span><span class="sxs-lookup"><span data-stu-id="3b691-2142">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="3b691-2143">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2143">Yes</span></span> |
| <span data-ttu-id="3b691-2144">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2144">userName</span></span> |<span data-ttu-id="3b691-2145">Användarnamn för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2145">Username for Windows authentication.</span></span> |<span data-ttu-id="3b691-2146">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-2146">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="3b691-2147">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2147">password</span></span> |<span data-ttu-id="3b691-2148">Lösenordet för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2148">Password for Windows authentication.</span></span> |<span data-ttu-id="3b691-2149">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-2149">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="3b691-2150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2150">gatewayName</span></span> |<span data-ttu-id="3b691-2151">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello HDFS.</span><span class="sxs-lookup"><span data-stu-id="3b691-2151">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="3b691-2152">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2152">Yes</span></span> |
| <span data-ttu-id="3b691-2153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-2153">encryptedCredential</span></span> |<span data-ttu-id="3b691-2154">[Nya AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) utdata från hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3b691-2154">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="3b691-2155">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2155">No</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3b691-2156">Exempel: Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2156">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a><span data-ttu-id="3b691-2157">Exempel: Med Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2157">Example: Using Windows authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3b691-2158">Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2158">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-2159">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2159">Dataset</span></span>
<span data-ttu-id="3b691-2160">toodefine datauppsättningars HDFS set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2160">toodefine a HDFS dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2161">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2161">Property</span></span> | <span data-ttu-id="3b691-2162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2162">Description</span></span> | <span data-ttu-id="3b691-2163">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2163">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2164">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-2164">folderPath</span></span> |<span data-ttu-id="3b691-2165">Sökvägen toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-2165">Path toohello folder.</span></span> <span data-ttu-id="3b691-2166">Exempel:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="3b691-2166">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="3b691-2167">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-2167">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="3b691-2168">Till exempel: Ange mapp för folder\subfolder,\\\\undermapp och ange d: för d:\samplefolder,\\\\Exempelmapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-2168">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="3b691-2169">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="3b691-2169">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3b691-2170">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2170">Yes</span></span> |
| <span data-ttu-id="3b691-2171">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-2171">fileName</span></span> |<span data-ttu-id="3b691-2172">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2172">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="3b691-2173">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2173">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="3b691-2174">Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="3b691-2174">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="3b691-2175">Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3b691-2175">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3b691-2176">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2176">No</span></span> |
| <span data-ttu-id="3b691-2177">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-2177">partitionedBy</span></span> |<span data-ttu-id="3b691-2178">partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2178">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3b691-2179">Exempel: folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2179">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-2180">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2180">No</span></span> |
| <span data-ttu-id="3b691-2181">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-2181">format</span></span> | <span data-ttu-id="3b691-2182">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2182">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-2183">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-2183">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-2184">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2184">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-2185">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-2185">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-2186">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2186">No</span></span> |
| <span data-ttu-id="3b691-2187">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-2187">compression</span></span> | <span data-ttu-id="3b691-2188">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2188">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-2189">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2189">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-2190">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2190">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-2191">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-2191">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-2192">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2192">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3b691-2193">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2193">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-2194">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2194">Example</span></span>

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3b691-2195">Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2195">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3b691-2196">Källa för System i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2196">File System Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2197">Om du vill kopiera data från HDFS, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2197">If you are copying data from HDFS, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="3b691-2198">**FileSystemSource** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3b691-2198">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="3b691-2199">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2199">Property</span></span> | <span data-ttu-id="3b691-2200">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2200">Description</span></span> | <span data-ttu-id="3b691-2201">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2201">Allowed values</span></span> | <span data-ttu-id="3b691-2202">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2202">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2203">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-2203">recursive</span></span> |<span data-ttu-id="3b691-2204">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2204">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-2205">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-2205">True, False (default)</span></span> |<span data-ttu-id="3b691-2206">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2206">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2207">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2207">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3b691-2208">Mer information finns i [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2208">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) article.</span></span>

## <a name="sftp"></a><span data-ttu-id="3b691-2209">SFTP</span><span class="sxs-lookup"><span data-stu-id="3b691-2209">SFTP</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-2210">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2210">Linked service</span></span>
<span data-ttu-id="3b691-2211">toodefine en SFTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Sftp**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2211">toodefine an SFTP linked service, set hello **type** of hello linked service too**Sftp**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2212">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2212">Property</span></span> | <span data-ttu-id="3b691-2213">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2213">Description</span></span> | <span data-ttu-id="3b691-2214">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2214">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2215">värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2215">host</span></span> | <span data-ttu-id="3b691-2216">Namn eller IP-adressen för hello SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2216">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="3b691-2217">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2217">Yes</span></span> |
| <span data-ttu-id="3b691-2218">port</span><span class="sxs-lookup"><span data-stu-id="3b691-2218">port</span></span> |<span data-ttu-id="3b691-2219">Port på vilken hello SFTP servern lyssnar.</span><span class="sxs-lookup"><span data-stu-id="3b691-2219">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="3b691-2220">hello standardvärdet är: 21</span><span class="sxs-lookup"><span data-stu-id="3b691-2220">hello default value is: 21</span></span> |<span data-ttu-id="3b691-2221">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2221">No</span></span> |
| <span data-ttu-id="3b691-2222">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2222">authenticationType</span></span> |<span data-ttu-id="3b691-2223">Ange autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2223">Specify authentication type.</span></span> <span data-ttu-id="3b691-2224">Tillåtna värden: **grundläggande**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2224">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="3b691-2225">Se för[använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2225">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="3b691-2226">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2226">Yes</span></span> |
| <span data-ttu-id="3b691-2227">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="3b691-2227">skipHostKeyValidation</span></span> | <span data-ttu-id="3b691-2228">Ange om tooskip värd viktiga validering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2228">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="3b691-2229">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2229">No.</span></span> <span data-ttu-id="3b691-2230">Hej standardvärdet: false</span><span class="sxs-lookup"><span data-stu-id="3b691-2230">hello default value: false</span></span> |
| <span data-ttu-id="3b691-2231">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="3b691-2231">hostKeyFingerprint</span></span> | <span data-ttu-id="3b691-2232">Ange hello fingeravtryck hello värden nyckel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2232">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="3b691-2233">Ja om hello `skipHostKeyValidation` toofalse anges.</span><span class="sxs-lookup"><span data-stu-id="3b691-2233">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="3b691-2234">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2234">gatewayName</span></span> |<span data-ttu-id="3b691-2235">Namnet på hello Data Management Gateway tooconnect tooan lokalt SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2235">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="3b691-2236">Ja om du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2236">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="3b691-2237">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-2237">encryptedCredential</span></span> | <span data-ttu-id="3b691-2238">Krypterade autentiseringsuppgifter tooaccess hello SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2238">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="3b691-2239">Genereras automatiskt när du anger grundläggande autentisering (användarnamn och lösenord) eller SshPublicKey autentisering (användarnamn + privat sökväg eller innehåll) i Kopiera guiden eller hello ClickOnce popup-dialogruta.</span><span class="sxs-lookup"><span data-stu-id="3b691-2239">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="3b691-2240">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2240">No.</span></span> <span data-ttu-id="3b691-2241">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2241">Apply only when copying data from an on-premises SFTP server.</span></span> |

#### <a name="example-using-basic-authentication"></a><span data-ttu-id="3b691-2242">Exempel: Använder grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2242">Example: Using basic authentication</span></span>

<span data-ttu-id="3b691-2243">toouse grundläggande autentisering, ange `authenticationType` som `Basic`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-2243">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="3b691-2244">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2244">Property</span></span> | <span data-ttu-id="3b691-2245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2245">Description</span></span> | <span data-ttu-id="3b691-2246">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2246">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2247">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2247">username</span></span> | <span data-ttu-id="3b691-2248">Användare som har åtkomst till toohello SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="3b691-2248">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="3b691-2249">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2249">Yes</span></span> |
| <span data-ttu-id="3b691-2250">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2250">password</span></span> | <span data-ttu-id="3b691-2251">Lösenordet för hello (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="3b691-2251">Password for hello user (username).</span></span> | <span data-ttu-id="3b691-2252">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2252">Yes</span></span> |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="3b691-2253">Exempel: Grundläggande autentisering med krypterade autentiseringsuppgifter **</span><span class="sxs-lookup"><span data-stu-id="3b691-2253">Example: Basic authentication with encrypted credential**</span></span>

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="3b691-2254">Med hjälp av autentisering med SSH offentlig nyckel: **</span><span class="sxs-lookup"><span data-stu-id="3b691-2254">Using SSH public key authentication:**</span></span>

<span data-ttu-id="3b691-2255">toouse grundläggande autentisering, ange `authenticationType` som `SshPublicKey`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-2255">toouse basic authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="3b691-2256">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2256">Property</span></span> | <span data-ttu-id="3b691-2257">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2257">Description</span></span> | <span data-ttu-id="3b691-2258">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2258">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2259">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2259">username</span></span> |<span data-ttu-id="3b691-2260">Användare som har åtkomst till toohello SFTP-servern</span><span class="sxs-lookup"><span data-stu-id="3b691-2260">User who has access toohello SFTP server</span></span> |<span data-ttu-id="3b691-2261">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2261">Yes</span></span> |
| <span data-ttu-id="3b691-2262">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="3b691-2262">privateKeyPath</span></span> | <span data-ttu-id="3b691-2263">Ange absolut sökväg toohello fil för privat nyckel som gateway kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2263">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="3b691-2264">Ange antingen hello `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2264">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="3b691-2265">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2265">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="3b691-2266">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="3b691-2266">privateKeyContent</span></span> | <span data-ttu-id="3b691-2267">En serialiserad textsträng hello privata nyckel innehåll.</span><span class="sxs-lookup"><span data-stu-id="3b691-2267">A serialized string of hello private key content.</span></span> <span data-ttu-id="3b691-2268">hello guiden Kopiera kan läsa hello-fil för privat nyckel och extrahera hello privata nyckel innehållet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2268">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="3b691-2269">Om du använder någon annan verktyget/SDK, Använd hello privateKeyPath egenskapen i stället.</span><span class="sxs-lookup"><span data-stu-id="3b691-2269">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="3b691-2270">Ange antingen hello `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2270">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="3b691-2271">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="3b691-2271">passPhrase</span></span> | <span data-ttu-id="3b691-2272">Ange hello pass frasen/lösenord toodecrypt hello privata nyckel om hello nyckelfilen skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="3b691-2272">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="3b691-2273">Ja om hello privata nyckeln skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="3b691-2273">Yes if hello private key file is protected by a pass phrase.</span></span> |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="3b691-2274">Exempel: SshPublicKey autentisering med hjälp av privat nyckel innehåll **</span><span class="sxs-lookup"><span data-stu-id="3b691-2274">Example: SshPublicKey authentication using private key content**</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

<span data-ttu-id="3b691-2275">Mer information finns i [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2275">For more information, see [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-2276">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2276">Dataset</span></span>
<span data-ttu-id="3b691-2277">toodefine en SFTP-datauppsättning set hello **typen** på hello datamängd för**filresursen**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2277">toodefine an SFTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2278">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2278">Property</span></span> | <span data-ttu-id="3b691-2279">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2279">Description</span></span> | <span data-ttu-id="3b691-2280">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2280">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2281">folderPath</span><span class="sxs-lookup"><span data-stu-id="3b691-2281">folderPath</span></span> |<span data-ttu-id="3b691-2282">Sökvägen toohello undermapp.</span><span class="sxs-lookup"><span data-stu-id="3b691-2282">Sub path toohello folder.</span></span> <span data-ttu-id="3b691-2283">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-2283">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="3b691-2284">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2284">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3b691-2285">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="3b691-2285">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3b691-2286">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2286">Yes</span></span> |
| <span data-ttu-id="3b691-2287">fileName</span><span class="sxs-lookup"><span data-stu-id="3b691-2287">fileName</span></span> |<span data-ttu-id="3b691-2288">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2288">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="3b691-2289">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2289">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="3b691-2290">Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="3b691-2290">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="3b691-2291">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3b691-2291">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3b691-2292">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2292">No</span></span> |
| <span data-ttu-id="3b691-2293">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3b691-2293">fileFilter</span></span> |<span data-ttu-id="3b691-2294">Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2294">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="3b691-2295">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="3b691-2295">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3b691-2296">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="3b691-2296">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="3b691-2297">Exempel 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="3b691-2297">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="3b691-2298">fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b691-2298">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="3b691-2299">Den här egenskapen stöds inte med HDFS.</span><span class="sxs-lookup"><span data-stu-id="3b691-2299">This property is not supported with HDFS.</span></span> |<span data-ttu-id="3b691-2300">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2300">No</span></span> |
| <span data-ttu-id="3b691-2301">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3b691-2301">partitionedBy</span></span> |<span data-ttu-id="3b691-2302">partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2302">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3b691-2303">Till exempel folderPath som innehåller parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2303">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3b691-2304">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2304">No</span></span> |
| <span data-ttu-id="3b691-2305">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-2305">format</span></span> | <span data-ttu-id="3b691-2306">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2306">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-2307">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b691-2307">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="3b691-2308">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2308">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3b691-2309">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="3b691-2309">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3b691-2310">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2310">No</span></span> |
| <span data-ttu-id="3b691-2311">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-2311">compression</span></span> | <span data-ttu-id="3b691-2312">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2312">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-2313">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2313">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-2314">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2314">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-2315">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-2315">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-2316">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2316">No</span></span> |
| <span data-ttu-id="3b691-2317">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="3b691-2317">useBinaryTransfer</span></span> |<span data-ttu-id="3b691-2318">Ange om använder binära överföringsläge.</span><span class="sxs-lookup"><span data-stu-id="3b691-2318">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="3b691-2319">True för en binär och FALSKT ASCII.</span><span class="sxs-lookup"><span data-stu-id="3b691-2319">True for binary mode and false ASCII.</span></span> <span data-ttu-id="3b691-2320">Standardvärde: True.</span><span class="sxs-lookup"><span data-stu-id="3b691-2320">Default value: True.</span></span> <span data-ttu-id="3b691-2321">Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2321">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="3b691-2322">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2322">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3b691-2323">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2323">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-2324">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2324">Example</span></span>

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3b691-2325">Mer information finns i [SFTP connector](data-factory-sftp-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2325">For more information, see [SFTP connector](data-factory-sftp-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3b691-2326">Källa för System i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2326">File System Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2327">Om du kopierar data från en källa för SFTP ange hello **typ av datakälla** av hello kopieringsaktiviteten för**FileSystemSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2327">If you are copying data from an SFTP source, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2328">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2328">Property</span></span> | <span data-ttu-id="3b691-2329">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2329">Description</span></span> | <span data-ttu-id="3b691-2330">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2330">Allowed values</span></span> | <span data-ttu-id="3b691-2331">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2331">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2332">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="3b691-2332">recursive</span></span> |<span data-ttu-id="3b691-2333">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2333">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="3b691-2334">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-2334">True, False (default)</span></span> |<span data-ttu-id="3b691-2335">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2335">No</span></span> |



#### <a name="example"></a><span data-ttu-id="3b691-2336">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2336">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

<span data-ttu-id="3b691-2337">Mer information finns i [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2337">For more information, see [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="http"></a><span data-ttu-id="3b691-2338">HTTP</span><span class="sxs-lookup"><span data-stu-id="3b691-2338">HTTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-2339">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2339">Linked service</span></span>
<span data-ttu-id="3b691-2340">toodefine en HTTP länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Http**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2340">toodefine a HTTP linked service, set hello **type** of hello linked service too**Http**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2341">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2341">Property</span></span> | <span data-ttu-id="3b691-2342">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2342">Description</span></span> | <span data-ttu-id="3b691-2343">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2343">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2344">URL: en</span><span class="sxs-lookup"><span data-stu-id="3b691-2344">url</span></span> | <span data-ttu-id="3b691-2345">Bas-URL: en toohello webbserver</span><span class="sxs-lookup"><span data-stu-id="3b691-2345">Base URL toohello Web Server</span></span> | <span data-ttu-id="3b691-2346">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2346">Yes</span></span> |
| <span data-ttu-id="3b691-2347">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2347">authenticationType</span></span> | <span data-ttu-id="3b691-2348">Anger hello autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="3b691-2348">Specifies hello authentication type.</span></span> <span data-ttu-id="3b691-2349">Tillåtna värden är: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2349">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="3b691-2350">Läs respektive toosections under den här tabellen på fler egenskaper och JSON-exempel för dessa typer av autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2350">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="3b691-2351">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2351">Yes</span></span> |
| <span data-ttu-id="3b691-2352">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3b691-2352">enableServerCertificateValidation</span></span> | <span data-ttu-id="3b691-2353">Ange om tooenable server SSL-certifikatverifieringen om datakällan är HTTPS-webbserver</span><span class="sxs-lookup"><span data-stu-id="3b691-2353">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="3b691-2354">Nej, standard är SANT</span><span class="sxs-lookup"><span data-stu-id="3b691-2354">No, default is true</span></span> |
| <span data-ttu-id="3b691-2355">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2355">gatewayName</span></span> | <span data-ttu-id="3b691-2356">Namnet på hello Data Management Gateway tooconnect tooan lokalt http-källa.</span><span class="sxs-lookup"><span data-stu-id="3b691-2356">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="3b691-2357">Ja om du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="3b691-2357">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="3b691-2358">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-2358">encryptedCredential</span></span> | <span data-ttu-id="3b691-2359">Krypterade autentiseringsuppgifter tooaccess hello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2359">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="3b691-2360">Genereras automatiskt när du konfigurerar hello autentiseringsinformation i Kopiera guiden eller hello ClickOnce popup-dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b691-2360">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="3b691-2361">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2361">No.</span></span> <span data-ttu-id="3b691-2362">Gäller bara när du kopierar data från en lokal HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2362">Apply only when copying data from an on-premises HTTP server.</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="3b691-2363">Exempel: Med grundläggande, sammanfattad eller Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2363">Example: Using Basic, Digest, or Windows authentication</span></span>
<span data-ttu-id="3b691-2364">Ange `authenticationType` som `Basic`, `Digest`, eller `Windows`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-2364">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="3b691-2365">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2365">Property</span></span> | <span data-ttu-id="3b691-2366">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2366">Description</span></span> | <span data-ttu-id="3b691-2367">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2367">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2368">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2368">username</span></span> | <span data-ttu-id="3b691-2369">Användarnamnet tooaccess hello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2369">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="3b691-2370">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2370">Yes</span></span> |
| <span data-ttu-id="3b691-2371">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2371">password</span></span> | <span data-ttu-id="3b691-2372">Lösenordet för hello (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="3b691-2372">Password for hello user (username).</span></span> | <span data-ttu-id="3b691-2373">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2373">Yes</span></span> |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a><span data-ttu-id="3b691-2374">Exempel: Med ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2374">Example: Using ClientCertificate authentication</span></span>

<span data-ttu-id="3b691-2375">toouse grundläggande autentisering, ange `authenticationType` som `ClientCertificate`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-2375">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="3b691-2376">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2376">Property</span></span> | <span data-ttu-id="3b691-2377">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2377">Description</span></span> | <span data-ttu-id="3b691-2378">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2378">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2379">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="3b691-2379">embeddedCertData</span></span> | <span data-ttu-id="3b691-2380">hello Base64-kodade innehåll för binära data i hello Personal Information Exchange (PFX)-fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-2380">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="3b691-2381">Ange antingen hello `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2381">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="3b691-2382">certThumbprint</span><span class="sxs-lookup"><span data-stu-id="3b691-2382">certThumbprint</span></span> | <span data-ttu-id="3b691-2383">Hej tumavtrycket för certifikatet för hello som har installerats på gateway-datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="3b691-2383">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="3b691-2384">Gäller bara när du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="3b691-2384">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="3b691-2385">Ange antingen hello `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2385">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="3b691-2386">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2386">password</span></span> | <span data-ttu-id="3b691-2387">Lösenordet som är associerat med hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="3b691-2387">Password associated with hello certificate.</span></span> | <span data-ttu-id="3b691-2388">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2388">No</span></span> |

<span data-ttu-id="3b691-2389">Om du använder `certThumbprint` för autentisering och hello certifikat installeras i hello personliga arkivet i hello lokala datorn, behöver du toogrant hello läsbehörighet toohello gateway-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="3b691-2389">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="3b691-2390">Starta Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="3b691-2390">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="3b691-2391">Lägg till hello **certifikat** snapin-modulen som mål hello **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2391">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="3b691-2392">Expandera **certifikat**, **personliga**, och klicka på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2392">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="3b691-2393">Högerklicka på hello certifikatet från datorarkivet hello och välj **alla aktiviteter**->**hantera privata nycklar...**</span><span class="sxs-lookup"><span data-stu-id="3b691-2393">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="3b691-2394">På hello **säkerhet** lägger du till hello-användarkonto som Data Management Gateway-värdtjänsten körs under med hello läsbehörighet toohello certifikat.</span><span class="sxs-lookup"><span data-stu-id="3b691-2394">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

<span data-ttu-id="3b691-2395">**Exempel: använder klientcertifikat:** detta länkade tjänsten länkar din data factory tooan lokalt HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2395">**Example: using client certificate:** This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="3b691-2396">Den använder ett klientcertifikat som är installerad på datorn hello med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="3b691-2396">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="3b691-2397">Exempel: använder klientcertifikat i en fil</span><span class="sxs-lookup"><span data-stu-id="3b691-2397">Example: using client certificate in a file</span></span>
<span data-ttu-id="3b691-2398">Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="3b691-2398">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="3b691-2399">Det använder en klient-certifikatfil på hello datorn med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="3b691-2399">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

<span data-ttu-id="3b691-2400">Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2400">For more information, see [HTTP connector](data-factory-http-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-2401">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2401">Dataset</span></span>
<span data-ttu-id="3b691-2402">toodefine en HTTP-dataset, ange hello **typen** på hello datamängd för**Http**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2402">toodefine a HTTP dataset, set hello **type** of hello dataset too**Http**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2403">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2403">Property</span></span> | <span data-ttu-id="3b691-2404">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2404">Description</span></span> | <span data-ttu-id="3b691-2405">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2405">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-2406">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="3b691-2406">relativeUrl</span></span> | <span data-ttu-id="3b691-2407">En relativ URL toohello resurs som innehåller hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2407">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="3b691-2408">Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad.</span><span class="sxs-lookup"><span data-stu-id="3b691-2408">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="3b691-2409">dynamisk tooconstruct-URL som du kan använda [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md), exempel: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2409">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), Example: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span></span> | <span data-ttu-id="3b691-2410">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2410">No</span></span> |
| <span data-ttu-id="3b691-2411">requestMethod</span><span class="sxs-lookup"><span data-stu-id="3b691-2411">requestMethod</span></span> | <span data-ttu-id="3b691-2412">HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="3b691-2412">Http method.</span></span> <span data-ttu-id="3b691-2413">Tillåtna värden är **hämta** eller **efter**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2413">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="3b691-2414">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2414">No.</span></span> <span data-ttu-id="3b691-2415">Standardvärdet är `GET`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2415">Default is `GET`.</span></span> |
| <span data-ttu-id="3b691-2416">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="3b691-2416">additionalHeaders</span></span> | <span data-ttu-id="3b691-2417">Ytterligare HTTP-begärans sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="3b691-2417">Additional HTTP request headers.</span></span> | <span data-ttu-id="3b691-2418">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2418">No</span></span> |
| <span data-ttu-id="3b691-2419">requestBody</span><span class="sxs-lookup"><span data-stu-id="3b691-2419">requestBody</span></span> | <span data-ttu-id="3b691-2420">Brödtext för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="3b691-2420">Body for HTTP request.</span></span> | <span data-ttu-id="3b691-2421">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2421">No</span></span> |
| <span data-ttu-id="3b691-2422">Format</span><span class="sxs-lookup"><span data-stu-id="3b691-2422">format</span></span> | <span data-ttu-id="3b691-2423">Om du vill toosimply **hämta hello data från HTTP-slutpunkt som-är** hoppa över den här formatinställningar utan parsning den.</span><span class="sxs-lookup"><span data-stu-id="3b691-2423">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="3b691-2424">Om du vill tooparse hello HTTP-svar innehåll vid kopiering, hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2424">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3b691-2425">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2425">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="3b691-2426">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2426">No</span></span> |
| <span data-ttu-id="3b691-2427">Komprimering</span><span class="sxs-lookup"><span data-stu-id="3b691-2427">compression</span></span> | <span data-ttu-id="3b691-2428">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2428">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="3b691-2429">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2429">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3b691-2430">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2430">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3b691-2431">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3b691-2431">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3b691-2432">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2432">No</span></span> |

#### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="3b691-2433">Exempel: genom att använda metoden för hello GET (standard)</span><span class="sxs-lookup"><span data-stu-id="3b691-2433">Example: using hello GET (default) method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-hello-post-method"></a><span data-ttu-id="3b691-2434">Exempel: använda hello POST-metoden</span><span class="sxs-lookup"><span data-stu-id="3b691-2434">Example: using hello POST method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="3b691-2435">Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2435">For more information, see [HTTP connector](data-factory-http-connector.md#dataset-properties) article.</span></span>

### <a name="http-source-in-copy-activity"></a><span data-ttu-id="3b691-2436">HTTP-källan i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2436">HTTP Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2437">Om du kopierar data från en HTTP-källa, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**HttpSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2437">If you are copying data from a HTTP source, set hello **source type** of hello copy activity too**HttpSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2438">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2438">Property</span></span> | <span data-ttu-id="3b691-2439">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2439">Description</span></span> | <span data-ttu-id="3b691-2440">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2440">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3b691-2441">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="3b691-2441">httpRequestTimeout</span></span> | <span data-ttu-id="3b691-2442">Hej timeout (TimeSpan) för hello HTTP-begäran tooget ett svar.</span><span class="sxs-lookup"><span data-stu-id="3b691-2442">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="3b691-2443">Det är hello timeout tooget ett svar inte hello timeout tooread svarsdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-2443">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="3b691-2444">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2444">No.</span></span> <span data-ttu-id="3b691-2445">Standardvärde: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="3b691-2445">Default value: 00:01:40</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-2446">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2446">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-2447">Mer information finns i [HTTP-anslutningen](data-factory-http-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2447">For more information, see [HTTP connector](data-factory-http-connector.md#copy-activity-properties) article.</span></span>

## <a name="odata"></a><span data-ttu-id="3b691-2448">OData</span><span class="sxs-lookup"><span data-stu-id="3b691-2448">OData</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-2449">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2449">Linked service</span></span>
<span data-ttu-id="3b691-2450">toodefine en OData länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OData**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2450">toodefine an OData linked service, set hello **type** of hello linked service too**OData**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2451">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2451">Property</span></span> | <span data-ttu-id="3b691-2452">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2452">Description</span></span> | <span data-ttu-id="3b691-2453">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2453">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2454">URL: en</span><span class="sxs-lookup"><span data-stu-id="3b691-2454">url</span></span> |<span data-ttu-id="3b691-2455">URL till hello OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2455">Url of hello OData service.</span></span> |<span data-ttu-id="3b691-2456">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2456">Yes</span></span> |
| <span data-ttu-id="3b691-2457">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2457">authenticationType</span></span> |<span data-ttu-id="3b691-2458">Typ av autentisering används tooconnect toohello OData-källan.</span><span class="sxs-lookup"><span data-stu-id="3b691-2458">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="3b691-2459">För molnet OData är möjliga värden anonym, grundläggande och OAuth (Observera Azure Data Factory för närvarande endast stöder Azure Active Directory-baserad OAuth).</span><span class="sxs-lookup"><span data-stu-id="3b691-2459">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="3b691-2460">För lokala OData är möjliga värden anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="3b691-2460">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3b691-2461">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2461">Yes</span></span> |
| <span data-ttu-id="3b691-2462">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2462">username</span></span> |<span data-ttu-id="3b691-2463">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2463">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="3b691-2464">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-2464">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="3b691-2465">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2465">password</span></span> |<span data-ttu-id="3b691-2466">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2466">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-2467">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-2467">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="3b691-2468">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="3b691-2468">authorizedCredential</span></span> |<span data-ttu-id="3b691-2469">Om du använder OAuth, klickar du på **auktorisera** i hello guiden för Data Factory kopiera eller redigerare och ange dina autentiseringsuppgifter och sedan hello värdet på egenskapen kommer att genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2469">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="3b691-2470">Ja (endast om du använder OAuth-autentisering)</span><span class="sxs-lookup"><span data-stu-id="3b691-2470">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="3b691-2471">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2471">gatewayName</span></span> |<span data-ttu-id="3b691-2472">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2472">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="3b691-2473">Ange endast om du vill kopiera data från lokala OData-källan.</span><span class="sxs-lookup"><span data-stu-id="3b691-2473">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="3b691-2474">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2474">No</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="3b691-2475">Exempel: använder grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2475">Example - Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a><span data-ttu-id="3b691-2476">Exempel: använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2476">Example - Using Anonymous authentication</span></span>

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="3b691-2477">Exempel – med hjälp av Windows-autentisering åtkomst till lokala OData-källan</span><span class="sxs-lookup"><span data-stu-id="3b691-2477">Example - Using Windows authentication accessing on-premises OData source</span></span>

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="3b691-2478">Exempel – med åtkomst till molnet OData-källan OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2478">Example - Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

<span data-ttu-id="3b691-2479">Mer information finns i [OData connector](data-factory-odata-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2479">For more information, see [OData connector](data-factory-odata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3b691-2480">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2480">Dataset</span></span>
<span data-ttu-id="3b691-2481">toodefine en OData-datauppsättning set hello **typen** på hello datamängd för**ODataResource**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2481">toodefine an OData dataset, set hello **type** of hello dataset too**ODataResource**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2482">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2482">Property</span></span> | <span data-ttu-id="3b691-2483">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2483">Description</span></span> | <span data-ttu-id="3b691-2484">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2484">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2485">Sökväg</span><span class="sxs-lookup"><span data-stu-id="3b691-2485">path</span></span> |<span data-ttu-id="3b691-2486">Sökvägen toohello OData-resurs</span><span class="sxs-lookup"><span data-stu-id="3b691-2486">Path toohello OData resource</span></span> |<span data-ttu-id="3b691-2487">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2487">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2488">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2488">Example</span></span>

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

<span data-ttu-id="3b691-2489">Mer information finns i [OData connector](data-factory-odata-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2489">For more information, see [OData connector](data-factory-odata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-2490">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2490">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2491">Om du kopierar data från en OData-källa, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2491">If you are copying data from an OData source, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2492">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2492">Property</span></span> | <span data-ttu-id="3b691-2493">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2493">Description</span></span> | <span data-ttu-id="3b691-2494">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2494">Example</span></span> | <span data-ttu-id="3b691-2495">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2495">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2496">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-2496">query</span></span> |<span data-ttu-id="3b691-2497">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2497">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-2498">”? $select = namn, beskrivning och $top = 5”</span><span class="sxs-lookup"><span data-stu-id="3b691-2498">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="3b691-2499">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2499">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2500">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2500">Example</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

<span data-ttu-id="3b691-2501">Mer information finns i [OData connector](data-factory-odata-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2501">For more information, see [OData connector](data-factory-odata-connector.md#copy-activity-properties) article.</span></span>


## <a name="odbc"></a><span data-ttu-id="3b691-2502">ODBC</span><span class="sxs-lookup"><span data-stu-id="3b691-2502">ODBC</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-2503">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2503">Linked service</span></span>
<span data-ttu-id="3b691-2504">toodefine ODBC länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**OnPremisesOdbc**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2504">toodefine an ODBC linked service, set hello **type** of hello linked service too**OnPremisesOdbc**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2505">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2505">Property</span></span> | <span data-ttu-id="3b691-2506">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2506">Description</span></span> | <span data-ttu-id="3b691-2507">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2507">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2508">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-2508">connectionString</span></span> |<span data-ttu-id="3b691-2509">hello-access credential del av hello anslutningssträngen och en valfri krypteras autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3b691-2509">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="3b691-2510">Se exemplen i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2510">See examples in hello following sections.</span></span> |<span data-ttu-id="3b691-2511">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2511">Yes</span></span> |
| <span data-ttu-id="3b691-2512">autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="3b691-2512">credential</span></span> |<span data-ttu-id="3b691-2513">hello access credential delen av hello anslutningssträngen som angetts i drivrutinsspecifika egenskapsvärdet format.</span><span class="sxs-lookup"><span data-stu-id="3b691-2513">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="3b691-2514">Exempel ”: Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>”;.</span><span class="sxs-lookup"><span data-stu-id="3b691-2514">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="3b691-2515">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2515">No</span></span> |
| <span data-ttu-id="3b691-2516">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2516">authenticationType</span></span> |<span data-ttu-id="3b691-2517">Typ av autentisering används tooconnect toohello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-2517">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="3b691-2518">Möjliga värden är: anonyma och grundläggande.</span><span class="sxs-lookup"><span data-stu-id="3b691-2518">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="3b691-2519">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2519">Yes</span></span> |
| <span data-ttu-id="3b691-2520">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2520">username</span></span> |<span data-ttu-id="3b691-2521">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2521">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="3b691-2522">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2522">No</span></span> |
| <span data-ttu-id="3b691-2523">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2523">password</span></span> |<span data-ttu-id="3b691-2524">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2524">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-2525">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2525">No</span></span> |
| <span data-ttu-id="3b691-2526">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2526">gatewayName</span></span> |<span data-ttu-id="3b691-2527">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-2527">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="3b691-2528">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2528">Yes</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="3b691-2529">Exempel: använder grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2529">Example - Using Basic authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="3b691-2530">Exempel – med grundläggande autentisering och krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="3b691-2530">Example - Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="3b691-2531">Du kan kryptera hello autentiseringsuppgifterna med hjälp av hello [ny AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (version 1.0 av Azure PowerShell) cmdlet eller [ny AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 eller tidigare version av hello Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="3b691-2531">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3b691-2532">Exempel: Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2532">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3b691-2533">Mer information finns i [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2533">For more information, see [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-2534">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2534">Dataset</span></span>
<span data-ttu-id="3b691-2535">toodefine en ODBC-datauppsättning set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2535">toodefine an ODBC dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2536">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2536">Property</span></span> | <span data-ttu-id="3b691-2537">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2537">Description</span></span> | <span data-ttu-id="3b691-2538">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2538">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2539">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-2539">tableName</span></span> |<span data-ttu-id="3b691-2540">Namnet på hello tabell i hello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="3b691-2540">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="3b691-2541">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2541">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="3b691-2542">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2542">Example</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-2543">Mer information finns i [ODBC connector](data-factory-odbc-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2543">For more information, see [ODBC connector](data-factory-odbc-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-2544">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2544">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2545">Om du vill kopiera data från en ODBC-datalagret, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2545">If you are copying data from an ODBC data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2546">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2546">Property</span></span> | <span data-ttu-id="3b691-2547">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2547">Description</span></span> | <span data-ttu-id="3b691-2548">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2548">Allowed values</span></span> | <span data-ttu-id="3b691-2549">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2549">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2550">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-2550">query</span></span> |<span data-ttu-id="3b691-2551">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2551">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-2552">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3b691-2552">SQL query string.</span></span> <span data-ttu-id="3b691-2553">Till exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2553">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3b691-2554">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2554">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2555">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2555">Example</span></span>

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

<span data-ttu-id="3b691-2556">Mer information finns i [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2556">For more information, see [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) article.</span></span>

## <a name="salesforce"></a><span data-ttu-id="3b691-2557">Salesforce</span><span class="sxs-lookup"><span data-stu-id="3b691-2557">Salesforce</span></span>


### <a name="linked-service"></a><span data-ttu-id="3b691-2558">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2558">Linked service</span></span>
<span data-ttu-id="3b691-2559">toodefine en Salesforce länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Salesforce**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2559">toodefine a Salesforce linked service, set hello **type** of hello linked service too**Salesforce**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2560">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2560">Property</span></span> | <span data-ttu-id="3b691-2561">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2561">Description</span></span> | <span data-ttu-id="3b691-2562">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2562">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2563">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="3b691-2563">environmentUrl</span></span> | <span data-ttu-id="3b691-2564">Ange hello URL Salesforce-instans.</span><span class="sxs-lookup"><span data-stu-id="3b691-2564">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="3b691-2565">– Standardvärdet är ”https://login.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="3b691-2565">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="3b691-2566">-toocopy data från sandbox, ange ”https://test.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="3b691-2566">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="3b691-2567">-toocopy data från domän, till exempel ange ”https://[domain].my.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="3b691-2567">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="3b691-2568">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2568">No</span></span> |
| <span data-ttu-id="3b691-2569">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2569">username</span></span> |<span data-ttu-id="3b691-2570">Ange ett användarnamn för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3b691-2570">Specify a user name for hello user account.</span></span> |<span data-ttu-id="3b691-2571">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2571">Yes</span></span> |
| <span data-ttu-id="3b691-2572">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2572">password</span></span> |<span data-ttu-id="3b691-2573">Ange ett lösenord för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3b691-2573">Specify a password for hello user account.</span></span> |<span data-ttu-id="3b691-2574">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2574">Yes</span></span> |
| <span data-ttu-id="3b691-2575">securityToken</span><span class="sxs-lookup"><span data-stu-id="3b691-2575">securityToken</span></span> |<span data-ttu-id="3b691-2576">Ange en säkerhetstoken för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3b691-2576">Specify a security token for hello user account.</span></span> <span data-ttu-id="3b691-2577">Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta en säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="3b691-2577">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="3b691-2578">i allmänhet finns i toolearn om säkerhetstoken [säkerhet och hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="3b691-2578">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="3b691-2579">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2579">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2580">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2580">Example</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

<span data-ttu-id="3b691-2581">Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2581">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-2582">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2582">Dataset</span></span>
<span data-ttu-id="3b691-2583">toodefine en Salesforce-dataset set hello **typen** på hello datamängd för**RelationalTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2583">toodefine a Salesforce dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2584">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2584">Property</span></span> | <span data-ttu-id="3b691-2585">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2585">Description</span></span> | <span data-ttu-id="3b691-2586">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2586">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2587">tableName</span><span class="sxs-lookup"><span data-stu-id="3b691-2587">tableName</span></span> |<span data-ttu-id="3b691-2588">Namnet på hello tabell i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3b691-2588">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="3b691-2589">Nej (om en **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-2589">No (if a **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2590">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2590">Example</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3b691-2591">Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2591">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3b691-2592">Relationella källa i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2592">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2593">Om du vill kopiera data från Salesforce, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**RelationalSource**, och ange följande egenskaper i hello **källa** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2593">If you are copying data from Salesforce, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="3b691-2594">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2594">Property</span></span> | <span data-ttu-id="3b691-2595">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2595">Description</span></span> | <span data-ttu-id="3b691-2596">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3b691-2596">Allowed values</span></span> | <span data-ttu-id="3b691-2597">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2597">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b691-2598">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3b691-2598">query</span></span> |<span data-ttu-id="3b691-2599">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2599">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3b691-2600">En SQL-92-fråga eller [Salesforce objektet Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) frågan.</span><span class="sxs-lookup"><span data-stu-id="3b691-2600">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="3b691-2601">Till exempel: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="3b691-2601">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="3b691-2602">Nej (om hello **tableName** av hello **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="3b691-2602">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2603">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2603">Example</span></span>  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="3b691-2604">Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2604">hello "__c" part of hello API Name is needed for any custom object.</span></span>

<span data-ttu-id="3b691-2605">Mer information finns i [Salesforce-anslutningsprogrammet](data-factory-salesforce-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2605">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#copy-activity-properties) article.</span></span> 

## <a name="web-data"></a><span data-ttu-id="3b691-2606">Web-Data</span><span class="sxs-lookup"><span data-stu-id="3b691-2606">Web Data</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2607">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2607">Linked service</span></span>
<span data-ttu-id="3b691-2608">toodefine en webbplats länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**Web**, och ange följande egenskaper i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2608">toodefine a Web linked service, set hello **type** of hello linked service too**Web**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2609">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2609">Property</span></span> | <span data-ttu-id="3b691-2610">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2610">Description</span></span> | <span data-ttu-id="3b691-2611">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2611">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2612">URL</span><span class="sxs-lookup"><span data-stu-id="3b691-2612">Url</span></span> |<span data-ttu-id="3b691-2613">URL: en toohello webbadress</span><span class="sxs-lookup"><span data-stu-id="3b691-2613">URL toohello Web source</span></span> |<span data-ttu-id="3b691-2614">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2614">Yes</span></span> |
| <span data-ttu-id="3b691-2615">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3b691-2615">authenticationType</span></span> |<span data-ttu-id="3b691-2616">Anonym.</span><span class="sxs-lookup"><span data-stu-id="3b691-2616">Anonymous.</span></span> |<span data-ttu-id="3b691-2617">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2617">Yes</span></span> |
 

#### <a name="example"></a><span data-ttu-id="3b691-2618">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2618">Example</span></span>


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="3b691-2619">Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2619">For more information, see [Web Table connector](data-factory-web-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3b691-2620">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="3b691-2620">Dataset</span></span>
<span data-ttu-id="3b691-2621">toodefine datauppsättningars Web set hello **typen** på hello datamängd för**WebTable**, och ange följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2621">toodefine a Web dataset, set hello **type** of hello dataset too**WebTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="3b691-2622">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2622">Property</span></span> | <span data-ttu-id="3b691-2623">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2623">Description</span></span> | <span data-ttu-id="3b691-2624">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2624">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-2625">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2625">type</span></span> |<span data-ttu-id="3b691-2626">typ av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-2626">type of hello dataset.</span></span> <span data-ttu-id="3b691-2627">måste anges för**WebTable**</span><span class="sxs-lookup"><span data-stu-id="3b691-2627">must be set too**WebTable**</span></span> |<span data-ttu-id="3b691-2628">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2628">Yes</span></span> |
| <span data-ttu-id="3b691-2629">Sökväg</span><span class="sxs-lookup"><span data-stu-id="3b691-2629">path</span></span> |<span data-ttu-id="3b691-2630">En relativ URL toohello resurs som innehåller hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3b691-2630">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="3b691-2631">Nej.</span><span class="sxs-lookup"><span data-stu-id="3b691-2631">No.</span></span> <span data-ttu-id="3b691-2632">Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad.</span><span class="sxs-lookup"><span data-stu-id="3b691-2632">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="3b691-2633">Index</span><span class="sxs-lookup"><span data-stu-id="3b691-2633">index</span></span> |<span data-ttu-id="3b691-2634">hello index för hello tabellen i hello resurs.</span><span class="sxs-lookup"><span data-stu-id="3b691-2634">hello index of hello table in hello resource.</span></span> <span data-ttu-id="3b691-2635">Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg toogetting index för en tabell i en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="3b691-2635">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="3b691-2636">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2636">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3b691-2637">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2637">Example</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3b691-2638">Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#dataset-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2638">For more information, see [Web Table connector](data-factory-web-table-connector.md#dataset-properties) article.</span></span> 

### <a name="web-source-in-copy-activity"></a><span data-ttu-id="3b691-2639">Webbadress i en Kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2639">Web Source in Copy Activity</span></span>
<span data-ttu-id="3b691-2640">Om du kopierar data från en webbserver-tabell, ange hello **typ av datakälla** av hello kopieringsaktiviteten för**WebSource**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2640">If you are copying data from a web table, set hello **source type** of hello copy activity too**WebSource**.</span></span> <span data-ttu-id="3b691-2641">För närvarande när hello-källan i en Kopieringsaktivitet är av typen **WebSource**, inga ytterligare egenskaper som stöds.</span><span class="sxs-lookup"><span data-stu-id="3b691-2641">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>

#### <a name="example"></a><span data-ttu-id="3b691-2642">Exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2642">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3b691-2643">Mer information finns i [Webbtabell connector](data-factory-web-table-connector.md#copy-activity-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2643">For more information, see [Web Table connector](data-factory-web-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="compute-environments"></a><span data-ttu-id="3b691-2644">COMPUTE-MILJÖER</span><span class="sxs-lookup"><span data-stu-id="3b691-2644">COMPUTE ENVIRONMENTS</span></span>
<span data-ttu-id="3b691-2645">hello visar följande tabell hello beräkning miljöer som stöds av Data Factory och hello omvandling av aktiviteter som kan köras på dem.</span><span class="sxs-lookup"><span data-stu-id="3b691-2645">hello following table lists hello compute environments supported by Data Factory and hello transformation activities that can run on them.</span></span> <span data-ttu-id="3b691-2646">Klicka på hello länk hello beräkning som du är intresserad av toosee hello JSON-scheman för länkad tjänst toolink den tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-2646">Click hello link for hello compute you are interested in toosee hello JSON schemas for linked service toolink it tooa data factory.</span></span> 

| <span data-ttu-id="3b691-2647">Compute-miljö</span><span class="sxs-lookup"><span data-stu-id="3b691-2647">Compute environment</span></span> | <span data-ttu-id="3b691-2648">Aktiviteter</span><span class="sxs-lookup"><span data-stu-id="3b691-2648">Activities</span></span> |
| --- | --- |
| <span data-ttu-id="3b691-2649">[HDInsight-kluster på begäran](#on-demand-azure-hdinsight-cluster) eller [egna HDInsight-kluster](#existing-azure-hdinsight-cluster)</span><span class="sxs-lookup"><span data-stu-id="3b691-2649">[On-demand HDInsight cluster](#on-demand-azure-hdinsight-cluster) or [your own HDInsight cluster](#existing-azure-hdinsight-cluster)</span></span> |<span data-ttu-id="3b691-2650">[.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktivitet](#hdinsight-spark-activity)</span><span class="sxs-lookup"><span data-stu-id="3b691-2650">[.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity)</span></span> |
| [<span data-ttu-id="3b691-2651">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="3b691-2651">Azure Batch</span></span>](#azure-batch) |[<span data-ttu-id="3b691-2652">.NET-anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2652">.NET custom activity</span></span>](#net-custom-activity) |
| [<span data-ttu-id="3b691-2653">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3b691-2653">Azure Machine Learning</span></span>](#azure-machine-learning) | <span data-ttu-id="3b691-2654">[Maskininlärning Batchkörningsaktivitet](#machine-learning-batch-execution-activity), [Maskininlärning Uppdateringsresursaktivitet](#machine-learning-update-resource-activity)</span><span class="sxs-lookup"><span data-stu-id="3b691-2654">[Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity)</span></span> |
| [<span data-ttu-id="3b691-2655">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3b691-2655">Azure Data Lake Analytics</span></span>](#azure-data-lake-analytics) |[<span data-ttu-id="3b691-2656">Data Lake Analytics U-SQL</span><span class="sxs-lookup"><span data-stu-id="3b691-2656">Data Lake Analytics U-SQL</span></span>](#data-lake-analytics-u-sql-activity) |
| <span data-ttu-id="3b691-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQLServer](#sql-server-1)</span><span class="sxs-lookup"><span data-stu-id="3b691-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span></span> |[<span data-ttu-id="3b691-2658">Lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="3b691-2658">Stored Procedure</span></span>](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a><span data-ttu-id="3b691-2659">Azure HDInsight-kluster på begäran</span><span class="sxs-lookup"><span data-stu-id="3b691-2659">On-demand Azure HDInsight cluster</span></span>
<span data-ttu-id="3b691-2660">hello Azure Data Factory-tjänsten kan automatiskt skapa ett Windows/Linux-baserade på begäran HDInsight-kluster tooprocess data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2660">hello Azure Data Factory service can automatically create a Windows/Linux-based on-demand HDInsight cluster tooprocess data.</span></span> <span data-ttu-id="3b691-2661">hello klustret skapas i samma region som lagringskontot för hello (linkedServiceName-egenskapen i hello JSON) som är associerade med klustret hello hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2661">hello cluster is created in hello same region as hello storage account (linkedServiceName property in hello JSON) associated with hello cluster.</span></span> <span data-ttu-id="3b691-2662">Du kan köra hello efter omvandling aktiviteter på den här länkade tjänsten: [.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce activity ](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktiviteten](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="3b691-2662">You can run hello following transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2663">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2663">Linked service</span></span> 
<span data-ttu-id="3b691-2664">hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en länkad HDInsight på begäran-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2664">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an on-demand HDInsight linked service.</span></span>

| <span data-ttu-id="3b691-2665">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2665">Property</span></span> | <span data-ttu-id="3b691-2666">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2666">Description</span></span> | <span data-ttu-id="3b691-2667">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2667">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2668">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2668">type</span></span> |<span data-ttu-id="3b691-2669">hello typegenskapen ska anges för**HDInsightOnDemand**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2669">hello type property should be set too**HDInsightOnDemand**.</span></span> |<span data-ttu-id="3b691-2670">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2670">Yes</span></span> |
| <span data-ttu-id="3b691-2671">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="3b691-2671">clusterSize</span></span> |<span data-ttu-id="3b691-2672">Antalet worker/data noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2672">Number of worker/data nodes in hello cluster.</span></span> <span data-ttu-id="3b691-2673">Hej HDInsight-kluster skapas med 2 huvudnoderna tillsammans med hello antalet arbetarnoder som du anger för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2673">hello HDInsight cluster is created with 2 head nodes along with hello number of worker nodes you specify for this property.</span></span> <span data-ttu-id="3b691-2674">hello noder har storlek Standard_D3 med 4 kärnor, så ett kluster med noder 4 worker tar 24 kärnor (4\*4 = 16 kärnor för arbetarnoder plus 2\*4 = 8 kärnor för huvudnoderna).</span><span class="sxs-lookup"><span data-stu-id="3b691-2674">hello nodes are of size Standard_D3 that has 4 cores, so a 4 worker node cluster takes 24 cores (4\*4 = 16 cores for worker nodes, plus 2\*4 = 8 cores for head nodes).</span></span> <span data-ttu-id="3b691-2675">Se [skapa Linux-baserade Hadoop-kluster i HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) för ytterligare information om hello Standard_D3 nivå.</span><span class="sxs-lookup"><span data-stu-id="3b691-2675">See [Create Linux-based Hadoop clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) for details about hello Standard_D3 tier.</span></span> |<span data-ttu-id="3b691-2676">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2676">Yes</span></span> |
| <span data-ttu-id="3b691-2677">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="3b691-2677">timetolive</span></span> |<span data-ttu-id="3b691-2678">hello tillåten inaktivitetstid för hello på begäran HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2678">hello allowed idle time for hello on-demand HDInsight cluster.</span></span> <span data-ttu-id="3b691-2679">Anger hur länge hello på begäran HDInsight-kluster förblir aktiv efter slutförande av en aktivitet som kör om det finns inga aktiva jobb i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b691-2679">Specifies how long hello on-demand HDInsight cluster stays alive after completion of an activity run if there are no other active jobs in hello cluster.</span></span><br/><br/><span data-ttu-id="3b691-2680">Om en aktivitet som kör tar 6 minuter och timetolive är exempelvis ange too5 minuter hello kluster förblir alive för 5 minuter efter hello 6 minuter hello aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="3b691-2680">For example, if an activity run takes 6 minutes and timetolive is set too5 minutes, hello cluster stays alive for 5 minutes after hello 6 minutes of processing hello activity run.</span></span> <span data-ttu-id="3b691-2681">Om en annan aktivitet kör körs med hello 6 minuter fönster, men det bearbetas av hello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2681">If another activity run is executed with hello 6 minutes window, it is processed by hello same cluster.</span></span><br/><br/><span data-ttu-id="3b691-2682">Skapar ett HDInsight-kluster på begäran är en kostsam åtgärd (kan ta en stund), så Använd den här inställningen som behövs tooimprove prestanda för en datafabrik genom att återanvända ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="3b691-2682">Creating an on-demand HDInsight cluster is an expensive operation (could take a while), so use this setting as needed tooimprove performance of a data factory by reusing an on-demand HDInsight cluster.</span></span><br/><br/><span data-ttu-id="3b691-2683">Om du ställer in timetolive värdet too0 tas hello klustret bort när hello aktivitet köras i bearbetade.</span><span class="sxs-lookup"><span data-stu-id="3b691-2683">If you set timetolive value too0, hello cluster is deleted as soon as hello activity run in processed.</span></span> <span data-ttu-id="3b691-2684">På hello däremot om du anger ett högt värde hello klustret kan förblir inaktiva i onödan ledde höga kostnader.</span><span class="sxs-lookup"><span data-stu-id="3b691-2684">On hello other hand, if you set a high value, hello cluster may stay idle unnecessarily resulting in high costs.</span></span> <span data-ttu-id="3b691-2685">Det är därför viktigt att du ställer in hello lämpligt värde baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="3b691-2685">Therefore, it is important that you set hello appropriate value based on your needs.</span></span><br/><br/><span data-ttu-id="3b691-2686">Flera pipelines kan dela hello samma instans av HDInsight-kluster för hello på begäran om hello timetolive egenskapens värde är korrekt</span><span class="sxs-lookup"><span data-stu-id="3b691-2686">Multiple pipelines can share hello same instance of hello on-demand HDInsight cluster if hello timetolive property value is appropriately set</span></span> |<span data-ttu-id="3b691-2687">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2687">Yes</span></span> |
| <span data-ttu-id="3b691-2688">Version</span><span class="sxs-lookup"><span data-stu-id="3b691-2688">version</span></span> |<span data-ttu-id="3b691-2689">Version av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2689">Version of hello HDInsight cluster.</span></span> <span data-ttu-id="3b691-2690">Mer information finns i [HDInsight-versioner som stöds i Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3b691-2690">For details, see [supported HDInsight versions in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> |<span data-ttu-id="3b691-2691">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2691">No</span></span> |
| <span data-ttu-id="3b691-2692">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3b691-2692">linkedServiceName</span></span> |<span data-ttu-id="3b691-2693">Azure Storage länkade tjänsten toobe som används av hello på begäran klustret för lagring och bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="3b691-2693">Azure Storage linked service toobe used by hello on-demand cluster for storing and processing data.</span></span> <p><span data-ttu-id="3b691-2694">För närvarande kan du skapa ett HDInsight-kluster med på begäran som använder ett Azure Data Lake Store som hello lagring.</span><span class="sxs-lookup"><span data-stu-id="3b691-2694">Currently, you cannot create an on-demand HDInsight cluster that uses an Azure Data Lake Store as hello storage.</span></span> <span data-ttu-id="3b691-2695">Om du vill toostore hello Resultatdata från HDInsight som bearbetas i en Azure Data Lake Store kan använda en Kopieringsaktiviteten toocopy hello data från hello Azure Blob Storage toohello Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b691-2695">If you want toostore hello result data from HDInsight processing in an Azure Data Lake Store, use a Copy Activity toocopy hello data from hello Azure Blob Storage toohello Azure Data Lake Store.</span></span></p>  | <span data-ttu-id="3b691-2696">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2696">Yes</span></span> |
| <span data-ttu-id="3b691-2697">additionalLinkedServiceNames</span><span class="sxs-lookup"><span data-stu-id="3b691-2697">additionalLinkedServiceNames</span></span> |<span data-ttu-id="3b691-2698">Anger ytterligare lagringskonton för hello HDInsight länkade tjänsten så att hello Data Factory-tjänsten kan registrera dem å dina vägnar.</span><span class="sxs-lookup"><span data-stu-id="3b691-2698">Specifies additional storage accounts for hello HDInsight linked service so that hello Data Factory service can register them on your behalf.</span></span> |<span data-ttu-id="3b691-2699">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2699">No</span></span> |
| <span data-ttu-id="3b691-2700">osType</span><span class="sxs-lookup"><span data-stu-id="3b691-2700">osType</span></span> |<span data-ttu-id="3b691-2701">Typ av operativsystem.</span><span class="sxs-lookup"><span data-stu-id="3b691-2701">Type of operating system.</span></span> <span data-ttu-id="3b691-2702">Tillåtna värden är: (standard) för Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="3b691-2702">Allowed values are: Windows (default) and Linux</span></span> |<span data-ttu-id="3b691-2703">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2703">No</span></span> |
| <span data-ttu-id="3b691-2704">hcatalogLinkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3b691-2704">hcatalogLinkedServiceName</span></span> |<span data-ttu-id="3b691-2705">hello namnet på Azure SQL-länkade tjänsten punkt toohello HCatalog databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2705">hello name of Azure SQL linked service that point toohello HCatalog database.</span></span> <span data-ttu-id="3b691-2706">hello på begäran HDInsight-kluster skapas med hjälp av hello Azure SQL-databas som hello metastore.</span><span class="sxs-lookup"><span data-stu-id="3b691-2706">hello on-demand HDInsight cluster is created by using hello Azure SQL database as hello metastore.</span></span> |<span data-ttu-id="3b691-2707">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2707">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3b691-2708">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2708">JSON example</span></span>
<span data-ttu-id="3b691-2709">hello följande JSON definierar en Linux-baserade på begäran HDInsight länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2709">hello following JSON defines a Linux-based on-demand HDInsight linked service.</span></span> <span data-ttu-id="3b691-2710">hello Data Factory-tjänsten skapar automatiskt en **Linux-baserade** vid bearbetning av en datasektorn HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2710">hello Data Factory service automatically creates a **Linux-based** HDInsight cluster when processing a data slice.</span></span> 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

<span data-ttu-id="3b691-2711">Mer information finns i [Compute länkade tjänster](data-factory-compute-linked-services.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2711">For more information, see [Compute linked services](data-factory-compute-linked-services.md) article.</span></span> 

## <a name="existing-azure-hdinsight-cluster"></a><span data-ttu-id="3b691-2712">Befintligt Azure HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="3b691-2712">Existing Azure HDInsight cluster</span></span>
<span data-ttu-id="3b691-2713">Du kan skapa ett Azure HDInsight länkade tjänsten tooregister ditt eget kluster i HDInsight med Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-2713">You can create an Azure HDInsight linked service tooregister your own HDInsight cluster with Data Factory.</span></span> <span data-ttu-id="3b691-2714">Du kan köra hello följa data transformation aktiviteter på den här länkade tjänsten: [.NET anpassad aktivitet](#net-custom-activity), [Hive aktiviteten](#hdinsight-hive-activity), [svin aktivitet] (#hdinsight-pig-aktivitet, [MapReduce aktiviteten](#hdinsight-mapreduce-activity), [Hadoop-strömning aktiviteten](#hdinsight-streaming-activityd), [Väck aktiviteten](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="3b691-2714">You can run hello following data transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2715">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2715">Linked service</span></span>
<span data-ttu-id="3b691-2716">hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure HDInsight länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2716">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure HDInsight linked service.</span></span>

| <span data-ttu-id="3b691-2717">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2717">Property</span></span> | <span data-ttu-id="3b691-2718">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2718">Description</span></span> | <span data-ttu-id="3b691-2719">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2719">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2720">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2720">type</span></span> |<span data-ttu-id="3b691-2721">hello typegenskapen ska anges för**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2721">hello type property should be set too**HDInsight**.</span></span> |<span data-ttu-id="3b691-2722">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2722">Yes</span></span> |
| <span data-ttu-id="3b691-2723">clusterUri</span><span class="sxs-lookup"><span data-stu-id="3b691-2723">clusterUri</span></span> |<span data-ttu-id="3b691-2724">hello hello HDInsight-kluster-URI.</span><span class="sxs-lookup"><span data-stu-id="3b691-2724">hello URI of hello HDInsight cluster.</span></span> |<span data-ttu-id="3b691-2725">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2725">Yes</span></span> |
| <span data-ttu-id="3b691-2726">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2726">username</span></span> |<span data-ttu-id="3b691-2727">Ange hello namnet hello användaren toobe används tooconnect tooan befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2727">Specify hello name of hello user toobe used tooconnect tooan existing HDInsight cluster.</span></span> |<span data-ttu-id="3b691-2728">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2728">Yes</span></span> |
| <span data-ttu-id="3b691-2729">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2729">password</span></span> |<span data-ttu-id="3b691-2730">Ange lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2730">Specify password for hello user account.</span></span> |<span data-ttu-id="3b691-2731">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2731">Yes</span></span> |
| <span data-ttu-id="3b691-2732">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3b691-2732">linkedServiceName</span></span> | <span data-ttu-id="3b691-2733">Namnet på hello länkad Azure Storage-tjänst som refererar toohello Azure blob-lagring som används av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2733">Name of hello Azure Storage linked service that refers toohello Azure blob storage used by hello HDInsight cluster.</span></span> <p><span data-ttu-id="3b691-2734">För närvarande kan du ange ett Azure Data Lake Store länkade tjänsten för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2734">Currently, you cannot specify an Azure Data Lake Store linked service for this property.</span></span> <span data-ttu-id="3b691-2735">Du kan komma åt data i hello Azure Data Lake Store från Hive/Pig-skript om hello HDInsight-kluster har åtkomst toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3b691-2735">You may access data in hello Azure Data Lake Store from Hive/Pig scripts if hello HDInsight cluster has access toohello Data Lake Store.</span></span> </p>  |<span data-ttu-id="3b691-2736">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2736">Yes</span></span> |

<span data-ttu-id="3b691-2737">Versioner av HDInsight-kluster som stöds finns [HDInsight-versioner som stöds](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3b691-2737">For versions of HDInsight clusters supported, see [supported HDInsight versions](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> 

#### <a name="json-example"></a><span data-ttu-id="3b691-2738">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2738">JSON example</span></span>

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a><span data-ttu-id="3b691-2739">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="3b691-2739">Azure Batch</span></span>
<span data-ttu-id="3b691-2740">Du kan skapa ett Azure Batch länkade tjänsten tooregister Batch-pool för virtuella datorer (VM) med en data factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-2740">You can create an Azure Batch linked service tooregister a Batch pool of virtual machines (VMs) with a data factory.</span></span> <span data-ttu-id="3b691-2741">Du kan köra .NET anpassade aktiviteter med hjälp av Azure Batch eller Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3b691-2741">You can run .NET custom activities using either Azure Batch or Azure HDInsight.</span></span> <span data-ttu-id="3b691-2742">Du kan köra en [.NET anpassad aktivitet](#net-custom-activity) på den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2742">You can run a [.NET custom activity](#net-custom-activity) on this linked service.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2743">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2743">Linked service</span></span>
<span data-ttu-id="3b691-2744">hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure Batch länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2744">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Batch linked service.</span></span>

| <span data-ttu-id="3b691-2745">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2745">Property</span></span> | <span data-ttu-id="3b691-2746">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2746">Description</span></span> | <span data-ttu-id="3b691-2747">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2747">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2748">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2748">type</span></span> |<span data-ttu-id="3b691-2749">hello typegenskapen ska anges för**AzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2749">hello type property should be set too**AzureBatch**.</span></span> |<span data-ttu-id="3b691-2750">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2750">Yes</span></span> |
| <span data-ttu-id="3b691-2751">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2751">accountName</span></span> |<span data-ttu-id="3b691-2752">Namnet på hello Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="3b691-2752">Name of hello Azure Batch account.</span></span> |<span data-ttu-id="3b691-2753">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2753">Yes</span></span> |
| <span data-ttu-id="3b691-2754">accessKey</span><span class="sxs-lookup"><span data-stu-id="3b691-2754">accessKey</span></span> |<span data-ttu-id="3b691-2755">Åtkomstnyckeln för hello Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="3b691-2755">Access key for hello Azure Batch account.</span></span> |<span data-ttu-id="3b691-2756">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2756">Yes</span></span> |
| <span data-ttu-id="3b691-2757">Poolnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2757">poolName</span></span> |<span data-ttu-id="3b691-2758">Namnet på hello pool för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2758">Name of hello pool of virtual machines.</span></span> |<span data-ttu-id="3b691-2759">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2759">Yes</span></span> |
| <span data-ttu-id="3b691-2760">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3b691-2760">linkedServiceName</span></span> |<span data-ttu-id="3b691-2761">Namnet på hello länkad Azure Storage-tjänst som är associerad med den här Azure Batch länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2761">Name of hello Azure Storage linked service associated with this Azure Batch linked service.</span></span> <span data-ttu-id="3b691-2762">Den här länkade tjänsten används för mellanlagring av filer krävs toorun hello aktivitet och lagra hello aktivitetsloggar för körning.</span><span class="sxs-lookup"><span data-stu-id="3b691-2762">This linked service is used for staging files required toorun hello activity and storing hello activity execution logs.</span></span> |<span data-ttu-id="3b691-2763">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2763">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="3b691-2764">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2764">JSON example</span></span>

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a><span data-ttu-id="3b691-2765">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3b691-2765">Azure Machine Learning</span></span>
<span data-ttu-id="3b691-2766">Skapa en Azure Machine Learning länkade tjänsten tooregister en Machine Learning-batch bedömningsslutpunkten med en data factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-2766">You create an Azure Machine Learning linked service tooregister a Machine Learning batch scoring endpoint with a data factory.</span></span> <span data-ttu-id="3b691-2767">Två data transformation aktiviteter som kan köras på den här länkade tjänsten: [Machine Learning-Batchkörningsaktivitet](#machine-learning-batch-execution-activity), [Machine Learning-Uppdateringsresursaktivitet](#machine-learning-update-resource-activity).</span><span class="sxs-lookup"><span data-stu-id="3b691-2767">Two data transformation activities that can run on this linked service: [Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2768">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2768">Linked service</span></span>
<span data-ttu-id="3b691-2769">hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello Azure JSON-definitionen för en Azure Machine Learning länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2769">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Machine Learning linked service.</span></span>

| <span data-ttu-id="3b691-2770">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2770">Property</span></span> | <span data-ttu-id="3b691-2771">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2771">Description</span></span> | <span data-ttu-id="3b691-2772">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2772">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2773">Typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2773">Type</span></span> |<span data-ttu-id="3b691-2774">hello typegenskapen ska anges till: **AzureML**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2774">hello type property should be set to: **AzureML**.</span></span> |<span data-ttu-id="3b691-2775">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2775">Yes</span></span> |
| <span data-ttu-id="3b691-2776">mlEndpoint</span><span class="sxs-lookup"><span data-stu-id="3b691-2776">mlEndpoint</span></span> |<span data-ttu-id="3b691-2777">Hej batchbedömningsjobbet URL.</span><span class="sxs-lookup"><span data-stu-id="3b691-2777">hello batch scoring URL.</span></span> |<span data-ttu-id="3b691-2778">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2778">Yes</span></span> |
| <span data-ttu-id="3b691-2779">apiKey</span><span class="sxs-lookup"><span data-stu-id="3b691-2779">apiKey</span></span> |<span data-ttu-id="3b691-2780">hello publicerade arbetsytemodellens API.</span><span class="sxs-lookup"><span data-stu-id="3b691-2780">hello published workspace model’s API.</span></span> |<span data-ttu-id="3b691-2781">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2781">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3b691-2782">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2782">JSON example</span></span>

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a><span data-ttu-id="3b691-2783">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3b691-2783">Azure Data Lake Analytics</span></span>
<span data-ttu-id="3b691-2784">Du skapar en **Azure Data Lake Analytics** länkade tjänsten toolink ett Azure Data Lake Analytics beräkning service tooan Azure data factory innan du använder hello [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md) i en pipeline .</span><span class="sxs-lookup"><span data-stu-id="3b691-2784">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory before using hello [Data Lake Analytics U-SQL activity](data-factory-usql-activity.md) in a pipeline.</span></span>

### <a name="linked-service"></a><span data-ttu-id="3b691-2785">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2785">Linked service</span></span>

<span data-ttu-id="3b691-2786">hello i den följande tabellen finns beskrivningar hello egenskaper som används i hello JSON-definitionen för en länkad Azure Data Lake Analytics-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2786">hello following table provides descriptions for hello properties used in hello JSON definition of an Azure Data Lake Analytics linked service.</span></span> 

| <span data-ttu-id="3b691-2787">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2787">Property</span></span> | <span data-ttu-id="3b691-2788">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2788">Description</span></span> | <span data-ttu-id="3b691-2789">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2789">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2790">Typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2790">Type</span></span> |<span data-ttu-id="3b691-2791">hello typegenskapen ska anges till: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2791">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="3b691-2792">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2792">Yes</span></span> |
| <span data-ttu-id="3b691-2793">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2793">accountName</span></span> |<span data-ttu-id="3b691-2794">Azure Data Lake Analytics-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2794">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="3b691-2795">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2795">Yes</span></span> |
| <span data-ttu-id="3b691-2796">dataLakeAnalyticsUri</span><span class="sxs-lookup"><span data-stu-id="3b691-2796">dataLakeAnalyticsUri</span></span> |<span data-ttu-id="3b691-2797">Azure Data Lake Analytics-URI.</span><span class="sxs-lookup"><span data-stu-id="3b691-2797">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="3b691-2798">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2798">No</span></span> |
| <span data-ttu-id="3b691-2799">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2799">authorization</span></span> |<span data-ttu-id="3b691-2800">Auktoriseringskoden hämtas automatiskt när du klickar på **auktorisera** knappen i hello Data Factory-redigeraren och slutför hello OAuth-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2800">Authorization code is automatically retrieved after clicking **Authorize** button in hello Data Factory Editor and completing hello OAuth login.</span></span> |<span data-ttu-id="3b691-2801">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2801">Yes</span></span> |
| <span data-ttu-id="3b691-2802">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="3b691-2802">subscriptionId</span></span> |<span data-ttu-id="3b691-2803">Azure prenumerations-id</span><span class="sxs-lookup"><span data-stu-id="3b691-2803">Azure subscription id</span></span> |<span data-ttu-id="3b691-2804">Nej (om inte anges prenumeration hello datafabriken används).</span><span class="sxs-lookup"><span data-stu-id="3b691-2804">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="3b691-2805">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="3b691-2805">resourceGroupName</span></span> |<span data-ttu-id="3b691-2806">Azure resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="3b691-2806">Azure resource group name</span></span> |<span data-ttu-id="3b691-2807">Nej (om inte anges resursgruppen av hello datafabriken används).</span><span class="sxs-lookup"><span data-stu-id="3b691-2807">No (If not specified, resource group of hello data factory is used).</span></span> |
| <span data-ttu-id="3b691-2808">Sessions-ID</span><span class="sxs-lookup"><span data-stu-id="3b691-2808">sessionId</span></span> |<span data-ttu-id="3b691-2809">sessions-id från hello OAuth-auktorisering session.</span><span class="sxs-lookup"><span data-stu-id="3b691-2809">session id from hello OAuth authorization session.</span></span> <span data-ttu-id="3b691-2810">Varje sessions-id är unikt och får endast användas en gång.</span><span class="sxs-lookup"><span data-stu-id="3b691-2810">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="3b691-2811">När du använder hello Data Factory-redigeraren genereras detta ID automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2811">When you use hello Data Factory Editor, this ID is auto-generated.</span></span> |<span data-ttu-id="3b691-2812">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2812">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="3b691-2813">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2813">JSON example</span></span>
<span data-ttu-id="3b691-2814">hello följande exempel innehåller JSON-definitionen för en länkad Azure Data Lake Analytics-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b691-2814">hello following example provides JSON definition for an Azure Data Lake Analytics linked service.</span></span>

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a><span data-ttu-id="3b691-2815">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3b691-2815">Azure SQL Database</span></span>
<span data-ttu-id="3b691-2816">Du skapar en Azure SQL-länkade tjänst och använda den med hello [lagrade Proceduraktiviteten](#stored-procedure-activity) tooinvoke en lagrad procedur från Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2816">You create an Azure SQL linked service and use it with hello [Stored Procedure Activity](#stored-procedure-activity) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2817">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2817">Linked service</span></span>
<span data-ttu-id="3b691-2818">toodefine en Azure SQL Database länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDatabase**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2818">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2819">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2819">Property</span></span> | <span data-ttu-id="3b691-2820">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2820">Description</span></span> | <span data-ttu-id="3b691-2821">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2821">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2822">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-2822">connectionString</span></span> |<span data-ttu-id="3b691-2823">Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-2823">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="3b691-2824">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2824">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3b691-2825">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2825">JSON example</span></span>

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3b691-2826">Se [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) artikeln för information om den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2826">See [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) article for details about this linked service.</span></span>

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="3b691-2827">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-2827">Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3b691-2828">Du skapar en länkad Azure SQL Data Warehouse-tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2828">You create an Azure SQL Data Warehouse linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2829">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2829">Linked service</span></span>
<span data-ttu-id="3b691-2830">toodefine ett Azure SQL Data Warehouse länkade tjänsten, ange hello **typen** av hello länkade tjänsten för**AzureSqlDW**, och ange följande egenskaper i hello **typeProperties**avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3b691-2830">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="3b691-2831">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2831">Property</span></span> | <span data-ttu-id="3b691-2832">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2832">Description</span></span> | <span data-ttu-id="3b691-2833">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2833">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2834">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-2834">connectionString</span></span> |<span data-ttu-id="3b691-2835">Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3b691-2835">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="3b691-2836">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2836">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3b691-2837">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2837">JSON example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3b691-2838">Mer information finns i [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2838">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

## <a name="sql-server"></a><span data-ttu-id="3b691-2839">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3b691-2839">SQL Server</span></span> 
<span data-ttu-id="3b691-2840">Du skapar en SQL Server som är länkad tjänst och använda den med hello [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) tooinvoke en lagrad procedur från Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2840">You create a SQL Server linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3b691-2841">Länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3b691-2841">Linked service</span></span>
<span data-ttu-id="3b691-2842">Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2842">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="3b691-2843">hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2843">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="3b691-2844">hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-2844">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="3b691-2845">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2845">Property</span></span> | <span data-ttu-id="3b691-2846">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2846">Description</span></span> | <span data-ttu-id="3b691-2847">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2847">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2848">typ</span><span class="sxs-lookup"><span data-stu-id="3b691-2848">type</span></span> |<span data-ttu-id="3b691-2849">hello typegenskapen ska anges till: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2849">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="3b691-2850">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2850">Yes</span></span> |
| <span data-ttu-id="3b691-2851">connectionString</span><span class="sxs-lookup"><span data-stu-id="3b691-2851">connectionString</span></span> |<span data-ttu-id="3b691-2852">Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2852">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="3b691-2853">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2853">Yes</span></span> |
| <span data-ttu-id="3b691-2854">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3b691-2854">gatewayName</span></span> |<span data-ttu-id="3b691-2855">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-2855">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="3b691-2856">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2856">Yes</span></span> |
| <span data-ttu-id="3b691-2857">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2857">username</span></span> |<span data-ttu-id="3b691-2858">Ange användarnamnet om du använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3b691-2858">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="3b691-2859">Exempel: **domainname\\användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2859">Example: **domainname\\username**.</span></span> |<span data-ttu-id="3b691-2860">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2860">No</span></span> |
| <span data-ttu-id="3b691-2861">lösenord</span><span class="sxs-lookup"><span data-stu-id="3b691-2861">password</span></span> |<span data-ttu-id="3b691-2862">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3b691-2862">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="3b691-2863">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2863">No</span></span> |

<span data-ttu-id="3b691-2864">Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):</span><span class="sxs-lookup"><span data-stu-id="3b691-2864">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="3b691-2865">Exempel: JSON för att använda SQL-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2865">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="3b691-2866">Exempel: JSON för att använda Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b691-2866">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="3b691-2867">Om användarnamn och lösenord anges gateway använder dem tooimpersonate hello användaren konto tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="3b691-2867">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="3b691-2868">Gatewayen ansluter annars toohello SQL Server direkt med hello säkerhetskontexten för Gateway (dess start-konto).</span><span class="sxs-lookup"><span data-stu-id="3b691-2868">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3b691-2869">Mer information finns i [SQL Server-anslutningen](data-factory-sqlserver-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2869">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span>

## <a name="data-transformation-activities"></a><span data-ttu-id="3b691-2870">DATA TRANSFORMATION AKTIVITETER</span><span class="sxs-lookup"><span data-stu-id="3b691-2870">DATA TRANSFORMATION ACTIVITIES</span></span>

<span data-ttu-id="3b691-2871">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2871">Activity</span></span> | <span data-ttu-id="3b691-2872">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2872">Description</span></span>
-------- | -----------
[<span data-ttu-id="3b691-2873">HDInsight Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2873">HDInsight Hive activity</span></span>](#hdinsight-hive-activity) | <span data-ttu-id="3b691-2874">Hej HDInsight Hive aktivitet i en Data Factory-pipelinen kör Hive-frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2874">hello HDInsight Hive activity in a Data Factory pipeline executes Hive queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span> 
[<span data-ttu-id="3b691-2875">HDInsight Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2875">HDInsight Pig activity</span></span>](#hdinsight-pig-activity) | <span data-ttu-id="3b691-2876">Hej HDInsight Pig aktivitet i en Data Factory-pipelinen kör Pig frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2876">hello HDInsight Pig activity in a Data Factory pipeline executes Pig queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3b691-2877">HDInsight MapReduce-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2877">HDInsight MapReduce Activity</span></span>](#hdinsight-mapreduce-activity) | <span data-ttu-id="3b691-2878">Hej HDInsight MapReduce aktivitet i en Data Factory-pipelinen kör MapReduce program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2878">hello HDInsight MapReduce activity in a Data Factory pipeline executes MapReduce programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3b691-2879">HDInsight-strömningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2879">HDInsight Streaming Activity</span></span>](#hdinsight-streaming-activity) | <span data-ttu-id="3b691-2880">Hej HDInsight Streaming Activity i Data Factory-pipelinen kör Hadoop Streaming program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2880">hello HDInsight Streaming Activity in a Data Factory pipeline executes Hadoop Streaming programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3b691-2881">HDInsight Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2881">HDInsight Spark Activity</span></span>](#hdinsight-spark-activity) | <span data-ttu-id="3b691-2882">hello HDInsight Spark-aktivitet i en Data Factory-pipelinen körs Spark-program på din egen HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2882">hello HDInsight Spark activity in a Data Factory pipeline executes Spark programs on your own HDInsight cluster.</span></span> 
[<span data-ttu-id="3b691-2883">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2883">Machine Learning Batch Execution Activity</span></span>](#machine-learning-batch-execution-activity) | <span data-ttu-id="3b691-2884">Azure Data Factory aktiverar du tooeasily skapa pipelines som använder en publicerad Azure Machine Learning-webbtjänsten för förutsägelseanalys.</span><span class="sxs-lookup"><span data-stu-id="3b691-2884">Azure Data Factory enables you tooeasily create pipelines that use a published Azure Machine Learning web service for predictive analytics.</span></span> <span data-ttu-id="3b691-2885">Du kan anropa en Machine Learning web service toomake förutsägelser på hello data i batch med hello-Batchkörningsaktivitet i ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2885">Using hello Batch Execution Activity in an Azure Data Factory pipeline, you can invoke a Machine Learning web service toomake predictions on hello data in batch.</span></span> 
[<span data-ttu-id="3b691-2886">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2886">Machine Learning Update Resource Activity</span></span>](#machine-learning-update-resource-activity) | <span data-ttu-id="3b691-2887">Över tiden ange hello förutsägelsemodeller i hello Machine Learning bedömningsprofil experiment måste toobe retrained med nya datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="3b691-2887">Over time, hello predictive models in hello Machine Learning scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="3b691-2888">När du är klar med omtränings vill du tooupdate hello bedömningen webbtjänst med hello retrained Machine Learning-modellen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2888">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained Machine Learning model.</span></span> <span data-ttu-id="3b691-2889">Du kan använda hello Uppdateringsresursaktivitet tooupdate hello-webbtjänsten med hello nyligen tränats modell.</span><span class="sxs-lookup"><span data-stu-id="3b691-2889">You can use hello Update Resource Activity tooupdate hello web service with hello newly trained model.</span></span>
[<span data-ttu-id="3b691-2890">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2890">Stored Procedure Activity</span></span>](#stored-procedure-activity) | <span data-ttu-id="3b691-2891">Du kan använda hello lagrade proceduren aktivitet i en Data Factory-pipelinen tooinvoke en lagrad procedur i någon av följande datalager hello: Azure SQL Database, Azure SQL Data Warehouse, SQL Server-databas i ditt företag eller en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="3b691-2891">You can use hello Stored Procedure activity in a Data Factory pipeline tooinvoke a stored procedure in one of hello following data stores: Azure SQL Database, Azure SQL Data Warehouse, SQL Server Database in your enterprise or an Azure VM.</span></span> 
[<span data-ttu-id="3b691-2892">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2892">Data Lake Analytics U-SQL activity</span></span>](#data-lake-analytics-u-sql-activity) | <span data-ttu-id="3b691-2893">Data Lake Analytics U-SQL-aktivitet körs ett U-SQL-skript i ett Azure Data Lake Analytics-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2893">Data Lake Analytics U-SQL Activity runs a U-SQL script on an Azure Data Lake Analytics cluster.</span></span>  
[<span data-ttu-id="3b691-2894">.NET-anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2894">.NET custom activity</span></span>](#net-custom-activity) | <span data-ttu-id="3b691-2895">Om du behöver tootransform data på ett sätt som inte stöds av Data Factory kan du skapa en anpassad aktivitet med din egen databearbetning logik och använder hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-2895">If you need tootransform data in a way that is not supported by Data Factory, you can create a custom activity with your own data processing logic and use hello activity in hello pipeline.</span></span> <span data-ttu-id="3b691-2896">Du kan konfigurera hello anpassade .NET aktiviteten toorun med hjälp av Azure Batch-tjänsten eller ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-2896">You can configure hello custom .NET activity toorun using either an Azure Batch service or an Azure HDInsight cluster.</span></span> 

     
## <a name="hdinsight-hive-activity"></a><span data-ttu-id="3b691-2897">HDInsight Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2897">HDInsight Hive Activity</span></span>
<span data-ttu-id="3b691-2898">Du kan ange hello följande egenskaper i en definition av Hive aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="3b691-2898">You can specify hello following properties in a Hive Activity JSON definition.</span></span> <span data-ttu-id="3b691-2899">hello typegenskapen för hello aktiviteten måste vara: **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2899">hello type property for hello activity must be: **HDInsightHive**.</span></span> <span data-ttu-id="3b691-2900">Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2900">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-2901">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightHive:</span><span class="sxs-lookup"><span data-stu-id="3b691-2901">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightHive:</span></span>

| <span data-ttu-id="3b691-2902">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2902">Property</span></span> | <span data-ttu-id="3b691-2903">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2903">Description</span></span> | <span data-ttu-id="3b691-2904">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2904">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2905">Skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-2905">script</span></span> |<span data-ttu-id="3b691-2906">Ange hello Hive-skript infogade</span><span class="sxs-lookup"><span data-stu-id="3b691-2906">Specify hello Hive script inline</span></span> |<span data-ttu-id="3b691-2907">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2907">No</span></span> |
| <span data-ttu-id="3b691-2908">sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-2908">script path</span></span> |<span data-ttu-id="3b691-2909">Store hello registreringsdatafilen i ett Azure blob storage och ange hello sökväg toohello fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-2909">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="3b691-2910">Använd egenskapen 'script' eller 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="3b691-2910">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="3b691-2911">Båda kan inte användas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="3b691-2911">Both cannot be used together.</span></span> <span data-ttu-id="3b691-2912">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="3b691-2912">hello file name is case-sensitive.</span></span> |<span data-ttu-id="3b691-2913">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2913">No</span></span> |
| <span data-ttu-id="3b691-2914">definierar</span><span class="sxs-lookup"><span data-stu-id="3b691-2914">defines</span></span> |<span data-ttu-id="3b691-2915">Ange parametrar som nyckel/värde-par för refererar till i hello Hive-skript med hjälp av 'hiveconf'</span><span class="sxs-lookup"><span data-stu-id="3b691-2915">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="3b691-2916">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2916">No</span></span> |

<span data-ttu-id="3b691-2917">Dessa egenskaper finns specifika toohello Hive aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-2917">These type properties are specific toohello Hive Activity.</span></span> <span data-ttu-id="3b691-2918">Andra egenskaper (utanför hello typeProperties avsnittet) stöds för alla aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3b691-2918">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="3b691-2919">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2919">JSON example</span></span>
<span data-ttu-id="3b691-2920">hello följande JSON definierar en HDInsight Hive-aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-2920">hello following JSON defines a HDInsight Hive activity in a pipeline.</span></span>  

```json
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

<span data-ttu-id="3b691-2921">Mer information finns i [Hive aktiviteten](data-factory-hive-activity.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2921">For more information, see [Hive Activity](data-factory-hive-activity.md) article.</span></span> 

## <a name="hdinsight-pig-activity"></a><span data-ttu-id="3b691-2922">HDInsight-piggningsåtgärd</span><span class="sxs-lookup"><span data-stu-id="3b691-2922">HDInsight Pig Activity</span></span>
<span data-ttu-id="3b691-2923">Du kan ange följande egenskaper i en definition av Pig aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2923">You can specify hello following properties in a Pig Activity JSON definition.</span></span> <span data-ttu-id="3b691-2924">hello typegenskapen för hello aktiviteten måste vara: **HDInsightPig**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2924">hello type property for hello activity must be: **HDInsightPig**.</span></span> <span data-ttu-id="3b691-2925">Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2925">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-2926">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightPig:</span><span class="sxs-lookup"><span data-stu-id="3b691-2926">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightPig:</span></span> 

| <span data-ttu-id="3b691-2927">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2927">Property</span></span> | <span data-ttu-id="3b691-2928">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2928">Description</span></span> | <span data-ttu-id="3b691-2929">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2929">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2930">Skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-2930">script</span></span> |<span data-ttu-id="3b691-2931">Ange hello Pig-skriptet infogade</span><span class="sxs-lookup"><span data-stu-id="3b691-2931">Specify hello Pig script inline</span></span> |<span data-ttu-id="3b691-2932">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2932">No</span></span> |
| <span data-ttu-id="3b691-2933">sökvägen för skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-2933">script path</span></span> |<span data-ttu-id="3b691-2934">Lagra hello Pig-skriptet i en Azure blob storage och ange hello sökväg toohello fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-2934">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="3b691-2935">Använd egenskapen 'script' eller 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="3b691-2935">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="3b691-2936">Båda kan inte användas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="3b691-2936">Both cannot be used together.</span></span> <span data-ttu-id="3b691-2937">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="3b691-2937">hello file name is case-sensitive.</span></span> |<span data-ttu-id="3b691-2938">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2938">No</span></span> |
| <span data-ttu-id="3b691-2939">definierar</span><span class="sxs-lookup"><span data-stu-id="3b691-2939">defines</span></span> |<span data-ttu-id="3b691-2940">Ange parametrar som nyckel/värde-par för refererar till i hello Pig-skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-2940">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="3b691-2941">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2941">No</span></span> |

<span data-ttu-id="3b691-2942">Dessa egenskaper finns specifika toohello Pig aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-2942">These type properties are specific toohello Pig Activity.</span></span> <span data-ttu-id="3b691-2943">Andra egenskaper (utanför hello typeProperties avsnittet) stöds för alla aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3b691-2943">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="3b691-2944">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2944">JSON example</span></span>

```json
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

<span data-ttu-id="3b691-2945">Mer information finns i [Pig aktiviteten](#data-factory-pig-activity.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2945">For more information, see [Pig Activity](#data-factory-pig-activity.md) article.</span></span> 

## <a name="hdinsight-mapreduce-activity"></a><span data-ttu-id="3b691-2946">HDInsight MapReduce-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2946">HDInsight MapReduce Activity</span></span>
<span data-ttu-id="3b691-2947">Du kan ange följande egenskaper i en definition av MapReduce aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2947">You can specify hello following properties in a MapReduce Activity JSON definition.</span></span> <span data-ttu-id="3b691-2948">hello typegenskapen för hello aktiviteten måste vara: **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2948">hello type property for hello activity must be: **HDInsightMapReduce**.</span></span> <span data-ttu-id="3b691-2949">Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2949">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-2950">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightMapReduce:</span><span class="sxs-lookup"><span data-stu-id="3b691-2950">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightMapReduce:</span></span> 

| <span data-ttu-id="3b691-2951">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2951">Property</span></span> | <span data-ttu-id="3b691-2952">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2952">Description</span></span> | <span data-ttu-id="3b691-2953">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-2953">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-2954">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="3b691-2954">jarLinkedService</span></span> | <span data-ttu-id="3b691-2955">Namnet på hello länkad tjänst för hello Azure Storage som innehåller hello JAR-filen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2955">Name of hello linked service for hello Azure Storage that contains hello JAR file.</span></span> | <span data-ttu-id="3b691-2956">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2956">Yes</span></span> |
| <span data-ttu-id="3b691-2957">jarFilePath</span><span class="sxs-lookup"><span data-stu-id="3b691-2957">jarFilePath</span></span> | <span data-ttu-id="3b691-2958">Sökvägen toohello JAR-filen i hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3b691-2958">Path toohello JAR file in hello Azure Storage.</span></span> | <span data-ttu-id="3b691-2959">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2959">Yes</span></span> | 
| <span data-ttu-id="3b691-2960">Klassnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-2960">className</span></span> | <span data-ttu-id="3b691-2961">Namnet på hello huvudsakliga klassen i hello JAR-filen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2961">Name of hello main class in hello JAR file.</span></span> | <span data-ttu-id="3b691-2962">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-2962">Yes</span></span> | 
| <span data-ttu-id="3b691-2963">Argument</span><span class="sxs-lookup"><span data-stu-id="3b691-2963">arguments</span></span> | <span data-ttu-id="3b691-2964">En lista över kommaavgränsade argument för hello MapReduce program.</span><span class="sxs-lookup"><span data-stu-id="3b691-2964">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="3b691-2965">Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket.</span><span class="sxs-lookup"><span data-stu-id="3b691-2965">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="3b691-2966">toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden)</span><span class="sxs-lookup"><span data-stu-id="3b691-2966">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | <span data-ttu-id="3b691-2967">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-2967">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3b691-2968">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-2968">JSON example</span></span>

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
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
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

<span data-ttu-id="3b691-2969">Mer information finns i [MapReduce Activity](data-factory-map-reduce.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-2969">For more information, see [MapReduce Activity](data-factory-map-reduce.md) article.</span></span> 

## <a name="hdinsight-streaming-activity"></a><span data-ttu-id="3b691-2970">HDInsight-strömningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-2970">HDInsight Streaming Activity</span></span>
<span data-ttu-id="3b691-2971">Du kan ange följande egenskaper i en definition av Hadoop Streaming aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-2971">You can specify hello following properties in a Hadoop Streaming Activity JSON definition.</span></span> <span data-ttu-id="3b691-2972">hello typegenskapen för hello aktiviteten måste vara: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="3b691-2972">hello type property for hello activity must be: **HDInsightStreaming**.</span></span> <span data-ttu-id="3b691-2973">Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2973">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-2974">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightStreaming:</span><span class="sxs-lookup"><span data-stu-id="3b691-2974">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightStreaming:</span></span> 

| <span data-ttu-id="3b691-2975">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-2975">Property</span></span> | <span data-ttu-id="3b691-2976">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-2976">Description</span></span> | 
| --- | --- |
| <span data-ttu-id="3b691-2977">Mapper</span><span class="sxs-lookup"><span data-stu-id="3b691-2977">mapper</span></span> | <span data-ttu-id="3b691-2978">Namnet på hello mapper körbara.</span><span class="sxs-lookup"><span data-stu-id="3b691-2978">Name of hello mapper executable.</span></span> <span data-ttu-id="3b691-2979">I exemplet hello är cat.exe hello mapper körbara.</span><span class="sxs-lookup"><span data-stu-id="3b691-2979">In hello example, cat.exe is hello mapper executable.</span></span>| 
| <span data-ttu-id="3b691-2980">Reducer</span><span class="sxs-lookup"><span data-stu-id="3b691-2980">reducer</span></span> | <span data-ttu-id="3b691-2981">Namnet på hello reducer körbara.</span><span class="sxs-lookup"><span data-stu-id="3b691-2981">Name of hello reducer executable.</span></span> <span data-ttu-id="3b691-2982">I exemplet hello är wc.exe hello reducer körbara.</span><span class="sxs-lookup"><span data-stu-id="3b691-2982">In hello example, wc.exe is hello reducer executable.</span></span> | 
| <span data-ttu-id="3b691-2983">Indata</span><span class="sxs-lookup"><span data-stu-id="3b691-2983">input</span></span> | <span data-ttu-id="3b691-2984">Indatafilen (inklusive platsen) för hello mapper.</span><span class="sxs-lookup"><span data-stu-id="3b691-2984">Input file (including location) for hello mapper.</span></span> <span data-ttu-id="3b691-2985">I exemplet hello ”: wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt”: adfsample är hello blob-behållare, exempel/data/Gutenberg är hello mapp och davinci.txt är hello-blob.</span><span class="sxs-lookup"><span data-stu-id="3b691-2985">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span> |
| <span data-ttu-id="3b691-2986">Utdata</span><span class="sxs-lookup"><span data-stu-id="3b691-2986">output</span></span> | <span data-ttu-id="3b691-2987">Utdatafilen (inklusive platsen) för hello reducer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2987">Output file (including location) for hello reducer.</span></span> <span data-ttu-id="3b691-2988">hello utdata från hello Hadoop Streaming job skrivs toohello plats som anges för den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-2988">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span> |
| <span data-ttu-id="3b691-2989">filePaths</span><span class="sxs-lookup"><span data-stu-id="3b691-2989">filePaths</span></span> | <span data-ttu-id="3b691-2990">Sökvägar för hello mapper och reducer körbara filer.</span><span class="sxs-lookup"><span data-stu-id="3b691-2990">Paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="3b691-2991">I exemplet hello: ”adfsample/example/apps/wc.exe” adfsample är hello blob-behållare, exempel/appar är hello mapp och wc.exe är hello körbara.</span><span class="sxs-lookup"><span data-stu-id="3b691-2991">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span> | 
| <span data-ttu-id="3b691-2992">fileLinkedService</span><span class="sxs-lookup"><span data-stu-id="3b691-2992">fileLinkedService</span></span> | <span data-ttu-id="3b691-2993">Azure Storage länkade tjänst som representerar hello Azure-lagring som innehåller hello-filer som anges i hello filePaths avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-2993">Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span> | 
| <span data-ttu-id="3b691-2994">Argument</span><span class="sxs-lookup"><span data-stu-id="3b691-2994">arguments</span></span> | <span data-ttu-id="3b691-2995">En lista över kommaavgränsade argument för hello MapReduce program.</span><span class="sxs-lookup"><span data-stu-id="3b691-2995">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="3b691-2996">Vid körning kan du se några extra argument (till exempel: mapreduce.job.tags) från hello MapReduce-ramverket.</span><span class="sxs-lookup"><span data-stu-id="3b691-2996">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="3b691-2997">toodifferentiate din argument med hello MapReduce argument, Överväg att använda både alternativet och värde som argument som visas i följande exempel hello (- s,--indata--utdata osv., är en alternativ direkt följt av deras värden)</span><span class="sxs-lookup"><span data-stu-id="3b691-2997">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | 
| <span data-ttu-id="3b691-2998">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="3b691-2998">getDebugInfo</span></span> | <span data-ttu-id="3b691-2999">Ett valfritt element.</span><span class="sxs-lookup"><span data-stu-id="3b691-2999">An optional element.</span></span> <span data-ttu-id="3b691-3000">När den är inställd tooFailure laddas hello loggar endast vid fel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3000">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="3b691-3001">När den är inställd tooAll laddas alltid loggar oavsett hello Körstatus.</span><span class="sxs-lookup"><span data-stu-id="3b691-3001">When it is set tooAll, logs are always downloaded irrespective of hello execution status.</span></span> | 

> [!NOTE]
> <span data-ttu-id="3b691-3002">Du måste ange en utdatauppsättningen för hello Hadoop Streaming Activity för hello **matar ut** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3002">You must specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="3b691-3003">Den här datauppsättningen kan bara en dummy datamängd som är nödvändiga toodrive hello pipeline schema (varje timme, varje dag, osv.).</span><span class="sxs-lookup"><span data-stu-id="3b691-3003">This dataset can be just a dummy dataset that is required toodrive hello pipeline schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="3b691-3004">Om hello aktiviteten inte tar indata, du kan hoppa över att ange en inkommande datauppsättning för hello aktivitet för hello **indata** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3004">If hello activity doesn't take an input, you can skip specifying an input dataset for hello activity for hello **inputs** property.</span></span>  

## <a name="json-example"></a><span data-ttu-id="3b691-3005">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3005">JSON example</span></span>

```json
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
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
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
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

<span data-ttu-id="3b691-3006">Mer information finns i [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3006">For more information, see [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) article.</span></span> 

## <a name="hdinsight-spark-activity"></a><span data-ttu-id="3b691-3007">HDInsight Apache Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3007">HDInsight Spark Activity</span></span>
<span data-ttu-id="3b691-3008">Du kan ange följande egenskaper i en definition av Spark aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-3008">You can specify hello following properties in a Spark Activity JSON definition.</span></span> <span data-ttu-id="3b691-3009">hello typegenskapen för hello aktiviteten måste vara: **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3009">hello type property for hello activity must be: **HDInsightSpark**.</span></span> <span data-ttu-id="3b691-3010">Du måste först skapa en länkad HDInsight-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3010">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-3011">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooHDInsightSpark:</span><span class="sxs-lookup"><span data-stu-id="3b691-3011">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightSpark:</span></span> 

| <span data-ttu-id="3b691-3012">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3012">Property</span></span> | <span data-ttu-id="3b691-3013">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3013">Description</span></span> | <span data-ttu-id="3b691-3014">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3014">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3b691-3015">rootPath</span><span class="sxs-lookup"><span data-stu-id="3b691-3015">rootPath</span></span> | <span data-ttu-id="3b691-3016">hello Azure Blob-behållaren och mappen som innehåller hello Spark-fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-3016">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="3b691-3017">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="3b691-3017">hello file name is case-sensitive.</span></span> | <span data-ttu-id="3b691-3018">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3018">Yes</span></span> |
| <span data-ttu-id="3b691-3019">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="3b691-3019">entryFilePath</span></span> | <span data-ttu-id="3b691-3020">Relativ sökväg toohello rotmapp hello Spark kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3020">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="3b691-3021">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3021">Yes</span></span> |
| <span data-ttu-id="3b691-3022">Klassnamn</span><span class="sxs-lookup"><span data-stu-id="3b691-3022">className</span></span> | <span data-ttu-id="3b691-3023">Programmets Java/Spark huvudsakliga klass</span><span class="sxs-lookup"><span data-stu-id="3b691-3023">Application's Java/Spark main class</span></span> | <span data-ttu-id="3b691-3024">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3024">No</span></span> | 
| <span data-ttu-id="3b691-3025">Argument</span><span class="sxs-lookup"><span data-stu-id="3b691-3025">arguments</span></span> | <span data-ttu-id="3b691-3026">En lista med kommandoradsargument toohello Spark-program.</span><span class="sxs-lookup"><span data-stu-id="3b691-3026">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="3b691-3027">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3027">No</span></span> | 
| <span data-ttu-id="3b691-3028">proxyUser</span><span class="sxs-lookup"><span data-stu-id="3b691-3028">proxyUser</span></span> | <span data-ttu-id="3b691-3029">hello konto tooimpersonate tooexecute hello Spark användarprogram</span><span class="sxs-lookup"><span data-stu-id="3b691-3029">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="3b691-3030">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3030">No</span></span> | 
| <span data-ttu-id="3b691-3031">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="3b691-3031">sparkConfig</span></span> | <span data-ttu-id="3b691-3032">Spark konfigurationsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b691-3032">Spark configuration properties.</span></span> | <span data-ttu-id="3b691-3033">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3033">No</span></span> | 
| <span data-ttu-id="3b691-3034">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="3b691-3034">getDebugInfo</span></span> | <span data-ttu-id="3b691-3035">Anger när hello Spark loggfilerna kopierade toohello Azure storage som används av HDInsight-kluster (eller) anges av sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="3b691-3035">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="3b691-3036">Tillåtna värden: None, alltid eller fel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3036">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="3b691-3037">Standardvärde: Ingen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3037">Default value: None.</span></span> | <span data-ttu-id="3b691-3038">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3038">No</span></span> | 
| <span data-ttu-id="3b691-3039">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="3b691-3039">sparkJobLinkedService</span></span> | <span data-ttu-id="3b691-3040">hello länkad Azure Storage-tjänst som äger hello Spark fil, beroenden och loggar.</span><span class="sxs-lookup"><span data-stu-id="3b691-3040">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="3b691-3041">Om du inte anger ett värde för den här egenskapen används hello lagring som är associerade med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-3041">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="3b691-3042">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3042">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3b691-3043">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3043">JSON example</span></span>

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
<span data-ttu-id="3b691-3044">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="3b691-3044">Note hello following points:</span></span> 

- <span data-ttu-id="3b691-3045">Hej **typen** egenskapen för**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3045">hello **type** property is set too**HDInsightSpark**.</span></span>
- <span data-ttu-id="3b691-3046">Hej **rootPath** har angetts för**adfspark\\pyFiles** där adfspark är hello Azure Blob-behållare och pyFiles är bra mapp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="3b691-3046">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="3b691-3047">I det här exemplet är hello Azure Blob Storage hello en som är associerad med hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b691-3047">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="3b691-3048">Du kan ladda upp hello filen tooa olika Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3b691-3048">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="3b691-3049">Om du gör det, skapa en länkad Azure Storage service toolink storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="3b691-3049">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="3b691-3050">Ange hello namnet på hello länkade tjänst som ett värde för hello **sparkJobLinkedService** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3050">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="3b691-3051">Se [Spark Aktivitetsegenskaper](#spark-activity-properties) för ytterligare information om den här egenskapen och andra egenskaper som stöds av hello Spark-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3051">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>
- <span data-ttu-id="3b691-3052">Hej **entryFilePath** anges toohello **test.py**, vilket är hello python-fil.</span><span class="sxs-lookup"><span data-stu-id="3b691-3052">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span> 
- <span data-ttu-id="3b691-3053">Hej **getDebugInfo** egenskapen för**alltid**, vilket innebär att hello loggfiler är alltid genereras (lyckade eller misslyckade).</span><span class="sxs-lookup"><span data-stu-id="3b691-3053">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>  

    > [!IMPORTANT]
    > <span data-ttu-id="3b691-3054">Vi rekommenderar att du inte anger den här egenskapen tooAlways i en produktionsmiljö om du felsöker ett problem.</span><span class="sxs-lookup"><span data-stu-id="3b691-3054">We recommend that you do not set this property tooAlways in a production environment unless you are troubleshooting an issue.</span></span> 
- <span data-ttu-id="3b691-3055">Hej **matar ut** avsnittet innehåller en datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-3055">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="3b691-3056">Du måste ange en datamängd för utdata, även om hello spark-program inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-3056">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="3b691-3057">hello utdata dataset enheter hello schema för hello pipelinen (varje timme, varje dag, osv.).</span><span class="sxs-lookup"><span data-stu-id="3b691-3057">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>

<span data-ttu-id="3b691-3058">Mer information om hello aktivitet finns [Spark aktiviteten](data-factory-spark.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3058">For more information about hello activity, see [Spark Activity](data-factory-spark.md) article.</span></span>  

## <a name="machine-learning-batch-execution-activity"></a><span data-ttu-id="3b691-3059">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3059">Machine Learning Batch Execution Activity</span></span>
<span data-ttu-id="3b691-3060">Du kan ange följande egenskaper i en definition av Azure ML Batch Execution aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-3060">You can specify hello following properties in a Azure ML Batch Execution Activity JSON definition.</span></span> <span data-ttu-id="3b691-3061">hello typegenskapen för hello aktiviteten måste vara: **AzureMLBatchExecution**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3061">hello type property for hello activity must be: **AzureMLBatchExecution**.</span></span> <span data-ttu-id="3b691-3062">Du måste skapa en Azure Machine Learning länkade tjänsten först och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3062">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-3063">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooAzureMLBatchExecution:</span><span class="sxs-lookup"><span data-stu-id="3b691-3063">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLBatchExecution:</span></span>

<span data-ttu-id="3b691-3064">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3064">Property</span></span> | <span data-ttu-id="3b691-3065">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3065">Description</span></span> | <span data-ttu-id="3b691-3066">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3066">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="3b691-3067">webServiceInput</span><span class="sxs-lookup"><span data-stu-id="3b691-3067">webServiceInput</span></span> | <span data-ttu-id="3b691-3068">hello dataset toobe angavs som indata för hello Azure ML-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3068">hello dataset toobe passed as an input for hello Azure ML web service.</span></span> <span data-ttu-id="3b691-3069">Den här datauppsättningen måste också tas med i hello indata för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3069">This dataset must also be included in hello inputs for hello activity.</span></span> |<span data-ttu-id="3b691-3070">Använda webServiceInput eller webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="3b691-3070">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="3b691-3071">webServiceInputs</span><span class="sxs-lookup"><span data-stu-id="3b691-3071">webServiceInputs</span></span> | <span data-ttu-id="3b691-3072">Ange datauppsättningar toobe skickas som indata för hello Azure ML-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3072">Specify datasets toobe passed as inputs for hello Azure ML web service.</span></span> <span data-ttu-id="3b691-3073">Om hello webbtjänst tar flera indata kan använda hello webServiceInputs egenskapen i stället för att använda hello webServiceInput-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3073">If hello web service takes multiple inputs, use hello webServiceInputs property instead of using hello webServiceInput property.</span></span> <span data-ttu-id="3b691-3074">Datauppsättningar som refereras av hello **webServiceInputs** måste också tas med i hello aktiviteten **indata**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3074">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span> | <span data-ttu-id="3b691-3075">Använda webServiceInput eller webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="3b691-3075">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="3b691-3076">webServiceOutputs</span><span class="sxs-lookup"><span data-stu-id="3b691-3076">webServiceOutputs</span></span> | <span data-ttu-id="3b691-3077">hello datauppsättningar som är tilldelad som utdata för hello Azure ML web service.</span><span class="sxs-lookup"><span data-stu-id="3b691-3077">hello datasets that are assigned as outputs for hello Azure ML web service.</span></span> <span data-ttu-id="3b691-3078">hello webbtjänst returnerar utdata i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-3078">hello web service returns output data in this dataset.</span></span> | <span data-ttu-id="3b691-3079">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3079">Yes</span></span> | 
<span data-ttu-id="3b691-3080">globalParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-3080">globalParameters</span></span> | <span data-ttu-id="3b691-3081">Ange värden för hello webbtjänstparametrar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3081">Specify values for hello web service parameters in this section.</span></span> | <span data-ttu-id="3b691-3082">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3082">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3b691-3083">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3083">JSON example</span></span>
<span data-ttu-id="3b691-3084">I det här exemplet hello aktiviteten har hello dataset **MLSqlInput** som indata och **MLSqlOutput** som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-3084">In this example, hello activity has hello dataset **MLSqlInput** as input and **MLSqlOutput** as hello output.</span></span> <span data-ttu-id="3b691-3085">Hej **MLSqlInput** skickas som ett inkommande toohello webbtjänsten genom att använda hello **webServiceInput** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3085">hello **MLSqlInput** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="3b691-3086">Hej **MLSqlOutput** skickas som en webbtjänst för utdata toohello genom att använda hello **webServiceOutputs** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3086">hello **MLSqlOutput** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span> 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

<span data-ttu-id="3b691-3087">Hello JSON-exempelvis hello distribuerade tjänsten använder en läsare och skrivare modulen tooread/skriva data från Azure Machine Learning Web / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3b691-3087">In hello JSON example, hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="3b691-3088">Den här webbtjänsten visar hello följande fyra parametrar: databasen servernamnet, databasnamnet, Server-användarkonto och serverlösenord.</span><span class="sxs-lookup"><span data-stu-id="3b691-3088">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>

> [!NOTE]
> <span data-ttu-id="3b691-3089">Endast indata och utdata för hello AzureMLBatchExecution aktivitet kan skickas som parametrar toohello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3089">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="3b691-3090">Hello ovan JSON fragment är exempelvis MLSqlInput en inkommande toohello AzureMLBatchExecution aktivitet som skickas som ett inkommande toohello webbtjänsten via webServiceInput-parametern.</span><span class="sxs-lookup"><span data-stu-id="3b691-3090">For example, in hello above JSON snippet, MLSqlInput is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>

## <a name="machine-learning-update-resource-activity"></a><span data-ttu-id="3b691-3091">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3091">Machine Learning Update Resource Activity</span></span>
<span data-ttu-id="3b691-3092">Du kan ange följande egenskaper i en definition av Azure ML Update resurs aktiviteten JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-3092">You can specify hello following properties in a Azure ML Update Resource Activity JSON definition.</span></span> <span data-ttu-id="3b691-3093">hello typegenskapen för hello aktiviteten måste vara: **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3093">hello type property for hello activity must be: **AzureMLUpdateResource**.</span></span> <span data-ttu-id="3b691-3094">Du måste skapa en Azure Machine Learning länkade tjänsten först och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3094">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-3095">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooAzureMLUpdateResource:</span><span class="sxs-lookup"><span data-stu-id="3b691-3095">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLUpdateResource:</span></span>

<span data-ttu-id="3b691-3096">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3096">Property</span></span> | <span data-ttu-id="3b691-3097">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3097">Description</span></span> | <span data-ttu-id="3b691-3098">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3098">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="3b691-3099">trainedModelName</span><span class="sxs-lookup"><span data-stu-id="3b691-3099">trainedModelName</span></span> | <span data-ttu-id="3b691-3100">Namnet på hello retrained modell.</span><span class="sxs-lookup"><span data-stu-id="3b691-3100">Name of hello retrained model.</span></span> | <span data-ttu-id="3b691-3101">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3101">Yes</span></span> |  
<span data-ttu-id="3b691-3102">trainedModelDatasetName</span><span class="sxs-lookup"><span data-stu-id="3b691-3102">trainedModelDatasetName</span></span> | <span data-ttu-id="3b691-3103">DataSet pekar toohello iLearner-fil som returneras av hello omtränings igen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3103">Dataset pointing toohello iLearner file returned by hello retraining operation.</span></span> | <span data-ttu-id="3b691-3104">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3104">Yes</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3b691-3105">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3105">JSON example</span></span>
<span data-ttu-id="3b691-3106">hello pipeline har två aktiviteter: **AzureMLBatchExecution** och **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3106">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="3b691-3107">hello Azure ML-batchkörning aktiviteten tar hello utbildningsdata som indata och genererar en iLearner-fil som utdata.</span><span class="sxs-lookup"><span data-stu-id="3b691-3107">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="3b691-3108">hello-aktivitet anropar hello utbildning webbtjänst (träningsexperiment visas som en webbtjänst) med hello indata utbildning data och tar emot hello ilearner-fil från hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3108">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="3b691-3109">Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-3109">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="3b691-3110">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3110">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="3b691-3111">Du kan ange följande egenskaper i en definition av U-SQL-aktivitet JSON hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-3111">You can specify hello following properties in a U-SQL Activity JSON definition.</span></span> <span data-ttu-id="3b691-3112">hello typegenskapen för hello aktiviteten måste vara: **DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3112">hello type property for hello activity must be: **DataLakeAnalyticsU-SQL**.</span></span> <span data-ttu-id="3b691-3113">Du måste skapa en länkad Azure Data Lake Analytics-tjänst och ange hello namnet på det som ett värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3113">You must create an Azure Data Lake Analytics linked service and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-3114">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooDataLakeAnalyticsU-SQL:</span><span class="sxs-lookup"><span data-stu-id="3b691-3114">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDataLakeAnalyticsU-SQL:</span></span> 

| <span data-ttu-id="3b691-3115">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3115">Property</span></span> | <span data-ttu-id="3b691-3116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3116">Description</span></span> | <span data-ttu-id="3b691-3117">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3117">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-3118">ScriptPath</span><span class="sxs-lookup"><span data-stu-id="3b691-3118">scriptPath</span></span> |<span data-ttu-id="3b691-3119">Sökvägen toofolder som innehåller hello U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3119">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="3b691-3120">Namnet på hello-filen är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="3b691-3120">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="3b691-3121">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="3b691-3121">No (if you use script)</span></span> |
| <span data-ttu-id="3b691-3122">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="3b691-3122">scriptLinkedService</span></span> |<span data-ttu-id="3b691-3123">Länkade tjänst som länkar hello lagring som innehåller hello skriptet toohello data factory</span><span class="sxs-lookup"><span data-stu-id="3b691-3123">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="3b691-3124">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="3b691-3124">No (if you use script)</span></span> |
| <span data-ttu-id="3b691-3125">Skriptet</span><span class="sxs-lookup"><span data-stu-id="3b691-3125">script</span></span> |<span data-ttu-id="3b691-3126">Ange infogat skript i stället för att ange scriptPath och scriptLinkedService.</span><span class="sxs-lookup"><span data-stu-id="3b691-3126">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="3b691-3127">Till exempel: ”skript”: ”skapa databastest”.</span><span class="sxs-lookup"><span data-stu-id="3b691-3127">For example: "script": "CREATE DATABASE test".</span></span> |<span data-ttu-id="3b691-3128">Nej (om du använder scriptPath och scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="3b691-3128">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="3b691-3129">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="3b691-3129">degreeOfParallelism</span></span> |<span data-ttu-id="3b691-3130">hello maximalt antal noder används samtidigt toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="3b691-3130">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="3b691-3131">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3131">No</span></span> |
| <span data-ttu-id="3b691-3132">Prioritet</span><span class="sxs-lookup"><span data-stu-id="3b691-3132">priority</span></span> |<span data-ttu-id="3b691-3133">Anger vilka jobb av alla köas ska vara markerade toorun först.</span><span class="sxs-lookup"><span data-stu-id="3b691-3133">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="3b691-3134">hello lägre hello nummer, hello högre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3134">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="3b691-3135">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3135">No</span></span> |
| <span data-ttu-id="3b691-3136">parameters</span><span class="sxs-lookup"><span data-stu-id="3b691-3136">parameters</span></span> |<span data-ttu-id="3b691-3137">Parametrar för hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="3b691-3137">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="3b691-3138">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3138">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3b691-3139">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3139">JSON example</span></span>

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3b691-3140">Mer information finns i [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md).</span><span class="sxs-lookup"><span data-stu-id="3b691-3140">For more information, see [Data Lake Analytics U-SQL Activity](data-factory-usql-activity.md).</span></span> 

## <a name="stored-procedure-activity"></a><span data-ttu-id="3b691-3141">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3141">Stored Procedure Activity</span></span>
<span data-ttu-id="3b691-3142">Du kan ange hello följande egenskaper i en lagrad procedur aktivitet JSON-definition.</span><span class="sxs-lookup"><span data-stu-id="3b691-3142">You can specify hello following properties in a Stored Procedure Activity JSON definition.</span></span> <span data-ttu-id="3b691-3143">hello typegenskapen för hello aktiviteten måste vara: **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3143">hello type property for hello activity must be: **SqlServerStoredProcedure**.</span></span> <span data-ttu-id="3b691-3144">Du måste skapa en något av följande länkade tjänster hello och ange hello namnet på hello länkade tjänst som värde för hello **linkedServiceName** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="3b691-3144">You must create an one of hello following linked services and specify hello name of hello linked service as a value for hello **linkedServiceName** property:</span></span>

- <span data-ttu-id="3b691-3145">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3b691-3145">SQL Server</span></span> 
- <span data-ttu-id="3b691-3146">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3b691-3146">Azure SQL Database</span></span>
- <span data-ttu-id="3b691-3147">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3b691-3147">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="3b691-3148">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooSqlServerStoredProcedure:</span><span class="sxs-lookup"><span data-stu-id="3b691-3148">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooSqlServerStoredProcedure:</span></span>

| <span data-ttu-id="3b691-3149">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3149">Property</span></span> | <span data-ttu-id="3b691-3150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3150">Description</span></span> | <span data-ttu-id="3b691-3151">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3151">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b691-3152">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="3b691-3152">storedProcedureName</span></span> |<span data-ttu-id="3b691-3153">Ange hello namnet på hello lagrade procedur i hello Azure SQL database eller Azure SQL Data Warehouse som representeras av hello länkade tjänst som hello utdata tabellen använder.</span><span class="sxs-lookup"><span data-stu-id="3b691-3153">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="3b691-3154">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3154">Yes</span></span> |
| <span data-ttu-id="3b691-3155">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3b691-3155">storedProcedureParameters</span></span> |<span data-ttu-id="3b691-3156">Ange värden för parametrarna för lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="3b691-3156">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="3b691-3157">Om du behöver toopass null för en parameter, Använd hello syntax: ”param1”: null (alla gemen).</span><span class="sxs-lookup"><span data-stu-id="3b691-3157">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="3b691-3158">Se följande exempel toolearn om hur du använder den här egenskapen hello.</span><span class="sxs-lookup"><span data-stu-id="3b691-3158">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="3b691-3159">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3159">No</span></span> |

<span data-ttu-id="3b691-3160">Om du anger en inkommande datauppsättning, måste det vara tillgänglig (statusen ”klar”) för hello lagrade proceduren aktiviteten toorun.</span><span class="sxs-lookup"><span data-stu-id="3b691-3160">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="3b691-3161">hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter.</span><span class="sxs-lookup"><span data-stu-id="3b691-3161">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="3b691-3162">Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3162">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> <span data-ttu-id="3b691-3163">Du måste ange en datamängd för utdata för en lagrad procedur-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b691-3163">You must specify an output dataset for a stored procedure activity.</span></span> 

<span data-ttu-id="3b691-3164">Utdatauppsättningen anger hello **schema** för hello lagrade proceduraktiviteten (varje timme, varje vecka, månad, etc.).</span><span class="sxs-lookup"><span data-stu-id="3b691-3164">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <span data-ttu-id="3b691-3165">Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun.</span><span class="sxs-lookup"><span data-stu-id="3b691-3165">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="3b691-3166">Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3b691-3166">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in hello pipeline.</span></span> <span data-ttu-id="3b691-3167">Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset.</span><span class="sxs-lookup"><span data-stu-id="3b691-3167">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="3b691-3168">Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="3b691-3168">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="3b691-3169">I vissa fall hello utdatauppsättningen kan vara en **dummy dataset**, som används för endast toospecify hello schema för att köra hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3b691-3169">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span>  

### <a name="json-example"></a><span data-ttu-id="3b691-3170">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3170">JSON example</span></span>

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3b691-3171">Mer information finns i [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3171">For more information, see [Stored Procedure Activity](data-factory-stored-proc-activity.md) article.</span></span> 

## <a name="net-custom-activity"></a><span data-ttu-id="3b691-3172">.NET-anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3172">.NET custom activity</span></span>
<span data-ttu-id="3b691-3173">Du kan ange hello följande egenskaper i en anpassad aktivitet för .NET JSON-definitionen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3173">You can specify hello following properties in a .NET custom activity JSON definition.</span></span> <span data-ttu-id="3b691-3174">hello typegenskapen för hello aktiviteten måste vara: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3174">hello type property for hello activity must be: **DotNetActivity**.</span></span> <span data-ttu-id="3b691-3175">Du måste skapa en Azure HDInsight länkad tjänst eller ett Azure Batch länkade tjänsten och ange hello namnet på hello länkade tjänst som värde för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3175">You must create an Azure HDInsight linked service or an Azure Batch linked service, and specify hello name of hello linked service as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="3b691-3176">hello följande egenskaper stöds i hello **typeProperties** avsnittet när du ställer in hello typ av aktivitet tooDotNetActivity:</span><span class="sxs-lookup"><span data-stu-id="3b691-3176">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDotNetActivity:</span></span>
 
| <span data-ttu-id="3b691-3177">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3b691-3177">Property</span></span> | <span data-ttu-id="3b691-3178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3b691-3178">Description</span></span> | <span data-ttu-id="3b691-3179">Krävs</span><span class="sxs-lookup"><span data-stu-id="3b691-3179">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3b691-3180">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="3b691-3180">AssemblyName</span></span> | <span data-ttu-id="3b691-3181">Namnet på hello sammansättning.</span><span class="sxs-lookup"><span data-stu-id="3b691-3181">Name of hello assembly.</span></span> <span data-ttu-id="3b691-3182">I hello exempelvis är: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3182">In hello example, it is: **MyDotnetActivity.dll**.</span></span> | <span data-ttu-id="3b691-3183">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3183">Yes</span></span> |
| <span data-ttu-id="3b691-3184">EntryPoint</span><span class="sxs-lookup"><span data-stu-id="3b691-3184">EntryPoint</span></span> |<span data-ttu-id="3b691-3185">Namnet på hello-klass som implementerar hello IDotNetActivity gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="3b691-3185">Name of hello class that implements hello IDotNetActivity interface.</span></span> <span data-ttu-id="3b691-3186">I hello exempelvis är: **MyDotNetActivityNS.MyDotNetActivity** där MyDotNetActivityNS är hello namnområde och MyDotNetActivity är hello-klass.</span><span class="sxs-lookup"><span data-stu-id="3b691-3186">In hello example, it is: **MyDotNetActivityNS.MyDotNetActivity** where MyDotNetActivityNS is hello namespace and MyDotNetActivity is hello class.</span></span>  | <span data-ttu-id="3b691-3187">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3187">Yes</span></span> | 
| <span data-ttu-id="3b691-3188">PackageLinkedService</span><span class="sxs-lookup"><span data-stu-id="3b691-3188">PackageLinkedService</span></span> | <span data-ttu-id="3b691-3189">Namnet på hello länkad Azure Storage-tjänst som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3189">Name of hello Azure Storage linked service that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="3b691-3190">I hello exempelvis är: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3190">In hello example, it is: **AzureStorageLinkedService**.</span></span>| <span data-ttu-id="3b691-3191">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3191">Yes</span></span> |
| <span data-ttu-id="3b691-3192">PackageFile</span><span class="sxs-lookup"><span data-stu-id="3b691-3192">PackageFile</span></span> | <span data-ttu-id="3b691-3193">Namnet på hello zip-filen.</span><span class="sxs-lookup"><span data-stu-id="3b691-3193">Name of hello zip file.</span></span> <span data-ttu-id="3b691-3194">I hello exempelvis är: **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="3b691-3194">In hello example, it is: **customactivitycontainer/MyDotNetActivity.zip**.</span></span> | <span data-ttu-id="3b691-3195">Ja</span><span class="sxs-lookup"><span data-stu-id="3b691-3195">Yes</span></span> |
| <span data-ttu-id="3b691-3196">extendedProperties</span><span class="sxs-lookup"><span data-stu-id="3b691-3196">extendedProperties</span></span> | <span data-ttu-id="3b691-3197">Utökade egenskaper som du kan definiera och vidarebefordra toohello .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="3b691-3197">Extended properties that you can define and pass on toohello .NET code.</span></span> <span data-ttu-id="3b691-3198">I det här exemplet hello **SliceStart** variabeln anges tooa värde baserat på hello SliceStart systemvariabeln.</span><span class="sxs-lookup"><span data-stu-id="3b691-3198">In this example, hello **SliceStart** variable is set tooa value based on hello SliceStart system variable.</span></span> | <span data-ttu-id="3b691-3199">Nej</span><span class="sxs-lookup"><span data-stu-id="3b691-3199">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3b691-3200">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="3b691-3200">JSON example</span></span>

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

<span data-ttu-id="3b691-3201">Detaljerad information finns i [använda anpassade aktiviteter i Data Factory](data-factory-use-custom-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3b691-3201">For detailed information, see [Use custom activities in Data Factory](data-factory-use-custom-activities.md) article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3b691-3202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b691-3202">Next Steps</span></span>
<span data-ttu-id="3b691-3203">Se hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="3b691-3203">See hello following tutorials:</span></span> 

- [<span data-ttu-id="3b691-3204">Självstudier: skapa en pipeline med en kopia-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3204">Tutorial: create a pipeline with a copy activity</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [<span data-ttu-id="3b691-3205">Självstudier: skapa en pipeline med en hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3b691-3205">Tutorial: create a pipeline with a hive activity</span></span>](data-factory-build-your-first-pipeline-using-editor.md)