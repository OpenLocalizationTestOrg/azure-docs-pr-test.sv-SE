---
title: "aaa ”Azure Batch-pool ta bort händelsen klar | Microsoft Docs ”"
description: "Referens för Batch-pool att ta bort händelsen klar."
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
ms.openlocfilehash: 494c371e48ebfb1bf3d2973a7401829a939ba141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-complete-event"></a><span data-ttu-id="61d68-103">Händelsen i poolen ta bort klar</span><span class="sxs-lookup"><span data-stu-id="61d68-103">Pool delete complete event</span></span>

 <span data-ttu-id="61d68-104">Denna händelse genereras när en programpool borttagningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="61d68-104">This event is emitted when a pool delete operation has completed.</span></span>

 <span data-ttu-id="61d68-105">hello visar följande exempel hello brödtexten i en pool delete complete-händelse.</span><span class="sxs-lookup"><span data-stu-id="61d68-105">hello following example shows hello body of a pool delete complete event.</span></span>

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|<span data-ttu-id="61d68-106">Element</span><span class="sxs-lookup"><span data-stu-id="61d68-106">Element</span></span>|<span data-ttu-id="61d68-107">Typ</span><span class="sxs-lookup"><span data-stu-id="61d68-107">Type</span></span>|<span data-ttu-id="61d68-108">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="61d68-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="61d68-109">id</span><span class="sxs-lookup"><span data-stu-id="61d68-109">id</span></span>|<span data-ttu-id="61d68-110">Sträng</span><span class="sxs-lookup"><span data-stu-id="61d68-110">String</span></span>|<span data-ttu-id="61d68-111">hello-id för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="61d68-111">hello id of hello pool.</span></span>|
|<span data-ttu-id="61d68-112">startTime</span><span class="sxs-lookup"><span data-stu-id="61d68-112">startTime</span></span>|<span data-ttu-id="61d68-113">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="61d68-113">DateTime</span></span>|<span data-ttu-id="61d68-114">hello när hello poolen tar bort startas.</span><span class="sxs-lookup"><span data-stu-id="61d68-114">hello time hello pool delete started.</span></span>|
|<span data-ttu-id="61d68-115">endTime</span><span class="sxs-lookup"><span data-stu-id="61d68-115">endTime</span></span>|<span data-ttu-id="61d68-116">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="61d68-116">DateTime</span></span>|<span data-ttu-id="61d68-117">hello slutförts när hello poolen tar bort.</span><span class="sxs-lookup"><span data-stu-id="61d68-117">hello time hello pool delete completed.</span></span>|

## <a name="remarks"></a><span data-ttu-id="61d68-118">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="61d68-118">Remarks</span></span>
<span data-ttu-id="61d68-119">Mer information om tillstånd och felkoder för åtgärden Ändra storlek för poolen finns [ta bort poolen från ett konto](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span><span class="sxs-lookup"><span data-stu-id="61d68-119">For more information about states and error codes for pool resize operation, see [Delete a pool from an account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span></span>