---
title: "aaa ”Azure Batch-pool borttagning start-händelsen | Microsoft Docs ”"
description: "Referens för Batch-pool delete Starta händelsen."
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
ms.openlocfilehash: 79bb28bffc760a49cc0a95062f5086dc96c6a795
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-start-event"></a><span data-ttu-id="26d56-103">Starta händelsen för poolen borttagning</span><span class="sxs-lookup"><span data-stu-id="26d56-103">Pool delete start event</span></span>

 <span data-ttu-id="26d56-104">Denna händelse genereras när en programpool borttagningsåtgärd har startats.</span><span class="sxs-lookup"><span data-stu-id="26d56-104">This event is emitted when a pool delete operation has started.</span></span> <span data-ttu-id="26d56-105">Eftersom hello poolen delete är en asynkron händelse, du kan förvänta dig poolen ta bort händelsen klar toobe orsakat när hello borttagningsåtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="26d56-105">Since hello pool delete is an asynchronous event, you can expect a pool delete complete event toobe emitted once hello delete operation completes.</span></span>

 <span data-ttu-id="26d56-106">hello visar följande exempel hello brödtexten i en pool borttagning start-händelse.</span><span class="sxs-lookup"><span data-stu-id="26d56-106">hello following example shows hello body of a pool delete start event.</span></span>

```
{
    "id": "myPool1"
}
```

|<span data-ttu-id="26d56-107">Element</span><span class="sxs-lookup"><span data-stu-id="26d56-107">Element</span></span>|<span data-ttu-id="26d56-108">Typ</span><span class="sxs-lookup"><span data-stu-id="26d56-108">Type</span></span>|<span data-ttu-id="26d56-109">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="26d56-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="26d56-110">id</span><span class="sxs-lookup"><span data-stu-id="26d56-110">id</span></span>|<span data-ttu-id="26d56-111">Sträng</span><span class="sxs-lookup"><span data-stu-id="26d56-111">String</span></span>|<span data-ttu-id="26d56-112">hello-id för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="26d56-112">hello id of hello pool.</span></span>|
