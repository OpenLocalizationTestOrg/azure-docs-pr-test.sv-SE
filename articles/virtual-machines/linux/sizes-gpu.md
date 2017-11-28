---
title: aaaAzure Linux VM-storlekar - GPU | Microsoft Docs
description: "Visar hello olika GPU optimerade storlekar för virtuella Linux-datorer i Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: e98f720499be37df4048aeb513aa4f6b187b7335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="gpu-linux-vm-sizes"></a><span data-ttu-id="d4ebb-103">GPU Linux VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="d4ebb-103">GPU Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

<span data-ttu-id="d4ebb-104">Drivrutinen installation och verifiering anvisningar finns [N-serien drivrutinsinstallation för Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="d4ebb-104">For driver installation and verification steps, see [N-series driver setup for Linux](n-series-driver-setup.md).</span></span>

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* <span data-ttu-id="d4ebb-105">Vi rekommenderar inte att installera X server eller andra system som använder hello nouveau drivrutinen på Ubuntu NC virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d4ebb-105">We don't recommend installing X server or other systems that use hello nouveau driver on Ubuntu NC VMs.</span></span> <span data-ttu-id="d4ebb-106">Innan du installerar NVIDIA GPU drivrutiner, måste toodisable hello nouveau drivrutin.</span><span class="sxs-lookup"><span data-stu-id="d4ebb-106">Before installing NVIDIA GPU drivers, you need toodisable hello nouveau driver.</span></span>  

## <a name="other-sizes"></a><span data-ttu-id="d4ebb-107">Andra storlekar</span><span class="sxs-lookup"><span data-stu-id="d4ebb-107">Other sizes</span></span>
- [<span data-ttu-id="d4ebb-108">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="d4ebb-108">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="d4ebb-109">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="d4ebb-109">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="d4ebb-110">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="d4ebb-110">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="d4ebb-111">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="d4ebb-111">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="d4ebb-112">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="d4ebb-112">High performance compute</span></span>](sizes-hpc.md)

## <a name="next-steps"></a><span data-ttu-id="d4ebb-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4ebb-113">Next steps</span></span>
<span data-ttu-id="d4ebb-114">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="d4ebb-114">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>