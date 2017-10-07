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
# <a name="high-performance-compute-linux-vm-sizes"></a>Högpresterande compute Linux VM-storlekar

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA-kompatibla instanser
En delmängd av hello beräkningsintensiva instanser (H16r, H16mr, A8 och A9) har ett nätverksgränssnitt för remote direct memory access (RDMA)-anslutning. Det här gränssnittet är dessutom toohello Azure standardnätverk gränssnittet tillgängliga tooother VM-storlekar. 
  
Det här gränssnittet kan hello RDMA-kompatibla instanser toocommunicate InfiniBand-nätverk, arbetar med FDR priser för H16r och H16mr virtuella datorer och QDR priser för A8 och A9 virtuella datorer. Dessa funktioner för RDMA kan öka hello skalbarhet och prestanda för Message Passing Interface (MPI) program.

Följande är krav för RDMA-kompatibla virtuella Linux-datorer tooaccess hello Azure RDMA nätverk:
 
* **Distributioner** – du måste distribuera virtuella datorer från RDMA-kompatibla SUSE Linux Enterprise Server (SLES) eller falsk Wave-programvara (tidigare OpenLogic) CentOS-baserade HPC-avbildningar i hello Azure Marketplace. hello följande Marketplace-bilder har stöd för RDMA-anslutningen:
  
    * SLES 12 SP1 för HPC eller SLES 12 SP1 för HPC (Premium)
    
    * CentOS-baserade 7.3 HPC, CentOS-baserade 7.1 HPC, CentOS-baserade 6,8 HPC eller CentOS-baserade 6.5 HPC  
 
        > [!NOTE]
        > För virtuella datorer H-serien rekommenderar vi en SLES 12 SP1 för HPC-avbildningen eller CentOS-baserade 7.1 eller senare HPC-bild.
        >
        > Hej CentOS-baserade HPC-avbildningar i kernel-uppdateringarna har inaktiverats i hello **yum** konfigurationsfilen. Detta beror på att hello Linux RDMA drivrutiner distribueras som en RPM-paket och drivrutinsuppdateringar kanske inte fungerar om hello kernel uppdateras.
        > 
        > 
* **MPI** -Intel MPI biblioteket 5.x
  
    Beroende på hello Marketplace-avbildning du väljer separat licensiering, installation eller konfiguration av Intel MPI kan behövas, enligt följande: 
  
  * **SLES 12 SP1 för HPC-avbildningen** -Intel MPI paket distribueras på hello VM. Installera genom att köra följande kommando hello:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **CentOS-baserade HPC-avbildningar** -Intel MPI 5.1 redan är installerad.  
    
    Ytterligare konfiguration är nödvändiga toorun MPI-jobb på klustrade virtuella datorer. Till exempel i ett kluster för virtuella datorer, måste tooestablish förtroende mellan hello compute-noder. Vanliga inställningar finns [ställa in en Linux RDMA klustret toorun MPI-program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="network-topology-considerations"></a>Topologiöverväganden för nätverk
* Eth1 är reserverat för RDMA-nätverkstrafik på RDMA-aktiverade Linux virtuella datorer i Azure. Ändra inte Eth1 inställningar eller information i hello konfigurationsfilen hänvisar toothis nätverk. Eth0 är reserverat för vanliga Azure nätverkstrafik.

* IP över InfiniBand (IB) stöds inte i Azure. Endast RDMA över IB stöds.

## <a name="using-hpc-pack"></a>Med HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, är ett alternativ för du toouse hello beräkningsintensiva instanser med Linux. hello senaste versioner av HPC Pack stöd för flera Linux-distributioner toorun på compute-noder som är distribuerad i virtuella Azure-datorer hanteras av en Windows Server-huvudnod. Med RDMA-kompatibla Linux datornoderna kör Intel MPI, kan HPC Pack schemalägga och köra Linux MPI program åtkomst hello RDMA nätverket. Se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Andra storlekar
- [Generellt syfte](sizes-general.md)
- [Beräkningsoptimerad](sizes-compute.md)
- [Minnesoptimerad](sizes-memory.md)
- [Lagringsoptimerad](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Nästa steg

- tooget igång distribuerar och använder beräkningsintensiva storlekar med RDMA på Linux, se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Läs mer om hur [Azure compute-enheter (ACU)](acu.md) kan hjälpa dig att jämföra beräkning prestanda över Azure SKU: er.




