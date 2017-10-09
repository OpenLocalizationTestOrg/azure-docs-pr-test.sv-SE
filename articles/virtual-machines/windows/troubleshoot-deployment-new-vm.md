---
title: aaaTroubleshoot distributionen av Windows VM i Azure | Microsoft Docs
description: "Felsökning av distributionsproblem med Resource Manager när du skapar en ny Windows virtuell dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a><span data-ttu-id="3a27f-103">Felsöka distributionsproblem när du skapar en ny Windows virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="3a27f-103">Troubleshoot deployment issues when creating a new Windows VM in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="3a27f-104">De främsta problemen</span><span class="sxs-lookup"><span data-stu-id="3a27f-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="3a27f-105">Andra Virtuella distributionsproblem och frågor finns i [felsöka distribution problem med Windows virtuell dator i Azure](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="3a27f-105">For other VM deployment issues and questions, see [Troubleshoot deploying Windows virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>

## <a name="collect-activity-logs"></a><span data-ttu-id="3a27f-106">Samla in aktiviteten loggar</span><span class="sxs-lookup"><span data-stu-id="3a27f-106">Collect activity logs</span></span>
<span data-ttu-id="3a27f-107">toostart felsökning, samla in hello aktivitet loggar tooidentify hello fel som är associerade med hello problemet.</span><span class="sxs-lookup"><span data-stu-id="3a27f-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="3a27f-108">hello innehåller följande länkar detaljerad information om hello processen toofollow.</span><span class="sxs-lookup"><span data-stu-id="3a27f-108">hello following links contain detailed information on hello process toofollow.</span></span>

[<span data-ttu-id="3a27f-109">Visa distributionsåtgärder</span><span class="sxs-lookup"><span data-stu-id="3a27f-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="3a27f-110">Visa aktiviteten loggar toomanage Azure resurser</span><span class="sxs-lookup"><span data-stu-id="3a27f-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="3a27f-111">**Y:** om hello OS är Windows generaliserad, och det är upp och/eller avbildas med hello generaliserad inställningen kommer det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="3a27f-111">**Y:** If hello OS is Windows generalized, and it is uploaded and/or captured with hello generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="3a27f-112">På liknande sätt, om hello OS är specialanpassat för Windows och den är upp och/eller avbildas med särskilda hello-inställningen och det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="3a27f-112">Similarly, if hello OS is Windows specialized, and it is uploaded and/or captured with hello specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="3a27f-113">**Överför fel:**</span><span class="sxs-lookup"><span data-stu-id="3a27f-113">**Upload Errors:**</span></span>

<span data-ttu-id="3a27f-114">**N<sup>1</sup>:** om hello OS är Windows generaliserad och det överförs som specialiserade, du får ett etablering timeout-fel med hello VM fastnat på hello OOBE-skärmen.</span><span class="sxs-lookup"><span data-stu-id="3a27f-114">**N<sup>1</sup>:** If hello OS is Windows generalized, and it is uploaded as specialized, you will get a provisioning timeout error with hello VM stuck at hello OOBE screen.</span></span>

<span data-ttu-id="3a27f-115">**N<sup>2</sup>:** om hello OS är specialanpassat för Windows och det överförs som generaliserad, får du en allokering felmeddelande med hello VM som fastnat på hello OOBE-skärmen eftersom hello nya virtuella datorn körs med hello ursprungsdatorn namn, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a27f-115">**N<sup>2</sup>:** If hello OS is Windows specialized, and it is uploaded as generalized, you will get a provisioning failure error with hello VM stuck at hello OOBE screen because hello new VM is running with hello original computer name, username and password.</span></span>

<span data-ttu-id="3a27f-116">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="3a27f-116">**Resolution**</span></span>

<span data-ttu-id="3a27f-117">tooresolve båda dessa fel, Använd [Lägg till AzureRmVhd tooupload hello ursprungliga VHD](https://msdn.microsoft.com/library/mt603554.aspx), tillgänglig lokalt, med hello samma inställning som för hello OS (generaliserad/specialiserade).</span><span class="sxs-lookup"><span data-stu-id="3a27f-117">tooresolve both these errors, use [Add-AzureRmVhd tooupload hello original VHD](https://msdn.microsoft.com/library/mt603554.aspx), available on-premises, with hello same setting as that for hello OS (generalized/specialized).</span></span> <span data-ttu-id="3a27f-118">tooupload som generaliserad, Kom ihåg toorun sysprep först.</span><span class="sxs-lookup"><span data-stu-id="3a27f-118">tooupload as generalized, remember toorun sysprep first.</span></span>

<span data-ttu-id="3a27f-119">**Avbilda fel:**</span><span class="sxs-lookup"><span data-stu-id="3a27f-119">**Capture Errors:**</span></span>

<span data-ttu-id="3a27f-120">**N<sup>3</sup>:** om hello OS är Windows generaliserad och avbildas som specialiserade, du får en allokering tidsgränsfel eftersom hello ursprungliga VM inte kan användas eftersom den har markerats som generaliserad.</span><span class="sxs-lookup"><span data-stu-id="3a27f-120">**N<sup>3</sup>:** If hello OS is Windows generalized, and it is captured as specialized, you will get a provisioning timeout error because hello original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="3a27f-121">**N<sup>4</sup>:** om hello OS är specialanpassat för Windows och avbildningen som generaliserad, får du en allokering felmeddelande eftersom hello nya virtuella datorn körs med hello ursprungliga datornamn, användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a27f-121">**N<sup>4</sup>:** If hello OS is Windows specialized, and it is captured as generalized, you will get a provisioning failure error because hello new VM is running with hello original computer name, username, and password.</span></span> <span data-ttu-id="3a27f-122">Dessutom hello ursprungliga VM kan inte användas eftersom den har markerats som särskilda.</span><span class="sxs-lookup"><span data-stu-id="3a27f-122">Also, hello original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="3a27f-123">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="3a27f-123">**Resolution**</span></span>

<span data-ttu-id="3a27f-124">tooresolve båda dessa fel, ta bort hello nuvarande avbildningen från hello-portalen och [avbilda från hello aktuella virtuella hårddiskar](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello med samma inställning som för hello OS (generaliserad/specialiserade).</span><span class="sxs-lookup"><span data-stu-id="3a27f-124">tooresolve both these errors, delete hello current image from hello portal, and [recapture it from hello current VHDs](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) with hello same setting as that for hello OS (generalized/specialized).</span></span>

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a><span data-ttu-id="3a27f-125">Problem: Marketplace-anpassad/galleriet bilden. Allokeringsfel</span><span class="sxs-lookup"><span data-stu-id="3a27f-125">Issue: Custom/gallery/marketplace image; allocation failure</span></span>
<span data-ttu-id="3a27f-126">Det här felet uppstår i situationer när hello ny VM-begäran är fäst tooa kluster som antingen har inte stöd för hello VM-storlek som begärs eller saknar lediga utrymmet tooaccommodate hello begäran.</span><span class="sxs-lookup"><span data-stu-id="3a27f-126">This error arises in situations when hello new VM request is pinned tooa cluster that either cannot support hello VM size being requested, or does not have available free space tooaccommodate hello request.</span></span>

<span data-ttu-id="3a27f-127">**Orsak 1:** hello kluster stöder inte hello begärda VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="3a27f-127">**Cause 1:** hello cluster cannot support hello requested VM size.</span></span>

<span data-ttu-id="3a27f-128">**Lösning 1:**</span><span class="sxs-lookup"><span data-stu-id="3a27f-128">**Resolution 1:**</span></span>

* <span data-ttu-id="3a27f-129">Försök hello förfrågan med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="3a27f-129">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="3a27f-130">Om hello efterfrågade storleken hello VM inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="3a27f-130">If hello size of hello requested VM cannot be changed:</span></span>
  * <span data-ttu-id="3a27f-131">Stoppa alla hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="3a27f-131">Stop all hello VMs in hello availability set.</span></span>
    <span data-ttu-id="3a27f-132">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="3a27f-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="3a27f-133">När alla hello stoppa virtuella datorer, skapa hello ny virtuell dator i hello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="3a27f-133">After all hello VMs stop, create hello new VM in hello desired size.</span></span>
  * <span data-ttu-id="3a27f-134">Starta hello nya virtuella datorn först och sedan väljer av hello stoppas virtuella datorer och klicka på **starta**.</span><span class="sxs-lookup"><span data-stu-id="3a27f-134">Start hello new VM first, and then select each of hello stopped VMs and click **Start**.</span></span>

<span data-ttu-id="3a27f-135">**Orsak 2:** hello klustret har inte frigöra resurser.</span><span class="sxs-lookup"><span data-stu-id="3a27f-135">**Cause 2:** hello cluster does not have free resources.</span></span>

<span data-ttu-id="3a27f-136">**Lösning 2:**</span><span class="sxs-lookup"><span data-stu-id="3a27f-136">**Resolution 2:**</span></span>

* <span data-ttu-id="3a27f-137">Försök igen med begäran hello vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="3a27f-137">Retry hello request at a later time.</span></span>
* <span data-ttu-id="3a27f-138">Om hello ny virtuell dator kan vara en del av en annan tillgänglighetsuppsättning in</span><span class="sxs-lookup"><span data-stu-id="3a27f-138">If hello new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="3a27f-139">Skapa en ny virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region).</span><span class="sxs-lookup"><span data-stu-id="3a27f-139">Create a new VM in a different availability set (in hello same region).</span></span>
  * <span data-ttu-id="3a27f-140">Lägg till hello nya VM toohello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3a27f-140">Add hello new VM toohello same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a27f-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a27f-141">Next steps</span></span>
<span data-ttu-id="3a27f-142">Om du får problem när du startar en stoppad virtuell Windows-dator eller ändra storlek på en befintlig Windows virtuell dator i Azure, se [felsöka Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a27f-142">If you encounter issues when you start a stopped Windows VM or resize an existing Windows VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

