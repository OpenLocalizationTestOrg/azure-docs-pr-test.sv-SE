---
title: "aaaVM startar om eller ändrar storlek på frågor | Microsoft Docs"
description: "Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: fb1dc88bb1b83043c434590118bc8810ad402872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a><span data-ttu-id="666a8-103">Felsöka klassisk distributionsproblem med att starta om eller ändrar storlek på en befintlig virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="666a8-103">Troubleshoot classic deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="666a8-104">Klassisk</span><span class="sxs-lookup"><span data-stu-id="666a8-104">Classic</span></span>](restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="666a8-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="666a8-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<span data-ttu-id="666a8-106">När du försöker toostart en stoppad Azure virtuell dator (VM) eller ändra storlek på en befintlig virtuell Azure-dator, är hello vanliga fel du stöter på ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="666a8-106">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="666a8-107">Det här felet uppstår när hello kluster eller den region antingen inte har resurser som är tillgängliga eller inte support hello begärda VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="666a8-107">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="666a8-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="666a8-108">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="666a8-109">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="666a8-109">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="666a8-110">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="666a8-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="666a8-111">Hello Resource Manager version finns [här](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="666a8-111">For hello Resource Manager version, see [here](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="666a8-112">Samla in granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="666a8-112">Collect audit logs</span></span>
<span data-ttu-id="666a8-113">toostart felsökning, samla in hello audit loggar tooidentify hello fel som är associerade med hello problemet.</span><span class="sxs-lookup"><span data-stu-id="666a8-113">toostart troubleshooting, collect hello audit logs tooidentify hello error associated with hello issue.</span></span>

<span data-ttu-id="666a8-114">I hello Azure-portalen klickar du på **Bläddra** > **virtuella datorer** > *Linux-dator*  >   **Inställningar för** > **granskningsloggar**.</span><span class="sxs-lookup"><span data-stu-id="666a8-114">In hello Azure portal, click **Browse** > **Virtual machines** > *your Linux virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="666a8-115">Problem: Fel när du startar en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="666a8-115">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="666a8-116">Du försöker toostart en stoppad virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="666a8-116">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="666a8-117">Orsak</span><span class="sxs-lookup"><span data-stu-id="666a8-117">Cause</span></span>
<span data-ttu-id="666a8-118">hello begäran toostart hello stoppats VM har toobe med hello ursprungliga klustret som är värd för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="666a8-118">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="666a8-119">Hello-klustret har dock inte ledigt utrymme tillgängligt toofulfill hello begäran.</span><span class="sxs-lookup"><span data-stu-id="666a8-119">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="666a8-120">Lösning</span><span class="sxs-lookup"><span data-stu-id="666a8-120">Resolution</span></span>
* <span data-ttu-id="666a8-121">Skapa en ny molntjänst och associera den med antingen en region eller en region-baserat virtuellt nätverk, men inte en tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="666a8-121">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="666a8-122">Ta bort hello stoppad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="666a8-122">Delete hello stopped VM.</span></span>
* <span data-ttu-id="666a8-123">Återskapa hello VM i hello ny molntjänst med hjälp av hello diskar.</span><span class="sxs-lookup"><span data-stu-id="666a8-123">Recreate hello VM in hello new cloud service by using hello disks.</span></span>
* <span data-ttu-id="666a8-124">Starta hello återskapas VM.</span><span class="sxs-lookup"><span data-stu-id="666a8-124">Start hello re-created VM.</span></span>

<span data-ttu-id="666a8-125">Om du får ett felmeddelande när du försöker toocreate en ny molntjänst försök vid ett senare tillfälle eller ändra hello region för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="666a8-125">If you get an error when trying toocreate a new cloud service, either retry at a later time or change hello region for hello cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="666a8-126">hello ny molntjänst får ett nytt namn och VIP, så behöver du toochange informationen för alla hello beroenden som använder denna information för hello befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="666a8-126">hello new cloud service will have a new name and VIP, so you will need toochange that information for all hello dependencies that use that information for hello existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="666a8-127">Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="666a8-127">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="666a8-128">Du försöker tooresize en befintlig virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="666a8-128">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="666a8-129">Orsak</span><span class="sxs-lookup"><span data-stu-id="666a8-129">Cause</span></span>
<span data-ttu-id="666a8-130">hello begäran tooresize hello VM har toobe försökt på hello ursprungliga klustret värdar hello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="666a8-130">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="666a8-131">Dock hello kluster stöder inte hello begärda VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="666a8-131">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="666a8-132">Lösning</span><span class="sxs-lookup"><span data-stu-id="666a8-132">Resolution</span></span>
<span data-ttu-id="666a8-133">Minska hello begärda VM-storlek och försök igen hello storlek på begäran.</span><span class="sxs-lookup"><span data-stu-id="666a8-133">Reduce hello requested VM size, and retry hello resize request.</span></span>

* <span data-ttu-id="666a8-134">Klicka på **Bläddra igenom alla** > **virtuella datorer (klassisk)** > *din virtuella dator* > **inställningar** > **storlek**.</span><span class="sxs-lookup"><span data-stu-id="666a8-134">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="666a8-135">Detaljerade anvisningar finns i [ändra storlek på virtuella hello](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="666a8-135">For detailed steps, see [Resize hello virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="666a8-136">Om det inte är möjligt tooreduce hello VM-storlek, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="666a8-136">If it is not possible tooreduce hello VM size, follow these steps:</span></span>

* <span data-ttu-id="666a8-137">Skapa en ny molntjänst säkerställer den inte är länkade tooan tillhörighetsgrupp och inte är kopplad till ett virtuellt nätverk som är länkade tooan tillhörighetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="666a8-137">Create a new cloud service, ensuring it is not linked tooan affinity group and not associated with a virtual network that is linked tooan affinity group.</span></span>
* <span data-ttu-id="666a8-138">Skapa en ny, större storlek virtuell dator i den.</span><span class="sxs-lookup"><span data-stu-id="666a8-138">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="666a8-139">Du kan konsolidera dina virtuella datorer i hello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="666a8-139">You can consolidate all your VMs in hello same cloud service.</span></span> <span data-ttu-id="666a8-140">Om en befintlig molntjänst är associerad med en region-baserat virtuellt nätverk, kan du ansluta hello nya cloud service toohello befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="666a8-140">If your existing cloud service is associated with a region-based virtual network, you can connect hello new cloud service toohello existing virtual network.</span></span>

<span data-ttu-id="666a8-141">Om hello befintlig molntjänst inte är associerad med en region-baserat virtuellt nätverk, sedan du har toodelete hello virtuella datorer i hello befintlig molntjänst och återskapa dem i hello ny molntjänst från deras diskar.</span><span class="sxs-lookup"><span data-stu-id="666a8-141">If hello existing cloud service is not associated with a region-based virtual network, then you have toodelete hello VMs in hello existing cloud service, and recreate them in hello new cloud service from their disks.</span></span> <span data-ttu-id="666a8-142">Det är dock viktigt tooremember att hello ny molntjänst får ett nytt namn och VIP, så behöver du tooupdate dessa för alla hello beroenden som för närvarande använder den här informationen för hello befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="666a8-142">However, it is important tooremember that hello new cloud service will have a new name and VIP, so you will need tooupdate these for all hello dependencies that currently use this information for hello existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="666a8-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="666a8-143">Next steps</span></span>
<span data-ttu-id="666a8-144">Om du får problem när du skapar en ny Linux VM i Azure, se [felsöka distributionsproblem med att skapa en ny virtuell Linux-dator i Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="666a8-144">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

