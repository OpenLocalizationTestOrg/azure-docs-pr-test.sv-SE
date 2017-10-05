---
title: "Alternativ för Linux HPC Pack kluster i molnet | Microsoft Docs"
description: "Lär dig mer om alternativen with Microsoft HPC Pack för att skapa och hantera en Linux med höga prestanda HPC-kluster i Azure-molnet"
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 616006d29149f7f47b03bd127cf3c83ad63a5c39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Alternativ med HPC Pack för att skapa och hantera ett HPC-kluster i Azure för Linux-arbetsbelastningar
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Den här artikeln fokuserar på alternativ för att köra arbetsbelastningar för Linux med HPC Pack. Det finns också alternativ för att köra [Windows HPC-arbetsbelastningar med HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Köra ett HPC Pack-kluster i virtuella Azure-datorer
### <a name="azure-templates"></a>Azure-mallar
* (GitHub) [HPC Pack 2016 klustret mallar](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack kluster för Linux-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Snabbstart) [Skapa ett HPC-kluster med beräkningsnoder för Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>PowerShell-distributionsskriptet
* [Skapa ett Linux-HPC-kluster med HPC Pack IaaS-distributionsskriptet](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Självstudier
* [Självstudier: Kom igång med Linux compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Självstudier: Kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Självstudier: Kör STAR-CCM + med Microsoft HPC Pack på en Linux RDMA kluster i Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Klusterhantering
* [Skicka jobb till ett HPC Pack kluster i Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Hantering av jobb i HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Skapa kluster som RDMA för MPI arbetsbelastningar
* [Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Ställa in ett Linux RDMA-kluster som kör MPI-program](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

