---
title: Azure Batch Analytics | Microsoft Docs
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
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="batch-analytics"></a><span data-ttu-id="60983-103">Batch Analytics</span><span class="sxs-lookup"><span data-stu-id="60983-103">Batch Analytics</span></span>
<span data-ttu-id="60983-104">Avsnitt i Batchanalyser innehåller referensinformation för händelser och varningar som är tillgängliga för Batch-tjänsten resurser.</span><span class="sxs-lookup"><span data-stu-id="60983-104">The topics in Batch Analytics contain reference information for the events and alerts available for Batch service resources.</span></span>

<span data-ttu-id="60983-105">Se [Azure Batch diagnostikloggning](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) mer information om att aktivera och använda Batch diagnostikloggar.</span><span class="sxs-lookup"><span data-stu-id="60983-105">See [Azure Batch diagnostic logging](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) for more information on enabling and consuming Batch diagnostic logs.</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="60983-106">Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="60983-106">Diagnostic logs</span></span>

<span data-ttu-id="60983-107">Azure Batch-tjänsten skickar följande diagnostiska logghändelser under livslängden för vissa Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="60983-107">The Azure Batch service emits the following diagnostic log events during the lifetime of certain Batch resources.</span></span>

<span data-ttu-id="60983-108">**Tjänsten logghändelser**</span><span class="sxs-lookup"><span data-stu-id="60983-108">**Service Log events**</span></span>
* [<span data-ttu-id="60983-109">Skapa poolen</span><span class="sxs-lookup"><span data-stu-id="60983-109">Pool create</span></span>](batch-pool-create-event.md)
* [<span data-ttu-id="60983-110">Poolen delete start</span><span class="sxs-lookup"><span data-stu-id="60983-110">Pool delete start</span></span>](batch-pool-delete-start-event.md)
* [<span data-ttu-id="60983-111">Poolen ta bort klar</span><span class="sxs-lookup"><span data-stu-id="60983-111">Pool delete complete</span></span>](batch-pool-delete-complete-event.md)
* [<span data-ttu-id="60983-112">Poolen storleksändring start</span><span class="sxs-lookup"><span data-stu-id="60983-112">Pool resize start</span></span>](batch-pool-resize-start-event.md)
* [<span data-ttu-id="60983-113">Poolen storleksändring slutförd</span><span class="sxs-lookup"><span data-stu-id="60983-113">Pool resize complete</span></span>](batch-pool-resize-complete-event.md)
* [<span data-ttu-id="60983-114">Uppgiften start</span><span class="sxs-lookup"><span data-stu-id="60983-114">Task start</span></span>](batch-task-start-event.md)
* [<span data-ttu-id="60983-115">Uppgift slutförd</span><span class="sxs-lookup"><span data-stu-id="60983-115">Task complete</span></span>](batch-task-complete-event.md)
* [<span data-ttu-id="60983-116">Aktiviteten misslyckas</span><span class="sxs-lookup"><span data-stu-id="60983-116">Task fail</span></span>](batch-task-fail-event.md)