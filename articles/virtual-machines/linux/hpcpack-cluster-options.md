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
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a><span data-ttu-id="0ca43-103">Alternativ med HPC Pack för att skapa och hantera ett HPC-kluster i Azure för Linux-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="0ca43-103">Options with HPC Pack to create and manage an HPC cluster in Azure for Linux workloads</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="0ca43-104">Den här artikeln fokuserar på alternativ för att köra arbetsbelastningar för Linux med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="0ca43-104">This article focuses on options to use HPC Pack to run Linux workloads.</span></span> <span data-ttu-id="0ca43-105">Det finns också alternativ för att köra [Windows HPC-arbetsbelastningar med HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0ca43-105">There are also options for running [Windows HPC workloads with HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="0ca43-106">Köra ett HPC Pack-kluster i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="0ca43-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="0ca43-107">Azure-mallar</span><span class="sxs-lookup"><span data-stu-id="0ca43-107">Azure templates</span></span>
* <span data-ttu-id="0ca43-108">(GitHub) [HPC Pack 2016 klustret mallar](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="0ca43-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="0ca43-109">(Marketplace) [HPC Pack kluster för Linux-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span><span class="sxs-lookup"><span data-stu-id="0ca43-109">(Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span></span>
* <span data-ttu-id="0ca43-110">(Snabbstart) [Skapa ett HPC-kluster med beräkningsnoder för Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span><span class="sxs-lookup"><span data-stu-id="0ca43-110">(Quickstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span></span>

### <a name="powershell-deployment-script"></a><span data-ttu-id="0ca43-111">PowerShell-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="0ca43-111">PowerShell deployment script</span></span>
* [<span data-ttu-id="0ca43-112">Skapa ett Linux-HPC-kluster med HPC Pack IaaS-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="0ca43-112">Create a Linux HPC cluster with the HPC Pack IaaS deployment script</span></span>](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="0ca43-113">Självstudier</span><span class="sxs-lookup"><span data-stu-id="0ca43-113">Tutorials</span></span>
* [<span data-ttu-id="0ca43-114">Självstudier: Kom igång med Linux compute-noder i ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-114">Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="0ca43-115">Självstudier: Kör NAMD with Microsoft HPC Pack på Linux compute-noder i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-115">Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="0ca43-116">Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-116">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="0ca43-117">Självstudier: Kör STAR-CCM + med Microsoft HPC Pack på en Linux RDMA kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-117">Tutorial: Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="0ca43-118">Klusterhantering</span><span class="sxs-lookup"><span data-stu-id="0ca43-118">Cluster management</span></span>
* [<span data-ttu-id="0ca43-119">Skicka jobb till ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-119">Submit jobs to an HPC Pack cluster in Azure</span></span>](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="0ca43-120">Hantering av jobb i HPC Pack</span><span class="sxs-lookup"><span data-stu-id="0ca43-120">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="0ca43-121">Skapa kluster som RDMA för MPI arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="0ca43-121">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="0ca43-122">Självstudier: Kör OpenFOAM with Microsoft HPC Pack på en Linux RDMA-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0ca43-122">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="0ca43-123">Ställa in ett Linux RDMA-kluster som kör MPI-program</span><span class="sxs-lookup"><span data-stu-id="0ca43-123">Set up a Linux RDMA cluster to run MPI applications</span></span>](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

