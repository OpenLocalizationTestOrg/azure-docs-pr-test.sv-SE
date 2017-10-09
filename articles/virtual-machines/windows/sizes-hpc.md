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
# <a name="high-performance-compute-vm-sizes"></a>Högpresterande beräkning VM-storlekar

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-kompatibla instanser
En delmängd av hello beräkningsintensiva instanser (H16r, H16mr, A8 och A9) har ett nätverksgränssnitt för remote direct memory access (RDMA)-anslutning. Det här gränssnittet är dessutom toohello Azure standardnätverk gränssnittet tillgängliga tooother VM-storlekar. 
  
Det här gränssnittet kan hello RDMA-kompatibla instanser toocommunicate InfiniBand-nätverk, arbetar med FDR priser för H16r och H16mr virtuella datorer och QDR priser för A8 och A9 virtuella datorer. Dessa funktioner för RDMA kan öka hello skalbarhet och prestanda för Message Passing Interface (MPI) program.

Följande är krav för RDMA-kompatibla virtuella Windows-datorer tooaccess hello Azure RDMA nätverk: 

* **Operativsystem**
  
  Windows Server 2012 R2, Windows Server 2012
  
  > [!NOTE]
  > Windows Server 2016 stöder för närvarande inte RDMA-anslutningar i Azure.
  >

* **Tillgänglighetsuppsättning eller Molntjänsten** – distribuera hello RDMA-kompatibla virtuella datorer i hello samma tillgänglighetsuppsättning (när du använder hello Azure Resource Manager-distributionsmodellen) eller hello samma tjänst i molnet (när du använder hello klassiska distributionsmodellen). Om du använder Azure Batch hello RDMA-kompatibla virtuella datorer måste vara i hello samma pool.

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 eller senare, Intel MPI biblioteket 5.x

  Det används hello Microsoft Network Direct gränssnittet toocommunicate mellan instanser stöds MPI-implementeringar. 

* **RDMA nätverks-adressutrymme** -hello RDMA nätverk i Azure reserverar hello adressutrymme 172.16.0.0/16. toorun MPI program på instanser distribuerade i Azure-nätverk, kontrollera att hello virtuella nätverkets adressutrymme inte överlappar hello RDMA nätverk.

* **HpcVmDrivers VM-tillägget** -på RDMA-kompatibla virtuella datorer, måste du lägga till hello HpcVmDrivers tillägget tooinstall Windows network-drivrutiner för RDMA-anslutning. (I vissa distributioner av A8- och A9 instanser hello HpcVmDrivers tillägg läggs automatiskt.) tooadd hello VM-tillägget tooa VM som du kan använda [Azure PowerShell](/powershell/azure/overview) cmdlets. 

  
  Hej följande kommando installerar hello senaste version 1.1 HpcVMDrivers tillägg på en befintlig RDMA-kompatibla virtuell dator med namnet *myVM* distribuerats i hello resursgrupp med namnet *myResourceGroup* i hello  *USA, västra* region:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Mer information finns i [tillägg för virtuell dator och funktioner](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Du kan också fungera med tillägg för virtuella datorer som distribueras i hello [klassiska distributionsmodellen](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Med HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, är ett alternativ för toocreate ett beräkningskluster i Azure toorun MPI för Windows-baserade program och andra HPC-arbetsbelastningar. HPC Pack 2012 R2 och senare versioner innehåller en körningsmiljö för MS-MPI som använder hello Azure RDMA nätverk när de distribueras på RDMA-kompatibla virtuella datorer.




## <a name="other-sizes"></a>Andra storlekar
- [Generellt syfte](sizes-general.md)
- [Beräkningsoptimerad](sizes-compute.md)
- [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md)
- [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md)
- [GPU-optimerad](sizes-gpu.md)

## <a name="next-steps"></a>Nästa steg

- Checklistor toouse hello beräkningsintensiva instanser med HPC Pack på Windows Server, se [ställer in ett RDMA-Windows-kluster med HPC Pack toorun MPI program](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- När du kör MPI-program med Azure Batch-toouse beräkningsintensiva instanser finns [Använd flera instanser av åtgärder toorun Message Passing Interface (MPI) program i Azure Batch](../../batch/batch-mpi.md).

- Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.




