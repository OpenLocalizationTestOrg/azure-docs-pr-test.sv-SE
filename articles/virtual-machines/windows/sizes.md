---
title: aaaWindows VM-storlekar i Azure | Microsoft Docs
description: "Visar hello olika storlekar för virtuella Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a><span data-ttu-id="7a59e-103">Storlekar för virtuella Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="7a59e-103">Sizes for Windows virtual machines in Azure</span></span>

<span data-ttu-id="7a59e-104">Den här artikeln beskriver hello tillgängliga storlekar och alternativ för hello Azure virtuella datorer som du kan använda toorun dina Windows-appar och arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="7a59e-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Windows apps and workloads.</span></span> <span data-ttu-id="7a59e-105">Det ger också distribution överväganden toobe medveten om när du planerar toouse dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="7a59e-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span>  <span data-ttu-id="7a59e-106">Den här artikeln är också tillgängligt för [virtuella Linux-datorer](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a59e-106">This article is also available for [Linux virtual machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


| <span data-ttu-id="7a59e-107">Typ</span><span class="sxs-lookup"><span data-stu-id="7a59e-107">Type</span></span>                     | <span data-ttu-id="7a59e-108">Storlekar</span><span class="sxs-lookup"><span data-stu-id="7a59e-108">Sizes</span></span>           |    <span data-ttu-id="7a59e-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a59e-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="7a59e-110">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="7a59e-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="7a59e-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="7a59e-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span> | <span data-ttu-id="7a59e-112">Balanserat förhållande mellan processor och minne.</span><span class="sxs-lookup"><span data-stu-id="7a59e-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="7a59e-113">Idealisk för testning och utveckling, små toomedium databaser och låg toomedium trafik webbservrar.</span><span class="sxs-lookup"><span data-stu-id="7a59e-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="7a59e-114">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="7a59e-115">FS, F</span><span class="sxs-lookup"><span data-stu-id="7a59e-115">Fs, F</span></span>             | <span data-ttu-id="7a59e-116">Högt förhållande mellan processor och minne.</span><span class="sxs-lookup"><span data-stu-id="7a59e-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="7a59e-117">Bra för webbservrar med medelhög trafik, nätverkstillämpningar, batchprocesser och programservrar.</span><span class="sxs-lookup"><span data-stu-id="7a59e-117">Good for medium traffic web servers, network appliances, batch processes, and application servers.</span></span>        |
| [<span data-ttu-id="7a59e-118">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-118">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)         | <span data-ttu-id="7a59e-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="7a59e-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="7a59e-120">Förhållandet mellan hög minne till CPU.</span><span class="sxs-lookup"><span data-stu-id="7a59e-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="7a59e-121">Perfekt för relationsdatabas servrar, medelhög toolarge cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="7a59e-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="7a59e-122">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-122">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)        | <span data-ttu-id="7a59e-123">Ls</span><span class="sxs-lookup"><span data-stu-id="7a59e-123">Ls</span></span>                | <span data-ttu-id="7a59e-124">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="7a59e-124">High disk throughput and IO.</span></span> <span data-ttu-id="7a59e-125">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="7a59e-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="7a59e-126">GPU</span><span class="sxs-lookup"><span data-stu-id="7a59e-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="7a59e-127">NV NC</span><span class="sxs-lookup"><span data-stu-id="7a59e-127">NV, NC</span></span>            | <span data-ttu-id="7a59e-128">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="7a59e-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="7a59e-129">Tillgängligt med en eller flera GPU-kort.</span><span class="sxs-lookup"><span data-stu-id="7a59e-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="7a59e-130">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="7a59e-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="7a59e-131">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="7a59e-131">H, A8-11</span></span>          | <span data-ttu-id="7a59e-132">Våra virtuella datorer med de snabbaste och mest kraftfulla processorerna med nätverksgränssnitt för stora dataflöden (RDMA).</span><span class="sxs-lookup"><span data-stu-id="7a59e-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br> 

- <span data-ttu-id="7a59e-133">Mer information om priser för hello olika storlekar finns [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span><span class="sxs-lookup"><span data-stu-id="7a59e-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span></span> 
- <span data-ttu-id="7a59e-134">toosee allmänna begränsningar på Azure Virtual Machines finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="7a59e-134">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="7a59e-135">Lagringskostnader beräknas separat använda sidor i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7a59e-135">Storage costs are calculated separately based on used pages in hello storage account.</span></span> <span data-ttu-id="7a59e-136">Mer information [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="7a59e-136">For details, [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>
- <span data-ttu-id="7a59e-137">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="7a59e-137">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>



## <a name="rest-api"></a><span data-ttu-id="7a59e-138">REST-API</span><span class="sxs-lookup"><span data-stu-id="7a59e-138">Rest API</span></span>

<span data-ttu-id="7a59e-139">Information om hur du använder hello REST API tooquery storlekar på VM finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="7a59e-139">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="7a59e-140">Visa en lista med tillgängliga virtuella datorstorlekar för storleksändring</span><span class="sxs-lookup"><span data-stu-id="7a59e-140">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="7a59e-141">Visa en lista med tillgängliga virtuella datorstorlekar för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a59e-141">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="7a59e-142">Visa en lista med tillgängliga virtuella datorstorlekar i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="7a59e-142">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="7a59e-143">ACU</span><span class="sxs-lookup"><span data-stu-id="7a59e-143">ACU</span></span>

<span data-ttu-id="7a59e-144">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="7a59e-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a59e-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a59e-145">Next steps</span></span>

<span data-ttu-id="7a59e-146">Läs mer om hello olika storlekar på VM som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="7a59e-146">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="7a59e-147">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="7a59e-147">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="7a59e-148">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-148">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="7a59e-149">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-149">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="7a59e-150">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-150">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="7a59e-151">GPU-optimerad</span><span class="sxs-lookup"><span data-stu-id="7a59e-151">GPU optimized</span></span>](sizes-gpu.md)
- [<span data-ttu-id="7a59e-152">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="7a59e-152">High performance compute</span></span>](sizes-hpc.md)



