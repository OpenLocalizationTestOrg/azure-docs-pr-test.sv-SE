---
title: "Azure Batch aktiviteten Starta händelsen | Microsoft Docs"
description: "Referens för batchaktiviteten Starta händelsen."
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="task-start-event"></a><span data-ttu-id="bc4f4-103">Aktiviteten Starta händelsen</span><span class="sxs-lookup"><span data-stu-id="bc4f4-103">Task start event</span></span>

 <span data-ttu-id="bc4f4-104">Denna händelse genereras när en aktivitet har schemalagts att starta på en beräkningsnod av Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-104">This event is emitted once a task has been scheduled to start on a compute node by the scheduler.</span></span> <span data-ttu-id="bc4f4-105">Observera att om aktiviteten är ett nytt försök eller begärdes kommer orsakat den här händelsen igen för samma aktivitet, men antal försök och systemversionen aktivitet uppdateras.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-105">Note that if the task is retried or requeued this event will be emitted again for the same task, but the retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="bc4f4-106">I följande exempel visar innehållet i en aktivitet starta en händelse.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-106">The following example shows the body of a task start event.</span></span>

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
        "retryCount": 0
    }
}
```

|<span data-ttu-id="bc4f4-107">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="bc4f4-107">Element name</span></span>|<span data-ttu-id="bc4f4-108">Typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-108">Type</span></span>|<span data-ttu-id="bc4f4-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc4f4-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bc4f4-110">JobId</span><span class="sxs-lookup"><span data-stu-id="bc4f4-110">jobId</span></span>|<span data-ttu-id="bc4f4-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="bc4f4-111">String</span></span>|<span data-ttu-id="bc4f4-112">Id för jobbet som innehåller aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-112">The id of the job containing the task.</span></span>|
|<span data-ttu-id="bc4f4-113">id</span><span class="sxs-lookup"><span data-stu-id="bc4f4-113">id</span></span>|<span data-ttu-id="bc4f4-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="bc4f4-114">String</span></span>|<span data-ttu-id="bc4f4-115">Id för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-115">The id of the task.</span></span>|
|<span data-ttu-id="bc4f4-116">taskType</span><span class="sxs-lookup"><span data-stu-id="bc4f4-116">taskType</span></span>|<span data-ttu-id="bc4f4-117">Sträng</span><span class="sxs-lookup"><span data-stu-id="bc4f4-117">String</span></span>|<span data-ttu-id="bc4f4-118">Typ av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-118">The type of the task.</span></span> <span data-ttu-id="bc4f4-119">Detta kan vara visar det är en projektaktivitet manager ' JobManager' eller 'User' som anger den inte är en projektaktivitet manager.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="bc4f4-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="bc4f4-120">systemTaskVersion</span></span>|<span data-ttu-id="bc4f4-121">Int32</span><span class="sxs-lookup"><span data-stu-id="bc4f4-121">Int32</span></span>|<span data-ttu-id="bc4f4-122">Detta är räknaren interna försök för en uppgift.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-122">This is the internal retry counter on a task.</span></span> <span data-ttu-id="bc4f4-123">Internt kan Batch-tjänsten försöka med att en aktivitet för att tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-123">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="bc4f4-124">De här problemen kan innehålla interna schemaläggning fel eller försöker återställa från compute-noder i ett felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-124">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="bc4f4-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="bc4f4-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="bc4f4-126">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-126">Complex Type</span></span>|<span data-ttu-id="bc4f4-127">Innehåller information om Beräkningsnoden där aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-127">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="bc4f4-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="bc4f4-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="bc4f4-129">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-129">Complex Type</span></span>|<span data-ttu-id="bc4f4-130">Anger att det är flera Instansuppgift som kräver flera compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-130">Specifies that the task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="bc4f4-131">Se [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) mer information.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="bc4f4-132">villkor</span><span class="sxs-lookup"><span data-stu-id="bc4f4-132">constraints</span></span>](#constraints)|<span data-ttu-id="bc4f4-133">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-133">Complex Type</span></span>|<span data-ttu-id="bc4f4-134">Begränsningar för körning som gäller för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-134">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="bc4f4-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="bc4f4-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="bc4f4-136">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-136">Complex Type</span></span>|<span data-ttu-id="bc4f4-137">Innehåller information om körning av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-137">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="bc4f4-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="bc4f4-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="bc4f4-139">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="bc4f4-139">Element name</span></span>|<span data-ttu-id="bc4f4-140">Typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-140">Type</span></span>|<span data-ttu-id="bc4f4-141">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc4f4-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bc4f4-142">poolId</span><span class="sxs-lookup"><span data-stu-id="bc4f4-142">poolId</span></span>|<span data-ttu-id="bc4f4-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="bc4f4-143">String</span></span>|<span data-ttu-id="bc4f4-144">Id för poolen som aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-144">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="bc4f4-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="bc4f4-145">nodeId</span></span>|<span data-ttu-id="bc4f4-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="bc4f4-146">String</span></span>|<span data-ttu-id="bc4f4-147">Id för noden som aktiviteten kördes.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-147">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="bc4f4-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="bc4f4-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="bc4f4-149">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="bc4f4-149">Element name</span></span>|<span data-ttu-id="bc4f4-150">Typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-150">Type</span></span>|<span data-ttu-id="bc4f4-151">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc4f4-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bc4f4-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="bc4f4-152">numberOfInstances</span></span>|<span data-ttu-id="bc4f4-153">int</span><span class="sxs-lookup"><span data-stu-id="bc4f4-153">Int</span></span>|<span data-ttu-id="bc4f4-154">Antalet beräkningsnoder som uppgiften kräver.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-154">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="bc4f4-155"><a name="constraints"></a>villkor</span><span class="sxs-lookup"><span data-stu-id="bc4f4-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="bc4f4-156">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="bc4f4-156">Element name</span></span>|<span data-ttu-id="bc4f4-157">Typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-157">Type</span></span>|<span data-ttu-id="bc4f4-158">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc4f4-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bc4f4-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="bc4f4-159">maxTaskRetryCount</span></span>|<span data-ttu-id="bc4f4-160">Int32</span><span class="sxs-lookup"><span data-stu-id="bc4f4-160">Int32</span></span>|<span data-ttu-id="bc4f4-161">Maximalt antal gånger uppgiften får köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-161">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="bc4f4-162">Batch-tjänsten försöker upprätta en uppgift om dess slutkod inte är noll.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-162">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="bc4f4-163">Observera att det här värdet uttryckligen styr antalet försök.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-163">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="bc4f4-164">Batch-tjänsten försöker aktiviteten en gång och försök sedan upp till den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-164">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="bc4f4-165">Till exempel maximalt antal försök är 3, Batch försöker en aktivitet upp till gånger 4 (ett första försök och 3 försök).</span><span class="sxs-lookup"><span data-stu-id="bc4f4-165">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="bc4f4-166">Maximalt antal försök är 0, försök uppgifter inte igen av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-166">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="bc4f4-167">Om det högsta antalet nya försök är -1, försöker Batch-tjänsten aktiviteter utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-167">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="bc4f4-168">Standardvärdet är 0 (inga nya försök).</span><span class="sxs-lookup"><span data-stu-id="bc4f4-168">The default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="bc4f4-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="bc4f4-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="bc4f4-170">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="bc4f4-170">Element name</span></span>|<span data-ttu-id="bc4f4-171">Typ</span><span class="sxs-lookup"><span data-stu-id="bc4f4-171">Type</span></span>|<span data-ttu-id="bc4f4-172">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc4f4-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bc4f4-173">retryCount</span><span class="sxs-lookup"><span data-stu-id="bc4f4-173">retryCount</span></span>|<span data-ttu-id="bc4f4-174">Int32</span><span class="sxs-lookup"><span data-stu-id="bc4f4-174">Int32</span></span>|<span data-ttu-id="bc4f4-175">Antal gånger som aktiviteten gjordes av Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bc4f4-175">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="bc4f4-176">Uppgiften försöks om avslutas med ett annat värde än noll slutkoden upp till den angivna MaxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="bc4f4-176">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount</span></span>|
