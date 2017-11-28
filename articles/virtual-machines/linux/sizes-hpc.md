---
title: aaaAzure Linux VM-storlekar - HPC | Microsoft Docs
description: "Visar hello olika storlekar för Linux med höga prestanda virtuella datorer i Azure."
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
ms.openlocfilehash: 0b02ee592902ff8097a71898faea1ac17a947d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a><span data-ttu-id="ae418-103">Högpresterande compute Linux VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="ae418-103">High performance compute Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="ae418-104">RDMA-kompatibla instanser</span><span class="sxs-lookup"><span data-stu-id="ae418-104">RDMA-capable instances</span></span>
<span data-ttu-id="ae418-105">En delmängd av hello beräkningsintensiva instanser (H16r, H16mr, A8 och A9) har ett nätverksgränssnitt för remote direct memory access (RDMA)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ae418-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="ae418-106">Det här gränssnittet är dessutom toohello Azure standardnätverk gränssnittet tillgängliga tooother VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="ae418-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="ae418-107">Det här gränssnittet kan hello RDMA-kompatibla instanser toocommunicate InfiniBand-nätverk, arbetar med FDR priser för H16r och H16mr virtuella datorer och QDR priser för A8 och A9 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae418-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="ae418-108">Dessa funktioner för RDMA kan öka hello skalbarhet och prestanda för Message Passing Interface (MPI) program.</span><span class="sxs-lookup"><span data-stu-id="ae418-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="ae418-109">Följande är krav för RDMA-kompatibla virtuella Linux-datorer tooaccess hello Azure RDMA nätverk:</span><span class="sxs-lookup"><span data-stu-id="ae418-109">Following are requirements for RDMA-capable Linux VMs tooaccess hello Azure RDMA network:</span></span>
 
* <span data-ttu-id="ae418-110">**Distributioner** – du måste distribuera virtuella datorer från RDMA-kompatibla SUSE Linux Enterprise Server (SLES) eller falsk Wave-programvara (tidigare OpenLogic) CentOS-baserade HPC-avbildningar i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ae418-110">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="ae418-111">hello följande Marketplace-bilder har stöd för RDMA-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="ae418-111">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="ae418-112">SLES 12 SP1 för HPC eller SLES 12 SP1 för HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="ae418-112">SLES 12 SP1 for HPC, or SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="ae418-113">CentOS-baserade 7.3 HPC, CentOS-baserade 7.1 HPC, CentOS-baserade 6,8 HPC eller CentOS-baserade 6.5 HPC</span><span class="sxs-lookup"><span data-stu-id="ae418-113">CentOS-based 7.3 HPC, CentOS-based 7.1 HPC, CentOS-based 6.8 HPC, or CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="ae418-114">För virtuella datorer H-serien rekommenderar vi en SLES 12 SP1 för HPC-avbildningen eller CentOS-baserade 7.1 eller senare HPC-bild.</span><span class="sxs-lookup"><span data-stu-id="ae418-114">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 or later HPC image.</span></span>
        >
        > <span data-ttu-id="ae418-115">Hej CentOS-baserade HPC-avbildningar i kernel-uppdateringarna har inaktiverats i hello **yum** konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="ae418-115">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="ae418-116">Detta beror på att hello Linux RDMA drivrutiner distribueras som en RPM-paket och drivrutinsuppdateringar kanske inte fungerar om hello kernel uppdateras.</span><span class="sxs-lookup"><span data-stu-id="ae418-116">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="ae418-117">**MPI** -Intel MPI biblioteket 5.x</span><span class="sxs-lookup"><span data-stu-id="ae418-117">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="ae418-118">Beroende på hello Marketplace-avbildning du väljer separat licensiering, installation eller konfiguration av Intel MPI kan behövas, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ae418-118">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="ae418-119">**SLES 12 SP1 för HPC-avbildningen** -Intel MPI paket distribueras på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ae418-119">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="ae418-120">Installera genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ae418-120">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="ae418-121">**CentOS-baserade HPC-avbildningar** -Intel MPI 5.1 redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="ae418-121">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="ae418-122">Ytterligare konfiguration är nödvändiga toorun MPI-jobb på klustrade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae418-122">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="ae418-123">Till exempel i ett kluster för virtuella datorer, måste tooestablish förtroende mellan hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="ae418-123">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="ae418-124">Vanliga inställningar finns [ställa in en Linux RDMA klustret toorun MPI-program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="ae418-124">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

### <a name="network-topology-considerations"></a><span data-ttu-id="ae418-125">Topologiöverväganden för nätverk</span><span class="sxs-lookup"><span data-stu-id="ae418-125">Network topology considerations</span></span>
* <span data-ttu-id="ae418-126">Eth1 är reserverat för RDMA-nätverkstrafik på RDMA-aktiverade Linux virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="ae418-126">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="ae418-127">Ändra inte Eth1 inställningar eller information i hello konfigurationsfilen hänvisar toothis nätverk.</span><span class="sxs-lookup"><span data-stu-id="ae418-127">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="ae418-128">Eth0 är reserverat för vanliga Azure nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="ae418-128">Eth0 is reserved for regular Azure network traffic.</span></span>

* <span data-ttu-id="ae418-129">IP över InfiniBand (IB) stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="ae418-129">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="ae418-130">Endast RDMA över IB stöds.</span><span class="sxs-lookup"><span data-stu-id="ae418-130">Only RDMA over IB is supported.</span></span>

## <a name="using-hpc-pack"></a><span data-ttu-id="ae418-131">Med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="ae418-131">Using HPC Pack</span></span>
<span data-ttu-id="ae418-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, är ett alternativ för du toouse hello beräkningsintensiva instanser med Linux.</span><span class="sxs-lookup"><span data-stu-id="ae418-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="ae418-133">hello senaste versioner av HPC Pack stöd för flera Linux-distributioner toorun på compute-noder som är distribuerad i virtuella Azure-datorer hanteras av en Windows Server-huvudnod.</span><span class="sxs-lookup"><span data-stu-id="ae418-133">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="ae418-134">Med RDMA-kompatibla Linux datornoderna kör Intel MPI, kan HPC Pack schemalägga och köra Linux MPI program åtkomst hello RDMA nätverket.</span><span class="sxs-lookup"><span data-stu-id="ae418-134">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="ae418-135">Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ae418-135">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="other-sizes"></a><span data-ttu-id="ae418-136">Andra storlekar</span><span class="sxs-lookup"><span data-stu-id="ae418-136">Other sizes</span></span>
- [<span data-ttu-id="ae418-137">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="ae418-137">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="ae418-138">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="ae418-138">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="ae418-139">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="ae418-139">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="ae418-140">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="ae418-140">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="ae418-141">GPU</span><span class="sxs-lookup"><span data-stu-id="ae418-141">GPU</span></span>](../windows/sizes-gpu.md)


## <a name="next-steps"></a><span data-ttu-id="ae418-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae418-142">Next steps</span></span>

- <span data-ttu-id="ae418-143">tooget igång distribuerar och använder beräkningsintensiva storlekar med RDMA på Linux, se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ae418-143">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="ae418-144">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="ae418-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




