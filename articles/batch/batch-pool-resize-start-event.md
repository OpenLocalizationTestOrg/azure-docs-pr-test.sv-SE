---
title: "Azure Batch-pool storleksändring Starta händelsen | Microsoft Docs"
description: "Referens för Batch-pool storleksändringar start."
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-start-event"></a><span data-ttu-id="ec269-103">Poolen storleksändringar start</span><span class="sxs-lookup"><span data-stu-id="ec269-103">Pool resize start event</span></span>

 <span data-ttu-id="ec269-104">Denna händelse genereras när en storlek för poolen har startats.</span><span class="sxs-lookup"><span data-stu-id="ec269-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="ec269-105">Eftersom storleksändringen av poolen är en asynkron händelse, du kan förvänta dig en pool fullständig storleksändringar som ska skickas när åtgärden Ändra storlek har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ec269-105">Since the pool resize is an asynchronous event, you can expect a pool resize complete event to be emitted once the resize operation completes.</span></span>

 <span data-ttu-id="ec269-106">I följande exempel visar innehållet i en pool start storleksändringar för en pool storleksändring från 0 till 2 noder med en manuell.</span><span class="sxs-lookup"><span data-stu-id="ec269-106">The following example shows the body of a pool resize start event for a pool resizing from 0 to 2 nodes with a manual resize.</span></span>

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|<span data-ttu-id="ec269-107">Element</span><span class="sxs-lookup"><span data-stu-id="ec269-107">Element</span></span>|<span data-ttu-id="ec269-108">Typ</span><span class="sxs-lookup"><span data-stu-id="ec269-108">Type</span></span>|<span data-ttu-id="ec269-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ec269-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="ec269-110">poolId</span><span class="sxs-lookup"><span data-stu-id="ec269-110">poolId</span></span>|<span data-ttu-id="ec269-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec269-111">String</span></span>|<span data-ttu-id="ec269-112">Id för poolen.</span><span class="sxs-lookup"><span data-stu-id="ec269-112">The id of the pool.</span></span>|
|<span data-ttu-id="ec269-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="ec269-113">nodeDeallocationOption</span></span>|<span data-ttu-id="ec269-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec269-114">String</span></span>|<span data-ttu-id="ec269-115">Anger om noder kan tas bort från poolen, om poolstorleken minskar.</span><span class="sxs-lookup"><span data-stu-id="ec269-115">Specifies when nodes may be removed from the pool, if the pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="ec269-116">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="ec269-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="ec269-117">**meddelanden** – avsluta pågående aktiviteter och ställ dem i kö.</span><span class="sxs-lookup"><span data-stu-id="ec269-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="ec269-118">Aktiviteterna körs igen när jobbet har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="ec269-118">The tasks will run again when the job is enabled.</span></span> <span data-ttu-id="ec269-119">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="ec269-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="ec269-120">**Avsluta** – avsluta pågående aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="ec269-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="ec269-121">Uppgifterna kan inte köras igen.</span><span class="sxs-lookup"><span data-stu-id="ec269-121">The tasks will not run again.</span></span> <span data-ttu-id="ec269-122">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="ec269-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="ec269-123">**taskcompletion** – Tillåt pågående aktiviteter för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="ec269-123">**taskcompletion** – Allow currently running tasks to complete.</span></span> <span data-ttu-id="ec269-124">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="ec269-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="ec269-125">Ta bort noder när alla aktiviteter har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ec269-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="ec269-126">**Retaineddata** -Låt pågående aktiviteter slutföras och vänta i alla aktivitetskvarhållningsperioder data upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="ec269-126">**Retaineddata** - Allow currently running tasks to complete, then wait for all task data retention periods to expire.</span></span> <span data-ttu-id="ec269-127">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="ec269-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="ec269-128">Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="ec269-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="ec269-129">Standardvärdet är meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ec269-129">The default value is requeue.</span></span><br /><br /> <span data-ttu-id="ec269-130">Om poolstorleken ökar sedan värdet till **ogiltigt**.</span><span class="sxs-lookup"><span data-stu-id="ec269-130">If the pool size is increasing then the value is set to **invalid**.</span></span>|
|<span data-ttu-id="ec269-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="ec269-131">currentDedicated</span></span>|<span data-ttu-id="ec269-132">Int32</span><span class="sxs-lookup"><span data-stu-id="ec269-132">Int32</span></span>|<span data-ttu-id="ec269-133">Antal compute-noder för närvarande tilldelat till poolen.</span><span class="sxs-lookup"><span data-stu-id="ec269-133">The number of compute nodes currently assigned to the pool.</span></span>|
|<span data-ttu-id="ec269-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="ec269-134">targetDedicated</span></span>|<span data-ttu-id="ec269-135">Int32</span><span class="sxs-lookup"><span data-stu-id="ec269-135">Int32</span></span>|<span data-ttu-id="ec269-136">Antal compute-noder som har begärts för poolen.</span><span class="sxs-lookup"><span data-stu-id="ec269-136">The number of compute nodes that are requested for the pool.</span></span>|
|<span data-ttu-id="ec269-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="ec269-137">enableAutoScale</span></span>|<span data-ttu-id="ec269-138">bool</span><span class="sxs-lookup"><span data-stu-id="ec269-138">Bool</span></span>|<span data-ttu-id="ec269-139">Anger om justerar poolstorleken automatiskt med tiden.</span><span class="sxs-lookup"><span data-stu-id="ec269-139">Specifies whether the pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="ec269-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="ec269-140">isAutoPool</span></span>|<span data-ttu-id="ec269-141">bool</span><span class="sxs-lookup"><span data-stu-id="ec269-141">Bool</span></span>|<span data-ttu-id="ec269-142">Speficies om poolen har skapats via ett jobb AutoPool mekanism.</span><span class="sxs-lookup"><span data-stu-id="ec269-142">Speficies whether the pool was created via a job's AutoPool mechanism.</span></span>|
