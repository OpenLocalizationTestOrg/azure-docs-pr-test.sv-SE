---
title: "aaa ”Azure Batch-pool storleksändring Starta händelsen | Microsoft Docs ”"
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
ms.openlocfilehash: 2ca2a4f1195c3f785ae5b051b63340f70eecbc22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-start-event"></a><span data-ttu-id="1c288-103">Poolen storleksändringar start</span><span class="sxs-lookup"><span data-stu-id="1c288-103">Pool resize start event</span></span>

 <span data-ttu-id="1c288-104">Denna händelse genereras när en storlek för poolen har startats.</span><span class="sxs-lookup"><span data-stu-id="1c288-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="1c288-105">Eftersom hello poolen storlek är en asynkron händelse, du kan förvänta dig poolen storleksändring slutförd händelse toobe orsakat när hello att ändra storlek på har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1c288-105">Since hello pool resize is an asynchronous event, you can expect a pool resize complete event toobe emitted once hello resize operation completes.</span></span>

 <span data-ttu-id="1c288-106">följande exempel visar hello brödtexten i en pool start storleksändringar för en pool storleksändring från 0 too2 noder med en manuell hello storlek.</span><span class="sxs-lookup"><span data-stu-id="1c288-106">hello following example shows hello body of a pool resize start event for a pool resizing from 0 too2 nodes with a manual resize.</span></span>

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

|<span data-ttu-id="1c288-107">Element</span><span class="sxs-lookup"><span data-stu-id="1c288-107">Element</span></span>|<span data-ttu-id="1c288-108">Typ</span><span class="sxs-lookup"><span data-stu-id="1c288-108">Type</span></span>|<span data-ttu-id="1c288-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1c288-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="1c288-110">poolId</span><span class="sxs-lookup"><span data-stu-id="1c288-110">poolId</span></span>|<span data-ttu-id="1c288-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c288-111">String</span></span>|<span data-ttu-id="1c288-112">hello-id för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="1c288-112">hello id of hello pool.</span></span>|
|<span data-ttu-id="1c288-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="1c288-113">nodeDeallocationOption</span></span>|<span data-ttu-id="1c288-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="1c288-114">String</span></span>|<span data-ttu-id="1c288-115">Anger om noder kan tas bort från poolen hello, om hello poolstorleken minskar.</span><span class="sxs-lookup"><span data-stu-id="1c288-115">Specifies when nodes may be removed from hello pool, if hello pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="1c288-116">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="1c288-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="1c288-117">**meddelanden** – avsluta pågående aktiviteter och ställ dem i kö.</span><span class="sxs-lookup"><span data-stu-id="1c288-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="1c288-118">hello aktiviteterna körs igen när jobbet hello är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="1c288-118">hello tasks will run again when hello job is enabled.</span></span> <span data-ttu-id="1c288-119">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="1c288-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="1c288-120">**Avsluta** – avsluta pågående aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="1c288-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="1c288-121">hello aktiviteter körs inte igen.</span><span class="sxs-lookup"><span data-stu-id="1c288-121">hello tasks will not run again.</span></span> <span data-ttu-id="1c288-122">Ta bort noder när aktiviteterna har avslutats.</span><span class="sxs-lookup"><span data-stu-id="1c288-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="1c288-123">**taskcompletion** – Tillåt pågående aktiviteter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1c288-123">**taskcompletion** – Allow currently running tasks toocomplete.</span></span> <span data-ttu-id="1c288-124">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="1c288-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="1c288-125">Ta bort noder när alla aktiviteter har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1c288-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="1c288-126">**Retaineddata** - Låt pågående aktiviteter toocomplete och vänta tills alla aktivitets datakvarhållning punkter tooexpire.</span><span class="sxs-lookup"><span data-stu-id="1c288-126">**Retaineddata** - Allow currently running tasks toocomplete, then wait for all task data retention periods tooexpire.</span></span> <span data-ttu-id="1c288-127">Schemalägg inga nya aktiviteter väntan.</span><span class="sxs-lookup"><span data-stu-id="1c288-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="1c288-128">Ta bort noder när kvarhållningsperioder för alla aktivitet har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="1c288-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="1c288-129">hello standardvärdet är meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1c288-129">hello default value is requeue.</span></span><br /><br /> <span data-ttu-id="1c288-130">Om hello poolstorlek ökar sedan hello värdet för**ogiltigt**.</span><span class="sxs-lookup"><span data-stu-id="1c288-130">If hello pool size is increasing then hello value is set too**invalid**.</span></span>|
|<span data-ttu-id="1c288-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="1c288-131">currentDedicated</span></span>|<span data-ttu-id="1c288-132">Int32</span><span class="sxs-lookup"><span data-stu-id="1c288-132">Int32</span></span>|<span data-ttu-id="1c288-133">hello antalet compute-noder tilldelade för närvarande toohello pool.</span><span class="sxs-lookup"><span data-stu-id="1c288-133">hello number of compute nodes currently assigned toohello pool.</span></span>|
|<span data-ttu-id="1c288-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="1c288-134">targetDedicated</span></span>|<span data-ttu-id="1c288-135">Int32</span><span class="sxs-lookup"><span data-stu-id="1c288-135">Int32</span></span>|<span data-ttu-id="1c288-136">hello antal compute-noder som har begärts för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="1c288-136">hello number of compute nodes that are requested for hello pool.</span></span>|
|<span data-ttu-id="1c288-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="1c288-137">enableAutoScale</span></span>|<span data-ttu-id="1c288-138">bool</span><span class="sxs-lookup"><span data-stu-id="1c288-138">Bool</span></span>|<span data-ttu-id="1c288-139">Anger om justerar hello poolstorlek automatiskt med tiden.</span><span class="sxs-lookup"><span data-stu-id="1c288-139">Specifies whether hello pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="1c288-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="1c288-140">isAutoPool</span></span>|<span data-ttu-id="1c288-141">bool</span><span class="sxs-lookup"><span data-stu-id="1c288-141">Bool</span></span>|<span data-ttu-id="1c288-142">Speficies om hello poolen har skapats via ett jobb AutoPool mekanism.</span><span class="sxs-lookup"><span data-stu-id="1c288-142">Speficies whether hello pool was created via a job's AutoPool mechanism.</span></span>|
