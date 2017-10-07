---
title: "aaa ”Azure Batch aktiviteten Starta händelsen | Microsoft Docs ”"
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
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a><span data-ttu-id="1a2c5-103">Aktiviteten Starta händelsen</span><span class="sxs-lookup"><span data-stu-id="1a2c5-103">Task start event</span></span>

 <span data-ttu-id="1a2c5-104">Denna händelse genereras när en uppgift har schemalagda toostart på en beräkningsnod av hello Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-104">This event is emitted once a task has been scheduled toostart on a compute node by hello scheduler.</span></span> <span data-ttu-id="1a2c5-105">Observera att om hello uppgift är ett nytt försök eller placerats i kö händelsen kommer att orsakat igen för hello samma aktivitet, men hello antal återförsök och systemversionen aktivitet uppdateras.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-105">Note that if hello task is retried or requeued this event will be emitted again for hello same task, but hello retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="1a2c5-106">hello visar följande exempel hello brödtexten i en händelse för aktiviteten starta.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-106">hello following example shows hello body of a task start event.</span></span>

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

|<span data-ttu-id="1a2c5-107">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="1a2c5-107">Element name</span></span>|<span data-ttu-id="1a2c5-108">Typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-108">Type</span></span>|<span data-ttu-id="1a2c5-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a2c5-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1a2c5-110">JobId</span><span class="sxs-lookup"><span data-stu-id="1a2c5-110">jobId</span></span>|<span data-ttu-id="1a2c5-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a2c5-111">String</span></span>|<span data-ttu-id="1a2c5-112">hello-id för hello jobb som innehåller hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-112">hello id of hello job containing hello task.</span></span>|
|<span data-ttu-id="1a2c5-113">id</span><span class="sxs-lookup"><span data-stu-id="1a2c5-113">id</span></span>|<span data-ttu-id="1a2c5-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a2c5-114">String</span></span>|<span data-ttu-id="1a2c5-115">hello-id för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-115">hello id of hello task.</span></span>|
|<span data-ttu-id="1a2c5-116">taskType</span><span class="sxs-lookup"><span data-stu-id="1a2c5-116">taskType</span></span>|<span data-ttu-id="1a2c5-117">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a2c5-117">String</span></span>|<span data-ttu-id="1a2c5-118">hello typ av uppgift som hello.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-118">hello type of hello task.</span></span> <span data-ttu-id="1a2c5-119">Detta kan vara visar det är en projektaktivitet manager ' JobManager' eller 'User' som anger den inte är en projektaktivitet manager.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="1a2c5-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="1a2c5-120">systemTaskVersion</span></span>|<span data-ttu-id="1a2c5-121">Int32</span><span class="sxs-lookup"><span data-stu-id="1a2c5-121">Int32</span></span>|<span data-ttu-id="1a2c5-122">Detta är hello interna försök räknare för en uppgift.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-122">This is hello internal retry counter on a task.</span></span> <span data-ttu-id="1a2c5-123">Internt kan hello Batch-tjänsten försöka med att en uppgift tooaccount för tillfälliga problem.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-123">Internally hello Batch service can retry a task tooaccount for transient issues.</span></span> <span data-ttu-id="1a2c5-124">De här problemen kan innehålla interna schemaläggning fel eller försök toorecover från compute-noder i ett felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-124">These issues can include internal scheduling errors or attempts toorecover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="1a2c5-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="1a2c5-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="1a2c5-126">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-126">Complex Type</span></span>|<span data-ttu-id="1a2c5-127">Innehåller information om hello compute-nod på vilka hello uppgiften kördes.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-127">Contains information about hello compute node on which hello task ran.</span></span>|
|[<span data-ttu-id="1a2c5-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="1a2c5-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="1a2c5-129">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-129">Complex Type</span></span>|<span data-ttu-id="1a2c5-130">Anger att hello är flera Instansuppgift som kräver flera compute-noder.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-130">Specifies that hello task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="1a2c5-131">Se [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) mer information.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="1a2c5-132">villkor</span><span class="sxs-lookup"><span data-stu-id="1a2c5-132">constraints</span></span>](#constraints)|<span data-ttu-id="1a2c5-133">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-133">Complex Type</span></span>|<span data-ttu-id="1a2c5-134">hello körning begränsningar som gäller toothis aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-134">hello execution constraints that apply toothis task.</span></span>|
|[<span data-ttu-id="1a2c5-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="1a2c5-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="1a2c5-136">Komplex typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-136">Complex Type</span></span>|<span data-ttu-id="1a2c5-137">Innehåller information om hello körningen av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-137">Contains information about hello execution of hello task.</span></span>|

###  <span data-ttu-id="1a2c5-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="1a2c5-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="1a2c5-139">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="1a2c5-139">Element name</span></span>|<span data-ttu-id="1a2c5-140">Typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-140">Type</span></span>|<span data-ttu-id="1a2c5-141">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a2c5-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1a2c5-142">poolId</span><span class="sxs-lookup"><span data-stu-id="1a2c5-142">poolId</span></span>|<span data-ttu-id="1a2c5-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a2c5-143">String</span></span>|<span data-ttu-id="1a2c5-144">hello-id för hello poolen på vilka hello uppgiften kördes.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-144">hello id of hello pool on which hello task ran.</span></span>|
|<span data-ttu-id="1a2c5-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="1a2c5-145">nodeId</span></span>|<span data-ttu-id="1a2c5-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="1a2c5-146">String</span></span>|<span data-ttu-id="1a2c5-147">hello-id för hello nod på vilka hello uppgiften kördes.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-147">hello id of hello node on which hello task ran.</span></span>|

###  <span data-ttu-id="1a2c5-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="1a2c5-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="1a2c5-149">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="1a2c5-149">Element name</span></span>|<span data-ttu-id="1a2c5-150">Typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-150">Type</span></span>|<span data-ttu-id="1a2c5-151">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a2c5-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1a2c5-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="1a2c5-152">numberOfInstances</span></span>|<span data-ttu-id="1a2c5-153">int</span><span class="sxs-lookup"><span data-stu-id="1a2c5-153">Int</span></span>|<span data-ttu-id="1a2c5-154">hello antal datornoderna krävs hello uppgiften.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-154">hello number of compute nodes required by hello task.</span></span>|

###  <span data-ttu-id="1a2c5-155"><a name="constraints"></a>villkor</span><span class="sxs-lookup"><span data-stu-id="1a2c5-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="1a2c5-156">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="1a2c5-156">Element name</span></span>|<span data-ttu-id="1a2c5-157">Typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-157">Type</span></span>|<span data-ttu-id="1a2c5-158">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a2c5-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1a2c5-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="1a2c5-159">maxTaskRetryCount</span></span>|<span data-ttu-id="1a2c5-160">Int32</span><span class="sxs-lookup"><span data-stu-id="1a2c5-160">Int32</span></span>|<span data-ttu-id="1a2c5-161">hello maximalt antal gånger hello uppgift får köras igen.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-161">hello maximum number of times hello task may be retried.</span></span> <span data-ttu-id="1a2c5-162">hello Batch-tjänsten försöker upprätta en uppgift om dess slutkod inte är noll.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-162">hello Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="1a2c5-163">Observera att det här värdet uttryckligen styr hello antalet försök.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-163">Note that this value specifically controls hello number of retries.</span></span> <span data-ttu-id="1a2c5-164">hello Batch-tjänsten försöker hello aktiviteten en gång och försök sedan in toothis gränsen.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-164">hello Batch service will try hello task once, and may then retry up toothis limit.</span></span> <span data-ttu-id="1a2c5-165">Om hello högsta antal försök är 3, försöker Batch en uppgift upp too4 gånger (en inledande försök och 3 försök).</span><span class="sxs-lookup"><span data-stu-id="1a2c5-165">For example, if hello maximum retry count is 3, Batch tries a task up too4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="1a2c5-166">Om hello högsta antal försök är 0 hello Batch-tjänsten inte att försök uppgifter.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-166">If hello maximum retry count is 0, hello Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="1a2c5-167">Om hello högsta antal försök är -1 försöker hello Batch-tjänsten aktiviteter utan begränsning.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-167">If hello maximum retry count is -1, hello Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="1a2c5-168">hello standardvärdet är 0 (inga nya försök).</span><span class="sxs-lookup"><span data-stu-id="1a2c5-168">hello default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="1a2c5-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="1a2c5-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="1a2c5-170">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="1a2c5-170">Element name</span></span>|<span data-ttu-id="1a2c5-171">Typ</span><span class="sxs-lookup"><span data-stu-id="1a2c5-171">Type</span></span>|<span data-ttu-id="1a2c5-172">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a2c5-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1a2c5-173">retryCount</span><span class="sxs-lookup"><span data-stu-id="1a2c5-173">retryCount</span></span>|<span data-ttu-id="1a2c5-174">Int32</span><span class="sxs-lookup"><span data-stu-id="1a2c5-174">Int32</span></span>|<span data-ttu-id="1a2c5-175">hello antal gånger hello aktiviteten gjordes av hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1a2c5-175">hello number of times hello task has been retried by hello Batch service.</span></span> <span data-ttu-id="1a2c5-176">hello uppgiften försöks om avslutas med slutkoden noll anges MaxTaskRetryCount in toohello</span><span class="sxs-lookup"><span data-stu-id="1a2c5-176">hello task is retried if it exits with a nonzero exit code, up toohello specified MaxTaskRetryCount</span></span>|
