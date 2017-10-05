---
title: Azure Windows VM-storlekar - HPC | Microsoft Docs
description: "Listar de olika storlekarna som är tillgängliga för Windows med höga prestanda virtuella datorer i Azure."
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
ms.openlocfilehash: a0596d134e9c26877848f93d72f35bfd2c957570
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="high-performance-compute-vm-sizes"></a><span data-ttu-id="c052c-103">Högpresterande beräkning VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="c052c-103">High performance compute VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="c052c-104">RDMA-kompatibla instanser</span><span class="sxs-lookup"><span data-stu-id="c052c-104">RDMA-capable instances</span></span>
<span data-ttu-id="c052c-105">En delmängd av beräkningsintensiva instanser (H16r, H16mr, A8 och A9) har ett nätverksgränssnitt för remote direct memory access (RDMA)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c052c-105">A subset of the compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="c052c-106">Det här gränssnittet är utöver standard Azure nätverksgränssnittet tillgänglig för andra storlekar på VM.</span><span class="sxs-lookup"><span data-stu-id="c052c-106">This interface is in addition to the standard Azure network interface available to other VM sizes.</span></span> 
  
<span data-ttu-id="c052c-107">Du kan använda RDMA-kompatibla instanser att kommunicera över ett InfiniBand-nätverk med FDR priser för H16r och H16mr virtuella datorer och QDR priser för A8 och A9 virtuella datorer i gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c052c-107">This interface allows the RDMA-capable instances to communicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="c052c-108">Dessa funktioner för RDMA kan öka den skalbarhet och prestanda för Message Passing Interface (MPI) program.</span><span class="sxs-lookup"><span data-stu-id="c052c-108">These RDMA capabilities can boost the scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="c052c-109">Följande är kraven för RDMA-kompatibla virtuella Windows-datorer att ansluta till Azure RDMA-nätverket:</span><span class="sxs-lookup"><span data-stu-id="c052c-109">Following are requirements for RDMA-capable Windows VMs to access the Azure RDMA network:</span></span> 

* <span data-ttu-id="c052c-110">**Operativsystem**</span><span class="sxs-lookup"><span data-stu-id="c052c-110">**Operating system**</span></span>
  
  <span data-ttu-id="c052c-111">Windows Server 2012 R2, Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="c052c-111">Windows Server 2012 R2, Windows Server 2012</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="c052c-112">Windows Server 2016 stöder för närvarande inte RDMA-anslutningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="c052c-112">Currently, Windows Server 2016 does not support RDMA connectivity in Azure.</span></span>
  >

* <span data-ttu-id="c052c-113">**Tillgänglighetsuppsättning eller Molntjänsten** – distribuera RDMA-kompatibla virtuella datorer i samma tillgänglighetsuppsättning (när du använder Azure Resource Manager-distributionsmodellen) eller samma tjänst i molnet (när du använder den klassiska distributionsmodellen).</span><span class="sxs-lookup"><span data-stu-id="c052c-113">**Availability set or cloud service** – Deploy the RDMA-capable VMs in the same availability set (when you use the Azure Resource Manager deployment model) or the same cloud service (when you use the classic deployment model).</span></span> <span data-ttu-id="c052c-114">Om du använder Azure Batch måste RDMA-kompatibla virtuella datorer vara i samma pool.</span><span class="sxs-lookup"><span data-stu-id="c052c-114">If you use Azure Batch, the RDMA-capable VMs must be in the same pool.</span></span>

* <span data-ttu-id="c052c-115">**MPI** -Microsoft MPI (MS-MPI) 2012 R2 eller senare, Intel MPI biblioteket 5.x</span><span class="sxs-lookup"><span data-stu-id="c052c-115">**MPI** - Microsoft MPI (MS-MPI) 2012 R2 or later, Intel MPI Library 5.x</span></span>

  <span data-ttu-id="c052c-116">Stöds MPI-implementeringar kan du använda Microsoft Network Direct-gränssnittet för kommunikation mellan instanser.</span><span class="sxs-lookup"><span data-stu-id="c052c-116">Supported MPI implementations use the Microsoft Network Direct interface to communicate between instances.</span></span> 

* <span data-ttu-id="c052c-117">**RDMA nätverks-adressutrymme** -av RDMA-nätverk i Azure reserverar adressutrymme 172.16.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="c052c-117">**RDMA network address space** - The RDMA network in Azure reserves the address space 172.16.0.0/16.</span></span> <span data-ttu-id="c052c-118">Kontrollera att virtuella nätverkets adressutrymme inte överlappar RDMA-nätverk för att köra MPI program på instanser distribuerade i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="c052c-118">To run MPI applications on instances deployed in an Azure virtual network, make sure that the virtual network address space does not overlap the RDMA network.</span></span>

* <span data-ttu-id="c052c-119">**HpcVmDrivers VM-tillägget** -på RDMA-kompatibla virtuella datorer, måste du lägga till tillägget HpcVmDrivers för att installera drivrutiner för Windows-nätverk för RDMA-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c052c-119">**HpcVmDrivers VM extension** - On RDMA-capable VMs, you must add the HpcVmDrivers extension to install Windows network device drivers for RDMA connectivity.</span></span> <span data-ttu-id="c052c-120">(I vissa distributioner av A8- och A9 instanser HpcVmDrivers tillägg läggs automatiskt.) Om du vill lägga till tillägg för virtuell dator till en virtuell dator, kan du använda [Azure PowerShell](/powershell/azure/overview) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c052c-120">(In certain deployments of A8 and A9 instances, the HpcVmDrivers extension is added automatically.) To add the VM extension to a VM, you can use [Azure PowerShell](/powershell/azure/overview) cmdlets.</span></span> 

  
  <span data-ttu-id="c052c-121">Senaste version 1.1 HpcVMDrivers tillägget installeras i följande kommando på en befintlig RDMA-kompatibla virtuell dator med namnet *myVM* distribuerats i resursgruppen med namnet *myResourceGroup* i den  *USA, västra* region:</span><span class="sxs-lookup"><span data-stu-id="c052c-121">The following command installs the latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named *myVM* deployed in the resource group named *myResourceGroup* in the *West US* region:</span></span>

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  <span data-ttu-id="c052c-122">Mer information finns i [tillägg för virtuell dator och funktioner](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c052c-122">For more information, see [Virtual machine extensions and features](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="c052c-123">Du kan också fungera med tillägg för virtuella datorer som distribueras i det [klassiska distributionsmodellen](classic/manage-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="c052c-123">You can also work with extensions for VMs deployed in the [classic deployment model](classic/manage-extensions.md).</span></span>


## <a name="using-hpc-pack"></a><span data-ttu-id="c052c-124">Med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c052c-124">Using HPC Pack</span></span>

<span data-ttu-id="c052c-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, är ett alternativ för att skapa ett beräkningskluster i Azure kör MPI för Windows-baserade program och andra HPC-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="c052c-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you to create a compute cluster in Azure to run Windows-based MPI applications and other HPC workloads.</span></span> <span data-ttu-id="c052c-126">HPC Pack 2012 R2 och senare versioner innehåller en körningsmiljö för MS-MPI som använder Azure RDMA-nätverk när de distribueras på RDMA-kompatibla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c052c-126">HPC Pack 2012 R2 and later versions include a runtime environment for MS-MPI that uses the Azure RDMA network when deployed on RDMA-capable VMs.</span></span>




## <a name="other-sizes"></a><span data-ttu-id="c052c-127">Andra storlekar</span><span class="sxs-lookup"><span data-stu-id="c052c-127">Other sizes</span></span>
- [<span data-ttu-id="c052c-128">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="c052c-128">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="c052c-129">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="c052c-129">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="c052c-130">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="c052c-130">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="c052c-131">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="c052c-131">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="c052c-132">GPU-optimerad</span><span class="sxs-lookup"><span data-stu-id="c052c-132">GPU optimized</span></span>](sizes-gpu.md)

## <a name="next-steps"></a><span data-ttu-id="c052c-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c052c-133">Next steps</span></span>

- <span data-ttu-id="c052c-134">Checklistor använda beräkningsintensiva instanser med HPC Pack på Windows Server, se [ställa in ett RDMA-Windows-kluster med HPC Pack som kör MPI program](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c052c-134">For checklists to use the compute-intensive instances with HPC Pack on Windows Server, see [Set up a Windows RDMA cluster with HPC Pack to run MPI applications](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="c052c-135">Om du vill använda beräkningsintensiva instanser när du kör MPI-program med Azure Batch, se [använder flera instanser uppgifter för att köra program för Message Passing Interface (MPI) i Azure Batch](../../batch/batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="c052c-135">To use compute-intensive instances when running MPI applications with Azure Batch, see [Use multi-instance tasks to run Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).</span></span>

- <span data-ttu-id="c052c-136">Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.</span><span class="sxs-lookup"><span data-stu-id="c052c-136">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




