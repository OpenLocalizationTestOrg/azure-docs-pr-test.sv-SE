---
title: "aaa ”Azure Batch-pool ändra storlek på händelsen klar | Microsoft Docs ”"
description: "Referens för Batch-pool ändra storlek på händelsen klar."
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
ms.openlocfilehash: dc64711a01aa4cf6192edba1a2c4cad56f953766
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-complete-event"></a><span data-ttu-id="6278a-103">Fullständig storleksändringar pool</span><span class="sxs-lookup"><span data-stu-id="6278a-103">Pool resize complete event</span></span>

 <span data-ttu-id="6278a-104">Denna händelse genereras när en storlek för poolen har slutförts eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="6278a-104">This event is emitted when a pool resize has completed or failed.</span></span>

 <span data-ttu-id="6278a-105">hello visar följande exempel hello brödtexten i en pool fullständig storleksändringar för en pool som ökar i storlek och har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6278a-105">hello following example shows hello body of a pool resize complete event for a pool that increased in size and completed successfully.</span></span>

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "hello operation succeeded"
}
```

|<span data-ttu-id="6278a-106">Element</span><span class="sxs-lookup"><span data-stu-id="6278a-106">Element</span></span>|<span data-ttu-id="6278a-107">Typ</span><span class="sxs-lookup"><span data-stu-id="6278a-107">Type</span></span>|<span data-ttu-id="6278a-108">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6278a-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="6278a-109">id</span><span class="sxs-lookup"><span data-stu-id="6278a-109">id</span></span>|<span data-ttu-id="6278a-110">Sträng</span><span class="sxs-lookup"><span data-stu-id="6278a-110">String</span></span>|<span data-ttu-id="6278a-111">hello-id för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="6278a-111">hello id of hello pool.</span></span>|
|<span data-ttu-id="6278a-112">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="6278a-112">nodeDeallocationOption</span></span>|<span data-ttu-id="6278a-113">Sträng</span><span class="sxs-lookup"><span data-stu-id="6278a-113">String</span></span>|<span data-ttu-id="6278a-114">Anger om noder kan tas bort från poolen hello, om hello poolstorleken minskar.</span><span class="sxs-lookup"><span data-stu-id="6278a-114">Specifies when nodes may be removed from hello pool, if hello pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="6278a-115">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="6278a-115">Possible values are:</span></span><br /><br /> <span data-ttu-id="6278a-116">**meddelanden** – avsluta pågående aktiviteter och ställ dem i kö.</span><span class="sxs-lookup"><span data-stu-id="6278a-116">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="6278a-117">hello aktiviteterna körs igen när jobbet hello är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="6278a-117">hello tasks will run again when hello job is enabled.</span></span> <span data-ttu-id="6278a-118">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="6278a-118">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="6278a-119">**Avsluta** – avsluta pågående aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6278a-119">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="6278a-120">hello aktiviteter körs inte igen.</span><span class="sxs-lookup"><span data-stu-id="6278a-120">hello tasks will not run again.</span></span> <span data-ttu-id="6278a-121">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="6278a-121">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="6278a-122">**taskcompletion** – Tillåt pågående aktiviteter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6278a-122">**taskcompletion** – Allow currently running tasks toocomplete.</span></span> <span data-ttu-id="6278a-123">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="6278a-123">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="6278a-124">Ta bort noder när alla aktiviteter har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6278a-124">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="6278a-125">**Retaineddata** - Låt pågående aktiviteter toocomplete och vänta tills alla aktivitets datakvarhållning punkter tooexpire.</span><span class="sxs-lookup"><span data-stu-id="6278a-125">**Retaineddata** -  Allow currently running tasks toocomplete, then wait for all task data retention periods tooexpire.</span></span> <span data-ttu-id="6278a-126">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="6278a-126">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="6278a-127">Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="6278a-127">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="6278a-128">hello standardvärdet är meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6278a-128">hello default value is requeue.</span></span><br /><br /> <span data-ttu-id="6278a-129">Om hello poolstorlek ökar sedan hello värdet för**ogiltigt**.</span><span class="sxs-lookup"><span data-stu-id="6278a-129">If hello pool size is increasing then hello value is set too**invalid**.</span></span>|
|<span data-ttu-id="6278a-130">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="6278a-130">currentDedicated</span></span>|<span data-ttu-id="6278a-131">Int32</span><span class="sxs-lookup"><span data-stu-id="6278a-131">Int32</span></span>|<span data-ttu-id="6278a-132">hello antalet compute-noder tilldelade för närvarande toohello pool.</span><span class="sxs-lookup"><span data-stu-id="6278a-132">hello number of compute nodes currently assigned toohello pool.</span></span>|
|<span data-ttu-id="6278a-133">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="6278a-133">targetDedicated</span></span>|<span data-ttu-id="6278a-134">Int32</span><span class="sxs-lookup"><span data-stu-id="6278a-134">Int32</span></span>|<span data-ttu-id="6278a-135">hello antal compute-noder som har begärts för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="6278a-135">hello number of compute nodes that are requested for hello pool.</span></span>|
|<span data-ttu-id="6278a-136">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="6278a-136">enableAutoScale</span></span>|<span data-ttu-id="6278a-137">bool</span><span class="sxs-lookup"><span data-stu-id="6278a-137">Bool</span></span>|<span data-ttu-id="6278a-138">Anger om justerar hello poolstorlek automatiskt med tiden.</span><span class="sxs-lookup"><span data-stu-id="6278a-138">Specifies whether hello pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="6278a-139">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="6278a-139">isAutoPool</span></span>|<span data-ttu-id="6278a-140">bool</span><span class="sxs-lookup"><span data-stu-id="6278a-140">Bool</span></span>|<span data-ttu-id="6278a-141">Anger om hello poolen har skapats via ett jobb AutoPool mekanism.</span><span class="sxs-lookup"><span data-stu-id="6278a-141">Specifies whether hello pool was created via a job's AutoPool mechanism.</span></span>|
|<span data-ttu-id="6278a-142">startTime</span><span class="sxs-lookup"><span data-stu-id="6278a-142">startTime</span></span>|<span data-ttu-id="6278a-143">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="6278a-143">DateTime</span></span>|<span data-ttu-id="6278a-144">hello tid hello poolen ändra storlek på Starta.</span><span class="sxs-lookup"><span data-stu-id="6278a-144">hello time hello pool resize started.</span></span>|
|<span data-ttu-id="6278a-145">endTime</span><span class="sxs-lookup"><span data-stu-id="6278a-145">endTime</span></span>|<span data-ttu-id="6278a-146">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="6278a-146">DateTime</span></span>|<span data-ttu-id="6278a-147">hello slutförts tid hello poolen ändra storlek på.</span><span class="sxs-lookup"><span data-stu-id="6278a-147">hello time hello pool resize completed.</span></span>|
|<span data-ttu-id="6278a-148">ResultCode</span><span class="sxs-lookup"><span data-stu-id="6278a-148">resultCode</span></span>|<span data-ttu-id="6278a-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="6278a-149">String</span></span>|<span data-ttu-id="6278a-150">hello resultatet av hello storlek.</span><span class="sxs-lookup"><span data-stu-id="6278a-150">hello result of hello resize.</span></span>|
|<span data-ttu-id="6278a-151">resultMessage</span><span class="sxs-lookup"><span data-stu-id="6278a-151">resultMessage</span></span>|<span data-ttu-id="6278a-152">Sträng</span><span class="sxs-lookup"><span data-stu-id="6278a-152">String</span></span>|<span data-ttu-id="6278a-153">hello storleksändring fel innehåller hello information om hello resultat.</span><span class="sxs-lookup"><span data-stu-id="6278a-153">hello resize error includes hello details of hello result.</span></span><br /><br /> <span data-ttu-id="6278a-154">Om hello ändrar storlek på slutförts det tillstånd som hello åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="6278a-154">If hello resize completed successfully it states that hello operation succeeded.</span></span>|
