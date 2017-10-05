---
title: "VM att starta om eller ändrar storlek på problem i Azure | Microsoft Docs"
description: "Felsökning av Resource Manager distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 078c4666f047604b1732e828d27e7e26383aa616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a><span data-ttu-id="aa75b-103">Felsöka distributionsproblem med att starta om eller ändrar storlek på en befintlig Windows virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="aa75b-103">Troubleshoot deployment issues with restarting or resizing an existing Windows VM in Azure</span></span>
<span data-ttu-id="aa75b-104">När du försöker starta en stoppad Azure virtuell dator (VM), eller ändra storlek på en befintlig Azure VM är vanligt fel du stöter på ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="aa75b-104">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="aa75b-105">Det här felet uppstår när det kluster eller den region har inte resurser som är tillgänglig eller har inte stöd för den begärda VM-storleken.</span><span class="sxs-lookup"><span data-stu-id="aa75b-105">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="aa75b-106">Samla in aktiviteten loggar</span><span class="sxs-lookup"><span data-stu-id="aa75b-106">Collect activity logs</span></span>
<span data-ttu-id="aa75b-107">Starta felsökning genom att samla in aktivitetsloggar för att identifiera fel som är associerade med problemet.</span><span class="sxs-lookup"><span data-stu-id="aa75b-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="aa75b-108">Följande länkar innehåller detaljerad information om processen:</span><span class="sxs-lookup"><span data-stu-id="aa75b-108">The following links contain detailed information on the process:</span></span>

[<span data-ttu-id="aa75b-109">Visa distributionsåtgärder</span><span class="sxs-lookup"><span data-stu-id="aa75b-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="aa75b-110">Visa aktivitetsloggar att hantera Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="aa75b-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="aa75b-111">Problem: Fel när du startar en stoppad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="aa75b-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="aa75b-112">Försök att starta en stoppad virtuell dator, men ett fel vid allokering.</span><span class="sxs-lookup"><span data-stu-id="aa75b-112">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="aa75b-113">Orsak</span><span class="sxs-lookup"><span data-stu-id="aa75b-113">Cause</span></span>
<span data-ttu-id="aa75b-114">Begäran om att starta stoppad VM måste göras i det ursprungliga klustret som är värd för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="aa75b-114">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="aa75b-115">Klustret har dock inte ledigt utrymme för att slutföra begäran.</span><span class="sxs-lookup"><span data-stu-id="aa75b-115">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="aa75b-116">Lösning</span><span class="sxs-lookup"><span data-stu-id="aa75b-116">Resolution</span></span>
* <span data-ttu-id="aa75b-117">Stoppa alla virtuella datorer i tillgänglighetsuppsättningen och sedan starta om varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa75b-117">Stop all the VMs in the availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="aa75b-118">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="aa75b-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="aa75b-119">När alla virtuella datorer har slutat, Välj varje stoppad virtuell dator och klicka på Start.</span><span class="sxs-lookup"><span data-stu-id="aa75b-119">After all the VMs stop, select each of the stopped VMs and click Start.</span></span>
* <span data-ttu-id="aa75b-120">Försöka starta om vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="aa75b-120">Retry the restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="aa75b-121">Problem: Ett fel uppstod när du ändrar storlek på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="aa75b-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="aa75b-122">Försök att ändra storlek på en befintlig virtuell dator men får ett Allokeringsfel.</span><span class="sxs-lookup"><span data-stu-id="aa75b-122">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="aa75b-123">Orsak</span><span class="sxs-lookup"><span data-stu-id="aa75b-123">Cause</span></span>
<span data-ttu-id="aa75b-124">Begäran att ändra storlek på den virtuella datorn måste göras i det ursprungliga klustret som är värd för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="aa75b-124">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="aa75b-125">Klustret stöder dock inte den begärda VM-storleken.</span><span class="sxs-lookup"><span data-stu-id="aa75b-125">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="aa75b-126">Lösning</span><span class="sxs-lookup"><span data-stu-id="aa75b-126">Resolution</span></span>
* <span data-ttu-id="aa75b-127">Försök igen med begäran med en mindre VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="aa75b-127">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="aa75b-128">Om storleken på den begärda virtuella datorn inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="aa75b-128">If the size of the requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="aa75b-129">Stoppa alla virtuella datorer i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="aa75b-129">Stop all the VMs in the availability set.</span></span>
     
     * <span data-ttu-id="aa75b-130">Klicka på **resursgrupper** > *resursgruppen* > **resurser** > *din tillgänglighetsuppsättning* > **virtuella datorer** > *din virtuella dator* > **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="aa75b-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="aa75b-131">Stoppa alla virtuella datorer, ändra storlek på den önskade virtuella datorn till en större storlek.</span><span class="sxs-lookup"><span data-stu-id="aa75b-131">After all the VMs stop, resize the desired VM to a larger size.</span></span>
  3. <span data-ttu-id="aa75b-132">Välj den storleksändrade virtuella datorn och klicka på **starta**, och sedan starta varje stoppad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa75b-132">Select the resized VM and click **Start**, and then start each of the stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa75b-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa75b-133">Next steps</span></span>
<span data-ttu-id="aa75b-134">Om du får problem när du skapar en ny Windows virtuell dator i Azure, se [felsöka distributionsproblem med att skapa en ny Windows virtuell dator i Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="aa75b-134">If you encounter issues when you create a new Windows VM in Azure, see [Troubleshoot deployment issues with creating a new Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

