---
title: "aaaAzure N-serien drivrutinsinstallation för Windows | Microsoft Docs"
description: "Hur tooset in NVIDIA GPU drivrutiner för N-serien virtuella datorer som kör Windows i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2acce57d4f9eb1d02bf3bc0303bcb9690475698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a><span data-ttu-id="33a74-103">Ställ in GPU drivrutiner för N-serien virtuella datorer som kör Windows Server</span><span class="sxs-lookup"><span data-stu-id="33a74-103">Set up GPU drivers for N-series VMs running Windows Server</span></span>
<span data-ttu-id="33a74-104">tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, installera stöd för grafik drivrutinerna.</span><span class="sxs-lookup"><span data-stu-id="33a74-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Windows Server 2016 or Windows Server 2012 R2, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="33a74-105">Den här artikeln innehåller drivrutinen konfigurationsstegen när du distribuerar en virtuell dator i N-serien.</span><span class="sxs-lookup"><span data-stu-id="33a74-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="33a74-106">Inställningsinformation för drivrutinen är också tillgängligt för [virtuella Linux-datorer](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33a74-106">Driver setup information is also available for [Linux VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="33a74-107">Grundläggande specifikationerna, lagringskapacitet och diskinformation finns [GPU Windows VM-storlekar](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33a74-107">For basic specs, storage capacities, and disk details, see [GPU Windows VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a><span data-ttu-id="33a74-108">Installation av drivrutiner</span><span class="sxs-lookup"><span data-stu-id="33a74-108">Driver installation</span></span>

1. <span data-ttu-id="33a74-109">Anslut med fjärrskrivbord tooeach N-serien VM.</span><span class="sxs-lookup"><span data-stu-id="33a74-109">Connect by Remote Desktop tooeach N-series VM.</span></span>

2. <span data-ttu-id="33a74-110">Hämta, extrahera och installera drivrutinen för hello stöds för Windows-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="33a74-110">Download, extract, and install hello supported driver for your Windows operating system.</span></span>

<span data-ttu-id="33a74-111">På Azure NV virtuella datorer startas om efter drivrutinsinstallationen av.</span><span class="sxs-lookup"><span data-stu-id="33a74-111">On Azure NV VMs, a restart is required after driver installation.</span></span> <span data-ttu-id="33a74-112">På NC virtuella datorer är inte en omstart krävs.</span><span class="sxs-lookup"><span data-stu-id="33a74-112">On NC VMs, a restart is not required.</span></span>

## <a name="verify-driver-installation"></a><span data-ttu-id="33a74-113">Verifiera installation av drivrutiner</span><span class="sxs-lookup"><span data-stu-id="33a74-113">Verify driver installation</span></span>

<span data-ttu-id="33a74-114">Du kan kontrollera installationen av drivrutinen i Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="33a74-114">You can verify driver installation in Device Manager.</span></span> <span data-ttu-id="33a74-115">hello följande exempel visar lyckad konfiguration av hello Tesla K80 kort på en Azure NC-VM.</span><span class="sxs-lookup"><span data-stu-id="33a74-115">hello following example shows successful configuration of hello Tesla K80 card on an Azure NC VM.</span></span>

![Egenskaper för GPU-drivrutin](./media/n-series-driver-setup/GPU_driver_properties.png)

<span data-ttu-id="33a74-117">tooquery hello GPU enhetstillstånd kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="33a74-117">tooquery hello GPU device state, run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span>

1. <span data-ttu-id="33a74-118">Öppna en kommandotolk och ändra toohello **C:\Program Files\NVIDIA Corporation\NVSMI** directory.</span><span class="sxs-lookup"><span data-stu-id="33a74-118">Open a command prompt and change toohello **C:\Program Files\NVIDIA Corporation\NVSMI** directory.</span></span>

2. <span data-ttu-id="33a74-119">Kör **nvidia smi**.</span><span class="sxs-lookup"><span data-stu-id="33a74-119">Run **nvidia-smi**.</span></span> <span data-ttu-id="33a74-120">Om hello drivrutinen är installerad visas utdata liknande toobelow.</span><span class="sxs-lookup"><span data-stu-id="33a74-120">If hello driver is installed you will see output similar toobelow.</span></span> <span data-ttu-id="33a74-121">Observera att **GPU-Util** visar **0%** om du använder en GPU arbetsbelastning på hello VM.</span><span class="sxs-lookup"><span data-stu-id="33a74-121">Note that **GPU-Util** shows **0%** unless you are currently running a GPU workload on hello VM.</span></span>

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a><span data-ttu-id="33a74-123">RDMA-nätverk för virtuella datorer NC24r</span><span class="sxs-lookup"><span data-stu-id="33a74-123">RDMA network for NC24r VMs</span></span>

<span data-ttu-id="33a74-124">RDMA-nätverksanslutning kan aktiveras på NC24r virtuella datorer som distribueras i hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="33a74-124">RDMA network connectivity can be enabled on NC24r VMs deployed in hello same availability set.</span></span> <span data-ttu-id="33a74-125">Hej HpcVmDrivers tillägg måste läggas tooinstall Windows nätverksenhetsdrivrutiner som möjliggör RDMA-anslutning.</span><span class="sxs-lookup"><span data-stu-id="33a74-125">hello HpcVmDrivers extension must be added tooinstall Windows network device drivers that enable RDMA connectivity.</span></span> <span data-ttu-id="33a74-126">tooadd hello VM-tillägget tooan NC24r VM, använda [Azure PowerShell](/powershell/azure/overview) cmdlets för Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="33a74-126">tooadd hello VM extension tooan NC24r VM, use [Azure PowerShell](/powershell/azure/overview) cmdlets for Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="33a74-127">För närvarande stöder endast Windows Server 2012 R2 hello RDMA-nätverk på NC24r virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="33a74-127">Currently, only Windows Server 2012 R2 supports hello RDMA network on NC24r VMs.</span></span>
> 

<span data-ttu-id="33a74-128">tooinstall hello senaste version 1.1 HpcVMDrivers tillägg på en befintlig RDMA-kompatibla virtuell dator med namnet myVM i hello västra USA region:</span><span class="sxs-lookup"><span data-stu-id="33a74-128">tooinstall hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named myVM in hello West US region:</span></span>
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  <span data-ttu-id="33a74-129">Mer information finns i [tillägg för virtuell dator och funktioner i Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33a74-129">For more information, see [Virtual machine extensions and features for Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="33a74-130">hello RDMA nätverket har stöd för Message Passing Interface (MPI) trafik för program som körs med [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) eller Intel MPI 5.x.</span><span class="sxs-lookup"><span data-stu-id="33a74-130">hello RDMA network supports Message Passing Interface (MPI) traffic for applications running with [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) or Intel MPI 5.x.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="33a74-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33a74-131">Next steps</span></span>

* <span data-ttu-id="33a74-132">Mer information om hello NVIDIA GPU-kort på hello VMs N-serien finns i:</span><span class="sxs-lookup"><span data-stu-id="33a74-132">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="33a74-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (för Azure NC virtuella datorer)</span><span class="sxs-lookup"><span data-stu-id="33a74-133">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="33a74-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (för Azure NV virtuella datorer)</span><span class="sxs-lookup"><span data-stu-id="33a74-134">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="33a74-135">Utvecklare som bygger GPU-snabbare program för hello NVIDIA Tesla GPU-kort kan också hämta och installera hello CUDA Toolkit 8 för [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) eller [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span><span class="sxs-lookup"><span data-stu-id="33a74-135">Developers building GPU-accelerated applications for hello NVIDIA Tesla GPUs can also download and install hello CUDA Toolkit 8 for [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) or [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe).</span></span> <span data-ttu-id="33a74-136">Mer information finns i hello [CUDA installationsguiden](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span><span class="sxs-lookup"><span data-stu-id="33a74-136">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).</span></span>


