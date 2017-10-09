---
title: aaaWindows HPC Pack Klusterdistribution i hello molnet | Microsoft Docs
description: "Lär dig mer om alternativen med Microsoft HPC Pack toocreate och hantera en Windows med höga prestanda HPC-kluster i hello Azure-molnet"
services: virtual-machines-windows,cloud-services,batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 6158d9dd0cecda38b14f85a2b2080163f18e8cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a>Alternativ med HPC Pack toocreate och hantera Windows HPC-kluster i Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Den här artikeln fokuserar på alternativ toocreate HPC Pack kluster toorun Windows arbetsbelastningar. Det finns också alternativ för att skapa HPC Pack kluster toorun [Linux HPC-arbetsbelastning](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Köra ett HPC Pack-kluster i virtuella Azure-datorer
### <a name="azure-templates"></a>Azure-mallar
* (GitHub) [HPC Pack 2016 klustret mallar](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack kluster för Windows-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)
* (Marketplace) [HPC Pack kluster för Excel arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)
* (Snabbstart) [Skapa ett HPC-kluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Snabbstart) [Skapa ett HPC-kluster med nod-avbildning för anpassade beräkningen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM-avbildningar
* [Huvudnod i HPC Pack 2016 på Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016 compute-nod på Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016 huvudnod i Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2016 compute-nod på Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [Huvudnod i HPC Pack 2012 R2 på Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [HPC Pack 2012 R2-beräkningsnod i Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [HPC Pack beräkningsnod med Excel på Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a>PowerShell-distributionsskriptet
* [Skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Självstudier
* [Självstudier: Distribuera ett HPC Pack 2016-kluster i Azure](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Självstudier: Kom igång med ett HPC Pack kluster i Azure toorun Excel och SOA-arbetsbelastningar](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a>Manuell distribution med hello Azure-portalen
* [Ställ in hello huvudnod i HPC Pack-kluster i en Azure VM](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a>Klusterhantering
* [Hantera compute-noder i ett HPC Pack kluster i Azure](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Växa eller krympa Azure-beräkningsresurser i ett HPC Pack-kluster](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Skicka jobb tooan HPC Pack kluster i Azure](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Hantering av jobb i HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)
* [Hantera ett HPC Pack kluster i Azure med Azure Active Directory](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a>Lägga till worker-rollen noder tooan HPC Pack kluster
* [Burst tooAzure worker-instanser med HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [Självstudier: Skapa en hybrid-kluster med HPC Pack i Azure](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [Lägg till Azure ”burst” noder tooan HPC Pack huvudnod i Azure](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a>Integrera med Azure Batch
* [TooAzure burst med HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Skapa kluster som RDMA för MPI arbetsbelastningar
* [Konfigurera ett RDMA för Windows-kluster med HPC Pack toorun MPI program](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

