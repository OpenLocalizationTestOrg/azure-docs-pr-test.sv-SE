---
title: aaaScheduler begrepp och entiteter | Microsoft Docs
description: "Begrepp, terminologi och entitetshierarki, inklusive jobb och jobbsamlingar, relaterade till Azure Scheduler.  Innehåller ett omfattande exempel på ett schemalagt jobb."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a><span data-ttu-id="03582-104">Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-104">Scheduler concepts, terminology, + entity hierarchy</span></span>
## <a name="scheduler-entity-hierarchy"></a><span data-ttu-id="03582-105">Entitetshierarki i Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-105">Scheduler entity hierarchy</span></span>
<span data-ttu-id="03582-106">hello beskrivs följande tabell hello huvudsakliga resurser visas eller används av hello Scheduler API:</span><span class="sxs-lookup"><span data-stu-id="03582-106">hello following table describes hello main resources exposed or used by hello Scheduler API:</span></span>

| <span data-ttu-id="03582-107">Resurs</span><span class="sxs-lookup"><span data-stu-id="03582-107">Resource</span></span> | <span data-ttu-id="03582-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="03582-108">Description</span></span> |
| --- | --- |
| <span data-ttu-id="03582-109">**Jobbsamling**</span><span class="sxs-lookup"><span data-stu-id="03582-109">**Job collection**</span></span> |<span data-ttu-id="03582-110">En jobbsamling innehåller ett antal jobb och underhåller inställningar, kvoter och begränsningar som delas av jobb i hello samling.</span><span class="sxs-lookup"><span data-stu-id="03582-110">A job collection contains a group of jobs and maintains settings, quotas, and throttles that are shared by jobs within hello collection.</span></span> <span data-ttu-id="03582-111">En jobbsamling skapas av en prenumerationsägare och grupperar jobb baserat på användnings- eller programgränser.</span><span class="sxs-lookup"><span data-stu-id="03582-111">A job collection is created by a subscription owner and groups jobs together based on usage or application boundaries.</span></span> <span data-ttu-id="03582-112">Det är begränsad tooone region.</span><span class="sxs-lookup"><span data-stu-id="03582-112">It’s constrained tooone region.</span></span> <span data-ttu-id="03582-113">Det gör även hello verkställandet av kvoter tooconstrain hello användning för alla jobb i samlingen.</span><span class="sxs-lookup"><span data-stu-id="03582-113">It also allows hello enforcement of quotas tooconstrain hello usage of all jobs in that collection.</span></span> <span data-ttu-id="03582-114">hello kvoter inkluderar MaxJobs och maxrecurrence-värdet.</span><span class="sxs-lookup"><span data-stu-id="03582-114">hello quotas include MaxJobs and MaxRecurrence.</span></span> |
| <span data-ttu-id="03582-115">**Jobb**</span><span class="sxs-lookup"><span data-stu-id="03582-115">**Job**</span></span> |<span data-ttu-id="03582-116">Ett jobb definierar en enskild återkommande åtgärd med enkla eller komplexa strategier för körning.</span><span class="sxs-lookup"><span data-stu-id="03582-116">A job defines a single recurrent action, with simple or complex strategies for execution.</span></span> <span data-ttu-id="03582-117">Exempel på åtgärder är HTTP, lagringskö, Service Bus-kö eller Service Bus-ämnesförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="03582-117">Actions may include HTTP, storage queue, service bus queue, or service bus topic requests.</span></span> |
| <span data-ttu-id="03582-118">**Jobbhistorik**</span><span class="sxs-lookup"><span data-stu-id="03582-118">**Job history**</span></span> |<span data-ttu-id="03582-119">En jobbhistorik representerar information om körningen av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="03582-119">A job history represents details for an execution of a job.</span></span> <span data-ttu-id="03582-120">Den innehåller information om huruvida jobbet lyckades eller misslyckades, samt eventuell svarsinformation.</span><span class="sxs-lookup"><span data-stu-id="03582-120">It contains success vs. failure, as well as any response details.</span></span> |

## <a name="scheduler-entity-management"></a><span data-ttu-id="03582-121">Entitetshantering i Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-121">Scheduler entity management</span></span>
<span data-ttu-id="03582-122">Exponera hello följande åtgärder på hello resurser på en hög nivå hello Schemaläggaren och hello service management API:</span><span class="sxs-lookup"><span data-stu-id="03582-122">At a high level, hello scheduler and hello service management API expose hello following operations on hello resources:</span></span>

| <span data-ttu-id="03582-123">Funktion</span><span class="sxs-lookup"><span data-stu-id="03582-123">Capability</span></span> | <span data-ttu-id="03582-124">Beskrivning och URI-adress</span><span class="sxs-lookup"><span data-stu-id="03582-124">Description and URI address</span></span> |
| --- | --- |
| <span data-ttu-id="03582-125">**Hantering av jobbsamlingar**</span><span class="sxs-lookup"><span data-stu-id="03582-125">**Job collection management**</span></span> |<span data-ttu-id="03582-126">Hämta, PLACERA och ta bort stöd för att skapa och ändra jobbsamlingar och jobb för hello som finns där.</span><span class="sxs-lookup"><span data-stu-id="03582-126">GET, PUT, and DELETE support for creating and modifying job collections and hello jobs contained therein.</span></span> <span data-ttu-id="03582-127">En jobbsamling är en behållare för jobb och mappa tooquotas och delade inställningar.</span><span class="sxs-lookup"><span data-stu-id="03582-127">A job collection is a container for jobs and maps tooquotas and shared settings.</span></span> <span data-ttu-id="03582-128">Exempel på kvoter, som beskrivs senare, är högsta antal jobb och minsta intervall.</span><span class="sxs-lookup"><span data-stu-id="03582-128">Examples of quotas, described later, are maximum number of jobs and smallest recurrence interval.</span></span> <p><span data-ttu-id="03582-129">PUT och DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="03582-129">PUT and DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p><p><span data-ttu-id="03582-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="03582-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p> |
| <span data-ttu-id="03582-131">**Jobbhantering**</span><span class="sxs-lookup"><span data-stu-id="03582-131">**Job management**</span></span> |<span data-ttu-id="03582-132">Stöd för GET, PUT, POST, PATCH och DELETE för att skapa och ändra jobb.</span><span class="sxs-lookup"><span data-stu-id="03582-132">GET, PUT, POST, PATCH, and DELETE support for creating and modifying jobs.</span></span> <span data-ttu-id="03582-133">Alla jobb måste tillhöra tooa jobbsamlingen som redan finns, så det finns ingen implicit skapande.</span><span class="sxs-lookup"><span data-stu-id="03582-133">All jobs must belong tooa job collection that already exists, so there is no implicit creation.</span></span> <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| <span data-ttu-id="03582-134">**Hantering av jobbhistorik**</span><span class="sxs-lookup"><span data-stu-id="03582-134">**Job history management**</span></span> |<span data-ttu-id="03582-135">Stöd för GET för att hämta 60 dagars jobbkörningshistorik, t.ex. förfluten tid och körningsresultat.</span><span class="sxs-lookup"><span data-stu-id="03582-135">GET support for fetching 60 days of job execution history, such as job elapsed time and job execution results.</span></span> <span data-ttu-id="03582-136">Lägger till parameterstöd för frågesträngar för filtrering baserat på tillstånd och status.</span><span class="sxs-lookup"><span data-stu-id="03582-136">Adds query string parameter support for filtering based on state and status.</span></span> <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a><span data-ttu-id="03582-137">Jobbtyper</span><span class="sxs-lookup"><span data-stu-id="03582-137">Job types</span></span>
<span data-ttu-id="03582-138">Det finns flera typer av jobb: HTTP-jobb (inklusive HTTPS-jobb som stöder SSL), jobb för lagringsköer, Service Bus-köjobb och Service Bus-ämnesjobb.</span><span class="sxs-lookup"><span data-stu-id="03582-138">There are multiple types of jobs: HTTP jobs (including HTTPS jobs that support SSL), storage queue jobs, service bus queue jobs, and service bus topic jobs.</span></span> <span data-ttu-id="03582-139">HTTP-jobben är idealiska om du har en slutpunkt för en befintlig arbetsbelastning eller tjänst.</span><span class="sxs-lookup"><span data-stu-id="03582-139">HTTP jobs are ideal if you have an endpoint of an existing workload or service.</span></span> <span data-ttu-id="03582-140">Du kan använda lagring kön jobb toopost meddelanden toostorage köer, så att dessa jobb är idealisk för arbetsbelastningar som använder lagringsköer.</span><span class="sxs-lookup"><span data-stu-id="03582-140">You can use storage queue jobs toopost messages toostorage queues, so those jobs are ideal for workloads that use storage queues.</span></span> <span data-ttu-id="03582-141">På liknande sätt är Service Bus-jobb idealiska för arbetsbelastningar som använder Service Bus-köer och Service Bus-ämnen.</span><span class="sxs-lookup"><span data-stu-id="03582-141">Similarly, service bus jobs are ideal for workloads that use service bus queues and topics.</span></span>

## <a name="hello-job-entity-in-detail"></a><span data-ttu-id="03582-142">Hej ”jobbet” entitet i detalj</span><span class="sxs-lookup"><span data-stu-id="03582-142">hello "job" entity in detail</span></span>
<span data-ttu-id="03582-143">På en grundläggande nivå har ett schemalagt jobb flera delar:</span><span class="sxs-lookup"><span data-stu-id="03582-143">At a basic level, a scheduled job has several parts:</span></span>

* <span data-ttu-id="03582-144">hello åtgärd tooperform när hello jobbet timer utlöses</span><span class="sxs-lookup"><span data-stu-id="03582-144">hello action tooperform when hello job timer fires</span></span>  
* <span data-ttu-id="03582-145">(Valfritt) hello toorun hello jobbet</span><span class="sxs-lookup"><span data-stu-id="03582-145">(Optional) hello time toorun hello job</span></span>  
* <span data-ttu-id="03582-146">(Valfritt) När och hur ofta toorepeat hello jobb</span><span class="sxs-lookup"><span data-stu-id="03582-146">(Optional) When and how often toorepeat hello job</span></span>  
* <span data-ttu-id="03582-147">(Valfritt) En åtgärd toofire om hello primär åtgärd misslyckas</span><span class="sxs-lookup"><span data-stu-id="03582-147">(Optional) An action toofire if hello primary action fails</span></span>  

<span data-ttu-id="03582-148">Internt, innehåller också ett schemalagt jobb systemdefinierade data, till exempel hello nästa schemalagda körning tid.</span><span class="sxs-lookup"><span data-stu-id="03582-148">Internally, a scheduled job also contains system-provided data such as hello next scheduled execution time.</span></span>

<span data-ttu-id="03582-149">hello följande kod innehåller en omfattande exempel på ett schemalagt jobb.</span><span class="sxs-lookup"><span data-stu-id="03582-149">hello following code provides a comprehensive example of a scheduled job.</span></span> <span data-ttu-id="03582-150">Information finns i de efterföljande avsnitten.</span><span class="sxs-lookup"><span data-stu-id="03582-150">Details are provided in subsequent sections.</span></span>

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

<span data-ttu-id="03582-151">Som det visas i hello exempel schemalagt jobb ovan, har en jobbdefinition flera delar:</span><span class="sxs-lookup"><span data-stu-id="03582-151">As seen in hello sample scheduled job above, a job definition has several parts:</span></span>

* <span data-ttu-id="03582-152">Starttid (”startTime”)</span><span class="sxs-lookup"><span data-stu-id="03582-152">Start time (“startTime”)</span></span>  
* <span data-ttu-id="03582-153">Åtgärd (”action”), som även omfattar en felåtgärd (”errorAction”)</span><span class="sxs-lookup"><span data-stu-id="03582-153">Action (“action”), which includes error action (“errorAction”)</span></span>
* <span data-ttu-id="03582-154">Upprepning (”recurrence”)</span><span class="sxs-lookup"><span data-stu-id="03582-154">Recurrence (“recurrence”)</span></span>  
* <span data-ttu-id="03582-155">Tillstånd (”state”)</span><span class="sxs-lookup"><span data-stu-id="03582-155">State (“state”)</span></span>  
* <span data-ttu-id="03582-156">Status (”status”)</span><span class="sxs-lookup"><span data-stu-id="03582-156">Status (“status”)</span></span>  
* <span data-ttu-id="03582-157">Återförsöksprincip (”retryPolicy”)</span><span class="sxs-lookup"><span data-stu-id="03582-157">Retry policy (“retryPolicy”)</span></span>  

<span data-ttu-id="03582-158">Nu ska vi titta närmare på var och en av dessa delar:</span><span class="sxs-lookup"><span data-stu-id="03582-158">Let’s examine each of these in detail:</span></span>

## <a name="starttime"></a><span data-ttu-id="03582-159">startTime</span><span class="sxs-lookup"><span data-stu-id="03582-159">startTime</span></span>
<span data-ttu-id="03582-160">Hej ”startTime” är hello starttid och gör att hello anroparen toospecify en tidszon förskjutning hello uppkopplat läge i [ISO 8601-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="03582-160">hello "startTime” is hello start time and allows hello caller toospecify a time zone offset on hello wire in [ISO-8601 format](http://en.wikipedia.org/wiki/ISO_8601).</span></span>

## <a name="action-and-erroraction"></a><span data-ttu-id="03582-161">action och errorAction</span><span class="sxs-lookup"><span data-stu-id="03582-161">action and errorAction</span></span>
<span data-ttu-id="03582-162">hello ”åtgärden” hello-åtgärd som anropas för varje förekomst och beskriver en typ av tjänst-anrop.</span><span class="sxs-lookup"><span data-stu-id="03582-162">hello “action” is hello action invoked on each occurrence and describes a type of service invocation.</span></span> <span data-ttu-id="03582-163">hello-åtgärden är vad kommer att utföras på hello tillhandahålls schema.</span><span class="sxs-lookup"><span data-stu-id="03582-163">hello action is what will be executed on hello provided schedule.</span></span> <span data-ttu-id="03582-164">Scheduler stöder HTTP, lagringskö, Service Bus-ämne och Service Bus-köåtgärder.</span><span class="sxs-lookup"><span data-stu-id="03582-164">Scheduler supports HTTP, storage queue, service bus topic, and service bus queue actions.</span></span>

<span data-ttu-id="03582-165">hello är i hello-exemplet ovan en HTTP-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="03582-165">hello action in hello example above is an HTTP action.</span></span> <span data-ttu-id="03582-166">Nedan är ett exempel på en åtgärd för lagringskön:</span><span class="sxs-lookup"><span data-stu-id="03582-166">Below is an example of a storage queue action:</span></span>

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

<span data-ttu-id="03582-167">Nedan är ett exempel på en Service Bus-ämnesåtgärd.</span><span class="sxs-lookup"><span data-stu-id="03582-167">Below is an example of a service bus topic action.</span></span>

  <span data-ttu-id="03582-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span><span class="sxs-lookup"><span data-stu-id="03582-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span></span>  
      <span data-ttu-id="03582-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Kan vara antingen netMessaging eller AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span><span class="sxs-lookup"><span data-stu-id="03582-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span></span>

<span data-ttu-id="03582-170">Nedan är ett exempel på en Service Bus-köåtgärd:</span><span class="sxs-lookup"><span data-stu-id="03582-170">Below is an example of a service bus queue action:</span></span>

  <span data-ttu-id="03582-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span><span class="sxs-lookup"><span data-stu-id="03582-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span></span>  
      <span data-ttu-id="03582-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Kan vara antingen netMessaging eller AMQP "authentication": {</span><span class="sxs-lookup"><span data-stu-id="03582-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span></span>  
        <span data-ttu-id="03582-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span><span class="sxs-lookup"><span data-stu-id="03582-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span></span>  
      <span data-ttu-id="03582-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span><span class="sxs-lookup"><span data-stu-id="03582-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span></span>

<span data-ttu-id="03582-175">Hej ”errorAction” är hello felhanterare, hello-åtgärd som anropas när hello primära åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="03582-175">hello “errorAction” is hello error handler, hello action invoked when hello primary action fails.</span></span> <span data-ttu-id="03582-176">Du kan använda den här variabeln toocall en felhantering slutpunkt eller skicka ett meddelande till användare.</span><span class="sxs-lookup"><span data-stu-id="03582-176">You can use this variable toocall an error-handling endpoint or send a user notification.</span></span> <span data-ttu-id="03582-177">Detta kan användas för att nå en sekundär slutpunkt i hello fall att hello primära är inte tillgänglig (t.ex. i hello fallet med en katastrof på hello endpoint platsen) eller kan användas för att meddela en felhantering slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="03582-177">This can be used for reaching a secondary endpoint in hello case that hello primary is not available (e.g., in hello case of a disaster at hello endpoint’s site) or can be used for notifying an error handling endpoint.</span></span> <span data-ttu-id="03582-178">Precis som hello primär åtgärd vara hello felåtgärd enkla eller sammansatta logik baserat på andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="03582-178">Just like hello primary action, hello error action can be simple or composite logic based on other actions.</span></span> <span data-ttu-id="03582-179">hur toocreate SAS-token finns för toolearn[skapar och använder en signatur för delad åtkomst](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="03582-179">toolearn how toocreate a SAS token, refer too[Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="recurrence"></a><span data-ttu-id="03582-180">recurrence</span><span class="sxs-lookup"><span data-stu-id="03582-180">recurrence</span></span>
<span data-ttu-id="03582-181">Upprepningskomponenten har flera delar:</span><span class="sxs-lookup"><span data-stu-id="03582-181">Recurrence has several parts:</span></span>

* <span data-ttu-id="03582-182">Frekvens: Minut, timme, dag, vecka, månad eller år.</span><span class="sxs-lookup"><span data-stu-id="03582-182">Frequency: One of minute, hour, day, week, month, year</span></span>  
* <span data-ttu-id="03582-183">Intervall: Intervall på hello angivna frekvensen för hello upprepning</span><span class="sxs-lookup"><span data-stu-id="03582-183">Interval: Interval at hello given frequency for hello recurrence</span></span>  
* <span data-ttu-id="03582-184">Föreskrivna schema: Ange minuter, timmar, veckodagar, månader och monthdays av hello-intervall</span><span class="sxs-lookup"><span data-stu-id="03582-184">Prescribed schedule: Specify minutes, hours, weekdays, months, and monthdays of hello recurrence</span></span>  
* <span data-ttu-id="03582-185">Antal: Antal förekomster.</span><span class="sxs-lookup"><span data-stu-id="03582-185">Count: Count of occurrences</span></span>  
* <span data-ttu-id="03582-186">Sluttid: inga jobb körs när hello angett sluttid</span><span class="sxs-lookup"><span data-stu-id="03582-186">End time: No jobs will execute after hello specified end time</span></span>  

<span data-ttu-id="03582-187">Ett jobb är återkommande om ett återkommande objekt har angetts i jobbets JSON-definition.</span><span class="sxs-lookup"><span data-stu-id="03582-187">A job is recurring if it has a recurring object specified in its JSON definition.</span></span> <span data-ttu-id="03582-188">Om både antal och endTime har angetts måste hanteras hello slutförande regel som inträffar först.</span><span class="sxs-lookup"><span data-stu-id="03582-188">If both count and endTime are specified, hello completion rule that occurs first is honored.</span></span>

## <a name="state"></a><span data-ttu-id="03582-189">state</span><span class="sxs-lookup"><span data-stu-id="03582-189">state</span></span>
<span data-ttu-id="03582-190">hello hello jobb är en av fyra värden: aktiverat, inaktiverat, slutfört eller fel.</span><span class="sxs-lookup"><span data-stu-id="03582-190">hello state of hello job is one of four values: enabled, disabled, completed, or faulted.</span></span> <span data-ttu-id="03582-191">Du kan publicera eller korrigering jobb så som tooupdate dem toohello aktiverat eller inaktiverat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="03582-191">You can PUT or PATCH jobs so as tooupdate them toohello enabled or disabled state.</span></span> <span data-ttu-id="03582-192">Om ett jobb har slutförts eller fel som är ett slutgiltigt tillstånd som inte kan uppdateras (även om hello jobbet kan fortfarande tas bort).</span><span class="sxs-lookup"><span data-stu-id="03582-192">If a job has been completed or faulted, that is a final state that cannot be updated (though hello job can still be DELETED).</span></span> <span data-ttu-id="03582-193">Ett exempel på hello tillstånd egenskapen är följande:</span><span class="sxs-lookup"><span data-stu-id="03582-193">An example of hello state property is as follows:</span></span>

        "state": "disabled", // enabled, disabled, completed, or faulted
<span data-ttu-id="03582-194">Jobb med tillståndet completed eller faulted tas bort efter 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="03582-194">Completed and faulted jobs are deleted after 60 days.</span></span>

## <a name="status"></a><span data-ttu-id="03582-195">status</span><span class="sxs-lookup"><span data-stu-id="03582-195">status</span></span>
<span data-ttu-id="03582-196">När ett jobb i Schemaläggaren har börjat returneras information om hello hello jobbets aktuella status.</span><span class="sxs-lookup"><span data-stu-id="03582-196">Once a Scheduler job has started, information will be returned about hello current status of hello job.</span></span> <span data-ttu-id="03582-197">Det här objektet går inte att ange hello användare – anges som hello system.</span><span class="sxs-lookup"><span data-stu-id="03582-197">This object is not settable by hello user—it’s set by hello system.</span></span> <span data-ttu-id="03582-198">Men ingår den i hello jobbobjektet (snarare än en separat länkade resurs) så att ett enkelt kan hämta hello status för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="03582-198">However, it is included in hello job object (rather than a separate linked resource) so that one can obtain hello status of a job easily.</span></span>

<span data-ttu-id="03582-199">Jobbstatus innehåller hello tiden för hello tidigare körning (eventuella) och hello hello nästa schemalagda körningstid (för pågående jobb) och hello körning antal hello jobb.</span><span class="sxs-lookup"><span data-stu-id="03582-199">Job status includes hello time of hello previous execution (if any), hello time of hello next scheduled execution (for in-progress jobs), and hello execution count of hello job.</span></span>

## <a name="retrypolicy"></a><span data-ttu-id="03582-200">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="03582-200">retryPolicy</span></span>
<span data-ttu-id="03582-201">Om ett jobb i Schemaläggaren misslyckas är möjliga toospecify en försök princip toodetermine om och hur hello åtgärden igen.</span><span class="sxs-lookup"><span data-stu-id="03582-201">If a Scheduler job fails, it is possible toospecify a retry policy toodetermine whether and how hello action is retried.</span></span> <span data-ttu-id="03582-202">Detta bestäms av hello **retryType** objekt – anges för**ingen** om det inte finns några återförsöksprincip som ovan.</span><span class="sxs-lookup"><span data-stu-id="03582-202">This is determined by hello **retryType** object—it is set too**none** if there is no retry policy, as shown above.</span></span> <span data-ttu-id="03582-203">Ställa in också**fast** om det finns en återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="03582-203">Set it too**fixed** if there is a retry policy.</span></span>

<span data-ttu-id="03582-204">tooset en återförsöksprincip två ytterligare inställningar kan anges: ett Återförsöksintervall (**retryInterval**) och hello antal återförsök (**retryCount**).</span><span class="sxs-lookup"><span data-stu-id="03582-204">tooset a retry policy, two additional settings may be specified: a retry interval (**retryInterval**) and hello number of retries (**retryCount**).</span></span>

<span data-ttu-id="03582-205">hello återförsöksintervall anges med hello **retryInterval** objekt, är hello intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="03582-205">hello retry interval, specified with hello **retryInterval** object, is hello interval between retries.</span></span> <span data-ttu-id="03582-206">Standardvärdet är 30 sekunder. Det minsta konfigurerbara värdet är 15 sekunder och det högsta värdet är 18 månader.</span><span class="sxs-lookup"><span data-stu-id="03582-206">Its default value is 30 seconds, its minimum configurable value is 15 seconds, and its maximum value is 18 months.</span></span> <span data-ttu-id="03582-207">Jobb i kostnadsfria jobbsamlingar har ett minsta konfigurerbart värde på 1 timme.</span><span class="sxs-lookup"><span data-stu-id="03582-207">Jobs in Free job collections have a minimum configurable value of 1 hour.</span></span>  <span data-ttu-id="03582-208">Det har definierats i hello ISO 8601-format.</span><span class="sxs-lookup"><span data-stu-id="03582-208">It is defined in hello ISO 8601 format.</span></span> <span data-ttu-id="03582-209">På liknande sätt hello värdet för hello antal försök har angetts med hello **retryCount** objekt; det är hello antalet gånger som ett nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="03582-209">Similarly, hello value of hello number of retries is specified with hello **retryCount** object; it is hello number of times a retry is attempted.</span></span> <span data-ttu-id="03582-210">Standardvärdet är 4 och maxvärdet är 20\.</span><span class="sxs-lookup"><span data-stu-id="03582-210">Its default value is 4, and its maximum value is 20\.</span></span> <span data-ttu-id="03582-211">Båda **retryInterval** och **retryCount** är valfria.</span><span class="sxs-lookup"><span data-stu-id="03582-211">Both **retryInterval** and **retryCount** are optional.</span></span> <span data-ttu-id="03582-212">De ges sina standardvärden om **retryType** har angetts för**fast** och inga värden anges explicit.</span><span class="sxs-lookup"><span data-stu-id="03582-212">They are given their default values if **retryType** is set too**fixed** and no values are specified explicitly.</span></span>

## <a name="see-also"></a><span data-ttu-id="03582-213">Se även</span><span class="sxs-lookup"><span data-stu-id="03582-213">See also</span></span>
 [<span data-ttu-id="03582-214">Vad är Scheduler?</span><span class="sxs-lookup"><span data-stu-id="03582-214">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="03582-215">Komma igång med Schemaläggaren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="03582-215">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="03582-216">Prenumerationer och fakturering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-216">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="03582-217">Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren</span><span class="sxs-lookup"><span data-stu-id="03582-217">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="03582-218">Referens för REST-API:et för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-218">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="03582-219">Referens för PowerShell-cmdlets för Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-219">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="03582-220">Hög tillgänglighet och tillförlitlighet i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-220">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="03582-221">Gränser, standardinställningar och felkoder i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-221">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="03582-222">Utgående autentisering i Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="03582-222">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

