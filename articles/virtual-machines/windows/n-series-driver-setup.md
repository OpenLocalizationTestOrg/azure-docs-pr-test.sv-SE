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
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Ställ in GPU drivrutiner för N-serien virtuella datorer som kör Windows Server
tootake nytta av hello GPU-funktionerna i Azure N-serien virtuella datorer som kör Windows Server 2016 eller Windows Server 2012 R2, installera stöd för grafik drivrutinerna. Den här artikeln innehåller drivrutinen konfigurationsstegen när du distribuerar en virtuell dator i N-serien. Inställningsinformation för drivrutinen är också tillgängligt för [virtuella Linux-datorer](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Grundläggande specifikationerna, lagringskapacitet och diskinformation finns [GPU Windows VM-storlekar](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Installation av drivrutiner

1. Anslut med fjärrskrivbord tooeach N-serien VM.

2. Hämta, extrahera och installera drivrutinen för hello stöds för Windows-operativsystemet.

På Azure NV virtuella datorer startas om efter drivrutinsinstallationen av. På NC virtuella datorer är inte en omstart krävs.

## <a name="verify-driver-installation"></a>Verifiera installation av drivrutiner

Du kan kontrollera installationen av drivrutinen i Enhetshanteraren. hello följande exempel visar lyckad konfiguration av hello Tesla K80 kort på en Azure NC-VM.

![Egenskaper för GPU-drivrutin](./media/n-series-driver-setup/GPU_driver_properties.png)

tooquery hello GPU enhetstillstånd kör hello [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) kommandoradsverktyg som installerats med hello-drivrutinen.

1. Öppna en kommandotolk och ändra toohello **C:\Program Files\NVIDIA Corporation\NVSMI** directory.

2. Kör **nvidia smi**. Om hello drivrutinen är installerad visas utdata liknande toobelow. Observera att **GPU-Util** visar **0%** om du använder en GPU arbetsbelastning på hello VM.

![NVIDIA Enhetsstatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA-nätverk för virtuella datorer NC24r

RDMA-nätverksanslutning kan aktiveras på NC24r virtuella datorer som distribueras i hello samma tillgänglighetsuppsättning. Hej HpcVmDrivers tillägg måste läggas tooinstall Windows nätverksenhetsdrivrutiner som möjliggör RDMA-anslutning. tooadd hello VM-tillägget tooan NC24r VM, använda [Azure PowerShell](/powershell/azure/overview) cmdlets för Azure Resource Manager.

> [!NOTE]
> För närvarande stöder endast Windows Server 2012 R2 hello RDMA-nätverk på NC24r virtuella datorer.
> 

tooinstall hello senaste version 1.1 HpcVMDrivers tillägg på en befintlig RDMA-kompatibla virtuell dator med namnet myVM i hello västra USA region:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Mer information finns i [tillägg för virtuell dator och funktioner i Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

hello RDMA nätverket har stöd för Message Passing Interface (MPI) trafik för program som körs med [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) eller Intel MPI 5.x. 


## <a name="next-steps"></a>Nästa steg

* Mer information om hello NVIDIA GPU-kort på hello VMs N-serien finns i:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (för Azure NC virtuella datorer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (för Azure NV virtuella datorer)

* Utvecklare som bygger GPU-snabbare program för hello NVIDIA Tesla GPU-kort kan också hämta och installera hello CUDA Toolkit 8 för [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) eller [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe). Mer information finns i hello [CUDA installationsguiden](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


