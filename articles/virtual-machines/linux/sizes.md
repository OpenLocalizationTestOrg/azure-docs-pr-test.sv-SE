---
title: aaaLinux VM-storlekar i Azure | Microsoft Docs
description: "Visar hello olika storlekar för virtuella Linux-datorer i Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 56cbe0a0d7d34def07636edba74c4c699e336012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a><span data-ttu-id="de03b-103">Storlekar för virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="de03b-103">Sizes for Linux virtual machines in Azure</span></span>
<span data-ttu-id="de03b-104">Den här artikeln beskriver hello tillgängliga storlekar och alternativ för hello Azure virtuella datorer som du kan använda toorun dina Linux appar och arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="de03b-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Linux apps and workloads.</span></span> <span data-ttu-id="de03b-105">Det ger också distribution överväganden toobe medveten om när du planerar toouse dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="de03b-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span> <span data-ttu-id="de03b-106">Den här artikeln är också tillgängligt för [virtuella Windows-datorer](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="de03b-106">This article is also available for [Windows virtual machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


| <span data-ttu-id="de03b-107">Typ</span><span class="sxs-lookup"><span data-stu-id="de03b-107">Type</span></span>                     | <span data-ttu-id="de03b-108">Storlekar</span><span class="sxs-lookup"><span data-stu-id="de03b-108">Sizes</span></span>           |    <span data-ttu-id="de03b-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="de03b-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="de03b-110">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="de03b-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="de03b-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="de03b-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span></span>  | <span data-ttu-id="de03b-112">Balanserat förhållande mellan processor och minne.</span><span class="sxs-lookup"><span data-stu-id="de03b-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="de03b-113">Idealisk för testning och utveckling, små toomedium databaser och låg toomedium trafik webbservrar.</span><span class="sxs-lookup"><span data-stu-id="de03b-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="de03b-114">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="de03b-115">FS, F</span><span class="sxs-lookup"><span data-stu-id="de03b-115">Fs, F</span></span>             | <span data-ttu-id="de03b-116">Högt förhållande mellan processor och minne.</span><span class="sxs-lookup"><span data-stu-id="de03b-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="de03b-117">Bra för medelhög trafik webbservrar, nätverksinstallationer, bad processer och programservrar.</span><span class="sxs-lookup"><span data-stu-id="de03b-117">Good for medium traffic web servers, network appliances, bath processes, and application servers.</span></span>        |
| [<span data-ttu-id="de03b-118">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-118">Memory optimized</span></span>](sizes-memory.md)         | <span data-ttu-id="de03b-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="de03b-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="de03b-120">Förhållandet mellan hög minne till CPU.</span><span class="sxs-lookup"><span data-stu-id="de03b-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="de03b-121">Perfekt för relationsdatabas servrar, medelhög toolarge cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="de03b-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="de03b-122">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-122">Storage optimized</span></span>](sizes-storage.md)        | <span data-ttu-id="de03b-123">Ls</span><span class="sxs-lookup"><span data-stu-id="de03b-123">Ls</span></span>                | <span data-ttu-id="de03b-124">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="de03b-124">High disk throughput and IO.</span></span> <span data-ttu-id="de03b-125">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="de03b-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="de03b-126">GPU</span><span class="sxs-lookup"><span data-stu-id="de03b-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="de03b-127">NV NC</span><span class="sxs-lookup"><span data-stu-id="de03b-127">NV, NC</span></span>            | <span data-ttu-id="de03b-128">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="de03b-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="de03b-129">Tillgängligt med en eller flera GPU-kort.</span><span class="sxs-lookup"><span data-stu-id="de03b-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="de03b-130">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="de03b-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="de03b-131">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="de03b-131">H, A8-11</span></span>          | <span data-ttu-id="de03b-132">Våra virtuella datorer med de snabbaste och mest kraftfulla processorerna med nätverksgränssnitt för stora dataflöden (RDMA).</span><span class="sxs-lookup"><span data-stu-id="de03b-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br>

- <span data-ttu-id="de03b-133">Mer information om priser för hello olika storlekar finns [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="de03b-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span> 
- <span data-ttu-id="de03b-134">Tillgängligheten för VM-storlekar i Azure-regioner finns [produkter som är tillgängliga efter region](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="de03b-134">For availability of VM sizes in Azure regions, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
- <span data-ttu-id="de03b-135">toosee allmänna begränsningar på Azure Virtual Machines finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="de03b-135">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="de03b-136">Läs mer om hur [Azure compute-enheter (ACU)](../windows/acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="de03b-136">Learn more about how [Azure compute units (ACU)](../windows/acu.md) can help you compare compute performance across Azure SKUs.</span></span>


## <a name="rest-api"></a><span data-ttu-id="de03b-137">REST-API</span><span class="sxs-lookup"><span data-stu-id="de03b-137">Rest API</span></span>

<span data-ttu-id="de03b-138">Information om hur du använder hello REST API tooquery storlekar på VM finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="de03b-138">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="de03b-139">Visa en lista med tillgängliga virtuella datorstorlekar för storleksändring</span><span class="sxs-lookup"><span data-stu-id="de03b-139">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="de03b-140">Visa en lista med tillgängliga virtuella datorstorlekar för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="de03b-140">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="de03b-141">Visa en lista med tillgängliga virtuella datorstorlekar i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="de03b-141">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="de03b-142">ACU</span><span class="sxs-lookup"><span data-stu-id="de03b-142">ACU</span></span>

<span data-ttu-id="de03b-143">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="de03b-143">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de03b-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de03b-144">Next steps</span></span>

<span data-ttu-id="de03b-145">Läs mer om hello olika storlekar på VM som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="de03b-145">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="de03b-146">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="de03b-146">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="de03b-147">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-147">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="de03b-148">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-148">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="de03b-149">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="de03b-149">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="de03b-150">GPU</span><span class="sxs-lookup"><span data-stu-id="de03b-150">GPU</span></span>](sizes-gpu.md)
- [<span data-ttu-id="de03b-151">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="de03b-151">High performance compute</span></span>](sizes-hpc.md)



