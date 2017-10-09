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
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a><span data-ttu-id="ff962-103">Alternativ med HPC Pack toocreate och hantera Windows HPC-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-103">Options with HPC Pack toocreate and manage a Windows HPC cluster in Azure</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="ff962-104">Den här artikeln fokuserar på alternativ toocreate HPC Pack kluster toorun Windows arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="ff962-104">This article focuses on options toocreate HPC Pack clusters toorun Windows workloads.</span></span> <span data-ttu-id="ff962-105">Det finns också alternativ för att skapa HPC Pack kluster toorun [Linux HPC-arbetsbelastning](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ff962-105">There are also options for creating HPC Pack clusters toorun [Linux HPC workloads](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="ff962-106">Köra ett HPC Pack-kluster i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="ff962-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="ff962-107">Azure-mallar</span><span class="sxs-lookup"><span data-stu-id="ff962-107">Azure templates</span></span>
* <span data-ttu-id="ff962-108">(GitHub) [HPC Pack 2016 klustret mallar](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="ff962-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="ff962-109">(Marketplace) [HPC Pack kluster för Windows-arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span><span class="sxs-lookup"><span data-stu-id="ff962-109">(Marketplace) [HPC Pack cluster for Windows workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span></span>
* <span data-ttu-id="ff962-110">(Marketplace) [HPC Pack kluster för Excel arbetsbelastningar](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span><span class="sxs-lookup"><span data-stu-id="ff962-110">(Marketplace) [HPC Pack cluster for Excel workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span></span>
* <span data-ttu-id="ff962-111">(Snabbstart) [Skapa ett HPC-kluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span><span class="sxs-lookup"><span data-stu-id="ff962-111">(Quickstart) [Create an HPC cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span></span>
* <span data-ttu-id="ff962-112">(Snabbstart) [Skapa ett HPC-kluster med nod-avbildning för anpassade beräkningen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span><span class="sxs-lookup"><span data-stu-id="ff962-112">(Quickstart) [Create an HPC cluster with custom compute node image](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span></span>

### <a name="azure-vm-images"></a><span data-ttu-id="ff962-113">Azure VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="ff962-113">Azure VM images</span></span>
* [<span data-ttu-id="ff962-114">Huvudnod i HPC Pack 2016 på Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ff962-114">HPC Pack 2016 head node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="ff962-115">HPC Pack 2016 compute-nod på Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ff962-115">HPC Pack 2016 compute node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="ff962-116">HPC Pack 2016 huvudnod i Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ff962-116">HPC Pack 2016 head node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="ff962-117">HPC Pack 2016 compute-nod på Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ff962-117">HPC Pack 2016 compute node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="ff962-118">Huvudnod i HPC Pack 2012 R2 på Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ff962-118">HPC Pack 2012 R2 head node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [<span data-ttu-id="ff962-119">HPC Pack 2012 R2-beräkningsnod i Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ff962-119">HPC Pack 2012 R2 compute node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [<span data-ttu-id="ff962-120">HPC Pack beräkningsnod med Excel på Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="ff962-120">HPC Pack compute node with Excel on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a><span data-ttu-id="ff962-121">PowerShell-distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="ff962-121">PowerShell deployment script</span></span>
* [<span data-ttu-id="ff962-122">Skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="ff962-122">Create an HPC cluster with hello HPC Pack IaaS deployment script</span></span>](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="ff962-123">Självstudier</span><span class="sxs-lookup"><span data-stu-id="ff962-123">Tutorials</span></span>
* [<span data-ttu-id="ff962-124">Självstudier: Distribuera ett HPC Pack 2016-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-124">Tutorial: Deploy an HPC Pack 2016 cluster in Azure</span></span>](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="ff962-125">Självstudier: Kom igång med ett HPC Pack kluster i Azure toorun Excel och SOA-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="ff962-125">Tutorial: Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads</span></span>](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a><span data-ttu-id="ff962-126">Manuell distribution med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ff962-126">Manual deployment with hello Azure portal</span></span>
* [<span data-ttu-id="ff962-127">Ställ in hello huvudnod i HPC Pack-kluster i en Azure VM</span><span class="sxs-lookup"><span data-stu-id="ff962-127">Set up hello head node of an HPC Pack cluster in an Azure VM</span></span>](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="ff962-128">Klusterhantering</span><span class="sxs-lookup"><span data-stu-id="ff962-128">Cluster management</span></span>
* [<span data-ttu-id="ff962-129">Hantera compute-noder i ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-129">Manage compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="ff962-130">Växa eller krympa Azure-beräkningsresurser i ett HPC Pack-kluster</span><span class="sxs-lookup"><span data-stu-id="ff962-130">Grow and shrink Azure compute resources in an HPC Pack cluster</span></span>](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="ff962-131">Skicka jobb tooan HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-131">Submit jobs tooan HPC Pack cluster in Azure</span></span>](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="ff962-132">Hantering av jobb i HPC Pack</span><span class="sxs-lookup"><span data-stu-id="ff962-132">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)
* [<span data-ttu-id="ff962-133">Hantera ett HPC Pack kluster i Azure med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff962-133">Manage an HPC Pack cluster in Azure with Azure Active Directory</span></span>](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a><span data-ttu-id="ff962-134">Lägga till worker-rollen noder tooan HPC Pack kluster</span><span class="sxs-lookup"><span data-stu-id="ff962-134">Add worker role nodes tooan HPC Pack cluster</span></span>
* [<span data-ttu-id="ff962-135">Burst tooAzure worker-instanser med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="ff962-135">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="ff962-136">Självstudier: Skapa en hybrid-kluster med HPC Pack i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-136">Tutorial: Set up a hybrid cluster with HPC Pack in Azure</span></span>](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [<span data-ttu-id="ff962-137">Lägg till Azure ”burst” noder tooan HPC Pack huvudnod i Azure</span><span class="sxs-lookup"><span data-stu-id="ff962-137">Add Azure "burst" nodes tooan HPC Pack head node in Azure</span></span>](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a><span data-ttu-id="ff962-138">Integrera med Azure Batch</span><span class="sxs-lookup"><span data-stu-id="ff962-138">Integrate with Azure Batch</span></span>
* [<span data-ttu-id="ff962-139">TooAzure burst med HPC Pack</span><span class="sxs-lookup"><span data-stu-id="ff962-139">Burst tooAzure Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="ff962-140">Skapa kluster som RDMA för MPI arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="ff962-140">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="ff962-141">Konfigurera ett RDMA för Windows-kluster med HPC Pack toorun MPI program</span><span class="sxs-lookup"><span data-stu-id="ff962-141">Set up a Windows RDMA cluster with HPC Pack toorun MPI applications</span></span>](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

