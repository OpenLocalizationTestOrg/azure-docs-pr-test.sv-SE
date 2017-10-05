---
title: Azure Linux VM-storlekar - GPU | Microsoft Docs
description: "Visar olika GPU optimerad storlekar som finns tillgängliga för Linux virtuella datorer i Azure."
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
ms.openlocfilehash: 5c9bf89feba519147b07f2810fe4da882664e89e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="gpu-linux-vm-sizes"></a><span data-ttu-id="a23d4-103">GPU Linux VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="a23d4-103">GPU Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

<span data-ttu-id="a23d4-104">Drivrutinen installation och verifiering anvisningar finns [N-serien drivrutinsinstallation för Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="a23d4-104">For driver installation and verification steps, see [N-series driver setup for Linux](n-series-driver-setup.md).</span></span>

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* <span data-ttu-id="a23d4-105">Vi rekommenderar inte att installera X server eller andra system som använder drivrutinen nouveau på Ubuntu NC virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a23d4-105">We don't recommend installing X server or other systems that use the nouveau driver on Ubuntu NC VMs.</span></span> <span data-ttu-id="a23d4-106">Du måste inaktivera nouveau drivrutinen innan du installerar drivrutiner för NVIDIA GPU.</span><span class="sxs-lookup"><span data-stu-id="a23d4-106">Before installing NVIDIA GPU drivers, you need to disable the nouveau driver.</span></span>  

## <a name="other-sizes"></a><span data-ttu-id="a23d4-107">Andra storlekar</span><span class="sxs-lookup"><span data-stu-id="a23d4-107">Other sizes</span></span>
- [<span data-ttu-id="a23d4-108">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="a23d4-108">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="a23d4-109">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a23d4-109">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="a23d4-110">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="a23d4-110">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="a23d4-111">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="a23d4-111">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="a23d4-112">Databehandling med höga prestanda</span><span class="sxs-lookup"><span data-stu-id="a23d4-112">High performance compute</span></span>](sizes-hpc.md)

## <a name="next-steps"></a><span data-ttu-id="a23d4-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a23d4-113">Next steps</span></span>
<span data-ttu-id="a23d4-114">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="a23d4-114">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>