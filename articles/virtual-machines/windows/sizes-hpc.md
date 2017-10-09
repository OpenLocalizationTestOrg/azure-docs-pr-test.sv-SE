---
title: aaaAzure Windows VM-storlekar - HPC | Microsoft Docs
description: "Visar hello olika storlekar som är tillgängliga för Windows med höga prestanda virtuella datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 092bc32cfe048f439ad833911bef4ed17eda7977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-vm-sizes"></a><span data-ttu-id="3a31c-103">Högpresterande beräkning VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="3a31c-103">High performance compute VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="3a31c-104">RDMA-kompatibla instanser</span><span class="sxs-lookup"><span data-stu-id="3a31c-104">RDMA-capable instances</span></span>
<span data-ttu-id="3a31c-105">En delmängd av hello beräkningsintensiva instanser (H16r, H16mr, A8 och A9) har ett nätverksgränssnitt för remote direct memory access (RDMA)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3a31c-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="3a31c-106">Det här gränssnittet är dessutom toohello Azure standardnätverk gränssnittet tillgängliga tooother VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="3a31c-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="3a31c-107">Det här gränssnittet kan hello RDMA-kompatibla instanser toocommunicate InfiniBand-nätverk, arbetar med FDR priser för H16r och H16mr virtuella datorer och QDR priser för A8 och A9 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3a31c-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="3a31c-108">Dessa funktioner för RDMA kan öka hello skalbarhet och prestanda för Message Passing Interface (MPI) program.</span><span class="sxs-lookup"><span data-stu-id="3a31c-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="3a31c-109">Följande är krav för RDMA-kompatibla virtuella Windows-datorer tooaccess hello Azure RDMA nätverk:</span><span class="sxs-lookup"><span data-stu-id="3a31c-109">Following are requirements for RDMA-capable Windows VMs tooaccess hello Azure RDMA network:</span></span> 

* <span data-ttu-id="3a31c-110">**Operativsystem**</span><span class="sxs-lookup"><span data-stu-id="3a31c-110">**Operating system**</span></span>
  
  <span data-ttu-id="3a31c-111">Windows Server 2012 R2, Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="3a31c-111">Windows Server 2012 R2, Windows Server 2012</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="3a31c-112">Windows Server 2016 stöder för närvarande inte RDMA-anslutningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a31c-112">Currently, Windows Server 2016 does not support RDMA connectivity in Azure.</span></span>
  >

* <span data-ttu-id="3a31c-113">**Tillgänglighetsuppsättning eller Molntjänsten** – distribuera hello RDMA-kompatibla virtuella datorer i hello samma tillgänglighetsuppsättning (när du använder hello Azure Resource Manager-distributionsmodellen) eller hello samma tjänst i molnet (när du använder hello klassiska distributionsmodellen).</span><span class="sxs-lookup"><span data-stu-id="3a31c-113">**Availability set or cloud service** – Deploy hello RDMA-capable VMs in hello same availability set (when you use hello Azure Resource Manager deployment model) or hello same cloud service (when you use hello classic deployment model).</span></span> <span data-ttu-id="3a31c-114">Om du använder Azure Batch hello RDMA-kompatibla virtuella datorer måste vara i hello samma pool.</span><span class="sxs-lookup"><span data-stu-id="3a31c-114">If you use Azure Batch, hello RDMA-capable VMs must be in hello same pool.</span></span>

* <span data-ttu-id="3a31c-115">**MPI** -Microsoft MPI (MS-MPI) 2012 R2 eller senare, Intel MPI biblioteket 5.x</span><span class="sxs-lookup"><span data-stu-id="3a31c-115">**MPI** - Microsoft MPI (MS-MPI) 2012 R2 or later, Intel MPI Library 5.x</span></span>

  <span data-ttu-id="3a31c-116">Det används hello Microsoft Network Direct gränssnittet toocommunicate mellan instanser stöds MPI-implementeringar.</span><span class="sxs-lookup"><span data-stu-id="3a31c-116">Supported MPI implementations use hello Microsoft Network Direct interface toocommunicate between instances.</span></span> 

* <span data-ttu-id="3a31c-117">**RDMA nätverks-adressutrymme** -hello RDMA nätverk i Azure reserverar hello adressutrymme 172.16.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="3a31c-117">**RDMA network address space** - hello RDMA network in Azure reserves hello address space 172.16.0.0/16.</span></span> <span data-ttu-id="3a31c-118">toorun MPI program på instanser distribuerade i Azure-nätverk, kontrollera att hello virtuella nätverkets adressutrymme inte överlappar hello RDMA nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a31c-118">toorun MPI applications on instances deployed in an Azure virtual network, make sure that hello virtual network address space does not overlap hello RDMA network.</span></span>

* <span data-ttu-id="3a31c-119">**HpcVmDrivers VM-tillägget** -på RDMA-kompatibla virtuella datorer, måste du lägga till hello HpcVmDrivers tillägget tooinstall Windows network-drivrutiner för RDMA-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3a31c-119">**HpcVmDrivers VM extension** - On RDMA-capable VMs, you must add hello HpcVmDrivers extension tooinstall Windows network device drivers for RDMA connectivity.</span></span> <span data-ttu-id="3a31c-120">(I vissa distributioner av A8- och A9 instanser hello HpcVmDrivers tillägg läggs automatiskt.) tooadd hello VM-tillägget tooa VM som du kan använda [Azure PowerShell](/powershell/azure/overview) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="3a31c-120">(In certain deployments of A8 and A9 instances, hello HpcVmDrivers extension is added automatically.) tooadd hello VM extension tooa VM, you can use [Azure PowerShell](/powershell/azure/overview) cmdlets.</span></span> 

  
  <span data-ttu-id="3a31c-121">Hej följande kommando installerar hello senaste version 1.1 HpcVMDrivers tillägg på en befintlig RDMA-kompatibla virtuell dator med namnet *myVM* distribuerats i hello resursgrupp med namnet *myResourceGroup* i hello  *USA, västra* region:</span><span class="sxs-lookup"><span data-stu-id="3a31c-121">hello following command installs hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named *myVM* deployed in hello resource group named *myResourceGroup* in hello *West US* region:</span></span>

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  <span data-ttu-id="3a31c-122">Mer information finns i [tillägg för virtuell dator och funktioner](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a31c-122">For more information, see [Virtual machine extensions and features](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3a31c-123">Du kan också fungera med tillägg för virtuella datorer som distribueras i hello [klassiska distributionsmodellen](classic/manage-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="3a31c-123">You can also work with extensions for VMs deployed in hello [classic deployment model](classic/manage-extensions.md).</span></span>


## <a name="using-hpc-pack"></a><span data-ttu-id="3a31c-124">Med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="3a31c-124">Using HPC Pack</span></span>

<span data-ttu-id="3a31c-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, är ett alternativ för toocreate ett beräkningskluster i Azure toorun MPI för Windows-baserade program och andra HPC-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="3a31c-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toocreate a compute cluster in Azure toorun Windows-based MPI applications and other HPC workloads.</span></span> <span data-ttu-id="3a31c-126">HPC Pack 2012 R2 och senare versioner innehåller en körningsmiljö för MS-MPI som använder hello Azure RDMA nätverk när de distribueras på RDMA-kompatibla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3a31c-126">HPC Pack 2012 R2 and later versions include a runtime environment for MS-MPI that uses hello Azure RDMA network when deployed on RDMA-capable VMs.</span></span>




## <a name="other-sizes"></a><span data-ttu-id="3a31c-127">Andra storlekar</span><span class="sxs-lookup"><span data-stu-id="3a31c-127">Other sizes</span></span>
- [<span data-ttu-id="3a31c-128">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="3a31c-128">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="3a31c-129">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="3a31c-129">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="3a31c-130">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="3a31c-130">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="3a31c-131">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="3a31c-131">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="3a31c-132">GPU-optimerad</span><span class="sxs-lookup"><span data-stu-id="3a31c-132">GPU optimized</span></span>](sizes-gpu.md)

## <a name="next-steps"></a><span data-ttu-id="3a31c-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a31c-133">Next steps</span></span>

- <span data-ttu-id="3a31c-134">Checklistor toouse hello beräkningsintensiva instanser med HPC Pack på Windows Server, se [ställer in ett RDMA-Windows-kluster med HPC Pack toorun MPI program](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a31c-134">For checklists toouse hello compute-intensive instances with HPC Pack on Windows Server, see [Set up a Windows RDMA cluster with HPC Pack toorun MPI applications](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="3a31c-135">När du kör MPI-program med Azure Batch-toouse beräkningsintensiva instanser finns [Använd flera instanser av åtgärder toorun Message Passing Interface (MPI) program i Azure Batch](../../batch/batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="3a31c-135">toouse compute-intensive instances when running MPI applications with Azure Batch, see [Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).</span></span>

- <span data-ttu-id="3a31c-136">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="3a31c-136">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




