---
title: "Azure Batch-pool ändra storlek på händelsen klar | Microsoft Docs"
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
ms.openlocfilehash: 7072293d98526812cb42ce9c2f8e33bfcafaa149
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-complete-event"></a><span data-ttu-id="9bee4-103">Fullständig storleksändringar pool</span><span class="sxs-lookup"><span data-stu-id="9bee4-103">Pool resize complete event</span></span>

 <span data-ttu-id="9bee4-104">Denna händelse genereras när en storlek för poolen har slutförts eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="9bee4-104">This event is emitted when a pool resize has completed or failed.</span></span>

 <span data-ttu-id="9bee4-105">I följande exempel visar innehållet i en pool fullständig storleksändringar för en pool som ökar i storlek och har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9bee4-105">The following example shows the body of a pool resize complete event for a pool that increased in size and completed successfully.</span></span>

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
    "resizeError": "The operation succeeded"
}
```

|<span data-ttu-id="9bee4-106">Element</span><span class="sxs-lookup"><span data-stu-id="9bee4-106">Element</span></span>|<span data-ttu-id="9bee4-107">Typ</span><span class="sxs-lookup"><span data-stu-id="9bee4-107">Type</span></span>|<span data-ttu-id="9bee4-108">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9bee4-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="9bee4-109">id</span><span class="sxs-lookup"><span data-stu-id="9bee4-109">id</span></span>|<span data-ttu-id="9bee4-110">Sträng</span><span class="sxs-lookup"><span data-stu-id="9bee4-110">String</span></span>|<span data-ttu-id="9bee4-111">Id för poolen.</span><span class="sxs-lookup"><span data-stu-id="9bee4-111">The id of the pool.</span></span>|
|<span data-ttu-id="9bee4-112">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="9bee4-112">nodeDeallocationOption</span></span>|<span data-ttu-id="9bee4-113">Sträng</span><span class="sxs-lookup"><span data-stu-id="9bee4-113">String</span></span>|<span data-ttu-id="9bee4-114">Anger om noder kan tas bort från poolen, om poolstorleken minskar.</span><span class="sxs-lookup"><span data-stu-id="9bee4-114">Specifies when nodes may be removed from the pool, if the pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="9bee4-115">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="9bee4-115">Possible values are:</span></span><br /><br /> <span data-ttu-id="9bee4-116">**meddelanden** – avsluta pågående aktiviteter och ställ dem i kö.</span><span class="sxs-lookup"><span data-stu-id="9bee4-116">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="9bee4-117">Aktiviteterna körs igen när jobbet har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="9bee4-117">The tasks will run again when the job is enabled.</span></span> <span data-ttu-id="9bee4-118">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="9bee4-118">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="9bee4-119">**Avsluta** – avsluta pågående aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="9bee4-119">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="9bee4-120">Uppgifterna kan inte köras igen.</span><span class="sxs-lookup"><span data-stu-id="9bee4-120">The tasks will not run again.</span></span> <span data-ttu-id="9bee4-121">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="9bee4-121">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="9bee4-122">**taskcompletion** – Tillåt pågående aktiviteter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="9bee4-122">**taskcompletion** – Allow currently running tasks to complete.</span></span> <span data-ttu-id="9bee4-123">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="9bee4-123">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="9bee4-124">Ta bort noder när alla aktiviteter har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9bee4-124">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="9bee4-125">**Retaineddata** -Låt pågående aktiviteter slutföras och vänta i alla aktivitetskvarhållningsperioder data upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="9bee4-125">**Retaineddata** -  Allow currently running tasks to complete, then wait for all task data retention periods to expire.</span></span> <span data-ttu-id="9bee4-126">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="9bee4-126">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="9bee4-127">Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="9bee4-127">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="9bee4-128">Standardvärdet är meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9bee4-128">The default value is requeue.</span></span><br /><br /> <span data-ttu-id="9bee4-129">Om poolstorleken ökar sedan värdet till **ogiltigt**.</span><span class="sxs-lookup"><span data-stu-id="9bee4-129">If the pool size is increasing then the value is set to **invalid**.</span></span>|
|<span data-ttu-id="9bee4-130">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="9bee4-130">currentDedicated</span></span>|<span data-ttu-id="9bee4-131">Int32</span><span class="sxs-lookup"><span data-stu-id="9bee4-131">Int32</span></span>|<span data-ttu-id="9bee4-132">Antal compute-noder för närvarande tilldelat till poolen.</span><span class="sxs-lookup"><span data-stu-id="9bee4-132">The number of compute nodes currently assigned to the pool.</span></span>|
|<span data-ttu-id="9bee4-133">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="9bee4-133">targetDedicated</span></span>|<span data-ttu-id="9bee4-134">Int32</span><span class="sxs-lookup"><span data-stu-id="9bee4-134">Int32</span></span>|<span data-ttu-id="9bee4-135">Antal compute-noder som har begärts för poolen.</span><span class="sxs-lookup"><span data-stu-id="9bee4-135">The number of compute nodes that are requested for the pool.</span></span>|
|<span data-ttu-id="9bee4-136">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="9bee4-136">enableAutoScale</span></span>|<span data-ttu-id="9bee4-137">bool</span><span class="sxs-lookup"><span data-stu-id="9bee4-137">Bool</span></span>|<span data-ttu-id="9bee4-138">Anger om justerar poolstorleken automatiskt med tiden.</span><span class="sxs-lookup"><span data-stu-id="9bee4-138">Specifies whether the pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="9bee4-139">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="9bee4-139">isAutoPool</span></span>|<span data-ttu-id="9bee4-140">bool</span><span class="sxs-lookup"><span data-stu-id="9bee4-140">Bool</span></span>|<span data-ttu-id="9bee4-141">Anger om poolen har skapats via ett jobb AutoPool mekanism.</span><span class="sxs-lookup"><span data-stu-id="9bee4-141">Specifies whether the pool was created via a job's AutoPool mechanism.</span></span>|
|<span data-ttu-id="9bee4-142">startTime</span><span class="sxs-lookup"><span data-stu-id="9bee4-142">startTime</span></span>|<span data-ttu-id="9bee4-143">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="9bee4-143">DateTime</span></span>|<span data-ttu-id="9bee4-144">Ändra storlek på poolen tiden har startats.</span><span class="sxs-lookup"><span data-stu-id="9bee4-144">The time the pool resize started.</span></span>|
|<span data-ttu-id="9bee4-145">Sluttid</span><span class="sxs-lookup"><span data-stu-id="9bee4-145">endTime</span></span>|<span data-ttu-id="9bee4-146">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="9bee4-146">DateTime</span></span>|<span data-ttu-id="9bee4-147">Den tid ändra storlek på poolen slutförts.</span><span class="sxs-lookup"><span data-stu-id="9bee4-147">The time the pool resize completed.</span></span>|
|<span data-ttu-id="9bee4-148">ResultCode</span><span class="sxs-lookup"><span data-stu-id="9bee4-148">resultCode</span></span>|<span data-ttu-id="9bee4-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="9bee4-149">String</span></span>|<span data-ttu-id="9bee4-150">Resultatet av storlek.</span><span class="sxs-lookup"><span data-stu-id="9bee4-150">The result of the resize.</span></span>|
|<span data-ttu-id="9bee4-151">resultMessage</span><span class="sxs-lookup"><span data-stu-id="9bee4-151">resultMessage</span></span>|<span data-ttu-id="9bee4-152">Sträng</span><span class="sxs-lookup"><span data-stu-id="9bee4-152">String</span></span>|<span data-ttu-id="9bee4-153">Ändra storlek på fel finns information om resultatet.</span><span class="sxs-lookup"><span data-stu-id="9bee4-153">The resize error includes the details of the result.</span></span><br /><br /> <span data-ttu-id="9bee4-154">Om storleksändringen har slutförts det anger att åtgärden lyckades.</span><span class="sxs-lookup"><span data-stu-id="9bee4-154">If the resize completed successfully it states that the operation succeeded.</span></span>|