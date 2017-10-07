---
title: aaaAzure Batch Analytics | Microsoft Docs
description: "Referens för Azure Batch Analytics."
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
ms.openlocfilehash: 462fbad1ac522b485ae18c1e8891b9d2cabd45e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="batch-analytics"></a><span data-ttu-id="a62de-103">Batch Analytics</span><span class="sxs-lookup"><span data-stu-id="a62de-103">Batch Analytics</span></span>
<span data-ttu-id="a62de-104">hello innehåller Batch Analytics referensinformation för hello händelser och varningar som är tillgängliga för Batch-tjänsten resurser.</span><span class="sxs-lookup"><span data-stu-id="a62de-104">hello topics in Batch Analytics contain reference information for hello events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="a62de-105">Se [Azure Batch diagnostikloggning](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) mer information om att aktivera och använda Batch diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="a62de-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="a62de-106">Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="a62de-106">Diagnostic logs</span></span>

<span data-ttu-id="a62de-107">hello Azure Batch-tjänsten skickar hello följande diagnostiska logghändelser under hello livstid vissa Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="a62de-107">hello Azure Batch service emits hello following diagnostic log events during hello lifetime of certain Batch resources.</span></span>

<span data-ttu-id="a62de-108">**Tjänsten logghändelser**</span><span class="sxs-lookup"><span data-stu-id="a62de-108">**Service Log events**</span></span>
* [<span data-ttu-id="a62de-109">Skapa poolen</span><span class="sxs-lookup"><span data-stu-id="a62de-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="a62de-110">Poolen delete start</span><span class="sxs-lookup"><span data-stu-id="a62de-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="a62de-111">Poolen ta bort klar</span><span class="sxs-lookup"><span data-stu-id="a62de-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="a62de-112">Poolen storleksändring start</span><span class="sxs-lookup"><span data-stu-id="a62de-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="a62de-113">Poolen storleksändring slutförd</span><span class="sxs-lookup"><span data-stu-id="a62de-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="a62de-114">Uppgiften start</span><span class="sxs-lookup"><span data-stu-id="a62de-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="a62de-115">Uppgift slutförd</span><span class="sxs-lookup"><span data-stu-id="a62de-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="a62de-116">Aktiviteten misslyckas</span><span class="sxs-lookup"><span data-stu-id="a62de-116">Task fail</span></span>](batch-task-fail-event.md)