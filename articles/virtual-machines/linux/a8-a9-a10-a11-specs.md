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
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>Om H-serien och beräkningsintensiva A-series virtuella datorer för Linux
Här är bakgrundsinformation och vissa aspekter för att använda hello nyare Azure H-serien och hello tidigare A8, A9, A10 och A11 storlekar, även kallat *beräkningsintensiva* instanser. Den här artikeln fokuserar på att använda dessa storlekar för virtuella Linux-datorer. Du kan också använda dem för [virtuella Windows-datorer](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Grundläggande specifikationerna, lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-toohello-rdma-network"></a>Åtkomst toohello RDMA nätverk
Du kan skapa kluster av RDMA-kompatibla virtuella Linux-datorer som kör något av hello efter Linux HPC-distributioner som stöds och en stöds MPI implementering tootake utnyttja hello Azure RDMA-nätverk. Se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) för distributionsalternativ och exempel konfigurationssteg.

* **Distributioner** – du måste distribuera virtuella datorer från RDMA-kompatibla SUSE Linux Enterprise Server (SLES) eller falsk Wave-programvara (tidigare OpenLogic) CentOS-baserade HPC-avbildningar i hello Azure Marketplace. hello följande Marketplace-bilder har stöd för RDMA-anslutningen:
  
    * SLES 12 SP1 för HPC SLES 12 SP1 för HPC (Premium)
    
    * CentOS-baserade 7.1 HPC, CentOS-baserade 6.5 HPC  
 
        > [!NOTE]
        > För virtuella datorer H-serien rekommenderar vi antingen en SLES 12 SP1 för HPC-avbildningen eller CentOS-baserad 7.1 HPC-avbildning.
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
    
    Ytterligare konfiguration är nödvändiga toorun MPI-jobb på klustrade virtuella datorer. Till exempel i ett kluster för virtuella datorer, måste tooestablish förtroende mellan hello compute-noder. Vanliga inställningar Se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>Överväganden för HPC Pack och Linux
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofts ledigt HPC-kluster och jobbet hanteringslösningen, ger ett alternativ för du toouse hello beräkningsintensiva instanser med Linux. hello senaste versioner av HPC Pack stöd för flera Linux-distributioner toorun på compute-noder som är distribuerad i virtuella Azure-datorer hanteras av en Windows Server-huvudnod. Med RDMA-kompatibla Linux datornoderna kör Intel MPI, kan HPC Pack schemalägga och köra Linux MPI program åtkomst hello RDMA nätverket. tooget igång, se [komma igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Topologiöverväganden för nätverk
* Eth1 är reserverat för RDMA-nätverkstrafik på RDMA-aktiverade Linux virtuella datorer i Azure. Ändra inte Eth1 inställningar eller information i hello konfigurationsfilen hänvisar toothis nätverk. Eth0 är reserverat för vanliga Azure nätverkstrafik.
* IP över InfiniBand (IB) stöds inte i Azure. Endast RDMA över IB stöds.



## <a name="next-steps"></a>Nästa steg
* Mer information om tillgänglighet och pris i hello beräkningsintensiva storlekar finns [virtuella datorer priser](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Lagringskapacitet och diskinformation finns [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* tooget igång distribuerar och använder beräkningsintensiva storlekar med RDMA på Linux, se [ställa in en Linux RDMA klustret toorun MPI program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

