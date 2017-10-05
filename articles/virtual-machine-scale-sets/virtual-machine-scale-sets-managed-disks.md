---
title: "Använda Managed Disks med skalningsuppsättningar för virtuella Azure-datorer | Microsoft Docs"
description: "Lär dig varför och hur du använder hanterade diskar med skalningsuppsättningar för virtuella datorer"
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
ms.openlocfilehash: 3ab1d432a2f90db57b99f0e7d419d85e2958c308
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a><span data-ttu-id="30cbf-103">Skalningsuppsättningar för virtuella Azure-datorer och hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="30cbf-103">Azure VM scale sets and managed disks</span></span>

<span data-ttu-id="30cbf-104">[Skalningsuppsättningar för virtuella Azure-datorer](/azure/virtual-machine-scale-sets/) stöder nu virtuella datorer med hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="30cbf-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) supports virtual machines with managed disks.</span></span> <span data-ttu-id="30cbf-105">Det finns många fördelar med att använda hanterade diskar med skalningsuppsättningar, däribland:</span><span class="sxs-lookup"><span data-stu-id="30cbf-105">Using managed disks with scale sets has several benefits, including:</span></span>

* <span data-ttu-id="30cbf-106">Du behöver inte längre skapa och hantera lagringskonton för att lagra OS-diskar för de virtuella datorerna med skalningsuppsättningarna.</span><span class="sxs-lookup"><span data-stu-id="30cbf-106">You no longer need to pre-create and manage storage accounts to store the OS disks for the scale set VMs.</span></span>

* <span data-ttu-id="30cbf-107">Du kan koppla hanterade diskar till skalningsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="30cbf-107">You can attach managed data disks to the scale set.</span></span>

* <span data-ttu-id="30cbf-108">En skalningsuppsättning med en hanterad disk kan ha en kapacitet så hög som 1 000 virtuella datorer om den är baserad på en plattformsavbildning eller 100 virtuella datorer om den är baserad på en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="30cbf-108">With managed disk, a scale set can have capacity as high as 1,000 VMs if based on a platform image or 100 VMs if based on a custom image.</span></span>

## <a name="get-started"></a><span data-ttu-id="30cbf-109">Kom igång</span><span class="sxs-lookup"><span data-stu-id="30cbf-109">Get started</span></span>

<span data-ttu-id="30cbf-110">Ett enkelt sätt att komma igång med skalningsuppsättningar med hanterade diskar är att distribuera en från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="30cbf-110">A simple way to get started with managed disk scale sets is to deploy one from the Azure portal.</span></span> <span data-ttu-id="30cbf-111">Mer information finns i [den här artikeln](./virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="30cbf-111">For more information, see [this article](./virtual-machine-scale-sets-portal-create.md).</span></span> <span data-ttu-id="30cbf-112">Ett annat enkelt sätt att komma igång är att använda [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) för att distribuera en skalningsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="30cbf-112">Another simple way to get started is to use [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) to deploy a scale set.</span></span> <span data-ttu-id="30cbf-113">Följande exempel visar hur du skapar en Ubuntu-baserad skalningsuppsättning med 10 virtuella datorer, där var och en har en datadisk på 50 och 100 GB:</span><span class="sxs-lookup"><span data-stu-id="30cbf-113">The following example shows how to create an Ubuntu based scale set with 10 VMs, each with a 50-GB and 100-GB data disk:</span></span>

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

<span data-ttu-id="30cbf-114">Alternativt kan du titta i [GitHub-databasen för Azure-snabbstartsmallar](https://github.com/Azure/azure-quickstart-templates) och leta efter mappar som innehåller `vmss` för att se fördefinierade exempel på mallar som distribuerar skalningsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="30cbf-114">Alternatively, you could look in the [Azure Quickstart Templates GitHub repo](https://github.com/Azure/azure-quickstart-templates) for folders that contain `vmss` to see pre-built examples of templates that deploy scale sets.</span></span> <span data-ttu-id="30cbf-115">Om du vill se vilka mallar som redan använder hanterade diskar kan du referera till [den här listan](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span><span class="sxs-lookup"><span data-stu-id="30cbf-115">To tell which templates are already using managed disks, you can refer to [this list](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="30cbf-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30cbf-116">Next steps</span></span>

<span data-ttu-id="30cbf-117">Du hittar mer information om hanterade diskar i allmänhet i [den här artikeln](../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="30cbf-117">For more information on managed disks in general, see [this article](../virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="30cbf-118">Se [den här artikeln](./virtual-machine-scale-sets-convert-template-to-md.md) för information om hur du konverterar Resource Manager-mall för att etablera skalningsuppsättningar med hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="30cbf-118">To see how to convert a Resource Manager template to provision scale sets with managed disks, see [this article](./virtual-machine-scale-sets-convert-template-to-md.md).</span></span> <span data-ttu-id="30cbf-119">Ändringarna till Resource Manager-mallar tillämpas även på Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="30cbf-119">The same modifications to the Resource Manager templates apply to the Azure REST API as well.</span></span>

<span data-ttu-id="30cbf-120">Mer information om hur du använder hanterade datadiskar med skalningsuppsättningar finns i [den här artikeln](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="30cbf-120">To learn more about using managed data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="30cbf-121">Referera till [den här artikeln](./virtual-machine-scale-sets-placement-groups.md) för att börja arbeta med stora skalningsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="30cbf-121">To begin working with large scale sets, refer to [this article](./virtual-machine-scale-sets-placement-groups.md).</span></span>


