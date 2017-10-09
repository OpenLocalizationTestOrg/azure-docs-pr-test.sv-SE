---
title: "aaaAbout beräkningsintensiva virtuella datorer med Linux | Microsoft Docs"
description: "Hämta bakgrundsinformation och överväganden för att använda hello H-serien och A8 A9, A10 och A11 beräkningsintensiva storlekar för virtuella Linux-datorer"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 16cf6e2d-f401-4b22-af8c-9a337179f5f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 37636840a3f809ac19354a5a7993257216f675f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a><span data-ttu-id="2b327-103">Om H-serien och beräkningsintensiva A-series virtuella datorer för Linux</span><span class="sxs-lookup"><span data-stu-id="2b327-103">About H-series and compute-intensive A-series VMs for Linux</span></span>
<span data-ttu-id="2b327-104">Här är bakgrundsinformation och vissa aspekter för att använda hello nyare Azure H-serien och hello tidigare A8, A9, A10 och A11 storlekar, även kallat *beräkningsintensiva* instanser.</span><span class="sxs-lookup"><span data-stu-id="2b327-104">Here is background information and some considerations for using hello newer Azure H-series and hello earlier A8, A9, A10, and A11 sizes, also known as *compute-intensive* instances.</span></span> <span data-ttu-id="2b327-105">Den här artikeln fokuserar på att använda dessa storlekar för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="2b327-105">This article focuses on using these sizes for Linux VMs.</span></span> <span data-ttu-id="2b327-106">Du kan också använda dem för [virtuella Windows-datorer](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-106">You can also use them for [Windows VMs](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="2b327-107">Grundläggande specifikationerna, lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-107">For basic specs, storage capacities, and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-toohello-rdma-network"></a><span data-ttu-id="2b327-108">Åtkomst toohello RDMA nätverk</span><span class="sxs-lookup"><span data-stu-id="2b327-108">Access toohello RDMA network</span></span>
<span data-ttu-id="2b327-109">Du kan skapa kluster av RDMA-kompatibla virtuella Linux-datorer som kör något av hello efter Linux HPC-distributioner som stöds och en stöds MPI implementering tootake utnyttja hello Azure RDMA-nätverk.</span><span class="sxs-lookup"><span data-stu-id="2b327-109">You can create clusters of RDMA-capable Linux VMs that run one of hello following supported Linux HPC distributions and a supported MPI implementation tootake advantage of hello Azure RDMA network.</span></span> <span data-ttu-id="2b327-110">Se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) för distributionsalternativ och exempel konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="2b327-110">See [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) for deployment options and sample configuration steps.</span></span>

* <span data-ttu-id="2b327-111">**Distributioner** – du måste distribuera virtuella datorer från RDMA-kompatibla SUSE Linux Enterprise Server (SLES) eller falsk Wave-programvara (tidigare OpenLogic) CentOS-baserade HPC-avbildningar i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="2b327-111">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="2b327-112">hello följande Marketplace-bilder har stöd för RDMA-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="2b327-112">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="2b327-113">SLES 12 SP1 för HPC SLES 12 SP1 för HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="2b327-113">SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="2b327-114">CentOS-baserade 7.1 HPC, CentOS-baserade 6.5 HPC</span><span class="sxs-lookup"><span data-stu-id="2b327-114">CentOS-based 7.1 HPC, CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="2b327-115">För virtuella datorer H-serien rekommenderar vi antingen en SLES 12 SP1 för HPC-avbildningen eller CentOS-baserad 7.1 HPC-avbildning.</span><span class="sxs-lookup"><span data-stu-id="2b327-115">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 HPC image.</span></span>
        >
        > <span data-ttu-id="2b327-116">Hej CentOS-baserade HPC-avbildningar i kernel-uppdateringarna har inaktiverats i hello **yum** konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="2b327-116">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="2b327-117">Detta beror på att hello Linux RDMA drivrutiner distribueras som en RPM-paket och drivrutinsuppdateringar kanske inte fungerar om hello kernel uppdateras.</span><span class="sxs-lookup"><span data-stu-id="2b327-117">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="2b327-118">**MPI** -Intel MPI biblioteket 5.x</span><span class="sxs-lookup"><span data-stu-id="2b327-118">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="2b327-119">Beroende på hello Marketplace-avbildning du väljer separat licensiering, installation eller konfiguration av Intel MPI kan behövas, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2b327-119">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="2b327-120">**SLES 12 SP1 för HPC-avbildningen** -Intel MPI paket distribueras på hello VM.</span><span class="sxs-lookup"><span data-stu-id="2b327-120">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="2b327-121">Installera genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2b327-121">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="2b327-122">**CentOS-baserade HPC-avbildningar** -Intel MPI 5.1 redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="2b327-122">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="2b327-123">Ytterligare konfiguration är nödvändiga toorun MPI-jobb på klustrade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2b327-123">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="2b327-124">Till exempel i ett kluster för virtuella datorer, måste tooestablish förtroende mellan hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="2b327-124">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="2b327-125">Vanliga inställningar Se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-125">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="considerations-for-hpc-pack-and-linux"></a><span data-ttu-id="2b327-126">Överväganden för HPC Pack och Linux</span><span class="sxs-lookup"><span data-stu-id="2b327-126">Considerations for HPC Pack and Linux</span></span>
<span data-ttu-id="2b327-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, ger ett alternativ för du toouse hello beräkningsintensiva instanser med Linux.</span><span class="sxs-lookup"><span data-stu-id="2b327-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, provides one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="2b327-128">hello senaste versioner av HPC Pack stöd för flera Linux-distributioner toorun på compute-noder som är distribuerad i virtuella Azure-datorer hanteras av en Windows Server-huvudnod.</span><span class="sxs-lookup"><span data-stu-id="2b327-128">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="2b327-129">Med RDMA-kompatibla Linux datornoderna kör Intel MPI, kan HPC Pack schemalägga och köra Linux MPI program åtkomst hello RDMA nätverket.</span><span class="sxs-lookup"><span data-stu-id="2b327-129">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="2b327-130">tooget igång, se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-130">tooget started, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="network-topology-considerations"></a><span data-ttu-id="2b327-131">Topologiöverväganden för nätverk</span><span class="sxs-lookup"><span data-stu-id="2b327-131">Network topology considerations</span></span>
* <span data-ttu-id="2b327-132">Eth1 är reserverat för RDMA-nätverkstrafik på RDMA-aktiverade Linux virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="2b327-132">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="2b327-133">Ändra inte Eth1 inställningar eller information i hello konfigurationsfilen hänvisar toothis nätverk.</span><span class="sxs-lookup"><span data-stu-id="2b327-133">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="2b327-134">Eth0 är reserverat för vanliga Azure nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="2b327-134">Eth0 is reserved for regular Azure network traffic.</span></span>
* <span data-ttu-id="2b327-135">IP över InfiniBand (IB) stöds inte i Azure.</span><span class="sxs-lookup"><span data-stu-id="2b327-135">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="2b327-136">Endast RDMA över IB stöds.</span><span class="sxs-lookup"><span data-stu-id="2b327-136">Only RDMA over IB is supported.</span></span>



## <a name="next-steps"></a><span data-ttu-id="2b327-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b327-137">Next steps</span></span>
* <span data-ttu-id="2b327-138">Mer information om tillgänglighet och pris i hello beräkningsintensiva storlekar finns [virtuella datorer priser](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="2b327-138">For details about availability and pricing of hello compute-intensive sizes, see [Virtual Machines pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span>
* <span data-ttu-id="2b327-139">Lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-139">For storage capacities and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="2b327-140">tooget igång distribuerar och använder beräkningsintensiva storlekar med RDMA på Linux, se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2b327-140">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

