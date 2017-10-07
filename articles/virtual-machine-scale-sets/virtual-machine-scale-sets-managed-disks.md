---
title: "aaaUsing hanterade diskar med Azure Virtual Machine Skaluppsättningar | Microsoft Docs"
description: "Lär dig hur och varför toouse hanterade diskar med virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a><span data-ttu-id="34d4b-103">Skalningsuppsättningar för virtuella Azure-datorer och hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="34d4b-103">Azure VM scale sets and managed disks</span></span>

<span data-ttu-id="34d4b-104">[Skalningsuppsättningar för virtuella Azure-datorer](/azure/virtual-machine-scale-sets/) stöder nu virtuella datorer med hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="34d4b-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) supports virtual machines with managed disks.</span></span> <span data-ttu-id="34d4b-105">Det finns många fördelar med att använda hanterade diskar med skalningsuppsättningar, däribland:</span><span class="sxs-lookup"><span data-stu-id="34d4b-105">Using managed disks with scale sets has several benefits, including:</span></span>

* <span data-ttu-id="34d4b-106">Du behöver inte längre toopre-skapa och hantera konton toostore hello OS diskar med lagringsutrymme för hello skaluppsättning för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="34d4b-106">You no longer need toopre-create and manage storage accounts toostore hello OS disks for hello scale set VMs.</span></span>

* <span data-ttu-id="34d4b-107">Du kan koppla hanterade diskar toohello skala datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="34d4b-107">You can attach managed data disks toohello scale set.</span></span>

* <span data-ttu-id="34d4b-108">En skalningsuppsättning med en hanterad disk kan ha en kapacitet så hög som 1 000 virtuella datorer om den är baserad på en plattformsavbildning eller 100 virtuella datorer om den är baserad på en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="34d4b-108">With managed disk, a scale set can have capacity as high as 1,000 VMs if based on a platform image or 100 VMs if based on a custom image.</span></span>

## <a name="get-started"></a><span data-ttu-id="34d4b-109">Kom igång</span><span class="sxs-lookup"><span data-stu-id="34d4b-109">Get started</span></span>

<span data-ttu-id="34d4b-110">Ett enkelt sätt tooget igång med skaluppsättningar för hanterade diskar är toodeploy en från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="34d4b-110">A simple way tooget started with managed disk scale sets is toodeploy one from hello Azure portal.</span></span> <span data-ttu-id="34d4b-111">Mer information finns i [den här artikeln](./virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-111">For more information, see [this article](./virtual-machine-scale-sets-portal-create.md).</span></span> <span data-ttu-id="34d4b-112">Ett annat enkelt sätt igång tooget är toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="34d4b-112">Another simple way tooget started is toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy a scale set.</span></span> <span data-ttu-id="34d4b-113">hello följande exempel visas hur toocreate en Ubuntu baserade skaluppsättningen med 10 virtuella datorer, var och en med en datadisk på 50 GB och 100 GB:</span><span class="sxs-lookup"><span data-stu-id="34d4b-113">hello following example shows how toocreate an Ubuntu based scale set with 10 VMs, each with a 50-GB and 100-GB data disk:</span></span>

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

<span data-ttu-id="34d4b-114">Alternativt kan du titta i hello [Azure Quickstart mallar GitHub-repo](https://github.com/Azure/azure-quickstart-templates) för mappar som innehåller `vmss` toosee exempel på mallar som distribuerar skaluppsättningar som fördefinierade.</span><span class="sxs-lookup"><span data-stu-id="34d4b-114">Alternatively, you could look in hello [Azure Quickstart Templates GitHub repo](https://github.com/Azure/azure-quickstart-templates) for folders that contain `vmss` toosee pre-built examples of templates that deploy scale sets.</span></span> <span data-ttu-id="34d4b-115">tootell vilka mallar redan använder hanterade diskar, kan du läsa för[listan](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-115">tootell which templates are already using managed disks, you can refer too[this list](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34d4b-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34d4b-116">Next steps</span></span>

<span data-ttu-id="34d4b-117">Du hittar mer information om hanterade diskar i allmänhet i [den här artikeln](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-117">For more information on managed disks in general, see [this article](../virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="34d4b-118">toosee hur tooconvert som en Resource Manager mallen tooprovision skala uppsättningar med hanterade diskar, finns i [i den här artikeln](./virtual-machine-scale-sets-convert-template-to-md.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-118">toosee how tooconvert a Resource Manager template tooprovision scale sets with managed disks, see [this article](./virtual-machine-scale-sets-convert-template-to-md.md).</span></span> <span data-ttu-id="34d4b-119">hello gäller samma ändringar toohello Resource Manager-mallar även toohello Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="34d4b-119">hello same modifications toohello Resource Manager templates apply toohello Azure REST API as well.</span></span>

<span data-ttu-id="34d4b-120">toolearn mer information om hur du använder hanterade datadiskar med skalningsuppsättningar, se [i den här artikeln](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-120">toolearn more about using managed data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="34d4b-121">toobegin arbetar med stor skala anger finns för[i den här artikeln](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="34d4b-121">toobegin working with large scale sets, refer too[this article](./virtual-machine-scale-sets-placement-groups.md).</span></span>


