---
title: aaaLinux HPC Pack klustret alternativ i hello molnet | Microsoft Docs
description: "Lär dig mer om alternativen med Microsoft HPC Pack toocreate och hantera en Linux med höga prestanda HPC-kluster i hello Azure-molnet"
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
ms.openlocfilehash: 60d093b466f44a0391815842c421c328e8c7a0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a><span data-ttu-id="d6c76-103">Alternativ med HPC Pack toocreate och hantera ett HPC-kluster i Azure för Linux-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="d6c76-103">Options with HPC Pack toocreate and manage an HPC cluster in Azure for Linux workloads</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="d6c76-104">Den här artikeln fokuserar på alternativ toouse HPC Pack toorun Linux arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="d6c76-104">This article focuses on options toouse HPC Pack toorun Linux workloads.</span></span> <span data-ttu-id="d6c76-105">Det finns också alternativ för att köra [Windows HPC-arbetsbelastningar med HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d6c76-105">There are also options for running [Windows HPC workloads with HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="d6c76-106">Köra ett HPC Pack-kluster i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="d6c76-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="d6c76-107">Azure-mallar</span><span class="sxs-lookup"><span data-stu-id="d6c76-107">Azure templates</span></span>
* <span data-ttu-id="d6c76-108">(GitHub) [HPC Pack 2016 klustret mallar](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="d6c76-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="d6c76-109">(Marketplace) [HPC Pack kluster för Linux-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span><span class="sxs-lookup"><span data-stu-id="d6c76-109">(Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span></span>
* <span data-ttu-id="d6c76-110">(Snabbstart) [Skapa ett HPC-kluster med beräkningsnoder för Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span><span class="sxs-lookup"><span data-stu-id="d6c76-110">(Quickstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span></span>

### <a name="powershell-deployment-script"></a><span data-ttu-id="d6c76-111">PowerShell-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="d6c76-111">PowerShell deployment script</span></span>
* [<span data-ttu-id="d6c76-112">Skapa ett Linux-HPC-kluster med hello HPC Pack IaaS distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="d6c76-112">Create a Linux HPC cluster with hello HPC Pack IaaS deployment script</span></span>](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="d6c76-113">Självstudier</span><span class="sxs-lookup"><span data-stu-id="d6c76-113">Tutorials</span></span>
* [<span data-ttu-id="d6c76-114">Självstudier: Kom igång med Linux compute-noder i ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-114">Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="d6c76-115">Självstudier: Kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-115">Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="d6c76-116">Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-116">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="d6c76-117">Självstudier: Kör STAR-CCM + med Microsoft HPC Pack på en Linux RDMA kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-117">Tutorial: Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="d6c76-118">Klusterhantering</span><span class="sxs-lookup"><span data-stu-id="d6c76-118">Cluster management</span></span>
* [<span data-ttu-id="d6c76-119">Skicka jobb tooan HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-119">Submit jobs tooan HPC Pack cluster in Azure</span></span>](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d6c76-120">Hantering av jobb i HPC Pack</span><span class="sxs-lookup"><span data-stu-id="d6c76-120">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="d6c76-121">Skapa kluster som RDMA för MPI arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="d6c76-121">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="d6c76-122">Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="d6c76-122">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="d6c76-123">Ställa in en Linux RDMA klustret toorun MPI-program</span><span class="sxs-lookup"><span data-stu-id="d6c76-123">Set up a Linux RDMA cluster toorun MPI applications</span></span>](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

