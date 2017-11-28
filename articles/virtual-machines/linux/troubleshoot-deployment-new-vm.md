---
title: "Felsöka Linux VM distribution-RM | Microsoft Docs"
description: "Felsökning av distributionsproblem med Resource Manager när du skapar en ny virtuell Linux-dator i Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: aea5db05843b0175b8ef8b713944e12262e33010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a><span data-ttu-id="6076b-103">Felsökning av Resource Manager distributionsproblem med att skapa en ny virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="6076b-103">Troubleshoot Resource Manager deployment issues with creating a new Linux virtual machine in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="6076b-104">De främsta problemen</span><span class="sxs-lookup"><span data-stu-id="6076b-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="6076b-105">Andra Virtuella distributionsproblem och frågor finns i [felsöka distribuera virtuella Linux-problem i Azure](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="6076b-105">For other VM deployment issues and questions, see [Troubleshoot deploying Linux virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>
## <a name="collect-activity-logs"></a><span data-ttu-id="6076b-106">Samla in aktiviteten loggar</span><span class="sxs-lookup"><span data-stu-id="6076b-106">Collect activity logs</span></span>
<span data-ttu-id="6076b-107">Starta felsökning genom att samla in aktivitetsloggar för att identifiera fel som är associerade med problemet.</span><span class="sxs-lookup"><span data-stu-id="6076b-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="6076b-108">Följande länkar innehåller detaljerad information om processen för att följa.</span><span class="sxs-lookup"><span data-stu-id="6076b-108">The following links contain detailed information on the process to follow.</span></span>

[<span data-ttu-id="6076b-109">Visa distributionsåtgärder</span><span class="sxs-lookup"><span data-stu-id="6076b-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="6076b-110">Visa aktivitetsloggar att hantera Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="6076b-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="6076b-111">**Y:** om Operativsystemet är Linux generaliserad, och det är upp och/eller avbildas med inställningen generaliserad och det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="6076b-111">**Y:** If the OS is Linux generalized, and it is uploaded and/or captured with the generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="6076b-112">På samma sätt om Operativsystemet Linux specialiserade, den är upp och/eller avbildas med inställningen specialiserade och det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="6076b-112">Similarly, if the OS is Linux specialized, and it is uploaded and/or captured with the specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="6076b-113">**Överför fel:**</span><span class="sxs-lookup"><span data-stu-id="6076b-113">**Upload Errors:**</span></span>

<span data-ttu-id="6076b-114">**N<sup>1</sup>:** om Operativsystemet är Linux generaliserad och det överförs som specialiserade, du får en allokering tidsgränsfel eftersom den virtuella datorn har fastnat på läget etablering.</span><span class="sxs-lookup"><span data-stu-id="6076b-114">**N<sup>1</sup>:** If the OS is Linux generalized, and it is uploaded as specialized, you will get a provisioning timeout error because the VM is stuck at the provisioning stage.</span></span>

<span data-ttu-id="6076b-115">**N<sup>2</sup>:** om Operativsystemet Linux specialiserade och det överförs som generaliserad du får felet etablering misslyckades eftersom den nya virtuella datorn körs med ursprungliga datornamn, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6076b-115">**N<sup>2</sup>:** If the OS is Linux specialized, and it is uploaded as generalized, you will get a provisioning failure error because the new VM is running with the original computer name, username and password.</span></span>

<span data-ttu-id="6076b-116">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="6076b-116">**Resolution:**</span></span>

<span data-ttu-id="6076b-117">Överför den ursprungliga virtuella Hårddisken tillgänglig lokalt, med samma inställning som för OS (generaliserad/specialiserade) för att lösa båda dessa fel.</span><span class="sxs-lookup"><span data-stu-id="6076b-117">To resolve both these errors, upload the original VHD, available on-prem, with the same setting as that for the OS (generalized/specialized).</span></span> <span data-ttu-id="6076b-118">Om du vill överföra som generaliserad ihåg att köra - ta bort etableringen först.</span><span class="sxs-lookup"><span data-stu-id="6076b-118">To upload as generalized, remember to run -deprovision first.</span></span>

<span data-ttu-id="6076b-119">**Avbilda fel:**</span><span class="sxs-lookup"><span data-stu-id="6076b-119">**Capture Errors:**</span></span>

<span data-ttu-id="6076b-120">**N<sup>3</sup>:** om Operativsystemet är Linux generaliserad och avbildas som specialiserade, du får en allokering tidsgränsfel eftersom den ursprungliga virtuella datorn inte kan användas eftersom den har markerats som generaliserad.</span><span class="sxs-lookup"><span data-stu-id="6076b-120">**N<sup>3</sup>:** If the OS is Linux generalized, and it is captured as specialized, you will get a provisioning timeout error because the original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="6076b-121">**N<sup>4</sup>:** om Operativsystemet Linux specialiserade och avbildningen som generaliserad du får felet etablering misslyckades eftersom den nya virtuella datorn körs med ursprungliga datornamn, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6076b-121">**N<sup>4</sup>:** If the OS is Linux specialized, and it is captured as generalized, you will get a provisioning failure error because the new VM is running with the original computer name, username and password.</span></span> <span data-ttu-id="6076b-122">Dessutom ursprungligt virtuella datorn kan inte användas eftersom den har markerats som särskilda.</span><span class="sxs-lookup"><span data-stu-id="6076b-122">Also, the original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="6076b-123">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="6076b-123">**Resolution:**</span></span>

<span data-ttu-id="6076b-124">Ta bort den nuvarande avbildningen från portalen för att lösa båda dessa fel och [avbilda från de aktuella virtuella hårddiskarna](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med samma inställning som för OS (generaliserad/specialiserade).</span><span class="sxs-lookup"><span data-stu-id="6076b-124">To resolve both these errors, delete the current image from the portal, and [recapture it from the current VHDs](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) with the same setting as that for the OS (generalized/specialized).</span></span>

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a><span data-ttu-id="6076b-125">Problem: Anpassad / galleriet / marketplace-avbildning; Allokeringsfel</span><span class="sxs-lookup"><span data-stu-id="6076b-125">Issue: Custom/ gallery/ marketplace image; allocation failure</span></span>
<span data-ttu-id="6076b-126">Det här felet uppstår i situationer när den nya VM-begäranden är fäst på ett kluster som stöder inte den begärda VM-storlek eller saknar lediga utrymmet för begäran.</span><span class="sxs-lookup"><span data-stu-id="6076b-126">This error arises in situations when the new VM request is pinned to a cluster that either cannot support the VM size being requested, or does not have available free space to accommodate the request.</span></span>

<span data-ttu-id="6076b-127">**Orsak 1:** klustret stöder inte den begärda VM-storleken.</span><span class="sxs-lookup"><span data-stu-id="6076b-127">**Cause 1:** The cluster cannot support the requested VM size.</span></span>

<span data-ttu-id="6076b-128">**Lösning 1:**</span><span class="sxs-lookup"><span data-stu-id="6076b-128">**Resolution 1:**</span></span>

* <span data-ttu-id="6076b-129">Försök igen med begäran med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="6076b-129">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="6076b-130">Om storleken på den begärda virtuella datorn inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="6076b-130">If the size of the requested VM cannot be changed:</span></span>
  * <span data-ttu-id="6076b-131">Stoppa alla virtuella datorer i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6076b-131">Stop all the VMs in the availability set.</span></span>
    <span data-ttu-id="6076b-132">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="6076b-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="6076b-133">När alla virtuella datorer har slutat, kan du skapa den nya virtuella datorn i önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="6076b-133">After all the VMs stop, create the new VM in the desired size.</span></span>
  * <span data-ttu-id="6076b-134">Starta den nya virtuella datorn först och sedan markera varje stoppad virtuell dator och klicka på **starta**.</span><span class="sxs-lookup"><span data-stu-id="6076b-134">Start the new VM first, and then select each of the stopped VMs and click **Start**.</span></span>

<span data-ttu-id="6076b-135">**Orsak 2:** klustret har inte frigöra resurser.</span><span class="sxs-lookup"><span data-stu-id="6076b-135">**Cause 2:** The cluster does not have free resources.</span></span>

<span data-ttu-id="6076b-136">**Lösning 2:**</span><span class="sxs-lookup"><span data-stu-id="6076b-136">**Resolution 2:**</span></span>

* <span data-ttu-id="6076b-137">Försök begäran vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="6076b-137">Retry the request at a later time.</span></span>
* <span data-ttu-id="6076b-138">Om den nya virtuella datorn kan vara en del av en annan tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="6076b-138">If the new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="6076b-139">Skapa en ny virtuell dator i en annan tillgänglighetsuppsättning (i samma region).</span><span class="sxs-lookup"><span data-stu-id="6076b-139">Create a new VM in a different availability set (in the same region).</span></span>
  * <span data-ttu-id="6076b-140">Lägg till ny virtuell dator i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6076b-140">Add the new VM to the same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6076b-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6076b-141">Next steps</span></span>
<span data-ttu-id="6076b-142">Om du får problem när du startar en stoppad Linux VM eller ändra storlek på en befintlig Linux VM i Azure, se [felsöka Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6076b-142">If you encounter issues when you start a stopped Linux VM or resize an existing Linux VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

