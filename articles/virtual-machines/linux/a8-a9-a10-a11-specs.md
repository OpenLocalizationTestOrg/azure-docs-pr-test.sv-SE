---
title: "Om beräkningsintensiva virtuella datorer med Linux | Microsoft Docs"
description: "Hämta bakgrundsinformation och överväganden för att använda de H-serien och A8 A9, A10 och A11 beräkningsintensiva storlekarna för virtuella Linux-datorer"
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
ms.openlocfilehash: a2307f7055966ec7146b5da0b4daf1ad469abe2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>Om H-serien och beräkningsintensiva A-series virtuella datorer för Linux
Här är bakgrundsinformation och vissa överväganden för att använda den nyare Azure H-serien och tidigare A8, A9, A10 och A11 storlekar, även kallat *beräkningsintensiva* instanser. Den här artikeln fokuserar på att använda dessa storlekar för virtuella Linux-datorer. Du kan också använda dem för [virtuella Windows-datorer](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Grundläggande specifikationerna, lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Åtkomst till nätverket RDMA
Du kan skapa kluster av RDMA-kompatibla virtuella Linux-datorer som kör något av följande stöds Linux HPC-distributioner och en stöds MPI-implementeringen för att dra nytta av RDMA-Azure-nätverket. Se [ställa in ett Linux RDMA-kluster som kör MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) för distributionsalternativ och exempel konfigurationssteg.

* **Distributioner** -du måste distribuera virtuella datorer från RDMA-kompatibla SUSE Linux Enterprise Server (SLES) eller falsk Wave-programvara (tidigare OpenLogic) CentOS-baserade HPC avbildningar i Azure Marketplace. Följande Marketplace-bilder har stöd för RDMA-anslutningen:
  
    * SLES 12 SP1 för HPC SLES 12 SP1 för HPC (Premium)
    
    * CentOS-baserade 7.1 HPC, CentOS-baserade 6.5 HPC  
 
        > [!NOTE]
        > För virtuella datorer H-serien rekommenderar vi antingen en SLES 12 SP1 för HPC-avbildningen eller CentOS-baserad 7.1 HPC-avbildning.
        >
        > CentOS-baserade HPC-avbildningar i kernel-uppdateringarna har inaktiverats i den **yum** konfigurationsfilen. Detta beror på att drivrutinerna Linux RDMA distribueras som en RPM-paket och drivrutinsuppdateringar kanske inte fungerar om kernel har uppdaterats.
        > 
        > 
* **MPI** -Intel MPI biblioteket 5.x
  
    Beroende på Marketplace-avbildning du väljer separat licensiering, installation eller konfiguration av Intel MPI kan behövas, enligt följande: 
  
  * **SLES 12 SP1 för HPC-avbildningen** -Intel MPI paket distribueras på den virtuella datorn. Installera genom att köra följande kommando:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **CentOS-baserade HPC-avbildningar** -Intel MPI 5.1 redan är installerad.  
    
    Ytterligare konfiguration krävs för att köra MPI-jobb på klustrade virtuella datorer. Till exempel i ett kluster för virtuella datorer behöver du upprätta förtroende mellan compute-noder. Vanliga inställningar Se [ställa in ett Linux RDMA-kluster som kör MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>Överväganden för HPC Pack och Linux
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, ger ett alternativ som du kan använda beräkningsintensiva instanser med Linux. De senaste versionerna av HPC Pack stöd flera Linux-distributioner som ska köras på compute-noder som är distribuerad i virtuella Azure-datorer hanteras av en Windows Server-huvudnod. Med RDMA-kompatibla Linux datornoderna kör MPI Intel, HPC Pack schemalägga och köra Linux MPI program som kommer åt nätverket RDMA. Om du vill komma igång finns [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Topologiöverväganden för nätverk
* Eth1 är reserverat för RDMA-nätverkstrafik på RDMA-aktiverade Linux virtuella datorer i Azure. Ändra inte Eth1 inställningar eller information i konfigurationsfilen som hänvisar till det här nätverket. Eth0 är reserverat för vanliga Azure nätverkstrafik.
* IP över InfiniBand (IB) stöds inte i Azure. Endast RDMA över IB stöds.



## <a name="next-steps"></a>Nästa steg
* Mer information om tillgänglighet och pris i beräkningsintensiva storlekar finns [virtuella datorer priser](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Om du vill komma igång distribuerar och använder beräkningsintensiva storlekar med RDMA på Linux, se [ställa in ett Linux RDMA-kluster som kör MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

