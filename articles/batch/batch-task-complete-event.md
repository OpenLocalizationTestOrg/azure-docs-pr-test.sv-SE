---
title: "Azure Batch uppgiften händelsen klar | Microsoft Docs"
description: "Referens för Batch uppgiften complete-händelsen."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 015adf7dbc47c29a78df4e4889b2ee1ddcccdd8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="task-complete-event"></a><span data-ttu-id="3e36a-103">Fullständig händelseuppgiften</span><span class="sxs-lookup"><span data-stu-id="3e36a-103">Task complete event</span></span>

 <span data-ttu-id="3e36a-104">Denna händelse genereras när en aktivitet har slutförts, oavsett slutkoden.</span><span class="sxs-lookup"><span data-stu-id="3e36a-104">This event is emitted once a task is completed, regardless of the exit code.</span></span> <span data-ttu-id="3e36a-105">Den här händelsen kan användas för att fastställa varaktigheten för en aktivitet där aktiviteten kördes och om den gjordes.</span><span class="sxs-lookup"><span data-stu-id="3e36a-105">This event can be used to determine the duration of a task, where the task ran, and whether it was retried.</span></span>


 <span data-ttu-id="3e36a-106">I följande exempel visar innehållet i en aktivitet slutförts händelse.</span><span class="sxs-lookup"><span data-stu-id="3e36a-106">The following example shows the body of a task complete event.</span></span>

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 0,
        "retryCount": 0,
        "requeueCount": 0
    }
}
```

|<span data-ttu-id="3e36a-107">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3e36a-107">Element name</span></span>|<span data-ttu-id="3e36a-108">Typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-108">Type</span></span>|<span data-ttu-id="3e36a-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3e36a-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="3e36a-110">JobId</span><span class="sxs-lookup"><span data-stu-id="3e36a-110">jobId</span></span>|<span data-ttu-id="3e36a-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e36a-111">String</span></span>|<span data-ttu-id="3e36a-112">Id för jobbet som innehåller aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e36a-112">The id of the job containing the task.</span></span>|
|<span data-ttu-id="3e36a-113">id</span><span class="sxs-lookup"><span data-stu-id="3e36a-113">id</span></span>|<span data-ttu-id="3e36a-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e36a-114">String</span></span>|<span data-ttu-id="3e36a-115">Id för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e36a-115">The id of the task.</span></span>|
|<span data-ttu-id="3e36a-116">taskType</span><span class="sxs-lookup"><span data-stu-id="3e36a-116">taskType</span></span>|<span data-ttu-id="3e36a-117">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e36a-117">String</span></span>|<span data-ttu-id="3e36a-118">Typ av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="3e36a-118">The type of the task.</span></span> <span data-ttu-id="3e36a-119">Detta kan vara visar det är en projektaktivitet manager ' JobManager' eller 'User' som anger den inte är en projektaktivitet manager.</span><span class="sxs-lookup"><span data-stu-id="3e36a-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span> <span data-ttu-id="3e36a-120">Den här händelsen har inte genererats för jobbet förberedande uppgifter, versionen jobbuppgifter eller starta uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3e36a-120">This event is not emitted for job preparation tasks, job release tasks or start tasks.</span></span>|
|<span data-ttu-id="3e36a-121">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="3e36a-121">systemTaskVersion</span></span>|<span data-ttu-id="3e36a-122">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-122">Int32</span></span>|<span data-ttu-id="3e36a-123">Detta är räknaren interna försök för en uppgift.</span><span class="sxs-lookup"><span data-stu-id="3e36a-123">This is the internal retry counter on a task.</span></span> <span data-ttu-id="3e36a-124">Internt kan Batch-tjänsten försöka med att en aktivitet för att tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="3e36a-124">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="3e36a-125">De här problemen kan innehålla interna schemaläggning fel eller försöker återställa från compute-noder i ett felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3e36a-125">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="3e36a-126">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="3e36a-126">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="3e36a-127">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-127">Complex Type</span></span>|<span data-ttu-id="3e36a-128">Innehåller information om Beräkningsnoden där aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="3e36a-128">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="3e36a-129">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="3e36a-129">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="3e36a-130">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-130">Complex Type</span></span>|<span data-ttu-id="3e36a-131">Anger att aktiviteten är en aktivitet för flera instanser som kräver flera compute-noder.</span><span class="sxs-lookup"><span data-stu-id="3e36a-131">Specifies that the task is a Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="3e36a-132">Se [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) mer information.</span><span class="sxs-lookup"><span data-stu-id="3e36a-132">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="3e36a-133">villkor</span><span class="sxs-lookup"><span data-stu-id="3e36a-133">constraints</span></span>](#constraints)|<span data-ttu-id="3e36a-134">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-134">Complex Type</span></span>|<span data-ttu-id="3e36a-135">Begränsningar för körning som gäller för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="3e36a-135">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="3e36a-136">executionInfo</span><span class="sxs-lookup"><span data-stu-id="3e36a-136">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="3e36a-137">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-137">Complex Type</span></span>|<span data-ttu-id="3e36a-138">Innehåller information om körning av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="3e36a-138">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="3e36a-139"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="3e36a-139"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="3e36a-140">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3e36a-140">Element name</span></span>|<span data-ttu-id="3e36a-141">Typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-141">Type</span></span>|<span data-ttu-id="3e36a-142">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3e36a-142">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="3e36a-143">poolId</span><span class="sxs-lookup"><span data-stu-id="3e36a-143">poolId</span></span>|<span data-ttu-id="3e36a-144">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e36a-144">String</span></span>|<span data-ttu-id="3e36a-145">Id för poolen som aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="3e36a-145">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="3e36a-146">nodeId</span><span class="sxs-lookup"><span data-stu-id="3e36a-146">nodeId</span></span>|<span data-ttu-id="3e36a-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e36a-147">String</span></span>|<span data-ttu-id="3e36a-148">Id för noden som aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="3e36a-148">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="3e36a-149"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="3e36a-149"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="3e36a-150">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3e36a-150">Element name</span></span>|<span data-ttu-id="3e36a-151">Typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-151">Type</span></span>|<span data-ttu-id="3e36a-152">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3e36a-152">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="3e36a-153">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="3e36a-153">numberOfInstances</span></span>|<span data-ttu-id="3e36a-154">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-154">Int32</span></span>|<span data-ttu-id="3e36a-155">Antalet beräkningsnoder som uppgiften kräver.</span><span class="sxs-lookup"><span data-stu-id="3e36a-155">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="3e36a-156"><a name="constraints"></a>villkor</span><span class="sxs-lookup"><span data-stu-id="3e36a-156"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="3e36a-157">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3e36a-157">Element name</span></span>|<span data-ttu-id="3e36a-158">Typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-158">Type</span></span>|<span data-ttu-id="3e36a-159">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3e36a-159">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="3e36a-160">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="3e36a-160">maxTaskRetryCount</span></span>|<span data-ttu-id="3e36a-161">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-161">Int32</span></span>|<span data-ttu-id="3e36a-162">Maximalt antal gånger uppgiften får köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="3e36a-162">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="3e36a-163">Batch-tjänsten försöker upprätta en uppgift om dess slutkod inte är noll.</span><span class="sxs-lookup"><span data-stu-id="3e36a-163">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="3e36a-164">Observera att det här värdet uttryckligen styr antalet försök.</span><span class="sxs-lookup"><span data-stu-id="3e36a-164">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="3e36a-165">Batch-tjänsten försöker aktiviteten en gång och försök sedan upp till den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="3e36a-165">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="3e36a-166">Till exempel maximalt antal försök är 3, Batch försöker en aktivitet upp till gånger 4 (ett första försök och 3 försök).</span><span class="sxs-lookup"><span data-stu-id="3e36a-166">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="3e36a-167">Maximalt antal försök är 0, försök uppgifter inte igen av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e36a-167">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="3e36a-168">Om det högsta antalet nya försök är -1, försöker Batch-tjänsten aktiviteter utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="3e36a-168">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="3e36a-169">Standardvärdet är 0 (inga nya försök).</span><span class="sxs-lookup"><span data-stu-id="3e36a-169">The default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="3e36a-170"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="3e36a-170"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="3e36a-171">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="3e36a-171">Element name</span></span>|<span data-ttu-id="3e36a-172">Typ</span><span class="sxs-lookup"><span data-stu-id="3e36a-172">Type</span></span>|<span data-ttu-id="3e36a-173">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3e36a-173">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="3e36a-174">startTime</span><span class="sxs-lookup"><span data-stu-id="3e36a-174">startTime</span></span>|<span data-ttu-id="3e36a-175">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e36a-175">DateTime</span></span>|<span data-ttu-id="3e36a-176">Den tid då aktiviteten startades.</span><span class="sxs-lookup"><span data-stu-id="3e36a-176">The time at which the task started running.</span></span> <span data-ttu-id="3e36a-177">'Körs' motsvarar den **kör** tillstånd, så om aktiviteten anger resursfiler eller programpaket, visar starttiden tidpunkten då aktiviteten startades hämtas eller distribution av dessa.</span><span class="sxs-lookup"><span data-stu-id="3e36a-177">'Running' corresponds to the **running** state, so if the task specifies resource files or application packages, then the start time reflects the time at which the task started downloading or deploying these.</span></span>  <span data-ttu-id="3e36a-178">Om aktiviteten har startats om eller ett nytt försök, är det senaste tillfälle då aktiviteten startades.</span><span class="sxs-lookup"><span data-stu-id="3e36a-178">If the task has been restarted or retried, this is the most recent time at which the task started running.</span></span>|
|<span data-ttu-id="3e36a-179">Sluttid</span><span class="sxs-lookup"><span data-stu-id="3e36a-179">endTime</span></span>|<span data-ttu-id="3e36a-180">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e36a-180">DateTime</span></span>|<span data-ttu-id="3e36a-181">Den tid då aktiviteten är slutförd.</span><span class="sxs-lookup"><span data-stu-id="3e36a-181">The time at which the task completed.</span></span>|
|<span data-ttu-id="3e36a-182">Slutkod</span><span class="sxs-lookup"><span data-stu-id="3e36a-182">exitCode</span></span>|<span data-ttu-id="3e36a-183">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-183">Int32</span></span>|<span data-ttu-id="3e36a-184">Slutkoden för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e36a-184">The exit code of the task.</span></span>|
|<span data-ttu-id="3e36a-185">retryCount</span><span class="sxs-lookup"><span data-stu-id="3e36a-185">retryCount</span></span>|<span data-ttu-id="3e36a-186">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-186">Int32</span></span>|<span data-ttu-id="3e36a-187">Antal gånger som aktiviteten gjordes av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e36a-187">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="3e36a-188">Uppgiften försöks om avslutas med ett annat värde än noll slutkoden upp till den angivna MaxTaskRetryCount.</span><span class="sxs-lookup"><span data-stu-id="3e36a-188">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount.</span></span>|
|<span data-ttu-id="3e36a-189">requeueCount</span><span class="sxs-lookup"><span data-stu-id="3e36a-189">requeueCount</span></span>|<span data-ttu-id="3e36a-190">Int32</span><span class="sxs-lookup"><span data-stu-id="3e36a-190">Int32</span></span>|<span data-ttu-id="3e36a-191">Antalet gånger uppgiften har åter placerats i kö av Batch-tjänsten som ett resultat av en användarbegäran.</span><span class="sxs-lookup"><span data-stu-id="3e36a-191">The number of times the task has been requeued by the Batch service as the result of a user request.</span></span><br /><br /> <span data-ttu-id="3e36a-192">När användaren tar bort noder från en pool (genom att ändra storlek på eller minska storleken på poolen) eller när jobbet är inaktiverad, användaren kan ange att aktiviteter som körs på noderna vara placerats i kö för körning.</span><span class="sxs-lookup"><span data-stu-id="3e36a-192">When the user removes nodes from a pool (by resizing or shrinking the pool) or when the job is being disabled, the user can specify that running tasks on the nodes be requeued for execution.</span></span> <span data-ttu-id="3e36a-193">Räknaren håller reda på hur många gånger uppgiften har åter placerats i kö av dessa skäl.</span><span class="sxs-lookup"><span data-stu-id="3e36a-193">This count tracks how many times the task has been requeued for these reasons.</span></span>|
