---
title: "VM att starta om eller ändrar storlek på frågor | Microsoft Docs"
description: "Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/13/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 7fe0636366c60d4679cfc69bd96cd532695b080e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a><span data-ttu-id="d708d-103">Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="d708d-103">Troubleshoot classic deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d708d-104">Klassisk</span><span class="sxs-lookup"><span data-stu-id="d708d-104">Classic</span></span>](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="d708d-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d708d-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

<span data-ttu-id="d708d-106">När du försöker starta en stoppad Azure virtuell dator (VM), eller ändra storlek på en befintlig Azure VM är vanligt fel du stöter på ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="d708d-106">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="d708d-107">Det här felet uppstår när det kluster eller den region har inte resurser som är tillgänglig eller har inte stöd för den begärda VM-storleken.</span><span class="sxs-lookup"><span data-stu-id="d708d-107">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d708d-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d708d-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="d708d-109">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d708d-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="d708d-110">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d708d-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="d708d-111">Samla in granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="d708d-111">Collect audit logs</span></span>
<span data-ttu-id="d708d-112">Starta felsökning genom att samla in granskningsloggar för att identifiera fel som är associerade med problemet.</span><span class="sxs-lookup"><span data-stu-id="d708d-112">To start troubleshooting, collect the audit logs to identify the error associated with the issue.</span></span>

<span data-ttu-id="d708d-113">I Azure-portalen klickar du på **Bläddra** > **virtuella datorer** > *din Windows-dator* > **inställningar** > **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="d708d-113">In the Azure portal, click **Browse** > **Virtual machines** > *your Windows virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="d708d-114">Problem: Fel när du startar en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d708d-114">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="d708d-115">Försök att starta en stoppad virtuell dator, men ett fel vid allokering.</span><span class="sxs-lookup"><span data-stu-id="d708d-115">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="d708d-116">Orsak</span><span class="sxs-lookup"><span data-stu-id="d708d-116">Cause</span></span>
<span data-ttu-id="d708d-117">Begäran om att starta stoppad VM måste göras i det ursprungliga klustret som är värd för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d708d-117">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="d708d-118">Klustret har dock inte ledigt utrymme för att slutföra begäran.</span><span class="sxs-lookup"><span data-stu-id="d708d-118">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="d708d-119">Lösning</span><span class="sxs-lookup"><span data-stu-id="d708d-119">Resolution</span></span>
* <span data-ttu-id="d708d-120">Skapa en ny molntjänst och associera den med antingen en region eller en region-baserat virtuellt nätverk, men inte en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="d708d-120">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="d708d-121">Ta bort stoppad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d708d-121">Delete the stopped VM.</span></span>
* <span data-ttu-id="d708d-122">Skapa den virtuella datorn i den nya Molntjänsten med hjälp av diskarna.</span><span class="sxs-lookup"><span data-stu-id="d708d-122">Recreate the VM in the new cloud service by using the disks.</span></span>
* <span data-ttu-id="d708d-123">Starta den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d708d-123">Start the re-created VM.</span></span>

<span data-ttu-id="d708d-124">Om du får ett fel vid försök att skapa en ny molntjänst, försök igen senare eller ändra region för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d708d-124">If you get an error when trying to create a new cloud service, either retry later or change the region for the cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d708d-125">Ny molntjänst får ett nytt namn och VIP, så du behöver ändra den här informationen för alla beroenden som använder denna information för en befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="d708d-125">The new cloud service will have a new name and VIP, so you will need to change that information for all the dependencies that use that information for the existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="d708d-126">Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d708d-126">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="d708d-127">Försök att ändra storlek på en befintlig virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="d708d-127">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="d708d-128">Orsak</span><span class="sxs-lookup"><span data-stu-id="d708d-128">Cause</span></span>
<span data-ttu-id="d708d-129">Begäran att ändra storlek på den virtuella datorn måste göras i det ursprungliga klustret som är värd för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d708d-129">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="d708d-130">Klustret stöder dock inte den begärda VM-storleken.</span><span class="sxs-lookup"><span data-stu-id="d708d-130">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="d708d-131">Lösning</span><span class="sxs-lookup"><span data-stu-id="d708d-131">Resolution</span></span>
<span data-ttu-id="d708d-132">Minska den begärda VM-storleken och försök ändra storlek på begäran.</span><span class="sxs-lookup"><span data-stu-id="d708d-132">Reduce the requested VM size, and retry the resize request.</span></span>

* <span data-ttu-id="d708d-133">Klicka på **Bläddra igenom alla** > **virtuella datorer (klassisk)** > *din virtuella dator* > **inställningar** > **storlek**.</span><span class="sxs-lookup"><span data-stu-id="d708d-133">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="d708d-134">Detaljerade anvisningar finns i [ändra storlek på den virtuella datorn](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="d708d-134">For detailed steps, see [Resize the virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="d708d-135">Följ dessa steg om det inte är möjligt att minska VM-storlek:</span><span class="sxs-lookup"><span data-stu-id="d708d-135">If it is not possible to reduce the VM size, follow these steps:</span></span>

* <span data-ttu-id="d708d-136">Skapa en ny molntjänst säkerställer den inte kopplad till en tillhörighetsgrupp och inte är associerad med ett virtuellt nätverk som är kopplad till en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="d708d-136">Create a new cloud service, ensuring it is not linked to an affinity group and not associated with a virtual network that is linked to an affinity group.</span></span>
* <span data-ttu-id="d708d-137">Skapa en ny, större storlek virtuell dator i den.</span><span class="sxs-lookup"><span data-stu-id="d708d-137">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="d708d-138">Du kan konsolidera alla dina virtuella datorer i samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="d708d-138">You can consolidate all your VMs in the same cloud service.</span></span> <span data-ttu-id="d708d-139">Om en befintlig molntjänst är associerad med en region-baserat virtuellt nätverk kan du ansluta ny molntjänst till befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d708d-139">If your existing cloud service is associated with a region-based virtual network, you can connect the new cloud service to the existing virtual network.</span></span>

<span data-ttu-id="d708d-140">Om en befintlig molntjänst inte är associerad med en region-baserat virtuellt nätverk, har att ta bort de virtuella datorerna i en befintlig molntjänst och återskapa dem i den nya tjänsten molntjänster från sina diskar.</span><span class="sxs-lookup"><span data-stu-id="d708d-140">If the existing cloud service is not associated with a region-based virtual network, then you have to delete the VMs in the existing cloud service, and recreate them in the new cloud service from their disks.</span></span> <span data-ttu-id="d708d-141">Det är dock viktigt att komma ihåg att ny molntjänst får ett nytt namn och VIP, så behöver du uppdatera dessa för de beroenden som för närvarande använder den här informationen för en befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="d708d-141">However, it is important to remember that the new cloud service will have a new name and VIP, so you will need to update these for all the dependencies that currently use this information for the existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d708d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d708d-142">Next steps</span></span>
<span data-ttu-id="d708d-143">Om du får problem när du skapar en virtuell Windows-dator i Azure, se [felsöka distributionsproblem med att skapa en virtuell Windows-dator i Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d708d-143">If you encounter issues when you create a Windows VM in Azure, see [Troubleshoot deployment issues with creating a Windows virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

