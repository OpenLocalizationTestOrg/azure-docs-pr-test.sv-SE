---
title: "aaaDesign effektivt listan frågor - Azure Batch | Microsoft Docs"
description: "Öka prestandan genom att filtrera dina frågor när du begär information om Batch-resurser som pooler, jobb, uppgifter och beräkningsnoder."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7e554119ec9d0e9e8007ccfb1ca80fe142a5e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-queries-toolist-batch-resources-efficiently"></a><span data-ttu-id="61f24-103">Skapa frågor toolist Batch resurser effektivt</span><span class="sxs-lookup"><span data-stu-id="61f24-103">Create queries toolist Batch resources efficiently</span></span>

<span data-ttu-id="61f24-104">Här lär du dig hur tooincrease Azure Batch-programmets prestanda genom att minska hello mängden data som returneras av hello-tjänsten när du frågar projekt, uppgifter och beräkningsnoder med hello [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="61f24-104">Here you'll learn how tooincrease your Azure Batch application's performance by reducing hello amount of data that is returned by hello service when you query jobs, tasks, and compute nodes with hello [Batch .NET][api_net] library.</span></span>

<span data-ttu-id="61f24-105">Nästan alla program i batchen måste tooperform vissa typer av övervakning eller andra åtgärder som frågar ofta hello Batch-tjänsten med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="61f24-105">Nearly all Batch applications need tooperform some type of monitoring or other operation that queries hello Batch service, often at regular intervals.</span></span> <span data-ttu-id="61f24-106">Till exempel toodetermine om det finns kvar i ett jobb köade aktiviteter, måste du hämta data för varje aktivitet i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="61f24-106">For example, toodetermine whether there are any queued tasks remaining in a job, you must get data on every task in hello job.</span></span> <span data-ttu-id="61f24-107">toodetermine hello status för noder i din pool, måste du hämta data på varje nod i hello pool.</span><span class="sxs-lookup"><span data-stu-id="61f24-107">toodetermine hello status of nodes in your pool, you must get data on every node in hello pool.</span></span> <span data-ttu-id="61f24-108">Den här artikeln förklarar hur tooexecute sådana frågor i hello mest effektiva sättet.</span><span class="sxs-lookup"><span data-stu-id="61f24-108">This article explains how tooexecute such queries in hello most efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="61f24-109">hello Batch-tjänsten innehåller särskilda API-stöd för hello vanligt scenario för inventering aktiviteter i ett projekt.</span><span class="sxs-lookup"><span data-stu-id="61f24-109">hello Batch service provides special API support for hello common scenario of counting tasks in a job.</span></span> <span data-ttu-id="61f24-110">Du kan anropa hello istället för att använda en fråga för dessa [hämta uppgift räknar] [ rest_get_task_counts] igen.</span><span class="sxs-lookup"><span data-stu-id="61f24-110">Instead of using a list query for these, you can call hello [Get Task Counts][rest_get_task_counts] operation.</span></span> <span data-ttu-id="61f24-111">Hämta uppgift antal anger hur många aktiviteter väntar, kör eller slutföra och hur många aktiviteter har lyckades eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="61f24-111">Get Task Counts indicates how many tasks are pending, running or complete, and how many tasks have succeeded or failed.</span></span> <span data-ttu-id="61f24-112">Hämta uppgift räknar är effektivare än en fråga.</span><span class="sxs-lookup"><span data-stu-id="61f24-112">Get Task Counts is more efficient than a list query.</span></span> <span data-ttu-id="61f24-113">Mer information finns i [antal uppgifter för ett jobb efter status (förhandsgranskning)](batch-get-task-counts.md).</span><span class="sxs-lookup"><span data-stu-id="61f24-113">For more information, see [Count tasks for a job by state (Preview)](batch-get-task-counts.md).</span></span> 
>
> <span data-ttu-id="61f24-114">hello hämta uppgift räknar åtgärden är inte tillgänglig i Batch-tjänsten-versioner tidigare än 2017-06-01.5.1.</span><span class="sxs-lookup"><span data-stu-id="61f24-114">hello Get Task Counts operation is not available in Batch service versions earlier than 2017-06-01.5.1.</span></span> <span data-ttu-id="61f24-115">Om du använder en äldre version av hello-tjänsten sedan använda en lista över frågan toocount aktiviteter i ett jobb i stället.</span><span class="sxs-lookup"><span data-stu-id="61f24-115">If you are using an older version of hello service, then use a list query toocount tasks in a job instead.</span></span>
>
> 

## <a name="meet-hello-detaillevel"></a><span data-ttu-id="61f24-116">Uppfyller hello DetailLevel</span><span class="sxs-lookup"><span data-stu-id="61f24-116">Meet hello DetailLevel</span></span>
<span data-ttu-id="61f24-117">I produktion Batch program numrera entiteter t.ex projekt, uppgifter och beräkningsnoder i hello tusentalsavgränsare.</span><span class="sxs-lookup"><span data-stu-id="61f24-117">In a production Batch application, entities like jobs, tasks, and compute nodes can number in hello thousands.</span></span> <span data-ttu-id="61f24-118">När du begär information om dessa resurser kan måste potentiellt stora mängder data ”över hello överföring” från hello Batch-tooyour tjänstprogrammet på varje fråga.</span><span class="sxs-lookup"><span data-stu-id="61f24-118">When you request information on these resources, a potentially large amount of data must "cross hello wire" from hello Batch service tooyour application on each query.</span></span> <span data-ttu-id="61f24-119">Genom att begränsa hello antalet objekt och typ av information som returneras av en fråga kan du öka hello hastigheten på dina frågor och därför hello prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="61f24-119">By limiting hello number of items and type of information that is returned by a query, you can increase hello speed of your queries, and therefore hello performance of your application.</span></span>

<span data-ttu-id="61f24-120">Detta [Batch .NET] [ api_net] API kodfragment visar *varje* aktivitet som är associerad med ett jobb, tillsammans med *alla* hello egenskaper för varje uppgift:</span><span class="sxs-lookup"><span data-stu-id="61f24-120">This [Batch .NET][api_net] API code snippet lists *every* task that is associated with a job, along with *all* of hello properties of each task:</span></span>

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

<span data-ttu-id="61f24-121">Du kan utföra ett mycket effektivare listfrågan dock genom att använda en ”detaljnivån” tooyour fråga.</span><span class="sxs-lookup"><span data-stu-id="61f24-121">You can perform a much more efficient list query, however, by applying a "detail level" tooyour query.</span></span> <span data-ttu-id="61f24-122">Du kan göra detta genom att tillhandahålla en [ODATADetailLevel] [ odata] objekt toohello [JobOperations.ListTasks] [ net_list_tasks] metod.</span><span class="sxs-lookup"><span data-stu-id="61f24-122">You do this by supplying an [ODATADetailLevel][odata] object toohello [JobOperations.ListTasks][net_list_tasks] method.</span></span> <span data-ttu-id="61f24-123">Den här fragment returnerar endast hello-ID, kommandoraden och compute-nod information egenskaper för slutförda aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="61f24-123">This snippet returns only hello ID, command line, and compute node information properties of completed tasks:</span></span>

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties tooreturn
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply hello ODATADetailLevel toohello ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

<span data-ttu-id="61f24-124">I det här exemplet, om det finns tusentals uppgifter i jobbet hello hello från hello andra frågan vanligtvis blir resultatet returneras mycket snabbare än hello först.</span><span class="sxs-lookup"><span data-stu-id="61f24-124">In this example scenario, if there are thousands of tasks in hello job, hello results from hello second query will typically be returned much quicker than hello first.</span></span> <span data-ttu-id="61f24-125">Mer information om hur du använder ODATADetailLevel när listobjekt med hello Batch .NET API ingår [under](#efficient-querying-in-batch-net).</span><span class="sxs-lookup"><span data-stu-id="61f24-125">More information about using ODATADetailLevel when you list items with hello Batch .NET API is included [below](#efficient-querying-in-batch-net).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61f24-126">Vi rekommenderar starkt att du *alltid* varor en ODATADetailLevel objektet tooyour .NET API lista anropar tooensure högsta effektivitet och prestanda för ditt program.</span><span class="sxs-lookup"><span data-stu-id="61f24-126">We highly recommend that you *always* supply an ODATADetailLevel object tooyour .NET API list calls tooensure maximum efficiency and performance of your application.</span></span> <span data-ttu-id="61f24-127">Genom att ange en detaljnivå kan hjälpa du toolower Batch-tjänsten svarstider, förbättra belastningen på nätverket och minimera minnesanvändning av klientprogram.</span><span class="sxs-lookup"><span data-stu-id="61f24-127">By specifying a detail level, you can help toolower Batch service response times, improve network utilization, and minimize memory usage by client applications.</span></span>
> 
> 

## <a name="filter-select-and-expand"></a><span data-ttu-id="61f24-128">Filtrera, markera och utöka</span><span class="sxs-lookup"><span data-stu-id="61f24-128">Filter, select, and expand</span></span>
<span data-ttu-id="61f24-129">Hej [Batch .NET] [ api_net] och [Batch REST] [ api_rest] API: er ger hello möjlighet tooreduce båda hello antal objekt som returneras i en lista samt hello mängden information som returneras för varje.</span><span class="sxs-lookup"><span data-stu-id="61f24-129">hello [Batch .NET][api_net] and [Batch REST][api_rest] APIs provide hello ability tooreduce both hello number of items that are returned in a list, as well as hello amount of information that is returned for each.</span></span> <span data-ttu-id="61f24-130">Det gör du genom att ange **filter**, **Välj**, och **Expandera strängar** när du utför listan frågor.</span><span class="sxs-lookup"><span data-stu-id="61f24-130">You do so by specifying **filter**, **select**, and **expand strings** when performing list queries.</span></span>

### <a name="filter"></a><span data-ttu-id="61f24-131">Filter</span><span class="sxs-lookup"><span data-stu-id="61f24-131">Filter</span></span>
<span data-ttu-id="61f24-132">hello filtersträngen är ett uttryck som minskar hello antal objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-132">hello filter string is an expression that reduces hello number of items that are returned.</span></span> <span data-ttu-id="61f24-133">Till exempel listan endast hello pågående aktiviteter för ett jobb eller listan endast compute-noder som är redo toorun uppgifter.</span><span class="sxs-lookup"><span data-stu-id="61f24-133">For example, list only hello running tasks for a job, or list only compute nodes that are ready toorun tasks.</span></span>

* <span data-ttu-id="61f24-134">hello Filtersträngen består av en eller flera uttryck med ett uttryck som består av en egenskapsnamn, en operator och ett värde.</span><span class="sxs-lookup"><span data-stu-id="61f24-134">hello filter string consists of one or more expressions, with an expression that consists of a property name, operator, and value.</span></span> <span data-ttu-id="61f24-135">hello egenskaper som kan anges är specifika tooeach enhetstyp som du fråga som är hello operatorer som stöds för varje egenskap.</span><span class="sxs-lookup"><span data-stu-id="61f24-135">hello properties that can be specified are specific tooeach entity type that you query, as are hello operators that are supported for each property.</span></span>
* <span data-ttu-id="61f24-136">Du kan kombinera flera uttryck med hjälp av hello logiska operatorer `and` och `or`.</span><span class="sxs-lookup"><span data-stu-id="61f24-136">Multiple expressions can be combined by using hello logical operators `and` and `or`.</span></span>
* <span data-ttu-id="61f24-137">Det här exemplet filtrera strängar endast hello kör ”återge” uppgifter: `(state eq 'running') and startswith(id, 'renderTask')`.</span><span class="sxs-lookup"><span data-stu-id="61f24-137">This example filter string lists only hello running "render" tasks: `(state eq 'running') and startswith(id, 'renderTask')`.</span></span>

### <a name="select"></a><span data-ttu-id="61f24-138">Välj</span><span class="sxs-lookup"><span data-stu-id="61f24-138">Select</span></span>
<span data-ttu-id="61f24-139">hello väljer sträng begränsar hello egenskapsvärden som returneras för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="61f24-139">hello select string limits hello property values that are returned for each item.</span></span> <span data-ttu-id="61f24-140">Du anger en lista över egenskapsnamn och endast de värdena som returneras för hello objekt i hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="61f24-140">You specify a list of property names, and only those property values are returned for hello items in hello query results.</span></span>

* <span data-ttu-id="61f24-141">hello väljer sträng består av en kommaavgränsad lista med egenskapsnamn.</span><span class="sxs-lookup"><span data-stu-id="61f24-141">hello select string consists of a comma-separated list of property names.</span></span> <span data-ttu-id="61f24-142">Du kan ange hello egenskaper för hello entitetstypen du frågar.</span><span class="sxs-lookup"><span data-stu-id="61f24-142">You can specify any of hello properties for hello entity type you are querying.</span></span>
* <span data-ttu-id="61f24-143">Det här exemplet väljer sträng som anger att endast tre värden ska returneras för varje aktivitet: `id, state, stateTransitionTime`.</span><span class="sxs-lookup"><span data-stu-id="61f24-143">This example select string specifies that only three property values should be returned for each task: `id, state, stateTransitionTime`.</span></span>

### <a name="expand"></a><span data-ttu-id="61f24-144">Visa</span><span class="sxs-lookup"><span data-stu-id="61f24-144">Expand</span></span>
<span data-ttu-id="61f24-145">hello Expandera sträng minskar hello antalet API-anrop som är nödvändiga tooobtain viss information.</span><span class="sxs-lookup"><span data-stu-id="61f24-145">hello expand string reduces hello number of API calls that are required tooobtain certain information.</span></span> <span data-ttu-id="61f24-146">När du använder en sträng visa mer information om varje objekt kan erhållas med ett enda API-anrop.</span><span class="sxs-lookup"><span data-stu-id="61f24-146">When you use an expand string, more information about each item can be obtained with a single API call.</span></span> <span data-ttu-id="61f24-147">I stället för första erhålla hello-listan för entiteter och begär information för varje objekt i listan hello du använder en Expandera sträng tooobtain hello samma information i ett enda API-anrop.</span><span class="sxs-lookup"><span data-stu-id="61f24-147">Rather than first obtaining hello list of entities, then requesting information for each item in hello list, you use an expand string tooobtain hello same information in a single API call.</span></span> <span data-ttu-id="61f24-148">Mindre API-anrop innebär bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="61f24-148">Less API calls means better performance.</span></span>

* <span data-ttu-id="61f24-149">Liknande toohello väljer sträng, hello Expandera sträng kontroller om vissa data tas med i listan frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="61f24-149">Similar toohello select string, hello expand string controls whether certain data is included in list query results.</span></span>
* <span data-ttu-id="61f24-150">hello Expandera sträng stöds endast när den används i visar en lista över jobb, jobbscheman, uppgifter och pooler.</span><span class="sxs-lookup"><span data-stu-id="61f24-150">hello expand string is only supported when it is used in listing jobs, job schedules, tasks, and pools.</span></span> <span data-ttu-id="61f24-151">Den stöder för närvarande bara statistik.</span><span class="sxs-lookup"><span data-stu-id="61f24-151">Currently, it only supports statistics information.</span></span>
* <span data-ttu-id="61f24-152">När alla egenskaper som krävs och ingen väljer sträng anges, hello expanderar sträng *måste* vara används tooget statistikinformation.</span><span class="sxs-lookup"><span data-stu-id="61f24-152">When all properties are required and no select string is specified, hello expand string *must* be used tooget statistics information.</span></span> <span data-ttu-id="61f24-153">Om en väljer sträng är används tooobtain en delmängd av egenskaper, sedan `stats` kan anges i hello väljer sträng och hello Expandera sträng behöver inte toobe som angetts.</span><span class="sxs-lookup"><span data-stu-id="61f24-153">If a select string is used tooobtain a subset of properties, then `stats` can be specified in hello select string, and hello expand string does not need toobe specified.</span></span>
* <span data-ttu-id="61f24-154">Det här exemplet Expandera sträng Anger att statistik ska returneras för varje objekt i listan hello: `stats`.</span><span class="sxs-lookup"><span data-stu-id="61f24-154">This example expand string specifies that statistics information should be returned for each item in hello list: `stats`.</span></span>

> [!NOTE]
> <span data-ttu-id="61f24-155">När någon av hello tre fråga strängtyper (filtrera, markera och expandera), måste du se till att hello egenskapsnamn och fallet matcha motsvarigheterna REST API-element.</span><span class="sxs-lookup"><span data-stu-id="61f24-155">When constructing any of hello three query string types (filter, select, and expand), you must ensure that hello property names and case match that of their REST API element counterparts.</span></span> <span data-ttu-id="61f24-156">Till exempel när du arbetar med hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) klass, måste du ange **tillstånd** i stället för **tillstånd**, även om hello egenskap för .NET är [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span><span class="sxs-lookup"><span data-stu-id="61f24-156">For example, when working with hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) class, you must specify **state** instead of **State**, even though hello .NET property is [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span></span> <span data-ttu-id="61f24-157">Finns hello tabellerna nedan för egenskapsmappningar mellan hello .NET eller REST API: er.</span><span class="sxs-lookup"><span data-stu-id="61f24-157">See hello tables below for property mappings between hello .NET and REST APIs.</span></span>
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a><span data-ttu-id="61f24-158">Regler för filter, markera och utöka strängar</span><span class="sxs-lookup"><span data-stu-id="61f24-158">Rules for filter, select, and expand strings</span></span>
* <span data-ttu-id="61f24-159">Egenskaper för namnen i filtret, markera och utöka strängar som ska visas som i hello [Batch REST] [ api_rest] API – även när du använder [Batch .NET] [ api_net] eller en av hello andra Batch-SDK.</span><span class="sxs-lookup"><span data-stu-id="61f24-159">Properties names in filter, select, and expand strings should appear as they do in hello [Batch REST][api_rest] API--even when you use [Batch .NET][api_net] or one of hello other Batch SDKs.</span></span>
* <span data-ttu-id="61f24-160">Alla egenskapsnamn är skiftlägeskänsliga, men egenskapsvärden är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="61f24-160">All property names are case-sensitive, but property values are case insensitive.</span></span>
* <span data-ttu-id="61f24-161">Tidsvärdet strängar kan vara något av två format och måste föregås av `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="61f24-161">Date/time strings can be one of two formats, and must be preceded with `DateTime`.</span></span>
  
  * <span data-ttu-id="61f24-162">W3C-DTF format exempel:`creationTime gt DateTime'2011-05-08T08:49:37Z'`</span><span class="sxs-lookup"><span data-stu-id="61f24-162">W3C-DTF format example: `creationTime gt DateTime'2011-05-08T08:49:37Z'`</span></span>
  * <span data-ttu-id="61f24-163">RFC 1123 format exempel:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span><span class="sxs-lookup"><span data-stu-id="61f24-163">RFC 1123 format example: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span></span>
* <span data-ttu-id="61f24-164">Booleskt strängar är antingen `true` eller `false`.</span><span class="sxs-lookup"><span data-stu-id="61f24-164">Boolean strings are either `true` or `false`.</span></span>
* <span data-ttu-id="61f24-165">Om en ogiltig egenskap eller en operator har angetts en `400 (Bad Request)` fel returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-165">If an invalid property or operator is specified, a `400 (Bad Request)` error will result.</span></span>

## <a name="efficient-querying-in-batch-net"></a><span data-ttu-id="61f24-166">Effektiva frågor skickas i Batch .NET</span><span class="sxs-lookup"><span data-stu-id="61f24-166">Efficient querying in Batch .NET</span></span>
<span data-ttu-id="61f24-167">Inom hello [Batch .NET] [ api_net] API, hello [ODATADetailLevel] [ odata] klassen används för att tillhandahålla filter och välj utöka strängar toolist åtgärder.</span><span class="sxs-lookup"><span data-stu-id="61f24-167">Within hello [Batch .NET][api_net] API, hello [ODATADetailLevel][odata] class is used for supplying filter, select, and expand strings toolist operations.</span></span> <span data-ttu-id="61f24-168">Hej ODataDetailLevel klass har tre offentliga strängegenskaper som kan anges i hello konstruktorn eller ange direkt på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="61f24-168">hello ODataDetailLevel class has three public string properties that can be specified in hello constructor, or set directly on hello object.</span></span> <span data-ttu-id="61f24-169">Du sedan skicka hello ODataDetailLevel-objektet som en parameter toohello olika Liståtgärder som [ListPools][net_list_pools], [ListJobs][net_list_jobs], och [ListTasks][net_list_tasks].</span><span class="sxs-lookup"><span data-stu-id="61f24-169">You then pass hello ODataDetailLevel object as a parameter toohello various list operations such as [ListPools][net_list_pools], [ListJobs][net_list_jobs], and [ListTasks][net_list_tasks].</span></span>

* <span data-ttu-id="61f24-170">[ODATADetailLevel][odata].[ FilterClause][odata_filter]: begränsa hello antal objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-170">[ODATADetailLevel][odata].[FilterClause][odata_filter]: Limit hello number of items that are returned.</span></span>
* <span data-ttu-id="61f24-171">[ODATADetailLevel][odata].[ SelectClause][odata_select]: Ange vilka egenskapsvärden som returneras med varje objekt.</span><span class="sxs-lookup"><span data-stu-id="61f24-171">[ODATADetailLevel][odata].[SelectClause][odata_select]: Specify which property values are returned with each item.</span></span>
* <span data-ttu-id="61f24-172">[ODATADetailLevel][odata].[ ExpandClause][odata_expand]: hämta data för alla objekt i ett enda API-anrop i stället för anrop för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="61f24-172">[ODATADetailLevel][odata].[ExpandClause][odata_expand]: Retrieve data for all items in a single API call instead of separate calls for each item.</span></span>

<span data-ttu-id="61f24-173">hello använder följande kodavsnitt hello Batch .NET API tooefficiently frågan hello Batch-tjänsten för hello statistik för en specifik uppsättning pooler.</span><span class="sxs-lookup"><span data-stu-id="61f24-173">hello following code snippet uses hello Batch .NET API tooefficiently query hello Batch service for hello statistics of a specific set of pools.</span></span> <span data-ttu-id="61f24-174">I det här scenariot har hello Batch användaren både test- och pooler.</span><span class="sxs-lookup"><span data-stu-id="61f24-174">In this scenario, hello Batch user has both test and production pools.</span></span> <span data-ttu-id="61f24-175">hello test pool-ID: N föregås ”test” och ”produktprenumeration” föregås hello produktionspool-ID: N.</span><span class="sxs-lookup"><span data-stu-id="61f24-175">hello test pool IDs are prefixed with "test", and hello production pool IDs are prefixed with "prod".</span></span> <span data-ttu-id="61f24-176">Hello kodutdrag *myBatchClient* är en korrekt initierad instans av hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) klass.</span><span class="sxs-lookup"><span data-stu-id="61f24-176">In hello snippet, *myBatchClient* is a properly initialized instance of hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) class.</span></span>

```csharp
// First we need an ODATADetailLevel instance on which tooset hello filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want toopull only hello "test" pools, so we limit hello number of items returned
// by using a FilterClause and specifying that hello pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// toofurther limit hello data that crosses hello wire, configure hello SelectClause to
// limit hello properties that are returned on each CloudPool object tooonly
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify hello ExpandClause so that hello .NET API pulls hello statistics for the
// CloudPools in a single underlying REST API call. Note that we use hello pool's
// REST API element name "stats" here as opposed too"Statistics" as it appears in
// hello .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing hello amount of data that is returned
// by specifying hello detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> <span data-ttu-id="61f24-177">En instans av [ODATADetailLevel] [ odata] som har konfigurerats med utvalda och expandera-satser kan också skickas tooappropriate Get-metoder, exempelvis [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello mängden data som returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-177">An instance of [ODATADetailLevel][odata] that is configured with Select and Expand clauses can also be passed tooappropriate Get methods, such as [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), toolimit hello amount of data that is returned.</span></span>
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a><span data-ttu-id="61f24-178">Batch REST API för too.NET mappningar</span><span class="sxs-lookup"><span data-stu-id="61f24-178">Batch REST too.NET API mappings</span></span>
<span data-ttu-id="61f24-179">Egenskapsnamn i filtret, markera och expandera strängar *måste* återspeglar motsvarigheterna REST API både namn och fallet.</span><span class="sxs-lookup"><span data-stu-id="61f24-179">Property names in filter, select, and expand strings *must* reflect their REST API counterparts, both in name and case.</span></span> <span data-ttu-id="61f24-180">hello tabeller nedan innehåller mappningar mellan hello .NET och REST API-motsvarigheter.</span><span class="sxs-lookup"><span data-stu-id="61f24-180">hello tables below provide mappings between hello .NET and REST API counterparts.</span></span>

### <a name="mappings-for-filter-strings"></a><span data-ttu-id="61f24-181">Mappningar för filtersträngar</span><span class="sxs-lookup"><span data-stu-id="61f24-181">Mappings for filter strings</span></span>
* <span data-ttu-id="61f24-182">**.NET-metoder för listan**: hello .NET API-metoderna i den här kolumnen accepterar ett [ODATADetailLevel] [ odata] objekt som parameter.</span><span class="sxs-lookup"><span data-stu-id="61f24-182">**.NET list methods**: Each of hello .NET API methods in this column accepts an [ODATADetailLevel][odata] object as a parameter.</span></span>
* <span data-ttu-id="61f24-183">**Begäranden om REST**: varje REST API-sidan länkade tooin som den här kolumnen innehåller en tabell som anger hello egenskaper och åtgärder som tillåts i *filter* strängar.</span><span class="sxs-lookup"><span data-stu-id="61f24-183">**REST list requests**: Each REST API page linked tooin this column contains a table that specifies hello properties and operations that are allowed in *filter* strings.</span></span> <span data-ttu-id="61f24-184">Du använder dessa egenskapsnamn och åtgärder när du skapar en [ODATADetailLevel.FilterClause] [ odata_filter] sträng.</span><span class="sxs-lookup"><span data-stu-id="61f24-184">You will use these property names and operations when you construct an [ODATADetailLevel.FilterClause][odata_filter] string.</span></span>

| <span data-ttu-id="61f24-185">Metoder för .NET-lista</span><span class="sxs-lookup"><span data-stu-id="61f24-185">.NET list methods</span></span> | <span data-ttu-id="61f24-186">Begäranden för REST-lista</span><span class="sxs-lookup"><span data-stu-id="61f24-186">REST list requests</span></span> |
| --- | --- |
| <span data-ttu-id="61f24-187">[CertificateOperations.ListCertificates][net_list_certs]</span><span class="sxs-lookup"><span data-stu-id="61f24-187">[CertificateOperations.ListCertificates][net_list_certs]</span></span> |<span data-ttu-id="61f24-188">[Visa hello certifikat på ett konto][rest_list_certs]</span><span class="sxs-lookup"><span data-stu-id="61f24-188">[List hello certificates in an account][rest_list_certs]</span></span> |
| <span data-ttu-id="61f24-189">[CloudTask.ListNodeFiles][net_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="61f24-189">[CloudTask.ListNodeFiles][net_list_task_files]</span></span> |<span data-ttu-id="61f24-190">[Lista hello filer som hör till en aktivitet][rest_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="61f24-190">[List hello files associated with a task][rest_list_task_files]</span></span> |
| <span data-ttu-id="61f24-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="61f24-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span></span> |<span data-ttu-id="61f24-192">[Status för hello hello jobbförberedelseuppgiften och jobbet versionen uppgifter för ett jobb][rest_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="61f24-192">[List hello status of hello job preparation and job release tasks for a job][rest_list_jobprep_status]</span></span> |
| <span data-ttu-id="61f24-193">[JobOperations.ListJobs][net_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="61f24-193">[JobOperations.ListJobs][net_list_jobs]</span></span> |<span data-ttu-id="61f24-194">[Lista hello jobb i ett konto][rest_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="61f24-194">[List hello jobs in an account][rest_list_jobs]</span></span> |
| <span data-ttu-id="61f24-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="61f24-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span></span> |<span data-ttu-id="61f24-196">[Lista hello filer på en nod][rest_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="61f24-196">[List hello files on a node][rest_list_nodefiles]</span></span> |
| <span data-ttu-id="61f24-197">[JobOperations.ListTasks][net_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="61f24-197">[JobOperations.ListTasks][net_list_tasks]</span></span> |<span data-ttu-id="61f24-198">[Lista hello uppgifter som är kopplad till ett jobb][rest_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="61f24-198">[List hello tasks associated with a job][rest_list_tasks]</span></span> |
| <span data-ttu-id="61f24-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="61f24-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span></span> |<span data-ttu-id="61f24-200">[Lista hello jobbscheman på ett konto][rest_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="61f24-200">[List hello job schedules in an account][rest_list_job_schedules]</span></span> |
| <span data-ttu-id="61f24-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="61f24-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span></span> |<span data-ttu-id="61f24-202">[Lista hello jobb som är associerade med ett Jobbschema][rest_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="61f24-202">[List hello jobs associated with a job schedule][rest_list_schedule_jobs]</span></span> |
| <span data-ttu-id="61f24-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="61f24-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span></span> |<span data-ttu-id="61f24-204">[Lista hello compute-noder i en pool][rest_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="61f24-204">[List hello compute nodes in a pool][rest_list_compute_nodes]</span></span> |
| <span data-ttu-id="61f24-205">[PoolOperations.ListPools][net_list_pools]</span><span class="sxs-lookup"><span data-stu-id="61f24-205">[PoolOperations.ListPools][net_list_pools]</span></span> |<span data-ttu-id="61f24-206">[Lista hello pooler i ett konto][rest_list_pools]</span><span class="sxs-lookup"><span data-stu-id="61f24-206">[List hello pools in an account][rest_list_pools]</span></span> |

### <a name="mappings-for-select-strings"></a><span data-ttu-id="61f24-207">Mappningar för väljer strängar</span><span class="sxs-lookup"><span data-stu-id="61f24-207">Mappings for select strings</span></span>
* <span data-ttu-id="61f24-208">**Batch-.NET-typer**: Batch .NET API-typer.</span><span class="sxs-lookup"><span data-stu-id="61f24-208">**Batch .NET types**: Batch .NET API types.</span></span>
* <span data-ttu-id="61f24-209">**REST API-entiteter**: varje sida i den här kolumnen innehåller en eller flera tabeller som visar hello REST API-egenskapsnamn för hello-typen.</span><span class="sxs-lookup"><span data-stu-id="61f24-209">**REST API entities**: Each page in this column contains one or more tables that list hello REST API property names for hello type.</span></span> <span data-ttu-id="61f24-210">Dessa egenskapsnamn som används när du skapar *Välj* strängar.</span><span class="sxs-lookup"><span data-stu-id="61f24-210">These property names are used when you construct *select* strings.</span></span> <span data-ttu-id="61f24-211">Du ska använda samma egenskapsnamn när du skapar en [ODATADetailLevel.SelectClause] [ odata_select] sträng.</span><span class="sxs-lookup"><span data-stu-id="61f24-211">You will use these same property names when you construct an [ODATADetailLevel.SelectClause][odata_select] string.</span></span>

| <span data-ttu-id="61f24-212">Batch .NET-typer</span><span class="sxs-lookup"><span data-stu-id="61f24-212">Batch .NET types</span></span> | <span data-ttu-id="61f24-213">REST API-enheter</span><span class="sxs-lookup"><span data-stu-id="61f24-213">REST API entities</span></span> |
| --- | --- |
| <span data-ttu-id="61f24-214">[Certifikat][net_cert]</span><span class="sxs-lookup"><span data-stu-id="61f24-214">[Certificate][net_cert]</span></span> |<span data-ttu-id="61f24-215">[Hämta information om ett certifikat][rest_get_cert]</span><span class="sxs-lookup"><span data-stu-id="61f24-215">[Get information about a certificate][rest_get_cert]</span></span> |
| <span data-ttu-id="61f24-216">[CloudJob][net_job]</span><span class="sxs-lookup"><span data-stu-id="61f24-216">[CloudJob][net_job]</span></span> |<span data-ttu-id="61f24-217">[Hämta information om ett jobb][rest_get_job]</span><span class="sxs-lookup"><span data-stu-id="61f24-217">[Get information about a job][rest_get_job]</span></span> |
| <span data-ttu-id="61f24-218">[CloudJobSchedule][net_schedule]</span><span class="sxs-lookup"><span data-stu-id="61f24-218">[CloudJobSchedule][net_schedule]</span></span> |<span data-ttu-id="61f24-219">[Hämta information om ett Jobbschema][rest_get_schedule]</span><span class="sxs-lookup"><span data-stu-id="61f24-219">[Get information about a job schedule][rest_get_schedule]</span></span> |
| <span data-ttu-id="61f24-220">[ComputeNode][net_node]</span><span class="sxs-lookup"><span data-stu-id="61f24-220">[ComputeNode][net_node]</span></span> |<span data-ttu-id="61f24-221">[Hämta information om en nod][rest_get_node]</span><span class="sxs-lookup"><span data-stu-id="61f24-221">[Get information about a node][rest_get_node]</span></span> |
| <span data-ttu-id="61f24-222">[CloudPool][net_pool]</span><span class="sxs-lookup"><span data-stu-id="61f24-222">[CloudPool][net_pool]</span></span> |<span data-ttu-id="61f24-223">[Hämta information om en pool][rest_get_pool]</span><span class="sxs-lookup"><span data-stu-id="61f24-223">[Get information about a pool][rest_get_pool]</span></span> |
| <span data-ttu-id="61f24-224">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="61f24-224">[CloudTask][net_task]</span></span> |<span data-ttu-id="61f24-225">[Hämta information om en aktivitet][rest_get_task]</span><span class="sxs-lookup"><span data-stu-id="61f24-225">[Get information about a task][rest_get_task]</span></span> |

## <a name="example-construct-a-filter-string"></a><span data-ttu-id="61f24-226">Exempel: skapa en Filtersträng</span><span class="sxs-lookup"><span data-stu-id="61f24-226">Example: construct a filter string</span></span>
<span data-ttu-id="61f24-227">När du skapar en Filtersträng för [ODATADetailLevel.FilterClause][odata_filter], kontakta hello tabellen ovan under ”mappningar för filtersträngar” toofind hello REST API-dokumentationssida som motsvarar toohello lista åtgärden tooperform gärna.</span><span class="sxs-lookup"><span data-stu-id="61f24-227">When you construct a filter string for [ODATADetailLevel.FilterClause][odata_filter], consult hello table above under "Mappings for filter strings" toofind hello REST API documentation page that corresponds toohello list operation that you wish tooperform.</span></span> <span data-ttu-id="61f24-228">Du hittar hello filtrera egenskaper och deras stöds operatorer i hello första försök tabellen på sidan.</span><span class="sxs-lookup"><span data-stu-id="61f24-228">You will find hello filterable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="61f24-229">Om du inte vill tooretrieve alla aktiviteter vars avslutskoden var inte är noll, till exempel detta rad på [listan hello uppgifter som är kopplad till ett jobb] [ rest_list_tasks] anger hello tillämpliga egenskapssträng och tillåtna operatorer:</span><span class="sxs-lookup"><span data-stu-id="61f24-229">If you wish tooretrieve all tasks whose exit code was nonzero, for example, this row on [List hello tasks associated with a job][rest_list_tasks] specifies hello applicable property string and allowable operators:</span></span>

| <span data-ttu-id="61f24-230">Egenskap</span><span class="sxs-lookup"><span data-stu-id="61f24-230">Property</span></span> | <span data-ttu-id="61f24-231">Tillåtna åtgärder</span><span class="sxs-lookup"><span data-stu-id="61f24-231">Operations allowed</span></span> | <span data-ttu-id="61f24-232">Typ</span><span class="sxs-lookup"><span data-stu-id="61f24-232">Type</span></span> |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

<span data-ttu-id="61f24-233">Därför är hello Filtersträngen för att lista alla aktiviteter med slutkoden noll:</span><span class="sxs-lookup"><span data-stu-id="61f24-233">Thus, hello filter string for listing all tasks with a nonzero exit code would be:</span></span>

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a><span data-ttu-id="61f24-234">Exempel: skapa en väljer sträng</span><span class="sxs-lookup"><span data-stu-id="61f24-234">Example: construct a select string</span></span>
<span data-ttu-id="61f24-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], kontakta hello tabellen ovan under ”mappningar för väljer strängar” och navigera toohello REST API-sidan som motsvarar toohello typ av enhet som du är en lista med.</span><span class="sxs-lookup"><span data-stu-id="61f24-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], consult hello table above under "Mappings for select strings" and navigate toohello REST API page that corresponds toohello type of entity that you are listing.</span></span> <span data-ttu-id="61f24-236">Du hittar hello valbar egenskaper och deras stöds operatorer i hello första försök tabellen på sidan.</span><span class="sxs-lookup"><span data-stu-id="61f24-236">You will find hello selectable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="61f24-237">Om du inte vill tooretrieve endast hello-ID och kommandoraden för varje aktivitet i en lista, t.ex, du hittar dessa rader i hello tillämpliga tabellen på [hämta information om en aktivitet][rest_get_task]:</span><span class="sxs-lookup"><span data-stu-id="61f24-237">If you wish tooretrieve only hello ID and command line for each task in a list, for example, you will find these rows in hello applicable table on [Get information about a task][rest_get_task]:</span></span>

| <span data-ttu-id="61f24-238">Egenskap</span><span class="sxs-lookup"><span data-stu-id="61f24-238">Property</span></span> | <span data-ttu-id="61f24-239">Typ</span><span class="sxs-lookup"><span data-stu-id="61f24-239">Type</span></span> | <span data-ttu-id="61f24-240">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="61f24-240">Notes</span></span> |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

<span data-ttu-id="61f24-241">hello väljer sträng för att inkludera endast hello-ID och kommandoraden med uppgifterna listan blir:</span><span class="sxs-lookup"><span data-stu-id="61f24-241">hello select string for including only hello ID and command line with each listed task would then be:</span></span>

`id, commandLine`

## <a name="code-samples"></a><span data-ttu-id="61f24-242">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="61f24-242">Code samples</span></span>
### <a name="efficient-list-queries-code-sample"></a><span data-ttu-id="61f24-243">Kodexempel för effektiv listan frågor</span><span class="sxs-lookup"><span data-stu-id="61f24-243">Efficient list queries code sample</span></span>
<span data-ttu-id="61f24-244">Kolla in hello [EfficientListQueries] [ efficient_query_sample] exempelprojektet på GitHub toosee hur effektivt listan frågor kan påverka prestanda i ett program.</span><span class="sxs-lookup"><span data-stu-id="61f24-244">Check out hello [EfficientListQueries][efficient_query_sample] sample project on GitHub toosee how efficient list querying can affect performance in an application.</span></span> <span data-ttu-id="61f24-245">Den här C#-konsolprogram skapar och lägger till ett stort antal uppgifter tooa jobb.</span><span class="sxs-lookup"><span data-stu-id="61f24-245">This C# console application creates and adds a large number of tasks tooa job.</span></span> <span data-ttu-id="61f24-246">Sedan gör det flera anrop toohello [JobOperations.ListTasks] [ net_list_tasks] metoden och överför [ODATADetailLevel] [ odata] objekt som är konfigurerad med en annan egenskap värden toovary hello mängden data toobe returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-246">Then, it makes multiple calls toohello [JobOperations.ListTasks][net_list_tasks] method and passes [ODATADetailLevel][odata] objects that are configured with different property values toovary hello amount of data toobe returned.</span></span> <span data-ttu-id="61f24-247">Den genererar utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="61f24-247">It produces output similar toohello following:</span></span>

```
Adding 5000 tasks toojob jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER tooquery tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER toocontinue...
```

<span data-ttu-id="61f24-248">Som visas i hello förfluten tid minskar du avsevärt svarstider för frågan genom att begränsa hello egenskaper och hello antal objekt som returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-248">As shown in hello elapsed times, you can greatly lower query response times by limiting hello properties and hello number of items that are returned.</span></span> <span data-ttu-id="61f24-249">Du hittar det här och andra exempelprojekt i hello [azure-batch-samples] [ github_samples] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="61f24-249">You can find this and other sample projects in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span>

### <a name="batchmetrics-library-and-code-sample"></a><span data-ttu-id="61f24-250">BatchMetrics bibliotek och koden exempel</span><span class="sxs-lookup"><span data-stu-id="61f24-250">BatchMetrics library and code sample</span></span>
<span data-ttu-id="61f24-251">Dessutom toohello EfficientListQueries kodexemplet ovan, du kan hitta hello [BatchMetrics] [ batch_metrics] projekt i hello [azure-batch-samples] [ github_samples] GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="61f24-251">In addition toohello EfficientListQueries code sample above, you can find hello [BatchMetrics][batch_metrics] project in hello [azure-batch-samples][github_samples] GitHub repository.</span></span> <span data-ttu-id="61f24-252">Hej BatchMetrics exempelprojektet visar hur tooefficiently övervaka jobbförloppet i Azure Batch med hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="61f24-252">hello BatchMetrics sample project demonstrates how tooefficiently monitor Azure Batch job progress using hello Batch API.</span></span>

<span data-ttu-id="61f24-253">Hej [BatchMetrics] [ batch_metrics] exempel innehåller en .NET-klassbiblioteksprojektet som du kan införliva i dina projekt och ett enkelt kommandoradsverktyg programmet tooexercise och visar hello använder hello bibliotek.</span><span class="sxs-lookup"><span data-stu-id="61f24-253">hello [BatchMetrics][batch_metrics] sample includes a .NET class library project which you can incorporate into your own projects, and a simple command-line program tooexercise and demonstrate hello use of hello library.</span></span>

<span data-ttu-id="61f24-254">hello exempelprogrammet i hello projekt visar hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="61f24-254">hello sample application within hello project demonstrates hello following operations:</span></span>

1. <span data-ttu-id="61f24-255">Att markera specifika attribut i ordning toodownload endast hello egenskaper behöver du</span><span class="sxs-lookup"><span data-stu-id="61f24-255">Selecting specific attributes in order toodownload only hello properties you need</span></span>
2. <span data-ttu-id="61f24-256">Filtrering på tillstånd övergången gånger i ordning toodownload endast ändringar sedan senaste hello-fråga</span><span class="sxs-lookup"><span data-stu-id="61f24-256">Filtering on state transition times in order toodownload only changes since hello last query</span></span>

<span data-ttu-id="61f24-257">Exempelvis visas hello följande metod i hello BatchMetrics bibliotek.</span><span class="sxs-lookup"><span data-stu-id="61f24-257">For example, hello following method appears in hello BatchMetrics library.</span></span> <span data-ttu-id="61f24-258">Returnerar en ODATADetailLevel som anger att endast hello `id` och `state` egenskaper som ska hämtas för hello entiteter som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="61f24-258">It returns an ODATADetailLevel that specifies that only hello `id` and `state` properties should be obtained for hello entities that are queried.</span></span> <span data-ttu-id="61f24-259">Det även anger att endast enheter vars tillstånd har ändrats sedan hello anges `DateTime` parametern ska returneras.</span><span class="sxs-lookup"><span data-stu-id="61f24-259">It also specifies that only entities whose state has changed since hello specified `DateTime` parameter should be returned.</span></span>

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a><span data-ttu-id="61f24-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61f24-260">Next steps</span></span>
### <a name="parallel-node-tasks"></a><span data-ttu-id="61f24-261">Parallel-nod uppgifter</span><span class="sxs-lookup"><span data-stu-id="61f24-261">Parallel node tasks</span></span>
<span data-ttu-id="61f24-262">[Maximera Azure Batch beräkning Resursanvändning med samtidiga nod uppgifter](batch-parallel-node-tasks.md) är en annan artikel relaterade tooBatch programprestanda.</span><span class="sxs-lookup"><span data-stu-id="61f24-262">[Maximize Azure Batch compute resource usage with concurrent node tasks](batch-parallel-node-tasks.md) is another article related tooBatch application performance.</span></span> <span data-ttu-id="61f24-263">Vissa typer av arbetsbelastningar kan dra nytta av köra parallella aktiviteter på större-- men färre--compute-noder.</span><span class="sxs-lookup"><span data-stu-id="61f24-263">Some types of workloads can benefit from executing parallel tasks on larger--but fewer--compute nodes.</span></span> <span data-ttu-id="61f24-264">Kolla in hello [Exempelscenario](batch-parallel-node-tasks.md#example-scenario) i hello artikeln för information om sådant scenario.</span><span class="sxs-lookup"><span data-stu-id="61f24-264">Check out hello [example scenario](batch-parallel-node-tasks.md#example-scenario) in hello article for details on such a scenario.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="61f24-265">Batch-Forum</span><span class="sxs-lookup"><span data-stu-id="61f24-265">Batch Forum</span></span>
<span data-ttu-id="61f24-266">Hej [Azure Batch-Forum] [ forum] är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN.</span><span class="sxs-lookup"><span data-stu-id="61f24-266">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="61f24-267">HEAD på över för användbara ”Fäst” inlägg och publicera dina frågor när de uppstår när du skapar Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="61f24-267">Head on over for helpful "sticky" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job